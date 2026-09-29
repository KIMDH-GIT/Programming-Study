# C++ 자율진도형 학습 지도

## 교재 목표와 대상

이 교재는 C의 기본 문법, pointer, array, function, struct, dynamic memory를 학습한 전자공학·컴퓨터공학 학부생이 C++를 처음부터 체계적으로 익히도록 설계한다. 코딩 테스트 문법 암기가 아니라 object lifetime, resource ownership, abstraction, 표준 라이브러리, 실제 프로그램 구조를 이해하고 시스템·운영체제·임베디드·HW/SW Codesign 코드로 이어지는 것이 목표다.

- 총 **18개 Part, 148개 Step**으로 구성한다.
- C 문법 자체는 반복하지 않고 C++에서 의미가 달라지는 지점만 연결한다.
- 각 Step은 하나의 교재 절이 될 수 있는 크기로 묶고, 단순 문법 조각을 별도 Step으로 쪼개지 않는다.
- 기본 표준은 **C++17**이며 C++20 기능은 `[C++20]` 또는 `[C++20, 선택]`으로 표시한다.

## 기존 설계 검토와 재설계 원칙

기존 22개 Part·247개 Step 설계는 lifetime, RAII, copy/move, STL, program structure, systems/embedded까지 범위가 넓고 C++ 표준의 보장과 구현 관찰을 구분한 점이 좋았다. 그러나 C에서 이미 배운 제어문과 문법을 여러 Step으로 다시 나누고, sequence/associative container와 smart pointer/modern utility/error handling을 각각 독립 Part로 분리해 진도가 길어졌다. 또한 iterator를 배우기 전에 invalidation을 다루고, unique ownership이 RAII보다 한참 뒤에 등장하며, 실제 빌드 구조가 후반에 몰리는 선수 관계 문제가 있었다.

새 설계는 다음 원칙으로 정리한다.

1. C 복습은 차이와 진단 중심으로 압축한다.
2. reference → class/object → lifetime/resource → RAII/unique ownership → copy → move/value category 순서를 고정한다.
3. STL은 container → iterator → algorithm → callable/lambda의 관계로 학습한다.
4. `new/delete`는 원리와 실패를 이해하기 위한 학습 대상으로만 다루고 기본 설계는 RAII와 Rule of Zero로 둔다.
5. toolchain은 처음에 관찰하고, 마지막에 multi-file/library/CMake와 systems 경계로 다시 통합한다.
6. 문법 목록보다 선택 기준, 비용, lifetime, invalidation, 오류 경로를 설명한다.

## 사용법

- `C++ 8-7 공부하자`: 지정한 Step을 독립 튜토리얼로 학습한다.
- 각 Step의 note를 학습한 뒤 같은 번호의 exercise를 수행한다.
- 기간이 아니라 개념적 선후관계에 따라 Part 순서대로 진행한다.
- Step 제목에 표시된 표준 태그가 없으면 C++17을 기준으로 한다.

## 빌드 기준

일반 Step:

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g main.cpp -o main
```

C++20 Step:

```bash
g++ -std=c++20 -Wall -Wextra -pedantic -g main.cpp -o main
```

메모리와 undefined behavior를 관찰하는 Step에서는 필요에 따라 `-fsanitize=address`와 `-fsanitize=undefined`를 사용한다. Sanitizer가 조용하다는 사실을 undefined behavior가 없다는 증명으로 해석하지 않는다.

## 학습 흐름

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
→ operator·callable
→ template
→ STL container·iterator·algorithm·lambda
→ modern utility·advanced ownership
→ 실제 프로그램 구조·systems/embedded C++
```

## 각 Step의 교재 구조

1. 학습 목표
2. 선수 지식
3. 개념
4. 문법
5. 프로그램 내부에서 어떻게 동작하는가
6. 예제 코드
7. 실행 결과 분석
8. C와의 차이 — 필요한 경우만
9. 자주 하는 실수
10. 핵심 정리
11. 직접 확인할 문제
12. 다음 Step

