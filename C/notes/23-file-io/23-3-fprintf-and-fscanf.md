# 23-3. `fprintf`와 `fscanf`
## 1. 학습 목표
- formatted output을 file stream에 기록한다.
- `fscanf` assignment count를 검사한다.
- format string과 destination object type을 일치시킨다.
## 2. 선수 지식
Part 4 `printf`, Part 5 `scanf`, 23-1 stream lifecycle을 안다.
## 3. 핵심 개념
`fprintf`는 formatted output을 지정한 stream으로 보낸다. `fscanf`는 input stream의 text를 format에 따라 변환해 objects에 저장한다.
```c
fprintf(fp, "%d %s\n", id, name);
fscanf(fp, "%d %19s", &id, name);
```
file contents가 항상 valid하다고 가정하지 않고 return value를 확인한다.
## 4. 문법
```c
int fprintf(FILE *restrict stream,
            const char *restrict format, ...);
int fscanf(FILE *restrict stream,
           const char *restrict format, ...);
```
`fprintf`는 성공하면 전송한 characters 수를, 오류면 negative value를 반환한다. 이는 assignments 수를 반환하는 `fscanf`, complete elements 수를 반환하는 `fread`·`fwrite`와 구분한다. `fscanf`는 성공적으로 assignment한 input items 수를 반환하며, 첫 conversion 전 input failure이면 `EOF`를 반환한다.

외부 text를 format string으로 직접 사용하지 않는다. `fprintf(fp, "%s", text)`처럼 format을 고정한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_student.txt", "w");
    int id = 7;
    char name[20] = "Kim";

    if (fp == NULL) {
        return 1;
    }
    if (fprintf(fp, "%d %s\n", id, name) < 0) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_student.txt", "r");
    if (fp == NULL) {
        return 1;
    }
    if (fscanf(fp, "%d %19s", &id, name) != 2) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("%d %s\n", id, name);
    return 0;
}
```
## 6. 코드 해석
integer와 whitespace 없는 name을 text record로 저장한다. 다시 열어 두 conversions가 모두 성공했는지 확인한 뒤 출력한다.
## 7. 내부 동작
**[C17 stdio stream]** formatted functions는 stream의 text representation을 읽고 쓴다.

**[C standard library]** format specifier와 argument type 계약을 정의한다.

**[OS file / file descriptor]** `fprintf` call 하나가 `write` system call 하나가 된다는 보장은 없다.

**[filesystem]** file contents는 external input이므로 malformed할 수 있다.

**[device / storage]** buffering 때문에 library call 시점과 device write 시점이 다를 수 있다.
## 8. 자주 하는 실수
- `fscanf` return count를 검사하지 않는다.
- `char name[20]`에 width 없는 `%s`를 사용한다.
- file contents를 format string으로 직접 전달한다.
- `fprintf` write가 항상 성공한다고 가정한다.
- whitespace가 포함된 name도 `%s` 하나로 완전히 읽는다고 생각한다.
## 9. 필수 실습
고정 학습용 file에 id와 한 단어 name을 쓰고 정확히 두 fields를 읽는다.
[23-3 exercise](../../exercises/23-file-io/23-3/README.md)
## 10. 추가 실습
- ★ score field를 추가한다.
- ★★ format specifier를 틀렸을 때 compiler warning을 관찰한다.
- ★★★ 같은 record를 line-based parsing으로 읽는 방식을 비교한다.
## 11. 확인 문제
1. `fprintf`와 `printf`의 핵심 차이는?
2. `fscanf` return value는 무엇을 세는가?
3. `%19s`가 `char name[20]`에 맞는 이유는?
4. malformed record에서 assignment count가 줄어들 수 있는 이유는?
5. external string을 format으로 직접 쓰면 안 되는 이유는?
## 12. 핵심 정리
- formatted I/O도 모든 return 값을 검사한다.
- `%s`에는 destination capacity에 맞는 field width를 둔다.
- format과 argument type을 일치시킨다.
## 13. 다음 Step
[23-4. `fscanf` 반환값·field width·검증 한계](23-4-fscanf-results-width-and-limits.md)
## 14. 참고 자료
- N1570 7.21.6.1, 7.21.6.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fprintf](https://en.cppreference.com/w/c/io/fprintf)
- [cppreference: fscanf](https://en.cppreference.com/w/c/io/fscanf)
