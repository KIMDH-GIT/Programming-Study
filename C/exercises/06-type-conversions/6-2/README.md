# 6-2 실습: 정수 범위 축소
이론: [note](../../../notes/06-type-conversions/6-2-integer-narrowing.md)
## 실습 목적
unsigned 축소 변환을 한계 매크로와 연결한다.
## 작성할 파일
`integer_narrowing.c`
## 해야 할 일
`UCHAR_MAX+1`을 `unsigned char`로 변환해 출력한다.
## 사용할 개념
범위, unsigned 변환, `UCHAR_MAX`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_narrowing.c -o integer_narrowing
```
## 실행 방법
```sh
./integer_narrowing
```
## 예상 관찰 결과
축소 결과 0이 보인다.
## 확인 포인트
- 표준 관계로 설명했는가?
- signed 결과를 일반화하지 않았는가?
## 추가 실습
- ★ 기초: 범위 안 값.
- ★★ 응용: 최댓값+2.
- ★★★ 도전: signed 규칙 조사.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] unsigned 규칙을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