설명은 **왜 필요한가 → 무엇인가 → 문법 → 내부 동작 → 코드 → 실행 결과 → 잘못 사용하면 어떻게 되는가** 순서를 기본으로 한다.

## C와 비교하는 기준

필요한 Step에서만 다음 형식을 사용한다.

```text
C에서는:
C++에서는:
차이가 생기는 이유:
```

반드시 연결할 주제는 `printf/std::cout`, `scanf/std::cin`, C cast/C++ named cast, pointer/reference, `malloc/free`와 `new/delete`, C array/`std::array`/`std::vector`, char array/`std::string`, struct/class, function overloading, namespace, resource management, procedural design/class abstraction이다.

---

## Part 0. C++ 개발 환경과 Build

디렉터리: [notes](./notes/00-toolchain-and-build/) · [exercises](./exercises/00-toolchain-and-build/)

- 0-1. C++ 소스, 실행 파일, 최소 빌드
- 0-2. 전처리·컴파일·어셈블·링크와 translation unit
- 0-3. g++ 표준 선택과 -Wall -Wextra -pedantic -g
- 0-4. compiler warning·compile error·linker error·runtime error
- 0-5. GDB: breakpoint·step·backtrace·variable inspection
- 0-6. AddressSanitizer와 UndefinedBehaviorSanitizer
- 0-7. Part 0 종합: 빌드 결과를 단계별로 진단하기

## Part 1. C에서 C++로

디렉터리: [notes](./notes/01-from-c-to-cpp/) · [exercises](./exercises/01-from-c-to-cpp/)

- 1-1. C 프로그램과 C++ 프로그램의 공통점과 차이
- 1-2. int main()과 C++ 프로그램의 시작
- 1-3. std::cout·std::cerr와 출력 stream
- 1-4. std::cin·입력 상태·오류 복구
- 1-5. std::getline과 token 입력·line 입력
- 1-6. namespace·qualified name·using declaration
- 1-7. Part 1 종합: 안전한 대화형 프로그램

## Part 2. Type System, Initialization, const, Conversion

디렉터리: [notes](./notes/02-types-initialization-and-conversion/) · [exercises](./exercises/02-types-initialization-and-conversion/)

- 2-1. object·value·type과 fundamental type
- 2-2. 정수·실수·문자·bool의 범위와 sizeof
- 2-3. default·value·direct·list initialization
- 2-4. 초기화되지 않은 값과 narrowing 방지
- 2-5. const 객체와 수정 가능성
- 2-6. auto와 decltype type deduction [C++11]
- 2-7. implicit conversion·promotion·signed/unsigned
- 2-8. explicit conversion·static_cast와 C-style cast

## Part 3. Expression과 Control Flow

디렉터리: [notes](./notes/03-expressions-and-control-flow/) · [exercises](./exercises/03-expressions-and-control-flow/)

- 3-1. 식·연산자·우선순위·평가와 side effect
- 3-2. if·switch·조건식과 scope
- 3-3. while·do-while·for와 반복 상태
- 3-4. 경계값·off-by-one·signed/unsigned 비교
- 3-5. early return과 불변식을 드러내는 제어 흐름

## Part 4. Function과 Interface

디렉터리: [notes](./notes/04-functions-and-interfaces/) · [exercises](./exercises/04-functions-and-interfaces/)

- 4-1. 함수 선언·정의·호출
- 4-2. parameter·argument·return by value
- 4-3. 지역 scope·static local·function call
- 4-4. function overloading과 overload resolution
- 4-5. default argument와 inline 함수
- 4-6. function pointer·type alias·callback 기초
- 4-7. 계약이 드러나는 작은 함수 interface

## Part 5. Reference, Pointer, Array

디렉터리: [notes](./notes/05-references-pointers-and-arrays/) · [exercises](./exercises/05-references-pointers-and-arrays/)

