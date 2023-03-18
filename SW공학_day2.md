
# 자동차 소프트웨어 공학 2일차

Concern VS Requirment   
> 차이점?    
> Concern은 단지 '아 이거 필요한데?'라는 생각이다.  
> Requirment(요구사항)은 생각을 실제로 필요한 내용으로 변환한 것이다.

Function(기능) VS Non-Function(비기능)
> 요구사항은 기능과 비기능으로 나눠져 있고 비기능은 품질요구사항(Ex.성능, 보안) 제약사항으로 나누어져 있다.   
> SW요구사항은 크게 두가지(기능&비기능)로 이루어짐   
> 기능 혹은 비기능이 추가되면 Trade-off 성질이 발생 (Ex.성능이 올라가면 정확도가 떨어진다.)   

예시문제(기출 예정)    

<img src="./../IMG/0802.PNG" width="450px" height="210px" title=" " alt="예시 문제_수빈누나"></img><br/>   
* * *

## 소프트웨어 모델링 언어
* * *
 Unified Modeling Language (UML 모델링 언어)    
 
<img src="./../IMG/0801.PNG" width="450px" height="320px" title=" " alt="UML"></img><br/>   

그림에서 빨산색 박스 친 부분만 알면 된다.   
- 클래스 다이어그램
- 액티비티 다이어그램
- 유스 케이스 다이어그램
- 상태기계 다이어그램
- 순차 다이어그램

> ISO 26262   
> ASPICE와 다르게 더 구체적인 방향성을 제시   
> ASIL 같은 경우 국제 스탠다드로 A,B,C,D 등급으로 이루어져 있으며 등급이 높을수록(D에 가까울 수록) BAD   

Class Diagram   
> 추가 작성 필요 ㅠㅠ 왜이리 빨리 넘어가....
>

Usecase Diagram(추후에 다시 설명 예정)    

- 개요
> 사용자의 시각에서 SW 시스템의 범위와 기능을 정의한 모델   
> Actor와 Use case간의 관계를 정적으로 표현   
- 구성요소
> Actor, Usecase   
> 관계 : Association, Generalization, include, extend     
  
유즈케이스 다이어그램의 목적은?    

특징 : 시나리오를 그려서 모호함을 지운다.

1. 액터와 시스템 사이의 상호작용 분석
2. 최대한 간결하게 작성하여 설계시에 유연하게 접근 할 수 있도록 한다
3. 시스템의 내부 작동 방식을 포함해서 구체성 확보
4. 기능적 요구사항을 모두 포함하도록 작성


Sequence Diagram (순차 다이어그램)   

 기능 모델링의 결과로 식별된 유스 케이스별로 작성    


구성요소 : 실행 사건, 메시지 전달, 제어 로직     

실습 사진    

<img src="./../IMG/0803.PNG" width="450px" height="220px" title=" " alt="EA"></img><br/>  


State Machine Diagram (상태기계 다이어그램)     

시스템 상태들의 변화로 모델링하는 수단 
   
시스템이 어느 한 상태에 존재하다가 이벤트가 발생하면 다른 상태로 전이하는 동작을 표현   

상태기계 다이어그램 : 객체의 상태 변화 표현    

상태 전이를 유발하는 레이블 표현 형식    

<img src="./../IMG/0804.PNG" width="500px" height="80px" title=" " alt="SMD"></img><br/>  



* * *
## 소프트웨어 요구사항 개요
* * *

프로젝트 10대 실패 요인     

<img src="./../IMG/0805.PNG" width="450px" height="310px" title=" " alt="SMD"></img><br/>      

소프트웨어 공학의 목표는?   

계획된 개발 기간내에 소프트웨어 개발이 이루어질 수 있도록 개발 프로세스를 관리     
   
소프트웨어 개발에 관리기법을 적용하여 인력, 물적자원(예산,툴등) 효율적으로 관리    

* * *
## 소프트웨어 시스템과 컨텍스트
* * *

Stakeholders and Concerns

