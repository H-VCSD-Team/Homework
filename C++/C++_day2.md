# C++ 2일차

[https://isocpp.org/blog/2022/06/results-summary-2022-annual-cpp-developer-survey-lite](https://isocpp.org/blog/2022/06/results-summary-2022-annual-cpp-developer-survey-lite)

C++ 버전 중에서 C++ 14 버전 가장 많이 사용함

### namespace

같은 이름의 함수여도 여러 군데에 붙을수 있게 해둠

```cpp
#include <iostream>

using namespace std;

namespace BBQ {
	void GO() {
		cout << "BBQ" << endl;
	}
}

namespace KFC {
	void GO() {
		cout << "KFC" << endl;
	}
}

void GO() {
	cout << "HOME" << endl;
}

int main() {

	BBQ::GO();
	KFC::GO();
	GO();

	return 0;
}

출력
BBQ
KFC
HOME
```

input함수에서 숫자 1개를 입력받아주세요

그리고 process함수에서 그 숫자로 부터 1씩 더한 값들을 배열에 채워주세요

그리고 output함수에서 출력 해 주세요

ex) 만약 5를 입력받았다면

![https://pro.mincoding.co.kr/public/upload/34efa95178.png](https://pro.mincoding.co.kr/public/upload/34efa95178.png)

```cpp
#include <iostream>

using namespace std;

void input(int *m_input) {
	cin >> *m_input;
	return;
}

void process(int m_input, int(* m_arr)[3]) {
	for (int i = 0; i < 9; i++) {
		*(m_arr[0] + i) = i + m_input;
	}
}

void output(int(* m_arr)[3]) {
	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++)
			cout << m_arr[i][j] << " ";
		cout << endl;
	}
}

int main() {

	int input_1;
	int m_arr[3][3] = {};
	input(&input_1);
	process(input_1, m_arr);
	output(m_arr);

	

	return 0;
}
```

Q1

```cpp
#include<iostream>
#include<string>
using namespace std;
int main() {
	string a;
	getline(cin, a);
	
	string::size_type npos = -1;
	cout << a << ' ' << a.size() << endl;
	int wStart, wEnd;
	wStart = a.find_first_not_of(" \n", 0); 
// ‘공백‘과 ‘\n’ 이 아닌 문자를 찾는다. ( 개행 )
	while (wStart < npos) {
		wEnd = a.find_first_of(" \n", wStart); 
//현재 단어의 끝으로 설정
		string subsub = a.substr(wStart, wEnd - wStart); 
//사이 문자열 추출
		cout << subsub << ' ' << subsub.size() << endl;
		wStart = a.find_first_not_of(" \n", wEnd); 
//wStart를 다음 단어의 시작으로 설정
	}
	return 0;
}
```

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {

	string str1;

	getline(cin, str1);

	int size = str1.size();
	//int ttcount = str1.find();

	cout << str1 << " " << str1.size() << endl;

	int previous = 0, current = 0;
	current = str1.find(' ');
	string::size_type npos = -1;
	while (current != npos) {
		string substring = str1.substr(previous, current - previous);
		cout << substring << " " <<current-previous<< endl;
		previous = current + 1;
		//previous 부터 ,이 나오는 위치를 찾는다.
		current = str1.find(' ', previous);
	}
	//마지막 문자열 출력
	cout << str1.substr(previous, current - previous)<<
		" "<< size-previous<<endl;

	return 0;
}
```

## reference

참조

reference는 한번 생성되면 다른 객체를 참조할수 없다

값으로 전달할때보다 복사 비용을 줄일 수 있다

```cpp
#include <iostream>
using namespace std;
// 스마트 포인터 클래스

int main() {
    int a = 10;
    int b = 20;
    int& c = a;
    c = 5;           //a값도 5로 변경된다                      
    cout << a<<" "<<b<<" "<<c << endl;     
    c = b;           **//b를 넣어지만 c는 계속 a를 참조하고 있다**
    c = 30;          //b값은 30으로 변경되지 않는다
    cout << a << " " << b << " " << c << endl;
    

    return 0;
}

출력
5 20 5
30 20 30
```

### auto, typeid