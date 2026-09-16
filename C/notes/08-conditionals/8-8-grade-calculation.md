# 8-8. 학점 계산
점수 구간은 높은 기준부터 검사해 겹침을 제거한다.
## 1. 학습 목표
- 학점 경계를 설계한다.
- 조건 순서의 중요성을 설명한다.
## 2. 선수 지식
else-if chain을 사용한다.
## 3. 핵심 개념
90 이상 A, 80 이상 B처럼 높은 경계부터 검사한다. 입력 유효성 범위는 별도 계약이다.
## 4. 문법
```c
if(s>=90){}else if(s>=80){}else{}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int s=92;char g;if(s>=90){g='A';}else if(s>=80){g='B';}else if(s>=70){g='C';}else{g='F';}printf("%c\n",g);return 0;}
```
## 6. 코드 해석
첫 조건이 참이라 A가 저장된다.
## 7. 내부 동작
첫 참 이후 조건은 평가되지 않는다.
## 8. 자주 하는 실수
낮은 기준부터 검사하거나 경계값을 누락한다.
## 9. 필수 실습
89,90,79,80 경계를 기록한다. [실습 README](../../exercises/08-conditionals/8-8/README.md).
## 10. 추가 실습
- ★ 90. - ★★ 80. - ★★★ 유효범위 설계.
## 11. 확인 문제
1. 왜 높은 기준부터인가? 2. 90의 학점은? 3. 89는? 4. 유효성은 별도인가?
## 12. 핵심 정리
구간 분류는 경계와 검사 순서를 표로 먼저 설계한다.
## 13. 다음 Step
[Step 8-9. 메뉴 기반 계산기](8-9-menu-calculator.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.1.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
