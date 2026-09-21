# 17-9 실습: Part 17 종합 복습

이론: [note](../../../notes/17-const-and-pointers/17-9-part-17-review.md)

## 실습 목적
const object, 네 pointer 형태, qualification 추가, 읽기 전용 parameter를 종합한다.

## 작성할 파일
`part17_review.c`

## 해야 할 일
1. 네 pointer/const 선언의 비교표를 주석으로 작성한다.
2. non-const 배열을 수정 가능한 const pointer와 읽기 전용 pointer로 각각 가리킨다.
3. `sum(const int values[], size_t count)`를 작성한다.
4. `print_text(const char text[])`로 결과 설명을 출력한다.
5. 금지된 assignment나 undefined behavior는 넣지 않는다.

## 사용할 개념
const object, pointer to const, const pointer, qualification conversion, array adjustment.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic part17_review.c -o part17_review
```

## 실행 방법
```sh
./part17_review
```

## 예상 관찰 결과
const 규칙을 지키며 수정된 배열 값과 합계, 설명 문자열이 출력된다.

## 확인 포인트
- pointer 자체와 pointed-to type의 const를 분리한다.
- 읽기 전용 parameter도 pointer value pass-by-value다.
- compile 성공과 C17 의미 검증을 같은 것으로 간주하지 않는다.

## 추가 실습
- ★ 기초: 모든 선언을 자연어로 읽는다.
- ★★ 응용: 하나의 object를 두 access path로 관찰한다.
- ★★★ 도전: qualification 추가·제거 사례를 실행 없는 분석표로 정리한다.

## 완료 기준
- Part 17의 네 pointer 형태를 정확히 구분한다.
- 모든 pointer가 valid lifetime의 object를 가리킨다.
- C17 옵션으로 warning 없이 compile된다.
