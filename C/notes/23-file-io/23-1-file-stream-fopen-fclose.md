# 23-1. `FILE *`, stream, `fopen`, `fclose`
## 1. 학습 목표
- `FILE`, `FILE *`, file name, stream을 구분한다.
- `fopen` 실패를 검사하고 열린 stream을 `fclose`로 닫는다.
- C stdio와 OS file descriptor를 같은 개념으로 설명하지 않는다.
## 2. 선수 지식
Part 13 C string, Part 14 pointer, Part 17 `const`를 안다.
## 3. 핵심 개념
```c
FILE *fp = fopen("training.txt", "w");
```
`FILE`은 C standard I/O library가 stream 상태를 나타내는 데 사용하는 object type이다. `FILE *`는 그 stream object를 다루는 pointer type이고, `fp`는 pointer value를 저장하는 object다. file name, stream, `FILE *`는 서로 같은 것이 아니다.

`FILE`의 내부 layout은 portable interface가 아니다. 특정 `fd`, buffer, offset member가 반드시 있다고 가정하거나 member에 직접 접근하지 않는다.
## 4. 문법
```c
#include <stdio.h>

FILE *fopen(const char *restrict filename,
            const char *restrict mode);
int fclose(FILE *stream);
```
`fopen`은 성공하면 stream을 가리키는 pointer를, 실패하면 null pointer를 반환한다. `fclose`는 성공 시 0, 실패 시 `EOF`를 반환한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_step1.txt", "w");

    if (fp == NULL) {
        fputs("open failed\n", stderr);
        return 1;
    }
    if (fputc('A', fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    puts("saved");
    return 0;
}
```
## 6. 코드 해석
학습용 file name으로 output stream을 연다. open과 write 결과를 검사하고 사용이 끝난 stream을 한 번만 닫는다.
## 7. 내부 동작
**[C17 stdio stream]** stream은 input/output function과 연결된 추상화이며 position, indicators, buffering 관련 상태를 가진다.

**[C standard library]** `fopen`·`fclose`와 `FILE *` interface를 제공한다.

**[OS file / file descriptor]** hosted implementation은 OS handle이나 file descriptor를 사용할 수 있지만 `FILE *`가 POSIX `int fd` 그 자체라는 C17 보장은 없다.

**[filesystem]** path 해석, permissions, directory separators는 host environment 특성이 크다.

**[device / storage]** stdio call 하나가 device operation 하나와 일대일 대응한다고 볼 수 없다.
## 8. 자주 하는 실수
- `FILE *`를 file 자체나 POSIX file descriptor라고 부른다.
- `fopen` 실패를 검사하지 않는다.
- `fclose`가 절대 실패하지 않는다고 생각한다.
- 닫은 stream을 다시 사용하거나 같은 stream을 두 번 닫는다. 두 경우 모두 실행 실습으로 만들지 않는다.
- program 종료 시 정리를 기대하고 명시적 `fclose`를 생략한다.
## 9. 필수 실습
고정된 학습용 파일을 열어 한 character를 쓰고 모든 반환값을 검사한 뒤 닫는다.
[23-1 exercise](../../exercises/23-file-io/23-1/README.md)
## 10. 추가 실습
- ★ 다른 character를 저장한다.
- ★★ 존재하지 않는 directory 경로로 open failure를 관찰한다.
- ★★★ C stream과 POSIX descriptor의 차이를 표로 정리한다.
## 11. 확인 문제
1. `FILE`과 `FILE *`의 차이는?
2. `fp`는 file인가 pointer object인가?
3. `fopen` 실패 시 무엇을 반환하는가?
4. `fclose` 성공과 실패 return은?
5. `FILE` 내부 member에 의존하면 안 되는 이유는?
## 12. 핵심 정리
- file name, stream, `FILE *`를 구분한다.
- open·I/O·close 결과를 검사한다.
- C stdio와 OS descriptor는 다른 interface 층이다.
## 13. 다음 Step
[23-2. 파일 모드와 열기 실패](23-2-file-modes-and-open-failure.md)
## 14. 참고 자료
- N1570 7.21.1, 7.21.3, 7.21.5.3, 7.21.5.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fopen](https://en.cppreference.com/w/c/io/fopen)
- [cppreference: fclose](https://en.cppreference.com/w/c/io/fclose)
