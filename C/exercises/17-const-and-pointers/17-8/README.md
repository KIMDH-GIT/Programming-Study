# 17-8 실습: 문자열 리터럴과 `const char *`

이론: [note](../../../notes/17-const-and-pointers/17-8-string-literals-and-const-char-pointer.md)

## 실습 목적
문자열을 읽기만 하는 parameter를 만들고 C17 string literal 규칙을 지킨다.

## 작성할 파일
`readonly_text.c`

## 해야 할 일
1. `void print_text(const char text[])`를 작성한다.
2. null character까지 한 글자씩 출력한다.
3. string literal과 수정 가능한 `char` 배열을 각각 전달한다.
4. 어느 입력도 함수에서 수정하지 않는다.

## 사용할 개념
`const char *`, string literal, null character, array parameter adjustment.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic readonly_text.c -o readonly_text
```

## 실행 방법
```sh
./readonly_text
```

## 예상 관찰 결과
두 문자열이 각각 한 줄에 출력된다.

## 확인 포인트
- C17 string literal 수정은 undefined behavior이므로 시도하지 않는다.
- C의 literal type을 C++의 `const char[N]` 규칙으로 설명하지 않는다.

## 추가 실습
- ★ 기초: `strlen`으로 길이를 출력한다.
- ★★ 응용: 특정 문자의 개수를 세는 함수를 작성한다.
- ★★★ 도전: 복사 함수에서 destination과 source qualifier를 설계한다.

## 완료 기준
- 함수 parameter가 읽기 전용 의도를 나타낸다.
- string literal 수정 코드가 없다.
- C17 옵션으로 warning 없이 compile된다.
