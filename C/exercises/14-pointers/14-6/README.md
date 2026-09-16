# 14-6 실습: `a`, `&a`, `p`, `&p`, `*p`

이론: [note](../../../notes/14-pointers/14-6-five-expressions.md)
## 실습 목적
다섯 expressions의 type과 의미를 구분한다.
## 작성할 파일
`five_pointer_expressions.c`
## 해야 할 일
`a`, `&a`, `p`, `&p`, `*p`를 올바른 formats로 출력한다.
## 사용할 개념
object value, pointer value, pointer object address, indirection, `%p`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic five_pointer_expressions.c -o five_pointer_expressions
```
## 실행 방법
```sh
./five_pointer_expressions
```
## 예상 관찰 결과
`a == *p`이고 `&a`와 p가 같은 pointer 표현으로 보인다.
## 확인 포인트
`&p`를 별도 object address로 설명하는가?
## 추가 실습
- ★ type 표 - ★★ 수정 후 확인 - ★★★ diagram
## 완료 기준
- [ ] 경고 없음 - [ ] formats 정확 - [ ] 다섯 의미 구분
