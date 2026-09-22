# 19-1. 구조체 정의와 멤버

구조체는 관련 있는 여러 객체를 하나의 새로운 aggregate type으로 묶는다. 배열과 달리 멤버들의 type은 서로 달라도 된다.

## 1. 학습 목표
- structure tag, member, structure object를 구분한다.
- 구조체 정의가 type을 선언하며 객체를 자동 생성하지 않음을 설명한다.
- 배열과 구조체의 차이를 설명한다.

## 2. 선수 지식
Part 2의 type과 객체, Part 11의 배열을 안다.

## 3. 핵심 개념
```c
struct Student {
    int id;
    double score;
    char grade;
};
```
`struct`는 C keyword, `Student`는 structure tag, `id`·`score`·`grade`는 members다. 이 declaration은 `struct Student`라는 type을 정의한다. runtime object는 `struct Student student;`처럼 별도로 선언한다. 구조체와 배열은 서로 다른 aggregate type이다.

## 4. 문법
```c
struct Point {
    int x;
    int y;
};

struct Point p;
```
C에서 tag만 정의했을 때 type 이름은 `struct Point`다. 별도 `typedef` 없이 `Point p;`라고 쓸 수 없다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

int main(void)
{
    struct Point p = {10, 20};
    printf("(%d, %d)\n", p.x, p.y);
    return 0;
}
```

## 6. 코드 해석
정의는 두 `int` member를 가진 type을 만든다. `p`는 그 type의 object이며 initializer가 `x`, `y`를 선언 순서대로 초기화한다.

## 7. 내부 동작
- **[C17 표준]** non-bit-field members와 그 안의 allocation unit은 선언 순서대로 더 높은 주소에 놓이며 첫 member 앞에는 padding이 없다. member 사이와 끝에는 unnamed padding이 있을 수 있다.
- **[compiler/ABI]** 실제 offset, alignment, 전체 크기를 정한다.
- **[CPU]** 생성된 load/store를 실행한다. 특정 ISA가 C 구조체 layout을 보장하는 것은 아니다.

## 8. 자주 하는 실수
- 구조체를 “여러 변수를 묶은 배열”이라고 부른다.
- 정의만으로 객체가 하나 생긴다고 생각한다.
- `struct Point`와 typedef 이름 `Point`를 혼동한다.
- 크기를 member 크기의 단순 합으로 단정한다.

## 9. 필수 실습
서로 다른 type의 member를 가진 `struct Student`를 정의하고 객체 하나를 초기화해 출력한다.
[19-1 exercise](../../exercises/19-structures/19-1/README.md)

## 10. 추가 실습
- ★ `struct Rectangle`을 정의한다.
- ★★ 배열과 구조체의 element/member type 차이를 표로 쓴다.
- ★★★ 같은 member 이름을 가진 서로 다른 structure type을 정의해 본다.

## 11. 확인 문제
1. `struct`, `Point`, `x`는 각각 무엇인가?
2. 구조체 정의만으로 runtime object가 생기는가?
3. `Point p;`가 가능한 조건은?
4. 배열과 구조체의 type 구성 차이는?
5. 첫 member 앞 padding에 관한 C17 규칙은?

## 12. 핵심 정리
- 구조체는 서로 다른 type의 named member를 가질 수 있는 aggregate type이다.
- tag를 쓸 때 type specifier는 `struct Tag`다.
- type 정의와 object 선언은 별개다.

## 13. 다음 Step
[19-2. 구조체 선언·초기화와 `.`](19-2-declaration-initialization-member-access.md)

## 14. 참고 자료
- N1570 6.7.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 structure 규칙은 C17에서도 유지된다.
- [cppreference: struct declaration](https://en.cppreference.com/w/c/language/struct)
- [cppreference: object](https://en.cppreference.com/w/c/language/object)
