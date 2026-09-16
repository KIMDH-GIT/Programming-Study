# 14-7 실습: 포인터 자체의 주소

이론: [note](../../../notes/14-pointers/14-7-pointer-object-address.md)
## 실습 목적
pointer value와 pointer object address를 구분한다.
## 작성할 파일
`pointer_object_address.c`
## 해야 할 일
`&number`, pointer, `&pointer`를 `%p`로 출력하고 대상과 type을 적는다.
## 사용할 개념
pointer object, pointer value, address-of pointer, type level.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_object_address.c -o pointer_object_address
```
## 실행 방법
```sh
./pointer_object_address
```
## 예상 관찰 결과
`&number`와 pointer는 같고 `&pointer`는 별도 address로 관찰된다.
## 확인 포인트
pointer-to-pointer를 이중 역참조 실습으로 확장하지 않았는가?
## 추가 실습
- ★ double pointer object - ★★ aliases - ★★★ type diagram
## 완료 기준
- [ ] 경고 없음 - [ ] 세 표현 구분 - [ ] type 설명
