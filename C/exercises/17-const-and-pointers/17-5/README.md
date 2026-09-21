# 17-5 실습: 선언 읽기

이론: [note](../../../notes/17-const-and-pointers/17-5-reading-declarations.md)

## 실습 목적
identifier에서 바깥으로 const-pointer 선언을 읽고 수정 가능 여부를 판별한다.

## 작성할 파일
`read_declarations.c`

## 해야 할 일
1. `int *p`, `const int *cp`, `int *const fp`, `const int *const fcp`를 각각 선언한다.
2. 모든 pointer가 valid `int` object를 가리키게 한다.
3. 허용되는 읽기와 수정 한 가지씩만 실행한다.
4. 각 선언에 세 질문의 답을 주석으로 적는다.

## 사용할 개념
declarator reading, pointer reassignment, pointed-to qualification.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic read_declarations.c -o read_declarations
```

## 실행 방법
```sh
./read_declarations
```

## 예상 관찰 결과
모든 pointer가 가리키는 현재 값이 출력되고 warning이 없다.

## 확인 포인트
- `p` 재지정과 `*p` 수정을 따로 판정한다.
- pointer type만 보고 실제 object가 const인지 단정하지 않는다.

## 추가 실습
- ★ 기초: 각 선언을 자연어로 쓴다.
- ★★ 응용: 같은 object를 가리키는 세 접근 경로를 그린다.
- ★★★ 도전: compiler가 거부할 식을 별도 분석표로 작성한다.

## 완료 기준
- 네 선언을 모두 정확히 읽는다.
- 실제 코드에는 허용되는 연산만 둔다.
- 세 질문에 각각 답한다.
