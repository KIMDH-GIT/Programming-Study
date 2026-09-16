# 13-3. 문자열의 `'\0'` 종료

C string은 값이 0인 null character `'\0'`으로 끝나는 character sequence다.

## 1. 학습 목표
- `'\0'`과 `'0'`을 구분한다.
- null terminator까지 string을 순회한다.
- null 없는 char array를 `%s`에 사용하지 않는다.

## 2. 선수 지식
Step 13-2의 char array와 Part 9의 loop를 사용한다.

## 3. 핵심 개념
`'\0'`은 값 0인 null character이고 `'0'`은 숫자 문자 zero다. string을 처리하는 함수와 `%s`는 null terminator를 만나 끝을 판단한다. 모든 char array가 자동으로 string이 되는 것은 아니다. `{'a','b','c'}`에는 terminator가 없어 그 자체를 `%s`로 출력하면 안전하지 않다.

## 4. 문법
```c
char word[4] = {'C', 'a', 't', '\0'};
for (size_t i = 0; word[i] != '\0'; ++i) {
    /* character 사용 */
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    char word[4] = {'C', 'a', 't', '\0'};
    size_t length = 0;

    while (word[length] != '\0') {
        ++length;
    }

    printf("%s\n", word);
    printf("length: %zu\n", length);
    printf("'0': %d, '\\0': %d\n", '0', '\0');
    return 0;
}
```

## 6. 코드 해석
loop는 C, a, t를 지나 index 3의 `'\0'`에서 멈춰 length 3을 얻는다. 마지막 출력은 `'0'`과 `'\0'`이 다른 값임을 보여 준다. 숫자 문자 값은 환경에 따라 달라도 null character 값은 0이다.

## 7. 내부 동작
[C17 표준] null character는 값 0인 character다. library string 규칙은 null-terminated byte string을 전제로 한다. terminator가 없는 array를 `%s`로 읽게 하면 배열 밖까지 읽을 수 있어 Undefined Behavior가 될 수 있으므로 실행 실습으로 만들지 않는다.

## 8. 자주 하는 실수
- `'0'`과 `'\0'`을 같은 값이라고 생각한다.
- 모든 char array가 string이라고 생각한다.
- terminator를 length에 포함한다.
- null 없는 array를 `%s`로 출력해 결과를 관찰한다.

## 9. 필수 실습
explicit char array 끝에 `'\0'`을 넣고 직접 length를 계산한다. [실습 README](../../exercises/13-characters-and-strings/13-3/README.md)

## 10. 추가 실습
- ★ 빈 string 길이 계산
- ★★ 중간에 `'\0'`이 있는 배열의 출력 범위 확인
- ★★★ char array와 valid string 판별 문제

## 11. 확인 문제
1. `'\0'`의 정수값은?
2. `'0'`과 같은가?
3. string length에 terminator가 포함되는가?
4. null 없는 char array를 `%s`에 주면 왜 위험한가?
5. 빈 string의 첫 element는?

## 12. 핵심 정리
C string은 `'\0'`으로 끝나며, char array가 string으로 쓰이려면 접근 가능한 범위 안에 terminator가 있어야 한다.

## 13. 다음 Step
[Step 13-4. 문자열 리터럴과 수정 가능한 배열](13-4-literal-modifiable-array.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 5.2.1, 6.4.5, 7.1.1
- [cppreference: Null-terminated byte strings](https://en.cppreference.com/w/c/string/byte.html)
