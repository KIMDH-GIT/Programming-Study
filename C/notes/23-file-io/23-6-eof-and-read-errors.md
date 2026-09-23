# 23-6. EOF와 읽기 오류
## 1. 학습 목표
- `fgetc` 결과를 `int`로 받고 `EOF`와 구분한다.
- read operation 결과를 loop condition으로 사용한다.
- `feof`와 `ferror`로 종료 이유를 구분한다.
## 2. 선수 지식
23-1 stream lifecycle과 Part 6 loop를 안다.
## 3. 핵심 개념
```c
int ch;

while ((ch = fgetc(fp)) != EOF) {
    putchar(ch);
}
```
`fgetc` return type은 `int`다. 표준 contract는 읽은 character를 `unsigned char`로 해석해 `int`로 변환한 값 또는 `EOF`를 반환한다. 일반적인 implementation에서는 character values와 별도의 negative `EOF`를 구분할 수 있다. unusual integer ranges까지 포함해 정확히 처리하려면 반환값을 반드시 `int`에 저장해 `EOF`와 먼저 비교해야 한다. `EOF`는 file 안에 저장된 특별한 character가 아니다.
## 4. 문법
read가 `EOF`를 반환한 뒤 종료 이유를 확인한다.
```c
if (ferror(fp)) {
    /* read error */
} else if (feof(fp)) {
    /* end-of-file */
}
```
`feof`는 다음 read가 끝에 도달할지 미리 알려주지 않는다. read가 end-of-file을 만난 뒤 설정된 indicator를 검사한다. `ferror`도 상세 errno code를 반환하는 function이 아니라 error indicator를 검사한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    FILE *fp = fopen("part23_chars.txt", "w");
    int ch;

    if (fp == NULL) {
        return 1;
    }
    if (fputs("ABC\n", fp) == EOF) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_chars.txt", "r");
    if (fp == NULL) {
        return 1;
    }
    while ((ch = fgetc(fp)) != EOF) {
        if (putchar(ch) == EOF) {
            fclose(fp);
            return 1;
        }
    }
    if (ferror(fp)) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    puts("EOF reached");
    return 0;
}
```
## 6. 코드 해석
read result를 직접 검사해 valid characters만 출력한다. loop 종료 후 error indicator를 확인하므로 정상 EOF와 read error를 구분한다.
## 7. 내부 동작
**[C17 stdio stream]** stream은 end-of-file indicator와 error indicator를 별도로 가진다.

**[C standard library]** `fgetc`는 `unsigned char`를 `int`로 변환한 값 또는 `EOF`를 반환한다.

**[OS file / file descriptor]** C EOF indicator는 특정 OS syscall return과 같은 object가 아니다.

**[filesystem]** regular file 외 stream source도 있을 수 있으므로 모든 stream을 seekable file로 가정하지 않는다.

**[device / storage]** end condition과 device error는 library가 구분해 stream state로 보고한다.
## 8. 자주 하는 실수
- `char ch = fgetc(fp)`로 `EOF`와 character를 합친다.
- `while (!feof(fp))`로 읽기 전에 EOF를 예측한다.
- `EOF`를 file에 들어 있는 sentinel byte라고 생각한다.
- `fgetc`가 `EOF`이면 무조건 정상 종료라고 생각한다.
- closed or null stream으로 read를 시도한다.
## 9. 필수 실습
character loop로 학습용 file을 읽고 종료 후 `ferror`를 검사한다.
[23-6 exercise](../../exercises/23-file-io/23-6/README.md)
## 10. 추가 실습
- ★ character 수를 센다.
- ★★ newline 수를 센다.
- ★★★ `clearerr`로 indicators를 지운 뒤 의미를 관찰하되 retry 가능성은 별도 판단한다.
## 11. 확인 문제
1. `fgetc` return type이 `int`인 이유는?
2. `EOF`는 file character인가?
3. `while (!feof(fp))`가 잘못된 이유는?
4. `feof`는 언제 의미 있는 값을 주는가?
5. `ferror`는 상세 error code를 반환하는가?
## 12. 핵심 정리
- read 결과를 직접 loop condition에서 검사한다.
- `EOF` 뒤 `feof`와 `ferror`로 종료 원인을 구분한다.
- `fgetc` 결과는 `int`에 저장한다.
## 13. 다음 Step
[23-7. text file과 binary file](23-7-text-and-binary-files.md)
## 14. 참고 자료
- N1570 7.21.1, 7.21.7.1, 7.21.10.2, 7.21.10.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fgetc](https://en.cppreference.com/w/c/io/fgetc)
- [cppreference: feof](https://en.cppreference.com/w/c/io/feof)
- [cppreference: ferror](https://en.cppreference.com/w/c/io/ferror)
