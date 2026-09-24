# 24-7 실습: external linkage와 `extern`
이론: [note](../../../notes/24-multi-file-programs/24-7-external-linkage-and-extern.md)
## 실습 목적
shared object의 declaration과 단일 definition을 구성한다.
## 작성할 파일
- `main.c`
- `counter.c`
- `counter.h`
## 해야 할 일
header에는 `extern int app_request_count;`, source에는 initializer가 있는 definition을 둔다.
## 사용할 개념
external linkage, `extern`, object declaration, definition.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c counter.c -o counter_app
```
## 실행 방법
```sh
./counter_app
```
## 예상 관찰 결과
`2`가 출력된다.
## 확인 포인트
`extern`을 file import로 설명하지 않고 definition은 하나만 둔다.
## 추가 실습
- ★ 초기값을 바꾼다.
- ★★ function API를 통해 값을 읽는다.
- ★★★ 세 가지 file-scope declaration을 definition 여부로 분류한다.
## 완료 기준
external object가 단 한 번 정의되고 두 source가 같은 declaration을 공유한다.
