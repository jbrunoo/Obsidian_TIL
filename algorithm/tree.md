그래프 - 정점(V), 간선(E)

트리 - 그래프 하위 종류, 사이클 없음, 무방향

이진 트리, 이진 탐색 트리
	이진 : 정 이진, 완전 이진, 변질 이진, 포화 이진, 균형 이진(양 두 하위 트리의 depth 차이가 1이하 !!)
		ps. map, set을 구성하는 레드블랙트리는 균형 이진 트리의 한 종류
	이진 탐색(BST) : 하위 노드의 값이 부모보다 작으면 왼쪽, 크면 오른쪽에 저장. 시간복잡도 O(logN)
		(depth만큼 걸림, log(N - 1))

이진 탐색 트리는 삽입 순서에 영향을 받음. 1-2-3-4-5 이렇게 삽입해버리면 O(N)이 걸리므로.
삽입 순서에 상관없이 노드를 회전시키는 등의 방법으로 균형잡히게 만드는 발전된 트리형태가 AVL트리, 레드블랙트리
(삽입, 탐색, 삭제, 수정 모두 logN인 map은 레드블랙트리 기반 구현)
ps. AVL 트리는 레드블랙트리보다 더 엄격하게 균형이 잡혀있기 때문에 최악의 경우 삽입, 삭제 시 더 많은 회전이 필요

- - -

정점과 간선의 그래프를 코딩으로 표현하려면 인접행렬 or 인접리스트

인접행렬 : 정점과 간선의 관계를 나타내는 bool 타입 정사각형 행렬
`val adjMatrix = Array(n + 1) { IntArray(n + 1) { 0 } } `

인접리스트(adj) : 연결리스트를 여러 개 구성. 각 정점과 연결된 정점을 리스트에 추가.
`val adjList = Array(n + 1) { mutableListOf<Int>() }`

차이
공간복잡도 : 인접행렬(V^2), 인접리스트(V+E)
시간복잡도 : 간선 하나 찾기(1, V) / 간선 모두 찾기(V^2, V+E)

그래프가 희소(Sparse)할 때 == 간선 연결이 적을 때 인접리스트 (인접행렬은 0으로 대부분 채우므로 메모리 낭비)
조밀(Dense)할 때 == 간선 연결 많을 때 인접행렬이 좋음.

무엇을 선택할까 -> 문제 자체에서 인접행렬이 예시로 나오면 그대로 쓰고 아니면 인접리스트를 기본적으로 쓰자
왜냐하면 sparse한 문제가 더 많음. dense 할수록 문제나 테케가 조금 더 복잡해지므로 경우가 적다고 함.

그래프를 표현하는 경우는 위 두가지지만 문제 자체가 맵으로 주어진다면 굳이 바꾸지말고 맵 기반 그대로 풀자
이동이 필요하면 방향 벡터까지 고려.

```kotlin
fun main() {
	val map = Array(3) { IntArray(3) { 1 } }
    val bool = Array(3) { BooleanArray(3) }

    map[0][1] = 1
    map[1][1] = 0
    map[2][0] = 0

    graph(0, 0, map, bool)
}

fun graph(y: Int, x: Int, map: Array<IntArray>, bool: Array<BooleanArray>) {
    bool[y][x] = true
    println("$y $x")
    val dy = listOf(-1, 0, 1, 0)
    val dx = listOf(0, 1, 0, -1)

    for(i in 0..3) {
        val ny = y + dy[i]
        val nx = x + dx[i]
	
        if(ny < 0 || nx < 0 || ny >= map.size || nx >= map[0].size ) continue
        if(map[ny][nx] == 0) continue
        if(bool[ny][nx]) continue
        graph(ny, nx, map, bool)
    }

}
```


