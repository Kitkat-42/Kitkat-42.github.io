---
layout: single
title: "[ft_printf] 가변 인자 함수"
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

인자(매개변수)의 타입과 개수가 명시되어 있는 strcmp와 달리, printf의 경우 마지막 인자가 `...`으로 표시되어 있다. printf 함수의 두 번째 인자로 사용되는 `...`이 바로 **가변 인자**라고 불리는 것이다.

# 2. 가변인자(Variadic arguments)

생각해보니, 우리는 지금까지 printf 함수를 쓸 때 자연스럽게 아래와 같이 여러 타입의 여러 매개변수(파라미터)를 넣어 사용하고 있었다.

```c
int main(){
    printf("%s %d * %d = %d", "Hello world!", 3, 5, 3*5);
}

결과> Hello world! 3 * 5 = 15
```

printf를 쓸 때, 매개변수를 아무것도 넘겨주지 않을 수도 있고, 이렇게 여러 개의 매개변수를 넘겨줄 수도 있었다. 이렇게 매번 함수에 들어가는 인자(argument)의 개수가 변하는 함수를 가변 인자 함수라고 한다.

## 2.1 가변 인자 함수 만들기

가변 인자 함수를 선언할 때는 **최소 1개 이상의 고정 인자**가 있어야 하며, 고정 인자 뒤에 `...`을 붙여 인자의 개수가 정해지지 않았다는 표시를 해준다. 단, 가변인자를 나타내는 `...`은 **인자 순서 상 가장 뒤**에 있어야 한다.

```c
#include <stdio.h>

// args는 고정 인자
void printNumbers(int args, ...)
{
    printf("%d ", args);
}

int main()
{
    printNumbers(1, 10);
    printNumbers(2, 10, 20);
    printNumbers(3, 10, 20, 30);
    printNumbers(4, 10, 20, 30, 40);

    return 0;
}
```

실행 결과>

```
1 2 3 4
```

첫번째 인자 args의 값만 출력이 되었다. printNumbers(4, 10, 20, 30, 40);이면 처음에 오는 4는 매개변수 args에 들어가고 나머지 값들은 `...` 부분에 들어간다. 이제 `...` 부분의 매개변수를 사용해보자.

## 2.2 <stdarg.h>

`...`으로 들어온 가변 인자를 사용하려면 `stdarg.h` 헤더파일에 정의된 매크로를 이용해야 한다. `stdarg.h`는 가변인자 처리 매크로가 담긴 헤더 파일이다.

> man 3 stdarg

### va_list

각 가변 인자의 시작 주소를 가리킬 포인터이다.

### va_start

```c
  void va_start(va_list ap, variable_name);
```

`va_list`로 만들어진 포인터에게 **가변인자 중 첫번째 선택적 인수(variable_name)의 주소를 가르쳐주는** 매크로이다.

- ap:
- variable_name:

### va_copy

### va_arg

가변 인자 포인터에서 특정 자료형 크기만큼 값을 가져온다.

### va_end

가변 인자 처리가 끝났을 때 포인터를 NULL로 초기화한다.

매크로의 설명들만 보면 당최 무슨 뜻인지 이해가 가지 않으니 가변 인자 함수를 만드는 예시를 보자.

```c
#include <stdio.h>
#include <stdarg.h>    // va_list, va_start, va_arg, va_end가 정의된 헤더 파일

void printNumbers(int args, ...)    // 가변 인자의 개수를 받음, ...로 가변 인자 설정
{
    va_list ap;    // 가변 인자 목록 포인터

    va_start(ap, args);    // 가변 인자 목록 포인터 설정
    for (int i = 0; i < args; i++)    // 가변 인자 개수만큼 반복
    {
        int num = va_arg(ap, int);    // int 크기만큼 가변 인자 목록 포인터에서 값을 가져옴
                                      // ap를 int 크기만큼 순방향으로 이동
        printf("%d ", num);           // 가변 인자 값 출력
    }
    va_end(ap);    // 가변 인자 목록 포인터를 NULL로 초기화

    printf("\n");    // 줄바꿈
}

int main()
{
    printNumbers(1, 10);                // 인수 개수 1개
    printNumbers(2, 10, 20);            // 인수 개수 2개
    printNumbers(3, 10, 20, 30);        // 인수 개수 3개
    printNumbers(4, 10, 20, 30, 40);    // 인수 개수 4개

    return 0;
}
```

