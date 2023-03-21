## OOP의 끝판왕은 “**다형성**”

**class 사이의 관계**

- 연관 association -”has a 관계”
- 집합 aggregation -”has a 관계”
- 구성 composition -”has a 관계”
- 의존 dependency -”use a 관계”

class간의 의존성을 낮춰서 유지 보수를 쉽게 하자

4가지 관계를 서로 명확한 구분을 하기는 어려움

composition/aggregation/association → has a 관계

dependency → use a 관계

## Association 연관

**의미**

서로 소유의 개념이 아님

person과 coffee는 cafe class를 통해 연결되어 있다

밑에 코드에서 커피를 시키는 사람이 어떤걸 마시는지 연결되어 있음

```cpp
#include <string>
#include <vector>
#include <iostream>
using namespace std;

class Person {
private:
    string name;
public:
    Person(string name) : name(name) {}     //이름을 정할 수 있는 생성자
    string getName() const { return name; } //당신의 이름은?
};

class Coffee {
private:
    string type;
public:
    Coffee(string type) : type(type) {}        //마찬가지로 coffee type 정하는 생성자
    string getType() const { return type; }    //coffee 타입은?
};

class Cafe {                                    //Person이 coffee 를 주문하는 cafe class
private:
    vector<pair<Person*, Coffee*>> orders;      //vector 에 person 객체와 coffee 객체를 pair 형태로 보관
public:
    void takeOrder(Person* person, Coffee* coffee) {    //주문할게요~
        orders.push_back(make_pair(person, coffee));    //주문을 order vector에 넣는다.
    }
    void printOrders() const {                          //주문 출력, 매출을 확인한다.
        for (auto order : orders) { 
            cout << order.first->getName() << " ordered a " << order.second->getType() << " coffee" << endl;
        }
    }
};

int main() {
    Person* person1 = new Person("FAKER");
    Person* person2 = new Person("JIWOO");
    Coffee* coffee1 = new Coffee("Latte");
    Coffee* coffee2 = new Coffee("Americano");
    Cafe* cafe = new Cafe();
    cafe->takeOrder(person1, coffee1);
    cafe->takeOrder(person2, coffee2);
    cafe->printOrders();
    delete person1;
    delete person2;
    delete coffee1;
    delete coffee2;
    delete cafe;
    return 0;
}

**출력**
FAKER ordered a Latte coffee
JIWOO ordered a Americano coffee
```

## Aggregation 소유(집합)

**의미**

한 객체가 다른 객체를 소유하지만, 서로 독립적임

학교는 학생을 가지고 있지만, 학생이 없어져도 학교가 없어지지는 않는다(서로 독립적)

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Student {                     //school class 에 속하는 student class
private:
    string name;                    //이름 
public:
    Student(string n) : name(n) {}  //이름 정하는 생성자
    string print() {                //이름 출력하는 함수
        return name;
    }
};

class School {                      //student를 추가하고, student의 이름을 출력하는 class
private:
    int num_students;               //학생수
    vector<Student*> students;      //student class 를 보관하는 vector class
public:
    School(int num_students) 
       : num_students(num_students) {}    //학생 수 정하는 생성자
    void add_student(Student* s) {  //학생 추가하는 함수
        students.push_back(s);
    }
    void print_student() {          //학생 이름 출력하는 함수
        for (int i = 0; i < num_students; i++) {
            cout << students[i]->print() << endl;
        }
    }
};

int main() {
    Student* student1 = new Student("John");
    Student* student2 = new Student("Jane");
    Student* student3 = new Student("Bob");
    School* school = new School(3);
    school->add_student(student1);
    school->add_student(student2);
    school->add_student(student3);
    school->print_student();
    return 0;
}
```

## Composition 구성

**의미**

자동차와 엔진의 관계처럼 내용물 클래스가 컨테이너 클래스에 포함되어 있다 컨테이너 클래스가 소멸되면 내용물 클래스도 소멸된다

Car class에 Engine class가 포함되어 있음

```cpp
#include <iostream>
#include <string>
using namespace std;

