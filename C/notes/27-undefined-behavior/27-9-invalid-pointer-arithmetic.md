# 27-9. invalid pointer arithmetic
## 1. 학습 목표
- pointer arithmetic의 array object 경계를 설명한다.
- unrelated pointer의 뺄셈과 relational comparison 제한을 안다.
- raw numeric address 모델을 C pointer 규칙 대신 사용하지 않는다.
## 2. 선수 지식
Part 14의 pointer type과 Part 15의 array pointer arithmetic을 안다.
## 3. 핵심 개념
pointer에 정수를 더하거나 빼는 연산은 같은 array object의 원소 또는 그 one-past 위치를 결과로 만들 때 정의된다. 단일 object도 이 규칙에서는 길이 1인 배열처럼 취급한다.

⚠ 분석용 — 실행하지 않는다.
```c
int a[2];
int b[2];
ptrdiff_t distance = &b[0] - &a[0]; /* unrelated arrays: UB */
int *bad = &a[0] + 3;                /* 허용 범위 밖 pointer 형성: UB */
```
## 4. 문법
같은 배열의 시작·끝 pointer를 사용한다.
```c
int *begin = values;
int *end = values + count;
for (int *p = begin; p != end; ++p) {
    use(*p);
}
```
같은 array의 두 pointer subtraction 결과는 `ptrdiff_t`로 표현 가능해야 한다. unrelated objects의 `<`, `>`를 portable total memory ordering으로 사용하지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int values[] = {4, 5, 6};
    int *begin = values;
    int *end = values + 3;
    ptrdiff_t count = end - begin;
    int sum = 0;

    for (int *p = begin; p != end; ++p) {
        sum += *p;
    }
    printf("%td %d\n", count, sum);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o pointer_arithmetic
./pointer_arithmetic
```
## 6. 코드 해석
모든 pointer가 같은 `values` 배열의 원소 또는 one-past를 나타낸다. 출력은 `3 15`다.
## 7. 내부 동작
**[C17]** additive pointer operators, subtraction, relational comparison은 array-object 관계를 사용한다. 단순히 주소 정수처럼 보인다는 사실은 정의 조건이 아니다.

**[compiler]** pointer provenance라는 용어의 세부 모델을 이 Step에서 확장하지 않고 C17의 명시된 array 조건을 따른다.

**[OS / CPU]** virtual address 숫자 차이나 ISA subtraction 가능 여부는 C pointer subtraction permission을 만들지 않는다.

**[alignment]** cast가 번역되었다고 결과 pointer의 정렬·대상 type·lifetime이 자동으로 유효해지지 않는다. alignment 세부는 이후 curriculum 범위를 따른다.
## 8. 자주 하는 실수
- unrelated object 주소를 빼서 portable byte distance를 얻는다.
- one-past를 넘어간 pointer도 역참조만 안 하면 된다고 생각한다.
- unrelated pointers에 `<`를 써서 전체 memory ordering을 만든다.
- pointer cast 성공을 dereference 안전성으로 해석한다.
## 9. 필수 실습
한 배열 안에서 begin/end 순회와 pointer difference를 사용한다.
[27-9 exercise](../../exercises/27-undefined-behavior/27-9/README.md)
## 10. 추가 실습
- ★ subrange의 길이를 계산한다.
- ★★ 빈 range에서 begin과 end가 같은 경우를 처리한다.
- ★★★ index loop와 pointer loop의 invariant를 비교한다.
## 11. 확인 문제
1. pointer arithmetic이 정의되는 object 범위는?
2. one-past pointer로 무엇을 할 수 있는가?
3. unrelated arrays의 pointer subtraction 분류는?
4. unrelated pointers의 relational comparison을 왜 피하는가?
5. raw address 계산이 C permission을 대신할 수 있는가?
## 12. 핵심 정리
- pointer arithmetic은 같은 array와 one-past 범위에 묶인다.
- subtraction과 ordering도 object 관계 조건을 확인한다.
- OS 주소·CPU arithmetic과 C17 pointer semantics를 분리한다.
## 13. 다음 Step
[27-10. compiler optimization과 UB](27-10-compiler-optimization-and-ub.md)
## 14. 참고 자료
- N1570 6.5.6p7-9, 6.5.8p5, 7.19 `ptrdiff_t`, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic)
