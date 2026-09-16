# 9-5. `break`와 `continue`

`break`는 현재 반복문을 끝내고, `continue`는 현재 반복의 남은 본문만 건너뛴다.

## 1. 학습 목표
- 두 jump statement의 목적지를 구분한다.
- 반복문 종류별 `continue`의 다음 단계를 설명한다.
- `switch`의 `break`와 반복문의 `break`를 구별한다.

## 2. 선수 지식
Part 8의 `switch`와 Step 9-2~9-4의 세 반복 구조를 안다.

## 3. 핵심 개념
`break`는 자신을 직접 감싼 가장 안쪽 `while`, `do-while`, `for`, `switch` 하나만 종료한다. `continue`는 반복문에서만 사용하며 현재 본문의 나머지를 건너뛴다. 그 다음 이동은 `while`에서는 조건, `do-while`에서는 아래쪽 조건, `for`에서는 iteration expression이다.

## 4. 문법
```c
if (stop_condition) {
    break;
}
if (skip_condition) {
    continue;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int i = 1; i <= 6; ++i) {
        if (i == 3) {
            continue;
        }
        if (i == 6) {
            break;
        }
        printf("%d\n", i);
    }
    return 0;
}
```

## 6. 코드 해석
1과 2는 출력된다. 3에서는 나머지 본문을 건너뛰지만 `for`의 `++i`는 실행된다. 4와 5는 출력되고 6에서는 반복문 자체를 끝내므로 6은 출력되지 않는다.

## 7. 내부 동작
[C 언어 관점] jump statement는 정해진 목적지로 제어를 옮긴다. `while`에서 변화 코드보다 앞에 `continue`가 있으면 변화가 건너뛰어져 무한 반복이 될 수 있다. `for`는 `continue` 뒤에도 iteration expression을 거치므로 같은 코드라도 흐름이 다르다.

## 8. 자주 하는 실수
- `continue`가 반복문을 종료한다고 생각한다.
- 중첩 반복에서 `break`가 바깥 반복까지 끝낸다고 생각한다.
- `while`의 변화 전에 `continue`해 상태가 고정된다.
- `switch` 안의 `break`가 바깥 반복도 끝낸다고 생각한다.

## 9. 필수 실습
1~10 중 3의 배수는 건너뛰고 8에서 종료한다. [실습 README](../../exercises/09-loops/9-5/README.md)

## 10. 추가 실습
- ★ 홀수만 출력
- ★★ 첫 번째 5의 배수에서 종료
- ★★★ 같은 흐름을 `while`과 `for`로 작성해 `continue` 목적지 비교

## 11. 확인 문제
1. `break` 뒤 제어는 어디로 가는가?
2. `for`의 `continue`는 다음에 무엇을 평가하는가?
3. `do-while`의 `continue`는 어디로 가는가?
4. 중첩 반복의 `break`가 종료하는 범위는?
5. `switch`의 `break`와 공통점은?

## 12. 핵심 정리
`break`는 가장 안쪽 문 하나를 종료하고, `continue`는 반복 종류에 맞는 다음 단계로 이동한다.

## 13. 다음 Step
[Step 9-6. 반복 범위와 off-by-one 오류](9-6-off-by-one.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.6.2, 6.8.6.3
- [cppreference: continue](https://en.cppreference.com/w/c/language/continue.html)
- [cppreference: break](https://en.cppreference.com/w/c/language/break.html)
