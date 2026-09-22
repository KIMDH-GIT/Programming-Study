# 19-3 실습: `typedef struct`
이론: [note](../../../notes/19-structures/19-3-typedef-struct.md)

## 실습 목적
structure tag와 typedef alias를 구분한다.
## 작성할 파일
`student_typedef.c`
## 해야 할 일
`struct Student`를 정의하고 `Student` alias를 선언한 뒤 두 표기로 object를 만들어 assignment한다.
## 사용할 개념
structure tag, typedef name, namespace, assignment.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_typedef.c -o student_typedef
```
## 실행 방법
```sh
./student_typedef
```
## 예상 관찰 결과
두 표기가 같은 structure type을 나타내어 값이 정상 출력된다.
## 확인 포인트
typedef 이전에는 `Student`만으로 object를 선언하지 않는다.
## 추가 실습
- ★ 결합형 선언을 사용한다.
- ★★ alias 없이 같은 프로그램을 작성한다.
- ★★★ pointer typedef의 장단점을 적는다.
## 완료 기준
tag와 alias의 역할을 말로 설명하고 warning 없이 실행한다.
