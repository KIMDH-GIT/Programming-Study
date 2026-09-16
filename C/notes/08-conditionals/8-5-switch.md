# 8-5. `switch`, `case`, `break`, `default`
`switch`는 정수형 controlling expression의 값과 case 정수 상수를 비교해 label 이후를 실행한다.
## 1. 학습 목표
- switch 구성요소를 구별한다.
- break와 fallthrough를 설명한다.
- default의 역할을 말한다.
## 2. 선수 지식
integer promotion과 assignment를 사용한다.
## 3. 핵심 개념
controlling expression에는 integer promotions가 적용된다. 일치 case부터 실행하며 `break`가 없으면 다음 label 뒤 statement로 계속될 수 있다. `break`는 가장 가까운 switch를 끝낸다. default는 일치 case가 없을 때 진입점이다.
## 4. 문법
```c
switch(value){case 1: statement; break; default: statement;}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int menu=2;switch(menu){case 1:printf("one\n");break;case 2:printf("two\n");break;default:printf("other\n");}return 0;}
```
## 6. 코드 해석
case 2가 일치하고 break로 switch를 끝낸다.
## 7. 내부 동작
컴파일러는 비교 분기나 jump table 등으로 구현할 수 있다.
## 8. 자주 하는 실수
- case가 자동 종료된다고 생각한다.
- case에 임의 실행식 값을 쓴다.
- break가 프로그램 전체를 끝낸다고 생각한다.
## 9. 필수 실습
세 메뉴 값을 분류한다. [실습 README](../../exercises/08-conditionals/8-5/README.md).
## 10. 추가 실습
- ★ default. - ★★ 의도된 fallthrough 분석. - ★★★ break 누락 찾기.
## 11. 확인 문제
1. controlling expression 형 범위는? 2. break 효과는? 3. fallthrough란? 4. default는 언제?
## 12. 핵심 정리
switch는 label 진입 후 break까지 실행되므로 fallthrough를 의도적으로 관리한다.
## 13. 다음 Step
[Step 8-6. 홀짝과 양수·음수·0 판별](8-6-parity-sign.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.2, 6.8.6.3.
- [cppreference: switch](https://en.cppreference.com/w/c/language/switch.html)
- [GCC implicit fallthrough](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
