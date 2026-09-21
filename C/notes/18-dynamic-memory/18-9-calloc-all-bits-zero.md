# 18-9. `calloc`의 all-bits-zero 의미

`calloc`은 배열을 위한 storage를 할당하고 모든 bits를 zero로 만든다.

## 1. 학습 목표
- `calloc` prototype과 initialization contract를 설명한다.
- all-bits-zero와 모든 타입의 semantic zero를 구분한다.
- `malloc` 후 명시적 저장과 비교한다.

## 2. 선수 지식
Step 18-5의 `malloc`과 object representation 개념을 안다.

## 3. 핵심 개념
```c
void *calloc(size_t nmemb, size_t size);
```
`nmemb` objects를 위한 storage를 할당하고 모든 bits를 zero로 초기화한다. 이 bit pattern이 모든 floating type의 `0.0`이나 모든 pointer type의 null pointer라고 일반화할 수 없다.

## 4. 문법
```c
unsigned char *bytes = calloc(count, sizeof *bytes);
```
all-bits-zero가 확실히 값 0인 unsigned character type으로 관찰한다.

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    size_t count = 4;
    unsigned char *bytes = calloc(count, sizeof *bytes);

    if (bytes == NULL) {
        return 1;
    }
    printf("%u\n", (unsigned)bytes[0]);
    free(bytes);
    return 0;
}
```

## 6. 코드 해석
네 unsigned bytes를 allocation하고 all-bits-zero 상태의 첫 값을 안전하게 출력한다.

## 7. 내부 동작
`calloc`은 두 인자의 곱으로 표현되는 배열 요청을 받는다. conforming implementation은 요청을 만족하거나 실패를 반환해야 하므로 단순한 unchecked undersized allocation으로 contract를 깨뜨릴 수 없다.

## 8. 자주 하는 실수
- 모든 타입 object가 C 의미의 값 0이 된다고 일반화한다.
- `calloc`과 `memset`을 모든 타입에서 같은 semantic initialization으로 본다.
- pointer 배열이 항상 null pointers로 초기화된다고 단정한다.

## 9. 필수 실습
`unsigned char` 배열을 `calloc`하고 모든 element가 0인지 출력한 뒤 해제한다.
[18-9 exercise](../../exercises/18-dynamic-memory/18-9/README.md)

## 10. 추가 실습
- ★ 일부 bytes에 값을 저장한다.
- ★★ `malloc` 후 loop 초기화와 source-level 차이를 비교한다.
- ★★★ bit representation과 value semantics 차이를 설명한다.

## 11. 확인 문제
1. `calloc`의 두 인자는 무엇인가?
2. 초기화되는 것은 값인가 bits인가?
3. all-bits-zero를 모든 pointer의 null로 일반화할 수 있는가?
4. `malloc`과 초기 내용이 어떻게 다른가?

## 12. 핵심 정리
- `calloc`은 all-bits-zero storage를 제공한다.
- bit pattern의 의미는 object type representation과 구분한다.
- failure 검사와 `free`는 여전히 필요하다.

## 13. 다음 Step
[18-10. 0 크기 할당의 구현 차이](18-10-zero-size-allocation.md)

## 14. 참고 자료
- N1570 7.22.3.2 `calloc`; 6.2.6 Representations of types. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: calloc](https://en.cppreference.com/w/c/memory/calloc)
- [cppreference: object representation](https://en.cppreference.com/w/c/language/object)
