---
layout: single
title: "[Libft] 프로젝트 소개, Part 1"
categories: "Libft"
tags: [Libft, C]
---

Libft는 나만의 함수 라이브러리를 만드는 프로젝트이다. 기존의 <code>string.h, stdlib.h, ctype.h</code> 등의 헤더파일에 선언되어 있는 유용한 C 함수들을 직접 만들고, 이후의 프로젝트에서도 사용하게 될 것이다.

## 1. 프로젝트 소개

- 전역변수는 사용이 금지된다.

- sub functions들은 static으로 정의하여 사용하는 것을 권장한다.  
나중에 있을 협업 프로젝트에서 개발자들 간에 함수 이름이 겹쳐서 발생하는 문제를 예방할 수 있기에 미리 익숙해지면 좋다.

- ar 명령어로 library를 생성한다. (libtool 명령어는 금지된다.)

## 2. Part 1: Libc 함수들 만들기

- C 표준 라이브러리인 Libc 함수들을 구현하여 작동원리와 사용법을 익힌다.

- 함수이름은 “ft_”로 생성한다 (예: strlen —> ft_strlen)￼

- Libc의 함수들은 터미널에 man 명령어를 입력하여 매뉴얼 페이지에 정의되어 있는 프로토타입들을 사용했다.

예)  

	man memset

### memset() - 바이트를 값으로 설정

	void *memset(void *b, int c, size_t len);

어떤 메모리의 시작점부터 연속된 범위를 어떤 값으로 (바이트 단위로) 모두 지정하고 싶을 때 사용하는 함수이다.

- b: 채우고자 하는 메모리의 시작 포인터(시작 주소)
- c : 채우고자 하는 값.  
int형으로 전달되지만 내부에서는 unsigned char(1byte)로 변환되어 저장된다.  

메모리에 접근할 때는 항상 unsigned char형을 쓰는데, 다른 type들은 내부 비트의 일부를 부호 비트로 사용하는 반면 unsigned char는 모든 비트를 값으로 사용하기 때문이다.

- len : 채우고자 하는 byte의 수 (채우고자 하는 메모리의 크기)  

바이트 단위로 지정되기 때문에, 예를 들어 int형 배열을 1로 모두 초기화 한다고 하면 배열이 모두 '16843009'로 초기화된다고 한다. 
16843009의 2진수 표현은 0001 00000001 00000001 00000001 이다. 즉, arr 배열은 1바이트(8비트)당 1로 초기화 된 것이다.

>size_t 자료형  
: unsigned int와 비슷하나, 운영체제에 따라 크기가 다르다. 32비트에서 4바이트, 64비트에서 8바이트로 정의되어 있다. C 언어 표준 함수에서도 크기를 의미하는 매개변수나 반환값은 size_t를 사용하고 있고, CPU 비트 수에 맞게 자료형을 제공하고 있으므로 크기를 의미하는 변수는 unsigned int 대신 size_t로 선언하는 것이 좋다. '<'stddef.h'>' 헤더파일에 정의되어 있다. libft.h에 선언하여 사용하도록 한다. 

---

### bzero() - 메모리 0으로 초기화

	void bzero(void *s, size_t n);

메모리 공간을 s 주소값부터 n바이트만큼 0으로 채운다.  
똑같이 메모리 초기화 목적인 memset함수의 하위호환이라고 한다.

---
### memcpy() - 바이트 복사

	void *memcpy(void *restrict dst, const void *restrict src, size_t n);

- src의 n 바이트를 dst로 복사하고, dst에 대한 포인터를 리턴한다.


- dst와 src가 null 포인터일 경우 예외처리를 해주어야 한다.

- 이 함수는 source 의 널 종료 문자('\0') 을 검사하지 않고 언제나 정확히 n 바이트 만큼을 복사한다.
- 만일 두 메모리 블록이 겹쳐져 있다면 memmove 함수를 이용해야 한다. src의 원본 값이 이전 src로 바뀐 상태에서 복사를 해버리기 때문이다.
> restrict 포인터  
: 메모리 접근에 관련된 최적화 기능(C99표준)으로,  각 포인터가 서로 다른 메모리 공간을 가리키고 있고, 다른 곳에서 접근하지 않으니 컴파일러가 최적화를 하라는 뜻이다. 같은 메모리 공간을 가리키는 포인터에 restrict 붙이면 잘못된 결과 나올 수 있으니 주의해야 한다.
  

