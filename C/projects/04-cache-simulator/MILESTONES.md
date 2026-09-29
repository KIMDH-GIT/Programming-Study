# Milestones

Milestone 1~6은 직접 사상 baseline의 필수 경로다. Milestone 7은 기본 프로젝트 완료 뒤의 **선택 확장**이며 수행하지 않아도 완료로 인정한다. 각 milestone에서 [테스트 계획](TEST_PLAN.md)의 해당 사례 외에 테스트를 최소 두 개 직접 만든다.

## Milestone 1 — 모델·trace 계약과 설정

### 목표

직접 사상 모델의 크기 단위와 입력·쓰기 정책을 정하고 구현 전에 설정을 검증한다.

### 관련 curriculum Step

- P4-1. 직접 사상 cache의 주소 폭·용량·block 크기
- P4-2. `R/W address` trace 형식
- P4-3. write-allocate와 비모델링 범위 명시
- P4-4. cache 설정 검증

### 관련 기존 Part

- Part 2~4: 정수 크기와 범위
- Part 23: 텍스트 파일·EOF
- Part 31: 모델과 하드웨어의 차이

### 필수 behavior

- 주소 폭, byte 단위 cache 용량·block 크기 및 16진 주소의 `R/W address` 레코드 계약을 설명한다.
- write miss가 line을 할당하는 write-allocate와 데이터 값·dirty bit·타이밍 등 비모델링 범위를 명시한다.
- 불가능하거나 지원 범위를 벗어난 설정을 상태 접근 전에 거부한다.

### 완료 조건

- 수락·거부할 설정과 주소 범위를 문서화하고 유효·무효 설정을 구분해 실행했다.
- 모델과 소프트웨어 구조, 실제 하드웨어의 차이를 설명할 수 있다.

### 검증해야 할 동작

- 정상 설정은 line 수와 주소 범위가 성립한다.
- block이 용량보다 크거나 정수배가 아니거나 필요한 폭을 표현할 수 없는 설정은 거부된다.

### 사용자가 직접 결정할 설계 사항

- 설정 전달·표현 방식, 주소 자료형, 오류 보고 방식.

### 사용자가 직접 생각할 질문

- 주소 폭을 표현할 수 있다는 주장에 필요한 검사와 테스트는 무엇인가?
- 캐시 용량과 block 크기의 단위를 섞으면 어떤 결과가 잘못되는가?

## Milestone 2 — 주소 분해

### 목표

주소의 Tag·Index·Offset 폭을 구하고 각 필드를 경계에서 정확히 추출한다.

### 관련 curriculum Step

- P4-5. Tag·Index·Offset bit 수
- P4-6. 주소에서 Tag·Index·Offset 추출

### 관련 기존 Part

- Part 3~4: bit 폭과 unsigned 범위
- Part 21: mask·shift 및 폭 경계
- Part 27: Undefined Behavior

### 필수 behavior

- `line 수 = 용량 / block 크기`에서 Offset·Index·Tag bit 수를 일관되게 도출한다.
- 허용 주소의 각 필드를 추출하고 0-bit Index·Offset을 안전하게 다룬다.

### 완료 조건

- 손으로 계산한 필드와 경계 주소의 추출 결과가 일치한다.
- 표현 폭과 같은 shift 또는 범위 밖 주소에 기대지 않는다.

### 검증해야 할 동작

- 같은 block 안의 두 주소는 Tag·Index가 같고 Offset은 다르다.
- 최저·최고 주소의 필드 값이 모델과 일치한다.

### 사용자가 직접 결정할 설계 사항

- 비트 폭 계산과 주소 분해의 인터페이스 및 자료형.

### 사용자가 직접 생각할 질문

- Index bit가 0일 때 어떤 주소 필드가 사라지는가?
- 최상위 주소에서 shift 횟수와 mask 폭은 안전한가?

## Milestone 3 — line 상태와 접근 판정