- 5-1. lvalue reference의 의미·선언·binding
- 5-2. const reference와 temporary lifetime extension
- 5-3. pointer와 reference의 의미·nullability 차이
- 5-4. pointer to const·const pointer·nullptr [C++11]
- 5-5. built-in array·pointer decay·pointer arithmetic
- 5-6. 값·pointer·reference·const reference parameter 선택
- 5-7. reference·pointer 반환과 dangling lifetime
- 5-8. ownership·borrowing·non-owning interface

## Part 6. std::string, std::array, std::vector

디렉터리: [notes](./notes/06-strings-arrays-and-vectors/) · [exercises](./exercises/06-strings-arrays-and-vectors/)

- 6-1. char array와 std::string의 값·소유권 차이
- 6-2. std::string 입력·결합·검색
- 6-3. std::array와 고정 크기 값 타입 [C++11]
- 6-4. std::vector의 size·capacity·연속 저장
- 6-5. range-based for의 값·reference 순회 [C++11]
- 6-6. array·string·vector의 선택과 invalidation

## Part 7. Class와 Object

디렉터리: [notes](./notes/07-classes-and-objects/) · [exercises](./exercises/07-classes-and-objects/)

- 7-1. C struct·C++ struct·class와 object
- 7-2. public·private와 class invariant
- 7-3. constructor·default constructor·explicit
- 7-4. member initializer list와 초기화 순서
- 7-5. this pointer와 member function
- 7-6. const member function과 const object
- 7-7. static data member와 static member function
- 7-8. composition과 절차적 설계에서 객체 설계로

## Part 8. Object Lifetime, Resource, RAII

디렉터리: [notes](./notes/08-lifetime-resources-and-raii/) · [exercises](./exercises/08-lifetime-resources-and-raii/)

- 8-1. scope·storage duration·object lifetime
- 8-2. automatic·static·dynamic storage duration
- 8-3. new/delete와 malloc/free의 의미 차이
- 8-4. new[]·delete[]와 수동 배열 lifetime
- 8-5. leak·dangling pointer·double delete·use-after-free
- 8-6. destructor와 파괴 순서
- 8-7. RAII: resource를 object lifetime에 결합하기
- 8-8. unique_ptr·make_unique와 unique ownership [C++11/14]
- 8-9. scope exit·exception unwinding 기초와 자동 cleanup

## Part 9. Copy Semantics

디렉터리: [notes](./notes/09-copy-semantics/) · [exercises](./exercises/09-copy-semantics/)

- 9-1. compiler가 생성하는 special member function
- 9-2. copy constructor와 복사 초기화
- 9-3. copy assignment와 이미 존재하는 객체
- 9-4. shallow copy·이중 소유·deep copy
- 9-5. resource-owning class와 Rule of Three
- 9-6. self-assignment·exception safety·copy-and-swap
- 9-7. 값 의미와 Rule of Zero

## Part 10. Move Semantics와 Value Category

디렉터리: [notes](./notes/10-move-semantics-and-value-categories/) · [exercises](./exercises/10-move-semantics-and-value-categories/)

- 10-1. lvalue·xvalue·prvalue와 표현식의 값 범주
- 10-2. rvalue reference와 reference binding [C++11]
- 10-3. 값 범주에 따른 overload resolution
- 10-4. std::move는 이동을 허용하는 cast [C++11]
- 10-5. move constructor와 소유권 이전
- 10-6. move assignment와 기존 resource 정리
- 10-7. moved-from object·noexcept·container relocation
- 10-8. Rule of Five와 Rule of Zero
- 10-9. copy elision·return value·C++17 prvalue

## Part 11. Inheritance와 Runtime Polymorphism

디렉터리: [notes](./notes/11-inheritance-and-polymorphism/) · [exercises](./exercises/11-inheritance-and-polymorphism/)

- 11-1. composition과 inheritance 선택
- 11-2. base·derived class와 public inheritance
- 11-3. base·member·derived 생성과 파괴 순서
- 11-4. virtual dispatch·override·final [C++11]
- 11-5. pure virtual function과 abstract interface
- 11-6. base pointer/reference·object slicing
- 11-7. virtual destructor와 polymorphic deletion
- 11-8. RTTI·dynamic_cast와 생성자·소멸자 안의 virtual call

