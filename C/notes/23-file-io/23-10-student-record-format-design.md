# 23-10. 학생 레코드 파일 형식 설계
## 1. 학습 목표
- student records의 text format contract를 정의한다.
- delimiter, range, field length, malformed line 정책을 정한다.
- 단순 whitespace format을 완전한 CSV라고 부르지 않는다.
## 2. 선수 지식
Part 19 student structure, 23-3 formatted I/O, 23-5 line input을 안다.
## 3. 핵심 개념
이 Step의 학습 format은 한 line에 다음 순서로 저장한다.
```text
id score name
```
contract:
- `id`: decimal positive int
- `score`: 0.0~100.0 decimal
- `name`: whitespace 없는 최대 19 characters
- 한 logical record는 한 line

spaces를 delimiter로 쓰므로 whitespace가 포함된 name은 지원하지 않는다. 이는 CSV가 아니며 quoted comma·embedded newline 규칙도 다루지 않는다.
## 4. 문법
```c
struct Student {
    int id;
    double score;
    char name[20];
};

fprintf(fp, "%d %.2f %s\n",
        student.id, student.score, student.name);
```
읽을 때는 assignment count와 ranges를 모두 검사한다. format 문자열 일치만으로 application constraints가 검증되지는 않는다.
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
    const struct Student student = {1001, 88.5, "Park"};
    FILE *fp = fopen("part23_format.txt", "w");

    if (fp == NULL) {
        return 1;
    }
    if (fprintf(fp, "%d %.2f %s\n",
                student.id, student.score, student.name) < 0) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("%d %.2f %s\n",
           student.id, student.score, student.name);
    return 0;
}
```
## 6. 코드 해석
structure fields를 정해진 순서와 representation으로 저장한다. name contract가 whitespace를 금지하므로 `%s` format과 일치한다.
## 7. 내부 동작
**[C17 stdio stream]** formatted output은 values를 text characters로 변환한다.

**[C standard library]** decimal conversion syntax를 제공하지만 application ranges를 정하지 않는다.

**[OS file / file descriptor]** format design은 OS handle type과 무관하다.

**[filesystem]** persisted record는 program 재실행 사이의 contract가 된다.

**[device / storage]** text readability와 storage efficiency 사이에는 trade-off가 있다.
## 8. 자주 하는 실수
- delimiter와 field order를 문서화하지 않는다.
- whitespace name을 `%s`로 저장·복원할 수 있다고 생각한다.
- decimal text를 쓰면 모든 locale·version 문제가 자동 해결된다고 생각한다.
- simple comma split을 완전한 RFC CSV parser라고 부른다.
- record ranges와 maximum lengths를 정하지 않는다.
## 9. 필수 실습
id·score·name의 text record contract를 적고 sample record를 저장한다.
[23-10 exercise](../../exercises/23-file-io/23-10/README.md)
## 10. 추가 실습
- ★ invalid id와 score examples를 적는다.
- ★★ version field를 추가할 필요가 생기는 조건을 설명한다.
- ★★★ whitespace name을 지원할 새 encoding을 설계한다.
## 11. 확인 문제
1. file format contract에 포함할 항목은?
2. `%s` name에 whitespace를 허용할 수 없는 이유는?
3. parsing success와 range validation은 왜 별개인가?
4. 단순 delimiter format을 완전한 CSV라고 부르면 안 되는 이유는?
5. text와 binary format 중 하나가 항상 우월한가?
## 12. 핵심 정리
- persistent data에는 명시적인 format contract가 필요하다.
- syntax와 application validation을 분리한다.
- 현재 범위에 맞는 단순 format을 사용한다.
## 13. 다음 Step
[23-11. 학생 데이터 저장](23-11-save-student-data.md)
## 14. 참고 자료
- N1570 7.21.6.1, 7.21.6.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fprintf](https://en.cppreference.com/w/c/io/fprintf)
- [cppreference: fscanf](https://en.cppreference.com/w/c/io/fscanf)
