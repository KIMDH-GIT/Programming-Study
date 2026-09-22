# 20-5. `union`의 공유 저장 공간

union은 모든 members가 같은 storage를 겹쳐 사용하도록 정의하는 union type이다. C17 용어에서 aggregate type은 array와 structure types를 가리키므로 union을 aggregate라고 부르지 않는다.

## 1. 학습 목표
- union tag, member, object를 구분한다.
- struct와 union의 storage 모델을 비교한다.
- 현재 의미 있는 member를 프로그램이 추적해야 함을 설명한다.

## 2. 선수 지식
Part 19 구조체와 object lifetime, 20-1 tag를 안다.

## 3. 핵심 개념
```c
union Value {
    int i;
    double d;
};
```
`union`은 keyword, `Value`는 union tag, `i`와 `d`는 members다. struct는 members가 독립된 storage를 갖지만 union은 같은 storage를 공유한다.

## 4. 문법
```c
union Value value = {.d = 3.14};
value.i = 10;
```
designated initializer는 `d`를 초기화한다. 이후 `i`에 저장하면 이제 `i` member가 저장된 value를 나타낸다. 이전 `d` 값이 별도 공간에 보존되는 것이 아니다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

union Value {
    int i;
    double d;
};

int main(void)
{
    union Value value = {.d = 3.5};
    printf("%.1f\n", value.d);
    value.i = 10;
    printf("%d\n", value.i);
    return 0;
}
```

## 6. 코드 해석
각 출력은 가장 최근에 저장한 member와 같은 member를 읽는다. 두 values가 동시에 독립적으로 남아 있지 않다.

## 7. 내부 동작
- **[C17 표준]** union은 members 중 최대 하나의 value만 언제든 저장할 수 있을 만큼의 storage를 가진다.
- suitably converted union pointer는 각 member를 가리키며, 그 반대도 성립한다. 모든 members는 같은 시작 주소에 대응한다.
- union size는 모든 member를 담기에 충분하고 끝 padding이 있을 수 있어 가장 큰 member `sizeof`와 정확히 같다고 보장할 수 없다.
- **[compiler/ABI]** 실제 size와 alignment를 정한다.

## 8. 자주 하는 실수
- union을 “메모리를 아끼는 작은 struct”로만 설명한다.
- 모든 member values가 동시에 보존된다고 생각한다.
- union size가 항상 가장 큰 member 크기와 정확히 같다고 단정한다.
- 다른 member 읽기를 portable numeric conversion으로 사용한다.

## 9. 필수 실습
`int`와 `double` member를 가진 union에서 각 member에 저장한 직후 같은 member를 출력한다.
[20-5 exercise](../../exercises/20-enum-typedef-union/20-5/README.md)

## 10. 추가 실습
- ★ member 주소를 `%p`와 `(void *)`로 관찰한다.
- ★★ struct와 union의 `sizeof`를 현재 구현에서 비교한다.
- ★★★ inactive member 읽기가 안전한 변환이 아닌 이유를 분석한다.

## 11. 확인 문제
1. struct와 union의 member storage 차이는?
2. `value.i = 10` 뒤 `d` 값도 독립적으로 보존되는가?
3. union members의 시작 주소 관계는?
4. union 크기는 항상 가장 큰 member 크기와 같은가?
5. 다른 member 읽기를 portable cast로 볼 수 있는가?

## 12. 핵심 정리
- union members는 같은 storage를 공유한다.
- 프로그램이 현재 저장한 member를 알아야 한다.
- size·alignment는 C17의 최소 요구 안에서 구현이 정한다.

## 13. 다음 Step
[20-6. tag와 union을 함께 쓰기](20-6-tagged-union.md)

## 14. 참고 자료
- N1570 6.5.2.3, 6.7.2.1, 6.7.9. N1570은 **C11 공개 Committee Draft**이며 관련 union 규칙은 C17에서도 유지된다.
- [cppreference: union declaration](https://en.cppreference.com/w/c/language/union)
- [cppreference: struct and union initialization](https://en.cppreference.com/w/c/language/struct_initialization)
