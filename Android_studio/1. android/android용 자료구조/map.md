(map으로 제목을 두었으나 다른 컬렉션 자료구조도 비슷함)
기본적인 Map, HashMap(LinkedHashMap) 자료구조 이외에도 최적화가 된 자료구조들이 있다.

- - -
자료구조를 생성할 때 기본적으로 immutable하게 선언하고
수정 시 immutable -> mutable -> immutable 과정을 거치는데,

초기에 생성과 동시에 복잡한 연산의 값을 입력해야 한다면,
buildList, buildMap 과 같은 함수를 이용하면 가변적으로 값을 입력하고 최종적으로 불변적인 자료구조를 반환한다. 

- - -
EnumMap, EnumSet etc.. (java.util 임)
key 값으로 enum class를 받는다.

HashMap과 비교 글을 찾아봤는데 탐색 기준 1.5배가 빠르다는 내용을 보았다.
enum으로 단일 객체임을 보장하여 해싱 작업을 생략한다.

또한 enum class에 명시된 순서를 보장한다.

- - -
ScatterMap
