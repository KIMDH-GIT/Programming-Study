# 13-5. 제한된 문자열 입력과 잘린 줄 처리

`fgets`는 buffer 크기를 받아 최대 저장량을 제한하지만, 한 번의 호출이 전체 입력 줄을 모두 읽었다고 항상 보장하지는 않는다.

## 1. 학습 목표
- `fgets(buffer, sizeof buffer, stdin)`의 크기 제한을 설명한다.
- 반환값과 newline 포함 여부를 확인한다.
- buffer에 들어오지 못한 나머지 줄을 제거한다.

## 2. 선수 지식
Part 5의 input 검증, Step 13-3의 null terminator, Part 9의 loop를 사용한다.

## 3. 핵심 개념
크기 8인 char array에는 최대 7 input characters와 마지막 `'\0'`이 저장된다. newline까지 공간에 들어오면 `fgets`는 newline도 저장한다. newline이 없다면 EOF 직전일 수도 있고 줄이 잘렸을 수도 있다. interactive line input에서는 남은 characters를 newline 또는 EOF까지 읽어 다음 입력에 섞이지 않게 할 수 있다.

## 4. 문법
```c
if (fgets(line, sizeof line, stdin) == NULL) {
    /* EOF 또는 read error */
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    char line[8];

    printf("input: ");
    if (fgets(line, sizeof line, stdin) == NULL) {
        printf("no input\n");
        return 1;
    }

    size_t i = 0;
    while (line[i] != '\0' && line[i] != '\n') {
        ++i;
    }

    if (line[i] == '\n') {
        line[i] = '\0';
    } else {
        int ch;
        while ((ch = getchar()) != '\n' && ch != EOF) {
        }
    }

    printf("stored: %s\n", line);
    return 0;
}
```

## 6. 코드 해석
짧은 줄은 newline을 찾아 `'\0'`으로 바꾼다. 7자를 넘는 줄은 array가 null-terminated 상태로 유지되고, 남은 입력은 `getchar` loop가 제거한다. `fgets` 실패 시 buffer를 string으로 사용하지 않는다.

## 7. 내부 동작
[C17 표준] `fgets`는 성공 시 최대 `n-1` characters를 읽고 null character를 저장한다. newline을 읽으면 함께 저장한다. 반환값이 null pointer이면 EOF 또는 read error 경로이며 정확한 구분에는 stream 상태 검사가 더 필요하다. pointer 자체의 자세한 의미는 Part 14에서 다룬다.

## 8. 자주 하는 실수
- 반환값을 확인하지 않는다.
- newline이 항상 저장된다고 가정한다.
- 잘린 나머지 줄을 다음 입력에 남긴다.
- `scanf("%s", line)`을 field width 없이 안전한 기본 패턴으로 사용한다.

## 9. 필수 실습
작은 buffer로 짧은 줄과 긴 줄을 입력해 newline 제거와 나머지 줄 폐기를 확인한다. [실습 README](../../exercises/13-characters-and-strings/13-5/README.md)

## 10. 추가 실습
- ★ newline이 들어오는 짧은 입력
- ★★ buffer를 채우는 긴 입력
- ★★★ 연속 두 줄 입력에서 residue가 없는지 확인

## 11. 확인 문제
1. 크기 8 buffer에 저장 가능한 최대 input characters는?
2. `fgets`가 newline을 저장하는 조건은?
3. 반환값을 왜 확인하는가?
4. newline이 없으면 고려할 두 경우는?
5. 남은 줄을 제거하지 않으면 다음 입력에 어떤 영향이 있는가?

## 12. 핵심 정리
`fgets`의 크기·반환값·newline을 함께 확인하고 잘린 줄의 residue를 처리해야 반복 입력이 안전하다.

## 13. 다음 Step
[Step 13-6. 문자 배열 매개변수 문법 예고](13-6-char-array-parameter-preview.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.7.2
- [cppreference: fgets](https://en.cppreference.com/w/c/io/fgets.html)
