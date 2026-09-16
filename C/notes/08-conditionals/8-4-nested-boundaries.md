# 8-4. 중첩 조건문과 경계값
조건문 안에 조건문을 둘 수 있지만 경계와 포함 관계를 명시해야 한다.
## 1. 학습 목표
- 중첩 경로를 추적한다.
- inclusive/exclusive 경계를 구별한다.
- 중괄호로 구조를 명확히 한다.
## 2. 선수 지식
비교·논리 연산과 if-else를 사용한다.
## 3. 핵심 개념
바깥 조건이 참이어야 안쪽 조건을 평가한다. 경계값 바로 아래·같음·바로 위를 표로 시험한다.
## 4. 문법
```c
if(outer){if(inner){}else{}}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void){int value=10;if(value>=0){if(value<=10){printf("0..10\n");}else{printf("over 10\n");}}else{printf("negative\n");}return 0;}
```
## 6. 코드 해석
10은 두 inclusive 경계를 모두 만족한다.
## 7. 내부 동작
조건 경로는 C 의미이며 컴파일러는 동등한 제어 흐름으로 최적화할 수 있다.
## 8. 자주 하는 실수
- `<`와 `<=`를 혼동한다.
- 바깥 거짓인데 안쪽을 평가한다고 생각한다.
- dangling else 구조를 방치한다.
## 9. 필수 실습
-1, 0, 10, 11 경계를 표로 확인한다. [실습 README](../../exercises/08-conditionals/8-4/README.md).
## 10. 추가 실습
- ★ 0. - ★★ 10. - ★★★ 평면화 가능성 분석.
## 11. 확인 문제
1. 안쪽 조건 평가 전제는? 2. 10은 포함되는가? 3. `<`와 `<=` 차이는? 4. 중괄호 역할은?
## 12. 핵심 정리
중첩 조건은 바깥 전제와 각 경계의 포함 여부를 함께 추적한다.
## 13. 다음 Step
[Step 8-5. `switch`, `case`, `break`, `default`](8-5-switch.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.4.
- [cppreference: if](https://en.cppreference.com/w/c/language/if.html)
- [GCC warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