<img src="./../IMG/0806.PNG" width="450px" height="190px" title=" " alt="Stakeholders and Concerns"></img><br/> 

SDLC – Requirements Gathering    

-  소프트웨어 요구사항 수집   

SDLC – Requirements Analysis    

- 소프트웨어 요구사항 분석

SDLC – Software Design    

- 소프트웨어 설계


요구사항 분석에서 가장 먼저 하는 것?    

 > 개발의 범위를 명확하게 해야 한다. -> Context Diagram    

정확한 시스템 Goal 식별   
> 개발범위 정의
>  - Product의 boundary 정의
>  - 특정 릴리즈에 대한 범위 정의 : Divide and Conquer
>  - 범위확장 관리

컨텍스트 다이어그램 예시    

<img src="./../IMG/0807.PNG" width="450px" height="300px" title=" " alt="Stakeholders and Concerns"></img><br/> 



* * *
## 소프트웨어 시스템 문맥 (Context) 다이어그램
* * *

Static Modeling   

> Context Diagram을 그리기 위해서는 우선 시스템 관점에서 입출력 장치를 식별하고 시스템과의 관계를 구성(Composition) 또는 집합(Aggregation)으로 그려본다.   
> 관계가 있는 타 시스템 또는 Human을 식별하고 주고 받는 메시지를 작성한다.   
> UML의 스테레오타입을 사용하여 각 element들의 신분을 명확히 한다(모델링).   
> Element의 개수(multiplicity)를 설정하고 서로간의 주고 받는 메시지를 표준이름으로 모델링한다.   
> 시스템 관점의 Context Diagram과 소프트웨어 관점의 Context Diagram 의 차이점을 인지한다.  


* * *
## 소프트웨어 시스템 요구사항이란?
* * *

요구사항 : 소프트웨어 시스템이 수행해야 할 것과 소프트웨어 시스템에 있어야 할 특성을 기술한 문장   

> 기능적 요구사항   
>  사용자의 업무 처리와 직접 관련되어 소프트웨어 시스템이 수행해야 하는 요구 내용   
>  소프트웨어 개발에서 반드시 구현되어야 하는 항목    

> 비기능적 요구사항   
>  소프트웨어 시스템이 제공해야 하는 행위적 속성   
> 운영 요구사항, 자원 요구사항, 성능 요구사항, 보안 요구사항, 문화적/정책적 요구사항   

> 인터페이스 요구사항   
>  시스템을 사용하는 과정에서 지원해야 하는 GUI 요구사항   
>  시스템이 수행하는 과정에서 발생할 수 있는 기존 시스템과의 연동 요구사항   

> 기타 제약사항   
> 조직의 지침에 대한 적용의 필요성   
> 국내, 국제 표준(Standards)에 대한 적용 필요성    

Functional vs. Non-Functional Requirements   

>  Functional Requirements : 입력 대비 출력의 변형   
> - 기능이란 입력을 받아들여, 정의된 규칙에 의해 입력을 변형하여 출력을 만들어 내는 것   
  

> Non-Functional Requirements   
> - 사용자의 요구사항 중 기능적이지 않은 부분   
> - 일반적으로 Trade-off가 발생한다.

기능과 비기능은 종속적인 관계는 아니다.     
기능이 변하더라도 비기능이 변하진 않는다. >> "직교적인 관계"     
따라서 기능과 비기능의 Sector가 나눠져 있다.     
     
          

+) 요구사항에서 추적성이 굉장히 중요하다.


* * *
## 요구사항을 분석하라
* * *
요구사항 요약    

<img src="./../IMG/0808.PNG" width="450px" height="230px" title=" " alt="Stakeholders and Concerns"></img><br/>     


* * *
## 기능 모델링
* * *

기능 : 서로 상호작용하는 것    

> Use Case이름을 어떻게 지어야 할까?   
> '동사' (어떤 동작을 하는지)가 들어가야 한다.   




* * *
## 요구사항 명세서 작성하기
* * *

* * *
## 비기능 요구사항 모델링
* * *

 * * *
## 소프트웨어 아키텍처 개념
* * *



