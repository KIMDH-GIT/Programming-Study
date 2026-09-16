# 6-6 실습: signed·unsigned 혼합
이론: [note](../../../notes/06-type-conversions/6-6-signed-unsigned-mixed.md)
## 실습 목적
음수의 unsigned 변환을 이해한다.
## 작성할 파일
`signed_unsigned.c`
## 해야 할 일
`-1` 변환값과 `UINT_MAX`를 출력한다.
## 사용할 개념
공통형, 모듈러 변환, rank.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_unsigned.c -o signed_unsigned
```
## 실행 방법
```sh
./signed_unsigned
```
## 예상 관찰 결과
두 값이 같다.
## 확인 포인트
- UB라고 잘못 분류하지 않았는가?
- 모든 혼합으로 일반화하지 않았는가?
## 추가 실습
- ★ 기초: 0.
- ★★ 응용: -2.
- ★★★ 도전: rank 표.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 결과를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