class Engine {                  //타입을 정할 수 있는 engine class
private:
    string type;                //타입
public:
    Engine(string t) : type(t) {}   //타입 정하는 생성자
    void start() {              //타입 출력
        cout << "I'm " << type << " engine." << endl; 
    }
};

class Car {                     //엔진을 가지는 Car class
private:
    Engine* engine;             //Engine 객체를 변수로 추가
public:                     
    Car(Engine* e) : engine(e) {}   //자동차 생성 시 엔진 장착!
    void start() {                  //주행 시작!
        if (engine) {               
            cout << "부릉부릉" << endl;
            engine->start();
        }
        else {                      //엔진 없으면?
            cout << "Where is Engine?" << endl;
        }
    }
};

int main() {
    Engine engine("V8");
    Car car(&engine);
    car.start();
    car = Car(nullptr);
    car.start();
    return 0;
}

**출력결과**
부릉부릉
I'm V8 engine.
Where is Engine?
```

## Dependency 의존

**의미**

객체가 다른 객체를 사용하는것

의존된 객체의 기능이 수정될 경우, 의존하는 객체는 손상될 수 있다.

Calculator class가 수정될 경우 Client class에 매우 큰 영향을 미친다.

```cpp
#include <iostream>
using namespace std;

class Calculator { 
public:
    int add(int a, int b) {
        return a + b;
    }
};

class Client {
public:
    void doSomething() {
        Calculator calc;                //calc 객체 생성
        int result = calc.add(1, 2);    //Calculator class 의 add 함수 호출
        cout << "Result: " << result << endl;
    }
};

int main() {
    Client client;
    client.doSomething();
    return 0;
}
```

**다형성**

**하나의 코드가 다양한 타입의 객체에 대해 작동할 수 있는 능력**

상속 or 인터페이스를 통해 구현된다

같은 이름을 갖는 여러 형태의 함수를 클래스 별로 만들 수 있게 하는 기능

**다양한 종류의 객체들을 같은 이름의 함수에서 이용할 수 있게 하는 기능**

```cpp
#include <iostream>
#include <string>
using namespace std;

class Super {
public:
    void print() {
        cout << "슈퍼" << endl;
    }
};

class Sub : public Super {
public:
    void print() {
        cout << "서브" << endl;
    }
};

int main() {
    
    Super* s = new Super();
    s->print();
    delete s;

    s = new Sub;
    s->print();
    return 0;

    return 0;
}

**출력결과**
슈퍼
슈퍼
```

한 포인터로 다양한 객체를 사용할수있음

그러나 Sub class의 Super함수 사용 불가능! → **가상함수 (virtual)** 로 해결 가능!

```cpp
#include <iostream>
#include <string>
using namespace std;

class Super {
public:
    virtual void print() {
        cout << "슈퍼" << endl;
    }
};

class Sub : public Super {
public:
    virtual void print() {     **//상속받은 함수는 안붙여도 되는데 가독성을 위해 붙여둠**
        cout << "서브" << endl;
    }

};

int main() {
    
    Super* s = new Super();
    s->print();
    delete s;

    s = new Sub;
    s->print();
    return 0;
}

