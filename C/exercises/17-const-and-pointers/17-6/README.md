# 17-6 실습: const qualifier 추가·제거

이론: [note](../../../notes/17-const-and-pointers/17-6-qualification-conversions.md)

## 실습 목적
한 단계 object pointer에서 qualification 추가 방향을 안전하게 사용한다.

## 작성할 파일
`qualification_conversion.c`

## 해야 할 일
1. non-const `int`와 그 주소를 저장한 `int *p`를 만든다.
2. `const int *cp = p;`로 읽기 전용 경로를 추가한다.
3. `*p`로 값을 수정한다.
4. `*cp`로 수정 결과를 읽는다.
5. qualifier 제거나 cast는 사용하지 않는다.

## 사용할 개념
qualification conversion, pointer assignment, read-only access path.

## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic qualification_conversion.c -o qualification_conversion
```

## 실행 방법
```sh
./qualification_conversion
```

## 예상 관찰 결과
`*p`로 수정한 값이 `*cp`를 통해 출력된다.

## 확인 포인트
- qualification 추가는 object 자체의 선언을 바꾸지 않는다.
- 제거 방향은 정상 코드로 사용하지 않는다.

## 추가 실습
- ★ 기초: 두 pointer의 주소 값을 출력한다.
- ★★ 응용: 허용되는 초기화 방향을 화살표로 그린다.
- ★★★ 도전: qualifier 제거 diagnostic을 문서로만 분석한다.

## 완료 기준
- qualification 추가 방향만 실행한다.
- cast가 없다.
- C17 옵션으로 warning 없이 compile된다.
