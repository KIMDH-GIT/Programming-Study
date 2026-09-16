# 5-4 실습: 문자와 문자열 출력

이론: [5-4 note](../../../notes/05-input-output/5-4-character-string-output.md)

## 실습 목적
`%c`, `%s`, 문자열 정밀도를 구별한다.
## 작성할 파일
`character_string_output.c`
## 해야 할 일
문자 하나, 문자열 전체, 문자열 앞 네 문자를 출력한다.
## 사용할 개념
문자 상수, 문자열 리터럴, `%c`, `%s`, 정밀도.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic character_string_output.c -o character_string_output
```
## 실행 방법
```sh
./character_string_output
```
## 예상 관찰 결과
문자 하나와 문자열 두 형태가 표시된다.
## 확인 포인트
- 인자 종류가 서식과 맞는가?
- null 종료의 역할을 설명했는가?
## 추가 실습
- ★ **기초:** 다른 문자를 출력한다.
- ★★ **응용:** 정밀도를 바꾼다.
- ★★★ **도전:** 종료 없는 문자열의 위험을 설명한다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 세 출력이 예상과 맞다.
- [ ] `%c`와 `%s`를 구별했다.
- [ ] 답안 `.c`를 제공하지 않았다.
