# 18-3. 지역 변수 주소 반환의 문제

함수가 끝나면 automatic local object의 lifetime도 끝나므로 그 주소를 반환해 dereference할 수 없다.

## 1. 학습 목표
- local automatic object 주소 반환이 잘못된 이유를 설명한다.
- allocated object pointer 반환과 비교한다.
- 반환 함수의 ownership contract를 작성한다.

## 2. 선수 지식
Step 18-2의 object lifetime과 Part 16의 함수 반환을 안다.

## 3. 핵심 개념
`return &local;`은 함수 종료 뒤 lifetime이 끝난 object를 가리키는 pointer를 만든다. 반면 allocation 성공 pointer를 반환하면 allocated object는 함수 block 종료와 무관하게 살아 있고 caller가 `free`할 책임을 받을 수 있다.

## 4. 문법
```c
int *make_value(int initial);
/* 성공: allocated pointer, 실패: NULL, caller가 free */
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

static int *make_value(int initial)
{
    int *p = malloc(sizeof *p);

    if (p != NULL) {
        *p = initial;
    }
    return p;
}

int main(void)
{
    int *value = make_value(30);

    if (value == NULL) {
        return 1;
    }
    printf("%d\n", *value);
    free(value);
    return 0;
}
```

## 6. 코드 해석
callee는 성공 시 allocated pointer를 반환한다. caller는 실패를 검사하고 값을 사용한 뒤 `free`한다.

## 7. 내부 동작
automatic local object는 block execution 종료와 함께 lifetime이 끝난다. allocated object는 `free` 전까지 유지된다. allocator가 어느 OS API를 사용하는지는 C17이 정하지 않는다.

## 8. 자주 하는 실수
- pointer를 반환하면 pointed-to local도 살아 있다고 생각한다.
- allocated pointer 반환 함수의 실패 가능성을 무시한다.
- caller와 callee 중 누가 `free`할지 문서화하지 않는다.

## 9. 필수 실습
`make_value`의 성공·실패·해제 책임 contract를 적고 정상 경로를 실행한다.
[18-3 exercise](../../exercises/18-dynamic-memory/18-3/README.md)

## 10. 추가 실습
- ★ 잘못된 local 주소 반환을 실행 없이 분석한다.
- ★★ caller-owned allocation을 callee가 채우는 대안을 작성한다.
- ★★★ 두 API의 ownership 표를 비교한다.

## 11. 확인 문제
1. local 주소가 함수 종료 뒤 유효하지 않은 이유는?
2. allocated object가 함수 종료 뒤 유지되는 이유는?
3. allocation-returning function은 실패를 어떻게 나타내는가?
4. 누가 `free`해야 하는가?

## 12. 핵심 정리
- automatic local 주소를 반환하지 않는다.
- allocated pointer 반환은 명확한 failure·ownership contract가 필요하다.

## 13. 다음 Step
[18-4. 원소 수와 할당 크기 overflow 검증](18-4-allocation-size-overflow.md)

## 14. 참고 자료
- N1570 6.2.4 Storage durations; 7.22.3.4 `malloc`. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: lifetime](https://en.cppreference.com/w/c/language/lifetime)
- [CERT C: returning addresses of automatic objects](https://wiki.sei.cmu.edu/confluence/display/c/DCL30-C.+Declare+objects+with+appropriate+storage+durations)
