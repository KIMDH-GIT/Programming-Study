# Project 2. Student Database

## Project 2 개요

이 프로젝트는 학생 데이터를 동적 배열로 관리하고 파일에 저장·복원하는 프로그램을 직접 설계·구현·디버깅하는 학습 프로젝트다. 문서는 사용자에게 보이는 동작과 검증 기준만 제시하며, 실제 C 구현과 세부 설계는 학습자가 결정한다.

관련 문서:

- [프로젝트 명세](PROJECT_SPEC.md)
- [Milestones](MILESTONES.md)
- [테스트 계획](TEST_PLAN.md)
- [개발 기록](DEVLOG.md)

## 학습 목표

- 학생 데이터의 유효성, 중복 ID 정책, 각 기능의 입력·출력·오류 contract를 정의한다.
- `main`, student, file 모듈 사이의 interface와 데이터 ownership을 설계한다.
- 동적 학생 배열의 크기·용량·할당 실패·수명 경계를 관리한다.
- Add, Delete, Search, List, Sort의 정상·경계·오류 경로를 검증한다.
- 저장 형식을 명시하고 Save, Load, 시작 시 복원, 종료 시 정리를 통합한다.
- 파일 오류와 재실행을 확인하고 여러 C 파일을 warning 없이 build한다.
- 선택의 근거, 가설, 실패, 해결 과정을 개발 기록으로 남긴다.

## 실제 curriculum P2-1~P2-12

1. P2-1. 학생 데이터와 기능 규격
2. P2-2. `main`, student, file 모듈 interface
3. P2-3. 동적 학생 배열
4. P2-4. Add와 중복 ID 검사
5. P2-5. Delete
6. P2-6. Search
7. P2-7. List
8. P2-8. Sort
9. P2-9. 저장 파일 형식
10. P2-10. Save와 Load
11. P2-11. 시작 시 복원과 종료 시 정리
12. P2-12. 재실행·파일 오류·다중 파일 build 최종 검증

## 필요한 선수 Part

- Part 0~1: source·header, compile·link·실행, `main`과 종료
- Part 5, 8~10: 입출력 검증, 분기·반복, 함수 contract
- Part 11, 13~17: 배열·문자열, 포인터·배열·함수, `const`와 lifetime
- Part 18: 동적 할당, `realloc`, ownership, 해제와 동적 배열 경계
- Part 19: 구조체와 학생 등록·삭제·검색·목록·정렬
- Part 20: 상태 표현을 선택할 때의 `enum`과 `typedef`
- Part 23: 파일 열기·읽기·쓰기·닫기, 저장 형식과 오류
- Part 24~25: translation unit, 공개 interface, link, header guard

필요한 개념이 불확실하면 해당 Part를 다시 확인한 뒤 프로젝트로 돌아온다.

## 프로젝트 진행 방식

1. [PROJECT_SPEC.md](PROJECT_SPEC.md)의 behavior와 contract를 읽는다.
2. [MILESTONES.md](MILESTONES.md)의 milestone을 순서대로 진행한다.
3. 구현 전에 해당 milestone의 설계 결정과 질문에 답한다.
4. [TEST_PLAN.md](TEST_PLAN.md)의 필수 테스트와 직접 만든 테스트를 실행한다.
5. 결과와 시행착오를 [DEVLOG.md](DEVLOG.md)에 기록한다.
6. 완료 조건을 만족한 뒤에만 다음 milestone으로 이동한다.

## 직접 구현 원칙

- 학습자가 직접 설계하고 직접 C 코드를 작성한다.
- Student의 정확한 멤버, 최대 건수, 검색 기준, 함수 이름, parameter/return type을 정답으로 고정하지 않는다.
- 동적 배열은 필수지만 용량 증가 정책과 실패 처리 방식은 학습자가 정하고 검증한다.
- 메뉴 번호, 오류 표현, 저장 파일의 세부 형식, 모듈별 공개 API를 직접 결정한다.
- 선택의 장단점과 검증 방법을 기록하고, 하나의 구현 방식을 정답으로 받아들이지 않는다.

## AI 사용 원칙

AI는 기본적으로 reviewer / mentor 역할만 수행한다. AI가 먼저 완성 implementation을 제공하거나 사용자의 파일을 대신 수정하지 않는다.

막혔을 때 사용자는 먼저 다음을 수행한다.

1. 문제 상황을 기록한다.
2. compiler message 또는 실행 중 관찰한 증거를 정확히 기록한다.
3. 자신의 가설을 작성한다.
4. 이미 시도한 방법과 결과를 기록한다.
5. 그 후 필요한 경우 Hint 1을 요청한다.

힌트는 다음 단계로 요청한다.

- Hint 1: 관련 개념과 확인 지점
- Hint 2: 구조적 방향
- Hint 3: 현재 project 답안과 분리된 작은 예제
- Full solution: 다른 학습 방법이 모두 실패한 뒤의 마지막 수단

## 기본 compile rule

향후 작성하는 C 코드는 최소한 다음 baseline으로 검사한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror
```

실제 source 파일명과 output 옵션은 학습자가 선택한 다중 파일 구조에 맞게 추가한다. 완료 판정에서 warning option을 제거하거나 warning을 숨기지 않는다.

## Milestone 흐름

1. Milestone 1: 학생 데이터·기능 contract와 모듈 interface (P2-1~P2-2)
2. Milestone 2: 동적 배열과 중복 ID를 고려한 Add (P2-3~P2-4)
3. Milestone 3: Delete와 Search (P2-5~P2-6)
4. Milestone 4: List와 Sort (P2-7~P2-8)
5. Milestone 5: 저장 형식과 Save·Load (P2-9~P2-10)
6. Milestone 6: 시작·종료와 재실행·파일 오류·다중 파일 build (P2-11~P2-12)

## Milestone review 방식

Milestone 결과를 제출하면 AI는 코드를 직접 수정하지 않고 다음 순서로 검토한다.

1. compile error
2. warning
3. C17 correctness
4. Undefined Behavior (UB)
5. contract
6. boundary case
7. logic
8. lifetime / ownership
9. structure
10. readability
11. tests

각 finding에는 위치, 심각도, 문제, 이유, Hint 1만 제공한다. 완성 수정 코드나 전체 답안은 먼저 제공하지 않는다.

## Git 운영 방식

- milestone 하나가 독립적으로 검증될 때마다 작은 commit을 권장한다.
- 변경 목적에 따라 `feat:`, `fix:`, `refactor:`, `test:`, `docs:`를 구분한다.
- stage 전 `git status`와 diff를 확인한다.
- source, test, 문서를 필요한 경로만 명시적으로 stage한다.
- executable, object file, 임시 저장 파일은 commit하지 않는다.
- 실패한 milestone을 다음 milestone 변경과 한 commit에 섞지 않는다.
- push는 사용자가 별도로 결정하고 요청할 때만 수행한다.

## 완료 기준

- P2-1부터 P2-12까지의 완료 조건을 모두 만족한다.
- 빈 DB, Add·Delete·Search·List·Sort, 중복 ID 정책이 정의한 behavior대로 동작한다.
- 저장·복원, 시작·종료, 재실행과 파일 오류 경로가 데이터 contract를 지킨다.
- 모든 translation unit을 포함한 baseline build에서 warning과 error가 없다.
- [TEST_PLAN.md](TEST_PLAN.md)의 필수 테스트와 milestone별 직접 추가한 테스트가 통과한다.
- 설계 결정과 디버깅·검증 과정이 [DEVLOG.md](DEVLOG.md)에 기록되어 있다.
