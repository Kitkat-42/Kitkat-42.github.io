---
layout: single
title: "[ft_printf] 함수 'printf'에 대해"
categories: "ft_printf"
tags: [ft_printf, C]
---

# 1. 함수 printf

먼저 우리가 클론해야하는 함수 'printf'에 대해 알아보자.  
linux man 페이지에서 알아본 printf 함수의 원형은 다음과 같다.

> man 3 printf

```c
int	printf(const char * restrict format, ...);
```

우리가 지금까지 봐왔던 함수들의 프로토타입들과는 조금 다른 모습이다.  
strcmp의 프로토타입과 비교해보자.

```c
int strcmp(const char *s1, const char *s2);
```

인자의 타입과 개수가 명시되어 있는 strcmp와 달리, printf의 경우 마지막 인자가 `...`으로 표시되어 있다. printf 함수의 두 번째 인자로 사용되는 `...`이 바로 **가변 인자**라고 불리는 것이다.

# 2. 가변인자(Variadic arguments)

생각해보니, 우리는 지금까지 printf 함수를 쓸 때 자연스럽게 아래와 같이 여러 타입의 여러 매개변수(파라미터)를 넣어 사용하고 있었다.

```c
printf("%s %d * %d = %d", "Hello world!", 3, 5, 3*5);

<결과>
Hello world! 3 * 5 = 15
```

printf를 쓸 때, 매개변수를 아무것도 넘겨주지 않을 수도 있고, 여러 개의 매개변수를 넘겨줄 수도 있었다.

<img src="
https://m.blog.naver.com/angelcorean/220804530449?view=img_9" />

이런 방식으로 말이다.

## 2.1 <stdarg.h>

먼저, `가변인자 함수`를 만들기 위해서는 `stdarg.h` 헤더파일을 포함해야 한다. `stdarg.h`는 가변인자들의 리스트가 담긴 헤더 파일이다. 이 헤더 파일에 가변인자 함수를 만들 때 필요한 각종 매크로들이 정의되어 있다.

> man 3 stdarg

```
NAME
stdarg -- variable argument lists

SYNOPSIS
#include <stdarg.h>

     void
     va_start(va_list ap, last);

     type
     va_arg(va_list ap, type);

     void
     va_copy(va_list dest, va_list src);

     void
     va_end(va_list ap);
```

위 네가지(`va_start()`, `va_arg()`, `va_copy()`, `va_end()`)가 헤더파일에 포함된 매크로들이다.
각 함수들이 가진 인자의 타입 `va_list`는 **각 가변인자의 시작 주소를 가리킬 포인터**이다. `va_list`는 이 헤더파일에 선언되어 있다.

- **va_start()**: `va_list`로 만들어진 포인터에게

가변인자 함수를 선언할 때는 **최소 1개 이상의 고정 인수**가 있어야 한다. 가변인자를 나타내는 `...`은 **인자 순서 상 가장 뒤**에 있어야 한다.

사용 예시를 보자.

```c
void foo(char *fmt, ...)
{
    va_list ap;
    int d;
    char c, *s;

    va_start(ap, fmt);
    while (*fmt)
        switch(*fmt++) {
        case 's':                       /* string */
            s = va_arg(ap, char *);
            printf("string %s\n", s);
            break;
        case 'd':                       /* 정수 */
            d = va_arg(ap, int);
            printf("int %d\n", d);
            break;
        case 'c':                       /* 캐릭터 */
            /* 주의: 캐릭터는 정수를 생성한다.  */
            c = va_arg(ap, int);
            printf("char %c\n", c);
            break;
        }
    va_end(ap);
}
```
