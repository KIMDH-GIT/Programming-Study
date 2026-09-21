# 17-2 실습: `const int *p`

이론: [note](../../../notes/17-const-and-pointers/17-2-pointer-to-const.md)

## 실습 목적
pointer to const의 재지정 가능성과 읽기 전용 access를 구분한다.

## 작성할 파일
`pointer_to_const.c`

## 해야 할 일
1. 두 `int` 객체를 만든다.
2. `const int *p`를 첫 객체의 주소로 초기화한다.
3. `*p`를 출력한 뒤 `p`를 둘째 객체 주소로 재지정한다.
4. 둘째 값을 출력한다.
5. `*p`를 통한 수정은 작성하지 않는다.

## 사용할 개념
pointer to const, dereference, pointer reassignment, read-only access path.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_to_const.c -o pointer_to_const
```

## 실행 방법
```sh
./pointer_to_const
```

## 예상 관찰 결과
두 객체의 값이 순서대로 출력된다.

## 확인 포인트
- `p` 자체는 재지정할 수 있다.
- `p`가 가리키는 non-const 객체가 const 객체로 바뀌지는 않는다.

## 추가 실습
- ★ 기초: `int const *p` 표기로 바꾼다.
- ★★ 응용: 원래 객체 이름으로 수정한 값을 `*p`로 읽는다.
- ★★★ 도전: 금지되는 `*p` assignment의 이유만 별도 표에 적는다.

## 완료 기준
- 두 valid object만 가리킨다.
- pointer 재지정과 pointed-to object 수정을 혼동하지 않는다.
- C17 옵션으로 warning 없이 compile된다.
