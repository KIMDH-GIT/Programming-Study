# Step 1-3 실습: statement, 중괄호, 세미콜론

이론: [1-3 statement, 중괄호, 세미콜론](../../../notes/01-program-structure/1-3-statements-braces-semicolons.md)

완성 답안은 제공하지 않는다. 아래 책임에 맞게 학습자가 직접 `statements.c`를 작성한다.

## 준비

```sh
cd exercises/01-program-structure/1-3
gcc --version
```

## 필수 실습: 문장과 블록 구분

- **목적:** 함수 본문, 널 문장, `return` 문장을 다른 문법 단위로 식별한다.
- **해야 할 일:** `statements.c`에 `int main(void)` 함수 정의를 작성한다. 본문 `{ ... }` 안에는 널 문장 하나와 `return 0;` 하나만 둔다. `}` 뒤에는 세미콜론을 넣지 않는다. 별도 메모에는 각 줄이 함수 정의 머리, 복합 문장 시작, 널 문장, jump statement, 복합 문장 끝 중 무엇인지 적는다.
- **사용할 개념:** statement, compound statement/block, function body, null statement, 세미콜론.
- **예상 관찰 결과:** 아래 명령은 성공하고 화면 출력 없이 성공 종료한다.
- **확인 포인트:** `;`만 있는 줄이 빈 줄이 아니라 널 문장인가? `return 0;`의 세미콜론과 닫는 `}`의 역할을 구별했는가? 모든 statement가 식과 세미콜론의 조합이 아님을 설명할 수 있는가?

```sh
gcc -std=c17 -Wall -Wextra -pedantic statements.c -o statements && ./statements
printf 'status=%s\n' "$?"
```

`printf` 셸 명령은 바로 앞 명령의 상태를 보여 준다. 프로그램의 `printf` 함수와 다른 도구다.

## 추가 실습

- ★ `statements.c`의 널 문장만 삭제한 복사본 `without-null.c`을 작성하고 빌드한다. 실행 결과와 소스의 문장 수를 비교한다.
- ★★ `return 0;`의 세미콜론 하나만 삭제한 복사본 `missing-semicolon.c`을 만든다. 빌드 진단과 실제로 빠진 토큰을 기록한 뒤, 그 파일을 정상 답안으로 바꾸지 않는다.

## 완료 기준

- [ ] 학습자 작성 파일 이름은 `statements.c`다.
- [ ] 함수 본문은 복합 문장이고 그 안에 널 문장 하나가 있다.
- [ ] 함수 정의를 닫는 중괄호 뒤에 세미콜론을 넣지 않았다.
- [ ] 정상 빌드 결과와 의도적인 결함 복사본의 진단을 구별했다.
