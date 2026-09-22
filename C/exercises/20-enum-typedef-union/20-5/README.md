# 20-5 실습: `union`의 공유 저장 공간
이론: [note](../../../notes/20-enum-typedef-union/20-5-union-shared-storage.md)

## 실습 목적
union members가 같은 storage를 공유함을 이해한다.
## 작성할 파일
`union_value.c`
## 해야 할 일
`int`와 `double` member를 만들고 저장 직후 같은 member만 출력한다.
## 사용할 개념
union tag, member, designated initializer, shared storage.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic union_value.c -o union_value
```
## 실행 방법
```sh
./union_value
```
## 예상 관찰 결과
각 저장 직후 선택한 member의 값이 출력된다.
## 확인 포인트
inactive member를 변환 수단으로 읽지 않는다.
## 추가 실습
- ★ member 주소를 관찰한다.
- ★★ struct와 크기를 비교한다.
- ★★★ type-punning 위험을 분석한다.
## 완료 기준
공유 storage와 현재 member를 정확히 설명한다.
