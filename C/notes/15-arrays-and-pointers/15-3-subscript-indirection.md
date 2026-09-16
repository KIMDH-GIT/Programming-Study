# 15-3. `arr[i]`와 `*(arr + i)`

C subscript expression `arr[i]`는 정의상 `*(arr + i)`와 같은 element를 지정한다.

## 1. 학습 목표
- array conversion·pointer addition·indirection의 결합을 설명한다.
- pointer에도 subscript가 적용되는 이유를 이해한다.
- 같은 array bounds 안에서만 접근한다.

## 2. 선수 지식
Step 15-1의 conversion과 Part 14의 indirection을 사용한다.

## 3. 핵심 개념
`arr`가 일반 expression context에서 first element pointer로 변환되고 `+ i`가 같은 array의 i번째 element pointer를 계산한다. unary `*`가 그 element를 지정하므로 `arr[i]`와 `*(arr+i)`가 같다. 이유는 “배열이 pointer이기 때문”이 아니다.

## 4. 문법
```c
arr[i]
*(arr + i)
pointer[i]
*(pointer + i)
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *pointer = values;

    for (int i = 0; i < 5; ++i) {
        printf("%d %d %d\n",
               values[i],
               *(values + i),
               pointer[i]);
    }
    return 0;
}
```

## 6. 코드 해석
각 row에서 세 expressions가 동일한 i번째 int element를 지정해 같은 값을 출력한다. i는 0~4라 모든 accesses가 bounds 안이다.

## 7. 내부 동작
[C17 abstract machine] `E1[E2]`는 `*((E1)+(E2))`와 동일하게 정의된다. pointer arithmetic은 related array element 범위에서 적용된다. compiler가 실제 multiplication/add/load sequence를 반드시 그대로 생성하는 것은 아니다.

## 8. 자주 하는 실수
- equivalence 이유를 array와 pointer가 같은 object라서라고 설명한다.
- i가 count일 때도 dereference한다.
- pointer subscript가 무조건 임의 memory를 안전하게 읽는다고 생각한다.
- `i[arr]`를 기본 coding style로 권장한다.

## 9. 필수 실습
다섯 elements를 subscript와 pointer expression 두 방식으로 출력해 비교한다. [실습 README](../../exercises/15-arrays-and-pointers/15-3/README.md)

## 10. 추가 실습
- ★ pointer subscript로 값 수정
- ★★ double array에서 두 표현 비교
- ★★★ conversion부터 indirection까지 단계도 작성

## 11. 확인 문제
1. `arr[i]`의 정의상 형태는?
2. arr에는 어떤 conversion이 일어나는가?
3. `pointer[i]`도 가능한가?
4. i가 count이면 dereference 가능한가?
5. 이 관계가 array object와 pointer object가 같다는 뜻인가?

## 12. 핵심 정리
subscript는 pointer addition과 indirection으로 정의되지만 array object와 pointer object의 구분은 유지된다.

## 13. 다음 Step
[Step 15-4. `&arr[i]`와 `arr + i`](15-4-element-address.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.1, 6.5.6
- [cppreference: Subscript operator](https://en.cppreference.com/w/c/language/operator_member_access.html#Subscript)
