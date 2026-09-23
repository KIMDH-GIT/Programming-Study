# 23-8. `fread`와 `fwrite`
## 1. 학습 목표
- block I/O의 size와 element count arguments를 구분한다.
- 반환값을 성공한 elements 수로 해석한다.
- short read와 partial write를 검사한다.
## 2. 선수 지식
Part 8 arrays, Part 18 size arithmetic, 23-7 binary stream을 안다.
## 3. 핵심 개념
```c
size_t count = fread(buffer,
                     sizeof buffer[0],
                     element_count,
                     fp);
```
return은 일반적으로 성공적으로 읽은 elements 수다. element size가 1일 때만 그 수가 byte 수와 같아 보인다. 요청 전체를 항상 읽거나 완전히 실패하는 API가 아니다.
## 4. 문법
```c
size_t fread(void *restrict ptr, size_t size,
             size_t nmemb, FILE *restrict stream);
size_t fwrite(const void *restrict ptr, size_t size,
              size_t nmemb, FILE *restrict stream);
```
short read 뒤에는 `feof`와 `ferror`로 원인을 확인한다. return count가 가리키는 complete elements만 사용해야 하며, 부분적으로 읽힌 다음 element의 값은 indeterminate다. `fwrite` result가 requested count보다 작으면 write failure를 처리한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    const unsigned int source[] = {10u, 20u, 30u};
    unsigned int destination[3] = {0u, 0u, 0u};
    FILE *fp = fopen("part23_values.bin", "wb");
    size_t count;

    if (fp == NULL) {
        return 1;
    }
    count = fwrite(source, sizeof source[0], 3u, fp);
    if (count != 3u) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }

    fp = fopen("part23_values.bin", "rb");
    if (fp == NULL) {
        return 1;
    }
    count = fread(destination, sizeof destination[0], 3u, fp);
    if (count != 3u) {
        fclose(fp);
        return 1;
    }
    if (fclose(fp) == EOF) {
        return 1;
    }
    printf("%u %u %u\n",
           destination[0], destination[1], destination[2]);
    return 0;
}
```
## 6. 코드 해석
세 `unsigned int` object representations를 같은 program 환경에서 쓰고 읽는다. 반환값은 3 elements와 비교한다. 이 file이 다른 implementation에서도 portable하다고 주장하지 않는다.
## 7. 내부 동작
**[C17 stdio stream]** block I/O는 stream과 object array 사이에서 characters를 전송한다.

**[C standard library]** `size`와 `nmemb`로 element 단위를 정하고 count를 반환한다.

**[OS file / file descriptor]** library가 buffering이나 여러 OS operations를 사용할 수 있다.

**[filesystem]** file truncation이나 외부 변경으로 short read가 발생할 수 있다.

**[device / storage]** completed stdio call과 durable device commit은 같은 보장이 아니다.
## 8. 자주 하는 실수
- `fread` return을 항상 bytes로 해석한다.
- request 전체가 아니면 반드시 0이라고 생각한다.
- `fwrite`가 언제나 모든 elements를 쓴다고 가정한다.
- binary buffer를 C string으로 출력한다.
- element count와 buffer capacity의 곱 overflow를 무시한다.
## 9. 필수 실습
세 integers를 binary stream으로 write/read하고 element counts를 검사한다.
[23-8 exercise](../../exercises/23-file-io/23-8/README.md)
## 10. 추가 실습
- ★ 요청 count를 줄여 일부 elements만 읽는다.
- ★★ file 끝을 넘어 읽고 `feof`를 확인한다.
- ★★★ fixed byte encoding과 raw object representation을 비교한다.
## 11. 확인 문제
1. `fread` return의 단위는?
2. element size가 1일 때 생기는 착시는?
3. short read 원인을 어떻게 구분하는가?
4. `fwrite` result는 무엇과 비교하는가?
5. 같은 implementation에서 round trip 성공이 portability를 증명하지 않는 이유는?
## 12. 핵심 정리
- `size`, `nmemb`, returned element count를 구분한다.
- partial I/O를 검사한다.
- raw object representation의 portability 한계를 유지한다.
## 13. 다음 Step
[23-9. 구조체 통째 저장의 padding·pointer·호환성 문제](23-9-raw-struct-storage-problems.md)
## 14. 참고 자료
- N1570 7.21.8.1, 7.21.8.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fread](https://en.cppreference.com/w/c/io/fread)
- [cppreference: fwrite](https://en.cppreference.com/w/c/io/fwrite)
