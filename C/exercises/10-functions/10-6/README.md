# 10-6 실습: 자동 객체의 lifetime

이론: [note](../../../notes/10-functions/10-6-automatic-lifetime.md)
## 실습 목적
호출마다 parameter와 local object가 새로 만들어짐을 이해한다.
## 작성할 파일
`automatic_lifetime.c`
## 해야 할 일
local 계산값을 반환하는 함수를 같은 argument로 두 번 호출해 결과를 출력한다.
## 사용할 개념
automatic storage duration, lifetime, local variable, 반복 호출.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic automatic_lifetime.c -o automatic_lifetime
```
## 실행 방법
```sh
./automatic_lifetime
```
## 예상 관찰 결과
각 호출이 같은 초기 상태에서 계산되어 같은 결과를 출력한다.
## 확인 포인트
이전 호출의 local 값이 다음 호출에 남는다고 가정하지 않았는가?
## 추가 실습
- ★ 다른 argument - ★★ 안쪽 block - ★★★ stack 단정 오류 설명
## 완료 기준
- [ ] 경고 없음 - [ ] 두 호출 결과 설명 - [ ] scope와 lifetime 구분
