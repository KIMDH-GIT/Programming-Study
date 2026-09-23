# 23-9. 구조체 통째 저장의 padding·pointer·호환성 문제
## 1. 학습 목표
- raw struct object representation과 portable file format을 구분한다.
- padding, endianness, type representation, ABI 문제를 설명한다.
- pointer member의 값을 persistence data로 저장하지 않는다.
## 2. 선수 지식
Part 19 structures, Part 20 padding·endianness, 23-8 block I/O를 안다.
## 3. 핵심 개념
```c
fwrite(&record, sizeof record, 1u, fp);
```
이 방식은 동일한 program·implementation에서 제한적으로 round trip할 수 있지만 portable serialization이라고 일반화할 수 없다. structure에는 padding이 있을 수 있고 integer·floating representation과 byte order가 implementation마다 다를 수 있다.

pointer member를 raw로 저장하면 pointed-to object가 아니라 한 실행의 pointer representation만 저장된다. 다음 실행이나 다른 process에서 유효한 pointer가 되지 않는다.
## 4. 문법
portable format이 필요하면 fields를 명시적으로 encoding한다.
```c
fprintf(fp, "%d %.2f %s\n",
        record.id, record.score, record.name);
```
text format도 delimiter, range, maximum length 같은 contract가 필요하지만 structure padding과 pointer representation에는 의존하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Student {
    int id;
    double score;
    const char *label;
};

int main(void)
{
    struct Student student = {7, 91.5, "Kim"};
    FILE *fp = fopen("part23_struct.txt", "w");

    if (fp == NULL) {
        return 1;
    }
    if (fprintf(fp, "%d %.1f %s\n",
                student.id, student.score, student.label) < 0) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    puts("fields encoded as text");
    return 0;
}
```
## 6. 코드 해석
structure object 전체 bytes 대신 application이 선택한 세 field values를 text로 encoding한다. pointer value 자체가 아니라 pointed-to string contents를 저장한다.
## 7. 내부 동작
**[C17 stdio stream]** stream은 program이 선택한 representation을 전달할 뿐 schema portability를 자동 제공하지 않는다.

**[C standard library]** `fwrite`는 object representation을 쓸 수 있지만 의미 있는 serialization contract를 만들지는 않는다.

**[OS file / file descriptor]** OS가 bytes를 보존해도 ABI compatibility는 해결되지 않는다.

**[filesystem]** file format은 producer와 consumer 사이의 persistent contract다.

**[device / storage]** physical storage format과 C structure layout은 독립된 층이다.
## 8. 자주 하는 실수
- `sizeof struct` bytes를 쓰면 portable format이라고 생각한다.
- padding bytes가 항상 0이라고 가정한다.
- endianness와 integer representation을 무시한다.
- pointer member를 쓰면 pointed-to data도 저장된다고 생각한다.
- same-machine round trip 성공을 cross-version compatibility 증거로 삼는다.
## 9. 필수 실습
pointer member가 있는 student structure를 raw로 쓰지 않고 각 logical field를 text로 저장한다.
[23-9 exercise](../../exercises/23-file-io/23-9/README.md)
## 10. 추가 실습
- ★ structure의 `sizeof`와 member sizes 합을 비교한다.
- ★★ text format의 maximum field length를 문서화한다.
- ★★★ fixed-width binary encoding을 설계하되 host struct layout을 사용하지 않는다.
## 11. 확인 문제
1. structure padding이 file compatibility에 미치는 영향은?
2. raw integer bytes가 다른 implementation에서 달라질 수 있는 이유는?
3. pointer member를 저장하면 pointed-to object도 저장되는가?
4. same-program round trip이 portable serialization을 증명하는가?
5. text field encoding이 해결하는 문제와 남기는 문제는?
## 12. 핵심 정리
- raw struct persistence와 serialization을 구분한다.
- pointer values는 persistent record data가 아니다.
- explicit format contract를 설계한다.
## 13. 다음 Step
[23-10. 학생 레코드 파일 형식 설계](23-10-student-record-format-design.md)
## 14. 참고 자료
- N1570 6.2.6.1, 6.7.2.1, 7.21.8.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: object representation](https://en.cppreference.com/w/c/language/object)
- [cppreference: fwrite](https://en.cppreference.com/w/c/io/fwrite)
