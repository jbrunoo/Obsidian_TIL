인프런 김영한님 강의


듣는 이유 :
1) spring에 대한 인사이트 얻기.
2) 안드로이드 개발 관련 전체적인 강의가 없음. mvvm이나 DI, 객체지향 등 이해하고 적용해보고 싶은데 찾기가 어려움. spring 강의에 전반적인 구조 설계를 들으니 공통의 부분을 통해 함께 성장중.


스프링 빈 등록.
@Controller, @Service 등 컴포넌트 스캔을 통한 자동 주입.
자바 코드로 직접 config 하여 스프링 빈 등록.
xml 설정 방식은 최근 사용 x.
ps. @Autowired 통한 DI는 스프링이 관리하는 객체에서만 동작. 생성자가 하나면 생략 가능.



안드로이드 구조에 대한 이해도 함께 도움이 됨.
mvc로 구현되지만 DI, annotation, spring bean, repository 인터페이스, repository 구현체 만드는 방식 등
mvc(model view controller)로 디렉토리는 보편적으로 controller, domain, repository, service로 나눔.
domain에 객체, repository , service 실제 로직. controller



DI에는 필드 주입, setter 주입, 생성자 주입
의존 관계가 실행 중에 동적으로 변하는 경우는 거의 없으므로(실무에서 아예 없음) 생성자 주입 권장.
테스트 코드처럼 다른 곳에 쓸 것이 아니면 필드 주입을 써도 되긴 함.




DB 관리 기술 변천사
	메모리 저장(휘발) - 강의에서는 H2 db 사용. 순수 Jdbc(20년 된.. 애플리케이션 서버와 db 연결) - spring JdbcTemplate(MyBatis 라이브러리와 비슷, 애플리케이션에서 db로 SQL 날림, 이건 실무에서도 많이 씀) - JPA(개발자가 SQL 쿼리도 안짤 수 있게) - spring data JPA(JPA를 한번 더 감쌈)

JPA가 room db에서 기본 쿼리 쓸 수 있는 그런 느낌..?

- JdbcTemplate : 반복 코드 제거. 템플릿 메소드 패턴을 많이 활용해서 붙은 이름.
- JPA : 반복 코드 제거, JPQL, 기본적인 SQL은 JPA가 처리. SQL, db 중심 설계에서 객체 중심으로 패러다임 전환. 개발 생상성 향상. 객체와 ORM(Object Relational Mapping) 
- Spring data JPA : 더 간결한 문법. 주입도 간결. findByNameAndId 처럼 메서드 이름만으로도 조회 기능 제공. 자동으로 findBy 뒤의 이름 및 and or 등을 판단해서 SQL 쿼리를 짜줌. 페이지 기능 자동 제공.
PS. 실무에서 기본적으로 JPA, Spring data JPA를 사용하고 복잡한 동적 쿼리는 Querydsl 라이브러리 사용. JPA가 제공하는 네이티브 쿼리도 사용 가능하고 JdbcTemplate 사용도 가능.



테스트 : 단위 테스트, 통합 테스트

통합테스트
@SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행.
@Transactional : 테스트 케이스에 어노테이션 있으면 테스트 시작 전에 트랜잭션을 실행하고 테스트 완료 후 에 항상 롤백함. db에 데이터가 남지 않아서 다음 테스트에 영향 주지 않음.
(물론 서비스 같은데 붙으면 정상 동작. 테스트 케이스에서만 동작. @Commit 쓰면 롤백말고 커밋됨.)

무조건은 아니지만 단위 테스트가 훨씬 좋은 테스트일 가능성이 높다.
통합 테스트처럼 스프링 컨테이너까지 올려야 하는 상황이면 테스트 설계가 잘못됐을 확률이 높다.



AOP(Aspect Oriented Programming) 관점 지향 프로그래밍
필요 상황 : 
	모든 메소드의 호출 시간을 측정하고 싶다면?
	공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)
	회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

1. 공통 관심 사항과 핵심 관심 사항을 분리.

스프링은 프록시 방식의 AOP
실제 프로세스는 스프링 컨테이너에서 **proxy**를 통해 가짜에 DI를 하고 joinPoint.proceed() 일어나면 그때 진짜 호출. 스프링은 컨테이너에서 스프링 빈을 관리하므로 가짜를 만들어서 여기다 주입함.
