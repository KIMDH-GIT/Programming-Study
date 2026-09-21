# 18-17 실습: sanitizer 검사
이론: [note](../../../notes/18-dynamic-memory/18-17-memory-error-checking.md)

## 실습 목적
정의된 프로그램을 sanitizer build로 실행하고 report 의미를 구분한다.
## 작성할 파일
`checked_memory.c`
## 해야 할 일
정상 allocation·bounds·free 프로그램을 작성한다. sanitizer로 실행하고, note 4절의 가상 UB report는 실행 없이 access·free·allocation 위치만 분석한다.
## 사용할 개념
AddressSanitizer, UndefinedBehaviorSanitizer, implementation tool.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic \
    -fsanitize=address,undefined -g checked_memory.c -o checked_memory
```
## 실행 방법
```sh
./checked_memory
```
## 예상 관찰 결과
정상 값이 출력되고 sanitizer error report가 없다.
## 확인 포인트
report 부재를 모든 correctness의 증명으로 해석하지 않는다.
## 추가 실습
- ★ option 역할
- ★★ sample report 분류
- ★★★ tool limitation 조사
## 완료 기준
UB 코드를 실행하지 않고 도구와 C17을 구분한다.
