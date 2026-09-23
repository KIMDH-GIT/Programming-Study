# 23-7. text file과 binary file
## 1. 학습 목표
- C17 text stream과 binary stream을 구분한다.
- platform text translation을 C 일반 보장으로 확대하지 않는다.
- binary mode와 portable representation을 같은 개념으로 보지 않는다.
## 2. 선수 지식
Part 3 representation, Part 20 padding·endianness, 23-2 modes를 안다.
## 3. 핵심 개념
text stream은 lines로 구성된 characters의 ordered sequence이며 구현이 외부 표현과 C characters 사이를 변환할 수 있다. 예를 들어 일부 Windows environments에서 newline translation이 관찰될 수 있지만 C17이 Windows CRLF 형식을 규정하는 것은 아니다.

binary stream은 characters의 ordered sequence를 보존하는 성질을 제공하지만, C object representation 자체가 다른 implementation에서도 portable file format이 된다는 뜻은 아니다.
## 4. 문법
```c
FILE *text = fopen("values.txt", "w");
FILE *binary = fopen("values.bin", "wb");
```
POSIX environment에서 `b`가 observable 차이를 만들지 않을 수 있어도 이를 ISO C 전체 규칙으로 일반화하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    const unsigned char bytes[] = {0u, 10u, 255u};
    FILE *fp = fopen("part23_bytes.bin", "wb");
    size_t written;

    if (fp == NULL) {
        return 1;
    }
    written = fwrite(bytes, sizeof bytes[0], 3u, fp);
    if (written != 3u) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("written elements: %zu\n", written);
    return 0;
}
```
## 6. 코드 해석
세 `unsigned char` elements를 binary stream에 쓴다. 이는 byte values를 저장하는 예제이지 arbitrary struct가 portable하게 직렬화된다는 증명이 아니다.
## 7. 내부 동작
**[C17 stdio stream]** text와 binary stream은 positioning·translation 규칙이 다를 수 있다.

**[C standard library]** mode의 `b`로 binary stream을 요청한다.

**[OS file / file descriptor]** OS가 text/binary를 구분하지 않는 환경도 있지만 implementation 관찰일 뿐이다.

**[filesystem]** file은 bytes를 저장해도 그 bytes의 application format은 program이 설계한다.

**[device / storage]** binary mode가 physical disk bytes, cache, durability를 직접 규정하지 않는다.
## 8. 자주 하는 실수
- text와 binary mode가 모든 OS에서 완전히 같다고 단정한다.
- Windows newline behavior를 C17 규칙이라고 설명한다.
- binary mode면 endianness·padding·representation 문제가 사라진다고 생각한다.
- arbitrary binary buffer를 null-terminated string처럼 `%s`로 출력한다.
- embedded null byte 뒤 data를 C string function으로 처리한다.
## 9. 필수 실습
작은 `unsigned char` array를 binary stream에 쓰고 element count를 확인한다.
[23-7 exercise](../../exercises/23-file-io/23-7/README.md)
## 10. 추가 실습
- ★ 같은 values를 decimal text로 저장한다.
- ★★ text file과 binary file의 size를 현재 platform에서 관찰하고 implementation result라고 표시한다.
- ★★★ portable integer encoding에 필요한 byte order 규칙을 설계한다.
## 11. 확인 문제
1. text stream은 translation이 가능할까?
2. POSIX에서 `b` 차이가 없을 수 있다는 관찰을 일반화해도 되는가?
3. binary mode가 struct portability를 보장하지 않는 이유는?
4. binary buffer를 `%s`로 바로 출력하면 안 되는 이유는?
5. embedded null이 string processing에 미치는 영향은?
## 12. 핵심 정리
- text와 binary stream semantics를 구분한다.
- platform 관찰과 C17 보장을 분리한다.
- binary mode는 portable serialization 보장이 아니다.
## 13. 다음 Step
[23-8. `fread`와 `fwrite`](23-8-fread-and-fwrite.md)
## 14. 참고 자료
- N1570 7.21.2, 7.21.5.3, 7.21.8.1, 7.21.8.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: binary input/output](https://en.cppreference.com/w/c/io)
- [cppreference: fopen](https://en.cppreference.com/w/c/io/fopen)
