# 5-11. `scanf` 반환값과 입력 검증

입력 함수 호출이 끝났다는 사실은 원하는 값이 저장되었다는 뜻이 아니다. `scanf` 반환값을 기대한 대입 수와 비교해야 입력 결과를 사용할 근거가 생긴다.

## 1. 학습 목표

- 반환값을 성공 대입 수와 연결한다.
- 일치 실패, 입력 끝, 정상 입력을 구별한다.
- 초기화와 반환값 기록으로 안전하게 관찰한다.

## 2. 선수 지식

Step 5-7~5-10의 입력 서식과 저장 객체를 사용한다. 조건 분기 자체는 Part 8에서 자세히 배운다.

## 3. 핵심 개념

두 변환을 요청했다면 반환값 2일 때만 둘 모두 성공했다. 0은 첫 입력이 서식과 일치하지 않은 경우가 대표적이다. 첫 변환 전에 입력 끝이나 읽기 오류가 생기면 `EOF`가 반환될 수 있다.

이번 Step은 값을 초기화하고 반환값을 출력해 상태를 구별한다. 실제 프로그램에서 실패 경로를 나누는 `if`는 Part 8에서 배운다.

## 4. 문법

```c
int matched = scanf("%d %d", &left, &right);
printf("expected=2 actual=%d\n", matched);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int left = 0;
    int right = 0;
    int matched;

    matched = scanf("%d %d", &left, &right);
    printf("matched=%d left=%d right=%d\n", matched, left, right);
    return 0;
}
```

## 6. 코드 해석

1. 실패 관찰에서도 indeterminate 값을 읽지 않도록 초기화한다.
2. 두 정수 입력을 요청한다.
3. 반환값이 실제로 대입된 객체 수를 나타낸다.
4. 출력값만 보고 성공을 추측하지 않고 `matched`를 함께 본다.

## 7. 내부 동작

변환 실패를 일으킨 문자는 입력 스트림에 남을 수 있다. 같은 `scanf` 호출을 무조건 반복하면 같은 문자에서 다시 실패할 수 있다. 입력 복구 전략은 제어문과 문자열 입력을 배운 뒤 구성한다.

## 8. 자주 하는 실수

- 반환값을 입력한 숫자의 값으로 해석한다.
- 1을 전체 성공이라고 생각한다.
- 실패한 객체를 초기화 없이 출력한다.
- 실패 입력이 자동으로 제거된다고 가정한다.

## 9. 필수 실습

정상 입력, 두 번째 변환 실패, 첫 변환 실패를 각각 실행해 반환값을 기록한다. [실습 README](../../exercises/05-input-output/5-11/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 두 정수 정상 입력을 확인한다.
- ★★ **응용:** `12 x`를 입력한다.
- ★★★ **도전:** EOF를 보내 반환값을 관찰한다.

## 11. 확인 문제

1. 두 변환이 모두 성공한 반환값은 얼마인가?
2. 반환값 1은 무엇을 뜻하는가?
3. 첫 변환 전에 입력 끝이면 무엇을 반환할 수 있는가?
4. 실패 문자가 스트림에 남을 수 있다는 사실이 왜 중요한가?

## 12. 핵심 정리

`scanf`의 반환값은 성공한 대입 수다. 기대한 수와 일치해야 모든 입력이 준비되었다고 판단할 수 있다.

## 13. 다음 Step

[Step 5-12. 잘못된 format specifier와 Undefined Behavior](5-12-format-mismatch-ub.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.2.
- [cppreference: `scanf`](https://en.cppreference.com/w/c/io/fscanf.html)
- [GCC Format Warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wformat)
