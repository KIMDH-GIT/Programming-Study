# 26-4. initialized data와 BSS
## 1. 학습 목표
- static-duration object initialization을 C17 규칙으로 설명한다.
- initialized data와 BSS를 implementation observation으로 구분한다.
- `const`와 OS read-only memory를 동일시하지 않는다.
## 2. 선수 지식
26-1 static storage duration과 Part 17 `const`를 안다.
## 3. 핵심 개념
```c
int initialized = 10;
int zero_initialized;
const int table[] = {1, 2, 3};
```

세 file-scope objects는 static storage duration을 가진다. `zero_initialized`는 tentative definition일 수 있고, translation unit 끝의 규칙과 static initialization에 따라 0으로 초기화된다.

**[Linux + GCC/binutils 관찰]** initialized writable data, zero-initialized data, const-qualified data가 `.data`, BSS 성격 section, `.rodata` 등에 배치될 수 있지만 C17 보장은 아니다.
## 4. 문법
```c
static int count;       /* static-duration, zero initialization */
int initialized = 10;  /* static-duration, explicit initializer */
```

initializer 유무는 storage duration 종류의 정의가 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int initialized = 10;
int zero_initialized;
const int table[] = {1, 2, 3};

int main(void)
{
    printf("%d %d %d\n",
           initialized,
           zero_initialized,
           table[2]);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c -o data_app
./data_app
```

**[Linux + GCC/binutils 관찰]**
```sh
nm data_app
readelf -S data_app
size data_app
```
## 6. 코드 해석
portable result는 `10 0 3`이다. section names, symbol letters, sizes는 현재 compiler·linker·ELF build 결과다.
## 7. 내부 동작
**[C17]** static-duration objects의 initialization semantics를 규정한다. zero initialization의 언어 근거는 `.bss`가 아니다.

**[compiler / linker]** explicit values와 zero-filled storage를 object-file representation으로 효율적으로 표현할 수 있다.

**[OS / executable format]** loader/startup이 mappings와 zero-filled storage를 준비할 수 있다. BSS/NOBITS는 format·toolchain 전략이다.

**[CPU / ISA]** writable/read-only mappings의 protection은 OS·MMU 정책이며 `const` qualifier 자체와 동일하지 않다.
## 8. 자주 하는 실수
- uninitialized global은 항상 `.bss`라는 universal rule을 만든다.
- initialized global은 반드시 `.data`라고 C17 규칙으로 말한다.
- `const` object는 항상 `.rodata` 또는 ROM에 있다고 말한다.
- BSS zeroing이 C initialization semantics의 원인이라고 뒤집어 설명한다.
- tentative definition, linkage, duration을 같은 개념으로 본다.
## 9. 필수 실습
세 objects의 values를 확인하고 binutils output은 별도 implementation observation으로 기록한다.
[26-4 exercise](../../exercises/26-memory-structure/26-4/README.md)
## 10. 추가 실습
- ★ explicit zero initializer를 추가해 tool output 변화를 관찰한다.
- ★★ `const` qualification과 page protection 차이를 설명한다.
- ★★★ MCU startup의 `.data` copy와 BSS zeroing을 implementation strategy로 조사한다.
## 11. 확인 문제
1. 세 objects의 storage duration은?
2. zero initialization의 C17 근거는 `.bss`인가?
3. initializer 유무가 duration을 결정하는가?
4. `const`가 OS read-only page를 보장하는가?
5. tentative definition과 static duration은 같은 축인가?
## 12. 핵심 정리
- static initialization은 C17 semantics다.
- `.data`, `.bss`, `.rodata`는 implementation observations다.
- `const` qualification과 hardware/OS protection을 분리한다.
## 13. 다음 Step
[26-5. stack과 자동 객체](26-5-stack-and-automatic-objects.md)
## 14. 참고 자료
- N1570 6.2.2, 6.2.4, 6.7.9, 6.9.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Initialization](https://en.cppreference.com/w/c/language/initialization)
- GNU binutils documentation: `nm`, `readelf`, `size`
