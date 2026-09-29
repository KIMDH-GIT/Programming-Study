# Test Plan

## 테스트 원칙

- 파일당 새 simulator 상태에서 시작하고 기대값은 실행 전에 손으로 계산한다. 출력 문구보다 필드 값, 접근 순서, 성공·오류 구분, 통계가 계약에 맞는지 본다.
- 아래의 구체 설정은 **테스트 fixture일 뿐** 프로그램의 내부 표현·고정 설정을 지정하지 않는다. 기본 fixture는 주소 폭 8 bit, cache 용량 16 byte, block 크기 4 byte다. 따라서 4 line이며 Tag/Index/Offset은 4/2/2 bit다. 사용법에 맞는 설정 전달 방식은 학습자가 직접 정한다.
- `R 0x00`처럼 각 접근을 한 줄에 적는다. 여러 줄 trace는 표시 순서대로 파일에 둔다. 일부 사례는 기대 주소 필드를 남겨 학습자가 직접 계산한다.
- 접근별 결과를 출력하지 않는 구현이라면 같은 시작 상태에서 각 접두 trace를 독립 실행하여 누적 통계의 차이로 hit·miss 순서를 확인한다.

## 필수 Test Cases

### T-001 — 첫 접근 miss

- Test ID: T-001
- 목적: invalid line의 첫 접근을 검사한다.
- 입력 또는 초기 상태: 기본 fixture, 모든 line invalid; `R 0x00`.
- 수행 동작: 새 cache에서 trace를 실행한다.
- 기대 behavior: Tag 0, Index 0, Offset 0; miss 1건, line이 유효해진다.
- 검증 이유: 초기 tag 값과 무관하게 valid를 검사해야 한다.
- 관련 milestone: Milestone 2, 3

### T-002 — 같은 block 재접근

- Test ID: T-002
- 목적: 재접근 hit를 검사한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00` 다음 `R 0x00`.
- 수행 동작: 두 접근의 결과와 최종 통계를 비교한다.
- 기대 behavior: 첫 접근 miss, 다음 hit; Accesses 2, Hits 1, Misses 1.
- 검증 이유: 같은 Tag·Index의 valid line은 hit여야 한다.
- 관련 milestone: Milestone 3, 5

### T-003 — 같은 Index의 다른 block 충돌

- Test ID: T-003
- 목적: 직접 사상 충돌과 eviction을 확인한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00`, `R 0x10`, `R 0x00`.
- 수행 동작: 세 접근을 순서대로 실행한다.
- 기대 behavior: Index 0의 Tag 0과 Tag 1이 번갈아 교체되어 miss, miss, miss.
- 검증 이유: 같은 Index의 다른 Tag가 공존하면 안 된다.
- 관련 milestone: Milestone 3, 5

### T-004 — 서로 다른 Index

- Test ID: T-004
- 목적: 다른 line의 독립성을 확인한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00`, `R 0x04`, `R 0x00`.
- 수행 동작: 세 접근을 순서대로 실행한다.
- 기대 behavior: Index 0, 1, 0; miss, miss, hit.
- 검증 이유: 다른 Index의 miss가 기존 line을 지우면 안 된다.
- 관련 milestone: Milestone 2, 3

### T-005 — block 안의 Offset 변화

- Test ID: T-005
- 목적: Offset이 바뀌어도 같은 block임을 확인한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00`, `R 0x03`.
- 수행 동작: 두 접근의 필드와 hit·miss를 대조한다.
- 기대 behavior: Tag·Index는 같고 Offset은 0에서 3으로 변한다. miss 다음 hit.
- 검증 이유: Offset을 tag 또는 line 선택에 섞는 실수를 드러낸다.
- 관련 milestone: Milestone 2, 3

### T-006 — 주소 경계