---

### memccpy() - 특정 문자까지 바이트 복사

	void *memccpy(void *restrict dst, const void *restrict src, int c, size_t n);

unsigned char로 변환된 문자 c를 찾을 때까지 src string을 dst로 복사한다. src에서 c가 copy되면 copy 중단한다. 혹은, n바이트가 copy되면 중단한다. 둘 중 먼저 오는 것으로 중단된다고 보면 된다.

성공적일 경우 dst에서 문자 c 복사된 다음 번지의 포인터를 반환한다.
만약 src의 n바이트 까지 문자 c가 없을 경우 NULL 포인터를 반환한다.

---

### memmove() - 메모리 이동

	void *memmove(void *dst, const void *src, size_t len);

src가 가리키는 곳부터 len바이트 만큼을 dst가 가리키는 곳으로 복사한다. 두 string은 오버랩될 수 있으며, Non-destructive 한 방법으로 항상 복사된다.

* src의 주소가 dest보다 큰 값인 경우, 오버랩되더라도 src 모두 정상적으로 dest에 복사된다.

* src의 주소가 dest 보다 작은 값이면, 마지막 데이터부터 한바이트씩 dst의 마지막 바이트부터 복사한다.

src의 널 종료 문자를 확인하지 않고 정확히 len 바이트만큼 복사를 수행한다. 오버플로우를 방지하기 위해 dest와 src가 가리키는 배열 모두 적어도 len 바이트 이상 되어야 한다.  

dst의 포인터를 반환한다.

---
### memchr() - 메모리에서 문자 찾기

	void *memchr(const void *s, int c, size_t n);

unsigned char로 변환된 c의 처음 항목에 대해 버퍼(s)의 첫 번째 n바이트를 검색한다.
c를 찾거나 n 바이트를 검사할 때까지 검색을 계속한다.

버퍼(s)에서 c의 위치에 대한 포인터를 반환한다.  

c가 버퍼의 첫번째 n바이트 내에 있지 않은 경우 NULL을 반환한다.

---

### memcmp() - 메모리의 데이터 비교

	int	memcmp(const void *s1, const void *s2, size_t n);

- 두 string이 동일하면 0을 반환, 다를 경우 첫번째 다른 값의 차이를 반환한다. (unsigned char로 비교한다)
- 두 string 모두 길이가 0이면 항상 동일하므로 0을 반환한다.
- n은 버퍼 사이즈보다 작거나 같아야 한다. 그렇지 않으면 쓰레기 값끼리 비교한 값을 리턴한다. strncmp는 s1과 s2 모두 널 종료문자가 나오면 비교를 중단하고 0을 리턴했던 것과 차이가 있다.

---

### strlen() - 스트링의 길이 반환

	size_t strlen(const char *s);
	
string의 길이를 리턴하는 함수이다.

---

### strlcpy() - 스트링 복사

	size_t strlcpy(char * restrict dst, const char * restrict src, size_t dstsize);
	
src를 dst로 1 character씩 dstsize 까지 복사한다. dstsize가 0이 아니면 NULL로 끝나게끔 dstsize에 NULL값 공간이 포함되어있어야 한다.
src와 dst가 오버랩되는 경우 작동하지 않는다는 점에서 memmove와 차이가 있다.

---

### strlcat() - 크기가 제한된 스트링 연결

	size_t	strlcat(char * restrict dst, const char * restrict src, size_t dstsize);

- 복사될 문자열의 길이가 (size - dst길이 - 1)이고, 끝에 널 종료 문자 ‘\0'을 삽입한다.

- 결합되는 문자열의 총 길이를 반환한다.

- size가 dst길이보다 작게 주어질 경우 (src길이 + size)를 리턴한다.  
 size가 dst길이보다 크게 주어질 경우 (src길이 + dst길이)를 리턴한다.

---

### strchr() - 스트링에서 특정 문자 찾기

	char *strchr(const char *s, int c);

