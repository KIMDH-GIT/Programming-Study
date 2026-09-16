# 5-3 실습: 부동소수점 출력 정밀도

이론: [5-3 note](../../../notes/05-input-output/5-3-floating-output-precision.md)

## 실습 목적
`%f`, `%Lf`, 정밀도 표기를 구별한다.
## 작성할 파일
`floating_output.c`
## 해야 할 일
`float`, `double`, `long double` 값을 선언하고 두 가지 정밀도로 출력한다.
## 사용할 개념
기본 인수 승격, `%f`, `%Lf`, `%.nf`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic floating_output.c -o floating_output
```
## 실행 방법
```sh
./floating_output
```
## 예상 관찰 결과
같은 값도 지정한 소수 자릿수에 따라 다르게 표시된다.
## 확인 포인트
- `long double`에 `%Lf`를 썼는가?
- 표시와 저장을 구별했는가?
## 추가 실습
- ★ **기초:** 정밀도 1과 6을 비교한다.
- ★★ **응용:** 0.1을 길게 출력한다.
- ★★★ **도전:** 구현의 `long double` 특성을 기록한다.
## 완료 기준
- [ ] C17 경고 없이 컴파일된다.
- [ ] 세 형의 출력이 맞다.
- [ ] 출력 정밀도의 의미를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
