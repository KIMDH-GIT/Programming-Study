# Project 1. CLI Calculator

## Project 1 개요

이 프로젝트는 명령줄에서 사칙연산을 수행하는 계산기를 직접 설계·구현·디버깅하는 학습 프로젝트다. 문서는 요구사항과 검증 기준만 제시하며, 실제 C 구현과 세부 설계는 학습자가 결정한다.

관련 문서:

- [프로젝트 명세](PROJECT_SPEC.md)
- [Milestones](MILESTONES.md)
- [테스트 계획](TEST_PLAN.md)
- [개발 기록](DEVLOG.md)

## 학습 목표

- 사용자 관점의 입력·출력·종료 contract를 먼저 정의한다.
- 입력 성공, 잘못된 입력, EOF를 구분해 처리한다.
- 0으로 나누기를 포함한 오류 경로를 정상 경로와 분리한다.
- 연산, 메뉴, 반복 실행을 스스로 구조화한다.
- compiler warning을 모두 해결하고 정상·오류·종료 경로를 직접 검증한다.
- 구현 과정의 가설, 실패, 해결 과정을 개발 기록으로 남긴다.

## 실제 curriculum P1-1~P1-8

1. P1-1. 기능·입력·종료 규칙
2. P1-2. `scanf` 반환값과 EOF
3. P1-3. 잘못된 입력과 0으로 나누기
4. P1-4. 연산 함수 구현
5. P1-5. 메뉴와 함수 연결
6. P1-6. 반복 실행
7. P1-7. warning 없는 build
8. P1-8. 정상·오류·종료 경로 최종 검증

## 필요한 선수 Part

- Part 0: source를 compile하고 executable을 실행하는 과정
- Part 5: 표준 입출력, 입력 함수 반환값, 입력 검증
- Part 6~7: 형 변환, 산술 연산, 나눗셈 경계
- Part 8: 조건에 따른 흐름 제어
- Part 9: 반복 실행과 종료 조건
- Part 10: 함수의 contract, parameter, return value

필요한 개념이 불확실하면 해당 Part를 다시 확인한 뒤 프로젝트로 돌아온다.

## 프로젝트 진행 방식

1. [PROJECT_SPEC.md](PROJECT_SPEC.md)의 behavior와 contract를 읽는다.
2. [MILESTONES.md](MILESTONES.md)을 한 단계씩 진행한다.
3. 구현 전에 해당 milestone의 설계 질문에 답한다.
4. [TEST_PLAN.md](TEST_PLAN.md)의 요구 테스트와 직접 만든 테스트를 실행한다.
5. 결과와 시행착오를 [DEVLOG.md](DEVLOG.md)에 기록한다.
6. 완료 조건을 만족한 뒤에만 다음 milestone으로 이동한다.

## 직접 구현 원칙

- 학습자가 직접 설계하고 직접 C 코드를 작성한다.
- 함수 이름, 함수 개수, parameter/return type, `main` 구조는 미리 정답으로 고정하지 않는다.
- `scanf`와 `fgets` 중 무엇을 사용할지, parsing을 어떻게 구성할지 직접 결정한다.
- `switch` 사용 여부, 메뉴의 세부 구조, 오류 상태 표현 방식을 직접 결정한다.
- 한 파일과 여러 파일 중 어느 구성이 현재 단계에 적절한지 판단한다.
- 선택에는 장단점과 검증 방법을 기록하되, 익숙한 방식만을 이유로 결정하지 않는다.

## AI 사용 원칙

AI는 기본적으로 reviewer / mentor 역할만 수행한다. AI가 먼저 완성 implementation을 제공하거나 사용자의 파일을 대신 수정하지 않는다.

막혔을 때 사용자는 먼저 다음을 수행한다.

1. 문제 상황을 기록한다.
2. compiler message를 정확히 확인한다.
3. 자신의 가설을 작성한다.
4. 이미 시도한 방법과 결과를 기록한다.
5. 그 후 필요한 경우 Hint 1을 요청한다.

힌트는 다음 단계로 요청한다.

- Hint 1: 관련 개념과 확인 지점
- Hint 2: 구조적 방향
- Hint 3: 현재 코드와 분리된 작은 예제
- Full solution: 다른 학습 방법이 모두 실패한 뒤의 마지막 수단

## 기본 compile rule

향후 작성하는 C 코드는 최소한 다음 baseline으로 검사한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror
```

실제 source 파일명과 output 옵션은 학습자가 선택한 project 구조에 맞게 추가한다. 완료 판정에서 warning option을 제거하거나 warning을 숨기지 않는다.

## Milestone review 방식

Milestone 결과를 제출하면 AI는 코드를 직접 수정하지 않고 다음 순서로 검토한다.

1. compile error
2. warning
3. C17 correctness
4. Undefined Behavior
5. 입력 contract
6. boundary case
7. logic
8. 함수/모듈 구조
9. readability
10. test coverage

각 finding의 기본 형식은 다음과 같다.

- 위치
- 심각도
- 문제
- 이유
- Hint 1

완성 수정 코드나 전체 답안은 먼저 제공하지 않는다.

## Git 운영 방식

- milestone 하나가 독립적으로 검증될 때마다 작은 commit을 권장한다.
- 변경 목적에 따라 `feat:`, `fix:`, `refactor:`, `test:`, `docs:`를 구분한다.
- stage 전 `git status`와 diff를 확인한다.
- source, test, 문서를 필요한 경로만 명시적으로 stage한다.
- executable, object file, 임시 파일은 commit하지 않는다.
- 실패한 milestone을 다음 milestone 변경과 한 commit에 섞지 않는다.
- push는 사용자가 별도로 결정하고 요청할 때만 수행한다.

## 완료 기준

- P1-1부터 P1-8까지의 완료 조건을 모두 만족한다.
- 필수 정상·오류·EOF·종료 경로가 정의된 behavior대로 동작한다.
- 반복 실행 중 이전 입력이나 오류 상태가 다음 계산을 오염시키지 않는다.
- baseline compile에서 warning과 error가 없다.
- [TEST_PLAN.md](TEST_PLAN.md)의 필수 테스트와 직접 추가한 테스트가 통과한다.
- 설계 결정과 디버깅 과정이 [DEVLOG.md](DEVLOG.md)에 기록되어 있다.
