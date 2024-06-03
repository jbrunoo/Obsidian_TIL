main.c (소스 코드)
```C
#include <stdio.h>

int main(void) {
	printf("hello world\n")
}
```

terminal에서 `clang main.c` 명령어를 이용하여 컴파일할 수 있음 -> a.out 파일 생성됨(머신 코드)
`./a.out` 실행해보면 잘 출력된다. 터미널을 잘 활용하면 GUI 의존이 낮아진다. `mkdir, rm, ls` 등
`ls` 로 디렉토리를 살펴보면 컴파일 되어 실행할 수 있는 파일은 `a.out*`로 표시됨


예로 `#include <cs50.h>`처럼 다른 외부 파일을 읽는다면 `clang -o {name}.c -lcs50`처럼 `-l` 로 연결해줄 수 있음. => 복잡하기 때문에 운영체제에 있는 명령어 `make {programName}`으로 쉽게 처리 가능.

