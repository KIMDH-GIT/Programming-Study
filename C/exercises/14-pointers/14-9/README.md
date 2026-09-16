# 14-9 실습: 유효한 역참조 대상과 object lifetime

이론: [note](../../../notes/14-pointers/14-9-valid-dereference-lifetime.md)
## 실습 목적
object lifetime 안에서만 pointer를 dereference한다.
## 작성할 파일
`pointer_lifetime.c`
## 해야 할 일
inner block local을 block 안에서 읽고 block 밖에서는 pointer를 NULL로 교체한다.
## 사용할 개념
automatic lifetime, valid target, dangling risk, NULL reset.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic pointer_lifetime.c -o pointer_lifetime
```
## 실행 방법
```sh
./pointer_lifetime
```
## 예상 관찰 결과
42와 `no valid target`이 출력된다.
## 확인 포인트
lifetime 종료 후 old pointer value를 읽거나 dereference하지 않는가?
## 추가 실습
- ★ outer object - ★★ nested objects - ★★★ lifetime diagram
## 완료 기준
- [ ] 경고 없음 - [ ] lifetime 안 access - [ ] dangling dereference 없음