출력결과
슈퍼
서브
```

# 객체지향 특징

------

## Upcast/Downcast

**dynamic cast**

```cpp
class Super {
public:
	virtual void print() const {
		cout << "슈퍼 클래스" << endl;
	}
};
class Sub : public Super {
public:
	virtual void print() const {
		cout << "서브클래스" << endl;
	}
};
int main() {
	Super* p = new Sub();   /

	Sub* subPtr = dynamic_cast<Sub*>(p);
	if (subPtr != nullptr) {
		// p가 Sub 클래스 타입으로 다운캐스팅될 수 있는 경우
		// subPtr을 이용하여 작업을 수행합니다.
		subPtr->print();
	}
	else {
		// p가 Sub 클래스 타입으로 다운캐스팅될 수 없는 경우
		// 다운캐스팅이 실패한 것으로 처리합니다.
		cout << "다운 캐스팅 실패" << endl;
	}
}
```

UpCast : 더 큰 클래스인 자식 클래스(Sub)를 생성하고 부모 클래스(Super) 포인터에 주소 대입함

DownCast : Upcast된 포인터를 다시 자식 클래스 포인터 자료형으로 변경함

**Coupling**

클래스 간 상호 의존성의 정도

Coupling 덜할수록 좋다

**OCP ( Open-Closed Principle)**

새로운 기능 추가는 쉽고, 기존 내용 변경은 어렵게 하는 방식

캡슐화를 적용시켜, 객체내부 구현은 숨기고, 외부에서 객체의 인터페이스를 통해 접근할수 있도록 함

coupling 낮추고 확장성 높임

**다형성(Polymorphism)**

함수나 변수 선언 등이 다양한 자료형에 대하여 호환적임

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual double getArea() {}
};

class Circle : public Shape {
private:
    double radius;
public:
    Circle(double r) : radius(r) {}
    double getArea() {
        return radius * radius * 3.14;
    }
};

class Rectangle : public Shape {
private:
    double width;
    double height;
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    double getArea()  {
        return width * height;
    }
};

//다형성 함수!!
void printArea(Shape& shape) { //다형성
    cout << "넓이: " << shape.getArea() << endl;
}

int main() {
    Circle myCircle(2.0);
    Rectangle myRect(3.0, 4.0);
    printArea(myCircle); // 넓이: 12.56
    printArea(myRect); // 넓이: 12
    return 0;
}

출력
넓이: 12.56
넓이: 12
```

printArea 함수 매개변수의 자료형은 Shape로 부모 클래스 이지만 Circle,Rectangle 클래스도 Upcasting해서 input으로 들어올수 있다

# 가상함수, 추상 클래스, 인터페이스

**가상함수**

- 상속하는 클래스 내에서 같은 시그니처의 함수로 오버라이딩 될 수 있는 함수 또는 메소드

- 객체 지향 프로그래밍 (OOP)의 다형성에서 중요한 부분

  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;
  
  class Super {
  public:
      //가상함수
      virtual void print() {
          cout << "슈퍼" << endl;
      }
  };
  
  class Sub : public Super {
  public:
      virtual void print() {     **//상속받은 함수는 안붙여도 되는데 가독성을 위해 붙여둠**
          cout << "서브" << endl;
      }
  
  };
  
  int main() {
      
      Super* s = new Super();
      s->print();
      delete s;
  
      s = new Sub;
      s->print();
      return 0;
  }
  
  출력결과
  슈퍼
  서브
  ```

**추상 클래스**

- 순수 가상함수를 한개 이상 포함한 클래스
- 객체를 직접 생성할수 없다
- 상속받은 class에서 순수가상함수 구현해야 한다
- 상속을 통한 함수의 구현을 강제한다

**순수 가상 함수**

가상함수를 0으로 때려넣음

```cpp
#include<iostream>
using namespace std;

class Shape {
public:
	**virtual void draw() = 0;**	//순수 가상 함수

};

class Rectangle : public Shape {
public:
	void draw() {
		cout << "사각형" << endl;
	}

};

int main(){
	Shape* s = new Rectangle;
	s->draw();
	return 0;
}
```

**인터페이스(interface)**

멤버함수가 모두 순수 가상함수만으로 존재함

java에는 interface기능이 따로 있지만 C++는 따로 없어서 클래스 이름 앞에 I를 붙이는게 관습적

```cpp
#include <iostream>
using namespace std;

class IDrawable {               //인터페이스
public:
    virtual void draw() = 0;    //순수가상함수
};

class Circle : public IDrawable {
private:
    int radius;
public:
    Circle(int r) : radius(r) {}
    void draw() override {
        cout << "r : " << radius << endl;
    }
};

int main() {
    Circle c(30);
    c.draw();
    IDrawable* d = &c;
    d->draw();
    return 0;
}
```

**구체 클래스(Concrete class)**

인스턴스를 생성할 수 있는 class

추상 클래스의 반대 개념으로 봐도 됨

```cpp
#include <iostream>
using namespace std;

