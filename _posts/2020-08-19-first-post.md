---
layout: single
title: "[Libft] 프로젝트 소개, Part 1"
categories: "Libft"
tags: [Libft, C]
---

Libft는 나만의 함수 라이브러리를 만드는 프로젝트이다. 기존의 <code>'<string.h>', '<stdlib.h>', '<ctype.h>'</code> 등의 헤더파일에 선언되어 있는 유용한 C 함수들을 직접 만들고, 이후의 프로젝트에서도 사용하게 될 것이다.

## 1. 프로젝트 소개

- 전역변수는 사용이 금지된다.

- sub functions들은 static으로 정의하여 사용하는 것을 권장한다.  
나중에 있을 협업 프로젝트에서 개발자들 간에 함수 이름이 겹쳐서 발생하는 문제를 예방할 수 있기에 미리 익숙해지면 좋다.

- ar 명령어로 library를 생성한다. (libtool 명령어는 금지된다.)

## 2. Part 1: Libc 함수들 만들기

- C 표준 라이브러리인 Libc 함수들을 구현하여 작동원리와 사용법을 익힌다.

- 함수이름은 “ft_”로 생성한다 (예: strlen —> ft_strlen)￼

- Libc의 함수들은 터미널에 man 명령어를 입력하여 매뉴얼에 정의되어 있는 프로토타입들을 사용했다.

예)  

	man memset

### memset() - 바이트를 값으로 설정

	void *memset(void *b, int c, size_t len);

'''C
	#include "libft.h"

	void	*ft_memset(void *str, int c, size_t len)
	{
		size_t			i;
		unsigned char	*pstr;

		i = 0;
		pstr = (unsigned char *)str;
		while (i < len)
		{
			pstr[i] = c;
			i++;
		}
		return (str);
	}
'''

어떤 메모리의 시작점부터 연속된 범위를 어떤 값으로 (바이트 단위로) 모두 지정하고 싶을 때 사용하는 함수이다.

- b: 채우고자 하는 메모리의 시작 포인터(시작 주소)
- c : 채우고자 하는 값.  
int형으로 전달되지만 내부에서는 unsigned char(1byte)로 변환되어 저장된다.  

	_* 메모리에 접근할 때는 항상 unsigned char형을 쓰는데, 다른 type들은 내부 비트의 일부를 부호 비트로 사용하는 반면 unsigned char는 모든 비트를 값으로 사용하기 때문이다_

- len : 채우고자 하는 byte의 수 (채우고자 하는 메모리의 크기)  

바이트 단위로 지정되기 때문에, 예를 들어 int형 배열을 1로 모두 초기화 한다고 하면 배열이 모두 '16843009'로 초기화된다고 한다. 
16843009의 2진수 표현은 0001 00000001 00000001 00000001 이다. 즉, arr 배열은 1바이트(8비트)당 1로 초기화 된 것이다.

---

### bzero() - 메모리 0으로 초기화

	void bzero(void *s, size_t n);

메모리 공간을 s 주소값부터 n바이트만큼 0으로 채운다.  
똑같이 메모리 초기화 목적인 memset함수의 하위호환이라고 한다.

---
### memcpy() - 바이트 복사

	void *memcpy(void *restrict dst, const void *restrict src, size_t n);

src의 n 바이트를 dst로 복사하고, dst에 대한 포인터를 리턴한다.

_* restrict 포인터_ : 
메모리 접근에 관련된 최적화 기능(C99표준)으로,  
각 포인터가 서로 다른 메모리 공간을 가리키고 있고, 다른 곳에서 접근하지 않으니 컴파일러가 최적화를 하라는 뜻이다.  
(같은 메모리 공간을 가리키는 포인터에 restrict 붙이면 잘못된 결과 나올 수 있으니 주의해야 한다)  
dst와 src가 null 포인터일 경우 예외처리

---

### memccpy() - 

	void *memccpy(void *restrict dst, const void *restrict src, int c, size_t n);

문자 c (unsigned char로 변환된) 찾을 때까지 src string을 dst로 copy
src에서 c가 copy되면 copy 중단 / n바이트가 copy되면 중단
(둘 중 먼저 오는 것으로 중단)

성공적일 경우 : dst에서 c카피된 다음의 포인터 반환
Src의 n바이트 까지 c가 없을 경우 NULL 포인터 반환

---

memmove() -

	void *memmove(void *dst, const void *src, size_t len);

src가 가리키는 곳부터 len바이트 만큼을 dst가 가리키는 곳으로 copy.

두 string은 오버랩될 수 있음. Non-destructive 한 방법으로 항상 copy됨

= src의 주소가 dest보다 큰 값인 경우, 오버랩되더라도 src 모두 정상적으로 dest에 복사. (버퍼 이용하기 때문)

= src의 주소가 dest 보다 작은 값이면, 마지막 데이터부터 한바이트씩 dst의 마지막 바이트부터 복사.

