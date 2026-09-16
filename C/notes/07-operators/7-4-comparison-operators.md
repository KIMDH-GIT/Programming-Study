# 7-4. `==`, `!=`, `<`, `>`, `<=`, `>=`

비교 연산자는 두 피연산자를 변환·비교하고 C17의 `int` 값 0 또는 1을 만든다.
## 1. 학습 목표
- equality와 relational 연산자를 구별한다.
- 결과형이 `int`임을 설명한다.
- `=`와 `==`를 구별한다.
## 2. 선수 지식
Part 6 usual arithmetic conversions를 사용한다.
## 3. 핵심 개념
`==`, `!=`는 같음, `<`, `>`, `<=`, `>=`는 순서를 비교한다. 산술 피연산자는 usual arithmetic conversions를 거친다. 참이면 `int` 1, 거짓이면 `int` 0이다. C17의 별도 `bool` 결과형이라고 설명하지 않는다.
## 4. 문법
```c
int equal = left == right;
int less = left < right;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int left = 3;
    double right = 3.0;
    printf("%d %d %d\n", left == right, left != right, left < 4);
    return 0;
}
```
## 6. 코드 해석
비교 전 `left`가 `double`로 변환된다. 결과는 1, 0, 1이다.
## 7. 내부 동작
비교 결과는 C의 `int` 값이다. CPU flag나 명령 형태는 구현 세부다.
## 8. 자주 하는 실수
- `=`와 `==`를 혼동한다.
- 결과형을 항상 `bool`이라고 부른다.
- signed·unsigned 변환을 무시한다.
## 9. 필수 실습
같음·다름·순서 비교를 정수로 출력한다. [실습 README](../../exercises/07-operators/7-4/README.md).
## 10. 추가 실습
- ★ 기초: 같은 정수.
- ★★ 응용: int와 double.
- ★★★ 도전: signed/unsigned 비교 분석.
## 11. 확인 문제
1. 비교 결과형은?
2. 참과 거짓 값은?
3. `=`와 `==`의 차이는?
4. 혼합형 비교 전에 무엇이 일어나는가?
## 12. 핵심 정리
비교 연산은 변환 뒤 수행되고 `int` 0 또는 1을 결과로 만든다.
## 13. 다음 Step
[Step 7-5. `&&`, `||`, `!`와 short-circuit](7-5-logical-short-circuit.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.8~9.
- [cppreference: comparison operators](https://en.cppreference.com/w/c/language/operator_comparison.html)
- [cppreference: conversions](https://en.cppreference.com/w/c/language/conversion.html)
