---
layout: single
title: "[Libft] Part2, bonus part 함수 정리"
categories: "Libft"
tags: [Libft, C]
---

## 3. Part 2: 추가 함수들

### substr() - 스트링의 서브스트링 리턴

	char *ft_substr(char const *s, unsigned int start, size_t len);

- 스트링의 start번째 인덱스부터 최대길이 len까지 서브스트링을 생성한다. (malloc으로 할당)

- 서브스트링을 리턴한다.  
할당 실패시 NULL을 리턴한다.

---

### strjoin() - 스트링 연결

	char *ft_strjoin(char const *s1, char const *s2);

-s1: prefix(접두) 스트링  
s2: suffix(접미) 스트링

- malloc으로 s1과 s2를 합친 새로운 스트링을 할당하고 리턴.

- 합쳐진 새로운 스트링 반환
할당 실패시 NULL리턴

---

### strtrim() - 스트링 처음과 끝 다듬기

	char *ft_strtrim(char const *s1, char const *set);

- s1: 트리밍할 스트링  
s2: 트리밍할 참조 문자 세트

- malloc으로 할당하고, 'set'에 지정된 문자가 문자열의 시작과 끝에서 제거 된 s1의 사본을 반환한다.

	1) s1 = "ABCCBA" set = "AB"
	résultat : "CC"
	2) s1 = "ACCBACBA" set = "AB"
	résultat : "CCBAC"
	3) s1 = "Hello World!" set = "Hlde"
	résultat : "o World!"
​
- 즉, 처음부터 시작해서 맞는 문자가 있다면 다음 문자도 확인하며, 가장 뒷 문자도 동일하게 진행한다.

- 트리밍된 스트링을 리턴한다. (malloc할당 실패시 NULL을 리턴한다)

* 예외처리  
(s1, set)  
	“aaaaa” “a”  
	“” “bc”  
와 같은 경우들도 처리해주어야 한다.

---

### split() - 구분자로 스트링 분리

	char **ft_split(char const *s, char c);

- malloc으로 할당하고 문자 'c'를 구분자로 사용하여 string을 분리하여 얻은 문자열 배열을 반환한다.  
배열은 NULL 포인터로 끝나야한다.

- malloc, free 이용

- 새로운 문자열을 반환한다.  
할당 실패시 NULL을 반환한다.

---

### itoa() - 정수를 스트링으로 변환

	char *ft_itoa(int n);

n: 변환할 정수

malloc으로 할당하고 인수로받은 정수를 나타내는 문자열을 리턴한다.  
음수도 처리해야 한다.

---

### ft_strmapi() - 스트링의 각 문자에 함수적용

	char *ft_strmapi(char const *s, char (*f)(unsigned int, char));

- s: 처리할 스트링  
f: 각 문자에 적용할 함수

- “malloc”이용
- 각 문자에 함수 f를 적용한 스트링 ’s’ 반환
malloc 할당 실패시 NULL 리턴

---

### ft_putchar_fd() - 문자 출력

	void ft_putchar_fd(char c, int fd);
	
- “write”이용

- 문자 c를 주어진 파일 디스크립터에 출력

---

### ft_putstr_fd() - 스트링 출력

	void ft_putstr_fd(char *s, int fd);

- “write”이용
- 스트링 s를 주어진 파일 디스크립터에 출력

---

### ft_putendl_fd() - 스트링 줄바꿈 출력

	void ft_putendl_fd(char *s, int fd);

- “write”이용  
- 스트링 s와 줄바꿈을 주어진 파일 디스크립터에 출력

---

### ft_putnbr_fd() - 정수 출력

	void ft_putnbr_fd(int n, int fd);

- “write”이용  
- 정수 n을 주어진 파일 디스크립터에 출력

> file descriptor :  
	- 시스템으로부터 할당 받은 파일을 대표하는 0이 아닌 정수 값  
	- 프로세스에서 열린 파일의 목록을 관리하는 테이블의 인덱스

## 4. Bonus part: 구조체 함수들

- libft.h 헤더파일에 구조체 추가

<code>

	typedef struct		s_list
	{
		void			*content;
		struct s_list	*next;
	}					t_list;

</code>

- content: void *로 어떤 타입의 데이터든 저장한다.  
next: 다음 element 의 주소 (마지막 element일 경우 NULL)

- make bonus 명령어로 libft.a 라이브러리에 보너스 함수 추가되게

- .c 파일과 헤더에 _bonus 추가할 필요 x. (보너스 함수들 있는 파일들에만 추가)


### ft_lstnew() - 리스트에 element 추가

	t_list *ft_lstnew(void *content);

content: 새로운 element를 만들 content
“Malloc”이용
새로운 element 리턴
변수 ‘content’는 매개변수 ‘content’의 값으로 초기화됨
변수 ‘next’는 NULL로 초기화됨

---

### ft_lstadd_front() - 리스트의 앞에 새 element 추가

	void ft_lstadd_front(t_list **lst, t_list *new);

lst: 구조체의 첫번째 link를 가리키는 포인터의 주소
new: 구조체에 추가될 element를 가리키는 포인터의 주소

---

### ft_lstsize() - list에 있는 element들의 개수 반환

	int ft_lstsize(t_list *lst);

lst: list의 시작부분
list의 길이 반환

---

### ft_lstlast() - list의 마지막 element 반환

	t_list *ft_lstlast(t_list *lst);

lst: 리스트의 시작부분
리스트의 마지막 element 반환

---

### ft_lstadd_back() - 리스트의 마지막에 새 element 추가

	void ft_lstadd_back(t_list **lst, t_list *new);

lst: 리스트의 첫번째 link를 가리키는 포인터의 주소
new: 리스트에 추가될 element를 가리키는 포인터의 주소
element ‘new’를 리스트의 마지막부분에 추가

---

### ft_lstdelone() - 리스트에서 element 하나 삭제

	void ft_lstdelone(t_list *lst, void (*del)(void *));

lst: free할 element
Del: content를 삭제하기 위해 사용된 함수의 주소
“Free”사용

### ft_lstmap() -
