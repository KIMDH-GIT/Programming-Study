# 17-1 실습: `const` 객체

이론: [note](../../../notes/17-const-and-pointers/17-1-const-object.md)

## 실습 목적
const-qualified object를 읽고 modifiable lvalue가 아님을 설명한다.

## 작성할 파일
`const_object.c`

## 해야 할 일
1. `const int temperature = 25;`를 선언한다.
2. 값을 설명 문장과 함께 출력한다.
3. 수정 assignment는 넣지 않는다.
4. `const`와 compile-time constant가 같지 않은 이유를 주석 한 줄로 적는다.

## 사용할 개념
type qualifier, const object, lvalue, modifiable lvalue.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic const_object.c -o const_object
```

## 실행 방법
```sh
./const_object
```

## 예상 관찰 결과
초기화한 온도 25가 출력되고 warning이나 error가 없다.

## 확인 포인트
- 읽기는 허용되지만 assignment의 왼쪽 operand로는 사용할 수 없다.
- `const`만으로 저장 위치를 알 수 없다.

## 추가 실습
- ★ 기초: `const double` 값을 출력한다.
- ★★ 응용: non-const 객체와 출력 결과를 비교한다.
- ★★★ 도전: `case` label의 integer constant expression 규칙을 문서로만 조사한다.

## 완료 기준
- C17 옵션으로 warning 없이 compile된다.
- const 객체를 수정하지 않는다.
- compile-time constant나 ROM이라고 잘못 설명하지 않는다.
