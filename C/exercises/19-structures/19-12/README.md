# 19-12 실습: 학생 검색
이론: [note](../../../notes/19-structures/19-12-student-search.md)
## 실습 목적
검색 성공과 실패를 안전하게 구분한다.
## 작성할 파일
`student_search.c`
## 해야 할 일
const array와 count를 받아 id index 또는 sentinel을 반환한다.
## 사용할 개념
linear search, const, `SIZE_MAX`, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_search.c -o student_search
```
## 실행 방법
```sh
./student_search
```
## 예상 관찰 결과
있는 id는 record가, 없는 id는 안내가 출력된다.
## 확인 포인트
sentinel을 index로 사용하지 않는다.
## 추가 실습
- ★ score 검색을 한다.
- ★★ pointer 반환과 비교한다.
- ★★★ 검색 함수를 등록에서 재사용한다.
## 완료 기준
성공·실패 경로 모두 bounds 안에서 종료한다.