class IAnimal {
public:
    virtual void makeSound() = 0; // 순수 가상 함수
};

class Dog : public IAnimal {
public:
    void makeSound() override {
        cout << "멍멍!" << endl;
    }
};

class Cat : public IAnimal {
public:
    void makeSound() override {
        cout << "야옹~" << endl;
    }
};

void callSound(IAnimal& animal) { //다형성, Animal class의 객체가 무엇인지 신경쓰지 않는다.
    animal.makeSound();
}

int main() {
    Dog myDog;
    Cat myCat;
    callSound(myDog); // "멍멍!"
    callSound(myCat); // "야옹~"
    return 0;
}
```

**분할 컴파일**

.cpp파일 컴파일 후 배포하면 됨—> 캡슐화 원칙

```cpp
HYPEBOY.h

#ifndef __HYPEBOY__
#define __HYPEBOY__

#include <iostream>
using namespace std;

//클래스 선언 -> .h
class HYPEBOY {

public:
	void print();
};

#endif
HYPEBOY.cpp

#include "HYPEBOY.h"

//클래스 멤버함수 정의 -> .cpp
void HYPEBOY::print() {
	cout << "하이 뉴진스" << endl;
}
main.cpp

#include "HYPEBOY.h"

int main() {
	HYPEBOY* h = new HYPEBOY;
	h->print();
	return 0;
}
```

# **연산자 오버로드**

**목적**

- 사용자 정의 타입을 만들고 계산하고 출력하기 위하여 사용
- 클래스 멤버함수 or 일반 비 멤버함수 두가지 방식으로 정의 가능

1. **클래스 멤버함수 정의 방식**

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
       }   //a에다가 b의 값을 복사해서 덧셈을 한 뒤 그 결과를 새로운 complex 객체로 만들어서 return
   
       void print() {
           cout << real << " + " << imag << "j" << endl;
       }
   };
   
   int main() {
       Complex a(1.0, 2.0);
       Complex b(3.0, 4.0);
       Complex c = a + b;    //a+b의 결과가 Complex type
       c.print();
       return 0;
   }
   
   출력
   4 + 6j
   ```

2. **비멤버함수 정의 방식**

   class멤버에 접근 불가→

   1. friend 붙여서 선언, 멤버의 값을 가져올 수 있도록 getter 함수 구현
   2. 객체 2개를 복사해서 매개변수로 받아와 각각의 객체의 값을 getter로 읽어와서 덧셈을 한 뒤 덧셈 값을 가진 새로운 complex 객체 생성해서 return

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

# 오버로딩 종류

1. **단항 연산자 오버로딩**

   ```cpp
   #include <iostream>
   using namespace std;
   class Fraction {
   public:
       int numerator;
       int denominator;
       Fraction(int num, int den) : numerator(num), denominator(den) {}
   
       // 단항 연산자 - 오버로딩 함수
       Fraction operator-() const {
           return Fraction(-numerator, denominator);
       }
   
       // 단항 연산자 + 오버로딩 함수
       Fraction operator+() const {
           return Fraction(+numerator, denominator);
       }
   
       void print() const {
           cout << numerator << "/" << denominator << endl;
       }
   };
   
   int main() {
       Fraction f1(3, 4);
       Fraction f2 = -f1;  // 단항 연산자 오버로딩 호출
       Fraction f3 = -f2;  // 단항 연산자 오버로딩 호출
       f1.print();  // 3/4 출력
       f2.print();  // -3/4 출력
       f3.print(); // 3/4 출력
       return 0;
   }
   ```

2. **전위증감 연산자 오버로딩**

   ```cpp
   #include <iostream>
   using namespace std;
   class Counter {
   private:
       int count;
   public:
       Counter() : count(0) {}
       int getCount() const { return count; }
       // 전위 증가 연산자 오버로딩
       Counter& operator++() {
           count++;
           return *this;
       }
       // 전위 감소 연산자 오버로딩
       Counter& operator--() {
           count--;
           return *this;
       }
   };
   int main() {
       Counter c1;
       cout << "Count: " << c1.getCount() << endl;
       ++c1;
       cout << "Count: " << c1.getCount() << endl;
       --c1;
       cout << "Count: " << c1.getCount() << endl;
       return 0;
   }
   ```

