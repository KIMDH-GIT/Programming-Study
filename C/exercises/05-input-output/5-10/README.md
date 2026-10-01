# 5-10 실습: 문자·문자열 입력

이론: [5-10 note](../../../notes/05-input-output/5-10-character-string-input.md)

## 실습 목적
공백 처리와 문자열 buffer 한계를 지킨다.

## 작성할 파일
`character_string_input.c`

## 해야 할 일

1. `char letter = '?';`, `char word[16] = "";`, `char next = '?';`를 선언한다.
2. 첫 `scanf`에서 `" %c %15s"`로 문자와 단어를 읽고 반환값을 `matched`에 저장한다.
3. 두 번째 `scanf`에서 `" %c"`로 다음 문자 하나를 읽고 반환값을 `next_matched`에 저장한다.
4. `matched`, `letter`, `word`, `next_matched`, `next`를 출력한다.
5. 짧은 token, 정확히 15자인 token, 16자인 token을 각각 새 프로세스로 실행한다.
6. 16자 token에서 `%15s`가 저장하지 않은 마지막 문자를 두 번째 `scanf`가 읽는지 확인한다.

## 사용할 개념
`" %c"`, `%15s`, null 종료, 배열 용량, field width, stream에 남은 입력, assignment count.

## 컴파일 방법

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror character_string_input.c -o character_string_input
```

## 실행 방법

짧은 token:

```sh
printf '%s\n' 'Q hello X' | ./character_string_input
```

정확히 15자인 token:

```sh
printf '%s\n' 'Q abcdefghijklmno X' | ./character_string_input
```

16자인 token:

```sh
printf '%s\n' 'Q abcdefghijklmnop' | ./character_string_input
```

## 예상 관찰 결과

| 사례 | `matched` | `letter` | `word` | `next_matched` | `next` |
|---|---:|---|---|---:|---|
| `Q hello X` | 2 | `Q` | `hello` | 1 | `X` |
| `Q abcdefghijklmno X` | 2 | `Q` | `abcdefghijklmno` | 1 | `X` |
| `Q abcdefghijklmnop` | 2 | `Q` | `abcdefghijklmno` | 1 | `p` |

16칸 배열에는 앞의 15문자와 종료 null 문자가 저장된다. 16번째 문자 `p`는 첫 `%15s`가 소비하지 않으므로 다음 변환이 읽을 수 있다.

## 확인 포인트

- 폭이 15 이하인가?
- 문자 앞 공백 규칙을 설명했는가?
- 15자 token과 16자 token에서 `word`가 같은 15문자를 저장하는가?
- 16자 token의 마지막 `p`가 stream에 남았다가 `next`에 저장되는가?
- `%15s`가 입력 전체를 버리는 기능이 아니라 최대 저장 문자 수를 제한하는 기능임을 설명했는가?

## 추가 실습

- ★ **기초:** 문자 앞 공백을 여러 개 넣어 `" %c"`가 건너뛰는 범위를 관찰한다.
- ★★ **응용:** 20자 token을 입력하고 첫 15문자와 다음 한 문자를 기록한다.
- ★★★ **도전:** `%s`가 공백 포함 한 줄 전체를 읽지 못하는 이유와 이후 `fgets`가 필요한 상황을 설명한다.

## 완료 기준

- [ ] strict C17 옵션과 `-Werror`에서 진단 없이 compile된다.
- [ ] 세 boundary 실행을 모두 기록했다.
- [ ] 모든 첫 호출에서 `matched=2`를 확인했다.
- [ ] 15자·16자 입력에서 `word=abcdefghijklmno`를 확인했다.
- [ ] 16자 입력에서 `next=p`를 확인했다.
- [ ] 배열 16칸 중 한 칸이 null 문자에 필요함을 설명했다.
- [ ] 남은 입력이 다음 변환에 영향을 주는 이유를 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
