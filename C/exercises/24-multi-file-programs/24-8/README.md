# 24-8 실습: internal linkage와 file-scope `static`
이론: [note](../../../notes/24-multi-file-programs/24-8-internal-linkage-and-file-scope-static.md)
## 실습 목적
public function과 file-local helper를 분리한다.
## 작성할 파일
- `main.c`
- `calculator.c`
- `calculator.h`
## 해야 할 일
`calculator_absolute`는 공개하고 `normalize_sign`은 source 안의 `static` helper로 만든다.
## 사용할 개념
internal linkage, external linkage, file scope, private implementation.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c calculator.c -o calculator_app
```
## 실행 방법
```sh
./calculator_app
```
## 예상 관찰 결과
`7`이 출력된다.
## 확인 포인트
private helper declaration을 public header에 넣지 않는다.
## 추가 실습
- ★ helper 이름을 바꾼다.
- ★★ 두 source에 같은 static helper 이름을 쓴다.
- ★★★ 공개·private 이름 표를 만든다.
## 완료 기준
공개 API만 header에 있고 file-local helper를 통해 warning 없이 실행된다.
