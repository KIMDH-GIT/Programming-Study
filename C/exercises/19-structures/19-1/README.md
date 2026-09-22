# 19-1 실습: 구조체 정의와 멤버
이론: [note](../../../notes/19-structures/19-1-structure-definition-and-members.md)

## 실습 목적
structure tag, member, object declaration을 구분한다.
## 작성할 파일
`student_structure.c`
## 해야 할 일
`id`, `score`, `grade`를 가진 `struct Student`를 정의하고 객체를 초기화해 각 member를 출력한다.
## 사용할 개념
`struct`, tag, member, object, aggregate initialization.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_structure.c -o student_structure
```
## 실행 방법
```sh
./student_structure
```
## 예상 관찰 결과
초기화한 학번, 점수, 등급이 순서대로 출력된다.
## 확인 포인트
객체 선언에서 `struct Student`를 사용하고 배열이라고 설명하지 않는다.
## 추가 실습
- ★ member 값을 바꾼다.
- ★★ 다른 structure type을 정의한다.
- ★★★ 두 type이 같은 member 이름을 가져도 충돌하지 않는지 확인한다.
## 완료 기준
warning 없이 build되고 세 member 값이 정확히 출력된다.
