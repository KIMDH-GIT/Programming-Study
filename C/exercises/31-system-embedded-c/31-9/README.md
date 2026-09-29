# 31-9 실습: register 자료구조의 alignment
이론: [note](../../../notes/31-system-embedded-c/31-9-register-structure-alignment.md)
## 실습 목적
struct member offset이 type alignment requirement를 만족하는지 portable condition으로 검사한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`_Alignof`와 `<stddef.h>`의 `offsetof`를 사용한다. concrete offset 숫자는 현재 ABI 관찰로만 기록하고 device layout과 동일하다고 가정하지 않는다.
## 사용할 개념
alignment, `_Alignof`, `offsetof`, padding, ABI.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o alignment
```
## 실행 방법
```sh
./alignment
```
## 예상 관찰 결과
`aligned_offset=1 object_alignment_ok=1`을 출력한다.
## 확인 포인트
CPU misaligned access 지원과 C pointer validity를 동일시하지 않는다. homemade NULL-pointer offset trick을 쓰지 않는다.
## 추가 실습
- ★ member 순서를 바꾼다.
- ★★ offset과 alignment를 표로 만든다.
- ★★★ ABI와 datasheet 비교 절차를 작성한다.
## 완료 기준
portable alignment condition은 통과하고 host 숫자는 implementation observation으로 표시한다.
