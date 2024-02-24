[kotlin 자료구조](https://velog.io/@cksgodl/Kotlin%EC%97%90%EC%84%9C%EC%9D%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0)

`import java.util.*` 대부분의 라이브러리, collection 함수 사용 가능.

![[Pasted image 20240220175832.png]]
[medium](https://medium.com/depayse/kotlin-data-structure-collections-4-stack-queue-deque-4c383efebee9)

	Stack은 class로 구현되어 있음. queue와 deque는 interface 형태로 LinkedList나 ArrayDeque로 구현 가능

#### 자료구조
	스택
	목록의 끝에서만 접근할 수 있는 제한적인 자료구조
	접근, 검색 : O(n) / 추가, 삭제 : O(1)
	Stack 클래스를 사용하게 되면 pop, push, peek, search, empty 함수 사용 가능
	linkedList, arrayDeque로 구현 가능. deque는 interation이 반대이며 search, empty 함수 사용 불가
	메서드 : push, pop, peek, empty, search



	큐
	한쪽 끝에서 삽입, 다른 끝에서 추출 작업이 일어남
	접근, 검색 : O(n) / 추가, 삭제 : O(1)
	linkedList, arrayDeque로 구현 가능. Enqueue/Dequeue 대신 offer/poll 함수 제공.
	메서드 : add, offer, remove, element, poll, peek
	add는 단순히 추가 / offer은 추가 + 성공적 추가 유무에 따른 true/false 반환
	remove : 선입 객체 제거 후 반환. 비어있다면 exception throw
	element : 선입 객체 찾고 반환. 비어있다면 exception throw
	poll :  선입 객체 제거 후 반환. 비어있다면 null 반환



	데크
	양쪽 끝에서 삽입과 삭제 가능
	linkedList, arrayDeque로 구현.
	메서드 : stack, queue의 메서드 모두 이용 가능 + deque의 메서드들
	addFirst/Last, offerFirst/Last, removeFirst/Last, pollFirst/Last, peekFirst/Last, getFirst/Last (비어있을 때 peek은 null 반환, get은 exception throw)



	우선순위 큐
	우선순위가 가장 높은 데이터를 가장 먼저 삭제하는 자료구조
	list나 heap으로 구현
	list : 삽입 - O(1), 삭제 - O(n) / heap : 삽입 - O(logn), 삭제 - O(logn)
	단순히 n개의 데이터를 힙에 넣었다가 모두 꺼내는 작업은 정렬과 동일 (힙 정렬) O(nlogn)
	힙으로 구현 방법
		힙 : 완전 이진 트리
		힙에서는 항상 루트 노드를 제거
		최소 힙 / 최대 힙 (루트 노드가 최소 값/최대 값)
		최소 힙 구성 함수 : Min-heapify() - 보편적으로 이렇게 부름
		삽입 : (상향식) 부모 노드를 거슬러 올라가며, 부모보다 자신의 값이 더 작은 경우 위치 교체
		제거 : 루트 노드 제거 및 마지막 노드를 루트 노드로. 이후 루트 노드부터 (하향식) Heapify() 
	대부분의 언어가 우선순위 큐 라이브러리 있음 코틀린도 java.util 패키지에 PriorityQueue 사용


#### 컬렉션
	특정한 자료구조를 편하게 다루기 위한 라이브러리 
	자체 컬렉션이 아닌 자바 컬렉션 확장. (상호작용 호환 쉽게 하기 위함)

	종류 : 리스트, 맵, 셋 (각각 mutable, immutable)

	컬렉션 생성
	list
		immutable : listOf
		mutable : mutableListOf, arrayListOf

	Set
		immutable : setOf
		mutable : mutableSetOf, hashSetOf, linkedHashSetOf, sortedSetOf

	Map
		immutable : mapOf
		mutable : mutableMapOf, hashMapOf, linkedMapOf, sortedMapOf


