# C 언어 자율진도형 학습 지도

## 사용법

- `C 0-1 공부하자`: 지정한 Step만 독립 튜토리얼로 학습한다.
- `C 14-3 다시 설명해줘`: 지정한 Step만 다시 학습한다.
- 실습 결과 제출은 선택이다. 오류나 코드를 보내면 해당 Step 맥락에서 검토한다.
- 기간이 아니라 개념적 선후관계에 따라 진행한다.

## 모든 Step의 구성

1. 이번 Step의 목표
2. 핵심 개념 설명
3. 최소 코드 예제
4. C·메모리·컴파일러·OS·CPU 관점의 필요한 내부 동작
5. 직접 확인하기
6. 기초·응용·선택 도전 문제
7. Step 요약
8. 다음 Step 제목

설명에서는 C17의 보장, 구현 정의, 미지정 동작, OS·CPU·ABI 의존 현상을 구분한다.

---

## Part 0. C 프로그램과 컴파일 과정

- 0-1. 전체 컴파일 과정 개요
- 0-2. C 소스 `.c`와 헤더 `.h`
- 0-3. 전처리기와 `.i` 파일: `gcc -E`
- 0-4. 컴파일러와 `.s` 파일: `gcc -S`
- 0-5. 어셈블러와 `.o` 파일: `gcc -c`
- 0-6. 링커·라이브러리와 실행 파일
- 0-7. 전체 빌드와 실행: `gcc hello.c -o hello`
- 0-8. 단계별 오류 구분
- 0-9. Part 0 종합 복습

## Part 1. C 프로그램 기본 구조

- 1-1. 첫 프로그램과 `int main(void)`
- 1-2. `#include`와 표준 헤더
- 1-3. statement·중괄호·세미콜론
- 1-4. `printf`와 여러 줄 출력
- 1-5. `\n`, `\t`, `\"`, `\\` escape sequence
- 1-6. `return 0`과 종료 상태
- 1-7. 주석·들여쓰기·자기소개 실습
- 1-8. Part 1 종합 복습

## Part 2. 변수와 자료형

- 2-1. 변수 선언·초기화와 이름 규칙
- 2-2. `char`, `short`, `int`
- 2-3. `long`, `long long`
- 2-4. `signed`와 `unsigned`
- 2-5. `float`, `double`, `long double`
- 2-6. `sizeof`, `size_t`, byte와 `CHAR_BIT`
- 2-7. `<limits.h>`와 정수 범위
- 2-8. `<float.h>`와 부동소수점 범위·정밀도
- 2-9. 구현별 자료형 크기 직접 확인
- 2-10. Part 2 종합 복습

## Part 3. 정수 표현

- 3-1. bit·byte와 자릿값
- 3-2. 10진수와 2진수
- 3-3. 16진수와 2진수
- 3-4. C의 10진·8진·16진 정수 리터럴
- 3-5. N-bit unsigned 표현과 범위
- 3-6. 2의 보수 모델과 C17이 허용하는 signed 표현
- 3-7. 같은 비트 패턴의 signed·unsigned 해석
- 3-8. unsigned 순환과 signed overflow
- 3-9. Part 3 종합 복습

## Part 4. `<stdint.h>`

- 4-1. 고정 폭 정수형이 필요한 이유
- 4-2. `int8_t`, `int16_t`, `int32_t`, `int64_t`
- 4-3. `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`
- 4-4. 정확한 폭의 타입이 제공되는 조건
- 4-5. 범위·상수 매크로와 `<inttypes.h>` 출력
- 4-6. `uint32_t register_value`와 레지스터 모델
- 4-7. Part 4 종합 복습

## Part 5. 입력과 출력

