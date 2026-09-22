# 19-4 실습: 구조체 배열
이론: [note](../../../notes/19-structures/19-4-structure-arrays.md)

## 실습 목적
구조체 배열을 초기화하고 안전하게 순회한다.
## 작성할 파일
`student_array.c`
## 해야 할 일
학생 세 명을 array에 저장하고 count를 계산해 각 `students[i].member`를 출력한다.
## 사용할 개념
array of structures, indexing, `.`, `sizeof`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic student_array.c -o student_array
```
## 실행 방법
```sh
./student_array
```
## 예상 관찰 결과
세 학생의 id와 score가 저장 순서대로 출력된다.
## 확인 포인트
loop bound는 element count이며 array 자체에 `.`를 적용하지 않는다.
## 추가 실습
- ★ 평균을 출력한다.
- ★★ 최고 점수 학생을 찾는다.
- ★★★ 입력한 id의 index를 찾는다.
## 완료 기준
모든 access가 bounds 안이고 세 record가 warning 없이 출력된다.
