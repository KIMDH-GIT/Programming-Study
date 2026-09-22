# 20-9. `unsigned char`로 byte representation 관찰

C는 어떤 object든 `unsigned char` lvalue를 통해 object representation의 bytes로 관찰할 수 있게 한다.

## 1. 학습 목표
- abstract value와 object representation을 구분한다.
- `unsigned char *`로 object bytes를 읽는다.
- 관찰 bytes를 semantic equality나 serialization으로 오용하지 않는다.

## 2. 선수 지식
Part 14 포인터, 20-7 size, 20-8 padding을 안다.

## 3. 핵심 개념
object representation은 object를 이루는 `unsigned char` 배열 관점의 bytes다. 같은 semantic value라도 padding이나 여러 representation 가능성 때문에 byte sequence를 일반적인 value 자체와 동일시할 수 없다.

## 4. 문법
```c
const unsigned char *bytes = (const unsigned char *)&object;
for (size_t i = 0; i < sizeof object; ++i) {
    printf("%02X ", (unsigned int)bytes[i]);
}
```
`unsigned char` access는 object representation 관찰에 허용된 특별한 경로다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    unsigned int value = 0x0102u;
    const unsigned char *bytes = (const unsigned char *)&value;

    for (size_t i = 0; i < sizeof value; ++i) {
        printf("%02X%s", (unsigned int)bytes[i],
               i + 1 == sizeof value ? "\n" : " ");
    }
    return 0;
}
```

## 6. 코드 해석
`value`를 변환하지 않고 현재 implementation의 object representation을 byte 단위로 출력한다. byte 순서나 전체 길이를 미리 고정하지 않는다.

## 7. 내부 동작
- **[C17 표준]** non-bit-field object는 `unsigned char` 배열처럼 복사·관찰할 수 있다. `sizeof object`가 representation의 byte 수다.
- structure/union에는 padding bytes가 있을 수 있고 저장 뒤 그 값은 unspecified일 수 있다.
- **[compiler/ABI/CPU]** integer representation과 byte order를 정한다.

## 8. 자주 하는 실수
- 출력 bytes가 모든 platform에서 같다고 생각한다.
- raw bytes를 곧바로 numeric semantic value라고 부른다.
- `memcmp`로 struct/union semantic equality를 판단한다.
- raw bytes를 파일·network에 쓰면 portable serialization이라고 생각한다.

## 9. 필수 실습
`unsigned int` object의 모든 bytes를 `unsigned char` pointer로 출력한다.
[20-9 exercise](../../exercises/20-enum-typedef-union/20-9/README.md)

## 10. 추가 실습
- ★ 여러 unsigned values를 관찰한다.
- ★★ structure bytes를 관찰하되 padding 의미를 부여하지 않는다.
- ★★★ value comparison과 representation comparison을 표로 구분한다.

## 11. 확인 문제
1. abstract value와 object representation 차이는?
2. 왜 `unsigned char` access가 사용되는가?
3. `sizeof object`는 무엇을 정하는가?
4. padding bytes를 semantic field로 볼 수 있는가?
5. `memcmp`가 일반 semantic equality가 아닌 이유는?

## 12. 핵심 정리
- bytes는 representation 관찰 결과다.
- 관찰 결과는 implementation에 의존할 수 있다.
- semantic comparison과 portable encoding은 별도 설계가 필요하다.

## 13. 다음 Step
[20-10. endianness 기초](20-10-endianness-basics.md)

## 14. 참고 자료
- N1570 6.2.6.1, 6.5 p7. N1570은 **C11 공개 Committee Draft**이며 character-type access 규칙은 C17에서도 유지된다.
- [cppreference: object representation](https://en.cppreference.com/w/c/language/object)
- [SEI CERT EXP42-C](https://wiki.sei.cmu.edu/confluence/display/c/EXP42-C.+Do+not+compare+padding+data)
