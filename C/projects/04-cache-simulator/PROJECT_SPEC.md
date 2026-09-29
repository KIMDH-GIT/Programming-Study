# Project Specification

## 프로젝트 목적

주소 trace를 이용해 직접 사상 cache 모델의 주소 분해, line 선택, hit·miss, write-allocate와 통계를 C17로 검증한다. 모델의 기능적 정확성이 우선이고 simulator의 성능은 별도 관심사다.

## 사용자 관점 동작

- 사용자가 cache 설정과 trace 파일을 제공하면 프로그램은 유효성을 검사하고 순서대로 접근을 처리한다.
- 결과에서 Accesses, Hits, Misses, Hit Rate를 확인할 수 있다.
- 유효하지 않은 설정·trace 또는 파일 읽기 오류를 정상 완료로 보고하지 않는다.
- 사용자에게 보이는 인자 방식, 출력 부가 정보와 화면 문구는 학습자가 정하고 기록한다.

## 세 층의 범위

- **[Cache 모델]** 주소 폭의 범위 안에서 byte-addressed 직접 사상 cache를 모델링한다. 용량과 block 크기는 byte 단위다. 한 접근은 읽기 또는 쓰기 한 번이며 valid·tag로 hit 여부를 정한다.
- **[Simulator software structure]** 설정 입력, 검증, trace 해석, 상태 관리, 결과 표시를 위한 소프트웨어다. 내부 표현이나 함수 배치는 계약이 아니다.
- **[실제 hardware cache]** cache 계층, 물리 주소 변환, 타이밍, 회로·메모리 버스 동작을 재현하지 않는다. 모델은 특정 ISA와 연결되지 않는다.

## 필수 기능

- 직접 사상 cache 설정의 주소 폭·용량·block 크기를 검증하고 line 수와 Tag·Index·Offset bit 수를 도출한다.
- 주소에서 세 필드를 추출하고 Index가 선택한 line의 valid·tag로 hit·miss를 판단한다.
- miss이면 선택 line을 유효한 새 tag로 갱신한다. 같은 Index의 다른 tag는 이전 block을 대체한다.
- 읽기와 쓰기 모두 접근 한 건으로 집계하고, write miss에도 block을 할당하는 **write-allocate**를 적용한다. write hit은 hit로 집계하고 line은 유효하게 유지한다.
- trace 파일의 레코드를 순서대로 읽고 Accesses, Hits, Misses, Hit Rate를 출력한다.
- 빈 trace, 충돌, 경계 주소 및 잘못된 입력에 대해 계약에 맞는 결과를 보인다.

## 선택 기능

- P4-12: set-associative cache. 직접 사상 baseline이 완성된 뒤 set과 way의 의미 및 주소 분해 계약을 새로 정한다.
- P4-13: LRU replacement. 여러 way를 둔 선택 확장에서 replacement가 필요한 경우에만 상태 갱신과 동률 정책을 정의한다.
- 어느 확장도 기본 Project 4 완료에 필요하지 않으며 필수 동작을 깨뜨려서는 안 된다.

## 입력 contract

- 설정은 양의 주소 폭, 양의 cache 용량, 양의 block 크기를 가진다. block 크기와 line 수(`용량 / block 크기`)는 2의 거듭제곱이며 용량은 block 크기의 정수배다. 주소 폭 안에서 세 필드의 bit 수가 성립하고 선택한 C17 표현에서 안전하게 계산·표현 가능한 설정만 받는다.
- 주소는 `0`부터 선택한 주소 폭의 최댓값까지다. 최댓값을 넘거나 숫자로 완전히 해석할 수 없는 주소는 거부한다.
- trace는 텍스트 파일이며 비어 있지 않은 각 줄이 `R address` 또는 `W address` 한 건이다. 주소는 `0x` 접두사를 가진 16진 unsigned 값으로 쓴다. 예: `R 0x00`, `W 0x10`. 필드 사이의 공백은 허용하고 빈 줄은 무시한다. 추가 필드, 누락된 필드, 다른 동작 문자 및 잘못된 숫자는 오류다.
- 물리적 EOF는 정상 입력 종료다. 길이가 0인 파일과 빈 줄만 있는 파일은 모두 빈 trace다. 파일 열기 실패와 중간 읽기 오류는 입력 오류다.
- 설정을 어떤 방식으로 전달하는지, 줄을 읽고 숫자로 바꾸는 방법과 주소의 구체적인 C 자료형은 학습자가 결정하고 사용법에 적는다.

## 출력 contract

