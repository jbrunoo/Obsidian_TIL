kotlin의 class는 자동으로 getter, setter를 생성해준다. val은 getter, var은 getter, setter를 생성.
클래스 파라미터로 넘겨주면 생성자까지 지정된다.


[블로그](https://ksabs.tistory.com/247)
사용하던 변수를 리턴해도 된다면 private set()으로 변수의 setter만 막자
로직을 추가하여 반환해야 한다면 Backing Proerties (get(), set())을 사용하자

primitive type은 그자체가 변경되어야 해서 var을 많이 쓰지만 참조타입은 val로도 내부의 값을 변경할 수 있다.
ps. 즉, val/var은 불변을 나누기보다 읽기전용인지 정도로 해석하는 것이 좋다.

그래서 안드로이드에서 `by` 키워드는 위임 패턴을 따르는데 `=` 대신 사용할 때 getter, setter 함께 생성한다.