# 8-6. 홀짝과 양수·음수·0 판별
두 독립 분류를 안전한 조건식으로 연결한다.
## 1. 학습 목표
- `% 2`로 홀짝을 판별한다.
- 세 부호 구간을 완전하게 나눈다.
## 2. 선수 지식
나머지와 if-else를 사용한다.
## 3. 핵심 개념
`value % 2 == 0`이면 짝수다. 부호는 `>0`, `<0`, 나머지 0으로 나눈다.
## 4. 문법
```c
if(value%2==0){}else{}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int v=-7;if(v%2==0){printf("even ");}else{printf("odd ");}if(v>0){printf("positive\n");}else if(v<0){printf("negative\n");}else{printf("zero\n");}return 0;}
```
## 6. 코드 해석
-7은 홀수이자 음수다.
## 7. 내부 동작
나머지와 비교 규칙이 먼저 적용된다.
## 8. 자주 하는 실수
음수 나머지를 무시하거나 0 구간을 빠뜨린다.
## 9. 필수 실습
세 부호와 홀짝 표를 만든다. [실습 README](../../exercises/08-conditionals/8-6/README.md).
## 10. 추가 실습
- ★ 0. - ★★ 음수 짝수. - ★★★ 경계표.
## 11. 확인 문제
1. 짝수 조건은? 2. -7은 홀수인가? 3. 0의 부호 분류는? 4. 두 분류는 독립인가?
## 12. 핵심 정리
홀짝은 나머지, 부호는 상호 배타적인 세 구간으로 판별한다.
## 13. 다음 Step
[Step 8-7. 세 수의 최댓값](8-7-maximum-of-three.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5, 6.8.4.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
