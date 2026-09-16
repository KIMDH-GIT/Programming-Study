# 15-7 실습: 포인터 차이와 `ptrdiff_t`

이론: [note](../../../notes/15-arrays-and-pointers/15-7-pointer-difference.md)
## 실습 목적
same-array pointer distance를 element positions로 계산한다.
## 작성할 파일
`pointer_difference.c`
## 해야 할 일
indices 1과 4 pointers의 양방향 차이를 `%td`로 출력한다.
## 사용할 개념
pointer subtraction, same array, `ptrdiff_t`, `%td`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_difference.c -o pointer_difference
```
## 실행 방법
```sh
./pointer_difference
```
## 예상 관찰 결과
3과 -3이 출력된다.
## 확인 포인트
byte 차이가 아니라 element position 차이라고 설명하는가?
## 추가 실습
- ★ one-past distance - ★★ negative - ★★★ validity 분류
## 완료 기준
- [ ] 경고 없음 - [ ] 3/-3 정확 - [ ] same-array 전제
