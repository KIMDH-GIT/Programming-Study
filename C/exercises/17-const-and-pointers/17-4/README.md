# 17-4 실습: `const int *const p`

이론: [note](../../../notes/17-const-and-pointers/17-4-const-pointer-to-const.md)

## 실습 목적
pointer와 pointed-to type 양쪽의 const qualification을 구분한다.

## 작성할 파일
`const_pointer_to_const.c`

## 해야 할 일
1. const `int` 객체를 만든다.
2. `const int *const p`를 그 주소로 초기화한다.
3. `*p`를 읽어 출력한다.
4. 네 가지 pointer 선언의 `p` 재지정·`*p` 수정 가능 여부를 주석 표로 적는다.

## 사용할 개념
pointer to const, const pointer, const pointer to const.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic const_pointer_to_const.c -o const_pointer_to_const
```

## 실행 방법
```sh
./const_pointer_to_const
```

## 예상 관찰 결과
초기 const 객체의 값이 출력된다.

## 확인 포인트
- `p` 재지정과 `*p` 수정이 모두 제한된다.
- 두 `const`가 서로 다른 타입 층을 한정한다.

## 추가 실습
- ★ 기초: 네 선언을 자연어로 읽는다.
- ★★ 응용: non-const 객체를 `const int *const`로 읽는다.
- ★★★ 도전: 허용·금지 연산을 코드 실행 없이 분류한다.

## 완료 기준
- const pointer to const를 정확히 초기화한다.
- 금지된 assignment를 실행 코드에 넣지 않는다.
- 비교표의 네 행이 정확하다.
