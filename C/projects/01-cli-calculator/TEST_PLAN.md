# Test Plan

## 테스트 원칙

- test는 특정 함수 이름이나 control-flow 구현이 아니라 사용자에게 보이는 behavior를 검증한다.
- exact prompt 문구보다 입력의 분류, 결과, 오류, 종료 여부를 우선 확인한다.
- 각 test는 다른 test의 실행 순서나 이전 상태에 의존하지 않게 준비한다.
- 선택한 숫자 표현과 입력 contract가 정해지면 입력 표기를 그 contract에 맞게 구체화한다.
- 기대 behavior를 만족시키는 구현 코드는 이 문서에 제공하지 않는다.

## 필수 Test Cases

### T-001 — 정상 덧셈

- Test ID: T-001
- 목적: 기본 정상 계산 경로를 확인한다.
- 입력 또는 조건: 덧셈 선택과 유효한 두 피연산자
- 기대 behavior: 두 피연산자의 합이 정상 결과로 제공되고 다음 행동을 선택할 수 있다.
- 검증 이유: 가장 작은 end-to-end 정상 경로다.
- 관련 milestone: Milestone 1, Milestone 3

### T-002 — 피연산자 순서

- Test ID: T-002
- 목적: 순서에 민감한 연산의 operand mapping을 확인한다.
- 입력 또는 조건: 서로 다른 두 값으로 뺄셈 또는 나눗셈 요청
- 기대 behavior: 입력 contract에서 정의한 첫째·둘째 피연산자 순서가 결과에 반영된다.
- 검증 이유: 메뉴 연결이나 argument 전달 순서 오류를 발견한다.
- 관련 milestone: Milestone 1, Milestone 3

### T-003 — 숫자가 아닌 입력

- Test ID: T-003
- 목적: 변환 실패를 정상 입력과 구분하는지 확인한다.
- 입력 또는 조건: 숫자를 요구하는 지점에 숫자로 해석할 수 없는 입력
- 기대 behavior: 계산 결과를 만들지 않고 정의한 오류 또는 복구 behavior를 보인다.
- 검증 이유: 입력 함수 반환값을 무시하는 결함을 발견한다.
- 관련 milestone: Milestone 2

### T-004 — 입력 시작 전 EOF

- Test ID: T-004
- 목적: 연산 선택 단계의 EOF 종료를 확인한다.
- 입력 또는 조건: 새 요청을 시작하는 입력에서 EOF
- 기대 behavior: 이전 값을 사용하거나 무한 반복하지 않고 정의한 종료 behavior를 보인다.
- 검증 이유: EOF를 일반 변환 실패와 혼동하는 결함을 발견한다.
- 관련 milestone: Milestone 2, Milestone 4

### T-005 — 피연산자 입력 중 EOF

- Test ID: T-005
- 목적: 일부 입력만 받은 상태의 EOF 처리를 확인한다.
- 입력 또는 조건: 연산은 선택했지만 필요한 피연산자가 모두 입력되기 전에 EOF
- 기대 behavior: 부분 입력으로 계산하지 않고 안전하게 종료한다.
- 검증 이유: stale value 또는 초기화되지 않은 값 사용을 방지한다.
- 관련 milestone: Milestone 2

### T-006 — 지원하지 않는 연산

- Test ID: T-006
- 목적: invalid operator를 거부하는지 확인한다.
- 입력 또는 조건: contract에 없는 연산 선택
- 기대 behavior: 필수 연산을 임의로 실행하지 않고 정의한 오류 또는 재입력 behavior를 보인다.
- 검증 이유: menu mapping의 default/error 경로를 검증한다.
- 관련 milestone: Milestone 3, Milestone 4

### T-007 — 0으로 나누기

- Test ID: T-007
- 목적: division-by-zero를 평가 전에 차단하는지 확인한다.
- 입력 또는 조건: 나눗셈 요청에서 divisor가 0
- 기대 behavior: 나눗셈을 실행하지 않고 정상 결과와 구분되는 오류 behavior를 보인다.
- 검증 이유: Undefined Behavior 또는 잘못된 부동소수점 가정을 방지한다.
- 관련 milestone: Milestone 3

### T-008 — 음수 피연산자

- Test ID: T-008
- 목적: 선택한 숫자 contract에서 negative value 처리를 확인한다.
- 입력 또는 조건: 지원 범위 안의 음수를 포함한 유효한 계산
- 기대 behavior: 문서화한 부호와 연산 규칙에 맞는 결과를 제공한다.
- 검증 이유: 입력 format, 형 변환, 부호 처리 오류를 발견한다.
- 관련 milestone: Milestone 3

