# Step 1-1 실습: 첫 프로그램과 `int main(void)`

이론: [1-1 첫 프로그램과 `int main(void)`](../../../notes/01-program-structure/1-1-first-program-main.md).

완성 소스는 제공하지 않는다. 학습자가 `first-program.c`를 직접 작성하고 관찰을 기록한다.

## 준비

- **[GCC/Linux 구현]** Linux 셸과 GCC 개발 환경을 사용한다.
- `C/` 디렉터리에서 이 디렉터리로 이동한다.
- 학습자가 만들 소스 파일 이름은 `first-program.c`이다.

```sh
cd exercises/01-program-structure/1-1
gcc --version
```

노트의 최소 예제를 읽고 `first-program.c`를 직접 입력한다. 문자열은 짧은 ASCII 인사말로 바꾸되 `int main(void)`과 `return 0;`은 유지한다.

## 필수 과제: 시작 함수와 실행 결과 기록

- **목적:** `main` 선언, 출력, 종료 상태를 구별한다.
- **해야 할 일:** 소스를 경고 옵션과 함께 빌드하고, 성공했을 때만 실행한다. 문자열을 바꾼 뒤 재빌드·실행한다.
- **사용할 개념:** `int main(void)`, `puts`, `return 0;`, `$?`, `&&`.
- **예상 관찰 결과:** 선택한 문장이 한 줄 출력되고 **[GNU/Linux 구현]** 셸에서 보통 상태 0이다. 빌드만으로는 문장이 출력되지 않는다.
- **확인 포인트:** `int`가 반환형이고 `(void)`가 매개변수 없음을 뜻하는가? 상태를 읽기 전에 다른 명령을 실행하지 않았는가?

```sh
gcc -std=c17 -Wall -Wextra -pedantic first-program.c -o first-program
printf 'build status=%s\n' "$?"
./first-program
printf 'run status=%s\n' "$?"
```

성공한 빌드 결과만 실행하려면 다음을 사용한다.

```sh
gcc -std=c17 -Wall -Wextra -pedantic first-program.c -o first-program && ./first-program
```

## 기록할 내용

| 항목 | 직접 기록 |
|---|---|
| 만든 소스 파일 이름 | |
| 빌드 명령과 상태 | |
| 실행 명령과 상태 | |
| 수정 전·후 출력 | |
| C 표준의 사실 한 가지 | |
| GCC/Linux 구현 관찰 한 가지 | |

## 추가 과제

- ★ `implicit-return.c`를 직접 만들어 명시적 `return 0;`과 비교한다.
- ★★ `first-program.c`를 `-S`로 번역해 `first-program.s`에서 `main` 기호를 찾는다.

## 완료 기준

- [ ] 직접 작성한 `first-program.c`가 있다.
- [ ] 빌드 상태와 실행 상태를 분리해 기록했다.
- [ ] `int main(void)`과 `return 0;`의 역할을 설명했다.
- [ ] C 추상 환경의 `main` 호출과 OS 구현 경로를 구별했다.
