# 14-12. 포인터 기초 종합 실습

object를 만들고 address를 pointer에 저장한 뒤 읽기·수정·alias 관찰을 수행하면 pointer 핵심 관계를 한 흐름에서 확인할 수 있다.

## 1. 학습 목표
- object·pointer object·pointer value를 종합한다.
- two pointers가 같은 object를 가리키는 alias를 관찰한다.
- `%p`, dereference, NULL state를 올바르게 사용한다.

## 2. 선수 지식
Step 14-1부터 14-11까지의 declarations·address·indirection·lifetime을 사용한다.

## 3. 핵심 개념
여러 pointers가 같은 object를 가리키면 aliases다. 한 pointer로 object를 수정하면 다른 pointer로 읽어도 같은 object의 새 value가 보인다. 이는 pointers가 값을 공유해서가 아니라 둘의 pointer values가 같은 object를 지정하기 때문이다.

## 4. 문법
```c
int value = 10;
int *first = &value;
int *second = &value;
*first = 30;
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int value = 10;
    int *first = &value;
    int *second = &value;

    printf("address: %p\n", (void *)&value);
    printf("first: %d, second: %d\n", *first, *second);

    *first = 30;
    printf("value: %d, second: %d\n", value, *second);
    return 0;
}
```

## 6. 코드 해석
first와 second는 모두 value를 가리킨다. 첫 출력에서 둘 다 10이다. first를 통해 value를 30으로 바꾸면 direct read와 second dereference 모두 30이다.

## 7. 내부 동작
[C17 abstract machine] aliases는 같은 object를 지정하는 pointer values다. pointer value 전달이 함수에 등장하더라도 C의 argument passing은 pass by value이며 copied pointer parameter가 target object를 access할 수 있는 것이다. 실제 pointer parameter 사용은 Part 16에서 다룬다.

## 8. 자주 하는 실수
- aliases가 서로의 pointer objects를 수정한다고 생각한다.
- pointer를 넘기면 C가 call by reference로 바뀐다고 설명한다.
- pointer arithmetic이나 array conversion을 이번 실습에 추가한다.
- pointer가 value object를 소유한다고 말한다.

## 9. 필수 실습
two aliases를 만들고 한 pointer로 수정한 뒤 direct value와 다른 pointer로 확인한다. [실습 README](../../exercises/14-pointers/14-12/README.md)

## 10. 추가 실습
- ★ 세 aliases
- ★★ pointer 중 하나를 다른 object로 다시 지정
- ★★★ object와 pointer states 변화 diagram

## 11. 확인 문제
1. alias란 무엇인가?
2. first를 통해 수정하면 second로 읽는 값은?
3. 두 pointer objects 자체가 같은 object인가?
4. pointer argument도 어떤 전달 방식인가?
5. pointer arithmetic은 어느 Part에서 배우는가?

## 12. 핵심 정리
aliases는 같은 live object를 가리키며 한 alias의 indirection assignment가 그 shared pointed-to object를 바꾼다.

## 13. 다음 Step
[Step 14-13. Part 14 종합 복습](14-13-part-14-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.4, 6.3.2.3, 6.5.3.2
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
