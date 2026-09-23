# 21-14. `set`, `clear`, `toggle`, `read`
## 1. 학습 목표
- bit operations를 작은 함수로 캡슐화한다.
- position boundary를 모든 함수에서 검사한다.
- 성공·실패 contract를 명시한다.
## 2. 선수 지식
21-7~13의 masks와 GPIO object를 안다.
## 3. 핵심 개념
함수는 pointer value를 pass-by-value로 받고 유효 pointer가 가리키는 object를 수정한다. position은 8보다 작아야 한다.
## 4. 문법
```c
int set_bit(uint8_t *value, unsigned int position);
int read_bit(uint8_t value, unsigned int position, unsigned int *out);
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
static int set_bit(uint8_t *v, unsigned int p)
{
    if (p >= 8u) return 0;
    *v = (uint8_t)(*v | (uint8_t)(UINT8_C(1) << p));
    return 1;
}
static int read_bit(uint8_t v, unsigned int p, unsigned int *out)
{
    if (p >= 8u || out == NULL) return 0;
    *out = (unsigned int)((v & (uint8_t)(UINT8_C(1) << p)) != 0u);
    return 1;
}
int main(void)
{
    uint8_t gpio = 0u;
    unsigned int state;
    if (!set_bit(&gpio, 3u)) return 1;
    if (!read_bit(gpio, 3u, &state)) return 1;
    printf("%u\n", state);
    return 0;
}
```
## 6. 코드 해석
position과 output pointer를 검사한 뒤 caller object의 pin 3을 set한다. read 함수는 성공 여부를 반환하고 bit value는 output parameter에 저장하므로 invalid input과 valid zero가 구분된다.
## 7. 내부 동작
**[C17 표준]** pointer 자체와 uint8_t value는 pass-by-value다. caller가 바뀌는 이유는 유효 pointer dereference다. count guard가 shift UB를 막는다.
## 8. 자주 하는 실수
- guard 뒤가 아니라 shift 뒤에 position을 검사한다.
- pointer parameter를 call by reference라고 부른다.
- invalid read와 clear bit를 같은 return value로 모호하게 만든다.
## 9. 필수 실습
set, clear, toggle, read 네 함수를 같은 boundary policy로 작성한다.
[21-14 exercise](../../exercises/21-bitwise-operators/21-14/README.md)
## 10. 추가 실습
- ★ 네 functions를 연속 호출한다.
- ★★ invalid positions를 확인한다.
- ★★★ output pointer가 NULL인 경로를 확인한다.
## 11. 확인 문제
1. position guard는 언제 수행하는가?
2. caller object가 바뀌는 이유는?
3. pointer 전달도 pass-by-value인가?
4. status return과 output parameter를 분리하는 이유는?
## 12. 핵심 정리
- boundary 검사 후 mask를 만든다.
- 작은 functions로 operations를 일관되게 만든다.
- pointer와 pointee 변경을 구분한다.
## 13. 다음 Step
[21-15. Part 21 종합 복습](21-15-part-21-review.md)
## 14. 참고 자료
- N1570 6.5.7, 6.5.10~12, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [SEI CERT INT34-C](https://wiki.sei.cmu.edu/confluence/display/c/INT34-C.+Do+not+shift+an+expression+by+a+negative+number+of+bits+or+by+greater+than+or+equal+to+the+number+of+bits+that+exist+in+the+operand)
