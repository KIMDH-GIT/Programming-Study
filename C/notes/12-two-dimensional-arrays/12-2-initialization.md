# 12-2. 2차원 배열 초기화

중첩 initializer는 각 row 배열과 그 안의 element 초기값을 눈에 보이게 대응시킨다.

## 1. 학습 목표
- nested initializer와 row를 연결한다.
- 부분 초기화와 `{0}`의 C17 규칙을 설명한다.
- 필수 문법과 읽기 좋은 권장 중괄호 스타일을 구분한다.

## 2. 선수 지식
Part 11의 array initialization과 Step 12-1의 row array 구조를 안다.

## 3. 핵심 개념
`int matrix[2][3] = {{1,2,3},{4,5,6}};`에서 첫 nested initializer는 row 0, 둘째는 row 1이다. initializer가 부족한 aggregate element는 C17 규칙에 따라 0으로 초기화된다. nested braces는 row 구조를 명확히 보여 주는 권장 스타일이지만 C grammar를 “항상 각 row brace가 필수”라고 과도하게 단순화하지 않는다.

## 4. 문법
```c
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
int partial[2][3] = {{1, 2}, {3}};
int zeros[3][4] = {0};
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int matrix[2][3] = {
        {1, 2},
        {3}
    };

    printf("%d %d %d\n", matrix[0][0], matrix[0][1], matrix[0][2]);
    printf("%d %d %d\n", matrix[1][0], matrix[1][1], matrix[1][2]);
    return 0;
}
```

## 6. 코드 해석
row 0은 1, 2, 0이고 row 1은 3, 0, 0이다. 명시하지 않은 정수 elements가 우연히 0인 것이 아니라 aggregate initialization 규칙으로 0이 된다.

| element | value |
|---|---:|
| `matrix[0][0]` | 1 |
| `matrix[0][1]` | 2 |
| `matrix[0][2]` | 0 |
| `matrix[1][0]` | 3 |
| `matrix[1][1]`, `matrix[1][2]` | 0 |

## 7. 내부 동작
[C17 표준] aggregate initializer의 명시되지 않은 elements는 static storage duration 객체처럼 초기화된다. `{0}`은 첫 scalar element를 0으로 명시하고 나머지도 같은 규칙으로 0이 된다. 이는 `memset` 호출 문법이 아니다.

## 8. 자주 하는 실수
- row initializer와 column initializer를 반대로 읽는다.
- 부분 초기화의 나머지를 indeterminate value라고 생각한다.
- `{0}`을 특별한 memory function이라고 설명한다.
- 미초기화 automatic matrix를 읽는다.

## 9. 필수 실습
2x3 배열을 부분 초기화하고 0으로 채워지는 모든 element를 출력한다. [실습 README](../../exercises/12-two-dimensional-arrays/12-2/README.md)

## 10. 추가 실습
- ★ 2x2 완전 초기화
- ★★ 3x3 `{0}` 초기화
- ★★★ initializer와 row·column 대응표 작성

## 11. 확인 문제
1. 첫 nested initializer는 어느 row와 대응하는가?
2. `{{1,2},{3}}`에서 `matrix[0][2]`는?
3. `matrix[1][1]`은?
4. `{0}`이 전체를 0으로 만드는 이유는?
5. 각 row의 braces가 모든 C initializer에서 문법상 반드시 필요한가?

## 12. 핵심 정리
중첩 initializer는 row 구조를 드러내며 부족한 정수 elements는 C17 aggregate 규칙으로 0 초기화된다.

## 13. 다음 Step
[Step 12-3. 중첩 반복문 순회](12-3-nested-loop-traversal.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.7.9
- [cppreference: Array initialization](https://en.cppreference.com/w/c/language/array_initialization.html)
