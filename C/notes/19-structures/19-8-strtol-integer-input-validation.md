# 19-8. `strtol`로 정수 입력 검증
## 1. 학습 목표
- 한 줄을 읽고 정수로 완전히 parse한다.
- syntax, range, application range 오류를 구분한다.
## 2. 선수 지식
문자열, `fgets`, 반환값 검사를 안다.
## 3. 핵심 개념
먼저 `fgets`가 한 줄 전체를 얻었는지 확인한다. buffer에 newline이 없으면 남은 문자를 버리고 truncated line을 거부한다. 그 뒤 `strtol`의 `end`, `ERANGE`, trailing garbage, 목표 `int` 범위를 각각 검사한다.
## 4. 문법
```c
errno = 0;
char *end;
long value = strtol(text, &end, 10);
```
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

static int read_line(char text[], size_t size)
{
    if (fgets(text, size, stdin) == NULL) {
        return 0;
    }
    char *newline = strchr(text, '\n');
    if (newline == NULL) {
        int ch;
        while ((ch = getchar()) != '\n' && ch != EOF) {
        }
        return -1;
    }
    *newline = '\0';
    return 1;
}

int main(void)
{
    char text[32];
    int read_result = read_line(text, sizeof text);
    if (read_result != 1) {
        return 1;
    }
    char *end;
    errno = 0;
    long value = strtol(text, &end, 10);
    if (end == text || errno == ERANGE || *end != '\0'
        || value < INT_MIN || value > INT_MAX) {
        return 1;
    }
    printf("%d\n", (int)value);
    return 0;
}
```
## 6. 코드 해석
`fgets` 실패·EOF와 buffer에 다 들어오지 않은 줄을 거부한다. newline을 제거한 뒤 변환 전 `errno`를 0으로 만들고 `*end == '\0'`인지 확인한 후 안전하게 `int`로 cast한다.
## 7. 내부 동작
**[C17 표준 library]** base 10 conversion과 end pointer, range reporting contract를 정한다. 입력 문자열 lifetime과 writable `end` pointer object가 유효해야 한다.
## 8. 자주 하는 실수
- 반환값 0만으로 실패를 판단한다.
- trailing 문자를 무시한다.
- newline이 없는데 완전한 줄이라고 가정한다.
- truncated line의 나머지를 다음 입력으로 남긴다.
- `errno`를 초기화하지 않는다.
- `long`을 검사 없이 `int`로 줄인다.
## 9. 필수 실습
`fgets` 실패·EOF·truncation을 처리해 완전한 한 줄을 얻고 `strtol`로 양의 `int`인지 검증한다.
[19-8 exercise](../../exercises/19-structures/19-8/README.md)
## 10. 추가 실습
- ★ 앞뒤 공백 policy를 정한다.
- ★★ 허용 범위를 1~999999로 제한한다.
- ★★★ 재시도 loop를 만든다.
## 11. 확인 문제
1. `end == text`는 무엇을 뜻하는가?
2. `ERANGE` 검사는 왜 필요한가?
3. trailing garbage는 어떻게 찾는가?
4. `long`에서 `int` 범위 검사가 필요한 이유는?
5. buffer에 newline이 없을 때 무엇을 해야 하는가?
## 12. 핵심 정리
- 문자열 전체를 parse하고 범위를 단계별로 검사한다.
- 완전한 한 줄을 얻지 못하면 변환 전에 거부하고 remainder를 정리한다.
- library range와 application range는 별개다.
- 검증 뒤에만 좁은 type으로 변환한다.
## 13. 다음 Step
[19-9. `strtod`로 실수 입력 검증](19-9-strtod-floating-input-validation.md)
## 14. 참고 자료
- N1570 7.22.1.4. N1570은 **C11 공개 Committee Draft**이며 `strtol` contract는 C17에서도 유지된다.
- [cppreference: strtol](https://en.cppreference.com/w/c/string/byte/strtol)
