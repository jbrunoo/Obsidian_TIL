state의 변경을 알아차리기 위해 snapShot 시스템을 활용

- - - 
[참고](https://medium.com/sungbinland/jetpack-compose-%EC%8A%A4%EB%83%85%EC%83%B7-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%86%8C%EA%B0%9C-279b1f61d382)
snapShot 시스템을 이용하는 이유

compose의 composition은 무작위 병렬 실행 -> 동시성 문제 가능성

그래서 compose의 Runtime은 동시성 제어 기법 중 MVCC(MultiVersion Concurrency Control) 채택
ps. MVCC는 db transaction의 안정성 보장을 위한 ACID 중 isolation(격리성)을 구현한 방법 중 하나

MVCC는 write, read 작업에 lock을 걸어 고립을 지키지 않고 각 트랜잭션이 진행될 때 데이터베이스 스냅샷을 캡쳐하여 해당 스냅샷 안(고립)에서 트랜젝션을 수행함

- - -
compose에서 어떻게 구현된건지?

기본동작
snapShot에 기록될 대상은 StateObject로 부름,
SnapShot API에서 take(Mutable)Snapshot으로 스냅샷을 생성하여 StateObject 편집
편집하기 위해 .enter {}로 접근하고 .apply()로 변경 사항 커밋 or .dispose()로 삭제.

스냅샷에서 진행된 연산의 결과는 StateRecord에 저장
apply 없이 여러 개의 연산을 만들면 StateRecord는 linkedList 구조로 저장, StateObject가 시작점
StateRecord는 새로운 레코드 생성 시 값 수정을 위한 기존 레코드 값 복사하는 과정(create, assign 함수 존재)

스냅샷 충돌 시 mergeRecords 함수로 병합 방법 제공.
이는 mutableStateOf에 policy 인자로 설정 가능.

