# SWIP 02.22

### 직접접근과 간접접근

```c
int data = 0; // 변수명으로 해당공간을 접근 --> 직접 접근
data = 33;		//직접접근

int* ptr = &data;			// 포인터 변수 = 변수의 시작주소
// *ptr == *(&data) == data
*ptr = 55;		//간접 접근
```

## 배열

### 배열의 선언, 초기화

```c
//배열 : 많은 양의 데이터를 효율적으로 관리 ==> 배열
// 배열은 정적할당, 컴파일 할때 메모리를 잡으므로 배열의 길이를 넣어야 한다 --> 저장공간이 낭비될 수 있음
// 변수 초기화는 중괄호 이용해서 함
int arr[5] = {0};
```

### 배열의 직접접근

```c
arr[3] = 4;		//배열의 직접접근 --> 배열은 통째로 복사할 수 없다
```

### 배열명과 주소

```c
//&arr[0] == arr;   --> 배열명은 배열의 시작주소
```

 

### 포인터 연산과 배열

```c
//포인터 연산 --> 포인터 자료형의 크기만큼 주소가 증가시켜 메모리에 접근한다
int* pA = arr;
//*(pA + 0) == pA[0];
//*(pA + 1) == pA[1];
//*(pA + 2) == pA[2];		

printf("pA + 0 = 0x%p\n ", pA + 0);
printf("pA + 1 = 0x%p\n ", pA + 1);
printf("pA + 2 = 0x%p\n ", pA + 2);
printf("arr+1 = 0x%p\n ", arr+1);

printf("pA++ = 0x%p\n ", pA++);
printf("arr++ = 0x%p\n ", arr++);     //오류!!
**//배열명은 바뀔수 없는 포인터 상수이다(const int*)**

출력
pA + 0 = 0x0113FC30
pA + 1 = 0x0113FC34
pA + 2 = 0x0113FC38
```

※ ++ptr 과 (ptr+1) 차이 주의!!!

### 포인터 상수 and 상수 포인터

```c
int val = 55;
int res = 33;
printf("&val= 0x%p\n", &val);
printf("&res= 0x%p\n", &res);

//1  데이터 고정
int const *pT1 = &val;
pT1 = &res;		//주소는 바꿀 수 있지만
//*pT1 = 5;		// 오류 ->
//간접접근으로 상수화는 오류(포인터 변수로 간접해서 내용을 바꾸는 쓰기 문법은 상수화)
printf("*pT1= %d\n", *pT1);
printf("pT1= 0x%p\n\n", pT1);

//2   주소 고정
int* const pT2 = &val;
//pT2 = &res;		//오류!! -> 주소가 고정되어있음
*pT2 = 5;			//간접겁근
printf("*pT2= %d\n", *pT2);
printf("pT2= 0x%p\n\n", pT2);

//3  데이터, 주소 모두 고정
const int* const pT3 = &val;
//pT3 = &res;		//오류!! -> 주소가 고정되어있음
//*pT3 = 5;			//오류!! -> 간접겁근
printf("*pT3= %d\n", *pT3);
printf("pT3= 0x%p\n\n", pT3);

출력
&val= 0x00CFFE04
&res= 0x00CFFDF8

*pT1= 33
pT1= 0x00CFFDF8

*pT2= 5
pT2= 0x00CFFE04

*pT3= 5
pT3= 0x00CFFE04
```

const 오른쪽에 있는 구조를 변하지 않게 고정(상수화 한다!)

int const *pT1 → 데이터 고정

int* const pT2 →  주소 고정

### Q 함수에 배열 전달하는 문제

배열을 전달할때

배열의 이름(포인터), 배열의 크기(sizeof이용) 총 2개를 전달해야 배열에 접근 가능하다.

