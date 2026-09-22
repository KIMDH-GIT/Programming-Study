# 19-5. 구조체 포인터와 `->`
## 1. 학습 목표
- structure object와 그 주소를 구분한다.
- pointer로 member를 읽고 수정한다.
## 2. 선수 지식
Part 14 포인터와 19-2의 `.`를 안다.
## 3. 핵심 개념
`struct Point *ptr = &p;`에서 `ptr`은 `p` 자체가 아니라 `p`를 가리키는 pointer object다. `ptr->x`는 pointed-to structure의 `x` member를 선택한다.
## 4. 문법
```c
struct Point p = {10, 20};
struct Point *ptr = &p;
ptr->x = 30;
```
object에는 `object.member`, pointer에는 `pointer->member`를 쓴다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
struct Point { int x; int y; };
int main(void)
{
    struct Point p = {10, 20};
    struct Point *ptr = &p;
    ptr->x += 5;
    printf("%d %d\n", p.x, ptr->y);
    return 0;
}
```
## 6. 코드 해석
`ptr`이 유효한 `p`를 가리키므로 `ptr->x` 수정은 `p.x` 수정이다.
## 7. 내부 동작
**[C17 표준]** `->`의 왼쪽은 structure/union을 가리키는 pointer여야 한다. 유효한 lifetime과 alignment를 가진 object를 가리켜야 한다. **[compiler/CPU]** 주소 계산과 load/store 구현은 target에 따라 다르며 `->`가 별도 CPU 명령이라는 보장은 없다.
## 8. 자주 하는 실수
- `ptr.x` 또는 `p->x`를 쓴다.
- null·dangling pointer를 역참조한다.
- pointer value 전달을 call by reference라고 부른다.
## 9. 필수 실습
Point object의 주소를 pointer에 저장하고 `->`로 두 member를 수정한다.
[19-5 exercise](../../exercises/19-structures/19-5/README.md)
## 10. 추가 실습
- ★ `const struct Point *`로 읽기만 한다.
- ★★ pointer로 구조체 배열을 순회한다.
- ★★★ 유효 lifetime 조건을 표로 정리한다.
## 11. 확인 문제
1. `ptr`과 `*ptr`은 각각 무엇인가?
2. object와 pointer의 member operator는?
3. `ptr->x`가 caller object를 바꾸는 조건은?
4. null pointer에 `->`를 적용할 수 있는가?
## 12. 핵심 정리
- pointer는 structure object의 주소 값을 저장한다.
- `->`는 pointed-to structure의 member access다.
- 유효 pointer와 lifetime이 선행 조건이다.
## 13. 다음 Step
[19-6. `p->id`와 `(*p).id`](19-6-arrow-and-dereference-member-access.md)
## 14. 참고 자료
- N1570 6.5.2.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: member access](https://en.cppreference.com/w/c/language/operator_member_access)
