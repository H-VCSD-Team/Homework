# SWIP 02.20

## L-value, R-value

```c
int a, b;
a = 10;
b = a;
```

a=10 에서 a는 저장공간 → a의 값이 10으로 바뀜(L-value)

b=a에서 a는 값 → 저장 공간의 값을 복사하여 사용하므로 a 안바뀜→R-value(연산하거나 출력할때도 값을 복사하므로 변수의 값이 변하지 않는다)

## 자동 캐스팅

```c
int a = 2;
	~~~~double b = 3.0;

	printf("%lf", a + b);
```

a는 int, b는 double로 서로 다른 형태의 변수가 연산될 경우에, 데이터 보존을 위해 더 넓은 형태의 변수로 캐스팅 된다
따라서 a+b는 double형이 되게 됨

```c
//c언어 특: 이식성이 뛰어나다

// #전처리기 구문(컴파일 전단계에서 작동하는 코드)
// 컴파일: 영문 코드를 기계어로 번환해주는 과정
// 위에서부터 순서대로 실행함
// 컴파일러: 컴파일을 진행해주는 유틸리티

#include <stdio.h>	
//주석처리	컴파일 전 단계에서 stdio.h를 들고와서 포함하는 전처리문
//stdio.h : standard input ouput library 함수 원형 모음집
//함수 동작 하기 위해 필요한것 : 함수원형, 함수호출, 함수정의
//반환값 void 일때 return; 사용 가능

//main 함수는 프로젝트에서 1개만 있어야함
int main( void ) {

	//main함수의 body	
	printf("현대 엔지비 C프로그래밍!! \n");		//표준 출력 라이브러리 함수 호출
	return 0;		//0을 반환해주는 이유는 OS의 로그관리용(함수의 종료/ 값의 반환)
}
```

```c
#include <stdio.h>

int main() {
	
	//정수 데이터 1개를 메모리에 저장
	
	//int : 자료형은 자료를 저장하기 위해 memory에 필요한 공간을 알려준다
	//data(변수명) : 변수명은 사용될 memory 공간의 이름 tag
	//변수 선언
	int data;		// 변수 선언 ==> main 함수에 존재하는 지역변수지만,
				//main함수는 계속 돌고있기 때문에 전역변수처럼 행동하지만(stack에 올라감)
				// 접근 범위는 전역변수와 다르다

	//변수 사용
	data = 0;
				// 선언과 사용의 차이점 : 
				//항상 stack에 저장되는 지역변수는 초기화해야함
	//int data = 0;	//변수 선언과 동시에 초기화

#if(0)		//※주의※
			//코드 필요 없을때 주석처리 하지 말고 전처리 하여 사용하기
	printf("데이터의 사이즈 : %d byte\n", sizeof(data));			//data변수의 메모리 크기 체크
	printf("int형의 사이즈 : %d byte\n", sizeof(int));				//int 자료형 메모리 크기 체크
	printf("char데이터의 사이즈 : %d byte\n", sizeof(char));		//char변수의 메모리 크기 체크
	printf("float형의 사이즈 : %d byte\n", sizeof(float));			//float 자료형 메모리 크기 체크
	printf("double형의 사이즈 : %d byte\n", sizeof(double));		//double 자료형 메모리 크기 체크
#else
	printf("이거 컴파일중\n");

#endif
	
	printf("data : %d\n", data+5);
	
	char ch = 'A';	// -128 ~ +127
	printf("ch : %c \n", ch);

	double db=5.34;	//선언과 동시에 초기화
	printf("db : %.2lf \n", db);

	return 0;
}
```

