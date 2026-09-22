# 19-9 실습: `strtod`로 실수 입력 검증
이론: [note](../../../notes/19-structures/19-9-strtod-floating-input-validation.md)
## 실습 목적
학생 점수 문자열을 완전히 검증한다.
## 작성할 파일
`parse_score.c`
## 해야 할 일
`fgets`의 EOF·오류·truncation을 처리하고 `strtod`로 0~100의 유한한 점수만 출력한다.
## 사용할 개념
`errno`, end pointer, `isfinite`, domain range.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic parse_score.c -lm -o parse_score
```
## 실행 방법
```sh
./parse_score
```
## 예상 관찰 결과
정상 점수만 승인되고 NaN·무한대·garbage·범위 밖·overlong 입력은 거부된다.
## 확인 포인트
newline 제거 뒤 문자열 전체 소비, `ERANGE`, `isfinite`를 검사한다.
## 추가 실습
- ★ 경계를 시험한다.
- ★★ 지수 표기를 시험한다.
- ★★★ 재시도한다.
## 완료 기준
모든 오류 class를 구분해 처리한다.
