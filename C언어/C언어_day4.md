# C언어 4일차

포인터 사용할때는 **꼭 그림으로 그려**보면서 이해하기!!

배열포인터 : 배열을 가르키는 포인터

배열포인터는 어떠한 배열의 주소를 가르킨다. 
따라서 포인터 연산할때 (자료형 크기 x 배열의 길이) 만큼 주소 byte가 증가하게 된다.

```c
int data = 3;
int* pD = &data;
int** pS = &pD;

int ary[5] = { 1,2,3 };
int* pA = ary;

char* pSt = "hyundai";		//문자열 상수를 가리키는 포인터, 수정 불가능
char* pSa[3];				      //포인터 배열(위에 있는 주소 pSt가 3개 있는 배열) --> 배열의 각 요소는 문자열의 시작 주소이다( 시작 주소 3개를 저장한 배열이 됨)
char** pDs = pSa;		      //포인터 배열의 배열명은 포인터가 저장된 배열의 시작주소인 이중 포인터		--> 시작 주소 3개를 저장한 배열의 시작 주소
pSa[0];						        // 첫번째 string의 시작 주소

char* pOne = pSa[0];		//pSa[0]은  첫번째 string의 시작 주소

int ary1[5] = { 3,4,5 };
int ary2[5] = { 6,7,8 };
int ary3[5] = { 9,10,11 };

//2차원 배열 선언과 동시 초기화
int arydb[3][5] = { {3,4,5},{6,7,8},{9,10,11} }; //={ary1,ary2,ary3} 구조와 같음
printf("arydb : %d \n", sizeof(arydb));			     // 4 x 3 x 5 =(int형)x 행 x 열

//위의 arydb[0], arydb[1] .. 은 각 행의 시작주소이다.
printf("sizeof(arydb[0])=%d\n", sizeof(arydb[0]));		//-> 4 x 5 = int형 x 열 크기

//포인터 자료형 모를때 포인터 연산 해보기
printf("0x%p\n", arydb[0]);
printf("0x%p\n", arydb[0]+1);				   // 한 원소씩 건너뜀 --> 주소 4 증가했으므로 데이터는 int 형 single 포인터이다
int* pTr = arydb[0];

//포인터 연산으로 배열 출력하기
printf("%d\n", *(arydb[0] + 0));			//*(arydb[0]+0)=arydb[0][0] = 3
printf("%d\n", *(arydb[2] + 0));			//*(arydb[2]+0)=arydb[2][0] = 9
printf("%d\n", *(arydb[1] + 0));			//*(arydb[1]+0)=arydb[1][0] = 6

//2차원 배열의 이름도 2차원 배열의 시작주소임

//2차원 배열의 이름 포인터로 알아내기
printf("arydb : 0x%p\n", arydb);
printf("arydb + 1 : 0x%p\n", arydb + 1);		//--> 주소 20 증가함 --> 한 행씩(5열 만큼=20바이트) 건너뜀
//arydb는 주소 20씩 건너뛰는 포인터임

//Display();		// 2차월 배열의 모든 요소를 출력하는 함수

//int* pArdb = arydb;				//pArdb : 포인터 증감 연산시 4씩 건너뛰는 포인터, arydb: 20씩 건너뛰는 포인터
int (*pArdb)[5] = arydb;		//pArdb	: 포인터 증감 연산시 5씩 건너뛰는 포인터(배열 포인터) --> 배열을 가르키는 포인터
//배열 포인터 --> 배열을 가르키는 포인터
//포인터 배열 --> 포인터를 저장한 배열

arydb[0][0] == pArdb[0][0];		
//arydb[0][0] : 직접 접근
//pArdb[0][0] = *(*(pArdb+0)+0): 간접 접근	-> 포인터로 접근

//예제) 다음과 같은 방법으로 2차원배열을 배열포인터에 대입할 수 있다.
char str[5][30];
char (*pS)[30]=str;		//배열 포인터 변수 선언과 동시 초기화
char buf[10][30];
pS = buf;				//건너뛰는 크기 30으로 같으므로 가능한 문법!
```

```c
#include <stdio.h>

void StringDisplay(char (*pD)[50]) {
	//Strbuf[0] == pD[0];
	for (int idx = 0; idx < 3; idx++) {
		printf("%s\n", pD[idx]);
	}
}

void StringCpy(char(*pDest)[50], char**pSrc) {
	int row = 0, col = 0;
	for (row = 0; row < 3; row++) {
		col = 0;
		while (pSrc[row][col] != '\0') {
			pDest[row][col] = pSrc[row][col++];
		}

		pDest[row][col] = '\0';
	}
}

int main() {
	char* pStr[3] = { "hyundai NGV","C programming","Study" };
	char Strbuf[3][50];

	StringCpy(pStr,Strbuf);			//포인터 배열의 문자열을 strbuf 2차원 배열로 복사
	StringDisplay(Strbuf);		//

	return 0;
}
```

## 함수 포인터