3. **<<연산자 오버로딩**

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
       friend 
   };
   
   ostream& operator <<(ostream& os, const Point& p) {
       os << "(" << p.getX() << ", " << p.getY() << ")";
       return os;
   }
   
   int main() {
       Point p{ 1, 2 };     //-> uniform 초기화
       cout << p << endl; // (1, 2)
       return 0;
   }
   ```

   getX,getY는 비멤버함수인 operator <<에 멤버변수 x, y를 접근할수 있도록 하는 함수

   →그래서 friend 안써도 된다

   p{1,2} 는 uniform 초기화

4. **대입연산자 오버로딩**

   `a = b`는 `a.operator=(b)`로 간주할수 있다.

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

   또한, 연산자 반환은 참조로 하는 것을 확인할 수 있다. 이는 다음과 같이 `operator chaining`을 허용하기 위함이다.

   ```cpp
   int a, b, c, d, e;
   a = b = c = d = e = 42;
   ```

   ```cpp
   a = (b = (c = (d = (e = 42))));
   ```

   다르게 말해서, 대입(할당)은 우측결합이다. 마지막 대입(오른쪽) 연산을 먼저 계산하고, 좌측으로 연산하며 값이 전달되는 방식이다.

   1. **→, \* 연산자 오버로딩**

5. 사이트…

   [[문과 코린이의 IT 기록장\] C,C++ - 연산자 오버로딩 7 : 포인터 연산자 오버로딩(포인터 연산자 오버로딩, 스마트 포인터 (Smart Pointer), ( )연산자의 오버로딩과 펑터(Functor), 임시객체로의 자동 형 ..](https://vansoft1215.tistory.com/42)

**const 위치**

함수 이름 뒤 : 함수 내에서 수정 불가

매개변수 : 매개변수 전달 시 수정

함수 앞 : return값 불변

**상수 객체**

1. 클래스 멤버변수 수정 불가능

```cpp
#include<iostream>
using namespace std;

class MyClass {
private:
    int num;
public:
    MyClass(int n) : num(n) {}
    void setNum(int n) {
        num = n;
    }
    int getNum() const {
        return num;
    }
};

int main() {
    const MyClass a(10);    // const 객체
    MyClass b(20);          // 비 상수 객체
    //a.setNum(30);         // 컴파일 에러, const 멤버 함수만 호출 가능
    cout << "a.getNum(): " << a.getNum() << endl;
    b.setNum(30);           // 일반 멤버 함수 호출 가능
    cout << "b.getNum(): " << b.getNum() << endl;
    return 0;
}
```

2. 오버로딩 할때 상수객체와 비상수객체 각각 따로따로 오버로딩 해야한다

```cpp
#include <iostream>
using namespace std;

class MyInteger {
private:
    int value;
public:
    MyInteger(int v) : value(v) {}
    // 비상수 객체에 대한 연산자 함수
    //-> 밑에처럼 const로 오버로딩 정의되어 경우 생략 가능함!!!
   MyInteger operator+(const MyInteger& rhs) {
        return MyInteger(value + rhs.value);
    }
    // 상수 객체에 대한 연산자 함수
    MyInteger operator+(const MyInteger& rhs) const {
        return MyInteger(value + rhs.value);
    }
    void print() const {
        cout << value << endl;
    }
};

int main() {
    const MyInteger a(1);   //상수 객체
    const MyInteger b(2);   //상수 객체
    MyInteger c(3);         //비 상수 객체
    MyInteger d = a + b; // 상수 객체끼리 더하기 연산
    d.print();
    d = c + b; // 비상수 객체끼리 더하기 연산
    d.print();
    d = a + c; // 상수 객체에 대한 const 멤버 함수 호출
    d.print();
    return 0;
}
```