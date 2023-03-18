# SWIP 02.24

## strcmp

strcmp → 두 문자열 비교하고 같으면 0반환, 다르면 0 이외의 값 반환

## void형 포인터

void형 포인터는 어떤 자료형의 주소도 다 저장할수있다

```c
int a=3;
void* p3=&a;		
// *p3 = 4 --> 이거 불가능 (자료형 모르기 때문)
(*(int*)p3) = 4;

printf("%d", p3);
```

### strcmp

문자열 일치 확인 함수

```c
char* str1="Hyun";
char* str2 = "Hyundai";

printf("%d",strcmp(str1, str2));

출력:1
```

### 완벽하게 일치하지 않을때

```c
char* str1="Hyun";
char* str2 = "Hyun";

printf("%d",strcmp(str1, str2));

출력:0
```

## const 위치

```c
const int* p1=NULL;	//값을 상수화( *p1 = 0  불가능
int* const p2=NULL;	//주소를 상수화( p2 =0xffff 불가능)
```

## 2중 포인터

```c
int data = 3;
	int* pD = &data;
	int** pS = &pD;
```

## 1차원 배열

```c
int array[5];
	int* pA = array;		//배열 매개변수로 전달할때 포인터로 전달한다
	*(pA + 1) = pA[1];
```

## 2차원 배열

```c
//3. 2차원 배열
int arrdb[3][5];
//배열 포인터는 2차원 배열의 시작 주소를 가르키는 포인터
printf("%d\n", sizeof(arrdb[0][0]));				//--> int 형 자료
printf("%d\n", sizeof(arrdb[0]));					//1 행을 나타내는 시작주소 
int* pT1 = arrdb[0];								//(자료형 = int*)
printf("%d\n", *(arrdb[0] + 1) == arrdb[0][1]);		//--> 참

printf("%d\n", sizeof(arrdb));						//2차원 배열의 시작주소
printf("%d\n", arrdb == arrdb[0]);
printf("%d\n", arrdb[0] == &(arrdb[0][0]));			//arrdb=arrdb[0]=&(arrdb[0][0])
int(*pT)[5] = arrdb;	//2차원 배열 매개변수로 전달할때 배열 포인터로 전달한다.
printf("%d\n", sizeof(pT));		//4
printf("%d\n", pT[0] == pT);						//서로 같은 주소, 하지만 참조하는 포인터의 길이가 다르다
printf("%d\n", sizeof(pT[0]));						//20
printf("%d\n", sizeof(pT));							//4		서로 주소는 같지만 길이가 다르다
```

## string

```c
char* pS = "Hyundai";						//pS는 "Hyundai"라는 string 상수의 시작주소
char Name[30] = "Hyundai";					//Name이라는 char형 배열에 "Hyundai" 복사되어 들어감
```

## 구조체 padding byte

```c
#include <stdio.h>

typedef struct {
	char ch;					//1
	//패딩 바이트 발생
	char com_name[50];			//50
	char team_name[50];			//50
	int sawon;					    //4
	double weight;				  //8
}EMP;	//구조체 자료형 정의

int main() {

	//회사명, 부서명, 사원번호 3가지의 정보를 관리할 구조체 설계
	printf("int size : %d\n", sizeof(int));      //4
	printf("EMP size : %d\n", sizeof(EMP));      //120?? 

	return 0;
}
```

## char형 배열에 string 대입하기

```c
//배열명은 포인터 상수이기때문에 주소 변경할 수 없음!!
	char buf[50];
	**buf = "hyundai";**    **불가능!!!**
	//이렇게 일일이 처야 한다
  buf[0] = 'h';
	buf[1] = 'y';
	buf[2] = 'u';
	buf[3] = 'n';
	buf[4] = 'd';
	buf[5] = 'a';
	buf[6] = 'i';
```