dfs(depth first search)
```kotlin
import java.util.*  
  
val arr = Array(101) { IntArray(101) }  
val bool = Array(101) { BooleanArray(101) { false } }  
  
fun main() = with(System.`in`.bufferedReader()) {  
    val (n, m) = readLine().split(" ").map { it.toInt() }  
  
    repeat(n) { i ->  
        val row = StringTokenizer(readLine())  
  
        for(j in 1..n) {  
            arr[i + 1][j] = row.nextToken().toInt()  
        }  
    }  
  
    var cnt = 0  
    for(i in 1..m) {  
        for(j in 1..n) {  
            if(arr[i][j] == 1 && !bool[i][j]) {  
                dfs(i, j)  
                cnt++  
            }  
        }  
    }  
  
    println(cnt)  
}  
  
fun dfs(y: Int, x: Int) {  
    bool[y][x] = true  
    val dy = listOf(1, 0, -1, 0)  
    val dx = listOf(0, 1, 0, -1)  
  
    for(i in 0..3) {  
        val ny = y + dy[i]  
        val nx = x + dx[i]  
  
        if(ny < 1 || nx < 1 || ny >= arr.size || nx >= arr[0].size) continue  
        if(arr[ny][nx] == 1 && !bool[ny][nx]) {  
            println("???")  
            dfs(ny, nx)  
        }  
    }  
    return  
}
```

```kotlin
// 돌다리
fun dfs(here: Int) {
	visited[here] = true

	for(i in adj) {
		if(!visited[here]) {
			dfs(i)
		}
	}
}

// Go
fun dfs(here: Int) {
	if(visited[here]) return
	visited[here] = true

	for(i in adj) {
		dfs(i)
	}
}

```


ps. 좌표 평면과 배열의 차이. y, x 축으로 두는게 나은 이유.
전산 쪽에서 원점은 좌측상단 ex) 모니터
즉, 첫째는 원점의 기준이 어디인지 생각하고
두번쨰는 인덱스를 양수로 세기때문에 y축에서 아래로 내려가면 인덱스는 커지지만 값은 작아지는 의미.
그래서 dy, dx를 북 동 남 서 기준(시계 방향)으로 보게 되면 (-1, 0), (0, 1), (0, 1), (-1, 0)이 됨.
이외에도 첫번째 차원을 기준으로 하기 때문에 속도가 미세하기 더 빠르다고 함.


bfs(breadth first search)
```kotlin
import java.util.*  
  
val map = Array(101) { IntArray(101) }  
val visited = Array(101) { IntArray(101) }  
val dy = intArrayOf(-1, 0, 1, 0)  
val dx = intArrayOf(0, 1, 0, -1)  
  
fun main() = with(System.`in`.bufferedReader()) {  
    val (n, m) = readLine().split(" ").map { it.toInt() }  
    val (s, e) = readLine().split(" ").map { it.toInt() }  
    val (x, y) = readLine().split(" ").map { it.toInt() }  
  
    repeat(n) { i ->  
        val st = StringTokenizer(readLine())  
        for(j in 0 until m) {  
            map[j][i] = st.nextToken().toInt()  
        }  
    }  
    bfs(e, s)  
    print(visited[y][x])  
}  
  
fun bfs(sy: Int, sx: Int) {  
    val queue: Queue<Pair<Int, Int>> = LinkedList()  
    visited[sy][sx] = 1  
    queue.add(Pair(sy, sx))  
  
    while (queue.isNotEmpty()) {  
        val (y, x) = queue.poll()  // queue의 첫 인덱스로 연결 component 확인
  
        for(i in 0..3) {  
            val ny = y + dy[i]  
            val nx = x + dx[i]  
  
            if(nx < 0 || ny < 0 || nx >= 5 || ny >= 5) continue  
            if(map[ny][nx] == 1 && visited[ny][nx] == 0) {  
                visited[ny][nx] = visited[y][x] + 1  // bool 대신 1 증감, 최단거리 파악 가능
                queue.add(Pair(ny, nx))  
            }  
        }  
    }  
}
```


dfs vs bfs 시간복잡도 차이는 없음. 
위에 명시한 인접리스트 또는 인접행렬인지에 따른 차이 뿐.
dfs는 메모리를 덜 쓰고 코드 더 짧음. (완전탐색, SCC, 절단점 등을 구할 수 있음)
bfs는 메모리를 더 쓰고 가중치 같을 때 최단 거리 구할 수 있음.