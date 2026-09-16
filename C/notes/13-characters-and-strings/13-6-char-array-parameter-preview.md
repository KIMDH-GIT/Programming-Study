# 13-6. 문자 배열 매개변수 문법 예고

function parameter의 `char text[]` 표기는 일반 local char array 선언과 다른 특별한 adjustment 규칙을 가진다.

## 1. 학습 목표
- char array parameter 문법을 읽는다.
- parameter 안의 `[]`를 local array object 선언과 동일시하지 않는다.
- 실제 길이를 별도 정보나 terminator로 판단하는 이유를 설명한다.

## 2. 선수 지식
Part 10의 parameter·argument와 Step 13-3의 null-terminated string을 안다.

## 3. 핵심 개념
`void print_text(char text[])`처럼 function parameter에 배열 표기를 쓰면 C17은 이를 pointer parameter 형태로 조정한다. 지금은 “호출자가 제공한 character sequence를 함수가 index로 사용한다”는 문법만 예고한다. parameter 안에서 `sizeof(text)`로 원본 array count를 얻을 수 없으며 정확한 pointer 규칙은 Part 14~16에서 배운다.

## 4. 문법
```c
void print_text(char text[])
{
    printf("%s\n", text);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void print_text(char text[])
{
    printf("%s\n", text);
}

int main(void)
{
    char message[] = "hello";
    print_text(message);
    return 0;
}
```

## 6. 코드 해석
`main`의 valid string을 argument로 전달하고 `print_text`는 parameter 표기를 통해 같은 character sequence를 `%s`로 출력한다. `message`는 `main`의 실제 array object지만 parameter 선언은 새 array object를 복사해 만들지 않는다.

## 7. 내부 동작
[C17 표준] function parameter에서 “array of type” 선언은 “qualified pointer to type”으로 조정된다. 따라서 parameter `sizeof`는 caller array 전체 크기를 나타내지 않는다. 이 규칙을 “모든 배열은 pointer다”라고 일반화하면 안 된다.

## 8. 자주 하는 실수
- parameter에 배열 전체가 값으로 복사된다고 생각한다.
- parameter의 `sizeof(text)`로 caller array count를 구한다.
- 일반 array declaration과 parameter array notation을 같은 규칙이라고 말한다.
- pointer adjustment를 배웠다고 pointer 전체 문법까지 확장한다.

## 9. 필수 실습
valid char array를 받는 출력 함수를 정의하고 `main`에서 호출한다. [실습 README](../../exercises/13-characters-and-strings/13-6/README.md)

## 10. 추가 실습
- ★ 첫 character 출력 함수
- ★★ 두 messages를 각각 호출
- ★★★ local array와 parameter notation 비교표

## 11. 확인 문제
1. parameter `char text[]`는 새 array object를 만드는가?
2. parameter에서 어떤 adjustment가 일어나는가?
3. `sizeof(text)`가 caller array count를 주는가?
4. 일반 array가 pointer와 같은 type인가?
5. 정확한 pointer 관계는 어느 Part에서 배우는가?

## 12. 핵심 정리
array parameter notation은 특별히 pointer parameter로 조정되며 일반 array object와 동일한 선언 의미가 아니다.

## 13. 다음 Step
[Step 13-7. `my_strlen`](13-7-my-strlen.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.7.6.3
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html#Arrays_of_unknown_size)
