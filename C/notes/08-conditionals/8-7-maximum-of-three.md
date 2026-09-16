# 8-7. 세 수의 최댓값
현재 최댓값 객체를 조건에 따라 갱신한다.
## 1. 학습 목표
- 세 값을 빠짐없이 비교한다.
- 동률에서도 올바른 최댓값을 유지한다.
## 2. 선수 지식
비교와 대입을 사용한다.
## 3. 핵심 개념
첫 값을 초기 최댓값으로 두고 나머지 두 값이 더 클 때 대입한다.
## 4. 문법
```c
if(b>max){max=b;}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int a=7,b=12,c=9;int max=a;if(b>max){max=b;}if(c>max){max=c;}printf("%d\n",max);return 0;}
```
## 6. 코드 해석
12가 최댓값으로 남는다.
## 7. 내부 동작
각 if는 독립적으로 현재 max와 비교한다.
## 8. 자주 하는 실수
세 쌍을 복잡하게 중첩하거나 동률을 빠뜨린다.
## 9. 필수 실습
순서와 동률 사례를 시험한다. [실습 README](../../exercises/08-conditionals/8-7/README.md).
## 10. 추가 실습
- ★ 순서 변경. - ★★ 동률. - ★★★ 음수.
## 11. 확인 문제
1. 초기 max는? 2. 왜 두 if가 독립인가? 3. 동률은 안전한가? 4. 모두 음수여도 되는가?
## 12. 핵심 정리
현재 최댓값을 유지하며 각 후보를 한 번씩 비교한다.
## 13. 다음 Step
[Step 8-8. 학점 계산](8-8-grade-calculation.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.1.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
