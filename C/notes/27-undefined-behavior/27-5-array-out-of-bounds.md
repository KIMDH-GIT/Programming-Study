# 27-5. array out-of-bounds
## 1. 학습 목표
- 배열 경계 밖 원소 access가 UB임을 설명한다.
- one-past pointer 형성과 dereference를 구분한다.
- index 검사를 올바른 type과 순서로 수행한다.
## 2. 선수 지식
Part 11의 배열과 Part 15의 pointer arithmetic·one-past를 안다.
## 3. 핵심 개념
배열 원소는 index `0`부터 `N - 1`까지 존재한다. `array + N`인 one-past pointer는 비교와 순회 종료 표시에 형성할 수 있지만, 가리키는 원소가 없으므로 역참조할 수 없다.

⚠ 분석용 — 실행하지 않는다.
```c
int values[3] = {1, 2, 3};
int bad = values[3]; /* *(values + 3): UB */
```
## 4. 문법
```c
size_t count = sizeof values / sizeof values[0];
if (index < count) {
    printf("%d\n", values[index]);
}
```
외부에서 받은 signed index라면 음수 여부를 먼저 검사한 뒤 `size_t`와 비교한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int print_at(const int values[], size_t count, size_t index)
{
    if (index >= count) {
        return 0;
    }
    printf("%d\n", values[index]);
    return 1;
}

int main(void)
{
    int values[] = {1, 2, 3};

    (void)print_at(values, sizeof values / sizeof values[0], 2);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bounds
./bounds
```
## 6. 코드 해석
`index == 2`는 세 원소 배열의 마지막 유효 index다. 출력은 `3`이다.
## 7. 내부 동작
**[C17]** 배열 원소처럼 동작하는 pointer arithmetic은 같은 array object와 one-past 범위에 한정된다. one-past pointer의 형성은 허용되지만 `*end`는 허용되지 않는다.

**[optimizer]** 모든 defined access가 경계 안에 있다는 전제에서 코드를 변환할 수 있다.

**[runtime / OS]** 이웃 객체 값이 읽히거나 crash하지 않는 관찰도 정의된 access로 바꾸지 않는다.

**[security]** 경계 위반은 취약점으로 이어질 수 있지만 모든 UB가 곧 exploit 가능하다는 뜻은 아니다.
## 8. 자주 하는 실수
- `values[3]`이 “다음 변수”를 읽는다고 예측한다.
- one-past pointer 자체가 UB라고 한다.
- pointer가 mapped memory 안에만 있으면 C access가 유효하다고 생각한다.
- 정상 출력이나 ASan 무보고를 안전성 증명으로 삼는다.
## 9. 필수 실습
유효·무효 index를 반환값으로 구분하고 유효한 경우만 원소를 읽는다.
[27-5 exercise](../../exercises/27-undefined-behavior/27-5/README.md)
## 10. 추가 실습
- ★ 첫 원소와 마지막 원소를 검사한다.
- ★★ signed input의 음수 index를 먼저 거부한다.
- ★★★ 시작과 one-past pointer로 안전하게 순회한다.
## 11. 확인 문제
1. 길이 N인 배열의 유효 index 범위는?
2. `array + N` 형성은 허용되는가?
3. `*(array + N)`은 왜 다른가?
4. mapped address 여부가 C object 경계를 대신할 수 있는가?
5. 정상 실행이 경계 준수를 증명하는가?
## 12. 핵심 정리
- 실제 원소만 access한다.
- one-past pointer는 종료 위치 표현용이지 역참조 대상이 아니다.
- bounds check는 access 전에 수행한다.
## 13. 다음 Step
[27-6. NULL pointer dereference](27-6-null-pointer-dereference.md)
## 14. 참고 자료
- N1570 6.5.2.1, 6.5.6p7-8, 6.5.3.2p4, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic)