실행결과

```c
10
10 20
10 20 30
10 20 30 40
```

먼저 함수를 정의할 때 void printNumbers(int args, ...)와 같이 첫 번째 매개변수에서 가변 인자의 개수를 받을 수 있도록 지정하고, 두 번째 매개변수 부분에서 가변 인자를 받을 수 있도록 `...`으로 지정한다.

```c
void printNumbers(int args, ...)    // 가변 인자의 개수를 받음, ...로 가변 인자 설정
```

printNumbers 함수를 호출할 때는 맨 처음에 인수 개수를 넣어준 뒤 인수 개수에 맞게 인수를 콤마로 구분해 넣어준다.

```c
int main()
{
    printNumbers(1, 10);                // 인수 개수 1개
    printNumbers(2, 10, 20);            // 인수 개수 2개
    printNumbers(3, 10, 20, 30);        // 인수 개수 3개
    printNumbers(4, 10, 20, 30, 40);    // 인수 개수 4개

    return 0;
}
```

다시 printNumbers 함수 안을 살펴보자. 함수 안에서는 va_start 매크로에 가변 인자 목록 포인터 ap와 가변 인자 개수 args를 넣어 가변 인자를 가져올 수 있도록 준비한다. (ap는 argument pointer를 줄여서 쓴 것이다.)

```c
va_list ap;    // 가변 인자 목록 포인터

va_start(ap, args);    // 가변 인자 목록 포인터 설정
```

만약 가변 인자가 4개 들어있는 printNumbers(4, 10, 20, 30, 40);를 호출한 뒤 va_start 매크로를 실행하면 다음과 같은 모양이 된다.

<img src="https://dojang.io/pluginfile.php/641/mod_page/content/29/unit66-1.png" />

이제 반복문으로 가변 인자 개수만큼 반복하면서 va_arg 매크로로 값을 가져오면 된다. 이 때 va_arg에는 가변 인자의 자료형을 지정해준다.

```c
for (int i = 0; i < args; i++)    // 가변 인자 개수만큼 반복
    {
        int num = va_arg(ap, int);    // int 크기만큼 가변 인자 목록 포인터에서 값을 가져옴
                                      // ap를 int 크기만큼 순방향으로 이동
        printf("%d ", num);           // 가변 인자 값 출력
    }
```

## 2.2 자료형이 다른 가변 인자 함수 만들기

각각 자료형이 다른 가변 함수를 만들기 위해서는 가변인자의 자료형을 받아 각 자료형일 때 따로 처리해주는 함수를 만들면 된다.

먼저 가변 인자를 처리할 때 사용할 각 자료형의 약칭이다.

- 정수(int): 'i'
- 실수(double): 'd'
- 문자(char): 'c'
- 문자열(char \*): 's'

<img src="https://dojang.io/pluginfile.php/642/mod_page/content/26/unit66-3.png" />

만약 "Hello, world!"(문자열), 10(정수), 'a'(문자), 1.234567(실수)가 차례로 들어온다면, 첫 파라미터로 "sicd"를 받아 들어올 자료형들을 알려준다.

<img src="https://dojang.io/pluginfile.php/642/mod_page/content/26/unit66-4.png" />

그래야만 반복문에서 반복할 때마다 ap가 "sicd" 순서대로 char \*, int, char, double 크기만큼 이동해서 값을 가져올 수 있게되는 것이다.
