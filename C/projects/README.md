# C Projects

## 목적

이 디렉토리는 C curriculum의 Project 1~4를 사용자가 직접 설계·구현·디버깅하기 위한 자율 프로젝트 환경이다. 각 project 폴더의 문서는 요구 behavior와 검증 기준만 제공하며 완성 implementation이나 reference solution을 제공하지 않는다.

## Projects

| Project | Curriculum title | 학습 목적 | Scaffold | Implementation |
|---|---|---|---|---|
| [Project 1](01-cli-calculator/README.md) | CLI Calculator | 작은 CLI 프로그램의 입력·연산·오류·종료 경로를 완성한다. | READY | NOT STARTED |
| [Project 2](02-student-database/README.md) | Student Database | 여러 데이터의 상태 변화, 동적 저장 공간, file persistence를 가진 application을 설계한다. | READY | NOT STARTED |
| [Project 3](03-linked-list-library/README.md) | Linked List Library | pointer, dynamic allocation, ownership, 공개 API를 가진 library를 설계한다. | READY | NOT STARTED |
| [Project 4](04-cache-simulator/README.md) | Cache Simulator | C, bit operation, file input, state와 cache model을 통합한 simulator를 설계한다. | READY | NOT STARTED |

## 권장 진행 순서와 난이도 상승

```text
Project 1
작은 CLI 프로그램 완성 경험
        ↓
Project 2
여러 데이터와 persistence를 가진 application
        ↓
Project 3
pointer / ownership 기반 library
        ↓
Project 4
C + computer architecture 통합 simulator
```

각 project는 앞 단계의 경험을 활용하지만, 다음 project를 시작하기 전에 현재 project의 필수 완료 기준과 회고를 끝내는 것을 권장한다.

## 현재 상태

- [ ] Project 1 implementation
- [ ] Project 2 implementation
- [ ] Project 3 implementation
- [ ] Project 4 implementation

모든 project scaffold는 준비되어 있지만 실제 implementation은 시작되지 않았다. 준비 문서 존재와 구현 완료를 같은 상태로 보지 않는다.

## 프로젝트 공통 진행 방법

각 project 폴더에서 다음 순서로 진행한다.

```text
PROJECT_SPEC
      ↓
MILESTONES
      ↓
현재 milestone 선택
      ↓
사용자 직접 설계
      ↓
사용자 직접 구현
      ↓
직접 compile / debug
      ↓
TEST_PLAN 검증
      ↓
DEVLOG 작성
      ↓
Git commit
      ↓
필요할 때 AI review
```

1. `README.md`에서 curriculum 범위와 진행 원칙을 확인한다.
2. `PROJECT_SPEC.md`에서 필수 behavior, contract, invariant를 확인한다.
3. `MILESTONES.md`에서 현재 단계 하나만 선택한다.
4. 구현 전에 설계 선택과 이유를 `DEVLOG.md`에 기록한다.
5. 사용자가 직접 구현하고 strict C17 baseline으로 build한다.
6. `TEST_PLAN.md`의 필수 test와 직접 추가한 test를 실행한다.
7. 완료 조건을 만족한 milestone만 작은 Git commit으로 남긴다.

## AI 사용 원칙

AI는 기본적으로 reviewer / mentor이며 implementation을 대신 작성하지 않는다.

막혔을 때 사용자는 먼저 다음을 기록한다.

1. 문제 상황
2. compiler 또는 runtime 증거
3. 자신의 가설
4. 이미 시도한 것과 결과

그 후 필요한 최소 단계의 hint를 요청한다.

- Hint 1: 관련 개념과 확인 위치
- Hint 2: 구조적 방향
- Hint 3: project 답안과 분리된 작은 독립 예제
- Full solution: 마지막 수단

Milestone review에서 AI는 파일을 바로 수정하지 않고 위치, 심각도, 문제, 이유, Hint 1을 기본으로 제공한다.

## Git 진행 원칙

- milestone 하나의 검증 가능한 변화만 한 commit에 담는다.
- 변경 목적에 맞게 `feat:`, `fix:`, `refactor:`, `test:`, `docs:`를 구분한다.
- stage 전에 `git status`와 diff를 확인하고 필요한 파일만 경로로 지정한다.
- executable, object file, library binary, sanitizer log, 임시 파일을 commit하지 않는다.
- 실패한 실험과 다음 milestone의 기능을 한 commit에 섞지 않는다.
- push는 사용자가 history와 remote 상태를 확인한 뒤 별도로 결정한다.
