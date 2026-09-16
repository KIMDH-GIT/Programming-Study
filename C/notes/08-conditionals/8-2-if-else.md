# 8-2. `if-else`
`if-else`는 두 controlled statement 중 정확히 하나를 선택한다.
## 1. 학습 목표
- 참·거짓 경로를 구별한다.
- dangling else 규칙을 설명한다.
- 중괄호로 결합 의도를 명확히 한다.
## 2. 선수 지식
Step 8-1을 사용한다.
## 3. 핵심 개념
조건이 비0면 `if`, 0이면 `else` statement가 실행된다. `else`는 가장 가까운 unmatched `if`와 결합한다.
## 4. 문법
```c
if (condition) { a; } else { b; }
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int value=-2;if(value>=0){printf("nonnegative\n");}else{printf("negative\n");}return 0;}
```
## 6. 코드 해석
조건이 0이므로 else 경로만 실행된다.
## 7. 내부 동작
언어는 한 경로만 선택한다. 실제 branch 배치는 최적화에 따라 다르다.
## 8. 자주 하는 실수
- 두 경로가 모두 실행된다고 생각한다.
- 들여쓰기로 else 결합을 바꿀 수 있다고 생각한다.
- 경계 0을 빠뜨린다.
## 9. 필수 실습
0 이상과 음수 경로를 각각 관찰한다. [실습 README](../../exercises/08-conditionals/8-2/README.md).
## 10. 추가 실습
- ★ 0. - ★★ 음수. - ★★★ dangling else 재작성.
## 11. 확인 문제
1. else는 언제 실행되는가? 2. 몇 경로가 실행되는가? 3. dangling else 규칙은? 4. 중괄호 장점은?
## 12. 핵심 정리
`if-else`는 조건값에 따라 두 statement 중 하나를 실행한다.
## 13. 다음 Step
[Step 8-3. `else if`](8-3-else-if.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.1.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
- [GCC misleading indentation](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
