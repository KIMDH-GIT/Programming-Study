# 31-3 실습: `volatile`의 atomicity·동기화 한계
이론: [note](../../../notes/31-system-embedded-c/31-3-volatile-non-guarantees-atomicity-synchronization.md)
## 실습 목적
volatile increment가 single-thread에서는 정의되지만 atomic/synchronization 증거는 아님을 설명한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`volatile unsigned counter`를 single-thread에서 한 번 증가시키고 read-modify-write 단계로 분해한다. thread나 signal/ISR concurrency test로 확장하지 않는다.
## 사용할 개념
volatile, read-modify-write, atomicity, compiler barrier, CPU barrier.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o volatile_limits
```
## 실행 방법
```sh
./volatile_limits
```
## 예상 관찰 결과
`single_thread_value=1`을 출력한다. 이 결과는 atomicity나 thread safety를 검증하지 않는다.
## 확인 포인트
volatile ≠ atomic ≠ synchronization ≠ memory barrier이다.
## 추가 실습
- ★ 증가의 read/modify/write 단계를 적는다.
- ★★ atomic operation과 비교 표를 만든다.
- ★★★ interrupt model에 필요한 추가 contract를 조사한다.
## 완료 기준
정상 실행과 concurrency non-claim을 모두 명시한다.