```c
#include <stdio.h>

int main() {

	printf("C 프로그래밍\n\t");		//"" : 문자열 표현
	printf("study\n");			// '': 한 문자 표현
	// 제어문자(특수문자) : / '\n' '\r', '\t'
	// 밑 개행문자 어느정도는 아스키코드 몇인지 알아두기!
	// \n: 개행문자(줄바꿈) ==> 아스키코드에서 16진수로 0x0A==> 십진수로 10
	// '\r' ==> 캐리지리턴(출력하는 커서를 그 줄에서 맨 앞으로 이동시킴) ==> 0x0D(16진수), 13(10진수)
	// visual studio에서는 \n입력하면 뒤에 \r이 자동으로 붙어 터미널에서 볼때 자동으로 커서가 그 줄의 맨 앞으로 간다
	// '\t' : tab
	// 아스키코드표 ==> 0 ~ 127 까지( 128문자 )
	// 아스키코드표 ==> 0 ==> null 문자 ==> '\0'
	// '0' ==> 
	// '%x' 서식문자(~ 포멧으로 출력해라)==> 16진수로 출력하는거임
	printf("16진수로 값 출력 : 0x%x \n",'0');		//0x30으로 출력됨
	printf("10진수로 값 출력 : %d \n", '0');		//48으로 출력됨
	printf("한 문자로 값 출력 : %c \n", '0');		//0 으로 출력됨
	printf("stirng 으로 값 출력 : %s \n", "0");		//0 으로 출력됨==> string일 때는 큰따옴표만 사용해야함
	printf("문자 주소 출력 : %p \n", '0');		//0 으로 출력됨
	printf("double(long float)형 실수 : %lf \n", '0');		//0 으로 출력됨
	//출력하는게 중요한 이유: 디버깅 하면서 제대로 내가 입력한 코드가 잘 동작하는지 보기 위해서

	printf("문자로 값 출력 : %c \n", 'A');		//A 으로 출력됨
	printf("16진수로 값 출력 : 0x%x \n", 'A');		//0x41 ==> 0100 0001 (2진수)으로 출력됨
	printf("10진수로 값 출력 : %d \n", 'A');		//65으로 출력됨
	printf("나이 : %d, 이름 : % s, 몸무게 : %.2lf \n", 50, "hong", 68.34);
	//%.2lf : 소수점 이하 2자리 출력

	printf("정수 크기: %d byte \n", sizeof(5));
	printf("string 크기: %d byte \n", sizeof("the"));	//string의 맨 앞의 주소 저장됨
	printf("문자 크기: %d byte \n", sizeof('a'));
	printf("실수 상수 크기 : %d byte \n", sizeof(3.14));

	return 0;
}
```

```c
#include <stdio.h>

int main(void) {
	int data = 8;

	//데이터 입력받아 저장하기
	
	
	printf("data변수의 주소 : 0x%p \n", &data);		// & : 주소 반환 연산자
	//주소는 4바이트(32비트)
	printf("정수 데이터 입력 : ");
	scanf_s("%d", &data);
	printf("data : %d\n", data);

	return 0;
}
```

```c
#include <stdio.h>
void print_binary(unsigned char character_var)
{
	for (int idx = 0; idx < 8; idx++)
	{	// 0x80 = 1000 0000
		if ((character_var & (0x80 >> idx)) == 0)
			printf("0");
		else
			printf("1");

	}
	printf("\n");
	return;
	}

int main() {
	int val = 5;

#if(0)
	val++;	//증가 연산자==> val = val+1; , 선 연산 후 증가
	++val;	//선 증가 후 연산;

	//++ ==> 연산 후 변수가 업데이트 된다
	printf("val : %d \n", ++val);
	printf("val : %d \n", val++);
#endif

	printf("val : %d \n", val + 1);		//6
	printf("val : %d \n", val);			//5

	//쓰레기코드
	int data = 3;
	if (data = 5)		//--> 조건문이 안들어가고 대입문들어감
		// 문법적으로는 맞지만 사용자의 의도와 맞지 않는 코드(알콜코드)
		printf("data: %d\n", data);

	while (getchar() != '\n');	//스트림버퍼 클리어
	//마지막 개행문자가 나오면 입력받는걸 중지함

	//삼항연산자
	int data1 = 8;
	int val1 = 7;

	(data1 < val1) ? printf("크다!!\n") : printf("작다!!\n");

	//비트연산

	int a = 0b00000000000000000000000000001010;
	int b = 0b00000000000000000000000000001100;
	printf("a를 10진수로 : %d\n", a);

	unsigned char data3 = 0x6C;			//0110 1100
	unsigned char val3 = 0x37;			//0011 0111
	unsigned char result = 0;

	printf("& 연산 : 0x%x\n", data3 & val3);			//0010 0100
	printf("| 연산 : 0x%x\n", data3 | val3);		//0111 1111	
	printf("^ 연산 : 0x%x\n", data3 ^ val3);

#if(0)
	unsigned char data4 = 10;		//==>부호비트 사용하지 않음
	char data5 = -10;		//==> 부호비트 사용
	printf(">>2 연산 : 0x%x\n", data4 >> 2);
	printf(">>2 연산 : 0x%x\n", data5 >> 2);
#endif

	//2진수 출력하는 함수
	unsigned char data6=0x9C;		//= 1001 1011
	for (int idx = 0; idx < 8; idx++)
	{	// 0x80 = 1000 0000
		if ((data6 & (0x80>>idx)) == 0)
			printf("0");
		else
			printf("1");

	}
	printf("\n");

	print_binary(data6);

	return 0;
	

}
```