### 목표

valid·tag 상태를 이용해 직접 사상 hit·miss와 교체, write-allocate를 구현한다.

### 관련 curriculum Step

- P4-7. valid·tag를 가진 cache line 배열
- P4-8. hit·miss 판정과 line 갱신
- P4-3. write-allocate 재검증

### 관련 기존 Part

- Part 11: 배열과 경계
- Part 18~19: 소유권과 구조체
- Part 27~28: 상태 결함 재현

### 필수 behavior

- 모든 line은 invalid 상태에서 시작한다.
- 같은 Index·Tag의 재접근은 hit, 동일 Index의 다른 Tag는 miss와 교체가 된다.
- W miss도 line을 할당해 같은 block의 다음 접근은 hit가 된다.

### 완료 조건

- 첫 miss, 재접근 hit, 충돌 교체 후 재접근 miss가 각각 재현된다.
- 다른 Index의 상태가 불필요하게 바뀌지 않는다.

### 검증해야 할 동작

- invalid line은 저장 tag가 우연히 같더라도 hit가 아니다.
- R/W 구분 없이 write-allocate 계약에 맞는 line 갱신이 이루어진다.

### 사용자가 직접 결정할 설계 사항

- cache와 line의 표현, 상태 초기화·접근의 함수 경계.

### 사용자가 직접 생각할 질문

- 충돌 뒤 이전 block을 다시 읽으면 왜 miss인가?
- 값을 저장하지 않는 모델에서 W 접근이 바꾸는 것은 무엇인가?

## Milestone 4 — trace 파일 입력

### 목표

파일 레코드를 순서대로 해석하고 유효하지 않은 입력을 정상 접근과 구분한다.

### 관련 curriculum Step

- P4-9. trace file parsing
- P4-2. `R/W address` trace 형식 재검증

### 관련 기존 Part

- Part 13: 문자열·길이 경계
- Part 19: 숫자 변환 검증
- Part 23: 파일 열기, EOF, 읽기 오류

### 필수 behavior

- 각 비어 있지 않은 줄의 R 또는 W와 범위 안의 16진 주소를 한 건으로 처리한다.
- 빈 줄을 건너뛰고 malformed 레코드, 범위 밖 주소, 파일 오류를 보고한다.

### 완료 조건

- 정상 파일의 접근 순서가 보존되고 잘못된 파일은 성공 요약을 만들지 않는다.
- EOF와 읽기 실패를 구분한다.

### 검증해야 할 동작

- 여분 필드, 빠진 주소, 잘못된 숫자를 거부한다.
- 길이가 0인 trace를 안전하게 종료한다.

### 사용자가 직접 결정할 설계 사항

- 줄 읽기·변환 방식, 길이 제한과 거부 처리, 파일 오류 전달 구조.

### 사용자가 직접 생각할 질문

- 일부만 변환된 주소를 성공으로 오인하지 않으려면 무엇을 검사해야 하는가?
- 마지막 줄에 개행이 없어도 유효한 접근으로 처리할 수 있는가?

## Milestone 5 — 통계와 경계 검증

### 목표

접근별 판정을 집계하고 빈 trace·충돌·경계 주소에서 불변식을 확인한다.

### 관련 curriculum Step

- P4-10. Accesses·Hits·Misses·Hit Rate
- P4-11. 빈 trace·충돌·경계 주소 검증

### 관련 기존 Part

- Part 5~6: 수치 출력과 변환
- Part 9: 반복·경계
- Part 28: 테스트와 디버깅

### 필수 behavior

- Accesses = Hits + Misses를 유지하고 Hit Rate는 정상 접근이 0건이면 0%로 표시한다.
- 충돌과 최저·최고 주소를 포함한 trace를 처리한다.

### 완료 조건

- 접근 수·hit·miss·rate가 수작업 값과 맞고 빈 trace의 네 지표가 정의대로 나온다.
- strict C17 baseline에서 warning이 없다.