- 5-1. `printf` 서식 문자열과 인자 대응
- 5-2. `%d`, `%u`, `%x`
- 5-3. `%f`, `%Lf`와 출력 정밀도
- 5-4. `%c`, `%s`
- 5-5. `%p`, `(void *)`와 주소 출력 예고
- 5-6. `%zu`와 `sizeof` 출력
- 5-7. `scanf` 구조와 변수 주소
- 5-8. 정수 입력 서식 지정자
- 5-9. `scanf`의 `%f`, `%lf`, `%Lf`
- 5-10. 문자·문자열 입력과 buffer 한계
- 5-11. `scanf` 반환값과 입력 검증
- 5-12. 잘못된 format specifier와 Undefined Behavior
- 5-13. 계산 실습용 산술 연산자와 나눗셈 예고
- 5-14. 두 수의 사칙연산
- 5-15. BMI 계산
- 5-16. 섭씨·화씨 변환
- 5-17. 초를 시·분·초로 변환
- 5-18. Part 5 종합 복습

## Part 6. 형 변환

- 6-1. implicit conversion
- 6-2. 정수형 범위 축소와 범위 밖 변환
- 6-3. 정수와 부동소수점 사이 변환
- 6-4. integer promotion
- 6-5. usual arithmetic conversions
- 6-6. signed·unsigned 혼합 연산
- 6-7. explicit cast 문법
- 6-8. `5 / 2`와 `5.0 / 2`
- 6-9. cast 위치와 중간 결과
- 6-10. Part 6 종합 복습

## Part 7. 연산자

- 7-1. 식·피연산자·결과값
- 7-2. `+`, `-`, `*`, `/`, `%`
- 7-3. 나눗셈 경계: 0과 `INT_MIN / -1`
- 7-4. `==`, `!=`, `<`, `>`, `<=`, `>=`
- 7-5. `&&`, `||`, `!`와 short-circuit
- 7-6. `=`, `+=`, `-=`, `*=`, `/=`, `%=`
- 7-7. 전위·후위 `++`, `--`
- 7-8. 한 식에서 같은 객체를 여러 번 변경하는 위험
- 7-9. 우선순위·결합법칙·평가 순서
- 7-10. Part 7 종합 복습

## Part 8. 조건문

- 8-1. `if`
- 8-2. `if-else`
- 8-3. `else if`
- 8-4. 중첩 조건문과 경계값
- 8-5. `switch`, `case`, `break`, `default`
- 8-6. 홀짝과 양수·음수·0 판별
- 8-7. 세 수의 최댓값
- 8-8. 학점 계산
- 8-9. 메뉴 기반 계산기
- 8-10. Part 8 종합 복습

## Part 9. 반복문

- 9-1. 반복의 초기화·조건·변화
- 9-2. `while`
- 9-3. `do-while`
- 9-4. `for`
- 9-5. `break`와 `continue`
- 9-6. 반복 범위와 off-by-one 오류
- 9-7. 중첩 반복문
- 9-8. 실습: 1부터 100까지 출력
- 9-9. 실습: 1부터 100까지 합
- 9-10. 실습: 짝수 출력
- 9-11. 실습: 구구단
- 9-12. 실습: factorial
- 9-13. 실습: 최대공약수
- 9-14. 실습: 소수 판별
- 9-15. 실습: 범위 내 모든 소수
- 9-16. 실습: 별 출력
- 9-17. 실습: 중첩 반복문
- 9-18. Part 9 종합 복습

## Part 10. 함수

- 10-1. 함수 정의와 호출
- 10-2. 함수 선언과 prototype
- 10-3. 매개변수와 인수
- 10-4. 반환값과 `void`
- 10-5. 지역 변수와 scope
- 10-6. 자동 객체의 lifetime
- 10-7. 값에 의한 전달
- 10-8. call stack 기초
- 10-9. 계산기 연산 함수 분리
- 10-10. `print_menu`와 입력·처리·출력 분리
- 10-11. Part 10 종합 복습

### Project 진입점: Project 1

## Part 11. 배열

