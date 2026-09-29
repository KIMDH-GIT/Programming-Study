# 31-12. driver interface의 const correctness
## 1. 학습 목표
- read-only driver operation과 mutable operation을 type으로 구분한다.
- `const`가 ROM 배치나 deep const를 뜻하지 않음을 설명한다.
- volatile member와 const-qualified aggregate access를 구분한다.
## 2. 선수 지식
Part 17의 const pointer, 31-2의 volatile access, 31-4의 safe mock을 안다.
## 3. 핵심 개념
driver interface에서 logical read operation은 `const Device *`를 받아 structure를 통해 변경하지 않겠다는 계약을 표현할 수 있다. write operation은 mutable `Device *`가 필요하다.

`const`는 object를 ROM에 배치한다는 뜻이 아니며 다른 alias를 통한 변경을 막는 synchronization 장치도 아니다. pointer member가 가리키는 별도 object까지 자동으로 deep const가 되는 것도 아니다.
## 4. 문법
```c
static unsigned read_status(const Device *device);
static void write_control(Device *device, unsigned value);
```
두 함수 모두 non-NULL이며 lifetime이 유효한 `Device`를 precondition으로 받는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef struct {
    volatile unsigned control;
    volatile unsigned status;
} Device;

static unsigned read_status(const Device *device)
{
    return device->status;
}

static void write_control(Device *device, unsigned value)
{
    device->control = value;
}

int main(void)
{
    Device mock = {0u, 9u};
    write_control(&mock, 3u);
    printf("control=%u status=%u\n", mock.control, read_status(&mock));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o const_driver
./const_driver
```
## 6. 코드 해석
출력은 `control=3 status=9`다. read interface는 `Device` member를 수정하지 않으며 write interface만 control을 변경한다.
## 7. 내부 동작
**[C17 abstract machine]** `const Device *`를 통한 modifiable lvalue 생성은 제한된다. volatile member read의 access 의미는 여전히 적용된다.

**[compiler / device]** const qualifier만으로 ROM section, read-only bus, device register property가 정해지지 않는다.

API의 const correctness는 ownership과 mutation surface를 좁히지만 concurrent device change나 atomic snapshot을 보장하지 않는다.
## 8. 자주 하는 실수
- const object는 반드시 ROM에 있다고 말한다.
- `const Device *`가 device 상태 변화를 막는다고 생각한다.
- pointer member까지 자동 deep const라고 가정한다.
- read function에서 cast로 const를 제거해 write한다.
## 9. 필수 실습
read와 write function signature를 분리하고 read 함수가 state를 변경하지 않는지 확인한다. [31-12 exercise](../../exercises/31-system-embedded-c/31-12/README.md)
## 10. 추가 실습
- ★ status와 control getter를 분리한다.
- ★★ read-only configuration view를 추가한다.
- ★★★ const를 제거하는 cast가 계약을 깨는 이유를 설명한다.
## 11. 확인 문제
1. `const Device *`가 표현하는 계약은?
2. const가 ROM 배치를 보장하지 않는 이유는?
3. volatile member와 const aggregate는 어떻게 함께 적용되는가?
4. const가 synchronization이 아닌 이유는?
5. deep const가 자동으로 성립하지 않는 예는?
## 12. 핵심 정리
- read/write interface를 qualifier로 구분한다.
- const는 access 경로의 수정 제한이지 storage 위치가 아니다.
- device와 concurrency 보장은 별도 규약이다.
## 13. 다음 Step
[31-13. driver function pointer 호환형](31-13-compatible-driver-function-pointers.md)
## 14. 참고 자료
- WG14 N1570 6.5.16.1, 6.7.3. N1570은 C11 공개 Committee Draft이며 qualifier/assignment 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
