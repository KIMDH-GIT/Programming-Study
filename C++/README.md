# C++ Study

## 목적

C의 기본 문법과 pointer를 학습한 전자공학·컴퓨터공학 학부생이 C++를 단순한 문법 확장이 아니라 **type system, object lifetime, resource ownership, abstraction, 표준 라이브러리, 실제 프로그램 구조**의 관점에서 학습하는 교재다.

이 과정은 다음 능력을 목표로 한다.

- C와 C++의 공통점과 의미 있는 차이를 설명한다.
- reference, lifetime, copy, move, RAII, ownership을 코드 실행과 연결한다.
- class, runtime polymorphism, template, STL 코드를 읽고 작성한다.
- multi-file 프로그램을 빌드·디버깅하고 linker 오류와 memory 오류를 진단한다.
- systems·embedded C++ 코드에서 ABI, representation, allocation, `volatile`, atomic, runtime 정책을 판단한다.

전체 학습 순서와 148개 Step: [CPP_CURRICULUM.md](./CPP_CURRICULUM.md)

## 대상과 선수 지식

- C의 변수, 조건문, 반복문, 함수, 배열, struct를 학습했다.
- pointer와 dynamic memory의 기본 개념을 알고 있다.
- Linux terminal에서 C 소스를 compile하고 실행할 수 있다.
- C++는 처음부터 체계적으로 학습한다.

C 문법 자체는 반복하지 않는다. C++에서 의미가 달라지거나 안전한 설계에 영향을 주는 지점만 다음 형식으로 연결한다.

```text
C에서는:
C++에서는:
차이가 생기는 이유:
```

## 학습 원칙

```text
C와 C++의 차이와 toolchain
→ type·initialization·const·conversion
→ function interface
→ reference·pointer·array
→ standard value type
→ class·object
→ lifetime·resource·RAII·unique ownership
→ copy
→ move·value category
→ runtime polymorphism
→ template
→ STL container·iterator·algorithm·lambda
→ modern utility·advanced ownership
→ 실제 프로그램 구조·systems/embedded C++
```

- `new/delete`는 원리를 이해하는 학습 대상이며 기본 자원 관리 방식은 RAII와 Rule of Zero다.
- `std::unique_ptr`는 RAII와 ownership을 배우는 Part 8에서 도입한다.
- STL은 container 이름 암기보다 저장 구조, iterator, algorithm, callable, 복잡도, invalidation의 관계로 학습한다.
- 표준이 보장하는 의미와 GCC·Linux·ABI·hardware에서 관찰되는 결과를 구분한다.
- 각 핵심 Step은 구현, 결과 예측, 오류 분석, 코드 수정, lifetime 추론을 섞은 exercise와 연결한다.

## 기본 빌드 환경

대부분의 Step은 C++17을 기준으로 한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

C++20 전용 Step은 `[C++20]` 또는 `[C++20, 선택]`으로 표시한다.

```bash
g++ -std=c++20 -Wall -Wextra -pedantic -g main.cpp -o main
```

메모리와 undefined behavior를 관찰하는 실습에서는 AddressSanitizer와 UndefinedBehaviorSanitizer를 사용한다.

## 디렉터리 구조

```text
C++/
├── README.md
├── CPP_CURRICULUM.md
├── C++.md
├── notes/
│   └── NN-part-name/
│       └── N-K-step-name.md
└── exercises/
    └── NN-part-name/
        └── N-K/
            └── README.md
```

- `notes/`: Step별 개념, 문법, 내부 동작, 예제, 실수를 설명한다.
- `exercises/`: 같은 번호의 note를 확인하는 분석·구현 문제와 학습자 코드를 둔다.
- Part 디렉터리는 두 자리 번호와 kebab-case 이름을 사용한다.
- 이번 단계에서는 Part 디렉터리만 준비하며 실제 Step 본문은 다음 작업부터 추가한다.

## Part 구성

| Part | 주제 | 핵심 |
|---:|---|---|
| [0](./notes/00-toolchain-and-build/) | C++ 개발 환경과 Build | 번역 단계, `g++`, warning, GDB, sanitizer |
| [1](./notes/01-from-c-to-cpp/) | C에서 C++로 | program entry, stream I/O, namespace |
| [2](./notes/02-types-initialization-and-conversion/) | Type System, Initialization, const, Conversion | type, initialization, deduction, conversion |
| [3](./notes/03-expressions-and-control-flow/) | Expression과 Control Flow | expression, scope, boundary, invariant |
| [4](./notes/04-functions-and-interfaces/) | Function과 Interface | declaration, overload, callback, contract |
| [5](./notes/05-references-pointers-and-arrays/) | Reference, Pointer, Array | binding, nullability, borrowing, dangling |
| [6](./notes/06-strings-arrays-and-vectors/) | `string`, `array`, `vector` | value type, contiguous storage, invalidation |
| [7](./notes/07-classes-and-objects/) | Class와 Object | invariant, constructor, member, composition |
| [8](./notes/08-lifetime-resources-and-raii/) | Object Lifetime, Resource, RAII | storage duration, destructor, RAII, `unique_ptr` |
| [9](./notes/09-copy-semantics/) | Copy Semantics | copy, resource owner, Rule of Three/Zero |
| [10](./notes/10-move-semantics-and-value-categories/) | Move Semantics와 Value Category | value category, `std::move`, Rule of Five/Zero |
| [11](./notes/11-inheritance-and-polymorphism/) | Inheritance와 Runtime Polymorphism | virtual dispatch, abstract interface, RTTI |
| [12](./notes/12-operators-enums-and-callables/) | Operator, Enum, Callable | value interface, `enum class`, function object |
| [13](./notes/13-templates-and-generic-programming/) | Template와 Generic Programming | deduction, instantiation, compile-time design |
| [14](./notes/14-stl-containers/) | STL Container | storage, complexity, locality, invalidation |
| [15](./notes/15-iterators-algorithms-and-lambdas/) | Iterator, Algorithm, Lambda | ranges, algorithms, comparator, capture |
| [16](./notes/16-modern-cpp-and-ownership/) | Modern C++와 Ownership | vocabulary types, views, shared ownership, files, time |
| [17](./notes/17-program-structure-and-systems-cpp/) | 실제 프로그램 구조와 Systems/Embedded C++ | library, CMake, ABI, representation, MMIO, concurrency |

## Note 형식

```text
# Step N-K — 제목

## 학습 목표
## 선수 지식
## 1. 개념
## 2. 문법
## 3. 프로그램 내부에서 어떻게 동작하는가
## 4. 예제 코드
## 5. 실행 결과 분석
## 6. C와의 차이
## 7. 자주 하는 실수
## 8. 핵심 정리
## 9. 직접 확인할 문제
## 다음 Step
```

`C와의 차이`는 실제 오해를 막는 Step에서만 사용한다.

## Exercise 형식

```text
# Step N-K 실습 — 제목

## 실습 목적
## 작성할 파일
## 해야 할 일
## 사용할 개념
## 컴파일 방법
## 실행 방법
## 예상 관찰 결과
## 확인 포인트
## 추가 실험
## 완료 기준
```

exercise는 예제를 다시 입력하는 데 그치지 않고 출력 예측, compiler 오류 분석, 잘못된 코드 수정, memory/lifetime 문제 추론, 작은 기능 구현을 포함한다.
