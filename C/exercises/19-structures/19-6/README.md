# 19-6 실습: `p->id`와 `(*p).id`
이론: [note](../../../notes/19-structures/19-6-arrow-and-dereference-member-access.md)
## 실습 목적
두 pointer member access 표기의 관계를 확인한다.
## 작성할 파일
`arrow_equivalence.c`
## 해야 할 일
같은 Student를 `p->member`와 `(*p).member`로 읽고 수정한다.
## 사용할 개념
dereference, precedence, `.`, `->`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic arrow_equivalence.c -o arrow_equivalence
```
## 실행 방법
```sh
./arrow_equivalence
```
## 예상 관찰 결과
두 표기로 읽은 값이 같다.
## 확인 포인트
`(*p)` 괄호를 생략하지 않는다.
## 추가 실습
- ★ 모든 `->`를 동등한 표기로 바꾼다.
- ★★ precedence 표를 확인한다.
- ★★★ const pointer에 적용한다.
## 완료 기준
두 표현의 동일 member 지정 이유를 설명한다.
