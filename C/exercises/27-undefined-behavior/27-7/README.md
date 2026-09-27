# 27-7 실습: object lifetime과 ownership
이론: [note](../../../notes/27-undefined-behavior/27-7-dangling-pointer-and-object-lifetime.md)
## 실습 목적
allocated object를 lifetime 안에서만 사용한다.
## 작성할 파일
- `main.c`
## 해야 할 일
생성 함수로 정수 하나를 할당하고 마지막 사용 뒤 정확히 한 번 해제한다.
## 사용할 개념
object lifetime, ownership, `malloc`, `free`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o lifetime
```
## 실행 방법
dangling access, use-after-free, double free는 실행하지 않는다.
```sh
./lifetime
```
## 예상 관찰 결과
`42`를 출력하고 정상 종료한다.
## 확인 포인트
모든 성공·실패 path의 owner와 cleanup 횟수를 확인한다.
## 추가 실습
- ★ cleanup 지점을 하나로 만든다.
- ★★ alias를 만든 뒤 owner와 구분한다.
- ★★★ 두 allocation의 부분 실패를 처리한다.
## 완료 기준
lifetime 밖 access와 중복 해제가 없다.
