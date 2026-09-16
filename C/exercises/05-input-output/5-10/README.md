# 5-10 실습: 문자·문자열 입력

이론: [5-10 note](../../../notes/05-input-output/5-10-character-string-input.md)

## 실습 목적
공백 처리와 문자열 buffer 한계를 지킨다.
## 작성할 파일
`character_string_input.c`
## 해야 할 일
문자 하나와 `char word[16]`에 한 단어를 입력하고 반환값을 출력한다.
## 사용할 개념
`" %c"`, `%15s`, null 종료, 배열 용량.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic character_string_input.c -o character_string_input
```
## 실행 방법
```sh
./character_string_input
```
## 예상 관찰 결과
정상 입력에서 반환값 2와 두 값이 보인다.
## 확인 포인트
- 폭이 15 이하인가?
- 문자 앞 공백 규칙을 설명했는가?
## 추가 실습
- ★ **기초:** 공백 token을 관찰한다.
- ★★ **응용:** 긴 token을 입력한다.
- ★★★ **도전:** 남은 입력을 기록한다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 배열 범위를 지켰다.
- [ ] `%s` 한계를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