### 검증해야 할 동작

- Offset만 바뀐 접근과 같은 Index의 다른 Tag가 집계에 서로 다른 영향을 준다.
- 통계 합산과 rate 변환이 선택 자료형의 범위를 어기지 않는다.

### 사용자가 직접 결정할 설계 사항

- 통계 자료형, 출력 정밀도, overflow 정책.

### 사용자가 직접 생각할 질문

- 0건에서 0으로 나누지 않고 무엇을 출력할 것인가?
- 출력된 비율의 반올림 오차를 어떻게 검증할 것인가?

## Milestone 6 — 수작업 oracle과 최종 검증

### 목표

작은 trace의 각 접근을 손으로 먼저 계산하고 통합 simulator 결과를 대조한다.

### 관련 curriculum Step

- P4-14. 수작업 기대값과 simulator 출력 최종 검증
- P4-1~P4-11. 필수 baseline 회귀 검증

### 관련 기존 Part

- Part 0: 빌드와 실행
- Part 21: 주소 필드 재검산
- Part 27~28: UB·warning·회귀 검증

### 필수 behavior

- 짧은 trace의 Tag·Index·Offset, hit·miss 순서, 최종 통계를 독립 계산한다.
- 필수 오류·경계·write-allocate 테스트와 직접 추가한 테스트를 실행한다.

### 완료 조건

- 수작업 기대값과 전체 trace 및 접두 trace의 출력이 일치한다.
- strict C17 build가 성공하고 결과·설계 선택·남은 범위를 `DEVLOG.md`에 기록한다.

### 검증해야 할 동작

- 오류 파일을 성공으로 오인하지 않으며 전체 통계와 각 접두 통계가 일관된다.
- 같은 테스트를 독립 실행해도 순서나 이전 실행에 의존하지 않는다.

### 사용자가 직접 결정할 설계 사항

- 접두 trace 작성·비교 방법, 테스트 자동화 범위, 결과 기록 방식.

### 사용자가 직접 생각할 질문

- 예상값을 simulator에서 복사하면 어떤 검증 능력을 잃는가?
- 모델이 옳아도 프로그램이 틀릴 수 있는 경계는 어디인가?

## Milestone 7 — 선택 확장: 연관도와 LRU

### 목표

필수 baseline이 완성된 경우에만 set-associative와 LRU를 순서대로 탐색한다.

### 관련 curriculum Step

- P4-12. 선택 확장: set-associative cache
- P4-13. 선택 확장: LRU replacement

### 관련 기존 Part

- Part 11, 19: 집합·way 상태를 표현할 배열·구조체
- Part 21: 새 Index bit와 주소 분해
- Part 28: 교체 순서 검증

### 필수 behavior

- **확장을 선택했다면** set·way 수와 hit·miss의 의미를 새로 정의하고 직접 사상 회귀를 유지한다.
- **LRU까지 선택했다면** 접근마다 최근 사용 순서와 victim 선택을 검증한다. 선택하지 않으면 추가 구현은 없다.

### 완료 조건

- 확장을 선택한 경우에만 선택 기능의 계약·테스트·결과를 기록한다.
- 수행 여부와 관계없이 Milestone 1~6의 완료 판정은 바뀌지 않는다.

### 검증해야 할 동작

- 선택한 연관도에서 같은 set의 복수 tag가 공존한다.
- LRU를 선택한 경우 최근 사용한 way가 아닌 오래된 way가 교체된다.

### 사용자가 직접 결정할 설계 사항

- set·way 표현, 연관도 설정, LRU 상태·동률 정책, 확장 출력 범위.

### 사용자가 직접 생각할 질문

- 직접 사상에서 필요 없던 replacement 정책은 왜 여러 way에서 필요한가?
- 기존 직접 사상 기대값을 확장 전후에 어떻게 유지·비교할 것인가?