- s가 가리키는 string에서 첫번째로 나오는 문자 c를 가리키는 포인터를 리턴한다.
- int c로 전달받지만 함수 내부에서 char로 변환된다.
- 마지막 NULL문자도 string의 일부로 생각되어 만약 c = ‘\0’이면 ‘\0’를 가리키는 포인터 반환한다. 이 때문에 이 함수는 문자열의 끝 부분을 가리키는 포인터를 얻기 위해 사용할 수도 있다.
- 문자 c를 가리키는 포인터를 반환하고, 문자 c가 string에 나타나지 않을 경우 NULL을 반환한다.

---

### strrchr() - 스트링의 뒤에서부터 특정 문자 찾기

	char *strrchr(const char *s, int c)

strchr와 같지만, 가장 마지막에 나타난 c를 가리키는 포인터 반환한다.

---

### strnstr() - 서브스트링 찾기

	char *strnstr(const char *haystack, const char *needle, size_t len);

- haystack 스트링 안에서 (NULL로 끝나는) needle 스트링을 검색한다. (Len 바이트까지)  
‘\0’ 이후에 오는 문자는 검색되지 않는다.

- haystack에서 needle의 첫번째 시작 위치에 대한 포인터를 리턴한다.  
needle이 haystack에서 나타나지 않으면 NULL을 리턴한다.  
needle이 길이가 0인 스트링을 가리키면 haystack을 리턴한다.

---

### strncmp() - 스트링 비교

	int	strncmp(const char *s1, const char *s2, size_t n);

- strcmp와 달리 n개 문자까지만 비교한다. ‘\0’ 다음에 나타나는 캐릭터들은 비교되지 않는다.

- 같으면 0을 반환하고, 다르면 차이 반환

- memcmp와 달리 중간에 NULL이오면 비교를 종료한다. (memcmp는 정확히 n바이트까지의 모든 메모리를 비교한다)

---

### atoi() - 문자 스트링을 정수로 변환

	int	atoi(const char *str);

- 선행 공백 문자(whitespace)를 무시한다.

- 입력 문자를 숫자로 해석하여 생성되는 int값을 리턴한다.
(변환할 수 없는 경우 0을 리턴한다)
- 리턴값은 오버플로우의 경우 정의되지 않는다.

---

### isalpha() - 알파벳 테스트

	int	isalpha(int c);

c가 알파벳(‘a’~’z’, ‘A’~’Z’)인지 확인한다.

————

### isdigit() - 십진법숫자 테스트

	int	isdigit(int c);

unsigned char가 십진법 수 (‘0’~’9’)가 아니면 0을 리턴한다.  
십진법 수이면 non-zero 리턴을 한다.

---

### isalnum() - 알파벳/십진법숫자 테스트

     int	isalnum(int c);

isalpha 함수와 isdigit 함수를 합친 것이라고 보면 된다.
 (‘0’~’9’, ’a’~’z’, ‘A’~’Z’)

---

### isascii() - ASCII 문자 테스트

	int	isascii(int c);

0 부터 octal 0177까지 모두 포함된다.

---

### isprint() - 출력 가능한 문자 테스트

	int	isprint(int c);

ASCII 코드 (8진법) 040~0176에 해당한다.

---

### toupper() - 대문자 변환

	int	toupper(int c);

알파벳 소문자를 대문자로 변환한다.

---

### tolower() - 소문자 변환

	int	tolower(int c);

알파벳 대문자를 소문자로 변환한다.

---

### calloc() -  기억장치 예약 및 초기화

	void	*calloc(size_t count, size_t size);

- (num * size) 바이트의 메모리를 힙에서 할당하여 반환한다.

- (각각 길이가 size바이트인) count개 요소 배열에 대한 기억장치 공간을 예약한다.  그런 다음 각 요소의 모든 비트에 초기 값을 0으로 채워준다.

- 예약 공간을 가리키는 포인터를 리턴한다. malloc과 같이, 메모리 할당에 실패하면 NULL을 리턴한다.

---

### strdup() - 스트링 복제

	char	*strdup(const char *s1);

- malloc을 호출하여 string의 사본에 대한 기억장치 공간을 예약한다.
(string이 끝에 널문자를 포함한다고 예상한다)

- 나중에 예약된 기억장치를 해제해야 한다.

- 복사된 스트링을 포함하는 기억장치 공간에 대한 포인터를 리턴한다.
메모리 할당에 실패하면 NULL을 리턴한다.
<br>