## Part 12. Operator, Enum, Callable

디렉터리: [notes](./notes/12-operators-enums-and-callables/) · [exercises](./exercises/12-operators-enums-and-callables/)

- 12-1. operator overloading의 목적·제한·관례
- 12-2. 산술·비교 operator와 대칭성
- 12-3. operator<<·friend와 제한된 접근
- 12-4. operator[]의 const·non-const overload
- 12-5. enum class와 type-safe state [C++11]
- 12-6. explicit conversion과 function object·operator()

## Part 13. Template와 Generic Programming

디렉터리: [notes](./notes/13-templates-and-generic-programming/) · [exercises](./exercises/13-templates-and-generic-programming/)

- 13-1. generic programming과 function template
- 13-2. template argument deduction과 instantiation
- 13-3. class template와 표준 타입 읽기
- 13-4. type parameter와 non-type template parameter
- 13-5. template definition을 header에 두는 이유
- 13-6. overload·specialization·if constexpr [C++17]
- 13-7. dependent name·typename·compile diagnostic
- 13-8. concepts·requires 입문 [C++20, 선택]

## Part 14. STL Container

디렉터리: [notes](./notes/14-stl-containers/) · [exercises](./exercises/14-stl-containers/)

- 14-1. container·element·allocator·복잡도 모델
- 14-2. array·vector의 memory organization과 접근
- 14-3. deque·list의 저장 구조와 삽입·삭제
- 14-4. map·set과 strict weak ordering
- 14-5. unordered_map·hash·equality·rehash [C++11]
- 14-6. 공통 연산: access·insert·emplace·erase
- 14-7. iterator·reference invalidation 비교
- 14-8. locality·Big-O·ownership으로 container 선택

## Part 15. Iterator, Algorithm, Lambda

디렉터리: [notes](./notes/15-iterators-algorithms-and-lambdas/) · [exercises](./exercises/15-iterators-algorithms-and-lambdas/)

- 15-1. iterator·반열린 범위·iterator category
- 15-2. begin·end·const iterator와 algorithm 계약
- 15-3. find·count·transform으로 순회 분리
- 15-4. sort·comparator·strict weak ordering
- 15-5. accumulate와 fold 사고방식
- 15-6. lambda와 closure object [C++11]
- 15-7. capture·generic lambda·lifetime [C++14]
- 15-8. function pointer·function object·std::function 선택
- 15-9. erase-remove idiom과 std::erase [C++20]

## Part 16. Modern C++와 Ownership

디렉터리: [notes](./notes/16-modern-cpp-and-ownership/) · [exercises](./exercises/16-modern-cpp-and-ownership/)

- 16-1. constexpr·constant evaluation·static_assert [C++11]
- 16-2. pair·tuple·structured binding [C++17]
- 16-3. std::optional과 값의 부재 [C++17]
- 16-4. std::variant·std::visit과 대안 타입 [C++17]
- 16-5. std::string_view와 non-owning lifetime [C++17]
- 16-6. custom deleter와 C resource의 unique ownership
- 16-7. shared_ptr·weak_ptr·control block·cycle [C++11]
- 16-8. exception·stack unwinding·exception safety
- 16-9. file stream과 std::filesystem [C++17]
- 16-10. std::chrono와 type-safe 시간 [C++11]
- 16-11. std::span과 ranges/view의 lifetime [C++20, 선택]

## Part 17. 실제 C++ 프로그램 구조와 Systems/Embedded C++

디렉터리: [notes](./notes/17-program-structure-and-systems-cpp/) · [exercises](./exercises/17-program-structure-and-systems-cpp/)

