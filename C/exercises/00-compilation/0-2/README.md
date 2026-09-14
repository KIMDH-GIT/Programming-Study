# Step 0-2 실습: C 소스와 헤더

이론: [0-2 C 소스 `.c`와 헤더 `.h`](../../../notes/00-compilation/0-2-c-source-and-header.md)

완성 코드 대신 파일별 책임과 검증 명령을 제공한다. 명령은 GCC/Linux용이고 작성할 코드는 C17이다.

## 준비

`C/` 디렉터리에서 시작한다.

```sh
cd exercises/00-compilation/0-2
gcc --version
```

이후 모든 파일과 명령은 이 디렉터리를 기준으로 한다. 노트의 최소 예제는 매크로를 공유하는 관찰용이다. 아래 필수 과제는 함수 선언과 정의를 직접 분리하는 별도 과제다.

## 필수 실습: 세 파일, 두 번역 단위

- **목적:** 공유 선언과 구현을 분리하고 헤더 포함만으로 정의가 생기지 않음을 확인한다.
- **해야 할 일:** 다음 책임에 맞게 세 파일을 직접 작성한다.
  - `status.h`: 고유한 include guard, 인수를 받지 않고 `int`를 반환하는 `study_status` 함수 선언. 함수 본문은 넣지 않는다.
  - `status.c`: `status.h`를 포함하고 `study_status`를 정의한다. 반환값은 0으로 한다.
  - `main.c`: `status.h`를 포함하고 `int main(void)`를 정의한다. `study_status`를 호출한 결과를 반환한다.
- **사용할 개념:** 함수 선언·정의, include guard, 번역 단위, 링크.

정상 빌드와 실행:

```sh
gcc -std=c17 -Wall -Wextra -pedantic main.c status.c -o status-demo && ./status-demo
echo $?
```

`echo $?`는 바로 앞 명령 목록의 종료 상태를 출력하는 셸 명령이다. 사이에 다른 명령을 실행하지 않는다. 프로그램이 화면에 문구를 출력하지 않아도 오류는 아니다.

정의 파일을 빼는 비교 실험:

```sh
gcc -std=c17 -Wall -Wextra -pedantic main.c -o status-missing
echo $?
```

두 번째 빌드의 실패는 의도한 관찰이다. `status-missing`을 실행하지 말고 오류 메시지와 종료 상태를 기록한다. 오류를 숨기거나 성공한 빌드로 기록하지 않는다.

각 소스를 따로 전처리하여 자신의 선언·정의 부분도 찾아본다.

```sh
gcc -std=c17 -E main.c -o main.i
gcc -std=c17 -E status.c -o status.i
```

- **예상 관찰 결과:** 정상 빌드는 성공 종료한다. `status.c`를 뺀 빌드는 함수 정의를 찾지 못하는 링크 오류가 난다. 각 `.i`에는 해당 소스가 포함한 공통 선언이 반영된다.
- **확인 포인트:** `.h`를 GCC의 독립 입력으로 넣지 않았는가? 두 `.c`가 각각 하나의 번역 단위를 이루는 이유를 설명했는가? guard가 번역 단위를 가로질러 정의를 제거한다고 오해하지 않았는가?

## 추가 실습

- ★ 헤더를 한 소스에서 두 번 포함하고 전처리 결과에서 자신의 함수 선언이 몇 번 남는지 확인한다. "에러가 없다" 대신 결과 내용을 근거로 guard를 설명한다.
- ★★ `status.h`를 직접 만든 `include/` 디렉터리로 옮긴다. `#include "status.h"`는 그대로 두고 다음 명령의 성공과 `-I include`를 뺀 명령의 결과를 비교한다. 동일 이름의 다른 헤더가 주변에 없는지 유의한다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic -I include main.c status.c -o status-demo && ./status-demo
```

## 완료 기준

- [ ] 헤더에는 공유 선언, 소스에는 함수 정의를 배치했다.
- [ ] 정상 빌드와 의도한 실패의 결과를 구별하여 기록했다.
- [ ] 헤더 파일 수와 번역 단위 수가 일치하지 않는 이유를 설명했다.
- [ ] 노트의 확인 문제를 답하고 접힌 해설로 점검했다.
