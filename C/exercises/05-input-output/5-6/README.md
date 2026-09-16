# 5-6 실습: `sizeof` 출력

이론: [5-6 note](../../../notes/05-input-output/5-6-sizeof-output.md)

## 실습 목적
`size_t`, `%zu`, C byte를 연결한다.
## 작성할 파일
`sizeof_output.c`
## 해야 할 일
여러 기본형과 변수의 `sizeof`, `CHAR_BIT`를 출력한다.
## 사용할 개념
`sizeof`, `size_t`, `%zu`, C byte, `CHAR_BIT`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic sizeof_output.c -o sizeof_output
```
## 실행 방법
```sh
./sizeof_output
```
## 예상 관찰 결과
구현의 자료형 크기가 C byte 단위로 표시된다.
## 확인 포인트
- `%zu`를 사용했는가?
- byte와 bit를 구별했는가?
## 추가 실습
- ★ **기초:** 기본형을 더 관찰한다.
- ★★ **응용:** 저장 bit 수를 계산한다.
- ★★★ **도전:** 구현 차이를 기록한다.
## 완료 기준
- [ ] 경고 없이 컴파일된다.
- [ ] 단위를 정확히 기록했다.
- [ ] `CHAR_BIT`와 연결했다.
- [ ] 답안 `.c`를 제공하지 않았다.
