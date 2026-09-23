# 23-4 실습: `fscanf` 반환값·field width·검증 한계
이론: [note](../../../notes/23-file-io/23-4-fscanf-results-width-and-limits.md)
## 실습 목적
valid record, matching failure, input failure를 구분한다.
## 작성할 파일
`fscanf_validation.c`
## 해야 할 일
valid와 malformed records를 읽어 assignment count에 따라 분기한다.
## 사용할 개념
return count, matching failure, `EOF`, field width.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic fscanf_validation.c -o fscanf_validation
```
## 실행 방법
```sh
./fscanf_validation
```
## 예상 관찰 결과
첫 record는 valid, 두 번째 result는 0으로 관찰된다.
## 확인 포인트
partial assignment를 성공으로 취급하지 않고 destination capacity를 지킨다.
## 추가 실습
- ★ 한 field만 있는 record를 시험한다.
- ★★ width를 capacity에 맞춘다.
- ★★★ `fgets` parsing과 비교한다.
## 완료 기준
0, `EOF`, expected assignment count 차이를 설명한다.
