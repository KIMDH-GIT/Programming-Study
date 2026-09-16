# 11-11. Part 11 종합 복습

Part 11에서는 1차원 배열의 선언·초기화·index·순회·배치·크기·경계와 기본 계산·검색을 학습했다.

## 1. 학습 목표
- 배열의 type, element count, index 범위를 종합한다.
- `sizeof` 기반 count와 안전한 순회를 적용한다.
- 배열 제약·Undefined Behavior·후속 pointer 범위를 구분한다.

## 2. 선수 지식
Step 11-1부터 11-10까지와 Part 7~10의 대입·조건·반복·함수 개념을 사용한다.

## 3. 핵심 개념
배열은 같은 element type 객체들의 연속된 집합이다. N개 배열의 유효 index는 0~N-1이며 모든 접근은 범위 안이어야 한다. 실제 array object에서는 전체 `sizeof`를 element `sizeof`로 나눠 count를 계산할 수 있다. 배열은 pointer와 동일하지 않으며 array parameter 규칙은 Part 15~16에서 다룬다.

## 4. 문법
```c
int values[] = {3, 1, 4, 1, 5};
size_t count = sizeof(values) / sizeof(values[0]);
for (size_t i = 0; i < count; ++i) {
    /* values[i] 사용 */
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[] = {3, 1, 4, 1, 5};
    size_t count = sizeof(values) / sizeof(values[0]);
    int sum = 0;

    for (size_t i = 0; i < count; ++i) {
        sum += values[i];
    }

    printf("count: %zu\n", count);
    printf("sum: %d\n", sum);
    return 0;
}
```

## 6. 코드 해석
initializer 수로 count 5가 결정된다. 모든 index 0~4를 순회해 합 14를 계산한다. index 5는 접근하지 않는다.

## 7. 내부 동작
[C17 표준] elements는 연속 배치되고 subscript는 유효 범위를 지정해야 한다. out-of-bounds는 Undefined Behavior다. 배열은 수정 가능한 lvalue가 아니므로 `b = a;` 형태의 배열 전체 assignment는 constraint violation이며 diagnostic 대상이다. `a == b`도 두 배열 element를 자동 비교하는 표현이 아니다. element 비교는 loop로 수행한다.

## 8. 자주 하는 실수
- element count와 마지막 index, byte 수를 혼동한다.
- 미초기화 local array element를 읽는다.
- `i <= count`로 순회한다.
- 배열 전체 assignment나 자동 element-wise 비교를 기대한다.
- 배열은 pointer라고 설명하거나 array parameter의 `sizeof`를 일반화한다.

## 9. 필수 실습
5개 배열을 초기화하고 count·모든 값·합을 출력한 뒤 경계를 설명한다. [실습 README](../../exercises/11-arrays/11-11/README.md)

## 10. 추가 실습
- ★ 모든 element 0 초기화
- ★★ 최댓값과 검색을 함께 수행
- ★★★ 잘못된 assignment·비교·범위 코드를 실행 없이 분류

## 11. 확인 문제
1. 5개 배열의 유효 index 범위는?
2. 부분 초기화의 나머지 정수 element 값은?
3. `sizeof(array)`와 count의 차이는?
4. out-of-bounds access의 C17 분류는?
5. 배열 전체를 `b = a`로 대입할 수 있는가?
6. `a == b`가 element 전체 비교인가?
7. 배열과 pointer가 같은 type인가?

## 12. 핵심 정리
배열은 연속된 element를 가진 독립적인 array type이며 초기화, count, index 경계와 제약을 지켜 1차원 데이터를 처리한다.

## 13. 다음 Step
Step 12-1. 행·열과 2차원 배열 선언

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.5.2.1, 6.5.3.4, 6.5.16, 6.7.6.2, 6.7.9
- [cppreference: Arrays](https://en.cppreference.com/w/c/language/array.html)
- [GCC: Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