- 11-1. 배열 선언과 초기화
- 11-2. index로 원소 읽기·쓰기
- 11-3. 반복문으로 배열 순회
- 11-4. 연속 메모리 배치와 원소 주소
- 11-5. `sizeof(array)`와 원소 수
- 11-6. 배열 범위 밖 접근과 Undefined Behavior
- 11-7. 합과 평균
- 11-8. 최댓값과 최솟값
- 11-9. 역순 출력
- 11-10. 배열 검색
- 11-11. Part 11 종합 복습

## Part 12. 2차원 배열

- 12-1. 행·열과 2차원 배열 선언
- 12-2. 2차원 배열 초기화
- 12-3. 중첩 반복문 순회
- 12-4. row-major 메모리 배치
- 12-5. `sizeof`로 행·원소 크기 확인
- 12-6. 행렬 덧셈
- 12-7. 전치 행렬
- 12-8. 행렬 곱셈
- 12-9. Part 12 종합 복습

## Part 13. 문자와 문자열

- 13-1. `char`와 문자 `'A'`
- 13-2. 문자열 `"A"`와 문자 배열
- 13-3. 문자열의 `'\0'` 종료
- 13-4. 문자열 리터럴과 수정 가능한 배열
- 13-5. 제한된 문자열 입력과 잘린 줄 처리
- 13-6. 문자 배열 매개변수 문법 예고
- 13-7. `my_strlen`
- 13-8. `my_strcpy`
- 13-9. `my_strcmp`
- 13-10. `strlen`과 `sizeof`
- 13-11. `strcpy`와 목적지 용량
- 13-12. `strcmp`의 반환값
- 13-13. `strcat`과 연결 용량
- 13-14. 선택 실습: `my_strcat`
- 13-15. 문자·대문자·소문자·숫자·공백 개수 분석
- 13-16. Part 13 종합 복습

## Part 14. 포인터 기초

- 14-1. 메모리 주소란 무엇인가
- 14-2. 주소 연산자 `&`
- 14-3. 포인터 변수 선언
- 14-4. 역참조 연산자 `*`
- 14-5. 포인터를 통한 값 수정
- 14-6. `a`, `&a`, `p`, `&p`, `*p`
- 14-7. 포인터 자체의 주소
- 14-8. `NULL` pointer
- 14-9. 유효한 역참조 대상과 object lifetime
- 14-10. 포인터 타입과 접근형
- 14-11. 잘못된 포인터 접근
- 14-12. 포인터 기초 종합 실습
- 14-13. Part 14 종합 복습

## Part 15. 포인터와 배열

- 15-1. 배열 이름과 첫 원소 주소
- 15-2. 배열 객체와 포인터 변수의 차이
- 15-3. `arr[i]`와 `*(arr + i)`
- 15-4. `&arr[i]`와 `arr + i`
- 15-5. pointer arithmetic의 원소 단위
- 15-6. 같은 배열과 one-past 경계
- 15-7. 포인터 차이와 `ptrdiff_t`
- 15-8. 포인터로 배열 순회
- 15-9. 시작 전 포인터를 만들지 않는 역순 순회
- 15-10. 포인터만 이용한 배열 실습
- 15-11. Part 15 종합 복습

## Part 16. 포인터와 함수

- 16-1. 값 매개변수로 원본을 바꾸지 못하는 이유
- 16-2. 포인터 매개변수로 주소 전달
- 16-3. 역참조로 호출자 객체 변경
- 16-4. 출력 매개변수
- 16-5. `swap(int *a, int *b)`
- 16-6. 배열 매개변수와 pointer adjustment
- 16-7. 배열 길이를 별도로 전달하는 이유
- 16-8. 함수에서 배열 원소 변경
- 16-9. Part 16 종합 복습

## Part 17. `const`와 포인터

- 17-1. `const` 객체
- 17-2. `const int *p`
- 17-3. `int *const p`
- 17-4. `const int *const p`
- 17-5. 선언을 오른쪽에서 왼쪽으로 읽기
- 17-6. const qualifier 추가·제거
- 17-7. 읽기 전용 배열 매개변수
- 17-8. 문자열 리터럴과 `const char *`
- 17-9. Part 17 종합 복습

