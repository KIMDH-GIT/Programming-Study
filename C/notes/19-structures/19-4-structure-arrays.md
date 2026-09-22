# 19-4. 구조체 배열

구조체도 완전한 object type이므로 array의 element type이 될 수 있다.

## 1. 학습 목표
- 구조체 배열을 선언·초기화·순회한다.
- `students[i].id`의 연산 순서를 설명한다.
- 배열 member와 구조체 배열을 구분한다.

## 2. 선수 지식
Part 11의 배열과 19-2의 member access를 안다.

## 3. 핵심 개념
`struct Student students[3];`은 element type이 `struct Student`, element count가 3인 실제 array다. `students[i]`가 하나의 structure object를 선택하고 `.id`가 그 object의 member를 선택한다.

## 4. 문법
```c
struct Student students[3] = {
    {1, 90.0},
    {2, 85.5},
    {3, 78.0}
};
```
중첩 braces는 각 array element의 aggregate initializer를 분명하게 한다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Student {
    int id;
    double score;
};

int main(void)
{
    struct Student students[] = {
        {1, 90.0},
        {2, 85.5},
        {3, 78.0}
    };
    size_t count = sizeof students / sizeof students[0];

    for (size_t i = 0; i < count; ++i) {
        printf("%d %.1f\n", students[i].id, students[i].score);
    }
    return 0;
}
```

## 6. 코드 해석
`sizeof` 식은 현재 scope의 실제 array 전체 크기를 element 크기로 나눈다. loop는 `0 <= i < count`를 지켜 각 element와 member에 접근한다.

## 7. 내부 동작
- **[C17 표준]** array elements는 연속하며 각 element는 완전한 structure object다.
- 각 structure element 내부에는 구현이 정한 padding이 있을 수 있다.
- array stride는 `sizeof(struct Student)`이며 member 크기 합으로 임의 계산하지 않는다.

## 8. 자주 하는 실수
- `students.id`처럼 array에 직접 `.`를 적용한다.
- `students[i]`와 `students[i].id`를 같은 object라고 생각한다.
- bounds 밖 element에 접근한다.
- `sizeof` 원소 수 계산을 pointer parameter에도 그대로 적용한다.

## 9. 필수 실습
학생 세 명의 배열을 초기화하고 loop로 id와 score를 출력한다.
[19-4 exercise](../../exercises/19-structures/19-4/README.md)

## 10. 추가 실습
- ★ 평균 점수를 계산한다.
- ★★ 최고 점수 학생을 찾는다.
- ★★★ 조건에 맞는 학생만 새 array가 아닌 index 목록으로 표시한다.

## 11. 확인 문제
1. `students[3]`의 element type과 count는?
2. `students[i].id`에서 먼저 평가되는 선택은?
3. structure element들은 연속인가?
4. element 내부 member도 padding 없이 항상 붙는가?
5. pointer parameter에서 `sizeof`로 count를 구할 수 있는가?

## 12. 핵심 정리
- 구조체 배열은 structure object들의 array다.
- index로 element를 고른 뒤 `.`로 member를 고른다.
- bounds와 element count를 별도로 관리한다.

## 13. 다음 Step
[19-5. 구조체 포인터와 `->`](19-5-structure-pointers-arrow.md)

## 14. 참고 자료
- N1570 6.2.5, 6.5.2.1, 6.5.2.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: array](https://en.cppreference.com/w/c/language/array)
- [cppreference: member access](https://en.cppreference.com/w/c/language/operator_member_access)
