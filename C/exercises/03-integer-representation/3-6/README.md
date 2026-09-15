# Step 3-6 실습: 2의 보수와 C17 signed 표현
이론: [3-6](../../../notes/03-integer-representation/3-6-twos-complement-c17-signed-representations.md)
## 준비
```sh
cd exercises/03-integer-representation/3-6
```
완성 답안은 제공하지 않으며 UB·trap 패턴을 실행하지 않는다.
## 필수 과제
- **목적:** 2의 보수 모형과 C17 허용 표현을 구별한다.
- **작성할 파일:** `signed_model.c`.
- **해야 할 일:** 1, -1, `INT_MIN/MAX`를 출력하고 지정된 8-bit 패턴을 세 signed 모형으로 종이에서 해석한다.
- **사용할 개념:** signed 표현, negative zero, trap, 한계 매크로.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic signed_model.c -o signed_model
```
## 실행 방법
```sh
./signed_model
```
## 예상 관찰 결과
실행은 안전한 값과 구현 한계를 보이며, 종이표는 선택한 모형에 따라 결과가 다르다.
## 확인 포인트
- 표에 `[8-bit 구현 예]`를 붙였는가?
- C17이 2의 보수를 강제한다고 쓰지 않았는가?
- 예외 패턴을 실행하지 않았는가?
## 기록할 내용
| 항목 | 기록 |
|---|---|
| 빌드·실행 | |
| 구현 한계 | |
| 세 표현 비교 | |
## 추가 실습
- ★ -5 계산
- ★★ `10000101` 비교
- ★★★ 구현 문서 조사
## 완료 기준
- [ ] 경고 없이 빌드했다.
- [ ] 세 표현과 예외 패턴을 구별했다.
- [ ] 구현 예와 C17 보장을 분리했다.
다음: [3-7](../../../notes/03-integer-representation/3-7-same-bits-signed-unsigned.md)
