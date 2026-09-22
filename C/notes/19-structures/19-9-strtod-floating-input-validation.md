# 19-9. `strtod`로 실수 입력 검증
## 1. 학습 목표
- 실수 문자열 전체와 range를 검증한다.
- 유한한 학생 점수 범위를 적용한다.
## 2. 선수 지식
19-8의 end pointer와 `errno` 검사를 안다.
## 3. 핵심 개념
19-8처럼 완전한 입력 줄을 먼저 확보한다. `strtod`의 변환 시작·종료·`ERANGE`를 검사하고, application이 무한대와 NaN을 허용하지 않으므로 `isfinite`도 검사한다.
## 4. 문법
```c
errno = 0;
char *end;
double score = strtod(text, &end);
```
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <math.h>
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
    if (read_line(text, sizeof text) != 1) return 1;
    char *end;
    errno = 0;
    double value = strtod(text, &end);
    if (end == text || errno == ERANGE || *end != '\0'
        || !isfinite(value) || value < 0.0 || value > 100.0) return 1;
    printf("%.1f\n", value);
    return 0;
}
```
## 6. 코드 해석
완전한 줄을 확보해 newline을 제거한 뒤 문자열 전체가 변환되었는지 확인하고 학생 점수 domain range를 별도로 검사한다.
## 7. 내부 동작
**[C17 표준 library]** subject sequence, end pointer와 range error contract를 정한다. 부동소수점 representation과 rounding 세부는 구현 특성이 있다.
## 8. 자주 하는 실수
- 숫자 prefix 뒤 garbage를 허용한다.
- overlong input의 numeric prefix만 승인한다.
- EOF와 input error를 정상 점수로 처리한다.
- NaN이 비교에서 일반 수처럼 걸러진다고 생각한다.
- `%f` 입력 규칙과 `strtod`를 혼동한다.
## 9. 필수 실습
EOF·오류·truncation을 처리한 완전한 한 줄을 0.0~100.0의 유한한 `double`로 parse한다.
[19-9 exercise](../../exercises/19-structures/19-9/README.md)
## 10. 추가 실습
- ★ 경계 0과 100을 확인한다.
- ★★ 지수 표기 허용 여부를 기록한다.
- ★★★ 재시도 함수를 만든다.
## 11. 확인 문제
1. `end`는 무엇을 알려 주는가?
2. `isfinite`가 필요한 policy는?
3. library range와 점수 range 차이는?
4. NaN 비교가 주의 대상인 이유는?
5. 짧은 buffer에 `95.5x\n`이 잘려 들어오면 왜 전체 줄을 거부해야 하는가?
## 12. 핵심 정리
- conversion contract와 domain policy를 모두 검사한다.
- line acquisition contract를 먼저 확인하고 남은 입력을 정리한다.
- 유효한 전체 입력만 저장한다.
## 13. 다음 Step
[19-10. 학생 등록과 중복 검사](19-10-student-registration-duplicate-check.md)
## 14. 참고 자료
- N1570 7.12.3.2, 7.22.1.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: strtod](https://en.cppreference.com/w/c/string/byte/strtof)
