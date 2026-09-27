# 27-5 실습: 배열 경계 검사
이론: [note](../../../notes/27-undefined-behavior/27-5-array-out-of-bounds.md)
## 실습 목적
원소 access 전에 index 경계를 확인한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`print_at`을 구현해 유효한 index만 읽고 성공 여부를 반환한다.
## 사용할 개념
array bound, `size_t`, one-past pointer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bounds
```
## 실행 방법
out-of-bounds 코드는 실행하지 않는다. 검사된 version만 실행한다.
```sh
./bounds
```
## 예상 관찰 결과
index 2에서 `3`을 출력하고 index 3 요청은 실패한다.
## 확인 포인트
one-past pointer 형성과 역참조를 구분한다.
## 추가 실습
- ★ 양 끝 index를 검사한다.
- ★★ signed 입력을 안전하게 변환한다.
- ★★★ pointer loop로 같은 검사를 구현한다.
## 완료 기준
어떤 path에서도 경계 밖 원소를 access하지 않는다.