### scanf_s  char 변수형 입력받을때 sizeof 입력받는다

```c
//scanf 
char input;
scanf_s(”%c”, &input, sizeof(input);
```

### 스트림 버퍼 클리어 코드

```c
//스트림 버퍼 클리어 코드※※※※※※※※※※※※※※※※※※※※※※※※
	while (getchar() != '\n');	//개행문자를 만날때까지 스트림버퍼 내용을 읽어서 버려라!!!!!!!
	//while + ; non-operation
	//※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※

```

```c
#include<stdio.h>

//입력한 한 문자가 영문 한문자가 대문자이면 소문자로 출력
// 입력한 한 문자가 소문자이면 대문자로 변환 출력
int main() {

	char alphabet = 0;
	char diff = 'a' - 'A';
	scanf_s("%c", &alphabet,sizeof(char));

	//스트림 버퍼 클리어 코드※※※※※※※※※※※※※※※※※※※※※※※※
	while (getchar() != '\n');	//개행문자를 만날때까지 스트림버퍼 내용을 읽어서 버려라!!!!!!!
	//while + ; non-operation
	//※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※

	if (alphabet < 'a')	//alphabet 대문자일때
		printf("%c\n", alphabet + diff);
	else           //소문자일때
		printf("%c\n", alphabet - diff);

	return 0;
}
```

```c
#include <stdio.h>

int main() 
{
	int selmenu = 0;
	while (1) {
	printf("1. 입력 2.출력 3.종료\n");
	scanf_s("%d", &selmenu);
	while (getchar() != '\n');

	
		switch (selmenu) {
		case 1:
			printf("입력처리\n");
			break;
		case 2:
			printf("출력처리\n");
			break;
		case 3:
			printf("프로그램 종료\n");
			break;
		default:
			break;
		}

		if (selmenu == 3)	break;

	}
	
	return 0;
}
```

- C로 컴파일 할때는 sourcefilename.c로 확장자가 .c 이다( 따로 입력 안하면 .cpp로 됨)
- scanf_s 오류시 뒤에 sizeof(변수종류) 이렇게 하면 사이즈까지 입력받아서 좀더 안전하게 만들 수 있다

```c
scanf_s("%c", 변수이름, sizeof(변수이름));
scanf_s("%s", 변수이름, sizeof(변수이름));
```

- c언어 특: 이식성이 뛰어나다
- #전처리기 구문(컴파일 전단계에서 작동하는 코드)
- 컴파일: 영문 코드를 기계어로 번환해주는 과정
 위에서부터 순서대로 실행함
- 컴파일러: 컴파일을 진행해주는 유틸리티
- 입출력을 여러번 할때, 버퍼로 인해 오류가 생길수 있으므로 stringbuffer 제거하는 함수 만들어줘야 함
- 대입연산자 기준으로 양쪽의 자료형을 일치시켜야 한다
    
    

```c
double temp=8     (x)
double temp=8.0;  (o)
```

- 제어문 if,else if, else이용한다
경우의 수가 여러가지 있을때→ switch, case 이용

- double형 사용할때 항상 lf 사용

```c
double temp=0.0;
scanf_s("%lf",&temp);

```

- 스트링버퍼 클리어

```c
	while (getchar() != '\n');	//스트림버퍼 클리어
	//마지막 개행문자가 나오면 입력받는걸 중지함
```

## 2023.02.21

- 정수형일때 비트의 맨 앞 자리는 부호를 나타내는자리
- 연산할때 2진수의 보수가 들어가는 점….

```c

```

## switch case 구문 예제

```c
int temp = 3;		//초기화
	// 관리할 데이터 종류?
	printf("정수 데이터 입력하시오\n");
	scanf_s("%d", &temp);
		
	switch (temp) 
	{
	case 1:
		printf("temp ==1 \n");
		break;	// break: switch case문 탈출함(break 안쓰면 case 2 실행된다)
	case 2:
		printf("temp ==2 \n");
		break;	// break: switch case문 탈출함
	case 3:
		printf("temp ==3 \n");
		break;	// break: switch case문 탈출함

	default:	//else 와 같음
		break;
	}
```

