# C++ 3일차

# 객체지향 프로그래밍

<aside>
🏛️ class

</aside>

## class 생성

```cpp
#include <iostream>

using namespace std;

class POCKETMON			//class 이름: 대문자
	//class 안에는 변수와 함수
	//class 밖에 함수 정의를 한다
	//-> 약속, 캡슐화를 위한 것
	//설계도 제작
{
private:
	int hp;		//멤버변수, 필드

public:
	void setHP(int h);	//멤버함수, 메서드
	int getHP();

};

void POCKETMON::setHP(int h) {
	hp = h;
}

int POCKETMON::getHP() {
	cout << hp << endl;
	return hp;
}

int main() {

	POCKETMON pika;		//POCKETMON
	pika.setHP(100);
	pika.getHP();

	return 0;
}
```

- class 이름 → 대문자로 정의
- 클래스 : 설계도, 객체 : 설계도로 만들어진 모든 대상, 사용자 정의 타입
- 인스턴스 : 객체에 개인적인 속성을 부여함

### access  modifier(접근 제한자)→클래스 외부에서 멤버 접근 에 따라 구분

private : C++ default(외부에서 멤버변수 접근 불가능 하다)

public : 외부에서 멤버변수 접근 가능

protected : ??

왜 굳이 private 사용하는지? → OOP 설계 방식(캡슐화)

멤버 변수는 private로 정의

public 멤버 함수로 변수에 접근하게 설계한다

## 배포

class 생성: 헤더파일, 그냥 베포

class의 멤버함수 정의 : .cpp파일, 컴파일 후에 베포한다

### 객체를 만드는 2가지 방법

```cpp
//1. Stack에 만듬
POCKETMON pika;		//POCKETMON
pika.setHP(100);
pika.getHP();

//2. HEAP에 만듬
POCKETMON* GUGU = new POCKETMON();
GUGU->setHP(80);
GUGU->getHP();
delete GUGU;
```

스택과 힙의 차이점

1. stack에 만들면, return 0 하면 메모리에서 삭제됨
2. heap에 만들면, delete 하기 전까지 heap 메모리에서 삭제되지 않는다
따라서 꼭 delete 해줘야함!!!(메모리 누수 발생 방지)

생성자/소멸자

생성자 : 객체를 생성할때 세팅을 하는데 도움을 주는 함수

복사생성자 : 

소멸자: 객체가 사라질때 도움을 주는 함수(public 필수)

```cpp
#include <iostream>

using namespace std;

class CIRCLE {
private:
	int radius;

public:

	//생성자
	CIRCLE();
	CIRCLE(int m_radius);
	CIRCLE(const CIRCLE& c1);

	//소멸자
	~CIRCLE();

	void getRadius(int radius);
	int print();
};

void CIRCLE::getRadius(int m_radius) {
	radius = m_radius;
	return;
}

int CIRCLE::print() {
	cout << radius << endl;
	cout << "둥글다" << endl;
	return radius;
}

CIRCLE::CIRCLE(const CIRCLE& c1)
: radius(c1.radius)
{
	cout << "응애 나복사원" << endl;
}

CIRCLE::CIRCLE(int m_radius) 
	:radius(m_radius)
{
}

CIRCLE::CIRCLE()
	:radius(10) {
	
}

CIRCLE::~CIRCLE() {
	cout << "소멸자" << endl;
}

int main() {
	CIRCLE *c1=new CIRCLE();		//기본 생성자 객체 생성
	c1->print();

	CIRCLE* c2 = new CIRCLE(100);	//매개변수 있는 생성자

	c2->print();

	CIRCLE* c3 = new CIRCLE(*c2);	//복사 생성자

	c3->print();

	delete c1;
	delete c2;
	delete c3;

	return 0;
}

```

this

보통 객체 자기 자신을 return 할때 사용됨

```cpp
#include <iostream>
using namespace std;

class Myclass {
private:
	int id;
public:
	Myclass(int n);
	~Myclass();

};

Myclass::Myclass(int n) {
	id = n;
	cout << "응애 나 아기 객체" << this->id << endl;

}

Myclass::~Myclass() {
	cout << "bye-bye " << this->id << endl;
}

int main() {

	Myclass* o1 = new Myclass(100);
	Myclass o2(200);

	return 0;
}

출력값 
응애 나 아기 객체100
응애 나 아기 객체200
bye-bye 200
```