```c
#include <stdio.h>
#include <string.h>

typedef struct {
	char ch;					//1
	//패딩 바이트 발생
	char com_name[50];			//50
	char team_name[50];			//50
	int sawon;					//4
	double weight;				//8
}EMP;	//구조체 자료형 정의

copyStr(char* str, int str_length, char* str_copy) {

	for (int i = 0; i < str_length; i++) {
		str[i] = '\0';
	}
	for (int i = 0; str_copy[i] != '\0'; i++) {
		str[i] = str_copy[i];
	}

}

//동일한 구조체 = 구조체 변수는 멤버변수 대 멤버변수 자동 복사
//구조체를 매개변수로 하는 함수는 구조체 아래 있어야 한다
void DisplayData(EMP data) {
	printf("EmployeeID : %d\n", data.ch);
	printf("data.com_name : %s\n", data.com_name);
	printf("Team name : %s\n", data.team_name);
}

int main() {

	//배열과 동일하게 초기화 개수가 부족하면 
	//부족한 부분은 자동으로 0으로 초기화
	EMP data={ 5 , "Hyundai NGV", "QA" };
	//구조체 변수 선언과 동시 초기화
	//배열에서 초기화 요소 부족하면 나머지 0으로 초기화 되듯이
	//구조체도 비슷하다.(0 = NULL)
	printf("EmployeeID : %d\n", data.ch);
	printf("data.com_name : %s\n",data.com_name);
	printf("Team name : %s\n", data.team_name);

	//실질적으로 데이터는 멤버 변수에 들어감
	// 구조체는 데이터 시작 주소만 담고 있음
	// (.) 은 구조체 멤버 직접 접근 연산자
	// data.com_name = "hyundai";  //불가능
	copyStr(data.com_name,50, "hyundai");
	DisplayData(data);

	return 0;
}
```

## 구조체 함수 포인터 전달

```c
#include <stdio.h>

typedef struct {
	//패딩 바이트 발생
	int EmployeeID;					//4
	char ComName[30];				//50
	char Management[30];			//50
	
}EMP;	//구조체 자료형 정의

void DisplayInfo(EMP* m_pi, int length);

int main() {

	EMP personinfo[3] = {   {100,"Hyundai","car"},			//personinfo[0]
							{200,"KIA","Machine"},			//personinfo[1]
							{300, "NGV","QA" }};			//personinfo[2]		//구조체 배열-->엄청나게 큰 용량잡아먹음, 동시 초기화
	
	DisplayInfo(personinfo,3);

	return 0;
}

void DisplayInfo(EMP* m_pi,int length) {
	for (int i = 0; i < length; i++) {
		printf("%d\n", m_pi[i].EmployeeID);
		printf("%s\n", m_pi[i].ComName);
		printf("%s\n", m_pi[i].Management);
	}
}
```

## 구조체 동적할당으로 함수 내부에서 자료 생성하고 main문으로 동적할당 주소 보내주기

```c
#include <stdio.h>

typedef struct {
	//패딩 바이트 발생
	int EmployeeID;					//4
	char *ComName;				//50
	char *Management;			//50

}EMP;	//구조체 자료형 정의

EMP* InsertData(void);
void DisplayData(EMP* data);
void freeEMP(EMP* m_emp);

int main() {
	EMP* pSt = NULL;
	pSt = InsertData();			//1사람의 정보를 입력, 저장
	DisplayData(pSt);
	free(pSt);
	return 0;
}

void freeEMP(EMP* m_emp) {
	free(m_emp->ComName);
	free(m_emp->Management);
	free(m_emp);
	m_emp = NULL;
}

EMP* InsertData(void) {

	EMP* m_emp;
	m_emp = (EMP*)malloc(sizeof(EMP));		//heap에 할당하여 함수 종료되도 메모리에 남아있게 한다
	m_emp->ComName = (char*)malloc(sizeof(char) * 20);
	m_emp->Management = (char*)malloc(sizeof(char) * 20);
	printf("EmployeeID 입력하시오\n");
	scanf_s("%d", &(m_emp->EmployeeID));
	printf("ComName 입력하시오\n");
	scanf_s("%s", m_emp->ComName,20);
	printf("Management 입력하시오\n");
	scanf_s("%s", m_emp->Management,20);

	return m_emp;
}

void DisplayData(EMP* data) {

	printf("EmployeeID : %d\n", data->EmployeeID);
	printf("data.com_name : %s\n", data->ComName);
	printf("Team name : %s\n", data->Management);

}
```

## 함수 내부에서 동적할당 후 main 문에서 동적할당된 메모리 이용

