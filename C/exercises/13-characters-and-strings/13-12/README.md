# 13-12 실습: `strcmp`의 반환값

이론: [note](../../../notes/13-characters-and-strings/13-12-strcmp-result.md)
## 실습 목적
`strcmp` 결과를 magnitude가 아닌 sign으로 해석한다.
## 작성할 파일
`strcmp_result.c`
## 해야 할 일
equal, before, after string pairs를 비교해 관계 문장을 출력한다.
## 사용할 개념
`strcmp`, negative/zero/positive, content equality, valid string.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic strcmp_result.c -o strcmp_result
```
## 실행 방법
```sh
./strcmp_result
```
## 예상 관찰 결과
세 pair가 equal, before, after로 각각 분류된다.
## 확인 포인트
`== -1`이나 `== 1`에 의존하지 않는가?
## 추가 실습
- ★ equal - ★★ prefix - ★★★ magnitude 오류 분석
## 완료 기준
- [ ] 경고 없음 - [ ] 세 관계 정확 - [ ] result sign 사용
