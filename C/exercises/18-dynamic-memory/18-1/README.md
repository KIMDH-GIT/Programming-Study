# 18-1 실습: storage duration 모델
이론: [note](../../../notes/18-dynamic-memory/18-1-storage-duration-model.md)

## 실습 목적
automatic object, pointer object, allocated storage를 구분한다.
## 작성할 파일
`storage_duration.c`
## 해야 할 일
automatic `int`와 allocation한 `int`를 만들고 저장·출력·해제한다. 각 lifetime을 주석으로 표시한다.
## 사용할 개념
automatic storage duration, allocated storage duration, pointer object.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic storage_duration.c -o storage_duration
```
## 실행 방법
```sh
./storage_duration
```
## 예상 관찰 결과
두 값이 출력되고 allocation이 한 번 해제된다.
## 확인 포인트
- heap은 구현 모델임을 구분한다.
- pointer object와 target을 별도로 그린다.
## 추가 실습
- ★ lifetime 표 작성
- ★★ allocator/OS 역할 분리
- ★★★ abstract machine 설명 추가
## 완료 기준
failure 검사·초기화·free가 있고 C17 warning이 없다.
