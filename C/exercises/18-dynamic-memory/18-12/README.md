# 18-12 실습: realloc과 aliases
이론: [note](../../../notes/18-dynamic-memory/18-12-realloc-old-pointer-and-aliases.md)

## 실습 목적
realloc success 뒤 aliases를 새 base에서 다시 만든다.
## 작성할 파일
`realloc_aliases.c`
## 해야 할 일
element index를 보존하고 realloc 성공 뒤 `&values[index]`를 다시 계산해 출력한다.
## 사용할 개념
old allocation lifetime, interior pointer, index.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic realloc_aliases.c -o realloc_aliases
```
## 실행 방법
```sh
./realloc_aliases
```
## 예상 관찰 결과
보존된 element 값이 출력된다.
## 확인 포인트
realloc 전 interior pointer를 성공 뒤 사용하지 않는다.
## 추가 실습
- ★ alias diagram
- ★★ multiple indices
- ★★★ pointer vs index design
## 완료 기준
새 base에서만 aliases를 생성한다.