```c
#include <stdio.h>

int AddDataFunc(int, int);	//함수 원형
int SubDataFunc(int, int);	//함수 원형
int MulDataFunc(int, int);	//함수 원형

void Management(int (*pF) (int, int)) {
	pF(3,3);
}
int main() {
	
	int val = 5;
	int data = 3;
	int res = 0;;

	//배열명 ==> 배열의 시작주소
	//함수명 ==> 함수의 시작주소
	res = AddDataFunc(val, data);  //직접호출 : 함수명으로 호출
	
	//함수 포인터 선언과 동시에 초기화
	int (*pF) (int,int) = *AddDataFunc;
	res = pF(val, data);		//간접 호출

	printf("res: %d\n", res);

	pF = SubDataFunc;
	res = pF(val, data);
	printf("res: %d\n", res);

	pF = MulDataFunc;
	res = pF(val, data);
	printf("res: %d\n", res);

	Management(pF);

	//함수 포인터 배열
	int(*pF1[3])(int, int) = { AddDataFunc ,SubDataFunc ,MulDataFunc };

	return 0;
}

int AddDataFunc(int Va, int Da) {
	
	return Va + Da;
}

int SubDataFunc(int Va, int Da) {

	return Va - Da;

}

int MulDataFunc(int Va, int Da) {

	return Va * Da;
}
```