- 성공한 실행에는 `Accesses`, `Hits`, `Misses`, `Hit Rate`가 구분 가능하게 표시된다. Hit Rate는 `Hits / Accesses × 100%`이고 Accesses가 0이면 `0%`로 정의한다. 표시 자릿수와 부가적인 접근별 표시 여부는 학습자가 정하되 검증 가능한 일관성을 유지한다.
- invalid 설정·trace·파일 오류에서는 정상 요약으로 오인될 출력을 내지 않고 오류를 식별할 수 있어야 한다. 종료 상태와 사용자 메시지의 구체적인 관계는 학습자가 정한다.
- 접근별 결과를 별도로 출력하지 않더라도 짧은 trace의 각 접두 부분을 독립 실행해 누적 통계와 수작업 hit·miss 순서를 비교할 수 있어야 한다.

## 오류 contract

- 유효하지 않은 설정은 주소 추출이나 line 접근 전에 거부한다.
- 유효하지 않은 trace 레코드, 범위 밖 주소, 파일 열기·읽기 실패를 정상 접근으로 집계하지 않는다.
- 오류가 발생한 실행의 일부 통계를 성공한 최종 결과로 발표하지 않는다. 오류를 발견하고 종료·보고하는 소프트웨어 전략은 학습자가 정한다.
- 입력 길이, 산술 범위, 배열 범위와 shift 폭을 검사해 C17 Undefined Behavior에 의존하지 않는다.

## 상태와 불변식

- 유효한 설정에서 `line 수 = cache 용량 / block 크기`, `Offset bits = log2(block 크기)`, `Index bits = log2(line 수)`, `Tag bits = 주소 폭 - Index bits - Offset bits`이며 Tag bits는 음수가 아니다. 0-bit Index 또는 Offset도 가능한 설정이다.
- 주소의 Offset은 block 내 위치, Index는 선택 line, Tag는 비교할 상위 필드다. 같은 block 내 Offset이 바뀌어도 line의 hit·miss 상태는 같으며, 다른 Index의 접근은 기존 line을 교체하지 않는다.
- 최초 모든 line은 invalid다. hit는 선택 line이 valid이고 저장된 tag와 요청 tag가 일치할 때만 발생한다. miss 후 선택 line의 valid·tag는 새 block을 나타낸다.
- 매 정상 접근마다 `Accesses = Hits + Misses`가 유지된다. 빈 trace에서는 세 수가 모두 0이다.
- trace에는 byte 값, dirty bit, write-back/write-through, write buffer, 실제 메모리 갱신, 타이밍, 계층, 일관성 및 실제 CPU 동작을 모델링하지 않는다. write-allocate는 쓰기 miss의 line 할당 여부만 정의한다.

## 설계 결정으로 남겨 둘 영역

- cache 구조체 형태, line 표현, 주소 자료형과 설정 표현 방식
- parser의 API·분리 구조, 입력 길이 처리, 함수 개수와 책임
- 통계 자료형과 overflow를 다루는 방식, 오류 전달 전략
- 단일 파일/다중 파일 구성, 인자 전달 방식, 출력 정밀도와 부가 정보

선택 근거를 `DEVLOG.md`에 기록한다. 이 문서는 내부 설계를 정답으로 고정하지 않는다.

## C17 요구사항

- 최소 `gcc -std=c17 -Wall -Wextra -Wpedantic -Werror`로 warning 없이 build한다.
- 대상에서 선택한 주소 폭을 표현 가능한지 확인한다. unsigned 연산의 shift count가 피연산자 폭 이상이 되지 않게 하고, mask·용량 계산 및 통계 합산의 overflow를 검토한다.
- 초기화되지 않은 valid·tag, 범위 밖 line 접근, 실패한 입력 변환 값 사용 및 자료형·format 불일치에 의존하지 않는다.

## AI mentor 규칙

AI는 reviewer / mentor이며 학습자의 구현자가 아니다. 사용자가 오류, 가설, 시도 결과를 기록한 후 Hint 1(개념), Hint 2(구조), Hint 3(독립된 작은 예제) 순으로 도움을 요청한다. Full solution은 마지막 수단이다. 검토는 위치, 심각도, 문제, 이유, Hint 1을 먼저 제시한다.

## 금지사항

- 이 명세를 완성 source, 함수 구현, 고정 자료구조 설계로 바꾸지 않는다.
- 검증을 생략하거나 경고 옵션을 낮춰 통과시키지 않는다.
- 실제 하드웨어 cache의 시간·쓰기 정책·ISA 동작을 이 추상 모델의 결과로 주장하지 않는다.
- 선택 확장을 필수 baseline과 혼합해 초기 완료 조건으로 삼지 않는다.

## 완료 기준

- 직접 사상 baseline의 P4-1~P4-11 및 P4-14가 구현·검증되고 수작업 기대값과 일치한다.
- invalid 설정·trace, write-allocate, 빈 trace, 충돌, 경계 및 통계 불변식을 확인한다.
- strict C17 build와 필수·직접 추가한 테스트가 통과하고 설계 이유·결과를 기록한다.
- P4-12·P4-13 및 simulator 성능 최적화는 초기 완료와 무관하다.
