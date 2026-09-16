# 8-10. Part 8 종합 복습
조건문은 Part 7의 0/비0 식을 이용해 controlled statement를 선택한다.
## 1. 학습 목표
- if·else-if·switch를 선택한다.
- 경계와 fallthrough를 검토한다.
## 2. 선수 지식
Step 8-1~8-9 전체다.
## 3. 핵심 개념
범위 조건은 if chain, 이산 메뉴는 switch가 자연스럽다. assignment 조건은 유효한 식이므로 `=`와 `==`를 검토한다.
## 4. 문법
```c
if(c){}else{} switch(v){case 1:break;default:break;}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int v=0;if(v){printf("nonzero\n");}else{printf("zero\n");}switch(v){case 0:printf("case zero\n");break;default:printf("other\n");}return 0;}
```
## 6. 코드 해석
zero와 case zero가 출력된다.
## 7. 내부 동작
동등한 제어 흐름으로 최적화될 수 있다.
## 8. 자주 하는 실수
경계 누락, 대입 조건, break 누락.
## 9. 필수 실습
조건문 선택표와 안전 예제를 작성한다. [실습 README](../../exercises/08-conditionals/8-10/README.md).
## 10. 추가 실습
- ★ 진릿값. - ★★ 경계표. - ★★★ switch 점검.
## 11. 확인 문제
1. 0은? 2. else 결합 규칙은? 3. switch fallthrough란? 4. `if(x=5)`는 syntax error인가? 5. break 효과는?
## 12. 핵심 정리
조건식, 경계, statement 결합, break를 함께 검토한다.
## 13. 다음 Step
Step 9-1. 반복의 초기화·조건·변화
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.
- [cppreference: statements](https://en.cppreference.com/w/c/language/statements.html)
