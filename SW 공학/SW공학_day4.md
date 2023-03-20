# 자동차 소프트웨어 공학 3일차

<br>

아키텍처 관점에서 두가지 View가 있다.   

1. Runtime View : 동적인 View

2. Module View : 정적인 View

3. Allocation View 

View : 관점 // 왜 필요할까?   

이해관계자들은 각자 Concern을 가지고 있다. 

따라서 이해관계자가 자신의 Concern을 확인 하기 위해 추상화를 통한 View가 필요하다.    

<br>

* * *
## 소프트웨어 아키텍처 스타일
* * *

<br>

 Logical Views : 구현하려 하는 계층구조를 나타냄 (Module View와 비슷)   
 Process views : Runtime View   
 Development Views &  Physical Views : Allocation View   

>런타임 뷰는 쓰레드에만 영향을 미치는 것이 아니라 프로세스 등에도 영향을 미친다   

<br>

CME SEI Three Views   

- Module View(모듈 뷰) : 소프트웨어 구현 단위를 표현, 소스코드의 청사진으로 사용 될수 있음
- C&C (Component-and-Connector) View : 런타임에 상호작용 하는 요소들을 표현. 
    -  즉 실행시간에 나타나는 시스템의 전체 윤곽을 보여준다.
- Allocation View (할당뷰) : 소프트웨어 아키텍처 뷰을 비아키텍처적인 부분과 매핑하는 구조를 표현

<br>

* * *
## 아키텍쳐 뷰 & 스타일
* * *

<br>

스타일 : 기존에 설계했을 때 발생하는 반복되는 문제에 대한 솔루션   

정적인 View   

- 모듈 스타일, 사용 스타일, 일반 스타일, 레이어 스타일 등이 존재     

동적인 View : C&C View    

- 파이프-필터 스타일, 공유-데이터 스타일 등이 존재   

<br>

* * *
## 아키텍쳐 설계
* * *

<br>

인터페이스 분리 원칙 : 클라이언트는 자신이 사용하지 않는 메소드에 의존하지 않는다.   


>단일 책임의 원칙 : 하나의 클래스는 하나의 ?만 가져야 한다.   
>
>1. 하나의 객체는 하나의 책임만 가져야 한다.
>
>2. 여러 객체들이 하나의 책임만 가질 수 있도록 분배
>
>3. 시스템에 변화가 생겨도 영향을 최소화 할 수 있는다.  
  