```c
int EvenSumData(int* const m_arr, int arr_size);

int main() {
	int arr[8] = { 6,5,8,3,9,11,16,13 };
	int result = 0;
	int size = sizeof(arr)/sizeof(int);

	result = EvenSumData(arr, size);
	printf("%d\n", result);
	return 0;
}

int EvenSumData(int* const m_arr, int arr_size) {
	int sum = 0;
	for (int i = 0; i < 8; i++) {
		if (m_arr[i] % 2 == 0)
			sum += m_arr[i];
	}
	return sum;
}
```

# 문자열

---

## 문자열 초기화

```c
//문자배열
char str[8] = { 'H','y','u','n','d','a','i','\0'};
char str[8] = "Hyundai";
char str[]= "Hyundai";
```

문자열의 마지막 단어는 항상 null문자이어야 한다

문자열은 배열의 크기를 지정하지 않아도 자동으로 크기를 맞추어 준다

## 문자열 상수

```c
char *p="Hello world";
```

문자열 상수인 “Hello world”는 텍스트 세그먼트라는 메모리 영역에 담긴다.

※텍스트 세그먼트는 Read Only 이므로 변경할 수 없다. 

따라서 p는 “Hello World” 라는 문자열이 담긴 텍스트 세그먼트 영역 주소를 담고 있다.

따라서 p에 “another world” 라는 문자열을 넣을수 없다.

-출처: 

