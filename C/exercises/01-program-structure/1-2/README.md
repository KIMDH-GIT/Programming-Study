# Step 1-2 실습: `#include`와 표준 헤더

이론: [1-2 `#include`와 표준 헤더](../../../notes/01-program-structure/1-2-include-standard-headers.md).

완성 소스는 제공하지 않는다. 학습자가 `include-stdio.c`를 직접 작성해 선언과 구현을 구별해 관찰한다.

## 준비

- **[GCC/Linux 구현]** Linux 셸과 GCC 개발 환경을 사용한다.
- `C/` 디렉터리에서 이 디렉터리로 이동한다.
- 학습자가 만들 소스 파일은 `include-stdio.c`이며, 추가 과제에는 `missing-declaration.c`를 만든다.

```sh
cd exercises/01-program-structure/1-2
gcc --version
```

노트의 최소 예제를 읽고 `<stdio.h>`와 `puts`를 사용한 `include-stdio.c`를 직접 작성한다. 출력 문자열은 짧은 ASCII 문장으로 바꾼다.

## 필수 과제: 전처리 입력과 실행 결과 비교

- **목적:** `#include`가 runtime import가 아닌 전처리 지시문이며 헤더가 선언을 제공함을 확인한다.
- **해야 할 일:** `include-stdio.i`를 만들고 원본 및 `.i`에서 `puts`를 찾는다. 그 뒤 원본을 빌드·실행해 출력과 상태를 기록한다.
- **사용할 개념:** `#include <stdio.h>`, 전처리, 함수 선언, 링크, `gcc -E`.
- **예상 관찰 결과:** `.i`는 원본보다 길 수 있고 `puts` 선언과 호출을 찾을 수 있다. 전처리는 프로그램을 실행하지 않는다.
- **확인 포인트:** include 줄에 세미콜론이 없는가? 헤더 선언과 링크되는 구현을 구별했는가? `.i`를 실행 파일로 취급하지 않았는가?

```sh
gcc -std=c17 -E include-stdio.c -o include-stdio.i
grep -n 'puts' include-stdio.i
gcc -std=c17 -Wall -Wextra -pedantic include-stdio.c -o include-stdio
printf 'build status=%s\n' "$?"
./include-stdio
printf 'run status=%s\n' "$?"
```

성공한 빌드 결과만 실행하려면 다음을 사용한다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic include-stdio.c -o include-stdio && ./include-stdio
```

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| 만든 소스 파일 이름 | |
| 전처리 명령과 `.i` 파일 | |
| `.i`에서 찾은 `puts` 흔적 | |
| 빌드·실행 상태와 출력 | |
| 헤더 선언과 라이브러리 구현의 차이 | |

## 추가 과제

- ★ `missing-declaration.c`에서 `<stdio.h>`를 빼고 진단을 기록한다.
- ★★ `gcc -E -H include-stdio.c -o /dev/null`로 GCC의 헤더 포함 표시를 관찰한다.
- ★★★ `<...>`와 `"..."`의 검색 차이를 말로 정리한다.

## 완료 기준

- [ ] 직접 작성한 `include-stdio.c`가 있다.
- [ ] 전처리 결과와 실행 파일을 구별했다.
- [ ] `#include`가 runtime import가 아님을 설명했다.
- [ ] `<stdio.h>` 선언과 링크되는 `puts` 구현을 구별했다.
