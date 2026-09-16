# 14-2 실습: 주소 연산자 `&`

이론: [note](../../../notes/14-pointers/14-2-address-operator.md)
## 실습 목적
address operator result를 typed pointer object에 저장한다.
## 작성할 파일
`address_operator.c`
## 해야 할 일
int와 double objects에 각각 맞는 pointer를 만들고 addresses를 출력한다.
## 사용할 개념
unary `&`, pointer value, pointer object, pointed-to type.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic address_operator.c -o address_operator
```
## 실행 방법
```sh
./address_operator
```
## 예상 관찰 결과
각 pointer와 대응 object address가 같은 표현으로 관찰된다.
## 확인 포인트
각 pointer type이 pointed-to object type과 맞는가?
## 추가 실습
- ★ double pointer - ★★ aliases - ★★★ diagram
## 완료 기준
- [ ] 경고 없음 - [ ] `%p` 사용 - [ ] type 관계 설명
