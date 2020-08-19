---
layout: single
title: Libft
---

## 1. Introduction
- 전역변수 금지

- sub functions : static으로 define

- ar 명령어로 library 생성 (libtool 명령어 금지)

## 2. Part 1 - Libc 함수들 생성

- 함수이름 “ft_”로 생성 (ex. Strlen —> ft_strlen)￼

memset() - 바이트를 값으로 설정

	void *memset(void *b, int c, size_t len);

어떤 메모리의 시작점부터 연속된 범위를 어떤 값으로(바이트 단위) 모두 지정하고 싶을 때 사용하는 함수 <string.h>

Len : 채우고자 하는 byte의 수 (채우고자 하는 메모리의 크기)
c : 채우고자 하는 값 ((int 형이지만 내부에서는 unsigned char(1byte)로 변환되어 저장됨)
B: 채우고자 하는 메모리의 시작 포인터(시작 주소)

---

bzero() -

	void bzero(void *s, size_t n);

메모리 공간을 n바이트만큼 0으로 채운다 (s 주소값부터) - 메모리 초기화 목적 memset 하위호환 <string.h>

---
memcpy() - 바이트 복사

	void *memcpy(void *restrict dst, const void *restrict src, size_t n);

src의 n 바이트를 dst로 복사, dst에 대한 포인터를 리턴함

_* restrict 포인터_ : 메모리 접근에 관련된 최적화 기능(C99표준) - 각 포인터가 서로 다른 메모리 공간을 가리키고 있고, 다른 곳에서 접근하지 않으니 컴파일러가 최적화를 하라는 뜻
( 같은 메모리 공간을 가리키는 포인터에 restrict 붙이면 잘못된 결과 나올 수 있으니 주의 )

￼
dst와 src가 null 포인터일 경우 예외처리

---

memccpy() -

	Void *memccpy(void *restrict dst, const void *restrict src, int c, size_t n);

문자 c (unsigned char로 변환된) 찾을 때까지 src string을 dst로 copy
src에서 c가 copy되면 copy 중단 / n바이트가 copy되면 중단 (둘 중 먼저 오는 것으로 중단)

성공적일 경우 : dst에서 c카피된 다음의 포인터 반환
Src의 n바이트 까지 c가 없을 경우 NULL 포인터 반환

---

memmove() -

	Void *memmove(void *dst, const void *src, size_t len);

src가 가리키는 곳부터 len바이트 만큼을 dst가 가리키는 곳으로 copy. 두 string은 오버랩될 수 있음. Non-destructive 한 방법으로 항상 copy됨
= src의 주소가 dest보다 큰 값인 경우, 오버랩되더라도 src 모두 정상적으로 dest에 복사. (버퍼 이용하기 때문)
= src의 주소가 dest 보다 작은 값이면, 마지막 데이터부터 한바이트씩 dst의 마지막 바이트부터 복사.

src의 널 종료 문자 확인 안함. 정확히 len 바이트만큼 복사 수행.
오버플로우 방지 위해 dest와 src가 가리키는 배열 모두 적어도 len 바이트 이상 되어야.

dst의 포인터 리턴
---
memchr() -

	Void *memchr(const void *s, int c, size_t n) :

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
strlcat

---

strchr() -

	Char *strchr(const char *s, int c);

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

	Char *strnstr(const char *haystack, const char *needle, size_t Len);

Haystack 스트링 안에서 (null로 끝나는) needle 스트링을 검색 (Len 바이트까지)
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

Unsigned char가 십진법 수가 아니면 0 리턴
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

## 3. Part 2 - 추가 함수들

ft_ substr / ft_strjoin / ft_strtrim / ft_split / ft_itoa / ft_strmapi / ft_putchar_fd / ft_putstr_fd / ft_putendl_fd

substr() - 스트링의 서브스트링 리턴

	char *ft_substr(char const *s, unsigned int start, size_t len);

스트링의 Start번째 인덱스부터 최대길이 len까지 서브스트링 생성.   (Malloc으로 할당)

서브스트링 리턴
할당 실패시 NULL리턴

---

strjoin() -

	char *ft_strjoin(char const *s1, char const *s2);

s1: prefix(접두) 스트링
s2: suffix(접미) 스트링

malloc으로 s1과 s2를 합친 새로운 스트링을 할당하고 리턴.

합쳐진 새로운 스트링 반환
할당 실패시 NULL리턴

---

strtrim() -

	char *ft_strtrim(char const *s1, char const *set);

s1: 트리밍할 스트링
s2: 트리밍할 참조 문자 세트

malloc으로 할당하고 'set'에 지정된 문자가 문자열의 시작과 끝에서 제거 된 s1의 사본을 반환.

1) s1 = "ABCCBA" set = "AB"
résultat : "CC"
2) s1 = "ACCBACBA" set = "AB"
résultat : "CCBAC"
3) s1 = "Hello World!" set = "Hlde"
résultat : "o World!"
​
즉, 처음부터 시작해서 맞는 문자가 있다면 다음 문자도 확인하며, 가장 뒷 문자도 동일하게 진행.