Smart Pointer

동적할당 하고 자동으로 함수 범위가 끝나면 delete 해줌

```cpp
#include <iostream>

using namespace std;

int main() {

	//int형 데이터(객체)를 생성하고, 값을 10으로 초기화
	unique_ptr <int> a(new int(10));

	cout << *a << endl;

	return 0;
}

출력결과 
10
```

class 정의한후, 스마트포인터로 객체 생성

```cpp
#include <iostream>

using namespace std;

class Myclass {
private:
	int value = 10;
public:
	Myclass(int v);
	~Myclass();
	void print();

};

Myclass::Myclass(int v)
	:value(v) {
	cout << "객체 생성!" << endl;
}

Myclass::~Myclass() {
	cout << "객체 사라짐!" << endl;
}

void Myclass::print() {
	cout << value << endl;
}

int main() {

	//스마트 포인터로 메모리 해제 가능
	auto obj1 = make_unique<Myclass>(100);
	obj1->print();
	return 0;
}

**출력결과**
객체 생성!
100
객체 사라짐!
```

스마트포인터를 사용하면 객체는 자동으로 동적할당이 해제된다

## Static 멤버변수

```cpp
#include <iostream>

using namespace std;

class Myclass {
private:
	static int value;
	int power = 10;
public:
	static void powerUP();
	void print();
};

void Myclass::print() {
	cout << value << endl;
}

//static
int Myclass::value = 100;

void Myclass::powerUP() {
	value++;
}

int main() {

	Myclass* o1 = new Myclass();
	Myclass* o2 = new Myclass();

	o1->powerUP();
	o1->print();
	o2->print();

}

**출력결과**
101
101
```

1. class 선언 외부에서 초기화 해야함
2. 변수를 공유하므로, 다른 instance에서 접근해도 그 값은 같다

## const

1. 멤버변수 뒤에 —> 값이 변하지 않음
2. 멤버함수 뒤에 → 함수 안에서 변수 값을 변경할수 없다
3. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ccbbb0a8-a58f-4760-a38e-efb5f0b6cfb0/Untitled.png)

# 상속

```cpp
#include <iostream>
using namespace std;

class POKEMON {
private:
	int hp = 100;
public:
	void attack() {
		cout << "공격" << endl;
	}
};

class PIKACHU : public POKEMON {};

int main() {
	PIKACHU* p = new PIKACHU();
	p->attack();
	return 0;
}

**출력**
공격
```

## 위임(delegation)

객체 내부변수에 다른 객체를 생성하고, 그 다른 객체의 기능(함수)들을 원래 객체에서 이용할 경우에 위임이라고 한다.

```cpp
#include<iostream>
using namespace std;

class POLYGON {
private:
	int point;
public:
	void setPoint(int p);
	void getPoint();
};

void POLYGON::setPoint(int p) {
	point = p;
}

void POLYGON::getPoint() {
	cout << point << endl;
}

**class RECTANGLE : public POLYGON** {
private:
	int height;
	int weight;

public:
	void setLength(int h, int w);
	void getArea();
};

void RECTANGLE::setLength(int h, int w) {
	height = h;
	weight = w;
}

void RECTANGLE::getArea() {
	cout << height * weight << endl;
}

int main() {
	RECTANGLE* r1 = new RECTANGLE();
	r1->setPoint(4);
	r1->getPoint();
	r1->setLength(10, 4);
	r1->getArea();
	return 0;
}
```

**생성사,소멸자, 할당연산자는 상속되지 않음→**
서브클래스가 슈퍼클래스보다 확장되어 더 많은 멤버를 갖기 때문에 불필요

오버로드 vs 오버라이드

오버로드 : 함수 매개변수를 다르게 하여 입력 매개변수에 따라 함수 작동을 다르게 함

오버라이드 : 상속하여 메소드 함수를 재정의함

## 함수 오버라이드

함수 이름도 같고, input 자료형도 같음(주로 상속일때 발생),
오버라이딩(Overriding, 재정의)는 부모 클래스와 자식 클래스의 상속 관계에서, 부모 클래스에 이미 정의된 함수를 같은 이름으로 자식 클래스에서 재정의 하는것을 의미합니다