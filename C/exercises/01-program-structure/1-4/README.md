# Step 1-4 실습: `printf`와 여러 줄 출력

이론: [1-4 `printf`와 여러 줄 출력](../../../notes/01-program-structure/1-4-printf-multiline-output.md)

완성 답안은 제공하지 않는다. 아래 요구를 만족하는 `multiline.c`와 선택 파일을 학습자가 직접 작성한다.

## 준비

```sh
cd exercises/01-program-structure/1-4
gcc --version
```

## 필수 실습: 한 호출로 두 줄 출력

- **목적:** `printf`가 stdout에 문자열을 쓰고 `\n`이 줄바꿈을 나타냄을 확인한다.
- **해야 할 일:** `multiline.c`에 `<stdio.h>`를 포함하고 `int main(void)`를 작성한다. 한 번의 `printf` 호출에 일반 문자열 하나를 넣어 본인이 정한 두 줄을 출력한다. 각 줄 끝에는 `\n`을 넣고 `main`은 `return 0;`으로 끝낸다. `%` 변환 지정자와 추가 인수는 사용하지 않는다.
- **사용할 개념:** `<stdio.h>`, `printf`, stdout, 문자열 리터럴, newline character, expression statement.
- **예상 관찰 결과:** 아래 명령은 성공하고 터미널에 정확히 두 줄이 보인다. 마지막 줄 뒤에도 줄바꿈이 있다.
- **확인 포인트:** include 지시문 뒤에 세미콜론을 붙이지 않았는가? `printf(...)` 호출 뒤에는 세미콜론이 있는가? `\n`이 문자열 리터럴 안에 있는가? 서식 지정자를 미리 쓰지 않았는가?

```sh
gcc -std=c17 -Wall -Wextra -pedantic multiline.c -o multiline && ./multiline
printf 'status=%s\n' "$?"
```

## 추가 실습

- ★ `two_calls.c`에 같은 두 줄을 `printf` 두 번으로 출력한다. `multiline.c`와 출력 내용은 같게 유지하고 호출 횟수만 비교한다.
- ★★ 정상 빌드한 `multiline`에 `./multiline > captured.txt`를 실행한다. 터미널 대신 `captured.txt`에서 두 줄을 확인하고 C 소스는 변경하지 않는다.

## 완료 기준

- [ ] 학습자 작성 필수 파일 이름은 `multiline.c`다.
- [ ] `<stdio.h>`를 포함하고 한 `printf` 호출로 두 줄을 출력했다.
- [ ] 각 줄 끝의 `\n`과 호출 문장 끝의 세미콜론을 구별했다.
- [ ] stdout 재지정은 셸의 기능이며 `printf`는 stdout에 쓴다는 점을 설명할 수 있다.
