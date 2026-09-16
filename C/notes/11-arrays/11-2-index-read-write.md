# 11-2. index로 원소 읽기·쓰기

subscript expression `array[index]`는 배열 안의 특정 element를 지정하며 scalar 객체처럼 읽고 수정할 수 있다.

## 1. 학습 목표
- index로 element를 읽고 assignment한다.
- 배열 전체와 특정 element를 구분한다.
- 0부터 N-1까지의 유효 범위를 적용한다.

## 2. 선수 지식
Step 11-1의 array object와 Part 7의 assignment를 안다.

## 3. 핵심 개념
`numbers[2]`는 `numbers` 배열의 세 번째 `int` element를 지정한다. `numbers[2] = 30;`은 배열 전체 대입이 아니라 그 element 하나에 대한 assignment다. index expression은 정수형이어야 하며 실제 접근은 반드시 배열 범위 안이어야 한다.

## 4. 문법
```c
value = array[index];
array[index] = new_value;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int numbers[5] = {10, 20, 30, 40, 50};

    printf("%d\n", numbers[2]);
    numbers[2] = 300;
    printf("%d\n", numbers[2]);
    return 0;
}
```

## 6. 코드 해석
index 2는 세 번째 element다. 첫 출력은 30이고 assignment 후 같은 element 값이 300이 되어 두 번째 출력도 이를 반영한다. 다른 네 element는 바뀌지 않는다.

## 7. 내부 동작
[C17 표준] subscript expression은 배열 element를 지정하는 lvalue가 될 수 있어 assignment의 왼쪽에 놓인다. 배열과 pointer의 정확한 연산 관계는 Part 14~15에서 다룬다. 이번 Step에서는 index와 유효 범위에 집중한다.

## 8. 자주 하는 실수
- 첫 element를 index 1로 읽는다.
- `numbers[5]`를 5개 배열의 마지막 element로 생각한다.
- element assignment를 배열 전체 assignment라고 부른다.
- index가 음수여도 첫 element 앞의 값이 정의되어 있다고 생각한다.

## 9. 필수 실습
5개 배열의 두 번째와 네 번째 element를 읽고 각각 새 값으로 수정한다. [실습 README](../../exercises/11-arrays/11-2/README.md)

## 10. 추가 실습
- ★ index 0 값 변경
- ★★ 두 element 값 서로 계산해 새 값 저장
- ★★★ 여러 index 중 유효 범위 판별 문제

## 11. 확인 문제
1. `values[0]`은 몇 번째 element인가?
2. 5개 배열의 마지막 유효 index는?
3. `values[2] = 30;`은 무엇을 수정하는가?
4. 음수 index 접근은 정의되어 있는가?
5. 배열 indexing을 pointer로 본격 설명하지 않는 이유는?

## 12. 핵심 정리
`array[index]`는 element 하나를 지정하며 유효 index 0~N-1 안에서 읽기와 assignment를 수행한다.

## 13. 다음 Step
[Step 11-3. 반복문으로 배열 순회](11-3-loop-traversal.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.16
- [cppreference: Array subscripting](https://en.cppreference.com/w/c/language/operator_member_access.html#Subscript)
