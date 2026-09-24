# 24-8. internal linkage와 file-scope `static`
## 1. 학습 목표
- file-scope `static` function과 object의 internal linkage를 설명한다.
- 공개 function과 file-local helper를 구분한다.
- `static`을 storage duration 하나로만 설명하지 않는다.
## 2. 선수 지식
24-7 linkage와 Part 10의 scope를 안다.
## 3. 핵심 개념
file scope에서 `static`을 사용한 function 또는 object identifier는 internal linkage를 가질 수 있다. 같은 translation unit의 implementation에서만 필요한 이름을 감추는 데 유용하다.

| 이름 | 위치 | linkage | caller 사용 |
|---|---|---|---|
| `calculator_absolute` | header + source | external | 가능 |
| `normalize_sign` | source의 `static` | internal | 불가 |
## 4. 문법
```c
static int normalize_sign(int value);

static int normalize_sign(int value)
{
    return value < 0 ? -value : value;
}
```
## 5. 최소 코드 예제
`calculator.h`
```c
int calculator_absolute(int value);
```

`calculator.c`
```c
#include "calculator.h"

static int normalize_sign(int value)
{
    return value < 0 ? -value : value;
}

int calculator_absolute(int value)
{
    return normalize_sign(value);
}
```

`main.c`
```c
#include <stdio.h>

#include "calculator.h"

int main(void)
{
    printf("%d\n", calculator_absolute(-7));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c calculator.c -o calculator_app
./calculator_app
```
## 6. 코드 해석
caller는 header에 공개된 `calculator_absolute`만 사용한다. `normalize_sign`은 `calculator.c`의 implementation detail이며 public header에 declaration을 노출할 필요가 없다.
## 7. 내부 동작
**[preprocessor]** public declaration만 caller translation unit에 포함한다.

**[C translation unit]** `normalize_sign` declaration·definition·call은 `calculator.c` 기반 translation unit 안에 있다.

**[compiler]** internal-linkage name을 그 translation unit에 한정해 처리한다.

**[linker]** 다른 translation unit의 같은 internal-linkage spelling과 외부 entity 하나로 합치지 않는다.

**[OS / loader]** file-scope object를 다룬다면 static storage duration도 별도 속성으로 존재한다.

**[CPU / ISA]** internal linkage는 source-level name relation이며 특정 ISA의 private-function 기능이 아니다.
## 8. 자주 하는 실수
- `static`은 값 유지만 뜻한다고 말한다.
- private helper를 public header에 공개한다.
- `main.c`에 같은 declaration을 쓰면 static helper를 호출할 수 있다고 생각한다.
- header에 `static int counter;`를 두고 하나의 shared state라고 생각한다. 각 translation unit에 별개 internal-linkage object가 생길 수 있다.
- internal linkage와 scope를 같은 말로 사용한다.
## 9. 필수 실습
public function이 source 내부의 `static` helper를 호출하게 만든다.
[24-8 exercise](../../exercises/24-multi-file-programs/24-8/README.md)
## 10. 추가 실습
- ★ private helper 이름을 바꾸고 caller가 영향받지 않는지 확인한다.
- ★★ 두 `.c`에 같은 이름의 `static` helper를 각각 둔다.
- ★★★ public API와 implementation detail 표를 작성한다.
## 11. 확인 문제
1. file-scope `static` function의 핵심 효과는?
2. internal linkage와 scope는 같은 개념인가?
3. private helper를 header에 선언할 필요가 없는 이유는?
4. 다른 translation unit이 static helper를 같은 entity로 참조할 수 있는가?
5. header의 static object가 shared state가 아닌 이유는?
## 12. 핵심 정리
- file-scope `static`은 implementation name에 internal linkage를 줄 수 있다.
- 공개 contract는 header에, private helper는 implementation source에 둔다.
- linkage와 storage duration을 구분한다.
## 13. 다음 Step
[24-9. block-scope `static` 복습](24-9-block-scope-static-review.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.2, 6.2.4, 6.7.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Storage duration and linkage](https://en.cppreference.com/w/c/language/storage_duration)
- [cppreference: Function declaration](https://en.cppreference.com/w/c/language/function_declaration)
