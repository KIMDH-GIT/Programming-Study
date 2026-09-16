# 10-10 실습: `print_menu`와 입력·처리·출력 분리

이론: [note](../../../notes/10-functions/10-10-menu-separation.md)
## 실습 목적
메뉴 출력, 입력 검증, 계산, 결과 출력의 역할을 구분한다.
## 작성할 파일
`menu_separation.c`
## 해야 할 일
`print_menu(void)`와 `add(int, int)`를 정의하고 `main`에서 두 정수 입력을 검증해 결과를 출력한다.
## 사용할 개념
`void` 함수, prototype, `scanf` 반환값, 처리 함수, caller.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic menu_separation.c -o menu_separation
```
## 실행 방법
```sh
./menu_separation
```
## 예상 관찰 결과
정상 입력에서는 합, 잘못된 입력에서는 `invalid input`이 출력된다.
## 확인 포인트
입력 변환 개수를 확인하고 계산을 별도 함수가 담당하는가?
## 추가 실습
- ★ 뺄셈 메뉴 - ★★ `switch` 선택 - ★★★ 역할 표 작성
## 완료 기준
- [ ] 경고 없음 - [ ] 정상/오류 입력 확인 - [ ] 역할 분리