## Part 18. 동적 메모리

- 18-1. stack·heap 모델과 C의 storage duration
- 18-2. 자동 객체와 동적 객체의 lifetime
- 18-3. 지역 변수 주소 반환의 문제
- 18-4. 원소 수와 할당 크기 overflow 검증
- 18-5. `void *`와 `malloc`
- 18-6. `malloc(n * sizeof *ptr)`
- 18-7. 할당 실패와 `NULL`
- 18-8. `free`와 소유권
- 18-9. `calloc`의 all-bits-zero 의미
- 18-10. 0 크기 할당의 구현 차이
- 18-11. `realloc`과 임시 포인터
- 18-12. `realloc` 후 기존 포인터·내부 별칭
- 18-13. memory leak
- 18-14. dangling pointer와 use-after-free
- 18-15. double free와 `NULL` dereference
- 18-16. 동적 배열 out-of-bounds
- 18-17. 검사 실행 예제로 메모리 오류 관찰
- 18-18. 동적 배열 입력·평균·최댓값·최솟값
- 18-19. 동적 배열 정렬
- 18-20. Part 18 종합 복습

## Part 19. 구조체

- 19-1. 구조체 정의와 멤버
- 19-2. 구조체 선언·초기화와 `.`
- 19-3. `typedef struct`
- 19-4. 구조체 배열
- 19-5. 구조체 포인터와 `->`
- 19-6. `p->id`와 `(*p).id`
- 19-7. 구조체를 함수에 전달
- 19-8. `strtol`로 정수 입력 검증
- 19-9. `strtod`로 실수 입력 검증
- 19-10. 학생 등록과 중복 검사
- 19-11. 학생 삭제
- 19-12. 학생 검색
- 19-13. 학생 목록
- 19-14. 학생 정렬
- 19-15. 학생 관리 프로그램 통합
- 19-16. Part 19 종합 복습

## Part 20. `enum`, `typedef`, `union`

- 20-1. `enum`과 열거 상수
- 20-2. `typedef`
- 20-3. enum 기반 state 표현
- 20-4. `switch` 기반 state machine
- 20-5. `union`의 공유 저장 공간
- 20-6. tag와 union을 함께 쓰기
- 20-7. `sizeof`, `_Alignof`, `offsetof`
- 20-8. alignment와 padding
- 20-9. `unsigned char`로 byte representation 관찰
- 20-10. endianness 기초
- 20-11. Part 20 종합 복습

## Part 21. 비트 연산

- 21-1. `&`
- 21-2. `|`
- 21-3. `^`
- 21-4. `~`와 integer promotion
- 21-5. unsigned `<<`, `>>`
- 21-6. C17 signed shift의 UB·구현 정의 동작
- 21-7. 폭에 맞는 unsigned bit mask
- 21-8. bit set
- 21-9. bit clear
- 21-10. bit toggle
- 21-11. bit test
- 21-12. bit field 추출·갱신
- 21-13. `uint8_t GPIO` 가상 레지스터
- 21-14. `set`, `clear`, `toggle`, `read`
- 21-15. Part 21 종합 복습

## Part 22. 함수 포인터

- 22-1. 함수 주소와 함수 포인터 선언
- 22-2. 함수 포인터 대입과 호출
- 22-3. 함수 포인터 타입 호환성
- 22-4. `typedef` 함수 포인터
- 22-5. callback
- 22-6. 함수 포인터 배열
- 22-7. state machine과 함수 포인터
- 22-8. interrupt handler와 일반 callback의 차이
- 22-9. driver interface
- 22-10. 모의 GPIO driver와 callback
- 22-11. Part 22 종합 복습

## Part 23. 파일 입출력