switch case도 중첩사용 가능하다(트리구조)

## 전역변수 선언→ 자동으로 0으로 선언

```c
#include <stdio.h>

int count;

int main() {
	
	printf("%d", count);
	return 0;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/639592a6-90ae-4c24-9d04-8d39aa172f66/Untitled.png)

0으로 초기화 된다

**※지역변수는 초기화 안하면 쓰레기 값을 갖게 된다**

## 반복구문→ 동일한 명령을 여러번 반복 수행해야 할 경우 사용

- while(), for(), do~while()   → 조건이 참일 경우에 계속 수행

## for문

```c
for (int count = 0; count < 5;++count)	//count는 for문 안에서만 사용되는 변수가 됨
	{
		printf("count: %d\n", count);
	}

	printf("count: %d\n", count);    //오류 발생!!!!
```

※ count는 for문 안에서 선언된 지역변수이므로

for문 밖에서 접근하려고 하면 오류가 발생한다

```c
int count = 0;
for (count = 0; count < 5;++count)	//count는 for문 안에서만 사용되는 변수가 됨
	{
		printf("count: %d\n", count);
	}

	printf("count: %d\n", count);    //오류 발생 안함!!!!
```

다음과 같은 경우에는 오류가 발생하지 않는다

## while문 주의할점

```c
int count = 0;
	while (count < 5)		//조건문
	{
		printf("count : %d\n", count);
		count++;
	}

출력
count : 0
count : 1
count : 2
count : 3
count : 4
```

while문 뒤에 ; 있으므로 {} 괄호 안에 있는 내용이 실행되지 않는다

count++작동하지 않으므로 **무한루프 발생한다**

```c
int count = 0;
	while (count < 5);		//조건문
	{
		printf("count : %d\n", count);
		count++;
	}

출력
```

```c
for (data = 1; data < 5; data++); 
	{
		printf("data\n");
	}
```

```c
data 
```

data 한번 출력된다

data는 5까지 실행됨

<aside>
📝 퀴즈

</aside>

## Q1. 1~10 사이 정수 중 짝수의 합을 계산해서 출력

```c
int sum = 0;
	for (int count = 1; count <= 10; count++) {
		if (count % 2 == 0)
			sum += count;
	}
	printf("sum: %d", sum);
```

## Q2. 작은 정수와 큰 정수 두개의 정수를 입력 저장하고 두 정수 사이의 합을 계산해서 출력

```c
int big = 0, small = 0, sum = 0;
	scanf_s("%d %d", &big, &small);

	//치환
	if (small > big) {
		int temp = big;
		big = small;
		small = temp;
	}
	for (int i = small; i <= big; i++) {
		sum += i;
	}

	printf("큰수: %d, 작은수 : %d, 사이의 합 : %d", big, small, sum);
```

- 임베디드 쪽에서는 라이브러리 잘 안씀→ 헤더파일 불러올때 많은 함수 불러오게 됨

# 함수

### 함수 구조

- 함수 원형 : main()함수 위에 놓여짐(pre declaration), 컴파일러에게 특성함수가 호출될것을 미리 알려주는 역할
- 함수 호출: main() 함수 내부에서 호출
- 함수 정의: main함수 아래 ,다른 파일에 있는건 상관 없음

### 

```c
#include <stdio.h>

//함수 원형 선언
//함수 원형에서는 매개변수 이름 생략 가능(자료형은 써야한다)
int addData(int, int);

int main() {
	int num1 = 5, num2 = 4;

	//함수 호출
	//num1,num2: 전달인자
	//함수명은 변수명처럼 작명(의미를 부여한다)
	int result=addData(num1, num2);		//함수 호출--> 함수 정의부분으로 점프
	return 0;
}

