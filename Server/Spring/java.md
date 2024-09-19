#### collection
	set
		HashSet 가장 빠른 임의 접근 속도
		TreeSet 정렬방법 지정 가능
	List
		LinkedList 양방향 포인터, 삽입 삭제 빈번 시 유용 | 스택, 큐, 양방향 큐 등 만들기 유용
		Vector 과거 대용량 처리용, 내부에서 자동 동기화 처리로 무거움
		ArrayList 단방향 포인터 구조, 조회 성능 좋음
	Map
		HashTable HashMap보다 느리지만 동기화 지원, null 불가
		HashMap 중복, 순서 허용 x, null 가능
		TreeMap 정렬된 순서대로 키, 값 저장하여 검색 빠름
	etc (Queue, Stack ..)


주요 메서드
1. **boolean add(E e) :** 현재 컬렉션에 데이터 객체 e를 추가
2. **boolean addAll(Collection\<? extends E> c) :** 현재 컬렉션에 컬렉션 c의 모든 데이터를 추가 
3. **boolean contains(Object o) :** 현재 컬렉션에 객체 o의 포함 여부를 반환
4. **boolean containsAll(Collection\<?> c) :** 현재 컬렉션에 컬렉션 c의 모든 데이터가 포함되는지 여부 반환
5. **boolean remove(Object o) :** 현재 컬렉션에서 객체 o를 삭제
6. **boolean removeAll(Collection\<?> c) :** 현재 컬렉션에서 컬렉션 c와 일치하는 데이터를 삭제
7. **boolean retainAll(Collection\<?> c) :** 현재 컬렉션에서 컬렉션 c와 일치하는 데이터만 남기고 나머지는 삭제
8. **void clear( ) :** 현재 컬렉션의 모든 데이터를 삭제 
9. **int size( ) :** 현재 컬렉션에 포함된 데이터 개수를 반환 
10. **boolean isEmpty( ) :** 현재 컬렉션이 비어있는지 여부를 반환  
11. **Iterator<\E> iterator( ) :** 현재 컬렉션의 모든 요소에 대한 iterator를 반환 
12. **Object[ ] toArray( ) :** 현재 컬렉션에 저장된 데이터를 Object 배열로 반환
13. **<\T> T[ ] toArray(T[ ] a ) :** 현재 컬렉션에 저장된 데이터를 배열 a에 담고 배열 a를  반환

