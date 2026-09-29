# Test Plan

## 테스트 원칙

- 테스트는 특정 함수 이름·node 필드가 아닌 학습자가 문서화한 공개 API의 관찰 가능한 behavior를 검증한다.
- 매 사례는 독립된 초기 상태로 시작하고 소유한 자원을 정리한다. 출력 형식과 중복·재사용 정책을 먼저 계약에 기록한다.
- 실패 주입은 할당 실패가 실제로 일어난 지점을 확인해야 한다. 단순한 `NULL` 인자 전달은 allocation 실패 테스트가 아니다.
- 검증용 C 구현이나 정답 함수는 이 문서에 제공하지 않는다.

## 필수 Test Cases

### T-001 — 공개 client 빌드

- Test ID: T-001
- 목적: header·source·사용 예제 분리를 검증한다.
- 입력 또는 초기 상태: 공개 header만 포함한 별도 client와 library source.
- 수행 동작: strict C17 baseline으로 함께 compile·link하고 실행한다.
- 기대 behavior: warning 없이 빌드되고 공개 API 호출이 연결·실행된다.
- 검증 이유: 내부 source 포함, 누락된 선언 또는 정의 오류를 드러낸다.
- 관련 milestone: Milestone 1, Milestone 6

### T-002 — 빈 list 관찰

- Test ID: T-002
- 목적: 초기 빈 상태 계약을 확인한다.
- 입력 또는 초기 상태: head가 빈 list.
- 수행 동작: search와 print를 호출한다.
- 기대 behavior: 찾지 못함과 계약상 빈 출력이 구분되며 head는 그대로 비어 있다.
- 검증 이유: 빈 list 역참조와 출력의 무정의 상태를 찾는다.
- 관련 milestone: Milestone 1, Milestone 3, Milestone 5

### T-003 — 첫 node 생성·삽입

- Test ID: T-003
- 목적: pointer-to-pointer head 갱신을 확인한다.
- 입력 또는 초기 상태: 빈 list와 유효한 값.
- 수행 동작: create·insert 경로로 첫 node를 추가한다.
- 기대 behavior: 호출자의 head가 새 node를 나타내고 값이 검색된다.
- 검증 이유: 지역 head만 바뀌는 오류를 찾는다.
- 관련 milestone: Milestone 2

### T-004 — 여러 node 삽입과 순서

- Test ID: T-004
- 목적: 기존 연결을 잃지 않는지 확인한다.
- 입력 또는 초기 상태: 구분되는 값 여러 개와 삽입 위치 계약.
- 수행 동작: 반복 삽입한 뒤 검색·출력한다.
- 기대 behavior: 모든 값이 도달 가능하고 출력 순서가 계약과 일치한다.
- 검증 이유: lost pointer와 잘못된 link 갱신을 찾는다.
- 관련 milestone: Milestone 2, Milestone 3

### T-005 — 첫·중간·마지막 검색

- Test ID: T-005
- 목적: 전체 순회와 결과 수명을 확인한다.
- 입력 또는 초기 상태: 구분되는 값이 세 개 이상 있는 list.
- 수행 동작: 각 위치의 값을 검색한다.
- 기대 behavior: 세 위치 모두 찾고 검색 전후 list가 같다. 결과 사용은 명시한 수명 안에서만 이뤄진다.
- 검증 이유: 순회 경계 누락과 읽기 동작의 상태 변형을 찾는다.
- 관련 milestone: Milestone 3

### T-006 — 없는 대상 검색

- Test ID: T-006
- 목적: 찾지 못함을 정상 값과 구분한다.
- 입력 또는 초기 상태: 대상이 없는 비어 있지 않은 list.
- 수행 동작: search를 호출한다.
- 기대 behavior: 계약상 없음이 보고되고 연결은 그대로다.
- 검증 이유: 실패를 찾음으로 잘못 보고하는 경우를 찾는다.
- 관련 milestone: Milestone 3

### T-007 — 출력과 상태 보존

- Test ID: T-007
- 목적: 출력 결과와 비변경 계약을 확인한다.
- 입력 또는 초기 상태: 여러 node가 있는 list.
- 수행 동작: print 전후 검색·출력 결과를 비교한다.
- 기대 behavior: 정한 형식과 순서가 유지되고 node 집합과 head가 변하지 않는다.
- 검증 이유: 순회 중 연결을 변경하는 결함을 찾는다.
- 관련 milestone: Milestone 3

### T-008 — 첫 node 삭제

- Test ID: T-008
- 목적: 삭제 시 호출자의 head 갱신을 확인한다.
- 입력 또는 초기 상태: 여러 node가 있는 list.
- 수행 동작: 첫 node를 삭제한다.
- 기대 behavior: 다음 node가 새 head가 되고 대상은 찾지 못하며 나머지는 도달 가능하다.
- 검증 이유: dangling head와 첫 연결 유실을 찾는다.
- 관련 milestone: Milestone 4

### T-009 — 중간 node 삭제

- Test ID: T-009
- 목적: 양옆 연결 보존을 확인한다.
- 입력 또는 초기 상태: 세 개 이상의 구분되는 node.
- 수행 동작: 중간 node를 삭제한다.
- 기대 behavior: 앞뒤 node가 계약상 순서로 이어지고 대상은 찾지 못한다.
- 검증 이유: 잘못된 link 갱신과 유실된 suffix를 찾는다.
- 관련 milestone: Milestone 4

### T-010 — 마지막 node 삭제