//함수 정의
//함수 정의에는 대입 연산자가 숨어있다
// 매개변수 = 전달인자	--> 전달인자가 매개변수에 저장된다
//int m_num1, m_num2 = num1,num2
//num1, num2(main 함수의 지역변수) --> addData에서 접근 불가능
int addData(int m_num1, int m_num2) {
	int sum= m_num1 + m_num2;
	return sum;
}
```

**함수 원형 선언**

```c
int addData(int, int);
```

함수 원형에서는 매개변수 이름 생략 가능(자료형은 써야한다)

**함수 호출**

```c
addData(num1, num2);
int result=addData(num1, num2);		
```

return 값은 return 레지스터에 임시적으로 저장되어지지만, 버려질 여지가 있기 때문에 return값을 복사해서 따로 변수에 저장해야한다.

```c
int addData(int m_num1, int m_num2) {
	int sum= m_num1 + m_num2;
	return sum;
}
```

함수 정의
함수 정의에는 대입 연산자가 숨어있다
 매개변수 = 전달인자 → 전달인자가 매개변수에 저장된다
int m_num1, m_num2 = num1,num2
num1, num2(main 함수의 지역변수) --> addData에서 접근 불가능

## stack pointer

[[10. 컴퓨터 구조에 대한 세번째 이야기]](https://popcorntree.tistory.com/61)

## 분할 컴파일

함수가 실행되는 파일, 소스파일, 헤더파일로 분할컴파일 가능하다

### main.c

함수 호출

main 함수가 있는 파일(함수가 실행되는 파일)

```c
#include <stdio.h>
#include "myfunction.h"

int main() {
	int num1 = 5, num2 = 4
	int result=addData(num1, num2);
	return 0;
}
```

### myfunction.c( 소스파일)

함수 정의

함수를 정의해둔 파일

```c
#include <stdio.h>

int addData(int m_num1, int m_num2) {
	int sum = m_num1 + m_num2;
	return sum;
}

int display(int m_result) {
	int a = 2;
	double b = 3.0;

	printf("%lf", a + b);
}
```

### myfunction.h

함수 원형

```c
#pragma once		//중복 컴파일 방지

int addData(int, int);
int display(int);
```

## Q3. 입력한 영문 소문자 부터 z까지 출력해주는 함수 구현

```c
#include <stdio.h>

void Display_alpha(char);

int main() {
	char data = 0;
	printf("한문자 데이터 입력");
	scanf_s("%c", &data, sizeof(char));

	Display_alpha(data);
	return 0;
}

void Display_alpha(char m_data) {
	if ((m_data >= 'a') && (m_data <= 'z'))
	{
		for (char cnt = m_data; cnt <= 'z'; cnt++) {
			printf("%c\n", cnt);
		}
	}
}
```

## call by value, call by reference

# 포인터

---

## 포인터 용량

```c
int* iptr;
double* dptr;
char* cptr;

printf("%d\n", sizeof(iptr));
printf("%d\n", sizeof(dptr));
printf("%d\n", sizeof(cptr));

출력
4
4
4
```

포인터는 데이터가 저장되기 시작한 주소값이기 때문에 **변수 종류에 상관없이 모두 4byte**이다

```c
double db = 3.5;
&db(=double *)
&&db(=double**)
```

& 기호가 붙으면 원래 자료형에 *이 붙은 [자료형*]이 된다.

### 포인터 변수 출력법

```c
int data = 5;
int* ptr = &data;
printf("%d\n", data);
printf("0x%p\n", &data);
printf("0x%p\n", ptr);
printf("%d\n", sizeof(ptr));		
```

32bit 주소체계에서 모든 포인터변수의 크기는 4바이트의 크기로 동일

### 역참조(*)

```c
	printf("*ptr == *(&data) is true? %d\n", *ptr == *(&data));
	printf("*(&data) == data is true %d\n", *(&data) == data);

출력
1
1
```

*ptr == *(&data) == data == 5

# ※포인터 연산, 포인터 형변환

```c
#include <stdio.h>

