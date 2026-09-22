# 20-4. `switch` 기반 state machine

state machine은 현재 상태와 event에 따라 다음 상태를 결정한다. enum과 `switch`는 가능한 경우를 이름으로 표현한다.

## 1. 학습 목표
- state transition을 함수로 구현한다.
- `switch`의 `case`, `break`, `default`를 정확히 사용한다.
- enum 값의 전제를 함수 contract로 명시한다.

## 2. 선수 지식
Part 8의 switch와 20-3의 enum state를 안다.

## 3. 핵심 개념
transition 함수는 현재 state value를 받아 다음 state value를 반환한다. C17의 pass-by-value 원칙은 enum에도 동일하다.

## 4. 문법
```c
switch (state) {
case STATE_IDLE:
    return STATE_RUNNING;
case STATE_RUNNING:
    return STATE_DONE;
default:
    return STATE_ERROR;
}
```
`default`는 외부 입력이나 손상된 상태를 방어하는 contract에 유용하다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

enum State { STATE_IDLE, STATE_RUNNING, STATE_DONE, STATE_ERROR };

static enum State advance(enum State state)
{
    switch (state) {
    case STATE_IDLE: return STATE_RUNNING;
    case STATE_RUNNING: return STATE_DONE;
    case STATE_DONE: return STATE_DONE;
    case STATE_ERROR: return STATE_ERROR;
    default: return STATE_ERROR;
    }
}

int main(void)
{
    enum State state = STATE_IDLE;
    state = advance(state);
    state = advance(state);
    printf("%d\n", state == STATE_DONE);
    return 0;
}
```

## 6. 코드 해석
IDLE→RUNNING→DONE으로 전이한다. 반환 enum value를 caller object에 대입해야 caller state가 바뀐다.

## 7. 내부 동작
- **[C17 표준]** switch controlling expression에는 integer promotion이 적용되며 case labels는 integer constant expressions다.
- 같은 값의 enumerators를 같은 switch의 서로 다른 cases로 쓰면 duplicate case constraint violation이다.
- compiler warning은 case 누락을 도울 수 있어도 transition correctness를 보장하지 않는다.

## 8. 자주 하는 실수
- case 뒤 `break`/`return`을 빠뜨려 의도치 않게 fallthrough한다.
- 모든 enum object가 named enumerator 중 하나라고 무조건 가정한다.
- `default`가 있으면 state model이 자동 검증된다고 생각한다.
- state와 event를 한 숫자로 뒤섞는다.

## 9. 필수 실습
IDLE, RUNNING, DONE, ERROR transition 함수를 만들고 정상 경로를 출력한다.
[20-4 exercise](../../exercises/20-enum-typedef-union/20-4/README.md)

## 10. 추가 실습
- ★ ERROR 상태를 흡수 상태로 만든다.
- ★★ event enum을 추가한다.
- ★★★ transition table과 코드를 대조한다.

## 11. 확인 문제
1. enum parameter는 어떻게 전달되는가?
2. case label에 필요한 expression 종류는?
3. duplicate enum value를 cases에 함께 쓸 수 있는가?
4. `default` 필요성은 무엇에 따라 결정되는가?
5. caller state는 언제 바뀌는가?

## 12. 핵심 정리
- enum 이름으로 상태를 표현하고 switch로 transition을 분기한다.
- invalid state 정책을 contract로 정한다.
- 상태 변경은 반환값 대입 또는 유효 pointer 수정으로 명시한다.

## 13. 다음 Step
[20-5. `union`의 공유 저장 공간](20-5-union-shared-storage.md)

## 14. 참고 자료
- N1570 6.5.4, 6.8.4.2. N1570은 **C11 공개 Committee Draft**이며 관련 switch 규칙은 C17에서도 유지된다.
- [cppreference: switch statement](https://en.cppreference.com/w/c/language/switch)
