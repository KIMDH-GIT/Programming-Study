# 8-9. 메뉴 기반 계산기
메뉴 값은 switch로 연산 하나를 선택한다.
## 1. 학습 목표
- 메뉴와 case를 대응한다.
- 나눗셈 안전 조건을 적용한다.
## 2. 선수 지식
switch와 산술 연산을 사용한다.
## 3. 핵심 개념
각 case는 한 연산만 수행하고 break한다. 나눗셈 case는 0 제수를 별도 if로 검사한다.
## 4. 문법
```c
switch(menu){case 1:...;break;default:...;}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int menu=1,a=8,b=2;switch(menu){case 1:printf("%d\n",a+b);break;case 2:printf("%d\n",a-b);break;case 3:printf("%d\n",a*b);break;case 4:if(b!=0){printf("%d\n",a/b);}else{printf("zero divisor\n");}break;default:printf("invalid\n");}return 0;}
```
## 6. 코드 해석
menu 1은 합 10을 출력한다.
## 7. 내부 동작
switch 구현 방식은 컴파일러 선택이다.
## 8. 자주 하는 실수
break 누락, 0 제수, default 누락.
## 9. 필수 실습
네 메뉴와 잘못된 메뉴를 설계한다. [실습 README](../../exercises/08-conditionals/8-9/README.md).
## 10. 추가 실습
- ★ 덧셈. - ★★ 나눗셈 guard. - ★★★ fallthrough 점검.
## 11. 확인 문제
1. break 목적은? 2. default 목적은? 3. 나눗셈 전 조건은? 4. menu 1 결과는?
## 12. 핵심 정리
switch로 메뉴를 분리하고 위험 연산은 case 안에서 검증한다.
## 13. 다음 Step
[Step 8-10. Part 8 종합 복습](8-10-part-8-review.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.
- [cppreference: switch](https://en.cppreference.com/w/c/language/switch.html)
