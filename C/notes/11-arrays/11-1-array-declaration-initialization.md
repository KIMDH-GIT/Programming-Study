# 11-1. 배열 선언과 초기화

배열은 같은 element type의 객체를 정해진 개수만큼 연속해서 가지는 C의 aggregate type이다.

## 1. 학습 목표
- 배열 선언의 element type·identifier·element count를 구분한다.
- initializer list와 각 element의 초기값을 연결한다.
- 부분 초기화, `{0}`, 크기 생략 규칙을 설명한다.

## 2. 선수 지식
Part 2의 객체·자료형·초기화와 Part 10의 automatic object lifetime을 사용한다.

## 3. 핵심 개념
`int numbers[5];`에서 `int`는 element type, `numbers`는 array identifier, 5는 element count다. 유효한 index는 0~4다. 배열은 단순한 별도 변수 목록이 아니라 5개의 `int` element를 가진 하나의 array object다. automatic local 배열을 초기화하지 않으면 element 값도 정해지지 않으므로 저장 전에 읽지 않는다.

## 4. 문법
```c
int numbers[5] = {10, 20, 30, 40, 50};
int partial[5] = {1, 2};
int zeros[5] = {0};
int inferred[] = {10, 20, 30};
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int numbers[5] = {10, 20, 30, 40, 50};
    int partial[5] = {1, 2};
    int zeros[5] = {0};

    printf("%d %d\n", numbers[0], numbers[4]);
    printf("%d %d\n", partial[1], partial[4]);
    printf("%d\n", zeros[3]);
    return 0;
}
```

## 6. 코드 해석
initializer는 index 순서대로 들어간다. `numbers`의 첫·마지막 값은 10과 50이다. `partial`의 0, 1번은 1, 2이고 나머지는 C17 초기화 규칙에 따라 0이다. `{0}`은 첫 element를 0으로 명시하고 나머지도 0으로 초기화해 전체가 0이 된다.

## 7. 내부 동작
[C17 표준] initializer가 element보다 적으면 나머지는 static storage duration 객체와 같은 방식으로 초기화되어 정수 element는 0이 된다. `int inferred[] = {10, 20, 30};`처럼 initializer가 있으면 compiler가 count 3을 결정한다. 모든 미완성 `[]` 선언이 허용되는 것은 아니다.

## 8. 자주 하는 실수
- `[5]`를 마지막 index 5라고 생각한다.
- local `int numbers[5];`가 자동으로 모두 0이라고 생각한다.
- `{0}`을 `memset` 전용 문법이라고 설명한다.
- 배열 전체를 선언 후 다른 배열로 assignment할 수 있다고 생각한다.

## 9. 필수 실습
5개 정수 배열을 완전 초기화하고 첫·가운데·마지막 element를 출력한다. [실습 README](../../exercises/11-arrays/11-1/README.md)

## 10. 추가 실습
- ★ `{0}`으로 다섯 element 초기화
- ★★ 두 값만 명시해 나머지 0 확인
- ★★★ 크기를 생략한 initializer의 element count 설명

## 11. 확인 문제
1. `int values[5]`의 element type과 count는?
2. 유효한 마지막 index는?
3. `int values[5] = {1, 2};`의 `values[4]`는?
4. local 미초기화 배열 element를 읽어도 되는가?
5. `int values[];`를 모든 block 선언에서 사용할 수 있는가?

## 12. 핵심 정리
배열은 같은 타입 element를 연속해서 가지며, count와 index를 구분하고 initializer 규칙으로 모든 읽을 값을 먼저 정한다.

## 13. 다음 Step
[Step 11-2. index로 원소 읽기·쓰기](11-2-index-read-write.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.5, 6.7.6.2, 6.7.9
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html)
- [cppreference: Initialization](https://en.cppreference.com/w/c/language/array_initialization.html)
