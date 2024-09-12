Android Studio에서 Database 값 확인하기 // view -> Tool Windows -> App Inspection

3가지 구성요소
- Database Class : 데이터 연결을 위한 기본 엑세스 포인트 역할
- Entitiy : 데이터베이스 테이블
	기본형(primitive type)만 가능, [복잡한 객체 유형](https://developer.android.com/training/data-storage/room/referencing-data?hl=ko#kotlin) 참조(@TypeConverter, @Embeded)
	ps. list -> json 같은 타입 변환 시, @ProvidedTypeConverter 주석 활용.
- Data Access Object (DAO) : 데이터에 접근할 수 있는 객체로 쿼리, 업데이트, 삭제 등등의 메서드 제공


가장 먼저 데이터 연결을 위한 (1) Database Class  
어떤 데이터를 관리할 건지 (2) Entity를 정의 
마지막으로 Database의 데이터를 접근하기 위한 (3) Data Access Object (Dao)를 구현

[DB relationship with Room](http://batmask.net/index.php/2021/05/15/938/)


이전 강의 들을 때 코드 및 달아놓았던 설명
```
//project gradle 
plugins { id("com.android.application") version "8.0.2" apply false id("org.jetbrains.kotlin.android") version "1.8.0" apply false id("com.google.devtools.ksp") version "1.8.0-1.0.8" apply false } 

//module: app 
composeOptions { kotlinCompilerExtensionVersion = "1.4.1" }
```
버전의 2. 7. 1은 각각 major. minor. patch 를 의미

사소한거 수정 patch up 기능 추가 minor up 큰 변화 major up

1.4.1은 버전맞춘다고 바꿨음. room은 2.5.0

mainActivity
```
val db = Room.databaseBuilder( context, AppDatabase::class.java, "database-name" ).build() db
```

database.kt - mainActivity에서 빌드하던 것 함수로 db안에 넣어줌
```
@Database(entities = [User::class], version = 2) abstract class AppDatabase : RoomDatabase() { abstract fun userDao(): UserDAO // 아래 부분 추가 // 이게 한 세트기 때문에 앞으로 이 부분만 긁어와서 companion object { @Volatile // 캐시가 아닌 메모리에 바로 저장(?) 찾아보기 private var INSTANCE: AppDatabase? = null fun getDatabase(context: Context): AppDatabase { return INSTANCE ?: synchronized(this) { // 이 부분은 main activity에서 db = remember { 아래 부분 긁어옴 Room.databaseBuilder( context, AppDatabase::class.java, "contacts.db" // contacts.db 이름만 바꿔주면 됨 ).addMigrations(MIGRATION_1_2).build() } } } }
```
database는 한 번 생성해야 하는데 여러번 생성되면 에러가 나올 수 있으니, synchronized 키워드(jvm키워드)를 이용해서 lock을 걸어 한번만

UserDao
```
import kotlinx.coroutines.flow.Flow // 코루틴으로 import 
@Dao 
interface UserDAO { 
	@Query("SELECT * FROM user") 
	fun getAll(): Flow<List<User>> // Flow 제네릭
```


```
val context = LocalContext.current 
val db = remember { AppDatabase.getDatabase(context) }

val userList by db.userDao().getAll().collectAsState(initial = emptyList())
```

데이터베이스 초기화는 앱 삭제 or 앱 내 캐시, 데이터 삭제


db에 값 삽입 및 삭제
``` 
// 버튼 onClick
val newUser = User( uid = 0, name = name, phone = phone, email = email, age = age )
scope.launch(Dispatchers.IO) { if (existingUserName == null) { // 중복 값이 없는 경우 데이터 삽입 db.userDao().insertAll(newUser) }
```
scope 감싸줘야 함 -> UserDao에서 Flow로 return하므로

``` 
var delName by remember { mutableStateOf("") } // 삭제 버튼 OutlinedTextField( value = delName, onValueChange = { delName = it }, label = { Text(text = "del name") }, leadingIcon = { Icon(imageVector = Icons.Default.Clear, contentDescription = "clear") }, colors = TextFieldDefaults.outlinedTextFieldColors(containerColor = Color.White) ) Spacer(modifier = Modifier.height(8.dp)) Button( onClick = { if (delName.isNotEmpty()) { val delUser = userList.find { it.name == delName } // val deleteUser = User( // uid = UidNum.toInt(), // name = name, // phone = phone, // email = email, // age = age // ) scope.launch(Dispatchers.IO) { delUser?.let { db.userDao().delete(it) } } } }, colors = ButtonDefaults.buttonColors(Color.White, Color.Black), ) { Text(text = "Delete") }
```


메인 스레드에서 db에 접근할 수 없음
Cannot access database on the main thread since it may potentially lock the UI for a long period of time.
```
val existingUserName by db.userDao().findByName(name).collectAsState(null)
```
collectAsState 처리를 해주고 findByName 함수에 flow<\user> 처리를 해주었음.


- - -
앱 생성 시 db에 초기 값 입력하기
1. 사전 패키징된 데이터 베이스 추가 ([공식문서](https://developer.android.com/training/data-storage/room/prepopulate?hl=ko#kotlin))
	이 방식은 .db 파일을 가지고 있어야 함 ([db browser for sqlite 에서 db파일 생성 가능](https://sqlitebrowser.org/))
	.db 파일이 assets, file system 중 저장 위치에 따라 createFromAsset(".db"), createFromFile(".db") 
2. 직접 하드코딩한 간단한 더미 or json 파일
	간단한 코드라면 oncreate에서 코드 상으로 DAO insert 활용하여 집어넣거나
	room.databaseBuilder의 .addCallback() 메소드 활용


