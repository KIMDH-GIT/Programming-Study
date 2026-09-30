# Step 0-3 Exercise — g++ 표준 선택과 -Wall -Wextra -pedantic -g

학습 문서: [Step 0-3 — g++ 표준 선택과 -Wall -Wextra -pedantic -g](../../../notes/00-toolchain-and-build/0-3-gpp-standard-selection-and-warning-debug-options.md)

## 실습 목적

- compiler version과 C++ language mode를 구분한다.
- 같은 source에 option을 하나씩 추가하며 diagnostic 차이를 관찰한다.
- warning이 발생한 build의 성공 여부와 executable 생성을 직접 확인한다.
- `-g`가 warning option이 아니라 debug information 생성 option임을 확인한다.

## 준비

이 exercise directory로 이동해 compiler version을 기록한다.

```bash
cd exercises/00-toolchain-and-build/0-3
g++ --version
```

- GCC version / 선택한 C++ standard:
- 두 값이 나타내는 대상의 차이:

`diagnostics.cpp`를 다음과 같이 작성한다.

```cpp
int inspect(int input)
{
    int unused_value = 10;
    int count = 3;
    int values[count]{};
    return values[0];
}

int main()
{
    return inspect(1);
}
```

각 실습에서는 command를 실행하기 전에 warning, compile 성공 여부, executable 생성 여부를 먼저 예측한다.

## 실습 1 — 기본 Compile

```bash
g++ -std=c++17 diagnostics.cpp -o diagnostics-base
```

- 예측 — diagnostic / compile / executable:
- 관찰 — diagnostic / compile / executable:

## 실습 2 — `-Wall`

```bash
g++ -std=c++17 -Wall diagnostics.cpp -o diagnostics-wall
```

- 예측 — 새 warning / compile / executable:
- 관찰 — 새 warning과 `[-W...]` 이름 / source line:

## 실습 3 — `-Wextra`

```bash
g++ -std=c++17 -Wall -Wextra diagnostics.cpp -o diagnostics-extra
```

- 예측 — `-Wall` 결과에 추가될 warning:
- 관찰 — 추가된 warning과 대상 / compile / executable:

## 실습 4 — `-pedantic`

```bash
g++ -std=c++17 -Wall -Wextra -pedantic diagnostics.cpp -o diagnostics-pedantic
```

- 예측 — 표준 적합성 diagnostic / compile / executable:
- 관찰 — 추가된 diagnostic / GCC extension / compile / executable:
- `-pedantic`의 판단 기준:

## 실습 5 — `-g`

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g diagnostics.cpp -o diagnostics-debug
```

- 예측 — warning 변화 / compile / executable:
- 관찰 — warning 변화 / compile / executable:

`-g`가 만든 정보를 Linux 도구로 관찰한다.

```bash
file diagnostics-pedantic diagnostics-debug
readelf --sections --wide diagnostics-debug
```

- `file`의 debug information 표현:
- `.debug_...` section 두 개와 debugger가 활용할 정보:

GDB의 상세 명령은 아직 사용하지 않는다.

## Diagnostic 비교표

실제 결과를 요약한다. “있음”만 쓰지 말고 warning을 제어한 option 이름도 기록한다.

| build option | warning 종류 | compile 성공/실패 | executable 생성 | debug information |
|---|---|---|---|---|
| `-std=c++17` |  |  |  |  |
| `-std=c++17 -Wall` |  |  |  |  |
| `-std=c++17 -Wall -Wextra` |  |  |  |  |
| `-std=c++17 -Wall -Wextra -pedantic` |  |  |  |  |
| 위 option + `-g` |  |  |  |  |

## 실습 6 — Diagnostic을 읽고 수정

세 warning을 하나씩 없애도록 `diagnostics.cpp`를 수정한다.

`input`을 계산에 사용하고, 사용하지 않는 local variable을 정리하고, variable length array를 compile-time constant 크기의 배열로 바꾼다.

수정 후 기본 debugging build를 실행한다.

```bash
g++ -std=c++17 -Wall -Wextra -pedantic -g diagnostics.cpp -o diagnostics
./diagnostics
echo $?
```

- 수정한 line과 이유:
- 남은 diagnostic / program exit status:

## 생각해 볼 문제

1. GCC version과 C++ standard는 왜 같은 개념이 아닌가?
2. `-Wall`은 이름과 달리 왜 모든 warning을 의미하지 않는가?
3. `-Wall -Wextra`를 함께 사용하는 이유는 무엇인가?
4. warning 없이 compile됐다는 사실이 program의 정확성을 보장하는가?
5. `-pedantic`은 무엇을 기준으로 diagnostic을 내는가?
6. `-g`는 program을 실제로 debugging하는 option인가?
7. `-g`가 없으면 executable을 실행할 수 없는가?

## 힌트

- diagnostic 마지막의 `[-Wunused-variable]` 같은 표시는 관련 warning option을 알려 준다.
- warning이 있어도 command의 exit status와 output 파일을 별도로 확인한다.
- `-pedantic`은 `-std=`로 선택한 standard와 연결해서 해석한다.
- `file`의 `with debug_info`와 `readelf`의 `.debug_info`, `.debug_line`을 찾는다.
- `-g`와 warning option의 역할을 섞지 않는다.

## 완료 체크

- [ ] GCC version과 C++17의 차이를 설명했다.
- [ ] 다섯 build command를 순서대로 직접 실행했다.
- [ ] 각 단계에서 추가된 diagnostic을 기록했다.
- [ ] warning이 있어도 executable이 생성되는 경우를 확인했다.
- [ ] `-Wall`과 `-Wextra`의 차이를 실제 warning으로 확인했다.
- [ ] `-pedantic`이 진단한 GNU extension을 확인했다.
- [ ] `-g`를 사용한 executable의 debug information을 확인했다.
- [ ] 세 warning을 없애고 기본 debugging build로 다시 검증했다.
