# 22-10. 모의 GPIO driver와 callback
## 1. 학습 목표
- mock GPIO operations와 change callback을 function pointers로 연결한다.
- bounds·null·signature checks를 지키며 defined behavior만 실행한다.
- driver operation과 notification callback의 역할을 구분한다.
## 2. 선수 지식
Part 21 bit operations, 22-5 callback, 22-9 driver interface를 안다.
## 3. 핵심 개념
mock driver는 실제 hardware 없이 register state 변화를 C object로 관찰한다. driver interface는 state를 변경하고, callback은 변경 뒤 caller에게 값을 알린다.
```c
struct GpioDriver {
    void (*write)(unsigned int value);
    unsigned int (*read)(void);
};
typedef void (*GpioCallback)(unsigned int value);
```
`write`와 `GpioCallback`은 이 예제에서 모두 `void (unsigned int)` function을 가리키므로 C type은 같다. 하지만 하나는 driver operation이고 다른 하나는 변경 알림이라는 semantic role을 가진다. C type system만으로 두 역할을 구분할 수 없으므로 용도에 맞는 typedef name과 member name을 사용한다.
## 4. 문법
```c
void update_gpio(
    const struct GpioDriver *driver,
    unsigned int value,
    GpioCallback callback
);
```
`driver`는 구조체 object를 가리키는 object pointer이고, members와 callback은 function pointers다. `void *`로 섞지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static unsigned int gpio_register;

void mock_write(unsigned int value) { gpio_register = value; }
unsigned int mock_read(void) { return gpio_register; }
void print_change(unsigned int value) { printf("GPIO=%u\n", value); }

struct GpioDriver {
    void (*write)(unsigned int value);
    unsigned int (*read)(void);
};

typedef void (*GpioCallback)(unsigned int value);

void update_gpio(
    const struct GpioDriver *driver,
    unsigned int value,
    GpioCallback callback
)
{
    driver->write(value);
    if (callback != NULL) {
        callback(driver->read());
    }
}

int main(void)
{
    const struct GpioDriver driver = {
        .write = mock_write,
        .read = mock_read
    };

    update_gpio(&driver, 5u, print_change);
    return 0;
}
```
## 6. 코드 해석
`update_gpio`는 driver의 write operation으로 mock register를 바꾼다. callback이 null이 아니면 read result를 값으로 전달한다. `const struct GpioDriver *`의 const는 interface object를 이 function이 수정하지 않겠다는 뜻이며 function 자체를 const-qualified한다는 뜻이 아니다.
## 7. 내부 동작
**[C17 type system]** `driver`는 object pointer이고 `write`, `read`, `callback`은 각기 compatible function pointers다. callback argument `value`와 pointer value는 모두 pass-by-value다.

**[compiler]** structure member를 통해 operations를 호출하고 callback null check 뒤 call을 생성한다.

**[ABI]** 실제 GPIO driver는 memory-mapped I/O 규칙과 target ABI가 추가되지만 mock example에는 hardware access가 없다.

**[CPU / ISA]** mock state는 ordinary C object이고 실제 register semantics를 자동으로 갖지 않는다.
## 8. 자주 하는 실수
- mock object를 실제 hardware register와 같다고 설명한다.
- `driver` object pointer와 function pointer members를 모두 `void *`로 일반화한다.
- callback이 null인지 검사하지 않는다.
- callback에 pointer가 전달되므로 call-by-reference라고 말한다. 이 예제는 unsigned value와 function pointer value를 복사해 전달한다.
- local nested callback을 작성한다. nested function은 ISO C17 기능이 아니다.
## 9. 필수 실습
mock write/read operations와 change callback을 연결해 두 GPIO values를 순서대로 관찰한다.
[22-10 exercise](../../exercises/22-function-pointers/22-10/README.md)
## 10. 추가 실습
- ★ callback 없이 update하여 state만 바꾼다.
- ★★ callback에서 마지막 값을 누적한다.
- ★★★ bit mask를 적용한 set/clear operations를 interface에 추가한다.
## 11. 확인 문제
1. `driver` parameter와 `callback` parameter는 각각 어떤 pointer category인가?
2. callback 호출 전 null check가 필요한 이유는?
3. callback 전달이 pass-by-value인 이유는?
4. `const struct GpioDriver *`가 const로 만드는 대상은?
5. mock register가 실제 MMIO register와 다른 점은?
6. nested callback을 C17 예제로 쓰면 안 되는 이유는?
## 12. 핵심 정리
- mock driver operations와 notification callback을 compatible signatures로 연결한다.
- object pointer와 function pointer를 구분한다.
- null check와 defined behavior를 유지한다.
## 13. 다음 Step
[22-11. Part 22 종합 복습](22-11-part-22-review.md)
## 14. 참고 자료
- N1570 6.3.2.3, 6.5.2.2, 6.5.2.3, 6.7.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: const type qualifier](https://en.cppreference.com/w/c/language/const)
- [cppreference: pointer declaration](https://en.cppreference.com/w/c/language/pointer)
