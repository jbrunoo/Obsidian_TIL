Routing
Content Negotiation
Kotlinx.Serialization

dependency
[exposed](https://github.com/JetBrains/Exposed) - db와 상호작용을 위해(액세스)
ps. SQL래핑 DSL 방식과 DAO 방식 선택. Repo 패턴이 따로 없음.
[ORM 블로그 글](https://velog.io/@murphytklee/Spring-ORM-JPA-Hibernate-JDBC-%EC%B4%9D%EC%A0%95%EB%A6%AC)
RDBMS 같은 db는 객체 지향적인 프로그래밍 언어의 객체를 표현하기 부적합. 그것을 위해 ORM이 필요.
ORM을 위해 Hibernate, JPA(Hibernate 기반) 같은 프레임워크가 있음.(Kotlin에서 exposed)
그리고 각기 다른 db의 SQL 문법을 통일 시키는 것이 JDBC API

h2 db 
	MySQL 등 db 사용 시 별도 포트를 물고 프로세스로 띄워져 있거나 다른 서버에 실행되어 원격으로 접속해야 함.
	h2는 인메모리 db. application에서 접근 가능함. 별도의 클라이언트로 접근하려면 위처럼 서버에 띄워야 함.

HikariCP(connection pool) db와 연결될 자원의 풀을 무한정이 아닌 효율적으로 제어하기 위함