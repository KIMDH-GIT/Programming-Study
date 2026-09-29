# Milestones

## Milestone 1 — 공개 계약과 파일 경계

### 목표

외부 client가 사용할 계약과 소유권·수명을 먼저 정의하고 header, library source, 사용 예제를 분리한다.

### 관련 curriculum Step

- P3-1. 공개 API와 ownership 계약
- P3-2. header·source·사용 예제 분리

### 관련 기존 Part

- Part 10: 함수 계약
- Part 18: 할당 객체의 소유권
- Part 24~25: translation unit과 header guard

### 필수 behavior

- 호출자가 가능한 동작과 인자·결과·소유권 이전 여부를 공개 header의 계약으로 알 수 있다.
- 별도 사용 예제가 내부 구현을 포함하지 않고 공개 API를 사용한다.

### 완료 조건

- 생성·삽입·검색·출력·삭제·정리의 호출 전후 소유권과 실패 시 상태를 문서화했다.
- header와 source와 예제가 구분되며, 예제는 공개 선언만 참조한다.

### 검증해야 할 동작

- 독립 client의 선언 참조와 library 정의 연결이 가능한 구성인지 확인한다.
- 잘못된 ownership 가정 없이 client가 자원 정리 책임을 설명할 수 있다.

### 사용자가 직접 결정할 설계 사항

- List wrapper 또는 head 표현, 내부 node 노출 범위, 이름·인자·결과 형식.
- 빈 상태와 destroy 뒤 재사용, 중복 값 정책을 계약에 표현하는 방법.

### 사용자가 직접 생각할 질문

- 검색 결과를 받은 client는 언제까지 그 결과를 사용할 수 있는가?
- 공개해야 하는 정보와 source 안에만 남겨 둘 정보의 기준은 무엇인가?

## Milestone 2 — head 갱신과 생성·삽입

### 목표

pointer-to-pointer로 호출자의 head 변경을 전달하며 node를 생성하고 list에 삽입한다.

### 관련 curriculum Step

- P3-3. pointer-to-pointer로 head 갱신
- P3-4. create와 insert

### 관련 기존 Part

- Part 14, 16: 포인터와 호출자 객체 변경
- Part 18: 동적 할당과 lifetime
- Part 30: node 생성과 삽입

### 필수 behavior

- 빈 list 또는 첫 위치 삽입에서 호출자의 head가 새 node를 가리킨다.
- 삽입 뒤 기존 node도 계속 도달 가능하며 순서가 계약과 일치한다.

### 완료 조건

- 첫 node와 추가 node 생성·삽입을 독립 client로 관찰했다.
- 성공과 할당 실패 시 각각의 소유권·연결 상태를 설명했다.

### 검증해야 할 동작

- 빈 list에서 첫 삽입, 여러 node에서 첫 위치와 그 밖의 삽입을 확인한다.
- 변경 전 node를 잃거나 접근할 수 없는 node를 만들지 않는다.

### 사용자가 직접 결정할 설계 사항

- 삽입 위치와 중복 허용 정책, 생성과 삽입의 책임 경계.
- head 이외에 tail·size를 저장할지 여부.

### 사용자가 직접 생각할 질문

- head 자체가 바뀔 때 값으로 전달한 포인터만으로 충분한가?
- 새 node와 기존 연결 중 어느 부분이 언제부터 list의 소유가 되는가?

## Milestone 3 — 검색과 출력

### 목표

list를 바꾸지 않고 데이터를 찾거나 순서대로 관찰할 수 있게 한다.

### 관련 curriculum Step

- P3-5. search와 print

### 관련 기존 Part

- Part 17: 읽기 전용 접근
- Part 30: 순회·검색·출력
- Part 5: 출력 형식

### 필수 behavior

- 찾은 값과 찾지 못한 값을 구분하고 빈 list도 처리한다.
- print는 정한 순서·형식으로 현재 list를 보여 주며 연결을 바꾸지 않는다.

### 완료 조건

- 빈 list와 여러 node의 검색·출력 결과를 공개 API를 통해 확인했다.
- 반환된 검색 결과의 소유권과 유효 기간을 계약에 적었다.

### 검증해야 할 동작

- 첫·중간·마지막 node의 검색과 없는 값 검색을 구분한다.
- 출력 전후의 list 상태가 같다.

### 사용자가 직접 결정할 설계 사항

- 검색 결과의 표현, 중복 값 중 반환 대상, 출력 대상과 형식.

### 사용자가 직접 생각할 질문

- 반환한 결과가 이후 삭제나 destroy 뒤에도 유효한가?
- 출력 결과만 보고 순서 오류를 구분할 수 있는가?

## Milestone 4 — 삭제와 전체 정리

### 목표

대상 node를 삭제하고 전체 list를 정리하면서 남은 연결과 호출자 head를 보존한다.

