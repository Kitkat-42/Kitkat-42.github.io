---
layout: single
title: "[Get Next Line] 아이디어"
tags: [Get Next Line, C]
---

get_next_line는 파일 디스크립터로부터 read()함수를 이용해 한줄씩 읽어와 출력하는 프로그램이다.  
이를 위해서 어떤 함수들이 필요할지 아이디어를 정리해 보았다.

## 1. get_next_line.c

| 함수 이름	| get_next_line |
|:---|-----|
| 프로토타입 | int	get_next_line(int fd, char **line); |
| 제출할 파일 |get_next_line.c, get_next_line_utils.c, get_next_line.h |
| 파라미터 | #1. 읽어올 파일 디스크립터,  #2. 읽어진 값|
| 외부함수 | read, malloc, free |
| 리턴값 | 1: line이 읽어짐<br> 0: EOF(End-of-File, 파일 끝)에 도달함<br> -1: 에러 발생 |

우선 get_next_line의 프로토타입을 살펴보면, 파라미터로 읽어올 파일 디스크립터(fd)와 line의 이중 포인터를 받는것을 알 수 있다.

	int	get_next_line(int fd, char **line)

프로그램에는 포함되지 않는 main 함수로부터 fd와 line을 넘겨받아 read를 해주고 출력을 해주어야 한다. 먼저 read 함수에 대해 살펴보았다.

### 1.1. read() 함수

| 형태 | ssize_t read(int fd, void *buf, size_t count); |
|:---:|:---|
| 설명 | open() 함수 등으로 열린 파일에서 원하는 데이터를 읽어들인다. fd 에 읽을 데이터가 있다면 buf 에 담아서 가져온다. count 는 buf 에서 한번에 가져올 데이터의 크기를 나타낸다. |
| 헤더 | unistd.h |
| 인수 | int fd - 	파일 디스크립터<br> void *buf - 파일을 읽어 들일 버퍼<br> size_t  count - 버퍼의 크기 |
| 리턴값 | ssize_t	정상적으로 실행되었다면 읽어들인 바이트 수를, 실패했다면 -1을 반환한다. 0이라면 파일의 끝(EOF)을 의미한다.  |

read 함수가 파일 끝이 아닌 상태에서 파일에서 데이터를 가져오는데 성공했다면, 파일 포인터의 위치는 읽은 데이터의 크기만큼 이동하게 된다.
아래는 표준 입력(fd = 1)에서의 사용 예제이다.

	#include <unistd.h>
	#include <stdio.h>
	#include <string.h>

	int main()
	{
		char buf[80]; //버퍼를 80byte만큼 설정

		memset(buf, 0x00, 80); //버퍼 80byte를 모두 0으로 초기화
		if (read(1, buf, 80) < 0) //표준 입력(1)에서 read했을 때 에러가 날 경우
		{
			perror("read erro : ");  //에러 메시지 출력
			exit(0);
		}
		printf("%s", buf); //buf의 스트링 출력
	}
	

출처: https://www.joinc.co.kr/w/man/2/read

> size_t vs ssize_t  
: size_t는 size를 나타내기 위한 type으로 보통의 32bit machine에서는 32bit, 즉 unsigned int로 되어있다. 가장 유명한 sizeof라는 연산자가 반환하는 값을 담기 위한 type으로 보면 되는데 이 역시 크기를 의미하므로 많은 IO 함수에서 사용된다.  
ssize_t는 signed size type으로 보통의 32bit machine에서는 간단히 말해 int다. IO 함수의 반환값으로 많이 사용되는데 그 이유는 해당 IO 함수의 실패를 알려주기 위해서이다.
(read() 함수의 경우 에러 시 -1을 반환한다)

출처: https://lacti.github.io/2011/01/08/different-between-size-t-ssize-t/


### 1.2. 정적 변수(static variable)

먼저 정적 변수가 아닌 자동 변수(지역 변수)로 선언했을 때 실행 결과를 살펴보자.

	#include <stdio.h>

	void increaseNumber()
	{
		int num1 = 0;    // 변수 선언 및 값 초기화

		printf("%d\n", num1);    // 변수 num1의 값을 출력

		num1++;    // 변수의 값을 1씩 증가
	}

	int main()
	{
		increaseNumber();    // 0
		increaseNumber();    // 0
		increaseNumber();    // 0
		increaseNumber();    // 0: 변수가 매번 생성되고 사라지므로 0이 출력됨

		return 0;
	}

지역 변수로 선언을 하면 increaseNumber 함수를 벗어나면 값이 다시 사라진다. increaseNumber를 호출할 때마다 다시 새로운 변수가 생성되기 때문이다.  
static을 붙여 정적 변수로 선언을 하면 이렇게 된다.

	#include <stdio.h>

	void increaseNumber()
	{
		static int num1 = 0;     // 정적 변수 선언 및 값 초기화

		printf("%d\n", num1);    // 정적 변수 num1의 값을 출력

		num1++;    // 정적 변수 num1의 값을 1 증가시킴
	}

	int main()
	{
		increaseNumber();    // 0
		increaseNumber();    // 1
		increaseNumber();    // 2
		increaseNumber();    // 3: 정적 변수가 사라지지 않고 유지되므로 값이 계속 증가함

		return 0;
	}

즉, 정적 변수는 함수를 벗어나더라도 변수가 사라지지 않고 계속 유지되므로 ++ 연산자가 적용되어 값이 계속 증가하게 된다. static int num1 = 0;은 프로그램이 시작될 때 변수를 초기화하며 increaseNumber 함수가 호출될 때는 변수를 초기화하지 않고 무시한다. 정적 변수는 데이터(data) 영역에 저장되어 프로그램 종료시까지 남아있다.

Get Next Line에서 정적 변수가 중요한 이유는 

	int					get_next_line(int fd, char **line)
	{
		static char		*backup[OPEN_MAX];
		char			buf[BUFFER_SIZE + 1];
		int				read_size;
		int				cut_idx;

		if ((fd < 0) || (line == 0) || (BUFFER_SIZE <= 0))
			return (-1);
		while ((read_size = read(fd, buf, BUFFER_SIZE)) > 0)
		{
			buf[read_size] = '\0';
			backup[fd] = ft_strjoin(backup[fd], buf);
			if ((cut_idx = is_newline(backup[fd])) >= 0)
				return (split_line(&backup[fd], line, cut_idx));
		}
		return (return_all(&backup[fd], line, read_size));
	}

static으로 backup을 선언해 read한 버퍼를 백업해 놓아야 하기 때문이다.

1. 버퍼를 read할 임시 버퍼를 만든다.
	char buf[BUFFER_SIZE + 1];

2. read한 버퍼를 백업할 static 버퍼를 만든다.
	static char	*backup[OPEN_MAX];

3. read로 라인을 읽은 다음

4. buf를 static 변수 backup에 백업한다.

5. backup 안에 개행문자가 있는지 없는지 검사한다.

6. 개행문자가 있으면 다음 단계로 넘어가고, 없다면 3번으로 돌아가 파일을 계속 읽으면서
기존에 백업한 것과 계속 합쳐나간다. -> append_backup 함수

7. 개행문자가 있는 backup을 개행문자 전후로 잘라서, \n 전까지는 line에다가 주고, \n 후는
다시 static 변수 backup에 백업한다. -> split_line 함수


## 2. get_next_line_utils.c

>
필요한 함수들은 무엇이 있을까?  
그전에 만들어둔 libft는 사용하지 못하므로 적절한 함수들을 get_next_line_utils.c 에 넣어주어야 한다.

ft_strjoin :
ft_memcpy :



