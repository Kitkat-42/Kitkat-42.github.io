---
layout: single
title: "[Get Next Line] 아이디어"
tags: "get_next_line", "C"
summary: Summary of the article
---

get_next_line는 파일 디스크립터로부터 read()함수를 이용해 한줄씩 읽어와 출력하는 프로그램이다.  
이를 위해서 어떤 함수들이 필요할지 아이디어를 정리해 보았다.

## 1. get_next_line.c

| 함수 이름	| get_next_line |
|:---|-----|
| 프로토타입 | int	get_next_line(int fd, char **line); |
| 제출할 파일 |get_next_line.c, get_next_line_utils.c, get_next_line.h |
| 파라미터 | #1. 읽어올 파일 디스크립터,  #2. 읽어진 값|
| 리턴값 | 1: line이 읽어짐<br> 0: EOF(End-of-File, 파일 끝)에 도달함<br> -1: 에러 발생 |

우선 get_next_line의 프로토타입,

	int	 get_next_line(int fd, char **line);

을 살펴보면, 파라미터로 읽어올 파일 디스크립터(fd)와 line의 이중 포인터를 받는것을 알 수 있다.


## 2. get_next_line_utils.c

>
필요한 함수들은 무엇이 있을까?  
그전에 만들어둔 libft는 사용하지 못하므로 적절한 함수들을 get_next_line_utils.c 에 넣어주어야 한다.

