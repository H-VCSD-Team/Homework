# C++ 5일차

## C++지향하는것

**원하는 type 만들어서 기존의 type(자료형) 처럼 써라!!!**

**금지된 사항 : C++ 표준은 수정 불가**

## 연산자 오버로딩

→ 사용자 정의 타입을 만들고 계산하고 출력하자

객체의 복사: 얕은복사 깊은복사 일어난다

## 예외처리

STL

데이터를 저장하는 통과 접근/출력하는 방법

## 대입연산자

`a = b`는 `a.operator=(b)`로 간주할수 있다.

```cpp
#include <iostream>
using namespace std;
class MyInt {
private:
    int num;
public:
    MyInt(int num) : num(num) {}
    MyInt& operator=(const MyInt& other) {
        num = other.num;
        return *this;     //-> 자기 대입!!!
    }
    int getNum() const { return num; }
};
int main() {
    MyInt a(5);
    MyInt b(10);
    cout << "a: " << a.getNum() << endl;
    cout << "b: " << b.getNum() << endl;
    b = a;
    cout << "a: " << a.getNum() << endl;
    cout << "b: " << b.getNum() << endl;
    return 0;
}
```

또한, 연산자 반환은 참조로 하는 것을 확인할 수 있다. 이는 다음과 같이 `operator chaining`을 허용하기 위함이다.

```cpp
int a, b, c, d, e;
a = b = c = d = e = 42;
```

```cpp
a = (b = (c = (d = (e = 42))));
```

다르게 말해서, 대입(할당)은 우측결합이다. 마지막 대입(오른쪽) 연산을 먼저 계산하고, 좌측으로 연산하며 값이 전달되는 방식이다.

## 연산자 정의

## 멤버함수로 정의

```cpp
#include<iostream>
using namespace std;
class Complex {
private:
    double real;
    double imag;
public:

    Complex(double r, double i) : real(r), imag(i) {}

    Complex operator+(const Complex& rhs) const { //멤버함수로 구현
        return Complex(real + rhs.real, imag + rhs.imag);
    }   //return 결과가 Complex의 객체

    void print() {
        cout << real << ' ' << imag << endl;
    }

};
int main() {
    Complex a(1.0, 2.0);
    Complex b(3.0, 4.0);
    Complex c = a + b;
    c.print();
    return 0;
}
```

## 비멤버함수로 정의

```cpp
#include<iostream>
using namespace std;
class Complex {
private:
    double real;
    double imag;
public:
    Complex(double r, double i) : real(r), imag(i) {}
    double getReal() const {
        return real;
    }

    double getImag() const {
        return imag;
    }

    friend Complex operator+(const Complex& lhs, const Complex& rhs);
};

Complex operator+(const Complex& lhs, const Complex& rhs) { //비멤버함수로 재정의
    return Complex(lhs.getReal() + rhs.getReal(), lhs.getImag() + rhs.getImag());
}

int main() {
    Complex a(1.0, 2.0);
    Complex b(3.0, 4.0);
    Complex c = a + b;
    cout << c.getReal() << ' ' << c.getImag() << endl;
    return 0;
}
```

## C++ 교재 추천

