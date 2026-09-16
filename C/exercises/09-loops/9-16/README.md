# 9-16 실습: 별 출력

이론: [note](../../../notes/09-loops/9-16-star-patterns.md)
## 실습 목적
행·열 반복과 newline 위치로 패턴을 만든다.
## 작성할 파일
`star_pattern.c`
## 해야 할 일
1개부터 4개까지 늘어나는 왼쪽 정렬 별 삼각형을 출력한다.
## 사용할 개념
중첩 `for`, 행, 열, newline.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic star_pattern.c -o star_pattern
```
## 실행 방법
```sh
./star_pattern
```
## 예상 관찰 결과
4줄에 별이 각각 1, 2, 3, 4개 출력된다.
## 확인 포인트
newline이 내부 반복 뒤에 한 번만 실행되는가?
## 추가 실습
- ★ 사각형 - ★★ 역삼각형 - ★★★ 행 번호
## 완료 기준
- [ ] 경고 없음 - [ ] 4줄 모양 정확 - [ ] 총 별 10개
