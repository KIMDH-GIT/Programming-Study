# 6-4 실습: integer promotion
이론: [note](../../../notes/06-type-conversions/6-4-integer-promotion.md)
## 실습 목적
작은 정수형의 중간 계산형을 이해한다.
## 작성할 파일
`integer_promotion.c`
## 해야 할 일
작은 정수 둘을 더해 넓은 결과형에 저장한다.
## 사용할 개념
promotion, 중간형, 값 보존.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic integer_promotion.c -o integer_promotion
```
## 실행 방법
```sh
./integer_promotion
```
## 예상 관찰 결과
합 250.
## 확인 포인트
- 선언형과 계산형을 구별했는가?
- 구현 조건을 붙였는가?
## 추가 실습
- ★ 기초: signed char.
- ★★ 응용: short.
- ★★★ 도전: 다른 범위 모델.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] promotion을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
