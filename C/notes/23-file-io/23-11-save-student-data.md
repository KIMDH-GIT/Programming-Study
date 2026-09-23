# 23-11. 학생 데이터 저장
## 1. 학습 목표
- student array를 명시한 text format으로 저장한다.
- 각 write와 final close failure를 처리한다.
- 중요한 기존 file이 아닌 고정 학습용 file name을 사용한다.
## 2. 선수 지식
Part 19 structure arrays와 23-10 record format을 안다.
## 3. 핵심 개념
save operation은 output stream을 열고 records를 순서대로 encoding한 뒤 close한다. `"w"`는 기존 file을 truncate하므로 저장 대상 선택에 주의한다.

`fprintf`가 실패하면 이후 records를 성공했다고 계산하지 않는다. `fclose` failure도 저장 성공 보고 전에 확인한다.
## 4. 문법
```c
for (size_t i = 0; i < count; ++i) {
    if (fprintf(fp, "%d %.2f %s\n",
                students[i].id,
                students[i].score,
                students[i].name) < 0) {
        /* write failure */
    }
}
```
array count는 실제 element count와 일치해야 한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Student {
    int id;
    double score;
    char name[20];
};

int main(void)
{
    const struct Student students[] = {
        {1001, 88.5, "Park"},
        {1002, 93.0, "Choi"}
    };
    size_t count = sizeof students / sizeof students[0];
    FILE *fp = fopen("part23_students.txt", "w");

    if (fp == NULL) {
        return 1;
    }
    for (size_t i = 0; i < count; ++i) {
        if (fprintf(fp, "%d %.2f %s\n",
                    students[i].id,
                    students[i].score,
                    students[i].name) < 0) {
            fclose(fp);
            return 1;
        }
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("saved: %zu\n", count);
    return 0;
}
```
## 6. 코드 해석
두 records를 documented order로 저장한다. element count만큼 loop하고 각 output result를 검사한 뒤 stream을 닫는다.
## 7. 내부 동작
**[C17 stdio stream]** formatted output은 stream position을 진행시킨다.

**[C standard library]** each `fprintf`와 `fclose`가 실패를 보고할 수 있다.

**[OS file / file descriptor]** library buffering 때문에 records와 OS writes가 일대일 대응하지 않을 수 있다.

**[filesystem]** `"w"` success는 이전 contents를 제거하므로 safe training filename을 사용한다.

**[device / storage]** close success가 모든 hardware failure model을 제거하거나 영구 보존을 절대 보장한다는 뜻은 아니다.
## 8. 자주 하는 실수
- 실제 사용자 file을 학습 예제로 truncate한다.
- loop 안 `fprintf` result를 무시한다.
- close failure를 확인하기 전에 success를 출력한다.
- pointer member를 raw 저장한다.
- file contents를 format string으로 직접 사용한다.
## 9. 필수 실습
두 student records를 고정 학습용 text file에 저장하고 saved count를 출력한다.
[23-11 exercise](../../exercises/23-file-io/23-11/README.md)
## 10. 추가 실습
- ★ 세 번째 record를 추가한다.
- ★★ score range를 save 전에 검사한다.
- ★★★ 중간 failure에서 이미 열린 stream을 닫는 cleanup flow를 그린다.
## 11. 확인 문제
1. save에 `"w"`를 사용할 때 주의할 점은?
2. 각 `fprintf` result를 확인하는 이유는?
3. `fclose` result도 확인하는 이유는?
4. array count는 어떻게 계산하는가?
5. pointer member를 raw 저장하면 안 되는 이유는?
## 12. 핵심 정리
- 명시한 format대로 records를 순서대로 쓴다.
- write와 close 결과를 모두 검사한다.
- 안전한 학습용 path만 사용한다.
## 13. 다음 Step
[23-12. 학생 데이터 복원](23-12-load-student-data.md)
## 14. 참고 자료
- N1570 7.21.5.1, 7.21.5.3, 7.21.6.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fprintf](https://en.cppreference.com/w/c/io/fprintf)
- [cppreference: fclose](https://en.cppreference.com/w/c/io/fclose)