- 17-1. header·source·declaration·definition 분리
- 17-2. self-contained header·include guard·#pragma once
- 17-3. translation unit·linkage·One Definition Rule
- 17-4. namespace·forward declaration·dependency 관리
- 17-5. 여러 source file의 개별 compile과 link
- 17-6. static library·shared library·ABI 경계
- 17-7. CMake target으로 executable과 library 구성
- 17-8. warning·test·debug·sanitizer build 통합
- 17-9. object representation·sizeof·alignment·padding
- 17-10. function call·stack frame·object lifetime 관찰
- 17-11. aliasing·raw storage·reinterpret_cast·const_cast와 byte 복사
- 17-12. C interoperability와 extern C
- 17-13. raw pointer가 필요한 경계와 allocation 제한
- 17-14. volatile·memory-mapped I/O의 정확한 의미
- 17-15. thread lifetime·mutex·RAII locking [C++11]
- 17-16. std::atomic·data race·기초 memory ordering [C++11]
- 17-17. freestanding·exception·RTTI·STL 사용 정책

---

## 통합 프로젝트 진입점

프로젝트 문서는 각 진입점에 도달했을 때 별도로 추가하며 이번 커리큘럼 Step 수에는 포함하지 않는다.

### Project 1. 안전한 센서 데이터 CLI

진입점: Part 6 완료 후

- stream 입력 상태와 잘못된 입력 처리
- `std::string`, `std::array`, `std::vector`
- 함수 분리와 borrowing interface
- warning 없는 C++17 build

### Project 2. RAII 기반 Buffer와 C resource adapter

진입점: Part 10 완료 후

- manual resource owner의 copy/move 실패 모드 관찰
- Rule of Three/Five 구현과 Rule of Zero 재설계 비교
- `unique_ptr`와 custom deleter
- sanitizer로 leak·double delete·use-after-free 진단

### Project 3. 다형적 Device Registry

진입점: Part 15 완료 후

- abstract interface와 virtual destructor
- `unique_ptr` 기반 polymorphic ownership
- associative container와 algorithm·lambda
- shared ownership이 필요한지 근거로 판단

### Project 4. Multi-file Device Trace Inspector

진입점: Part 17 완료 후

- bounded binary input 검증과 오류 표현
- decoding·value type·index·presentation 모듈 분리
- `optional`·`variant`·view의 lifetime 검토
- static/shared library와 C ABI 경계
- CMake build, deterministic test, sanitizer 검증
- hosted 환경과 embedded target 정책 비교

## Exercise 설계 원칙

각 핵심 Step의 exercise는 다음 유형을 섞는다.

1. 기본 구현
2. 출력 결과 예측
3. 컴파일 오류 원인 분석
4. 잘못된 코드 수정
5. memory/lifetime 문제 찾기
6. 작은 기능 직접 구현

특히 reference, lifetime, copy, move, ownership에서는 코드를 읽고 어떤 일이 발생하는지 추론하는 문제를 포함한다. 실제 Step별 note와 exercise 본문은 다음 작업부터 생성한다.

## 검증 기준과 참고 자료

언어 규칙과 라이브러리 동작은 다음 자료를 우선 확인한다.

- [cppreference](https://en.cppreference.com/w/cpp/)
- [ISO C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [GCC 공식 문서](https://gcc.gnu.org/onlinedocs/)
- [CMake 공식 튜토리얼](https://cmake.org/cmake/help/latest/guide/tutorial/)

표준이 보장하는 의미와 GCC·Linux·ABI·CPU·target에서 관찰되는 결과를 구분하고, 정확하지 않은 표준 내용은 단정하지 않는다.

## 완주 기준

- type, initialization, reference, pointer, lifetime, ownership의 차이를 코드로 설명한다.
- constructor·destructor·copy·move가 호출되는 조건을 추적한다.
- RAII와 Rule of Zero를 기본 설계로 사용하고 raw ownership이 필요한 경계를 설명한다.
- runtime polymorphism과 template 기반 compile-time abstraction을 비교한다.
- container를 저장 구조·복잡도·locality·invalidation으로 선택한다.
- non-owning view와 smart pointer의 lifetime 조건을 검토한다.
- multi-file build의 declaration·definition·linkage·ODR 문제를 진단한다.
- C++ 표준의 보장과 compiler·OS·ABI·hardware의 구현 관찰을 구분한다.
- embedded 환경의 allocation·exception·RTTI·STL 정책을 비용과 제약으로 설명한다.
