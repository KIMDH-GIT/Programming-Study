# 17-3 실습: `int *const p`

이론: [note](../../../notes/17-const-and-pointers/17-3-const-pointer.md)

## 실습 목적
const pointer의 고정된 pointer value와 수정 가능한 pointed-to object를 확인한다.

## 작성할 파일
`const_pointer.c`

## 해야 할 일
1. `int value = 10;`을 선언한다.
2. `int *const p = &value;`로 초기화한다.
3. `*p`로 값을 두 번 수정한다.
4. 매번 `value`를 출력한다.
5. `p` 재지정 코드는 작성하지 않는다.

## 사용할 개념
const pointer, initializer, dereference assignment.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic const_pointer.c -o const_pointer
```

## 실행 방법
```sh
./const_pointer
```

## 예상 관찰 결과
`*p` assignment에 따라 `value`가 바뀐 결과가 출력된다.

## 확인 포인트
- const-qualified 대상은 pointer object `p`다.
- pointed-to `int`는 non-const이므로 수정 가능하다.

## 추가 실습
- ★ 기초: `*p += 5;`를 사용한다.
- ★★ 응용: `p`와 `&value`를 `%p`로 출력한다.
- ★★★ 도전: pointer 재지정이 금지되는 이유를 modifiable lvalue로 설명한다.

## 완료 기준
- const pointer를 선언 시 초기화한다.
- `p`를 재지정하지 않는다.
- `*p`를 통한 정상 수정을 관찰한다.
