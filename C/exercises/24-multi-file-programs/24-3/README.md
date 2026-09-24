# 24-3 실습: header file과 공개 interface
이론: [note](../../../notes/24-multi-file-programs/24-3-headers-and-public-interface.md)
## 실습 목적
공유 type과 function declaration을 공개 header로 제공한다.
## 작성할 파일
- `main.c`
- `student.c`
- `student.h`
## 해야 할 일
`struct Student`와 `student_print` declaration을 header에 두고 implementation을 분리한다.
## 사용할 개념
public interface, shared type definition, quote include, implementation source.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c student.c -o student_app
```
## 실행 방법
```sh
./student_app
```
## 예상 관찰 결과
`1001 Kim`이 출력된다.
## 확인 포인트
type definition을 복붙하지 않고 header에 ordinary global definition을 넣지 않는다.
## 추가 실습
- ★ ID 검사 함수를 추가한다.
- ★★ header만 include하는 test translation unit을 compile한다.
- ★★★ 공개 이름과 private 이름을 분류한다.
## 완료 기준
공유 선언이 한 header에 있고 두 source가 warning 없이 함께 build된다.
