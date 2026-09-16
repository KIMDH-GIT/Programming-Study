# 9-16. 실습: 별 출력

별 패턴은 외부 반복이 줄을, 내부 반복이 한 줄의 문자를 담당한다.

## 1. 학습 목표
- 행과 열 역할을 구분한다.
- 줄마다 내부 반복 횟수를 바꾼다.
- newline의 위치로 출력 모양을 제어한다.

## 2. 선수 지식
Step 9-7의 중첩 반복과 `printf` 출력이 필요하다.

## 3. 핵심 개념
삼각형의 `row`번째 줄에는 별을 `row`개 출력한다. 내부 반복이 끝난 뒤에만 newline을 출력해야 같은 줄의 별이 붙고 다음 행으로 이동한다. 배열이나 문자열 저장은 필요하지 않다.

## 4. 문법
```c
for (int row = 1; row <= height; ++row) {
    for (int column = 1; column <= row; ++column) {
        printf("*");
    }
    printf("\n");
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int row = 1; row <= 4; ++row) {
        for (int column = 1; column <= row; ++column) {
            printf("*");
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
행별 내부 실행 횟수는 1, 2, 3, 4다. 총 별 수는 10개이며 newline은 외부 반복마다 한 번씩 4회 출력된다.

## 7. 내부 동작
[C 언어 관점] 출력 호출 순서가 패턴을 결정한다. newline을 내부 반복에 넣으면 별마다 줄이 바뀐다. [GNU/Linux 환경] 터미널 출력은 buffering될 수 있지만 프로그램의 논리적 문자 순서는 유지된다.

## 8. 자주 하는 실수
- newline을 내부 반복 안에 둔다.
- `column <= 4`로 모든 줄을 같은 길이로 만든다.
- 내부 조건에서 `row`와 `column`을 바꾼다.
- 공백까지 필요하지 않은 패턴에 복잡한 정렬 로직을 추가한다.

## 9. 필수 실습
높이 4인 왼쪽 정렬 삼각형을 출력하고 총 별 수를 계산한다. [실습 README](../../exercises/09-loops/9-16/README.md)

## 10. 추가 실습
- ★ 4x4 사각형
- ★★ 4개에서 1개로 줄어드는 삼각형
- ★★★ 각 줄 앞에 행 번호 출력

## 11. 확인 문제
1. 외부 반복은 무엇을 나타내는가?
2. 세 번째 줄의 내부 반복 횟수는?
3. newline은 왜 내부 반복 밖에 있는가?
4. 총 별 수는?

## 12. 핵심 정리
패턴 출력은 줄과 줄 안의 문자를 나누고, 내부 경계와 newline 위치로 모양을 만든다.

## 13. 다음 Step
[Step 9-17. 실습: 중첩 반복문](9-17-nested-loop-practice.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5
- [cppreference: Statements](https://en.cppreference.com/w/c/language/statements.html)
