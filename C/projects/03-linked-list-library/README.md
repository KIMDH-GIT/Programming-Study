# Project 3. Linked List Library

## Project 3 개요

동적 node의 연결과 소유권을 직접 설계·구현·검증하는 C17 linked list library 학습 프로젝트다. 문서는 요구사항과 검증 기준만 제공하며, API의 구체적인 모양과 C 구현은 학습자가 결정한다.

관련 문서:

- [프로젝트 명세](PROJECT_SPEC.md)
- [Milestones](MILESTONES.md)
- [테스트 계획](TEST_PLAN.md)
- [개발 기록](DEVLOG.md)

## 학습 목표

- 공개 API의 호출 조건, 결과, 메모리 소유권과 수명을 명확히 설명한다.
- header, library source, 독립적인 사용 예제를 분리해 빌드한다.
- pointer-to-pointer를 사용해 첫 node의 변경을 호출자에게 반영한다.
- create, insert, search, print, delete, destroy의 정상·경계·실패 경로를 검증한다.
- 연결 불변식과 메모리 안전성을 warning 및 AddressSanitizer로 확인한다.

## 실제 curriculum P3-1~P3-10

1. P3-1. 공개 API와 ownership 계약
2. P3-2. header·source·사용 예제 분리
3. P3-3. pointer-to-pointer로 head 갱신
4. P3-4. create와 insert
5. P3-5. search와 print
6. P3-6. delete
7. P3-7. destroy
8. P3-8. 빈 list·단일 node·연속 삭제 검증
9. P3-9. allocation 실패 경로 검증
10. P3-10. AddressSanitizer로 leak·memory error 최종 검증

## 필요한 선수 Part

- Part 0, 24, 25: translation unit, header, link와 header guard
- Part 10, 14, 16, 17: 함수 계약, 포인터, pointer-to-pointer, 읽기 전용 접근
- Part 18: 동적 할당, 실패, ownership, 누수와 해제 후 접근
- Part 19, 30: 자기 참조 구조체와 linked list의 연결 불변식
- Part 27, 28: Undefined Behavior, warning, AddressSanitizer

필요한 개념이 불확실하면 해당 Part를 다시 확인한 뒤 프로젝트로 돌아온다.

## 프로젝트 진행 방식

1. [PROJECT_SPEC.md](PROJECT_SPEC.md)의 API·ownership 계약을 읽고 아직 열린 설계를 직접 결정한다.
2. [MILESTONES.md](MILESTONES.md)의 설계 질문에 답한 뒤 한 단계씩 구현한다.
3. [TEST_PLAN.md](TEST_PLAN.md)의 필수 사례와 milestone별 직접 만든 테스트를 실행한다.
4. 컴파일·실행·검사 결과와 시행착오를 [DEVLOG.md](DEVLOG.md)에 기록한다.
5. 완료 조건을 확인한 뒤 다음 milestone으로 이동한다.

## 직접 구현 원칙

- 학습자가 공개 API와 내부 표현을 직접 설계하고 C 코드를 직접 작성한다.
- List wrapper 사용 여부, head만 둘지 tail도 둘지, 크기 저장 여부를 직접 결정한다.
- 함수 이름·시그니처·반환 상태·출력 매개변수·node의 필수 연결 외 필드는 정답으로 고정하지 않는다.
- 다만 header·source·사용 예제의 분리와 pointer-to-pointer를 통한 head 갱신은 실제 curriculum 요구다.
- 선택의 장단점, 호출자가 지킬 계약과 검증 방법을 기록한다.

## AI 사용 원칙

AI는 기본적으로 reviewer / mentor 역할이다. AI가 먼저 파일을 수정하거나 완성 구현을 제공하지 않는다. 막히면 문제 상황, 정확한 compiler 또는 sanitizer 메시지, 자신의 가설, 시도와 결과를 먼저 적고 Hint 1을 요청한다.

- Hint 1: 관련 개념과 확인 지점
- Hint 2: 구조적 방향
- Hint 3: 프로젝트 답안과 분리된 작은 예제
- Full solution: 다른 학습 방법이 모두 실패한 뒤의 마지막 수단

## 기본 compile rule

향후 학습자가 작성할 C 코드는 최소한 다음 strict C17 baseline으로 library source와 사용 예제를 함께 검사한다.

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror
```

실제 source 경로와 출력 옵션은 자신이 선택한 구조에 맞게 추가한다. warning option을 제거하거나 warning을 숨겨 완료로 처리하지 않는다.

## Milestone 진행

계약과 파일 분리 → head 갱신·생성·삽입 → 검색·출력 → 삭제·전체 정리 → 경계·할당 실패 → strict build·ASan 최종 검증 순서로 진행한다. 각 milestone의 독립 완료 조건과 설계 질문은 [MILESTONES.md](MILESTONES.md)에 있다.

## Milestone review 방식

제출된 코드는 AI가 먼저 수정하지 않고 compile error, warning, C17 correctness, Undefined Behavior, API contract, logic, lifetime, ownership, structure, memory leak, use-after-free, double-free, dangling pointer, lost pointer, 잘못된 link 갱신, 빈 list·단일 node boundary, readability, tests 순서로 검토한다. 각 finding은 **위치 / 심각도 / 문제 / 이유 / Hint 1** 형식으로 보고하며 완성 수정 코드를 먼저 제시하지 않는다.

## Git 운영 방식

- milestone별 독립 검증 후 작은 commit을 권장한다. `feat:`, `fix:`, `refactor:`, `test:`, `docs:`를 목적에 맞게 구분한다.
- stage 전 `git status`와 diff를 확인하고 필요한 source·test·문서 경로만 명시적으로 stage한다.
- executable, object file, 검사 결과물은 commit하지 않는다. 실패한 단계와 다음 단계를 한 commit에 섞지 않는다.
- push는 학습자가 별도로 결정한다. 이 문서 scaffold 작성에서는 stage나 commit을 하지 않는다.

## 완료 기준

- P3-1부터 P3-10까지의 계약·구현·검증 기록이 대응한다.
- 별도 사용 예제가 공개 header만으로 library를 빌드·사용하고 정의된 결과를 확인한다.
- 빈 list, 단일 node, 연속 삭제, 할당 실패에서도 연결·ownership 계약이 유지된다.
- strict C17 baseline이 warning 없이 통과하고 ASan 실행에서 탐지 가능한 leak·memory error가 없다.
- 필수 테스트와 milestone별 직접 추가한 테스트가 통과하고 설계 결정·디버깅 결과가 `DEVLOG.md`에 남아 있다.
