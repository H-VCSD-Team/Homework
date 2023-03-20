- 정수형일때 비트의 맨 앞 자리는 부호를 나타내는자리
- 연산할때 2진수의 보수가 들어가는 점….

**switch case 구문 예제**

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

**전역변수 선언→ 자동으로 0으로 선언**

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

**반복구문→ 동일한 명령을 여러번 반복 수행해야 할 경우 사용**

- while(), for(), do~while()   → 조건이 참일 경우에 계속 수행

**for문**

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

**함수 구조**

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

**함수 정의**

함수 정의에는 대입 연산자가 숨어있다

매개변수 = 전달인자 → 전달인자가 매개변수에 저장된다

int m_num1, m_num2 = num1,num2

num1, num2(main 함수의 지역변수) --> addData에서 접근 불가능

**stack pointer**

[[10. 컴퓨터 구조에 대한 세번째 이야기]](https://popcorntree.tistory.com/61)

**분할 컴파일**

함수가 실행되는 파일, 소스파일, 헤더파일로 분할컴파일 가능하다

**main.c**

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

**myfunction.c( 소스파일)**

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

**myfunction.h**

함수 원형

```c
#pragma once		//중복 컴파일 방지

int addData(int, int);
int display(int);
```

**Q3. 입력한 영문 소문자 부터 z까지 출력해주는 함수 구현**

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

# 포인터

---

**포인터 용량**

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

**포인터 변수 출력법**

```c
int data = 5;
int* ptr = &data;
printf("%d\n", data);
printf("0x%p\n", &data);
printf("0x%p\n", ptr);
printf("%d\n", sizeof(ptr));		
```

32bit 주소체계에서 모든 포인터변수의 크기는 4바이트의 크기로 동일

**역참조(*)**

```c
	printf("*ptr == *(&data) is true? %d\n", *ptr == *(&data));
	printf("*(&data) == data is true %d\n", *(&data) == data);

출력
1
1
```

*ptr == *(&data) == data == 5

**※포인터 연산, 포인터 형변환**

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

포인터 연산에서는 나누기 곱하기 안되고 + - 만 가능!!!(시험문제 나옴)

1. **포인터변수 용도 메모리 주소 저장용**
2. **포인터변수 자료형의 역할 몇 바이트를 참조할지 결정하는 역할**
3. **포인터 연산은 포인터변수에 +1했을경우 포인터변수 자료형 크기만큼 주소가 증가**

# 배열

---

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

arrdb=&arrdb[0]즉 배열의 이름은 배열 시작점 주소값이다

**배열과 포인터연산의 관계**

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

**함수 매개변수로 배열 넘겨주기**

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

**직접접근  vs 간접접근**