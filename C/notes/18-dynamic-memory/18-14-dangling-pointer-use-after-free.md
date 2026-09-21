# 18-14. dangling pointer와 use-after-free

deallocation 뒤 old allocation을 가리키던 pointer는 valid target을 잃으며, 이를 통한 access는 undefined behavior다.

## 1. 학습 목표
- dangling pointer와 use-after-free를 구분한다.
- aliases가 함께 invalid해짐을 설명한다.
- UB 코드를 실행하지 않고 판별한다.

## 2. 선수 지식
Step 18-2의 lifetime과 Step 18-8의 aliases를 안다.

## 3. 핵심 개념
```c
int *q = p;
free(p);
/* *q 접근은 금지 */
```
`q`에 다른 pointer value가 저장되어 있어도 target lifetime은 끝났다. `p = NULL`은 `q`를 바꾸지 않는다.

## 4. 문법
안전한 순서는 모든 access를 끝낸 뒤 한 owner가 `free`하고, 이후 aliases를 사용하지 않는 것이다.

다음은 **분석 전용이며 실행하지 않는 코드**다.

```c
int *p = malloc(sizeof *p);
if (p != NULL) {
    int *alias = p;
    *p = 10;
    free(p);
    /* printf("%d\n", *alias); */ /* use-after-free: 실행 금지 */
}
```

`free(p)`가 allocation lifetime을 끝내므로 주석 처리된 dereference가 잘못된 줄이다.

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
    *p = 12;
    printf("%d\n", *p);
    free(p);
    p = NULL;
    return 0;
}
```

## 6. 코드 해석
마지막 access는 `free` 전이다. 해제 뒤에는 old pointer를 읽거나 dereference하지 않고 새 null value를 저장한다.

## 7. 내부 동작
use-after-free는 C17 abstract machine에서 lifetime 밖의 access이므로 UB다. segmentation fault, 조용한 오작동, 우연한 출력 중 무엇이 나타날지는 보장되지 않는다.

## 8. 자주 하는 실수
- 해제된 주소가 아직 보이면 안전하다고 생각한다.
- `p = NULL`이면 aliases도 안전하다고 생각한다.
- UB가 항상 특정 runtime error를 낸다고 생각한다.

## 9. 필수 실습
정상 lifetime timeline을 작성하고 4절의 분석 전용 fragment에서 잘못된 줄을 실행 없이 찾는다.
[18-14 exercise](../../exercises/18-dynamic-memory/18-14/README.md)

## 10. 추가 실습
- ★ owner와 borrower 역할을 표시한다.
- ★★ alias invalidation을 그림으로 표현한다.
- ★★★ index 기반 설계가 realloc aliases를 줄이는 이유를 설명한다.

## 11. 확인 문제
1. dangling pointer란 무엇인가?
2. use-after-free는 어떤 behavior 분류인가?
3. segmentation fault가 항상 발생하는가?
4. null 대입이 aliases를 해결하는가?

## 12. 핵심 정리
- deallocation 뒤 target access는 금지한다.
- aliases까지 lifetime 관계를 추적한다.
- UB와 특정 OS 증상을 구분한다.

## 13. 다음 Step
[18-15. double free와 `NULL` dereference](18-15-double-free-null-dereference.md)

## 14. 참고 자료
- N1570 6.2.4 Object lifetime; 7.22.3.3 `free`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: lifetime](https://en.cppreference.com/w/c/language/lifetime)
- [SEI CERT MEM30-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM30-C.+Do+not+access+freed+memory)