트리밍된 스트링 리턴 (malloc할당 실패시 NULL리턴)

<예외처리>
“aaaaa” “a”
“” “bc”

---

split() -

	char **ft_split(char const *s, char c);

malloc으로 할당하고 문자 'c'를 구분자로 사용하여 string을 분리하여 얻은 문자열 배열을 반환.
배열은 NULL 포인터로 끝나야함.

Malloc, free 이용

새로운 문자열 반환
할당 실패시 NULL반환

? Free 언제 해줘야 하는지?

---

itoa() - 정수를 스트링으로 변환

	char *ft_itoa(int n);

n: 변환할 정수

malloc으로 할당하고 인수로받은 정수를 나타내는 문자열 리턴.  음수도 처리해야함.

---

ft_strmapi() - 스트링의 각 문자에 함수적용

	char *ft_strmapi(char const *s, char (*f)(unsigned int, char));

s: 처리할 스트링
f: 각 문자에 적용할 함수
“Malloc”이용
각 문자에 함수 f를 적용한 스트링 ’s’ 반환
Malloc 할당 실패시 NULL 리턴

---

ft_putchar_fd()

	void ft_putchar_fd(char c, int fd);“write”이용

문자 c를 주어진 파일 디스크립터에 출력

---

ft_putstr_fd()

	void ft_putstr_fd(char *s, int fd);

“write”이용
스트링 s를 주어진 파일 디스크립터에 출력

---

ft_putendl_fd()

	void ft_putendl_fd(char *s, int fd);

“write”이용
스트링 s와 줄바꿈을 주어진 파일 디스크립터에 출력

---

ft_putnbr_fd()

	void ft_putnbr_fd(int n, int fd);

“write”이용
정수 n을 주어진 파일 디스크립터에 출력

_* file descriptor_
	-시스템으로부터 할당 받은 파일을 대표하는 0이 아닌 정수 값
	- 프로세스에서 열린 파일의 목록을 관리하는 테이블의 인덱스
<br>

## 4. Bonus part


- libft.h 헤더파일에 구조체 추가

content: void *로 어떤 타입의 데이터든 저장

next: 다음 element 의 주소 (마지막 element일 경우 NULL)
- make bonus 명령어로 libft.a 라이브러리에 보너스 함수 추가되게
- .c 파일과 헤더에 _bonus 추가할 필요 x. (보너스 함수들 있는 파일들에만 추가)

ft_lstnew()

	t_list *ft_lstnew(void *content);

content: 새로운 element를 만들 content
“Malloc”이용
새로운 element 리턴
변수 ‘content’는 매개변수 ‘content’의 값으로 초기화됨
변수 ‘next’는 NULL로 초기화됨

---

ft_lstadd_front() - 구조체의 시작부분에 새로운 element ‘new’추가

	void ft_lstadd_front(t_list **lst, t_list *new);

lst: 구조체의 첫번째 link를 가리키는 포인터의 주소
new: 구조체에 추가될 element를 가리키는 포인터의 주소

---

ft_lstsize() - list에 있는 element들의 개수를 셈

	int ft_lstsize(t_list *lst);

lst: list의 시작부분
list의 길이 반환

---

ft_lstlast() - list의 마지막 element 반환

	t_list *ft_lstlast(t_list *lst);

lst: 리스트의 시작부분
리스트의 마지막 element 반환

---

ft_lstadd_back() - element ‘new’를 리스트의 마지막에 추가

	void ft_lstadd_back(t_list **lst, t_list *new);

lst: 리스트의 첫번째 link를 가리키는 포인터의 주소
new: 리스트에 추가될 element를 가리키는 포인터의 주소
element ‘new’를 리스트의 마지막부분에 추가

---

ft_lstdelone()

	void ft_lstdelone(t_list *lst, void (*del)(void *));

lst: free할 element
Del: content를 삭제하기 위해 사용된 함수의 주소
“Free”사용