### T-009 — 0이 포함된 정상 연산

- Test ID: T-009
- 목적: 0 자체와 0으로 나누기 오류를 구분한다.
- 입력 또는 조건: 덧셈·뺄셈·곱셈의 피연산자 또는 나눗셈의 dividend가 0
- 기대 behavior: divisor 0이 아닌 경우 선택한 연산 규칙에 따라 정상 결과를 제공한다.
- 검증 이유: 모든 0을 오류로 처리하는 과도한 validation을 발견한다.
- 관련 milestone: Milestone 3

### T-010 — 지원 범위 경계 근처

- Test ID: T-010
- 목적: 학습자가 정의한 숫자 범위와 연산 경계를 확인한다.
- 입력 또는 조건: 선택한 표현에서 지원 범위의 경계에 가까운 유효한 값
- 기대 behavior: 문서화한 범위 안에서는 정의된 결과를 제공하고 Undefined Behavior에 의존하지 않는다.
- 검증 이유: 작은 정상 값만으로 발견되지 않는 범위 문제를 드러낸다.
- 관련 milestone: Milestone 3, Milestone 5

### T-011 — 반복 계산

- Test ID: T-011
- 목적: 여러 요청의 상태가 분리되는지 확인한다.
- 입력 또는 조건: 서로 다른 연산을 두 번 이상 연속 수행
- 기대 behavior: 각 결과는 해당 요청의 입력만 반영하고 프로그램은 정의한 시점까지 계속된다.
- 검증 이유: 이전 operand, operator, error status 재사용을 발견한다.
- 관련 milestone: Milestone 4

### T-012 — 오류 뒤 정상 계산

- Test ID: T-012
- 목적: 오류 복구 후 다음 요청이 깨끗하게 처리되는지 확인한다.
- 입력 또는 조건: 잘못된 입력 또는 invalid operator 뒤에 유효한 계산 입력
- 기대 behavior: 오류 정책에 따라 계속하기로 했다면 다음 정상 계산이 이전 오류의 영향을 받지 않는다.
- 검증 이유: input buffer와 반복 상태 복구를 검증한다.
- 관련 milestone: Milestone 4, Milestone 5

### T-013 — 명시적 종료

- Test ID: T-013
- 목적: 사용자의 종료 요청을 확인한다.
- 입력 또는 조건: contract에서 정의한 종료 선택
- 기대 behavior: 추가 연산이나 피연산자를 요구하지 않고 정상 종료한다.
- 검증 이유: 종료 선택이 invalid operator 또는 계산 요청으로 처리되는 결함을 발견한다.
- 관련 milestone: Milestone 1, Milestone 4

### T-014 — warning-free build

- Test ID: T-014
- 목적: 최종 source가 strict baseline을 통과하는지 확인한다.
- 입력 또는 조건: clean 상태에서 `gcc -std=c17 -Wall -Wextra -Wpedantic -Werror` baseline으로 build
- 기대 behavior: compile error와 warning 없이 executable이 생성된다.
- 검증 이유: type, format, declaration, control-flow 문제를 completion 전에 차단한다.
- 관련 milestone: Milestone 5

### T-015 — 최종 경로 회귀

- Test ID: T-015
- 목적: 정상·오류·종료 경로를 최종 상태에서 함께 검증한다.
- 입력 또는 조건: 정상 계산, 잘못된 입력, 0으로 나누기, 반복 계산, 종료를 포함한 독립 scenario
- 기대 behavior: 각 scenario가 문서화한 결과를 재현하고 서로 상태를 오염시키지 않는다.
- 검증 이유: 개별 기능 통과 후 통합 과정에서 생기는 regression을 발견한다.
- 관련 milestone: Milestone 5

## Milestone별 사용자가 직접 추가할 테스트

### Milestone 1

사용자가 직접 추가해야 할 테스트: 최소 2개. 자신이 정의한 정상 입력과 종료 contract에서 누락되기 쉬운 경우를 찾는다.

### Milestone 2

사용자가 직접 추가해야 할 테스트: 최소 2개. 입력 실패 위치와 EOF 발생 위치를 바꾸어 behavior를 예측한다.

### Milestone 3

사용자가 직접 추가해야 할 테스트: 최소 2개. 선택한 숫자 표현의 부호, 범위, 나눗셈 특성에서 경계를 찾는다.

### Milestone 4

사용자가 직접 추가해야 할 테스트: 최소 2개. 정상과 오류 요청의 순서를 조합해 상태 오염 가능성을 찾는다.

### Milestone 5

사용자가 직접 추가해야 할 테스트: 최소 2개. 기존 test가 놓친 전체 경로 조합과 regression 가능성을 찾는다.
