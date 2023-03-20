# C언어 1일차

**L-value, R-value**

```c
int a, b;
a = 10;
b = a;
```

a=10 에서 a는 저장공간 → a의 값이 10으로 바뀜(L-value)

b=a에서 a는 값 → 저장 공간의 값을 복사하여 사용하므로 a 안바뀜→R-value(연산하거나 출력할때도 값을 복사하므로 변수의 값이 변하지 않는다)

**자동 캐스팅**

```c
	int a = 2;
	~~~~double b = 3.0;

	printf("%lf", a + b);
```

a는 int, b는 double로 서로 다른 형태의 변수가 연산될 경우에, 데이터 보존을 위해 더 넓은 형태의 변수로 캐스팅 된다

따라서 a+b는 double형이 되게 됨

## **C언어 기본 구조**

---

**c언어 특**

- 이식성이 뛰어나다

 

**#전처리기 구문**

- (컴파일 전단계에서 작동하는 코드)

**컴파일**

- 영문 코드를 기계어로 번환해주는 과정
위에서부터 순서대로 실행함

**컴파일러**

컴파일을 진행해주는 유틸리티

대입연산자 기준으로 양쪽의 자료형을 일치시켜야 한다

```c
#include <stdio.h>	
//컴파일 전 단계에서 stdio.h를 들고와서 포함하는 전처리문
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

**변수 예제 코드**

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

**문자열 입출력 예제 코드**

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

**데이터 주소 출력 예제 코드**

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

**증감 연산자 예제 코드**

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

**scanf_s  char 변수형 입력받을때 sizeof 입력받는다**

```c
//scanf 
char input;
scanf_s(”%c”, &input, sizeof(input);
```

**스트림 버퍼 클리어 코드**

- 입출력을 여러번 할때, 버퍼로 인해 오류가 생길수 있으므로 stringbuffer 제거하는 함수 만들어줘야 함
- 즉 개행문자를 만날때까지 스트림버퍼 내용을 읽어서 버리는 역할 함

```c
	while (getchar() != '\n');
	//while ... ; non-operation
	//※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※

```

**스트링 버퍼 클리어 코드 예제**

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

**입출력 메뉴 예제 코드**

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

## 정리

---

- char 형 변수 입력 받을때 scanf_s 오류시, 뒤에 sizeof(변수종류) 이렇게 하면 사이즈까지 입력받아서 좀더 안전하게 만들 수 있다
    
    ```c
    scanf_s("%c", 변수이름, sizeof(변수이름));
    scanf_s("%s", 변수이름, sizeof(변수이름));
    ```
    
- 변수 자료형ㅇ ‘=’ 기준으로 일치시킨다

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

- 스트링버퍼 클리어 코드