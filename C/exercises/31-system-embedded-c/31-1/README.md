# 31-1 실습: C abstract machine과 observable behavior
이론: [note](../../../notes/31-system-embedded-c/31-1-c-abstract-machine-and-observable-behavior.md)
## 실습 목적
C source 의미와 compiler/ABI/CPU 구현을 분리하고 observable output을 보존하는 optimization을 관찰한다.
## 작성할 파일
- `main.c`
## 해야 할 일
정의된 integer 계산을 함수로 작성하고 결과를 `printf`한다. `-O0`과 `-O2` assembly를 `/tmp/part31-validation/`에만 생성해 함수 호출, 중간 object, instruction 수가 달라질 수 있음을 관찰한다.
## 사용할 개념
C abstract machine, observable behavior, as-if transformation, compiler observation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o abstract_machine
```
## 실행 방법
```sh
./abstract_machine
```
## 예상 관찰 결과
strict 실행은 `result=41`이다. optimization level과 무관하게 요구되는 출력은 같다. assembly 차이는 현재 GCC/target 관찰이다.
## 확인 포인트
C 문장·object와 instruction·register를 1:1로 대응하지 않는다. `printf` 효과를 compiler가 무조건 제거한다고 쓰지 않는다.
## 추가 실습
- ★ 입력 상수를 바꾼다.
- ★★ `-O0 -S`와 `-O2 -S`를 비교한다.
- ★★★ 차이를 C17 보장과 GCC 관찰로 나눠 기록한다.
## 완료 기준
strict build/run이 통과하고 abstract machine, compiler, ABI, CPU 층을 정확히 구분한다.
