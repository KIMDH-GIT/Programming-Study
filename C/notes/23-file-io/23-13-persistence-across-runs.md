# 23-13. 재실행 후 데이터 유지 확인
## 1. 학습 목표
- file data가 process lifetime과 분리되어 유지될 수 있음을 관찰한다.
- missing file과 malformed persisted data를 구분한다.
- 매 실행마다 open·read·close와 open·write·close를 완결한다.
## 2. 선수 지식
23-2 modes, 23-4 scan validation, 23-11~23-12 save/load를 안다.
## 3. 핵심 개념
automatic object는 program 실행이 끝나면 lifetime이 끝나지만 file contents는 host environment의 filesystem에 남을 수 있다. 23-11 saver process가 끝난 뒤 별도 verifier process가 같은 format을 parse하면 새 objects로 values를 복원한다.

이 persistence는 이전 C object나 pointer가 살아남는다는 뜻이 아니다. file availability, path semantics, permissions, durability는 environment 영향을 받는다.
## 4. 문법
```c
FILE *fp = fopen("part23_students.txt", "r");
```
verifier는 saver output을 read-only로 연다. open failure를 file absence라고 단정하거나 `"w"`로 다시 열면 permissions 등 다른 실패인데도 기존 data를 파괴할 수 있다.
## 5. 최소 코드 예제
```c
#include <errno.h>
#include <limits.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    char line[128];
    char name[20];
    char extra;
    char *end;
    char *score_start;
    long id;
    double score;
    FILE *fp = fopen("part23_students.txt", "r");

    if (fp == NULL || fgets(line, sizeof line, fp) == NULL) {
        if (fp != NULL) {
            fclose(fp);
        }
        return 1;
    }
    if (strchr(line, '\n') == NULL) {
        fclose(fp);
        return 1;
    }

    errno = 0;
    id = strtol(line, &end, 10);
    if (end == line || errno == ERANGE ||
        id <= 0 || id > INT_MAX) {
        fclose(fp);
        return 1;
    }
    score_start = end;
    errno = 0;
    score = strtod(score_start, &end);
    if (end == score_start || errno == ERANGE ||
        !isfinite(score) || score < 0.0 || score > 100.0 ||
        sscanf(end, " %19s %c", name, &extra) != 1) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("restored: %ld %.2f %s\n", id, score, name);
    return 0;
}
```
## 6. 코드 해석
23-11 saver가 만든 file의 첫 record를 별도 process에서 read-only로 연다. representability, finite score, ranges, exact field count를 검사한 뒤 restored values를 출력한다.
## 7. 내부 동작
**[C17 stdio stream]** 각 process는 자신의 stream을 새로 열고 닫는다.

**[C standard library]** bounded input과 checked conversions로 external representation을 새 objects로 복원한다.

**[OS file / file descriptor]** process 종료와 filesystem object lifetime은 다를 수 있다.

**[filesystem]** saver와 verifier는 같은 path·format contract를 공유한다.

**[device / storage]** success가 모든 hardware durability 정책을 C17 수준에서 보장하지는 않는다.
## 8. 자주 하는 실수
- saver와 loader가 서로 다른 file name이나 format을 사용한다.
- read-open failure 뒤 `"w"`로 열어 기존 file을 파괴한다.
- file이 열리면 contents도 반드시 valid하다고 생각한다.
- previous process의 `FILE *`나 pointer values가 다음 실행에서도 유효하다고 생각한다.
- test artifacts를 repository에 남긴다.
## 9. 필수 실습
임시 directory에서 23-11 saver process를 실행한 뒤 verifier process로 student를 복원한다.
[23-13 exercise](../../exercises/23-file-io/23-13/README.md)
## 10. 추가 실습
- ★ student value를 바꿔 저장한다.
- ★★ malformed student file을 거부한다.
- ★★★ version field를 추가해 migration 필요성을 분석한다.
## 11. 확인 문제
1. 재실행 시 이전 automatic object가 살아남는가?
2. 실제로 유지되는 것은 무엇인가?
3. saver와 verifier가 같은 format을 사용해야 하는 이유는?
4. read-open failure 뒤 write-open이 위험한 이유는?
5. 이전 실행의 pointer value를 저장하면 안 되는 이유는?
## 12. 핵심 정리
- persistence는 object lifetime이 아니라 external representation의 유지다.
- 다음 process는 file을 검증해 새 objects를 만든다.
- 검증은 read-only verifier와 안전한 임시 directory로 수행한다.
## 13. 다음 Step
[23-14. Part 23 종합 복습](23-14-part-23-review.md)
## 14. 참고 자료
- N1570 5.1.2, 7.21.3, 7.21.5.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fopen](https://en.cppreference.com/w/c/io/fopen)
- 검증된 systems programming 교재의 persistence와 serialization 장
