# 5-12 실습: format mismatch 분석

이론: [5-12 note](../../../notes/05-input-output/5-12-format-mismatch-ub.md)

## 실습 목적
형식 불일치 진단과 UB를 구별한다.
## 작성할 파일
`format_match.c`
## 해야 할 일
올바른 출력·입력 대응표를 작성하고 정상 코드만 실행한다. 잘못된 예는 진단만 기록한다.
## 사용할 개념
format specifier, 실제 인자형, 포인터형, UB, diagnostic.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic format_match.c -o format_match
```
## 실행 방법
```sh
./format_match
```
## 예상 관찰 결과
정상 코드는 경고 없이 실행된다.
## 확인 포인트
- UB 예제를 실행하지 않았는가?
- 경고와 syntax error를 구별했는가?
## 추가 실습
- ★ **기초:** 대응표를 만든다.
- ★★ **응용:** 진단을 해석한다.
- ★★★ **도전:** ABI 이유를 조사한다.
## 완료 기준
- [ ] 정상 코드가 경고 없이 컴파일된다.
- [ ] 불일치 사례를 올바르게 분류했다.
- [ ] UB를 실행 결과로 정의하지 않았다.
- [ ] 답안 소스를 제공하지 않았다.
