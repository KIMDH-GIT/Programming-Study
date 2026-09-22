# 19-3. `typedef struct`

`typedef`는 새 type을 만드는 문법이 아니라 기존 type에 사용할 이름을 선언한다.

## 1. 학습 목표
- structure tag와 typedef name을 구분한다.
- named structure에 typedef alias를 선언한다.
- alias 사용 전후 type identity를 설명한다.

## 2. 선수 지식
19-1의 `struct Tag`와 19-2의 object declaration을 안다.

## 3. 핵심 개념
```c
typedef struct Student Student;
```
이 declaration 뒤 `struct Student`와 `Student`는 같은 structure type을 가리키는 두 표기다. tag는 tag namespace의 이름이고 typedef name은 ordinary identifier namespace의 이름이다.

## 4. 문법
```c
struct Student {
    int id;
    double score;
};

typedef struct Student Student;

Student student = {1, 95.5};
```
커리큘럼이 이 Step에서 명시하므로 typedef를 도입한다. `typedef struct Student { ... } Student;`처럼 정의와 alias 선언을 합칠 수도 있다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Student {
    int id;
    double score;
};

typedef struct Student Student;

int main(void)
{
    Student a = {1, 90.0};
    struct Student b = a;
    printf("%d %.1f\n", b.id, b.score);
    return 0;
}
```

## 6. 코드 해석
`Student`는 `struct Student`의 alias다. `a`와 `b`는 compatible한 정도가 아니라 같은 declared structure type의 objects이므로 assignment할 수 있다.

## 7. 내부 동작
- **[C17 표준]** typedef name은 type의 synonym이며 distinct type을 생성하지 않는다.
- tag와 typedef name은 서로 다른 namespace에 있어 같은 spelling을 사용할 수 있다.
- typedef는 runtime storage나 machine instruction을 만들지 않는다.

## 8. 자주 하는 실수
- 모든 `struct Point`에서 `Point`가 자동 type name이라고 생각한다.
- typedef가 C++ class처럼 별도 type을 만든다고 생각한다.
- typedef name만 보고 pointer 여부나 ownership을 숨긴다.

## 9. 필수 실습
named `struct Student`를 먼저 정의하고 `Student` alias를 선언해 두 표기를 한 프로그램에서 사용한다.
[19-3 exercise](../../exercises/19-structures/19-3/README.md)

## 10. 추가 실습
- ★ 결합형 typedef declaration으로 바꾼다.
- ★★ tag 표기와 alias 표기를 섞어 assignment한다.
- ★★★ pointer alias가 readability를 해칠 수 있는 이유를 적는다.

## 11. 확인 문제
1. typedef는 새 distinct type을 만드는가?
2. `Student`와 `struct Student`의 관계는?
3. tag와 typedef name은 같은 namespace인가?
4. typedef declaration이 runtime object를 생성하는가?

## 12. 핵심 정리
- tag와 typedef name은 다른 이름 체계다.
- typedef는 기존 type의 alias다.
- alias는 문법을 줄이지만 type semantics를 바꾸지 않는다.

## 13. 다음 Step
[19-4. 구조체 배열](19-4-structure-arrays.md)

## 14. 참고 자료
- N1570 6.2.3, 6.7.8. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: typedef declaration](https://en.cppreference.com/w/c/language/typedef)
- [cppreference: name space](https://en.cppreference.com/w/c/language/name_space)
