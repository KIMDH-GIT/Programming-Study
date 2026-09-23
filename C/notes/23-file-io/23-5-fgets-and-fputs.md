# 23-5. `fgets`와 `fputs`
## 1. 학습 목표
- bounded line input과 string output을 사용한다.
- `fgets`가 newline을 보존할 수 있음을 설명한다.
- 긴 logical line이 여러 reads로 나뉠 수 있음을 안다.
## 2. 선수 지식
Part 13 string·truncated input과 23-1 stream lifecycle을 안다.
## 3. 핵심 개념
```c
char line[128];

if (fgets(line, sizeof line, fp) != NULL) {
    /* line is a string */
}
```
`fgets`는 최대 `n - 1` characters를 저장하고 성공 시 null character로 끝낸다. newline을 읽으면 buffer에 포함한다. buffer가 먼저 차면 한 logical line의 일부만 반환될 수 있다.

`fputs`는 string을 쓰지만 newline을 자동 추가하지 않는다. `puts`와 구분한다.
## 4. 문법
```c
char *fgets(char *restrict s, int n,
            FILE *restrict stream);
int fputs(const char *restrict s,
          FILE *restrict stream);
```
`fgets`는 성공 시 `s`를 반환한다. end-of-file을 어떤 character보다 먼저 만나면 null pointer를 반환하고 array contents는 바뀌지 않는다. read error이면 null pointer를 반환하며 array contents는 indeterminate다. 따라서 null result 뒤에는 buffer를 사용하지 않고 `ferror`로 error 여부를 확인한다. `fputs`는 write error면 `EOF`를 반환한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_lines.txt", "w");
    char line[32];

    if (fp == NULL) {
        return 1;
    }
    if (fputs("first line\nsecond line\n", fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_lines.txt", "r");
    if (fp == NULL) {
        return 1;
    }
    if (fgets(line, sizeof line, fp) == NULL) {
        fclose(fp);
        return 1;
    }
    printf("read: %s", line);
    if (fclose(fp) == EOF) {
        return 1;
    }
    return 0;
}
```
## 6. 코드 해석
두 lines를 저장한 뒤 첫 line을 bounded buffer로 읽는다. 저장된 newline이 `%s` output에 포함되므로 별도 newline을 붙이지 않는다.
## 7. 내부 동작
**[C17 stdio stream]** `fgets`는 newline, end-of-file, 또는 capacity limit까지 읽는다.

**[C standard library]** 성공한 read를 null-terminated string으로 만든다.

**[OS file / file descriptor]** 한 `fgets`가 한 OS read와 일치한다는 보장은 없다.

**[filesystem]** logical line length는 application이 정한 buffer보다 길 수 있다.

**[device / storage]** input chunk와 logical record boundary는 다를 수 있다.
## 8. 자주 하는 실수
- `fgets`가 newline을 자동 제거한다고 생각한다.
- 한 call이 항상 logical line 전체를 반환한다고 가정한다.
- return pointer를 확인하지 않고 buffer를 사용한다.
- `fputs`가 newline을 자동 추가한다고 생각한다.
- binary bytes를 `fgets`·`%s`용 C string으로 간주한다.
## 9. 필수 실습
두 lines를 저장하고 작은 buffer로 읽어 newline과 long-line split 여부를 관찰한다.
[23-5 exercise](../../exercises/23-file-io/23-5/README.md)
## 10. 추가 실습
- ★ newline 포함 여부를 `strchr`로 확인한다.
- ★★ buffer를 줄여 한 line이 여러 reads로 나뉘게 한다.
- ★★★ truncated line의 remainder를 소비하는 logic을 작성한다.
## 11. 확인 문제
1. `fgets`가 저장할 수 있는 최대 characters 수는?
2. newline을 읽으면 buffer에 포함하는가?
3. 성공한 `fgets` 결과가 C string인 이유는?
4. 한 call이 line 전체를 보장하지 않는 경우는?
5. `fputs`와 `puts`의 newline 차이는?
## 12. 핵심 정리
- `fgets` return과 capacity를 검사한다.
- newline이 남을 수 있고 long line은 나뉠 수 있다.
- `fputs`는 newline을 자동 추가하지 않는다.
## 13. 다음 Step
[23-6. EOF와 읽기 오류](23-6-eof-and-read-errors.md)
## 14. 참고 자료
- N1570 7.21.7.2, 7.21.7.4. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fgets](https://en.cppreference.com/w/c/io/fgets)
- [cppreference: fputs](https://en.cppreference.com/w/c/io/fputs)
