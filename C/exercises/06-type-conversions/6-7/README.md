# 6-7 실습: explicit cast
이론: [note](../../../notes/06-type-conversions/6-7-explicit-cast.md)
## 실습 목적
cast 위치로 계산형을 바꾼다.
## 작성할 파일
`explicit_cast.c`
## 해야 할 일
정수 둘의 평균을 실수로 계산한다.
## 사용할 개념
cast, 대상형, 공통형.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic explicit_cast.c -o explicit_cast
```
## 실행 방법
```sh
./explicit_cast
```
## 예상 관찰 결과
2.5.
## 확인 포인트
- 나눗셈 전에 cast했는가?
- cast 안전성을 과장하지 않았는가?
## 추가 실습
- ★ 기초: 다른 수.
- ★★ 응용: cast 제거 비교.
- ★★★ 도전: 불필요한 cast.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 2.5를 확인했다.
- [ ] 답안 소스를 제공하지 않았다.
