# 29-1. 입력·출력·경계 조건
## 1. 학습 목표
- 구현 전에 입력과 출력 contract를 작성한다.
- 빈 입력, 최소·최대 크기, 실패 표현을 먼저 결정한다.
- parsing, validation, algorithm, output 단계를 분리한다.
## 2. 선수 지식
Part 5의 입출력, Part 11의 배열, Part 16의 함수·포인터, Part 27의 UB를 안다.
## 3. 핵심 개념
알고리즘 구현은 code부터 시작하지 않는다.

```text
문제 정의
→ 입력 형태·허용 범위
→ 출력 형태·실패 표현
→ 경계 조건
→ algorithm invariant와 종료 조건
→ C17 구현 안전성
```

입력 contract에는 빈 입력 가능 여부, 원소 수 0, 중복, 정렬 여부, 값의 범위를 적는다. 출력 contract에는 정상 결과와 실패·미발견 표현을 적는다. 테스트 통과는 지정 입력에서의 evidence이며 모든 입력에 대한 correctness proof와 같지 않다.
## 4. 문법
```c
static int first_value(const int values[], size_t count, int *result);
```

입력 조건:
- 성공 경로에서 `result`는 호출 동안 살아 있는 writable `int` object를 가리킨다.
- `count > 0`이면 `values`는 호출 동안 읽을 수 있는 `count`개 `int` 원소를 나타낸다.
- non-NULL pointer의 lifetime·extent·writability는 caller precondition이며 이 함수가 검사할 수 없다.

결과 조건:
- 성공하면 `*result == values[0]`이고 1을 반환한다.
- `count == 0`, `values == NULL`, `result == NULL`이면 0을 반환한다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

static int first_value(const int values[], size_t count, int *result)
{
    if (result == NULL || count == 0 || values == NULL) {
        return 0;
    }
    *result = values[0];
    return 1;
}

int main(void)
{
    int values[] = {4, 2, 7, 1};
    int result;

    printf("empty=%d\n", first_value(NULL, 0, &result));
    if (first_value(values, sizeof values / sizeof values[0], &result)) {
        printf("first=%d\n", result);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o boundary_contract
./boundary_contract
```
## 6. 코드 해석
빈 입력은 `values[0]`을 평가하기 전에 거부한다. 일반 입력에서는 caller가 계산한 원소 수를 전달하고 output parameter에 첫 값을 기록한다. 출력은 `empty=0`, `first=4`다.
## 7. 내부 동작
**[Algorithm]** 이 예제의 절차는 “입력이 비어 있지 않으면 첫 원소를 결과로 선택한다”이다.

**[C17]** array access 전에 `count == 0`과 NULL arguments를 거부한다. non-NULL pointer의 lifetime·extent·writability는 runtime check가 아니라 caller precondition이다. `size_t`는 `sizeof` 결과와 배열 원소 수를 나타내는 unsigned integer type이며 `unsigned int`로 고정되지 않는다.

**[Compiler / CPU]** warning과 generated instruction은 구현 층이다. source의 조건 하나가 machine instruction 하나라는 뜻은 아니다.
## 8. 자주 하는 실수
- 배열에는 항상 원소가 하나 이상 있다고 가정한다.
- input validation과 algorithm logic을 한 loop에 섞는다.
- 실패 시 output parameter 값도 자동으로 유효하다고 생각한다.
- 몇 개의 성공 사례를 전체 correctness proof로 사용한다.
## 9. 필수 실습
문제의 입력·출력 contract와 `count == 0` 동작을 먼저 작성한 뒤 예제를 구현한다.
[29-1 exercise](../../exercises/29-basic-algorithms/29-1/README.md)
## 10. 추가 실습
- ★ 원소 하나 입력을 추가한다.
- ★★ null output pointer를 별도 실패 사례로 기록한다.
- ★★★ parsing·validation·algorithm·output을 네 단계 표로 정리한다.
## 11. 확인 문제
1. code보다 contract를 먼저 쓰는 이유는?
2. 빈 입력의 동작은 어디에서 결정하는가?
3. output parameter는 성공 전에 읽어도 되는가?
4. `size_t`를 `unsigned int`로 고정할 수 없는 이유는?
5. 테스트 통과와 correctness proof의 차이는?
## 12. 핵심 정리
- 입력 범위, 빈 입력, 중복, 정렬 여부를 구현 전에 정한다.
- 정상 결과와 실패 표현을 output contract에 포함한다.
- C17 bounds와 lifetime은 algorithm 설명과 별도로 검토한다.
## 13. 다음 Step
[29-2. Linear Search](29-2-linear-search.md)
## 14. 참고 자료
- N1570 6.5.2.1, 6.5.3.4, 7.19. N1570은 **C11 공개 Committee Draft**이며 관련 배열·`sizeof`·`size_t` 규칙은 C17에서도 유지된다.
- [SEI CERT C: ARR30-C](https://wiki.sei.cmu.edu/confluence/display/c/ARR30-C.+Do+not+form+or+use+out-of-bounds+pointers+or+array+subscripts)
