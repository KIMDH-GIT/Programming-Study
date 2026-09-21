# 17-7 실습: 읽기 전용 배열 매개변수

이론: [note](../../../notes/17-const-and-pointers/17-7-read-only-array-parameters.md)

## 실습 목적
읽기 전용 array parameter와 count parameter로 배열을 안전하게 순회한다.

## 작성할 파일
`readonly_array_parameter.c`

## 해야 할 일
1. `double average(const int values[], size_t count)`를 작성한다.
2. `count > 0`인 배열만 호출한다.
3. 함수는 element를 읽기만 한다.
4. caller에서 배열과 count를 전달하고 평균을 출력한다.

## 사용할 개념
array parameter adjustment, pointer to const, `size_t`, pass-by-value.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic readonly_array_parameter.c -o readonly_array_parameter
```

## 실행 방법
```sh
./readonly_array_parameter
```

## 예상 관찰 결과
입력 배열의 산술 평균이 출력되고 원소는 바뀌지 않는다.

## 확인 포인트
- parameter는 조정 후 `const int *`다.
- 배열 길이는 `sizeof(parameter)`가 아니라 `count`로 받는다.

## 추가 실습
- ★ 기초: 최댓값을 반환하는 읽기 전용 함수를 작성한다.
- ★★ 응용: parameter를 `const int *` 표기로 바꾼다.
- ★★★ 도전: 두 배열을 읽는 비교 함수를 작성한다.

## 완료 기준
- 함수가 element를 수정하지 않는다.
- `size_t` count로 경계를 제한한다.
- C17 옵션으로 warning 없이 compile된다.
