# 20-7. `sizeof`, `_Alignof`, `offsetof`

세 연산은 object type의 크기, alignment requirement, member offset을 현재 C implementation에서 관찰하게 한다.

## 1. 학습 목표
- `sizeof`, `_Alignof`, `offsetof`의 결과를 구분한다.
- 결과 type과 필요한 header를 안다.
- 관찰값을 모든 구현의 고정값으로 일반화하지 않는다.

## 2. 선수 지식
Part 2의 `sizeof`, Part 19 구조체, 20-5 union을 안다.

## 3. 핵심 개념
- `sizeof(T)`: type `T` object의 byte 크기
- `_Alignof(T)`: type `T`의 alignment requirement
- `offsetof(T, member)`: structure member의 byte offset
세 결과는 `size_t`로 표현된다.

## 4. 문법
```c
#include <stddef.h>
sizeof(struct Record)
_Alignof(struct Record)
offsetof(struct Record, value)
```
`offsetof`를 손으로 member 크기 합으로 대체하지 않는다.

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

struct Record {
    char code;
    int value;
};

int main(void)
{
    printf("size=%zu align=%zu offset=%zu\n",
           sizeof(struct Record),
           _Alignof(struct Record),
           offsetof(struct Record, value));
    return 0;
}
```

## 6. 코드 해석
현재 implementation의 크기, alignment, `value` offset을 출력한다. 특정 숫자를 정답으로 가정하지 않는다.

## 7. 내부 동작
- **[C17 표준]** `sizeof` 결과는 `size_t`이며 object size는 byte 단위다. `_Alignof`는 complete object type의 alignment requirement를 준다.
- `offsetof` macro는 지정 structure member offset을 `size_t`로 제공한다.
- **[compiler/ABI]** member type별 alignment와 실제 padding·offset을 결정한다.
- **[CPU]** alignment가 access 가능성이나 성능에 영향을 줄 수 있으나 C17이 특정 ISA 숫자를 강제하지 않는다.

## 8. 자주 하는 실수
- `sizeof(struct)`를 member `sizeof` 합으로 계산한다.
- `_Alignof` 결과를 주소 자체로 생각한다.
- offset을 이전 member 크기 합으로 계산한다.
- 한 GCC/x86-64 관찰값을 C17 보장이라고 부른다.

## 9. 필수 실습
두 member 순서를 가진 structures의 size, alignment, offsets를 출력한다.
[20-7 exercise](../../exercises/20-enum-typedef-union/20-7/README.md)

## 10. 추가 실습
- ★ union size/alignment를 관찰한다.
- ★★ member 순서를 바꾸어 비교한다.
- ★★★ enum size/alignment 관찰값을 compiler option과 비교한다.

## 11. 확인 문제
1. 세 결과의 type은?
2. `offsetof`에 필요한 header는?
3. alignment requirement는 주소와 같은가?
4. member 크기 합으로 offset을 portable하게 구할 수 있는가?
5. exact 관찰값을 일반화할 수 없는 이유는?

## 12. 핵심 정리
- size, alignment, offset은 서로 다른 속성이다.
- `offsetof`를 portable member offset interface로 사용한다.
- exact 숫자는 implementation 관찰값으로 기록한다.

## 13. 다음 Step
[20-8. alignment와 padding](20-8-alignment-and-padding.md)

## 14. 참고 자료
- N1570 6.2.8, 6.5.3.4, 7.19. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof)
- [cppreference: _Alignof](https://en.cppreference.com/w/c/language/alignof)
- [cppreference: offsetof](https://en.cppreference.com/w/c/types/offsetof)
