# 18-2. 자동 객체와 동적 객체의 lifetime

automatic object는 block 실행과 연결되고, allocated object의 lifetime은 allocation·deallocation 호출로 관리한다.

## 1. 학습 목표
- automatic object와 allocated object의 lifetime을 비교한다.
- pointer의 lifetime과 target의 lifetime을 분리한다.
- `free` 책임을 ownership discipline으로 표현한다.

## 2. 선수 지식
Step 18-1의 storage duration과 Part 14의 valid pointer 규칙을 안다.

## 3. 핵심 개념
pointer가 존재한다고 target의 lifetime이 유지되는 것은 아니다. allocation한 저장 공간을 가리키는 pointer를 잃으면 해제할 방법을 잃을 수 있고, `free` 후 alias가 남으면 dangling pointer가 된다.

## 4. 문법
```c
int *p = malloc(sizeof *p);
if (p != NULL) {
    *p = 10;
    free(p);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof *p);

    if (p == NULL) {
        return 1;
    }
    *p = 42;
    printf("%d\n", *p);
    free(p);
    p = NULL;
    return 0;
}
```

## 6. 코드 해석
allocation 성공 뒤 값을 저장해 object access를 시작한다. `free`가 lifetime을 끝낸 뒤 `p`에는 새 null pointer value를 저장한다. 이는 다른 aliases까지 바꾸지 않는다.

## 7. 내부 동작
**[C17]** allocated object의 lifetime은 allocation부터 deallocation까지다. **[구현]** allocator가 같은 주소를 나중에 재사용할 수 있지만 프로그램은 해제된 object에 접근할 수 없다.

## 8. 자주 하는 실수
- pointer가 scope 안에 있으면 target도 살아 있다고 생각한다.
- `p = NULL`이 모든 aliases를 안전하게 만든다고 생각한다.
- `free` 뒤 pointer value를 출력하거나 비교해도 항상 안전하다고 생각한다.

## 9. 필수 실습
allocation·값 저장·읽기·`free`·`NULL` 대입 순서를 주석으로 설명한다.
[18-2 exercise](../../exercises/18-dynamic-memory/18-2/README.md)

## 10. 추가 실습
- ★ lifetime timeline을 그린다.
- ★★ pointer object와 allocated object의 scope/lifetime을 비교한다.
- ★★★ 두 aliases가 있을 때 deallocation 이후 상태를 분석한다.

## 11. 확인 문제
1. pointer object가 남아 있으면 allocated object도 살아 있는가?
2. `free`는 어느 lifetime을 끝내는가?
3. `p = NULL`은 다른 alias에 어떤 영향을 주는가?
4. ownership discipline은 C의 공식 type system인가?

## 12. 핵심 정리
- pointer lifetime과 target lifetime은 다르다.
- `free` 책임을 명확히 정해야 한다.
- null 대입은 한 pointer object의 재사용 위험만 줄인다.

## 13. 다음 Step
[18-3. 지역 변수 주소 반환의 문제](18-3-returning-local-address.md)

## 14. 참고 자료
- N1570 6.2.4 Storage durations of objects, 7.22.3.3 `free`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: object lifetime](https://en.cppreference.com/w/c/language/lifetime)
- [cppreference: free](https://en.cppreference.com/w/c/memory/free)
