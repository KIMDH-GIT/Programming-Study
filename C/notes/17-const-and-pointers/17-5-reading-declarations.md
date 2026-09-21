# 17-5. 선언을 오른쪽에서 왼쪽으로 읽기

const-pointer 조합은 identifier에서 시작해 가까운 요소부터 바깥으로 읽으면 어떤 대상이 한정되는지 구분하기 쉽다.

## 1. 학습 목표
- identifier에서 시작해 현재 범위의 pointer 선언을 읽는다.
- `*` 오른쪽과 왼쪽의 `const` 역할을 구분한다.
- 선언을 수정 가능 여부 질문으로 검증한다.

## 2. 선수 지식
Step 17-2부터 17-4까지의 세 pointer/const 형태를 안다.

## 3. 핵심 개념
identifier `p`에서 시작한다.

```text
const int *p
           ^ p
         *   pointer
const int   const int를 가리킴
```

```text
int *const p
           ^ p
     const   const인
    *        pointer
int          int를 가리킴
```

```text
const int *const p
                 ^ p
           const   const인
          *        pointer
const int          const int를 가리킴
```

"오른쪽에서 왼쪽"은 이 단순 선언을 익히는 학습 규칙이다. 복잡한 모든 C declarator 이론으로 확장하지 않는다.

## 4. 문법
각 선언을 읽은 뒤 세 질문으로 확인한다.

1. pointer `p` 자체를 재지정할 수 있는가?
2. 해당 pointer를 통해 pointed-to object를 수정할 수 있는가?
3. pointed-to object 자체는 실제로 const object로 선언되었는가?

세 번째 답은 pointer 선언만으로 항상 알 수 없다. `const int *p`가 non-const 객체를 가리킬 수도 있기 때문이다.

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int value = 10;
    const int *read_only_path = &value;
    int *const fixed_path = &value;
    const int *const fixed_read_only_path = &value;

    *fixed_path = 20;
    printf("%d %d %d\n",
           *read_only_path,
           *fixed_path,
           *fixed_read_only_path);
    return 0;
}
```

## 6. 코드 해석
- `read_only_path`: 재지정 가능, 이 pointer를 통한 수정 불가.
- `fixed_path`: 재지정 불가, 이 pointer를 통한 수정 가능.
- `fixed_read_only_path`: 재지정 불가, 이 pointer를 통한 수정 불가.
- 세 pointer는 모두 같은 non-const `value`를 가리킬 수 있다.

## 7. 내부 동작
declarator는 identifier의 타입을 구성한다. `const`가 `*`로 만들어진 pointer type에 붙는지, `int`에 붙는지가 의미를 결정한다. 실행 시점에는 각 pointer object가 같은 주소 값을 저장할 수 있어도 compiler는 각 expression의 타입에 따라 허용되는 연산을 검사한다.

## 8. 자주 하는 실수
- 선언의 왼쪽에 `const`가 보이면 무조건 pointer 자체가 const라고 생각한다.
- `const` 개수만 세고 어느 타입 층에 적용되는지 보지 않는다.
- pointer 선언만 보고 실제 pointed-to object의 선언까지 const라고 단정한다.
- 여러 declarator를 한 줄에 섞어 읽기 어렵게 만든다.

## 9. 필수 실습
네 가지 기본 선언을 각각 한 줄에 쓰고 세 질문에 답한다. 허용되는 연산만 포함한 작은 프로그램으로 결과를 확인한다.

실습 안내: [17-5 exercise](../../exercises/17-const-and-pointers/17-5/README.md)

## 10. 추가 실습
- ★ 기초: 선언을 자연어로 한 줄씩 읽는다.
- ★★ 응용: 세 pointer가 같은 객체를 가리키는 그림을 그린다.
- ★★★ 도전: compiler가 금지할 연산을 실행하지 않는 주석형 분석표로 만든다.

## 11. 확인 문제
1. `const int *p`에서 `p`는 재지정 가능한가?
2. `int *const p`에서 `*p`는 수정 가능한가?
3. `const int *const p`에는 몇 개의 타입 층이 한정되는가?
4. pointer to const가 가리키는 실제 object가 반드시 const object인가?
5. 선언을 읽은 뒤 확인할 세 질문은 무엇인가?

## 12. 핵심 정리
- identifier에서 시작해 가까운 `const`, `*`, 기본 타입 순서로 읽는다.
- pointer 자체와 pointed-to type의 qualification을 분리한다.
- 수정 가능 여부는 `p`와 `*p`에 따로 묻는다.
- 실제 object의 선언은 pointer type만으로 단정하지 않는다.

## 13. 다음 Step
[17-6. const qualifier 추가·제거](17-6-qualification-conversions.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.7.6 Declarators. N1570은 **C11 공개 Committee Draft**이며 관련 declarator 규칙은 C17에서도 유지된다.
- [cppreference: declarations](https://en.cppreference.com/w/c/language/declarations)
- [cppreference: pointer declarations](https://en.cppreference.com/w/c/language/pointer)