- 23-1. `FILE *`, stream, `fopen`, `fclose`
- 23-2. 파일 모드와 열기 실패
- 23-3. `fprintf`와 `fscanf`
- 23-4. `fscanf` 반환값·field width·검증 한계
- 23-5. `fgets`와 `fputs`
- 23-6. EOF와 읽기 오류
- 23-7. text file과 binary file
- 23-8. `fread`와 `fwrite`
- 23-9. 구조체 통째 저장의 padding·pointer·호환성 문제
- 23-10. 학생 레코드 파일 형식 설계
- 23-11. 학생 데이터 저장
- 23-12. 학생 데이터 복원
- 23-13. 재실행 후 데이터 유지 확인
- 23-14. Part 23 종합 복습

## Part 24. 여러 C 파일로 프로그램 나누기

- 24-1. 기능별 source file 분리
- 24-2. declaration과 definition
- 24-3. header file과 공개 interface
- 24-4. source file과 translation unit
- 24-5. 개별 compile과 object file
- 24-6. 여러 object file link
- 24-7. external linkage와 `extern`
- 24-8. internal linkage와 file-scope `static`
- 24-9. block-scope `static` 복습
- 24-10. 중복 정의와 undefined reference
- 24-11. 학생 관리 프로그램 파일 분리
- 24-12. Part 24 종합 복습

## Part 25. 전처리기

- 25-1. 전처리 지시문과 결과
- 25-2. `#include`
- 25-3. 객체형 `#define`
- 25-4. 함수형 macro
- 25-5. `SQUARE(x)`와 괄호
- 25-6. macro 인자 중복 평가
- 25-7. `#if`, `#ifdef`, `#ifndef`, `#endif`
- 25-8. header guard
- 25-9. macro·함수·상수 선택
- 25-10. Part 25 종합 복습

### Project 진입점: Project 2

## Part 26. C 프로그램의 메모리 구조

- 26-1. C storage duration과 OS 배치의 구분
- 26-2. process virtual address space
- 26-3. text 영역
- 26-4. initialized data와 BSS
- 26-5. stack과 자동 객체
- 26-6. heap과 동적 객체
- 26-7. global·static·local·heap 객체 분석
- 26-8. `%p`로 주소 관찰
- 26-9. OS·ABI·최적화·ASLR에 따른 차이
- 26-10. Part 26 종합 복습

## Part 27. Undefined Behavior

- 27-1. Undefined Behavior
- 27-2. implementation-defined behavior
- 27-3. unspecified behavior
- 27-4. signed integer overflow
- 27-5. array out-of-bounds
- 27-6. `NULL` pointer dereference
- 27-7. dangling pointer와 object lifetime
- 27-8. uninitialized value
- 27-9. invalid pointer arithmetic
- 27-10. compiler optimization과 UB
- 27-11. compile 성공과 프로그램 정확성의 차이
- 27-12. Part 27 종합 복습

## Part 28. 디버깅

- 28-1. `-std=c17 -Wall -Wextra -Wpedantic`
- 28-2. compiler warning 읽고 수정하기
- 28-3. `-g -Og` debug build
- 28-4. AddressSanitizer build와 실행
- 28-5. ASan report 읽기와 탐지 한계
- 28-6. GDB 실행과 breakpoint
- 28-7. `step`, `next`, `continue`
- 28-8. 변수·메모리 출력
- 28-9. `backtrace`
- 28-10. `watch`
- 28-11. 결함 재현·수정·재검증
- 28-12. Part 28 종합 복습

## Part 29. C로 기본 알고리즘 구현

- 29-1. 입력·출력·경계 조건
- 29-2. Linear Search
- 29-3. Bubble Sort
- 29-4. Selection Sort
- 29-5. Insertion Sort
- 29-6. Binary Search
- 29-7. `my_strlen`
- 29-8. `my_strcpy`
- 29-9. `my_strcmp`
- 29-10. GCD
- 29-11. Prime Test
- 29-12. 배열·함수·포인터로 알고리즘 통합
- 29-13. Part 29 종합 복습