- Test ID: T-010
- 목적: 끝 연결의 정리를 확인한다.
- 입력 또는 초기 상태: 두 개 이상의 node.
- 수행 동작: 마지막 node를 삭제한다.
- 기대 behavior: 남은 끝 node 뒤에서 순회가 끝나고 삭제 대상은 없다.
- 검증 이유: 마지막 연결의 dangling pointer를 찾는다.
- 관련 milestone: Milestone 4

### T-011 — 없는 대상 삭제

- Test ID: T-011
- 목적: 삭제 실패와 기존 list 보존을 확인한다.
- 입력 또는 초기 상태: 대상이 없는 list.
- 수행 동작: 대상 삭제를 요청한다.
- 기대 behavior: 계약상 없음 결과가 제공되고 기존 연결·소유권은 변하지 않는다.
- 검증 이유: 잘못된 node 해제와 실패의 성공 위장을 찾는다.
- 관련 milestone: Milestone 4, Milestone 5

### T-012 — 빈 list와 단일 node 삭제

- Test ID: T-012
- 목적: 최소 크기 경계를 확인한다.
- 입력 또는 초기 상태: 빈 list 및 하나의 node만 있는 별도 list.
- 수행 동작: 빈 list의 삭제와 단일 node 삭제를 각각 수행한다.
- 기대 behavior: 빈 경우 계약상 결과를 반환하고 단일 node 삭제 뒤 호출자 head는 빈 상태다.
- 검증 이유: `NULL` 역참조와 마지막 node 해제 뒤 dangling head를 찾는다.
- 관련 milestone: Milestone 4, Milestone 5

### T-013 — 연속 삭제와 재삽입

- Test ID: T-013
- 목적: 반복 조작의 연결 불변식을 확인한다.
- 입력 또는 초기 상태: 여러 구분되는 node.
- 수행 동작: 첫·중간·마지막 삭제를 연속 실행한 뒤 다시 삽입한다.
- 기대 behavior: 매 단계의 남은 값과 순서가 맞고 재삽입 값도 도달 가능하다.
- 검증 이유: 한 번의 조작만으로 드러나지 않는 상태 오염을 찾는다.
- 관련 milestone: Milestone 5

### T-014 — destroy와 해제 후 정책

- Test ID: T-014
- 목적: 전체 ownership 정리와 재사용 계약을 확인한다.
- 입력 또는 초기 상태: 여러 node 및 빈 list의 독립 사례.
- 수행 동작: destroy 뒤 호출자 head를 확인하고 문서화한 post-destroy 재사용 정책을 시험한다.
- 기대 behavior: 모든 소유 node가 정리되고 head가 빈 상태다. 재사용을 허용했다면 새 삽입이 동작하고, 허용하지 않았다면 계약상 필요한 재초기화 뒤에만 사용한다.
- 검증 이유: 누수·이중 해제와 해제된 결과 재사용 위험을 드러낸다.
- 관련 milestone: Milestone 4, Milestone 5

### T-015 — allocation 실패

- Test ID: T-015
- 목적: 실패의 관찰과 기존 상태 보존을 확인한다.
- 입력 또는 초기 상태: 빈 list와 여러 node list를 별도로 준비하고 선택한 seam으로 실제 할당 실패를 재현한다.
- 수행 동작: 각각 create·insert 실패 경로를 실행하고 기존 값 검색·출력 후 destroy한다.
- 기대 behavior: 실패가 성공과 구분되고 기존 node는 모두 도달 가능하며 새 node나 임시 자원은 누수되지 않는다.
- 검증 이유: 실패 중 head 유실, 부분 연결, 메모리 누수를 찾는다.
- 관련 milestone: Milestone 2, Milestone 5

### T-016 — ASan 최종 실행

- Test ID: T-016
- 목적: 최종 메모리 오류와 leak을 검사한다.
- 입력 또는 초기 상태: 독립 client에 정상·빈 list·단일 node·연속 삭제·할당 실패 시나리오를 준비한다.
- 수행 동작: strict C17 빌드에 `-g -fsanitize=address -fno-omit-frame-pointer`를 더해 compile·link하고 누수 검사가 지원되는 환경에서 실행한다.
- 기대 behavior: 각 시나리오가 계약대로 끝나며 ASan에서 memory error 또는 leak 보고가 없다. 누수 검사가 지원되지 않으면 그 한계를 기록하고 지원 환경에서 다시 확인한다.
- 검증 이유: use-after-free, double-free, 접근 오류 및 누락된 해제의 최종 회귀 검사다.
- 관련 milestone: Milestone 5, Milestone 6

## Milestone별 직접 추가 테스트

### Milestone 1

사용자가 직접 추가할 테스트: 최소 2개. 공개 계약과 별도 client의 사용 경계를 살핀다.

### Milestone 2

사용자가 직접 추가할 테스트: 최소 2개. head 갱신과 삽입 위치의 경계를 살핀다.

### Milestone 3

사용자가 직접 추가할 테스트: 최소 2개. 검색 결과와 출력의 비변경 계약을 살핀다.

### Milestone 4

사용자가 직접 추가할 테스트: 최소 2개. 삭제 위치와 소유권 종료를 살핀다.

### Milestone 5

사용자가 직접 추가할 테스트: 최소 2개. 연속 상태 변화와 실제 할당 실패를 살핀다.

### Milestone 6

사용자가 직접 추가할 테스트: 최소 2개. 독립 client와 sanitizer가 놓칠 수 있는 경로를 살핀다.
