# Step 0-3 실습: 전처리 결과 관찰

이론: [0-3 전처리기와 `.i` 파일](../../../notes/00-compilation/0-3-preprocessor.md)

완성 코드는 제공하지 않는다. 노트의 최소 예제를 직접 입력한 뒤 요구사항에 따라 확장한다. 코드는 C17, 명령은 GCC/Linux 기준이다.

## 준비

`C/` 디렉터리에서 시작한다.

```sh
cd exercises/00-compilation/0-3
gcc --version
```

이 디렉터리에 `main.c`를 작성한다. 노트의 최소 예제를 바탕으로 일반 식별자 `MESSAGE`, 문자열 `"MESSAGE"`, 주석의 차이를 관찰할 수 있게 유지한다.

## 필수 실습: 정의 여부로 코드 선택하기

- **목적:** 토큰 치환과 조건부 포함을 실제 `.i`에서 확인하고 실행 결과와 연결한다.
- **해야 할 일:** `main` 안에 `#if defined(VERBOSE)`·`#else`·`#endif`를 추가한다. 정의된 경우 `verbose`, 아니면 `quiet`라는 ASCII 문구를 `puts`로 출력하는 호출 하나가 선택되도록 직접 작성한다. 소스에서 `VERBOSE`를 정의하지 말고 명령줄 조건을 사용한다.
- **사용할 개념:** 객체형 매크로, 문자열 토큰, 주석, 조건부 포함, `-E`, `-D`.

세 전처리 결과를 각각 생성한다. 각 명령의 성공을 확인한 후 진행한다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic -E main.c -o main-default.i
gcc -std=c17 -Wall -Wextra -pedantic -DVERBOSE=1 -E main.c -o main-one.i
gcc -std=c17 -Wall -Wextra -pedantic -DVERBOSE=0 -E main.c -o main-zero.i
```

편집기 또는 `less`에서 각 파일의 `int main`을 찾는다. 다음 항목을 스스로 기록한다.

1. 매크로 사용 위치에 남은 토큰과 문자열 내부에 남은 글자.
2. 주석 및 원래 `#define`·`#if` 지시문이 기본 출력에 남아 있는지.
3. 두 조건 그룹 중 어느 호출이 남았는지.
4. 줄 표시자가 가리키는 파일과 원본 위치.

원본이 아니라 생성된 `.i`를 각각 빌드하고 실행한다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic main-default.i -o preprocess-default && ./preprocess-default
gcc -std=c17 -Wall -Wextra -pedantic main-one.i -o preprocess-one && ./preprocess-one
gcc -std=c17 -Wall -Wextra -pedantic main-zero.i -o preprocess-zero && ./preprocess-zero
```

- **예상 관찰 결과:** 기본 경우와 매크로가 정의된 경우는 서로 다른 호출이 남는다. `=1`과 `=0`은 정의 여부 검사에서 같은 그룹을 선택한다. 문자열 내부 글자는 매크로 확장을 받지 않는다.
- **확인 포인트:** 선택되지 않은 호출이 전처리 결과에 없는가? 전처리만 했을 때와 실행했을 때의 출력을 구별했는가? 시스템 헤더의 줄 수나 설치 경로를 고정된 정답으로 삼지 않았는가?

## 추가 실습

- ★ 줄 표시자를 억제하여 읽기 쉬운 출력을 만든다. 기본 `.i`와 자신의 함수 내용 및 위치 정보의 차이를 비교한다.

```sh
gcc -std=c17 -E -P main.c -o main-plain.i
```

- ★★ 출력용 매크로를 프로젝트 헤더로 옮겨 포함한다. 전처리한 뒤 헤더의 문자열만 수정한다. 기존 `.i`를 빌드·실행하고, 원본을 새 이름의 `.i`로 다시 전처리하여 빌드·실행한다. 헤더 수정이 어느 결과에 반영되는지 기록한다. 기존 비교 파일을 덮어쓰기 전에 관찰을 마친다.

## 완료 기준

- [ ] 세 설정의 전처리 결과와 실제 실행 결과를 비교했다.
- [ ] "정의되어 있음"과 "값이 0이 아님"을 구별했다.
- [ ] `.i`가 실행 파일이 아니며 새 소스 변경을 자동 반영하지 않음을 설명했다.
- [ ] 노트의 확인 문제를 먼저 풀고 접힌 해설로 확인했다.
