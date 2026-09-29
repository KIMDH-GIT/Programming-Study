# 31-13 실습: driver function pointer 호환형
이론: [note](../../../notes/31-system-embedded-c/31-13-compatible-driver-function-pointers.md)
## 실습 목적
정확히 compatible한 callback typedef와 mock function을 연결한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`driver_read_fn` typedef를 정의하고 같은 return/parameter type의 mock을 indirect call한다. incompatible cast를 사용하지 않으며 NULL context/output failure를 분리한다.
## 사용할 개념
compatible function type, function pointer, `void *context`, status/output.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o driver_function
```
## 실행 방법
```sh
./driver_function
```
## 예상 관찰 결과
정상 호출은 `ok=1 value=42`다. NULL 인자는 status 0이며 output을 변경하지 않는다.
## 확인 포인트
cast가 compatibility를 만들지 않는다. ABI에서 우연히 호출돼 보이는 결과에 의존하지 않는다.
## 추가 실습
- ★ write callback type을 추가한다.
- ★★ const context read callback을 설계한다.
- ★★★ incompatible prototype은 compile diagnostic으로만 분석한다.
## 완료 기준
compatible type 호출과 failure contract가 정확하다.
