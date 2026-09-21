# 18-12. `realloc` 후 기존 포인터·내부 별칭

성공한 `realloc`은 old allocation을 끝내므로 old pointer와 내부 element alias를 다시 계산해야 한다.

## 1. 학습 목표
- realloc 성공 뒤 old pointer를 사용하지 않는다.
- interior pointer alias가 invalid해지는 이유를 설명한다.
- 새 base pointer에서 필요한 위치를 다시 계산한다.

## 2. 선수 지식
Step 18-11의 realloc contract와 Part 15의 pointer arithmetic을 안다.

## 3. 핵심 개념
```c
int *element = &values[1];
int *temp = realloc(values, new_size);
```
성공하면 `values`와 `element`는 old allocation 관계의 pointer values이므로 사용하지 않는다. 새 `temp`를 base로 `&temp[1]`을 다시 계산한다. 숫자상 주소가 같아 보여도 예외가 아니다.

## 4. 문법
```c
if (temp != NULL) {
    values = temp;
    element = &values[1];
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *values = malloc(2 * sizeof *values);

    if (values == NULL) {
        return 1;
    }
    values[0] = 4;
    values[1] = 8;

    int *temp = realloc(values, 3 * sizeof *values);
    if (temp == NULL) {
        free(values);
        return 1;
    }
    values = temp;
    int *element = &values[1];
    printf("%d\n", *element);
    free(values);
    return 0;
}
```

## 6. 코드 해석
interior pointer는 realloc 성공 후 새 base에서 생성한다. old aliases를 보존하지 않는다.

## 7. 내부 동작
성공한 realloc이 같은 numeric address를 반환할 수 있어도 old allocation의 lifetime은 끝난다. 양수 크기 요청의 failure branch에서는 old allocation과 그 aliases가 유지된다. zero-size 요청은 이 일반화에서 제외한다.

## 8. 자주 하는 실수
- 주소가 같으면 old pointer도 계속 유효하다고 생각한다.
- base pointer만 갱신하고 element aliases는 그대로 둔다.
- realloc 호출 뒤 성공 여부 전에 old pointer를 관찰한다.

## 9. 필수 실습
realloc 성공 뒤 필요한 interior pointer를 새 base에서 다시 계산한다.
[18-12 exercise](../../exercises/18-dynamic-memory/18-12/README.md)

## 10. 추가 실습
- ★ alias lifetime 그림을 그린다.
- ★★ index를 저장했다가 새 base에서 pointer를 복원한다.
- ★★★ pointer 대신 index 보존이 유리한 이유를 설명한다.

## 11. 확인 문제
1. 성공한 realloc 뒤 old base pointer를 사용할 수 있는가?
2. interior pointer도 다시 계산해야 하는 이유는?
3. 반환 주소가 같으면 규칙이 달라지는가?
4. 양수 크기 요청의 failure 시 old aliases는 어떻게 되는가?

## 12. 핵심 정리
- realloc success는 old allocation lifetime을 끝낸다.
- 새 pointer를 base로 aliases를 다시 만든다.
- 이동 가능한 저장 공간에는 index 보존이 안전한 설계가 될 수 있다.

## 13. 다음 Step
[18-13. memory leak](18-13-memory-leak.md)

## 14. 참고 자료
- N1570 6.2.4 Object lifetime; 7.22.3.5 `realloc`. N1570은 **C11 공개 Committee Draft**다. C17 zero-size 예외는 WG14 DR 400 반영 문구로 구분한다.
- [WG14 N2243: DR 400 `realloc` with size zero](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2243.htm)
- [cppreference: realloc](https://en.cppreference.com/w/c/memory/realloc)
- [cppreference: pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic)