## Part 30. 자료구조

- 30-1. 자기 참조 구조체 `Node`
- 30-2. node 생성과 소유권
- 30-3. linked list 순회·검색
- 30-4. linked list insert
- 30-5. linked list delete와 head 갱신
- 30-6. linked list print
- 30-7. linked list destroy
- 30-8. stack 불변식과 `push`
- 30-9. `pop`과 빈 stack
- 30-10. `peek`
- 30-11. queue 불변식과 `enqueue`
- 30-12. `dequeue`와 마지막 node
- 30-13. allocation 실패와 구조 보존
- 30-14. 메모리 누수 검증
- 30-15. Part 30 종합 복습

### Project 진입점: Project 3

## Part 31. 시스템·임베디드 C

- 31-1. C abstract machine과 observable behavior
- 31-2. `volatile` access와 compiler optimization
- 31-3. `volatile`이 보장하지 않는 atomicity·동기화
- 31-4. memory-mapped I/O
- 31-5. 레지스터 read-modify-write와 hardware 규약
- 31-6. 대상의 fixed-width integer 제공 여부
- 31-7. bit mask·promotion·shift 경계
- 31-8. endianness와 명시적 byte 조립
- 31-9. register 자료구조의 alignment
- 31-10. padding과 member offset
- 31-11. pointer casting·alignment·aliasing
- 31-12. driver interface의 const correctness
- 31-13. driver function pointer 호환형
- 31-14. callback context의 ownership·lifetime
- 31-15. Part 31 종합 복습

### Project 진입점: Project 4

---

## Project 1. CLI Calculator

- P1-1. 기능·입력·종료 규칙
- P1-2. `scanf` 반환값과 EOF
- P1-3. 잘못된 입력과 0으로 나누기
- P1-4. 연산 함수 구현
- P1-5. 메뉴와 함수 연결
- P1-6. 반복 실행
- P1-7. warning 없는 build
- P1-8. 정상·오류·종료 경로 최종 검증

## Project 2. Student Database

- P2-1. 학생 데이터와 기능 규격
- P2-2. `main`, student, file 모듈 interface
- P2-3. 동적 학생 배열
- P2-4. Add와 중복 ID 검사
- P2-5. Delete
- P2-6. Search
- P2-7. List
- P2-8. Sort
- P2-9. 저장 파일 형식
- P2-10. Save와 Load
- P2-11. 시작 시 복원과 종료 시 정리
- P2-12. 재실행·파일 오류·다중 파일 build 최종 검증

## Project 3. Linked List Library

- P3-1. 공개 API와 ownership 계약
- P3-2. header·source·사용 예제 분리
- P3-3. pointer-to-pointer로 head 갱신
- P3-4. create와 insert
- P3-5. search와 print
- P3-6. delete
- P3-7. destroy
- P3-8. 빈 list·단일 node·연속 삭제 검증
- P3-9. allocation 실패 경로 검증
- P3-10. AddressSanitizer로 leak·memory error 최종 검증

## Project 4. Cache Simulator

- P4-1. 직접 사상 cache의 주소 폭·용량·block 크기
- P4-2. `R/W address` trace 형식
- P4-3. write-allocate와 비모델링 범위 명시
- P4-4. cache 설정 검증
- P4-5. Tag·Index·Offset bit 수
- P4-6. 주소에서 Tag·Index·Offset 추출
- P4-7. valid·tag를 가진 cache line 배열
- P4-8. hit·miss 판정과 line 갱신
- P4-9. trace file parsing
- P4-10. Accesses·Hits·Misses·Hit Rate
- P4-11. 빈 trace·충돌·경계 주소 검증
- P4-12. 선택 확장: set-associative cache
- P4-13. 선택 확장: LRU replacement
- P4-14. 수작업 기대값과 simulator 출력 최종 검증
