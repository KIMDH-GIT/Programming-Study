# 19-7. 구조체를 함수에 전달
## 1. 학습 목표
- structure value parameter와 pointer parameter를 구분한다.
- structure value를 반환한다.
## 2. 선수 지식
Part 10·16의 pass-by-value와 19-5를 안다.
## 3. 핵심 개념
C 함수 인수는 모두 value로 전달된다. structure argument는 parameter object에 structure value가 전달되고, pointer argument는 pointer value가 전달된다.
## 4. 문법
```c
void print_point(struct Point p);
void move(struct Point *p);
struct Point make_point(int x, int y);
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
struct Point { int x; int y; };
static struct Point change_copy(struct Point p) { p.x = 100; return p; }
static void move(struct Point *p) { p->x += 1; }
static struct Point make_point(int x, int y)
{
    struct Point p = {x, y};
    return p;
}
int main(void)
{
    struct Point p = make_point(10, 20);
    struct Point changed = change_copy(p);
    move(&p);
    printf("original=%d changed=%d\n", p.x, changed.x);
    return 0;
}
```
## 6. 코드 해석
`change_copy`는 parameter object를 바꾼 뒤 별도 value로 반환하므로 caller의 원래 `p`는 직접 바뀌지 않는다. `move`도 pointer value를 받지만 dereference를 통해 caller의 `p.x`를 바꾼다. local structure value 반환은 유효하며 local object 주소 반환과 다르다.
## 7. 내부 동작
**[C17 표준]** semantics는 pass-by-value다. **[ABI/compiler]** register, memory, hidden mechanism을 사용할 수 있으나 자동 reference 전달이나 항상 stack 전체 복사를 뜻하지 않는다.
## 8. 자주 하는 실수
- 큰 구조체는 C가 자동 reference로 전달한다고 말한다.
- pointer parameter를 call by reference라고 부른다.
- local structure value 반환과 `&local` 반환을 혼동한다.
## 9. 필수 실습
value parameter 출력 함수, pointer parameter 수정 함수, structure 반환 함수를 작성한다.
[19-7 exercise](../../exercises/19-structures/19-7/README.md)
## 10. 추가 실습
- ★ `const struct Point *` 출력 함수를 만든다.
- ★★ 두 Point의 합을 반환한다.
- ★★★ value/pointer parameter의 효과를 표로 쓴다.
## 11. 확인 문제
1. structure argument 전달 방식은?
2. value parameter 수정이 caller를 바꾸는가?
3. pointer parameter가 caller를 바꾸는 이유는?
4. local structure value 반환은 유효한가?
5. ABI 구현을 C semantics로 단정할 수 있는가?
## 12. 핵심 정리
- structure와 pointer 모두 value로 전달된다.
- caller 수정은 유효 pointer를 통해 대상 member를 수정할 때 일어난다.
- structure value는 반환할 수 있다.
## 13. 다음 Step
[19-8. `strtol`로 정수 입력 검증](19-8-strtol-integer-input-validation.md)
## 14. 참고 자료
- N1570 6.5.2.2, 6.8.6.4. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: function call](https://en.cppreference.com/w/c/language/operator_other)