[싸니까 믿으니까 인터파크도서](https://book.interpark.com/product/BookDisplay.do?_method=detail&sc.saNo=001&sc.prdNo=342941581&product2020=true)

## →, * 연산자 오버로딩

[[문과 코린이의 IT 기록장] C,C++ - 연산자 오버로딩 7 : 포인터 연산자 오버로딩(포인터 연산자 오버로딩, 스마트 포인터 (Smart Pointer), ( )연산자의 오버로딩과 펑터(Functor), 임시객체로의 자동 형 ..](https://vansoft1215.tistory.com/42)

## ※ Smart Pointer 구현

객체를 heap 에 생성했을때

 객체를 삭제하는게 가장 중요!!!  → 객체의 포인터를 멤버변수로 가지고 있어서 heap 메모리가 자동으로 삭제되는 SmartPointer를 이용하자!!!!!!

```cpp
#include <iostream>
using namespace std;
// 분수를 나타내는 클래스
class Fraction {
private:
    int number;
    int denom;
public:
    Fraction(int n, int d) : number(n), denom(d) {} // 생성자
    void print() { // 분수를 출력하는 함수
        cout << number << '/' << denom << endl;
    }
};

// 스마트 포인터 클래스
class SmartPtr {
private:
    Fraction* ptr; // 포인터 멤버 변수
public:
    SmartPtr(Fraction* p) : ptr(p) { } // 생성자
    ~SmartPtr() { // 소멸자: 동적 할당한 객체 메모리를 해제한다
        delete ptr;
    }
    Fraction& operator*() const { // * 연산자 오버로딩: 참조 반환
        return *ptr;
    }
    Fraction* operator->() const { // -> 연산자 오버로딩: 포인터 반환
        return ptr;
    }
};
int main() {
    SmartPtr sp = new Fraction(2, 5); // 스마트 포인터 객체 생성
    (*sp).print(); // * 연산자를 이용한 접근
    sp->print(); // -> 연산자를 이용한 접근
    //sp->print()=(sp.operator->()) ShowData();
    return 0;
}
```

**num->ShowData() = (num.operator->()) ShowData();**

## Smart pointer & 참조 계수

```cpp
#include <iostream>
using namespace std;
// 스마트 포인터 클래스
class SmartPointer {
private:
    int* ptr;         // 관리하는 포인터
    int* count;       // 참조 계수
public:
    // 생성자
    SmartPointer(int* p = NULL) {
        ptr = p;
        count = new int(1);   // 초기 참조 계수 1로 설정
    }
    // 복사 생성자
    SmartPointer(const SmartPointer& sp) {
        ptr = sp.ptr;
        count = sp.count;
        (*count)++;           // 참조 계수 증가
    }
    // 대입 연산자
    SmartPointer& operator=(const SmartPointer& sp) {
        if (this != &sp) {         // 자기 자신에 대한 대입 방지
            if (--(*count) == 0) { // 참조 계수 감소
                delete ptr;        // 메모리 삭제
                delete count;
            }
            ptr = sp.ptr;
            count = sp.count;
            (*count)++;
        }
        return *this;
    }
    // 소멸자
    ~SmartPointer() {
        if (--(*count) == 0) {   // 참조 계수 감소
            delete ptr;          // 메모리 삭제
            delete count;
        }
    }
    // 참조 계수 반환
    int GetCount() {
        return *count;
    }
    // 포인터 반환
    int* GetPointer() {
        return ptr;
    }
};
int main() {
    int* p = new int(10);
    SmartPointer sp1(p);
    cout << sp1.GetCount() << endl; // 1
    SmartPointer sp2 = sp1;
    cout << sp1.GetCount() << endl; //2
    return 0;
}
```

## 함수객체(functor)

() 괄호 연산자를 operator overloading 하여 

함수와 다르게 상태를 계속해서 유지 가능

```cpp
#include <iostream>
using namespace std;
class Adder {
private:
    int sum;
public:
    Adder(int init = 0) : sum(init) {} // 생성자
    void set(int n) { sum = n; } // 상태 값 변경 함수
    int operator()(int n) { return sum += n; } // 함수 호출 연산자
};
int main() {
    Adder add(10); // 초기값을 10으로 설정하는 함수 객체 생성
    cout << add(5) << endl; // 출력 결과: 15
    cout << add(3) << endl; // 출력 결과: 18
    add.set(0); // 상태 값을 0으로 변경
    cout << add(7) << endl; // 출력 결과: 7
    return 0;
}
```

상태가 계속 유지된채로(sum 값이 남아있고) sum 괄호에있는 값이 더해진다

## << 오버로딩

```cpp
#include <iostream>
using namespace std;

class Point {
private:
    int x;
    int y;
public:
    Point(int x, int y) : x(x), y(y) {}
    int getX() const { return x; }
    int getY() const { return y; }
    //friend 사용 안해도 getX,getY함수 이용해서 출력 가능
    //friend ostream& operator<<(ostream& os, const Point& p);
};

ostream& operator<<(ostream& os, const Point& p) {
    os << "(" << p.getX() << ", " << p.getY() << ")";
    //friend 사용할때 밑에 방법으로도 가능
    //os << "(" << p.x << ", " << p.getY() << ")";         

    return os;
}

int main() {
    Point p{ 1, 2 };     //-> uniform 초기화
    cout << p << endl; // (1, 2)
    return 0;
}
```

## 복사

1. 얕은복사 : 객체의 주소만 그대로 복사

```cpp
#include<iostream>
using namespace std;

class Person {
private:
	int* age;
public:
	Person(int n) {
		age = new int;
		*age = n;
	}
	~Person() {	}
	void setAge(int n) {
		age = new int;
		*age = n;
	}
	void getAge() {
		cout << *age << endl;
	}
};

int main() {
	Person* a = new Person(10);		//복사생성자 호출
	a->getAge();					//10 출력

	Person* b;
	b = a;
	b->getAge();				//10출력
	b->setAge(100);
	a->getAge();	//a 객체의 멤버 변수 값도 영향을 받음
	b->getAge();
	//문제점 -> 복사를 했는데 메모리가 공유되고 있음!!!
	return 0;
}
```

얕은복사 문제점: **멤버변수가 동적할당된 포인터일때 포인터 주소 자체가 복사되는 현상 발생**

-> 복사를 했는데 heap 메모리가 공유되고 있음!!!

1. 깊은복사 : 실제 객체를 새로 생성하여 복사( 새로운 메모리 공간을 가진다)

```cpp
#include <iostream>
using namespace std;
class Person {
private:
    int* age;
public:
    Person(int n) {
        age = new int;
        *age = n;
    }
    // 깊은 복사 생성자
    Person(const Person& other) {
        age = new int;
        *age = *other.age;
    }
    // 깊은 복사가 일어나도록 대입 연산자 오버로딩(C++ 에서 기본적으로 제공함)
    Person& operator=(const Person& other) {
        if (this == &other) return *this;  // 자기 자신과의 대입 처리
        delete age;
        age = new int;
        *age = *other.age;
        return *this;
    }
    ~Person() {
        delete age;     //메모리 누수 방지
    }
    void setAge(int n) {
        *age = n;
    }
    void getAge() {
        cout << *age << endl;
    }
};
int main() {
    Person* a = new Person(10);
    a->getAge();
    Person* b = new Person(*a);  // 깊은 복사 생성자 호출
    b->getAge();        //10
    b->setAge(100);     //100
    a->getAge();        //10  // a 객체의 멤버 변수 값은 변경되지 않음
    b->getAge();        //100
    *a = *b;            // 깊은 복사 대입 연산자 호출
    a->getAge();        //100
    b->getAge();        //100
    delete a;
    delete b;
    return 0;
}
```

## 참조 계수

객체를 함수에 넣거나 return 하면 객체 복사 되더라→ 메모리 공유됨

참조계수 메모리를 공유→ 이 메모리를 따간 객체가 몇개인지 센다

→ 메모리 누수 방지!!!

메모리 누수 방지를 위해 smart pointer와 참조계수가 같이 사용된다

heap에 생성된 객체가 복사될 때마다 카운팅

```cpp
#include <iostream>
using namespace std;

class RefCountedObj {
private:
    int refCount;  // 참조 계수
public:
    RefCountedObj() : refCount(0) { cout << "Object created" << endl; }
    ~RefCountedObj() { cout << "Object destroyed" << endl; }
    void retain() { refCount++; }
    void release() {
        refCount--;
        if (refCount == 0) {
            delete this;
        }
    }
};

int main() {
    RefCountedObj* obj = new RefCountedObj();
    obj->retain();
    obj->retain();
    obj->release();
    obj->release();
    return 0;
}
```

## try catch

```cpp
#include<iostream>
using namespace std;

int quotient(int n1, int n2) {
	if (n2 == 0) throw 0;
	else if (n2 == 1) throw 1;
	else if (n2 == 2) throw 'a';
	return n1 / n2;
}

int main() {
	try {
		cout << quotient(10, 2) << endl;
	}
	catch (...) {		//throw 했을때 모든 자료형 받을 수 있다.
		cout << "HAHA" << endl;
	}
	/*catch (int x) {
		if( x==0 ) cout << "0으로 나눌 수 없다." << endl;
		if( x==1 ) cout << x << "은 의미가 없어요." << endl;
	}
	catch (char y) {
		cout << "hoho" << endl;
	}*/
	return 0;
}
```

‘

## 예외처리될대 stack 해제 되지 않을때

class에서 예외 발생 처리

- 생성자에서 예외처리 발생할때
메모리 정리되도록 함
- 소멸자에서 예외처리 발생할때

```cpp
#include<iostream>
#include<memory>
using namespace std;

class Myclass {
private:
	int* ptr;
public:
	Myclass() : ptr(new int) {
		cout << "응애" << endl;
	}
	~Myclass() {
		delete ptr;
		cout << "삭-제" << endl;
	}
	void setVal(int val) { *ptr = val; }
	int getVal() { return *ptr; }
};

int main() {
	unique_ptr<Myclass> ptr(new Myclass());
	ptr->setVal(43);
	cout << ptr->getVal() << endl;
}
```

smart pointer→ return 0 만나면 동적할당 해제된다

## class try catch

```cpp
#include <iostream>
void handleException() {
    try {
        // 예외 처리를 위한 코드
    }
    catch (std::exception const& ex) {
        std::cerr << "Caught exception: " << ex.what() << std::endl;
    }
    catch (...) {
        std::cerr << "Caught unknown exception" << std::endl;
    }
}
class MyClass {
public:
    MyClass() {
        // 객체 초기화
    }
    ~MyClass() noexcept {
        try {
            handleException(); // 예외 처리 함수 호출
        }
        catch (...) {
            std::terminate(); // 예외가 발생한 경우 강제 종료
        }
    }
    // 다른 멤버 함수 등
};
int main() {
    MyClass myObj;
    // 객체 사용 등
    return 0;
}
```

# STL

## std::vector

vector의 크기가 바뀌면 새롭게 메모리가 할당된다

push_back() VS insert() 차이점

push_back : 중간에 요소 삽입해도 비용높지않다

insert : 중간에 요소 삽입할 경우 비용이 높다

iterator

컨테이너에 있는 원소들에 접근할때 사용함

포인터처럼 사용함

```cpp

#include <iostream>
#include <vector>

using namespace std;

int main() {

	vector<int> a;

	for (int i = 0; i < 4; i++) {
		a.push_back(i);
	}

	//for (vector<int>::iterator it=a.begin(); it != a.end(); it++) {		//a.begin(), a.end()도 포인터다
	for (auto it = a.begin(); it != a.end(); it++) {		//a.begin(), a.end()도 포인터다
		cout << *it << ' ';								    //포인터처럼 하기 때문에 별로 씀 
	}

	return 0;
}
```

## std::deque

vector와 유사하지만, 앞에도 추가 가능함!

```cpp
#include <iostream>
#include <deque>

using namespace std;

int main() {

	deque<int> dq = { 10,20,30 };
	dq.push_front(1);

	for (auto i : dq) {
		cout << i << ' ';
	}

	return 0;
}
```

## std::list

```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {

	list<int> li= {10,4,5,6 };
	
	cout << *(li.begin()) << endl;

	for (auto i : li) {
		cout << i << ' ';
	}

	return 0;

}
```

장점: 어떠한 위치에도 데이터 삽입/삭제 가능
메모리 상에 연속적으로 배치되지 않음

단점:

## std::set

**중복 허용하지 않는** 원소들의 집합, key만 저장

```cpp
#include<iostream>
#include <set>
using namespace std;
int main() {
	set<int> s;
	s.insert(1);
	s.insert(88);
	s.insert(10);
	s.insert(99);
	for (auto it = s.begin(); it != s.end(); ++it) {
		cout << *it << ' ';
	}
	return 0;
}
```

## std::map

key-value 쌍을 저장하는 자료 구조(**중복 허용 x**)

```cpp
#include<iostream>
#include <map>
using namespace std;

int main() {

	map<int, char> m;
	m.insert({ 3,'A' });
	m.insert({ 3,'B' });		//중복 무시( 이번 줄 무시된다 )
	
	m.insert({ 7,'D' });
	m[7] = 'C';					//덮어 씌우기

	for (auto it = m.begin(); it != m.end(); ++it) {
		cout << (*it).first << ' ' << (*it).second << endl;
	}

	return 0;
}

출력
3 A
7 C
```

## std::stack

LIFO( Last Input First Output ) → 나중에 들어간 것이 먼저 나온다.

```cpp
#include<iostream>
#include <stack>

using namespace std;

int main() {

	stack<int> s;

	for (int i = 0; i < 5; i++) {
		s.push(i);
	}

	while (!s.empty()) {
		cout << s.top() << endl;
		s.pop();
	}

	return 0;
}

**출력**
4
3
2
1
0
```

## std::queue

```cpp
#include<iostream>
#include <queue>
using namespace std;

int main() {
	queue<int> q;

	for (int i = 0; i < 5; i++) {
		q.push(i);
	}

	while (!q.empty()) {
		cout << q.front() << endl;
		q.pop();
	}

	return 0;
}
```

## std::priority queue

```cpp
#include<iostream>
#include <queue>
using namespace std;
int main() {

	priority_queue<int> pq;
	
	pq.push(1);
	pq.push(10);
	pq.push(56);
	pq.push(2);
	pq.push(4);

	//top에 항상 최대값이 들어있다
	while (!pq.empty()) {
		cout << pq.top() << endl;
		pq.pop();
	}
	return 0;
}

출력
56
10
4
2
1
```

우선 순위에 따라 자동으로 정렬됨

완전 이진트리 구조( 이분법 ) → 공부하기

## std::remove

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

int main() {

	vector<int> a = { 10,4,3,25,1 };

	cout << a.size() << endl;

	remove(a.begin(), a.end(), 25);
	//원소 25만 삭제되고 나머지는 자리가 당겨진다

	for (auto i : a) {
		cout << i << ' ';
	}

	cout << endl << a.size() << endl;

	return 0;
}

**출력**
5
10 4 3 1 1
5
```

## std::sort

```cpp
#include<iostream>
#include<algorithm>
#include <functional>
#include<vector>
using namespace std;

int main() {

	vector<int> a = { 10,20,40,30 };
	vector<int> b = { 1,3,2,6 };
	swap(a, b);
	
	for (auto i : a)
		cout << i << " ";
	cout << endl;

	sort(b.begin(), b.end());		//sort를 오름차순으로 변경

	cout << "\nsort 오름차순" << endl;
	for (auto i : b)
		cout << i << " ";
	cout << endl;

	sort(b.begin(), b.end(), greater<int>());		//sort를 내림차순으로 변경(<functional>)

	cout << "\nsort 내림차순" << endl;
	for (auto i : b)
		cout << i << " ";
	cout << endl;

	return 0;
}

출력
1 3 2 6

sort 오름차순
10 20 30 40

sort 내림차순
40 30 20 10
```

## std::count, find

```cpp
#include<iostream>
#include<algorithm>
#include <functional>
#include <vector>
using namespace std;

int main() {

	vector<int> a = { 1,2,3 };
	vector<int> b = { 4,3,2,2,2,2 };
	
	cout << "원소 개수 찾기" << endl;
	cout << count(b.begin(), b.end(), 2) << endl;		

	cout << "원소 찾기" << endl;
	cout << *find(b.begin(), b.end(), 2) << endl;

	return 0;
}

출력
원소 개수 찾기
4
원소 찾기
2
```

## lower_bound, upper_bound, binary_search