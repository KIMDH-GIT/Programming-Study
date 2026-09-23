# 23-2. 파일 모드와 열기 실패
## 1. 학습 목표
- `r`, `w`, `a`와 update·binary mode의 의미를 구분한다.
- `w`의 truncation과 `a`의 append semantics를 설명한다.
- update stream의 read/write 전환 규칙을 안다.
## 2. 선수 지식
23-1의 `fopen`, null check, `fclose`를 안다.
## 3. 핵심 개념
| mode | 기존 file | 시작 동작 | 허용 operation |
|---|---|---|---|
| `"r"` | 필요 | beginning | read |
| `"w"` | 없어도 됨 | create 또는 truncate | write |
| `"a"` | 없어도 됨 | create, writes at end | write |
| `"r+"` | 필요 | beginning | read/write |
| `"w+"` | 없어도 됨 | create 또는 truncate | read/write |
| `"a+"` | 없어도 됨 | initial read position은 implementation-defined, writes at end | read/write |

`b`를 붙인 `"rb"`, `"wb"`, `"ab"` 등은 binary mode다. `"w"`가 성공해 기존 file을 열면 이전 contents는 버려진다. append mode에서는 output이 file end에 기록되므로 `fseek`로 다른 position을 선택했다고 앞부분을 덮어쓰는 mode가 아니다. `"a+"`에서 읽을 위치가 중요하면 initial position을 가정하지 말고 positioning function으로 명시해야 한다.
## 4. 문법
update stream에서는 output 뒤 input 전에 `fflush` 또는 positioning function이 필요하다. input 뒤 output 전에는 positioning function이 필요하지만 input operation이 end-of-file을 만난 경우는 예외다. `"r+"`가 아무 sequencing 없이 자유롭게 read/write를 섞는 mode라는 뜻은 아니다.

`fflush(stdin)`은 portable C17 input clearing 방법이 아니다. output flush 성공도 physical disk persistence를 보장하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_modes.txt", "w");

    if (fp == NULL) {
        return 1;
    }
    if (fputs("first\n", fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_modes.txt", "a");
    if (fp == NULL) {
        return 1;
    }
    if (fputs("second\n", fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    puts("created then appended");
    return 0;
}
```
## 6. 코드 해석
첫 open은 학습용 file을 새 contents로 만든다. 두 번째 open은 기존 contents 뒤에 한 line을 추가한다. 모든 open·write·close 결과를 검사한다.
## 7. 내부 동작
**[C17 stdio stream]** mode string은 stream의 read/write 가능성, initial position, create/truncate/append behavior를 정한다.

**[C standard library]** update stream의 방향 전환에는 sequencing 규칙이 있다.

**[OS file / file descriptor]** C mode가 특정 POSIX flag 조합과 항상 일대일 대응한다고 단정하지 않는다.

**[filesystem]** open failure에는 path나 permission 문제가 있을 수 있지만 C17이 host permission model을 정의하지 않는다.

**[device / storage]** stdio buffer flush와 durable storage commit은 다른 층이다.
## 8. 자주 하는 실수
- `"w"`가 기존 contents를 보존한다고 생각한다.
- `"a"`에서 seek한 위치에 overwrite할 수 있다고 생각한다.
- `"r+"`에서 read와 write를 아무 call 없이 번갈아 한다.
- `"b"`가 모든 platform에서 memory representation의 portability를 보장한다고 생각한다.
- `fflush(stdin)`을 portable input cleanup으로 사용한다.
## 9. 필수 실습
안전한 학습용 file에 `"w"`로 한 line을 저장한 뒤 `"a"`로 두 번째 line을 추가한다.
[23-2 exercise](../../exercises/23-file-io/23-2/README.md)
## 10. 추가 실습
- ★ 두 번째 실행에서 append 결과를 확인한다.
- ★★ 각 mode의 create·truncate behavior를 표로 만든다.
- ★★★ update stream sequencing 규칙을 작은 state diagram으로 정리한다.
## 11. 확인 문제
1. `"r"`은 존재하지 않는 file을 생성하는가?
2. `"w"`로 기존 file을 열면 어떻게 되는가?
3. `"a"` mode output position은 어떻게 정해지는가?
4. text와 binary mode를 구분하는 suffix는?
5. update stream에서 방향 전환 규칙이 필요한 이유는?
6. `fflush` 성공이 disk persistence를 보장하는가?
## 12. 핵심 정리
- mode마다 existence, truncation, append, access 권한이 다르다.
- update stream에는 read/write sequencing 규칙이 있다.
- file path와 permission은 host environment 특성이다.
## 13. 다음 Step
[23-3. `fprintf`와 `fscanf`](23-3-fprintf-and-fscanf.md)
## 14. 참고 자료
- N1570 7.21.2, 7.21.3, 7.21.5.3, 7.21.5.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fopen](https://en.cppreference.com/w/c/io/fopen)
- [cppreference: fflush](https://en.cppreference.com/w/c/io/fflush)