int main() {

	int data = 0x12345678;
	char* data2 = (char*)&data;  // 포인터의 형변환
	printf("0x%x \n", *data2(=data2[0]);         //0x78 출력
	printf("0x%x \n", *(data2+3)(=data2[3]));     //0x12 출력
	return 0;
}
```

&data는 4byte씩 접근,

(char*)&data로 형변환 하여서 1byte씩 접근하는 것으로 변경

대부분 컴퓨터는 little Endian 체제

[코딩교육 티씨피스쿨](http://www.tcpschool.com/c/c_refer_endian)

int data(4byte) 는 0x12345678에서 

78은 0x3000주소

56은 0x3001주소

34는 0x3002주소

12는 0x3003주소

에 저장되어 있다

포인터 연산에서는 나누기 곱하기 안되고 + - 만 가능!!!(시험문제 나옴 

1. **포인터변수 용도 메모리 주소 저장용**
2. **포인터변수 자료형의 역할 몇 바이트를 참조할지 결정하는 역할**
3. **포인터 연산은 포인터변수에 +1했을경우 포인터변수 자료형 크기만큼 주소가 증가**

## 배열

```c
#include <stdio.h>

int main() {
	
	double arrdb[5] = { 1.2,2.2,3.3,4.4,5.5 };
	int len = sizeof(arrdb) / sizeof(double);
	double total = 0;

	//반복문 활용해서 배열 각 요소의 총 합을 계산해서 total 변수
	for (int cnt = 0; cnt < len; cnt++) {
		total += arrdb[cnt];
		printf("arrdb[%d]: %.2lf , &arrdb[%d] : 0x%p\n", cnt, arrdb[cnt],cnt, &arrdb[cnt]);
	}

	//배열명은 배열의 시작주소 arrdb=&arrdb[0]
	printf("arrdb: %.2lf , &arrdb : 0x%p\n", *arrdb, arrdb);
	printf("%.2lf\n", total);

	return 0;
}
```

arrdb=&arrdb[0]

즉 배열의 이름은 배열 시작점 주소값이다

## 배열과 포인터연산의 관계

```c
double arrdb[5] = { 1.2,2.2,3.3,4.4,5.5 };
	int len = sizeof(arrdb) / sizeof(double);
	double total = 0;
	
	for (int cnt = 0; cnt < len; cnt++) {
		printf("arrdb[%d] : %.2lf,  &arrdb[%d] : 0x%p\n",cnt,arrdb[cnt],cnt,&(arrdb[cnt]));
	}

	for (int cnt = 0; cnt < len; cnt++) {
		printf("*(arrdb+%d) : %.2lf,  &(arrdb+%d) : 0x%p\n", cnt, *(arrdb+cnt), cnt, (arrdb+cnt));
	}

**출력**
arrdb[0] : 1.20,  &arrdb[0] : 0x003CFD54
arrdb[1] : 2.20,  &arrdb[1] : 0x003CFD5C
arrdb[2] : 3.30,  &arrdb[2] : 0x003CFD64
arrdb[3] : 4.40,  &arrdb[3] : 0x003CFD6C
arrdb[4] : 5.50,  &arrdb[4] : 0x003CFD74
*(arrdb+0) : 1.20,  &(arrdb+0) : 0x003CFD54
*(arrdb+1) : 2.20,  &(arrdb+1) : 0x003CFD5C
*(arrdb+2) : 3.30,  &(arrdb+2) : 0x003CFD64
*(arrdb+3) : 4.40,  &(arrdb+3) : 0x003CFD6C
*(arrdb+4) : 5.50,  &(arrdb+4) : 0x003CFD74
```

배열과 포인트 연산의 공식 : **arr[index] = *(arr + index)**  or  **&arr[index] = arr + index**

## 함수 매개변수로 배열 넘겨주기

```c
#include <stdio.h>

void DisplayArr(int len, double* m_arr) {
	for (int cnt = 0; cnt < len; cnt++) {
		printf("arrdb+%d : %.2lf,  arrdb+%d : 0x%p\n", cnt, *(m_arr + cnt), cnt, (m_arr + cnt));
    //printf("arrdb+%d : %.2lf,  arrdb+%d : 0x%p\n", cnt, m_arr[cnt], cnt, (m_arr + cnt));
	}
}

int main() {
	
	double arrdb[5] = { 1.2,2.2,3.3,4.4,5.5 };
	int len = sizeof(arrdb) / sizeof(double);
	DisplayArr(len, arrdb);
	return 0;
}

**출력**
arrdb+0 : 1.20,  arrdb+0 : 0x0053F7B0
arrdb+1 : 2.20,  arrdb+1 : 0x0053F7B8
arrdb+2 : 3.30,  arrdb+2 : 0x0053F7C0
arrdb+3 : 4.40,  arrdb+3 : 0x0053F7C8
arrdb+4 : 5.50,  arrdb+4 : 0x0053F7D0
```

**DisplayArr(len, arrdb);**

len: 배열의 길이

arrdb: 배열의 시작 주소

배열 이름이 배열 시작점의 주소이므로 배열 이름과 배열의 길이를 넘겨주어 , 포인터 연산을 통하여 배열 원소에 접근할수 있다. 이를 이용하여 배열 원소에 접근함

**간접접근** : 또한 포인터 arrdb를 넘겻을때 포인터 연산대신 m_arr[cnt]처럼 배열형태로 접근하여도 원소에 접근 가능하다

직접접근 간접접근