# 23-4. `fscanf` 반환값·field width·검증 한계
## 1. 학습 목표
- conversion success, matching failure, input failure를 구분한다.
- `%s` field width와 buffer capacity를 연결한다.
- `fscanf`만으로 record 전체의 유효성을 증명할 수 없는 이유를 설명한다.
## 2. 선수 지식
23-3 formatted file I/O와 Part 5 `scanf` 반환값을 안다.
## 3. 핵심 개념
`fscanf` result는 성공한 assignments 수다.
```text
2    두 conversions 성공
1    첫 conversion만 성공
0    matching failure
EOF  첫 conversion 전에 input failure 또는 end-of-file
```
width는 `%s` destination overflow 방지에 필요하지만 record trailing garbage, numeric range, logical constraints까지 자동 검증하지 않는다. 숫자 conversion 결과가 destination object에 표현되지 않으면 behavior가 undefined이므로 assignment count를 나중에 검사해도 보호되지 않는다. 외부 numeric text는 `fgets`로 bounded line을 읽은 뒤 `strtol`·`strtod`의 end pointer와 range를 검사하는 방식이 더 적합하다.
## 4. 문법
```c
int id;
char name[20];
int result = fscanf(fp, "%d %19s", &id, name);
```
`result == 2`일 때만 두 outputs를 사용한다. `%19s`는 최대 19 characters 뒤 null terminator 공간을 남긴다. whitespace 포함 field에는 이 단순 format이 맞지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_records.txt", "w");
    int id;
    char name[20];
    int result;

    if (fp == NULL) {
        return 1;
    }
    if (fputs("12 Lee\ninvalid record\n", fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_records.txt", "r");
    if (fp == NULL) {
        return 1;
    }
    result = fscanf(fp, "%d %19s", &id, name);
    if (result == 2) {
        printf("valid: %d %s\n", id, name);
    }
    result = fscanf(fp, "%d %19s", &id, name);
    printf("second result: %d\n", result);

    if (fclose(fp) == EOF) {
        return 1;
    }
    return 0;
}
```
## 6. 코드 해석
첫 record는 두 fields가 성공한다. 두 번째 scan은 다음 text가 integer에 match하지 않아 0을 반환한다. outputs는 성공한 conversions에 대해서만 신뢰한다.
## 7. 내부 동작
**[C17 stdio stream]** formatted input은 matching rules에 따라 stream에서 characters를 소비한다.

**[C standard library]** matching failure와 input failure를 return value로 구분한다.

**[OS file / file descriptor]** parse failure는 OS read failure와 같은 개념이 아니다.

**[filesystem]** file이 열려도 contents는 application format에 맞지 않을 수 있다.

**[device / storage]** storage가 bytes를 제공했다는 사실과 record가 valid하다는 사실은 별개다.
## 8. 자주 하는 실수
- return이 `EOF`인지 0인지 구분하지 않는다.
- width를 buffer 전체 크기와 같게 써 terminator 공간을 잊는다.
- `result != EOF`만 확인하고 partial assignment를 성공으로 취급한다.
- malformed token을 소비하지 않아 같은 matching failure를 반복한다.
- width만 있으면 value range와 record 전체가 검증됐다고 생각한다.
- numeric conversion overflow도 assignment count로 안전하게 발견된다고 생각한다.
## 9. 필수 실습
valid record와 malformed record를 한 file에 넣고 assignment count를 분기한다.
[23-4 exercise](../../exercises/23-file-io/23-4/README.md)
## 10. 추가 실습
- ★ 한 field만 있는 record를 시험한다.
- ★★ name capacity를 바꾸고 width도 함께 조정한다.
- ★★★ `fgets` 후 `strtol` 방식과 error recovery를 비교한다.
## 11. 확인 문제
1. return 0과 `EOF`의 차이는?
2. partial assignment result를 어떻게 검사해야 하는가?
3. `char name[20]`에 `%20s`가 위험한 이유는?
4. field width가 검증하지 못하는 것은?
5. matching failure 뒤 반복 loop가 멈출 수 있는 이유는?
## 12. 핵심 정리
- 필요한 assignment count와 정확히 비교한다.
- `%s` width는 terminator 공간을 고려한다.
- formatted scan의 validation 한계를 안다.
## 13. 다음 Step
[23-5. `fgets`와 `fputs`](23-5-fgets-and-fputs.md)
## 14. 참고 자료
- N1570 7.21.6.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fscanf](https://en.cppreference.com/w/c/io/fscanf)
- CERT C: formatted input guidance
