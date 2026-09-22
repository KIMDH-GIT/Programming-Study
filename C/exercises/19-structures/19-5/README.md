# 19-5 실습: 구조체 포인터와 `->`
이론: [note](../../../notes/19-structures/19-5-structure-pointers-arrow.md)
## 실습 목적
유효한 structure pointer로 원본 member를 수정한다.
## 작성할 파일
`point_pointer.c`
## 해야 할 일
Point object와 pointer를 만들고 `->`로 좌표를 변경해 출력한다.
## 사용할 개념
`&`, structure pointer, `->`, lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic point_pointer.c -o point_pointer
```
## 실행 방법
```sh
./point_pointer
```
## 예상 관찰 결과
pointer로 바꾼 좌표가 object를 통해서도 보인다.
## 확인 포인트
null·dangling pointer를 사용하지 않는다.
## 추가 실습
- ★ 읽기 전용 pointer를 쓴다.
- ★★ 두 Point를 pointer로 교환한다.
- ★★★ 배열 element 주소를 사용한다.
## 완료 기준
올바른 operator로 warning 없이 원본을 수정한다.