```c
#include <stdio.h>
char* dongjuck(void);

int main() {
	char* arr=NULL;
	arr=dongjuck();
	free(arr);

	return 0;
}

char* dongjuck(void) {

	char* temp_add;

	temp_add = (int*)malloc(sizeof(char) * 10);		//int형 자료 10개 들어가는 배열을 동적할당하고 시작주소를 temp_add에 반환함
	scanf_s("%s", temp_add,10);						//printf로 string 받을때 문자열 자리수 끝에 넣어줘야함

	return temp_add;
}
```

동적할당을 함수 내부에서 하면 메모리 heap 영역이 확보되고 시작 주소를 포인터로 반환한다. 

**함수가 종료되어도 malloc으로 확보된 heap 메모리 영역은 사라지지 않기 때문에** 포인터 주소를 return 하면 main 문에서도 동적 할당된 메모리를 사용할 수 있다.

## Stack 구현

```c
#include <stdio.h>

typedef struct {
	//패딩 바이트 발생
	int EmployeeID;					//4
	char ComName[30];				//50
	char Management[30];			//50
}EMP;

void PushStack(EMP* pPush, int* pSp);
int DisplayMenu(void);
EMP PopStack(EMP* pPop, int* pSp);
void DisplayStack(EMP* pDis, int pSp);

int main() {
	EMP Stack[5];
	//stack : 포인터를 제어하여 꺼낼 위치를 제어함
	int Sp = 0;			//스택 위치 조절 변수
	int selmenu = 0;
	EMP popdata;
	while (1) {
		selmenu = DisplayMenu();

		switch (selmenu) {
		case 1:
			if (Sp < 5)
				PushStack(Stack, &Sp);		//스택 공간에 데이터를 저장
			else
				printf("Full Stack!\n");
			break;
		case 2:
			if (Sp > 0)
			{
				popdata = PopStack(Stack, &Sp);
				printf("popdata : %d, %s, %s \n", popdata.EmployeeID, popdata.ComName, popdata.Management);
			}
			else
				printf("Empty Stack!\n");
			break;
		case 3:
			DisplayStack(Stack,Sp);
			break;
		case 4:
			printf("프로그램 종료!!\n");
			break;
		}
		if (selmenu == 4) {
			
		}

	}
	return 0;
}

EMP PopStack(EMP* pPop,int * pSp) {
	return pPop[--(*pSp)];
}

void PushStack(EMP* pPush,int* pSp)
{	
	printf("EmployeeID 입력 : ");
	scanf_s("%d", &pPush[*pSp].EmployeeID);
	printf("ComName 입력 : ");
	scanf_s("%s", pPush[*pSp].ComName,30);
	printf("Management 입력 : ");
	scanf_s("%s", pPush[*pSp].Management,30);
	(*pSp)++;

}

void DisplayStack(EMP* pDis, int pSp) {
	if (pSp == 0)
		return;
	for(int idx=pSp-1;idx>=0;idx--)
	{
	printf("EmployeeID: %d\n", pDis[idx].EmployeeID);
	printf("ComName : %s\n", pDis[idx].ComName);
	printf("Management : %s\n", pDis[idx].Management);
	}
}

int DisplayMenu(void) {

	int sel = 0;
	printf("1. PushStack\n");
	printf("2. PopStack\n");
	printf("3. Displaystack\n");
	printf("4. Exit \n");
	scanf_s("%d", &sel);
	while (getchar() != '\n');
	return sel;
}
```

( Last In First Out → LIFO 구조)

나중에 들어온 데이터가 제일 먼저 나가게 되는 구조

stack index 변수를 생성하여, 데이터를 넣고 뺄때마다 stack index 변수값을 조절한다

## Linked List

```c
#include <stdio.h>

#include <stdio.h>

typedef struct person{
	//패딩 바이트 발생
	char ComName[30];		

	struct person* next;		//자기 참조 포인터
}EMP;

int main(){
	
	EMP* pSt = NULL;
	EMP* pBackup;
	EMP data1 = {"Hyundai"};
	EMP data2 = { "NGV" };
	EMP data3 = { "Study" };
	pSt = &data1;
	data1.next = &data2;		
	data2.next = &data3;
	data3.next = NULL;
	pBackup = pSt;

	while (pBackup != NULL) {
		printf("출력 : %s \n", pBackup->ComName);
		pBackup = pBackup->next;
	}

	return 0;
}
```

## 구조체 비트필드

→ 임베디드에서 봄