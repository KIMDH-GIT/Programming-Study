# 24-4 실습: source file과 translation unit
이론: [note](../../../notes/24-multi-file-programs/24-4-source-files-and-translation-units.md)
## 실습 목적
두 source가 각각 preprocessing되어 독립 translation unit이 됨을 확인한다.
## 작성할 파일
- `main.c`
- `message.c`
- `message.h`
## 해야 할 일
예제를 작성하고 `gcc -E` 결과와 정상 build 결과를 비교한다.
## 사용할 개념
preprocessing, source inclusion, translation unit, external reference.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c message.c -o message_app
```
## 실행 방법
```sh
./message_app
```
## 예상 관찰 결과
`two translation units`가 출력된다.
## 확인 포인트
원본 `.c`, preprocessing 결과, object code를 같은 것으로 부르지 않는다.
## 추가 실습
- ★ `gcc -E main.c`에서 declaration을 찾는다.
- ★★ 두 source의 preprocessing 결과를 비교한다.
- ★★★ header 변경의 rebuild 대상을 표시한다.
## 완료 기준
두 translation units와 최종 link의 관계를 말하고 warning 없이 실행한다.
