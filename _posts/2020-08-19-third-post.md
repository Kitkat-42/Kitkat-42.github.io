---
layout: single
title: "[Libft] Part2, bonus 파트"
categories: "Libft"
tags: [Libft]
---

<img src= "../assets/images/teaser.jpg" width="400">

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

* 예외처리
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