### 관련 curriculum Step

- P3-6. delete
- P3-7. destroy

### 관련 기존 Part

- Part 18: free와 해제 후 접근
- Part 30: 삭제·head 갱신·destroy
- Part 27: Undefined Behavior

### 필수 behavior

- 첫·중간·마지막 node를 삭제하면 남은 node는 계약상 순서대로 도달 가능하다.
- destroy는 모든 소유 node를 해제하고 호출자 head를 빈 상태로 바꾼다.

### 완료 조건

- 삭제 대상 유무와 destroy 전후 상태를 호출자가 확인했다.
- 삭제·정리 경로에서 같은 node를 두 번 해제하거나 해제 뒤 사용하지 않는다.

### 검증해야 할 동작

- 첫·중간·마지막 삭제 및 없는 값 삭제의 연결 상태를 비교한다.
- 모든 node를 정리한 뒤 head가 빈 상태인지 확인한다.

### 사용자가 직접 결정할 설계 사항

- 중복 대상의 삭제 범위, 없는 대상의 결과 표현, destroy 뒤 재사용 정책.

### 사용자가 직접 생각할 질문

- 삭제할 node가 첫 node 또는 마지막 node일 때 무엇이 달라지는가?
- destroy 이후 client가 보유한 이전 검색 결과는 어떻게 되는가?

## Milestone 5 — 경계와 할당 실패

### 목표

빈 list·단일 node·연속 삭제와 할당 실패에서도 기존 상태 및 ownership 계약을 유지한다.

### 관련 curriculum Step

- P3-8. 빈 list·단일 node·연속 삭제 검증
- P3-9. allocation 실패 경로 검증

### 관련 기존 Part

- Part 18: 할당 실패와 누수
- Part 30: 구조 보존
- Part 28: 결함 재현과 재검증

### 필수 behavior

- 빈 list의 조작과 단일 node 삭제·destroy는 계약대로 빈 상태가 된다.
- 반복 삽입·삭제 뒤에도 도달 가능성과 연결 순서가 유지된다.
- 할당 실패가 재현되면 기존 list는 유효하고 임시 자원은 남지 않는다.

### 완료 조건

- 빈 상태, 하나만 있는 상태, 연속 삭제와 재삽입을 각각 확인했다.
- 선택한 실패 주입 지점에서 실패 결과와 변경 전후 상태를 확인했다.

### 검증해야 할 동작

- 단일 node 삭제 뒤 다시 삽입하는 경로와 연속된 첫·마지막 삭제를 확인한다.
- 할당 실패 전후 검색·출력·destroy가 여전히 계약대로 동작한다.

### 사용자가 직접 결정할 설계 사항

- 할당 실패 재현용 test seam과 오류 노출 방식.
- destroy 뒤 재사용을 허용하거나 거부하는 계약과 그 검증 방법.

### 사용자가 직접 생각할 질문

- 실패 시 어느 시점까지의 연결을 원상 보존해야 하는가?
- 반복 조작 중 size나 tail을 선택했다면 어떻게 일관성을 입증할 것인가?

## Milestone 6 — strict build와 메모리 최종 검증

### 목표

공개 client의 빌드와 전체 경로 실행을 엄격한 C17 compile 및 AddressSanitizer로 최종 점검한다.

### 관련 curriculum Step

- P3-10. AddressSanitizer로 leak·memory error 최종 검증

### 관련 기존 Part

- Part 0, 24: 다중 파일 compile·link
- Part 27: compile 성공과 정확성의 차이
- Part 28: warning과 ASan 검사·해석
- Part 30: 메모리 누수 검증

### 필수 behavior

- 공개 header를 사용하는 별도 client가 library source와 함께 strict C17 baseline으로 빌드된다.
- 정상·경계·할당 실패 시나리오를 ASan 적용 실행으로 확인한다.

### 완료 조건

- `gcc -std=c17 -Wall -Wextra -Wpedantic -Werror` 빌드가 warning 없이 성공했다.
- 지원 환경에서 ASan leak·memory error 보고가 없는 최종 실행 결과와 검사 범위를 기록했다.
- `TEST_PLAN.md`의 필수 사례와 직접 추가한 테스트가 통과하고 `DEVLOG.md`에 결과가 남았다.

### 검증해야 할 동작

- 사용 예제의 독립 빌드와 모든 필수 API의 연동을 확인한다.
- leak, use-after-free, double-free와 잘못된 link로 인한 접근 오류를 점검한다.

### 사용자가 직접 결정할 설계 사항

- 실제 source 경로·실행 시나리오·ASan에서 누수 확인이 가능한 환경과 기록 방식.

### 사용자가 직접 생각할 질문

- sanitizer가 조용해도 테스트가 실행하지 않은 경로는 무엇인가?
- 어느 시나리오가 head 갱신과 ownership을 동시에 검증하는가?
