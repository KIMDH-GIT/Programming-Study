# Project 4. Cache Simulator

## Project 4 개요

주소 trace를 읽어 직접 사상(direct-mapped) cache의 hit·miss를 계산하는 C17 프로그램을 학습자가 직접 설계·구현·검증한다. 이 문서들은 동작 계약과 검증 기준만 제시한다. 자료구조, 함수, 파일 구성과 구현 코드는 학습자가 결정한다.

관련 문서:

- [프로젝트 명세](PROJECT_SPEC.md)
- [Milestones](MILESTONES.md)
- [테스트 계획](TEST_PLAN.md)
- [개발 기록](DEVLOG.md)

## 학습 목표

- 주소 폭, cache 용량, block 크기로부터 직접 사상 cache의 line 수와 주소 필드를 설명한다.
- 설정을 검증한 뒤 주소의 Tag·Index·Offset과 valid·tag 상태를 이용해 hit·miss 및 충돌 교체를 추적한다.
- `R/W address` 파일을 읽고 write-allocate 규칙에 따라 접근별 상태와 통계를 검증한다.
- 작은 trace의 기대값을 손으로 먼저 계산하고 실제 출력과 비교한다.
- 모델의 규칙, 이를 구현한 소프트웨어, 실제 하드웨어 동작을 혼동하지 않는다.

## 실제 curriculum P4-1~P4-14

1. P4-1. 직접 사상 cache의 주소 폭·용량·block 크기
2. P4-2. `R/W address` trace 형식
3. P4-3. write-allocate와 비모델링 범위 명시
4. P4-4. cache 설정 검증
5. P4-5. Tag·Index·Offset bit 수
6. P4-6. 주소에서 Tag·Index·Offset 추출
7. P4-7. valid·tag를 가진 cache line 배열
8. P4-8. hit·miss 판정과 line 갱신
9. P4-9. trace file parsing
10. P4-10. Accesses·Hits·Misses·Hit Rate
11. P4-11. 빈 trace·충돌·경계 주소 검증
12. P4-12. 선택 확장: set-associative cache
13. P4-13. 선택 확장: LRU replacement
14. P4-14. 수작업 기대값과 simulator 출력 최종 검증

P4-12와 P4-13은 필수 완료 조건이 아니다. LRU는 선택한 연관 cache 확장을 진행할 때만 검토한다.

## 필요한 선수 Part

- Part 0, 2~4: C17 build, 정수 폭·범위와 unsigned 표현
- Part 11, 18~19: 배열 경계, 필요할 때의 메모리 관리, 구조체
- Part 21: unsigned 비트 연산과 shift 경계
- Part 23~24: 파일 입력·EOF, 필요하다면 여러 C 파일의 interface
- Part 27~28: Undefined Behavior, warning, 오류 재현과 디버깅
- Part 31: 시스템 모델과 실제 하드웨어에 대한 가정을 구분하기

필요한 개념이 불확실하면 해당 Part를 확인한 뒤 프로젝트로 돌아온다.

## 세 층의 구분

- **[Cache 모델]** 논리적 주소 폭, byte 단위 용량과 block 크기, 직접 사상 line, valid·tag, write-allocate, hit·miss를 정의하는 추상 규칙이다.
- **[Simulator software structure]** trace parsing, 설정 검증, 상태 보관, 통계 출력 및 오류 전달을 C17로 실현하는 프로그램의 설계 영역이다. 자료형·함수·파일 분할은 미리 정하지 않는다.
- **[실제 hardware cache]** 실제 CPU의 지연 시간, 계층, 일관성, 버스, 메모리 접근 등은 이 모델의 결과와 동일하다고 주장하지 않는다. 이 프로젝트는 특정 ISA를 대상으로 하지 않는다.

초기 완료 판정은 모델의 **기능적 정확성**과 입력·오류 계약에 따른다. simulator의 실행 속도나 메모리 사용량 최적화는 별개의 후속 과제이며 초기 완료 조건이 아니다.

## 프로젝트 진행 방식

1. [PROJECT_SPEC.md](PROJECT_SPEC.md)에서 관찰 가능한 동작과 범위를 확인한다.
2. [MILESTONES.md](MILESTONES.md)을 순서대로 진행하고 구현 전에 각 설계 질문에 답한다.
3. [TEST_PLAN.md](TEST_PLAN.md)의 기대값을 작은 입력에 대해 손으로 구하고, milestone마다 테스트 두 개 이상을 직접 추가한다.
4. 구현과 검증 결과, 실패 및 결정 이유를 [DEVLOG.md](DEVLOG.md)에 적는다.
5. 완료 조건을 검증한 뒤에만 다음 milestone으로 넘어간다.

## 직접 구현 원칙

- line과 cache의 표현, 주소와 통계의 자료형, 설정 전달 방식, parser와 함수의 책임, 오류 처리, 파일 분할을 학습자가 직접 결정한다.
- 하나의 올바른 설계를 정답으로 강요하지 않는다. 각 선택이 입력 계약, unsigned 연산의 폭과 경계, 검증 가능성을 만족하는지 설명한다.
- 예제 trace의 기대 결과는 검증 자료이지 그대로 옮겨 적을 답안 코드가 아니다.

## AI 사용 원칙

AI는 기본적으로 reviewer / mentor 역할만 한다. 먼저 완성 구현을 주거나 학습자의 파일을 대신 고치지 않는다. 막히면 문제 상황, 정확한 compiler message, 자신의 가설, 시도한 방법과 결과를 먼저 기록한다. 이후 Hint 1(개념·확인 지점), Hint 2(구조적 방향), Hint 3(프로젝트 답안과 분리된 작은 예제)를 차례로 요청한다. Full solution은 다른 학습 방법을 모두 시도한 뒤 마지막 수단이다.

## 기본 compile rule

향후 학습자가 작성하는 C 코드는 최소한 다음 baseline으로 검사한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror
```

실제 source와 output 옵션은 선택한 구조에 맞게 추가한다. warning을 숨기거나 option을 제거해 완료 판정을 통과하지 않는다.

## Milestone review 방식

Milestone 제출 시 AI는 파일을 직접 수정하지 않고 compile error, warning, C17 correctness, Undefined Behavior, contract, boundary, cache logic, lifetime, ownership, structure, readability, tests를 순서대로 검토한다. 각 finding은 위치, 심각도, 문제, 이유, Hint 1을 포함하며 완성 수정 코드를 먼저 제공하지 않는다.

## Git 운영 방식

- 독립적으로 검증한 milestone마다 작은 commit을 권장한다.
- 목적에 따라 `feat:`, `fix:`, `refactor:`, `test:`, `docs:`를 구분한다.
- stage 전 `git status`와 diff를 확인하고 필요한 경로만 명시적으로 stage한다.
- 실행 파일, object 파일, 임시 trace와 빌드 산출물은 commit하지 않는다.
- 실패한 milestone과 다음 milestone의 변경을 한 commit에 섞지 않는다.
- push는 학습자가 별도로 결정하고 요청할 때만 수행한다.

## 완료 기준

- 필수 P4-1~P4-11, P4-14의 동작과 검증을 만족한다. P4-12·P4-13은 선택 사항이다.
- 빈 trace, 충돌, 경계 주소, invalid 설정·trace와 write-allocate 동작이 정의된 계약과 일치한다.
- 수작업 주소 필드·접근 결과·통계가 simulator 결과와 일치한다.
- strict C17 baseline build에 warning과 error가 없으며 필수 및 직접 추가한 테스트가 통과한다.
- 결정 이유와 검증 기록을 개발 기록에 남긴다. simulator 성능 최적화는 초기 완료 조건이 아니다.
