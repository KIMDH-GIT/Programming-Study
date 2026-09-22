# 20-3. enum 기반 state 표현

enum은 서로 다른 상태에 의미 있는 이름을 주어 magic number보다 명확한 상태 모델을 만든다.

## 1. 학습 목표
- state object와 state constants를 구분한다.
- enum을 structure member와 함수 parameter로 사용한다.
- 외부 정수를 state로 받아들일 때 validation한다.

## 2. 선수 지식
20-1 enum과 Part 19 구조체·함수 전달을 안다.

## 3. 핵심 개념
```c
enum StudentState { STATE_ACTIVE, STATE_INACTIVE, STATE_ERROR };
```
enum은 이름과 type을 제공하지만 runtime validation을 자동으로 추가하지 않는다. 외부 입력은 허용된 상태인지 검사한 뒤 저장한다.

## 4. 문법
```c
struct Student {
    int id;
    enum StudentState state;
};
```
enum parameter도 일반 C 규칙대로 pass-by-value다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

enum StudentState { STATE_ACTIVE, STATE_INACTIVE, STATE_ERROR };
struct Student { int id; enum StudentState state; };

static int is_valid_state(int value)
{
    return value == STATE_ACTIVE
        || value == STATE_INACTIVE
        || value == STATE_ERROR;
}

int main(void)
{
    int input = 1;
    if (!is_valid_state(input)) return 1;
    struct Student student = {7, (enum StudentState)input};
    printf("%d %d\n", student.id, (int)student.state);
    return 0;
}
```

## 6. 코드 해석
정수 입력을 알려진 enumerator values와 비교한 뒤 enum type으로 변환해 structure member에 저장한다. cast 하나만으로 semantic validation이 끝나는 것은 아니다.

## 7. 내부 동작
- **[C17 표준]** enum object는 compatible integer type의 value representation과 range를 따른다. 정수→enum conversion 결과는 그 compatible type으로의 conversion 규칙을 따른다.
- 이름 붙은 enumerators가 해당 type의 모든 표현 가능한 값을 열거한다고 보장하지 않는다.
- **[compiler]** range 정보로 diagnostics나 optimization을 수행할 수 있으나 application validation을 대신하지 않는다.

## 8. 자주 하는 실수
- enum을 쓰면 잘못된 외부 값이 자동 거부된다고 생각한다.
- cast 결과가 반드시 named enumerator가 된다고 생각한다.
- enum 값을 hardware protocol encoding과 자동으로 동일시한다.
- `STATE_COUNT` 같은 sentinel이 C의 특별 기능이라고 생각한다.

## 9. 필수 실습
학생 상태 enum을 structure member로 사용하고 정수 입력 후보를 검사한 뒤 저장한다.
[20-3 exercise](../../exercises/20-enum-typedef-union/20-3/README.md)

## 10. 추가 실습
- ★ 상태를 출력하는 함수를 만든다.
- ★★ invalid input 경로를 확인한다.
- ★★★ state transition 표를 만든다.

## 11. 확인 문제
1. enum이 runtime validation을 자동 제공하는가?
2. integer cast와 enumerator validation 차이는?
3. enum parameter 전달 방식은?
4. sentinel enumerator는 어떤 종류의 규약인가?
5. hardware encoding과 enum representation은 같은가?

## 12. 핵심 정리
- enum은 상태 이름과 type을 제공한다.
- 외부 값은 named state 집합과 별도로 검증한다.
- structure member와 parameter에도 일반 type처럼 사용한다.

## 13. 다음 Step
[20-4. `switch` 기반 state machine](20-4-switch-based-state-machine.md)

## 14. 참고 자료
- N1570 6.3.1.3, 6.7.2.2. N1570은 **C11 공개 Committee Draft**이며 관련 conversion 규칙은 C17에서도 유지된다.
- [cppreference: enum declaration](https://en.cppreference.com/w/c/language/enum)
- [SEI CERT INT50-CPP](https://wiki.sei.cmu.edu/confluence/display/cplusplus/INT50-CPP.+Do+not+cast+to+an+out-of-range+enumeration+value) — C++ 자료이므로 C17 규칙과 혼동하지 않고 경계 validation 관점만 참고한다.