src의 널 종료 문자 확인 안함. 정확히 len 바이트만큼 복사 수행.
오버플로우 방지 위해 dest와 src가 가리키는 배열 모두 적어도 len 바이트 이상 되어야.

dst의 포인터 리턴

---
memchr() -

	void *memchr(const void *s, int c, size_t n);

부호없는 문자로 변환된 c의 처음 항목에 대해 버퍼(s)의 첫 번째 n바이트를 검색
c를 찾거나 n 바이트를 검사할 때까지 검색 계속함.

버퍼(s)에서 c의 위치에 대한 포인터 리턴
c가 버퍼의 첫번째 n바이트 내에 있지 않은 경우 NULL 리턴

---

memcmp() -

---

strlen() -

---

strlcpy() -

---
strlcat() -

	size_t	strlcat(char * restrict dst, const char * restrict src, size_t dstsize);
￼

---

strchr() -

	char *strchr(const char *s, int c);

s가 가리키는 string에서 첫번째로 나오는 c(char로 변환된) 를 가리킴
(마지막 null문자도 string의 일부로 생각되어 만약 c = ‘\0’이면 ‘\0’를 가리키는 포인터 반환)

c를 가리키는 포인터 반환
c가 string에 나타나지 않을 경우 NULL 반환

---

strrchr() -

	char *strrchr(const char *s, int c)

strchr와 같지만, 가장 마지막에 나타난 c를 가리키는 포인터 반환

---

strnstr() - 서브스트링 찾기

	char *strnstr(const char *haystack, const char *needle, size_t len);

haystack 스트링 안에서 (null로 끝나는) needle 스트링을 검색 (Len 바이트까지)
‘\0’ 이후에 오는 캐릭터는 검색되지 않음.

haystack에서 needle의 첫번째 시작 위치에 대한 포인터를 리턴
needle이 haystack에서 나타나지 않으면 NULL리턴
Needle이 길이가 0인 스트링을 가리키면 haystack 리턴

*Since the strnstr() function is a FreeBSD specific API, it should only be used when portability is not a concern.

---

strncmp() - 스트링 비교

	int	strncmp(const char *s1, const char *s2, size_t n);

strcmp와 달리 n개 캐릭터까지만 비교. ‘\0’ 다음에 나타나는 캐릭터들은 비교되지 않음.

같으면 0 반환
다르면 차이 반환

memcmp와 달리 중간에 NULL이오면 비교 종료 (memcmp는 정확히 n바이트까지 모두 메모리 비교)

---

atoi() - 문자 스트링을 정수로 변환
<stdlib.h>

	int	atoi(const char *str);

선행 공백 문자(whitespace) 무시

입력 문자를 숫자로 해석하여 생성되는 int값 리턴
(변환할 수 없는 경우 0 리턴)
리턴값은 오버플로우의 경우 정의되지 않음

---

isalpha() - 알파벳인지 테스트 (‘a’~’z’, ‘A’~’Z’)

	#include <ctype.h>

		int	isalpha(int c);

————

isdigit() - 십진법 숫자 char인지 테스트 (‘0’~’9’)

	int	isdigit(int c);

unsigned char가 십진법 수가 아니면 0 리턴
십진법 수이면 non-zero 리턴

---

isalnum() - isalpha + isdigit (‘0’~’9’, ’a’~’z’, ‘A’~’Z’)

     int	isalnum(int c);

---

isascii() - ascii 캐릭터인지 테스트

	int	isascii(int c);

0 부터 octal 0177까지 모두 포함

---

isprint() - 출력 가능한 문자인지 테스트

	int	isprint(int c);

ASCII 코드 (8진법) 040~0176

---

toupper() - 알파벳 소문자 -> 대문자 변환

	int	toupper(int c);

---

tolower() - 알파벳 대문자 -> 소문자 변환

	int	tolower(int c);

---

calloc() -  기억장치 예약 및 초기화

(num * size) 바이트의 메모리를 힙에서 할당하여 반환한다.

	#include <stdlib.h>

		void	*calloc(size_t count, size_t size);

(각각 길이가 size바이트인) count개 요소 배열에 대한 기억장치 공간을 예약
그런 다음 각 요소의 모든 비트에 초기 값을 0으로 채워줌.

예약 공간을 가리키는 포인터를 리턴
malloc과 같이, 메모리 할당에 실패하면 NULL리턴

---

strdup() - 스트링 복제

    #include <string.h>
    char	*strdup(const char *s1);

malloc을 호출하여 string의 사본에 대한 기억장치 공간을 예약
(string은 끝에 널문자를 포함한다고 예상함)
나중에 예약된 기억장치 해제해야 함.

복사된 스트링을 포함하는 기억장치 공간에 대한 포인터 리턴
메모리 할당에 실패하면 NULL리턴
<br>
<br>















