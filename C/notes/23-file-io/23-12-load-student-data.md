# 23-12. 학생 데이터 복원
## 1. 학습 목표
- bounded destination array에 records를 복원한다.
- valid record, malformed record, EOF를 구분한다.
- parsed values의 ranges를 검사한다.
## 2. 선수 지식
23-4 `fscanf` validation, 23-10 format, 23-11 save를 안다.
## 3. 핵심 개념
load loop는 다음 세 경계를 지킨다.
```text
destination capacity
record conversion count
application value ranges
```
정상 EOF는 loop 종료지만 malformed record는 자동 EOF가 아니다. 이 예제는 malformed input을 발견하면 전체 load를 실패시켜 recovery complexity를 제한한다.
## 4. 문법
외부 numeric text를 `fscanf`로 바로 변환하면 representable range를 벗어난 입력에서 undefined behavior가 될 수 있다. 이 Step은 bounded line을 먼저 읽고 `strtol`·`strtod`의 end pointer와 range를 검사한다.
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <limits.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    int id;
    double score;
    char name[20];
};

int parse_student(const char *line, struct Student *student)
{
    char *end;
    char *score_start;
    char extra;
    long id;
    double score;

    errno = 0;
    id = strtol(line, &end, 10);
    if (end == line || errno == ERANGE ||
        id <= 0 || id > INT_MAX) {
        return 0;
    }

    score_start = end;
    errno = 0;
    score = strtod(score_start, &end);
    if (end == score_start || errno == ERANGE ||
        !isfinite(score) || score < 0.0 || score > 100.0) {
        return 0;
    }
    if (sscanf(end, " %19s %c", student->name, &extra) != 1) {
        return 0;
    }

    student->id = (int)id;
    student->score = score;
    return 1;
}

int main(void)
{
    struct Student students[2];
    char line[128];
    size_t count = 0u;
    FILE *fp = fopen("part23_students.txt", "r");

    if (fp == NULL) {
        return 1;
    }
    while (fgets(line, sizeof line, fp) != NULL) {
        if (count == 2u ||
            strchr(line, '\n') == NULL ||
            !parse_student(line, &students[count])) {
            fclose(fp);
            return 1;
        }
        ++count;
    }
    if (ferror(fp) || count != 2u) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("loaded: %zu\n", count);
    return 0;
}
```
## 6. 코드 해석
23-11 saver가 만든 `part23_students.txt`를 read-only로 연다. 한 logical line씩 읽어 line completion, capacity, exact fields, numeric representability, finite score, ranges를 확인한다. excess·blank·truncated·malformed records는 전체 load failure다.
## 7. 내부 동작
**[C17 stdio stream]** formatted input이 stream position을 진행시키고 EOF/error indicators를 갱신한다.

**[C standard library]** `fgets`, conversion end pointers, `errno`, `ferror`로 line boundary·parse·I/O 상태를 구분한다.

**[OS file / file descriptor]** valid bytes read와 valid application record는 별개의 조건이다.

**[filesystem]** persisted file은 외부에서 변경되거나 잘릴 수 있다.

**[device / storage]** partial data availability를 application validation이 처리해야 한다.
## 8. 자주 하는 실수
- capacity보다 많은 records를 저장한다.
- numeric `fscanf` assignment count만 확인해 out-of-range conversion까지 안전하다고 생각한다.
- `NaN`을 ordered comparisons만으로 거부하려 한다.
- line boundary와 trailing field를 검사하지 않는다.
- malformed input에서 stream position을 진행하지 않은 채 무한 loop한다.
## 9. 필수 실습
두 student records를 bounded array로 읽고 count를 출력한다.
[23-12 exercise](../../exercises/23-file-io/23-12/README.md)
## 10. 추가 실습
- ★ 한 record file을 읽는다.
- ★★ invalid score가 있을 때 load를 거부한다.
- ★★★ `fgets`와 `strtol` 기반 line parser로 error recovery를 설계한다.
## 11. 확인 문제
1. load loop가 capacity를 먼저 검사하는 이유는?
2. numeric text를 `strtol`·`strtod`로 경계에서 parse하는 이유는?
3. EOF와 malformed record는 어떻게 다른가?
4. `isfinite`와 range validation이 모두 필요한 이유는?
5. line completion을 검사하는 이유는?
6. trailing field와 excess record를 거부하는 방법은?
## 12. 핵심 정리
- capacity, line boundary, exact fields, representable ranges를 모두 검사한다.
- EOF, malformed record, I/O error를 구분한다.
- valid records만 destination count에 포함한다.
## 13. 다음 Step
[23-13. 재실행 후 데이터 유지 확인](23-13-persistence-across-runs.md)
## 14. 참고 자료
- N1570 7.21.6.2, 7.21.10.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fscanf](https://en.cppreference.com/w/c/io/fscanf)
- [cppreference: ferror](https://en.cppreference.com/w/c/io/ferror)