- Test ID: T-006
- 목적: 유효 주소 양끝의 추출·입력 검증을 확인한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00`, `R 0xFF`.
- 수행 동작: 각 주소를 독립된 상태에서도 추출하고 함께 실행한다.
- 기대 behavior: `0x00`은 (Tag, Index, Offset) = (0, 0, 0); `0xFF`는 (15, 3, 3). 함께 실행하면 두 번 모두 miss.
- 검증 이유: 최상위 bit와 마지막 block·byte에서 경계 오류를 찾는다.
- 관련 milestone: Milestone 1, 2, 5

### T-007 — 수작업 짧은 trace

- Test ID: T-007
- 목적: 주소 필드·접근 순서·최종 통계를 독립 검산한다.
- 입력 또는 초기 상태: 기본 fixture; `R 0x00`, `R 0x03`, `R 0x10`, `R 0x04`, `R 0x00`.
- 수행 동작: 각 접두 trace와 전체 trace를 새 상태에서 실행한다.
- 기대 behavior: (Tag, Index, Offset)는 순서대로 (0,0,0), (0,0,3), (1,0,0), (0,1,0), (0,0,0); 결과는 miss, hit, miss, miss, miss; Accesses 5, Hits 1, Misses 4, Hit Rate 20%.
- 검증 이유: 추출·교체·독립 line·집계를 한 번에 점검하는 수작업 oracle이다.
- 관련 milestone: Milestone 2, 3, 5, 6

### T-008 — 빈 trace

- Test ID: T-008
- 목적: 접근 0건과 비율의 0 분모를 확인한다.
- 입력 또는 초기 상태: 기본 fixture, 길이 0인 파일; 별도 실행에서는 빈 줄만 있는 파일.
- 수행 동작: 두 파일을 각각 실행한다.
- 기대 behavior: 두 경우 모두 정상 종료; Accesses 0, Hits 0, Misses 0, Hit Rate 0%.
- 검증 이유: EOF와 0으로 나누기 및 빈 줄 해석 문제를 발견한다.
- 관련 milestone: Milestone 4, 5

### T-009 — 잘못된 설정

- Test ID: T-009
- 목적: 주소 연산 전 설정 검증을 확인한다.
- 입력 또는 초기 상태: 용량 10 byte와 block 4 byte; 별도 사례로 block 3 byte 또는 주소 폭 2 bit에 용량 16 byte와 block 4 byte.
- 수행 동작: 각 설정으로 실행을 시도한다.
- 기대 behavior: 정수배·2의 거듭제곱·주소 폭 조건에 맞지 않아 모두 거부되고 정상 요약은 없다.
- 검증 이유: 무효 line 수와 음수 Tag bit를 예방한다.
- 관련 milestone: Milestone 1

### T-010 — 잘못된 trace

- Test ID: T-010
- 목적: 파일 입력 실패를 정상 접근과 구분한다.
- 입력 또는 초기 상태: 기본 fixture; 각각 `X 0x00`, `R`, `R 0x100`, `W 0xGG`, `R 0x00 extra`를 담은 파일 및 존재하지 않는 파일.
- 수행 동작: 각 파일을 독립 실행한다.
- 기대 behavior: 각 경우 오류가 식별되고 성공한 최종 통계로 발표되지 않는다.
- 검증 이유: operation·필드 개수·16진 변환·범위·열기 실패를 검증한다.
- 관련 milestone: Milestone 1, 4

### T-011 — write-allocate

- Test ID: T-011
- 목적: 쓰기 miss가 line을 할당하는지 확인한다.
- 입력 또는 초기 상태: 기본 fixture; `W 0x10`, `R 0x13`, `W 0x10`, `R 0x00`.
- 수행 동작: 읽기·쓰기 혼합 trace를 순서대로 실행한다.
- 기대 behavior: miss, hit, hit, miss; Accesses 4, Hits 2, Misses 2, Hit Rate 50%.
- 검증 이유: W miss 할당과 W hit 집계 및 같은 Index의 충돌을 확인한다.
- 관련 milestone: Milestone 1, 3, 5

### T-012 — 집계 불변식

- Test ID: T-012
- 목적: 통계 출력과 비율을 확인한다.
- 입력 또는 초기 상태: 기본 fixture; T-007의 전체 및 각 접두 trace.
- 수행 동작: 각 실행의 네 지표를 기록하고 수작업으로 누적한다.
- 기대 behavior: 항상 Accesses = Hits + Misses; 전체는 5/1/4/20%이며 접두 실행의 차이는 한 접근당 한 hit 또는 miss다.
- 검증 이유: 집계 누락·중복·rate 계산 실수를 찾는다.
- 관련 milestone: Milestone 5, 6

### T-013 — strict C17 build와 최종 회귀

- Test ID: T-013
- 목적: 구현 완성 후 전체 baseline을 검증한다.
- 입력 또는 초기 상태: clean build와 T-001~T-012의 독립 입력.
- 수행 동작: 실제 파일 구성에 맞춰 `gcc -std=c17 -Wall -Wextra -Wpedantic -Werror`로 build하고 사례를 각각 실행한다.
- 기대 behavior: warning·error 없는 build와 필수 사례 통과; 실행 순서에 의존하지 않는다.
- 검증 이유: 결과뿐 아니라 C17 경계 및 통합 회귀를 확인한다.
- 관련 milestone: Milestone 6

## 선택 확장 Test Cases

### T-014 — 선택: set-associative

- Test ID: T-014
- 목적: 선택한 복수 way의 같은 set 공존을 확인한다.
- 입력 또는 초기 상태: P4-12를 선택한 경우에만 사용할 2-way·단일 set·4-byte block, 충분한 주소 폭; `R 0x00`, `R 0x04`, `R 0x00`.
- 수행 동작: 확장의 설정 계약에 맞춰 순서대로 실행한다.
- 기대 behavior: miss, miss, hit; 서로 다른 두 tag가 공존한다. 미선택 시 실행하지 않는다.
- 검증 이유: 연관 cache가 직접 사상과 다른 점을 점검한다.
- 관련 milestone: Milestone 7 (선택)

### T-015 — 선택: LRU

- Test ID: T-015
- 목적: 오래 사용하지 않은 way의 교체를 확인한다.
- 입력 또는 초기 상태: P4-12·P4-13 둘 다 선택한 경우의 T-014 설정; `R 0x00`, `R 0x04`, `R 0x00`, `R 0x08`, `R 0x04`.
- 수행 동작: 최근 사용 순서를 갱신하며 trace를 실행한다.
- 기대 behavior: miss, miss, hit, miss, miss; `0x08`은 오래된 `0x04`의 way를 교체한다. 미선택 시 실행하지 않는다.
- 검증 이유: LRU의 hit 후 순서 갱신과 victim 선택을 검증한다.
- 관련 milestone: Milestone 7 (선택)

## Milestone별 사용자가 직접 추가할 테스트

### Milestone 1

사용자가 직접 추가할 테스트: 최소 2개. 설정 경계와 `R/W address` 계약에서 예상 입력을 만든다.

### Milestone 2

사용자가 직접 추가할 테스트: 최소 2개. Index 또는 Offset 폭이 0인 설정의 주소 필드를 계산한다.

### Milestone 3

사용자가 직접 추가할 테스트: 최소 2개. 서로 다른 충돌·쓰기 순서에서 line 상태를 예측한다.

### Milestone 4

사용자가 직접 추가할 테스트: 최소 2개. 유효한 줄과 invalid 줄의 조합, EOF 경계를 살펴본다.

### Milestone 5

사용자가 직접 추가할 테스트: 최소 2개. 접근 수와 비율의 경계를 비교한다.

### Milestone 6

사용자가 직접 추가할 테스트: 최소 2개. 손으로 계산한 별도의 짧은 trace와 오류 회귀를 만든다.

### Milestone 7 (선택)

사용자가 직접 추가할 테스트: 최소 2개. 확장을 선택했다면 set 공존 또는 LRU 교체 사례를 만든다.
