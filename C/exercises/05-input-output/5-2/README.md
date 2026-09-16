# 5-2 실습: 정수 출력 서식

이론: [5-2 note](../../../notes/05-input-output/5-2-integer-output-formats.md)

## 실습 목적
signed 10진, unsigned 10진, unsigned 16진 출력을 구별한다.
## 작성할 파일
`integer_output.c`
## 해야 할 일
`int` 음수와 `unsigned int` 양수를 선언해 알맞은 서식으로 출력한다.
## 사용할 개념
`%d`, `%u`, `%x`, 필드 폭, 0 채움.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_output.c -o integer_output
```
## 실행 방법
```sh
./integer_output
```
## 예상 관찰 결과
양수는 10진과 16진 두 표기로 보인다.
## 확인 포인트
- 실제 인자형과 서식이 맞는가?
- `0x`를 일반 문자로 썼는가?
## 추가 실습
- ★ **기초:** 255U를 출력한다.
- ★★ **응용:** 필드 폭을 비교한다.
- ★★★ **도전:** 출력과 객체 표현을 구별한다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 서식의 역할을 설명했다.
- [ ] 형식 불일치를 만들지 않았다.
- [ ] 답안 `.c`를 제공하지 않았다.
