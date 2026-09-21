# 18-14 실습: dangling pointer 분석
이론: [note](../../../notes/18-dynamic-memory/18-14-dangling-pointer-use-after-free.md)

## 실습 목적
use-after-free를 실행하지 않고 lifetime으로 판별한다.
## 작성할 파일
`safe_lifetime.c`
## 해야 할 일
정상 allocation·access·free 순서를 구현하고, note 4절의 분석 전용 fragment에서 free 이후 금지 access를 글로 찾는다.
## 사용할 개념
dangling pointer, alias, undefined behavior.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic safe_lifetime.c -o safe_lifetime
```
## 실행 방법
```sh
./safe_lifetime
```
## 예상 관찰 결과
free 전 값만 출력되고 정상 종료한다.
## 확인 포인트
free 뒤 old pointer를 출력·비교·dereference하지 않으며 분석 fragment도 실행하지 않는다.
## 추가 실습
- ★ lifetime timeline
- ★★ owner/borrower labels
- ★★★ alias invalidation table
## 완료 기준
실행 코드는 정의된 동작만 포함하고 UB는 분석으로만 다룬다.
