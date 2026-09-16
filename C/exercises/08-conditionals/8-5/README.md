# 8-5 실습: switch
이론: [note](../../../notes/08-conditionals/8-5-switch.md)
## 실습 목적
case 선택과 break를 이해한다.
## 작성할 파일
`switch_menu.c`
## 해야 할 일
고정 메뉴 1/2/기타를 switch로 분류한다.
## 사용할 개념
switch, case, break, default.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic switch_menu.c -o switch_menu
```
## 실행 방법
```sh
./switch_menu
```
## 예상 관찰 결과
two.
## 확인 포인트
- 각 case에 break가 필요한가?
- default가 있는가?
## 추가 실습
- ★ default. - ★★ fallthrough 분석. - ★★★ 누락 찾기.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 흐름을 설명했다.
- [ ] 답안 소스를 제공하지 않았다.
