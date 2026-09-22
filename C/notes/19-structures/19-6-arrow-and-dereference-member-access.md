# 19-6. `p->id`와 `(*p).id`
## 1. 학습 목표
- `->`와 `(*p).member`의 관계를 설명한다.
- 괄호가 필요한 precedence 이유를 안다.
## 2. 선수 지식
역참조 `*`, `.`와 `->`를 안다.
## 3. 핵심 개념
`p`가 structure pointer라면 `*p`는 pointed-to structure object이고 `(*p).id`는 그 `id` member다. `p->id`는 같은 member access를 간결하게 표현한다.
## 4. 문법
```c
(*p).id = 1;
p->id = 1;
```
`.`가 unary `*`보다 우선하므로 `*p.id`가 아니라 `(*p).id`여야 한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
struct Student { int id; double score; };
int main(void)
{
    struct Student s = {1, 90.0};
    struct Student *p = &s;
    (*p).score += 5.0;
    printf("%d %.1f\n", p->id, p->score);
    return 0;
}
```
## 6. 코드 해석
역참조 뒤 member를 선택한 결과와 arrow 표현은 같은 member를 지정한다.
## 7. 내부 동작
**[C17 표준]** 두 표기는 같은 pointed-to member를 지정한다. 평가에 필요한 유효 pointer 조건도 같다. **[compiler]** source spelling에 별도 machine instruction을 요구하지 않는다.
## 8. 자주 하는 실수
- `*p.id`를 `(*p).id`로 오해한다.
- `->`가 pointer에서 주소를 “꺼내는” 별도 메모리 명령이라고 생각한다.
- 두 표기의 null pointer 위험이 다르다고 생각한다.
## 9. 필수 실습
같은 object를 두 표기로 읽고 한 표기로 수정한다.
[19-6 exercise](../../exercises/19-structures/19-6/README.md)
## 10. 추가 실습
- ★ 두 표기를 번갈아 쓴다.
- ★★ precedence를 괄호로 표시한다.
- ★★★ array element pointer에 적용한다.
## 11. 확인 문제
1. `p->id`의 논리적 세 단계는?
2. `(*p).id`에 괄호가 필요한 이유는?
3. 두 표기의 pointer 유효성 조건은 같은가?
4. `*p.id`는 무엇을 먼저 선택하는가?
## 12. 핵심 정리
- `p->m`과 `(*p).m`은 같은 member access다.
- 역참조 결과가 structure object다.
- precedence 때문에 괄호가 필요하다.
## 13. 다음 Step
[19-7. 구조체를 함수에 전달](19-7-passing-structures-to-functions.md)
## 14. 참고 자료
- N1570 6.5.2.3, Annex A. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: operator precedence](https://en.cppreference.com/w/c/language/operator_precedence)
