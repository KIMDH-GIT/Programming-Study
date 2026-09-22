# 20-8. alignment와 padding

alignment는 object가 놓일 수 있는 주소에 관한 implementation requirement이고, padding은 layout이 그 요구를 만족하도록 둘 수 있는 unnamed bytes다.

## 1. 학습 목표
- member 사이 padding과 structure 끝 padding을 설명한다.
- member declaration order와 실제 byte offset을 구분한다.
- C17, ABI, compiler, CPU 역할을 분리한다.

## 2. 선수 지식
20-7의 size/alignment/offset과 Part 19 structure layout을 안다.

## 3. 핵심 개념
```c
struct Example {
    char code;
    int value;
};
```
C17은 non-bit-field members가 선언 순서대로 증가하는 주소에 놓이고 첫 member 앞에는 padding이 없음을 보장한다. member 사이와 마지막 member 뒤에는 padding이 있을 수 있다.

## 4. 문법
layout을 검사할 때 `_Alignof`, `sizeof`, `offsetof`를 사용한다. pointer arithmetic으로 임의 offset을 만들거나 packed layout을 가정하지 않는다.

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

struct A { char c; int value; };
struct B { int value; char c; };

int main(void)
{
    printf("A: %zu %zu\n", sizeof(struct A), offsetof(struct A, value));
    printf("B: %zu %zu\n", sizeof(struct B), offsetof(struct B, c));
    return 0;
}
```

## 6. 코드 해석
member 순서가 다른 두 types의 현재 구현 결과를 관찰한다. 어느 structure size도 특정 숫자로 고정하지 않는다.

## 7. 내부 동작
- **[C17 표준]** first member 앞 padding은 없고, 뒤 members는 declaration order의 increasing addresses를 가진다. 필요한 unnamed padding을 허용한다.
- **[ABI]** platform data layout convention을 정한다.
- **[compiler]** ABI와 options에 맞춰 offsets를 선택한다.
- **[CPU]** misaligned access 제약·성능 특성을 가질 수 있다.
MIPS, RISC-V, x86-64 같은 ISA 이름만으로 C layout이 결정되지는 않는다.

## 8. 자주 하는 실수
- 모든 members 사이에 반드시 padding이 있다고 말한다.
- 모든 structures가 packed라고 가정한다.
- padding bytes에 semantic value가 있다고 생각한다.
- bit-field를 portable hardware layout 해결책으로 미리 도입한다.

## 9. 필수 실습
member order가 다른 structures의 size와 offsets를 관찰하고 결과를 “현재 구현”으로 기록한다.
[20-8 exercise](../../exercises/20-enum-typedef-union/20-8/README.md)

## 10. 추가 실습
- ★ 세 member 순서를 바꾼다.
- ★★ array stride와 structure size를 확인한다.
- ★★★ compiler/ABI/CPU 역할 표를 만든다.

## 11. 확인 문제
1. 첫 member 앞 padding이 가능한가?
2. member 사이와 끝 padding은 가능한가?
3. declaration order가 보장하는 것은?
4. exact alignment 숫자는 누가 정하는가?
5. bit-field가 portable register layout을 보장하는가?

## 12. 핵심 정리
- alignment requirement를 만족하도록 padding이 생길 수 있다.
- order 보장과 byte-wise adjacency는 다르다.
- exact layout은 implementation/ABI 관찰값이다.

## 13. 다음 Step
[20-9. `unsigned char`로 byte representation 관찰](20-9-observe-byte-representation.md)

## 14. 참고 자료
- N1570 6.2.6.1, 6.2.8, 6.7.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 layout 규칙은 C17에서도 유지된다.
- [cppreference: object](https://en.cppreference.com/w/c/language/object)
- [System V AMD64 ABI](https://gitlab.com/x86-psABIs/x86-64-ABI)
