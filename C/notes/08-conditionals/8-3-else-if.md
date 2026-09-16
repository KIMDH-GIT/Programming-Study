# 8-3. `else if`
`else if`는 하나의 키워드가 아니라 `else` 뒤에 또 다른 `if` statement가 오는 구조다.
## 1. 학습 목표
- 다중 분기 순서를 설명한다.
- 첫 참 조건만 선택됨을 설명한다.
- 최종 else의 역할을 말한다.
## 2. 선수 지식
if-else를 사용한다.
## 3. 핵심 개념
조건은 위에서 아래로 평가되고 처음 참인 경로 뒤 나머지는 평가되지 않는다. 범위는 겹치지 않게 설계한다.
## 4. 문법
```c
if(a){}else if(b){}else{}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int score=85;if(score>=90){printf("A\n");}else if(score>=80){printf("B\n");}else{printf("C\n");}return 0;}
```
## 6. 코드 해석
첫 조건은 거짓, 둘째는 참이므로 B만 출력된다.
## 7. 내부 동작
short-circuit처럼 선택된 이후 조건은 평가되지 않는다.
## 8. 자주 하는 실수
- 낮은 기준을 먼저 둔다.
- 여러 경로가 실행된다고 생각한다.
- `else if`를 별도 토큰으로 설명한다.
## 9. 필수 실습
세 구간 분기를 작성한다. [실습 README](../../exercises/08-conditionals/8-3/README.md).
## 10. 추가 실습
- ★ 경계 90. - ★★ 경계 80. - ★★★ 조건 순서 바꾸기 분석.
## 11. 확인 문제
1. else if는 별도 키워드인가? 2. 조건 순서는? 3. 처음 참 뒤 평가되는가? 4. 최종 else 역할은?
## 12. 핵심 정리
다중 분기는 순서대로 첫 참 경로 하나를 선택한다.
## 13. 다음 Step
[Step 8-4. 중첩 조건문과 경계값](8-4-nested-boundaries.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.1.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
- [GCC warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