[문자열(1) 문자열 상수와 포인터 변수](https://sean.tistory.com/112)

[[C/C++] 문자열 상수 포인터](https://lktprogrammer.tistory.com/52)

## 문자열 출력

```c
for (int idx = 0; idx < 50; idx++)
		printf("%c \n", str1[idx]);
```

너무 귀찮으므로 밑에 방식으로 할 수 있음

```c
	// %s ==> 시작주소~ null문자(\0)을 만날때까지 입출력
	// 문자열 ==> 마지막에 항상 '\0' NULL 문자를 포함하고 있어야함	
	printf("%s", str1);
```

```c
char str[50] = "Hyundai";
char buff[50];

printf("%s", buff);

출력결과
儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆Hyundai
```

stack에 Hyundai 있는데 stack은 FILO이므로 buff가 먼저 출력되고 나중에 hyundai와 null문자 만나서 출력 종료된다

## Q1: 문자열 복사하는 함수 만들기

```c
#include <stdio.h>

void MyStrCpy(const char * m_str, char * m_buff) {
	int i = 0;
	for ( i = 0; m_str[i] != '\0' ; i++) {
		m_buff[i] = m_str[i];
	}
  //※※※※※※가장 하기 쉬운 실수※※※※
	**m_buff[i] = '\0';**	
	//마지막에 꼭 null 문자 추가하기

	return 0;
}

//같은 함수지만 이 방법으로도 가능!!!
void MyStrCpy(const char * m_str, char * m_buff) {
	while (*m_str != '\0')
		*m_buff++ = *m_str++;		
	//마지막에 꼭 null 문자 추가하기
	*m_buff = '\0';
	return 0;
}

int main(void) {

	char str[50] = "Hyundai";
	char buff[50];

	printf("%s \n", buff);

	MyStrCpy(str,buff);		//str 배열의 내용을 Buf 배열로 복사
	printf("%s \n", buff);

	return 0;

}

출력
儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆儆Hyundai
Hyundai
```

## %s : 시작주소 ~ null 만날때까지 출력

```c
printf("%s \n", "Hyundai NGV!!");		// printf 안의 "Hyundai NGV!!" 는 시작주소임 -->RODATA에 올라가는 자기자신의 시작주소
printf("0x%x \n", "Hyundai NGV!!");		// "Hyundai NGV!!" 문자열의 주소
```

## 🤞포인터로 문자열 접근하기

1. 배열로 접근
    
    ```c
    //배열로 문자열 접근
    char buf[] = "Hyundai NGV";
    int len = 0;
    while (buf[len] != '\0')
    	len++;
    printf("%d", len)
    ```
    
     “Hyundai NGV” 라는 문자열 상수(=RODATA)를 복사하여 buf라는 배열이 할당된 메모리에 복사함
    
2. 포인터로 접근
    
    ```c
    //포인터로 문자열 접근
    const char* pSt= "Hyundai NGV";	// 문자열 상수를 가르키는 포인터
    while (pSt[len] != '\0')
    {
    	printf("%c", pSt[len]);
    	len++;
    }
    printf("%d", len);
    ```
    
    포인터는 “Hyundai NGV” 라는 RODATA 자체의 주소로 접근
    
    그래서 앞에 const 붙여서 포인터가 가르키는 원소들을 수정하지 못하게 해야 한다.
    

## 이중 포인터

**이중포인터** : 싱글포인터 변수의 시작주소를 저장하는 용도로 싱글 포인터를 가르키는 포인터

```c
int A = 5;
	int *pA = &A;
	int** ppA = &pA; //=&(&A)
	// A = *pA = **pA
	// pA = &pA = &&A

```

포인터 문제 → **항상 그림 그려보기**

다음으로 출력되도록 ReverseData 함수를 구현하시오

```c
 #include <stdio.h>

void ReverseData(int** m_1, int** m_2) {

	int* temp = *m_1;
	*m_1 = *m_2;
	*m_2 = temp;
	return;
}

int main() {
	
	int data = 5;
	int val = 9;
	int* pD = &data;
	int* pV = &val;
	printf("data : %d, val : %d \n", data, val);	//직접 접근
	printf("*pD : %d . * pV : %d \n", *pD, *pV);	//간접 접근

	ReverseData(&pD,&pV);

	printf("data : %d, val : %d \n", data, val);	//직접 접근
	printf("*pD : %d . * pV : %d \n", *pD, *pV);	//간접 접근

	return 0;
}

출력
data : 5, val : 9
*pD : 5 . * pV : 9
data : 5, val : 9
*pD : 9 . * pV : 5 
```

# static

## 지역변수에서 사용될때

```c
#include <stdio.h>

void OutPutNum(void); 
int main() {

	int idx = 0;
	for (idx = 0; idx < 5; idx++)
		OutPutNum();
		return 0;
}

void OutPutNum(void) {
	//static : 메모리에 전역변수처럼 올라가있지만, 접근은 그 함수 내에서만 할수 있음
	//따라서 함수가 종료될때도 변수가 사라지지 않고 또다시 함수가 호출될때 이전에 호출될때 저장된 지역변수 값을 갖고 있다.
	// 메모리 존재시간 ==> 전역변수 처럼
	// 접근범위 ==> 지역변수 처럼
	static int count = 0;
	count++;
	printf("count : %d\n", count);
}
```

## 전역변수에서 사용될때

```c
#include <stdio.h>

static int a = 5;
```

static 변수가 선언된 파일 안에서만 변수가 사용된다는걸 의미함

static 변수 접근 스코프는 함수 파일 내부

# 포인터 배열 vs 배열 포인터

## 포인터 배열

```c
char* pS1 = "1. insert";
char* pS2 = "2. Display";
char* pS3 = "3. Exit";
```

```c
//포인터 배열
char* pAr[3]={"1. insert", "2. Display"  ,"3. Exit"};     
```

```c
#include <stdio.h>

int FindChar(char** m_pS, char m_char);

int main(){

	char* pS1 =" 1. insert";
	char* pS2 = "2. Display";
	char* pS3 = "3. Exit";

	//포인터 배열
	char* pAr[3] = { "1. insert", "2. Display"  ,"3. Exits" };
	int count = 0;
	count = FindChar(pAr, 's');
	printf("counnt : %d \n", count);
	return 0;
}

int FindChar(char** m_pS, char m_char) {
	int count = 0;
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j != '\n'; j++)
			if (m_pS[i][j] == m_char)
				count++;
	}

	return count;
}
```

포인터배열, 배열포인터, string 너무 헷갈림