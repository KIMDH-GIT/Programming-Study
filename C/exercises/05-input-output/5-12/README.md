# 5-12 실습: format mismatch 분석

이론: [5-12 note](../../../notes/05-input-output/5-12-format-mismatch-ub.md)

## 실습 목적
형식 문자열과 실제 인자형의 계약을 표로 확인하고, 잘못된 대응은 실행하지 않은 채 compiler diagnostic과 C17의 Undefined Behavior를 구별한다.

## 작성할 파일

- `format_match.c`: 올바른 대응만 포함하는 정상 실행 프로그램
- `format_mismatch_diagnostics.c`: 진단 관찰 전용 파일. **compile-only이며 실행하지 않는다.**
- `format_analysis.md`: 올바른 대응표와 진단 분류 기록

## 해야 할 일

1. `format_analysis.md`에 다음 대응을 출력 값형과 입력 포인터형으로 나누어 기록한다.
   - `printf`: `int`/`%d`, `unsigned int`/`%u`·`%x`, `double`/`%f`, `long double`/`%Lf`, `void *`/`%p`
   - `scanf`: `int *`/`%d`, `unsigned int *`/`%u`·`%x`, `float *`/`%f`, `double *`/`%lf`, `long double *`/`%Lf`
2. `format_match.c`에 `int count = 3;`, `double ratio = 0.5;`를 선언한다.
3. 올바른 서식만 사용해 `count=3 ratio=0.50`을 출력한다.
4. 아래 strict build 명령으로 정상 프로그램을 compile하고 실행한다.
5. `format_mismatch_diagnostics.c`에는 다음 네 종류의 **의도적인 잘못된 호출**을 각각 하나씩 만든다.
   - `%d`에 `double` 값을 전달하는 출력 mismatch
   - `%Lf`에 `double` 값을 전달하는 출력 mismatch
   - 입력 `%d`에 `double *`를 전달하는 pointer mismatch
   - 입력 `%lf`에 `float *`를 전달하는 pointer mismatch
6. diagnostic 전용 명령으로 이 파일을 compile-only 검사한다. 실행 파일을 만들거나 호출을 실행하지 않는다.
7. 각 진단에 대해 “요구형”, “실제형”, “C17 분류”, “수정 방향”을 `format_analysis.md`에 기록한다.

## 사용할 개념

format specifier, 실제 값형, 입력 포인터형, 가변 인수 계약, compiler diagnostic, Undefined Behavior.

## 컴파일 방법

정상 프로그램의 최종 strict build:

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror format_match.c -o format_match
```

잘못된 호출의 diagnostic 관찰 전용 compile-only 명령:

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -fsyntax-only format_mismatch_diagnostics.c
```

두 번째 명령에는 의도적인 mismatch가 있으므로 진단이 나오는 것이 목적이다. `-Werror`를 붙여 정상 build와 섞지 않는다.

## 실행 방법

정상 프로그램만 실행한다.

```sh
./format_match
```

`format_mismatch_diagnostics.c`는 어떤 경우에도 실행하지 않는다.

## 예상 관찰 결과

- `format_match.c`는 strict build에서 진단 없이 compile되고 다음 한 줄을 출력한다.

```text
count=3 ratio=0.50
```

- compile-only 검사에서는 네 mismatch에 대해 GCC가 요구형과 실제형이 다르다는 format diagnostic을 낸다. 정확한 문구와 줄 배치는 GCC 버전에 따라 달라도 된다.
- diagnostic은 오류를 찾는 도구의 관찰이고, UB라는 분류는 C17의 함수 계약에서 나온다. “경고가 있으므로 UB” 또는 “경고가 없으므로 안전”이라고 결론 내리지 않는다.

## 확인 포인트

- 정상 파일에는 format mismatch가 하나도 없는가?
- diagnostic 파일의 네 사례에서 요구형과 실제형을 정확히 적었는가?
- `printf` 값형 mismatch와 `scanf` 포인터형 mismatch를 구별했는가?
- compile-only diagnostic 파일을 실행하지 않았는가?
- compiler warning, syntax error, Undefined Behavior를 서로 같은 말로 사용하지 않았는가?

## 추가 실습

- ★ **기초:** `%u`, `%x`, `%p`의 올바른 값형을 표에 추가로 설명한다.
- ★★ **응용:** 인자 개수가 부족한 `printf` 호출을 diagnostic 파일에 추가하고, 실행하지 않은 채 계약 위반을 분류한다.
- ★★★ **도전:** ABI가 정수와 부동소수점 인자를 다른 위치에 전달할 수 있다는 사실이 mismatch 위험을 키우는 이유를 조사한다.

## 완료 기준

- [ ] `format_match.c`가 strict build에서 진단 없이 compile된다.
- [ ] 정상 프로그램의 출력이 `count=3 ratio=0.50`과 일치한다.
- [ ] 대응표에 지정된 출력 값형과 입력 포인터형이 모두 있다.
- [ ] 네 mismatch가 요구형·실제형·C17 분류·수정 방향으로 기록되었다.
- [ ] diagnostic 전용 파일은 `-fsyntax-only`로만 검사했고 실행하지 않았다.
- [ ] compiler diagnostic과 language semantics를 구별해 설명할 수 있다.
- [ ] UB를 우연한 실행 결과로 정의하지 않았다.
- [ ] 답안 소스를 제공하지 않았다.