[SoEn:소프트웨어 공학 연구소](http://soen.kr/)

VOID형 포인터

```c
int data = 9;

//void형 포인터 - 어떤 변수의 주소값도 저장이 가능한 포인터
void* pD = &data;

//*pD = 12;     불가능한 코드! 자료형 뭔지 몰라서 몇바이트 접근할지 알수없음
*((int*)pD) = 12;		//강제로 인트형 포인터로 형변환 해야한다

printf("%d\n", data);
```

## 동적할당

```c
void* malloc(size_t size);

void* pNew = malloc(20);						//heap 메모리 20byte 할당 요청		
int* pNew2= (int*)malloc(sizeof(int) * 5);		//malloc 반환시에 int 포인터로 형변환 하여 바로 넘겨준다
//typedef ==> 컴파일러가 알고있는 기존 자료형을 새로운 자료형 이름으로 부여해서 사용

//할당 성공시 ==> 20byte heap 영역의 시작주소 반환해줌  ==> void 형 포인터로 받아야 한다.
//할당 실패시 ==> Null 포인터 반환()
int* ptr = NULL;	//널 포인터 ==> 아무것도 가르키지 말라
 
if (pNew == NULL) {		//동적할당 실패에 대한 error handling code
	printf("동적 메모리 할당 실패!\n");
	return -1;		//비정상 종료
}

//동적할당된 공간 접근 --> void 형 포인터는 자료형 모르기 때문에 접근할 포인터 데이터양 알아야 한다
*((int*)pNew) = 50;
*((int*)pNew+1) = 60;
*((int*)pNew+2) = 70;

*(pNew2 + 0) = 50;		//pNew[0]
*(pNew2 + 1) = 90;		//pNew[1]
pNew2[2] = 60;
pNew2[3] = 100;
pNew2[4] = 120;
pNew2[5] = 140;

for (int idx = 0; idx < 5; idx++) {
	printf("%d\n", pNew2[idx]);
}

free(pNew);		//동적 메모리 해제 함수
```

```c
#include <stdio.h>
#include <stdlib.h>
char* InsertCompany(void) {
	//회사 이름 입력 받게 설정
	//char* companyname = (char*)malloc(50);
	static char companyname[50];

//밑에 TestString 함수 있을때도 맞게 실행되려면 
//1. static 사용--> static으로 선언하면 데이터 영역에 저장되어 함수가 종료되도 
//스택 포인트는 내려오지 않고 계속 남아있고 단지 접근 범위를 함수 내에서만 하게 된거

//2. heap memory 사용하여 문자열 --> heap 메모리는 해제하기 전까지 남아있음
//함수 종료되어도 해제하지 않으면 메모리에 남아잉ㅆ다

	if (companyname != NULL) {
	gets_s(companyname, 50);
}

	return companyname;

}

void DisplayCompany(char* pD) {
	printf("%s", pD);
}

void TestString(void) {
	char Buf[50] = "Test String";

	char* companyname2 = (char*)malloc(50);
	companyname2 = "Test String2";
}

int main() {

	char* pSt;
	pSt = InsertCompany();					//회사 이름을 입력, 저장하는 함수
	TestString();
	DisplayCompany(pSt);					//입력한 회사 이름을 출력하는 함수

	return 0;
}
```

## 동적할당 함수 이용

```c
#include <stdio.h>
#include <stdlib.h>

void MyStrcpy(char* pDest, char* pSrc);

char* InsertCompany(void) {
	//회사 이름 입력 받게 설정
	//char* companyname = (char*)malloc(50);
	char companyname[50];
	char* p_companyname=NULL;		
	if (companyname != NULL) {
	gets_s(companyname, 50);
}
	int slen = 0;
	while (companyname[slen] != '\0') {
		slen++;
	}
	
	p_companyname = (char*)malloc(sizeof(char) * (slen+1));		//--> 포인터 변수 p_companyname은 사라지지만, 
	//malloc으로 스택에 할당된 메모리 자체는 해제되지 않는다
	//따라서 p_companyname 포인터 변수를 return 하여 나중에 동적할당 해제할수 있게 한드아!
	//+1한 이유는 문자열 끝에 NULL문자를 넣기 위해서 추가한거
	MyStrcpy(p_companyname, companyname);

	return p_companyname;

}

void DisplayCompany(char* pD) {
	printf("%s", pD);
}

void TestString(void) {
	char Buf[50] = "Test String";

	char* companyname2 = (char*)malloc(50);
	companyname2 = "Test String2";
}

void MyStrcpy(char* pDest, char* pSrc) {
	while (*pSrc != '\0')
		*pDest++ = *pSrc++;
	*pDest = '\0';
}

int main() {

	char* pSt;
	pSt = InsertCompany();					//회사 이름을 입력, 저장하는 함수
	//pSt에는 heap에 동적할당된 데이터의 첫 주소가 저장된다.
	//나중에 pSt로 동적할당 해제하면 됨
	TestString();
	DisplayCompany(pSt);					//입력한 회사 이름을 출력하는 함수
	free(pSt);

	return 0;
}
```

```c
#include <stdio.h>
#include <stdlib.h>

int* InsertData(void);
void DisplayData(int* pD);

int main() {
	
	int* pSt = NULL;
	pSt = InsertData();
	DisplayData(pSt);
	return 0;
}

int* InsertData(void) {
	//몇개의 정수를 입력 받기를 원하세요?
	printf("몇개의 정수 입력 받기 원하는지 쓰시오\n");
	int many = 0;
	scanf_s("%d", &many);
	int* num = NULL;
	num = (int*)malloc(sizeof(int) * (many+1));
	//원래의 메모리 크기보다 4바이트 크게하여 정수의 개수를 int형으로 따로 저장하면 됨
	num[0] = many;
	for (int i = 1; i <= many; i++) {
		printf("%d번째 정수 입력하세요", i);
		scanf_s("%d", num + i);
	}
	//예) 5 입력시 ==> 20byte 동적 메모리 확보 하고 저장
	//예) 7 입력시 ==> 28 byte 동적 메모리 확보하고 저장
	return num;
}

void DisplayData(int* pD) {
	//입력한 정수의 개수만큼 모든 정수 출력
	int cnt = pD[0];
	for (int i = 1; i <= cnt; i++) {
		printf("%d번째 정수: %d\n", i,pD[i]);
	}
	// 동적메모리 해제
}
```

```c
#include <stdio.h>
#include <stdlib.h>

int* InsertData(void);
void DisplayData(int* pD);

//return 갯수 부족하면, 포인터 이용해서 전달 가능하다!!!
//포인터를 전달할때는 이중 포인터 이용해서 전달할 수 있음

int main() {
	
	int* pSt = NULL;
	pSt = InsertData();
	DisplayData(pSt);

	free(pSt);

	return 0;
}

int* InsertData(void) {
	//몇개의 정수를 입력 받기를 원하세요?
	printf("몇개의 정수 입력 받기 원하는지 쓰시오\n");
	int many = 0;
	scanf_s("%d", &many);
	int* num = NULL;
	num = (int*)malloc(sizeof(int) * (many+1));
	//원래의 메모리 크기보다 4바이트 크게하여 정수의 개수를 int형으로 따로 저장하면 됨
	num[0] = many;
	for (int i = 1; i <= many; i++) {
		printf("%d번째 정수 입력하세요", i);
		scanf_s("%d", num + i);
	}
	//예) 5 입력시 ==> 20byte 동적 메모리 확보 하고 저장
	//예) 7 입력시 ==> 28 byte 동적 메모리 확보하고 저장
	return num;
}

void DisplayData(int* pD) {
	//입력한 정수의 개수만큼 모든 정수 출력
	int cnt = pD[0];
	for (int i = 1; i <= cnt; i++) {
		printf("%d번째 정수: %d\n", i,pD[i]);
	}
	// 동적메모리 해제
}
```

완전 실무 엔지니어 처럼 수업해주심

진짜 빡세다

포인터 거의 정복한것 같음

시험은 쉽게 나오는데

## 구조체

- 배열 단점
모든 자료형이 동일해야 하는 점
→자료형이 다른 많은 양의 데이터를 효율적으로 관리하기 위해 구조체 자료형 만든다
- structure--> struct
- 자료형 → 사용자 직접 정의한 자료형(사용자 정의 자료형)
- 컴파일러는 위에서 아래로 내려온다 & 함수에서 구조체 쓰일수 있다. 
→ 구조체 코드에서 맨 위에 있어야 한다.