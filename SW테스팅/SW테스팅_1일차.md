**SW란?**

프로그램 사용과 동작을 설명한 문서 + 명령어 +자료구조 + 산출물 업데이트 할 경우 Failure성 다시 올라감 **비가시성** → 제대로 코드가 개발되고 있는지 보기 어려움

**SW개발 프로세스**

기획(요구사항 분석)→ 설계 → 구현 → 테스팅

**iso26262기능안전 프로세스**

자동차 SW 개발 라이프사이클 모델 Validation & Verification 강조됨 이해하기 쉬운 코드가 좋은코드 → 테스트하기 좋은 코드

**Safety Critical system**

Functional safety 중요한 system 예) 항공기, 자동차, 인공위성, 의료 등등

## 3가지의 개발 프로세스 모델

1. **주먹구구식 모델(Build fix model)**

   신기술 개발하는 연구소, 규모가 작은 스타트업 에서 사용

   개발산출물 검증할 시간이 없기 때문

2. **폭포수모델( Waterfall Model )**

   1. 요구사항 분석

      SRS: Software Requirement Specification

      SysRS: System Requirement Specification

      HwRS, SwRS:  Hardware(Software) Requirement Specification

   2. 설계 상세설계 : SwDD(Software Detailed Design)

   3. 테스트 SRS, Design 문서 비교해서 잘 개발되는지 확인한다

   4. 유지보수

   앞단의 output이 뒷단의 input이 됨 → 지연될 경우 개발 프로세스 오래 걸림

   SW개발 프로세스중 가장 많이 사용됨

   SwRS문서가 나와야 설계 가능하다

3. **Agile 모델**

   ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/033997ae-63c3-45f8-8855-9d9452d0e805/Untitled.png)

   반복적 개발

   앞단계에서 요구사항 명확하게 정의될 수 없으므로(고객이 요구사항 지속적으로 요청) 폭포수 개발을 여러번 반복하며 개발하는 단계

   독일 중심으로 Agile-SPICE 발표

   자동차 SW에 Agile process 적용

   확정된 요구사항 먼저 개발,설계, 구현, 테스트

   추가된 요구사항 있으면 다시 위에 단계를 반복

   즉 **Waterfall 프로세스를 단위 SW마다 여러번 진행**

   모바일 application : 2주 단위 반복 자동차 전장 sw 4개월 단위 반복

## A-SPICE

------

**A-SPICE**

독일3사 주축으로 만든 모델

SPICE란 ISO15504

개발 잘하는 SW회사의 프로세스를 모은게 SPICE

**A-SPICE 단계**

0단계 : 주먹구구식 개발

1단계 : 개개인이 개발 체계 갖고있음

2단계 : Managed, 즉 우리의 부서는 체계가 있어서 process대로 개발됨

3단계 : HMC처럼 조직 내부는 모두 동일한 Process있어서 조직차원에서 관리가 됨

4단계 : 예측가능

5단계 : 개헌 가능

자동차 개발 프로세스에서는 보통 2~3단계 요구함

**요구사항**

원하는대로 동작하기 위한 기술성

> 명시적 요구사항 : 고객이 요구항 사항

> 묵시적 요구사항 : 요구하지 않았더라도 당연히 제공되어야한다고 가정되는 사항
>
> 예)엘리베이터 타면 닫히고 올라가는것, 로그인 될때 로그아웃도 되어야함

※제악사항: 동작되면 안되는 기능들

**요구사항 검증**

- 무결성 완전성: 누락된 요구사항이 없는지
- 일관성 : 요구사항이 서로 모순되지 않는지, 동일한 용어 사용
- 명확성 : 요구사항에 모호함이 없어 커뮤니케이션 가능 예) 많이, 적절히, 0에 가깝게 라는 단어 사용 금지
- 검증 가능성 : 요구사항 작성할때는 test가능하게 작성(입력, 출력값 정의)
- 추적성 : 요구사항이 추가되었을때 각 개발 process에 자동적으로 적용되는지 자동차에서는 양방향 추적성 중요

**유스케이스 다이어그램**

사용자의 관점에서 시스템의 서비스 혹은 기능 및 그와 관련된 외부 요소 보여주는 다이어그램

액터, 유스케이스(기능), 시스템, 관계로 구성됨

고객과 개발자가 함께 보며 요구사항에 대한 의견 조율 가능

소프트웨어와 의사소통하는 모든 외부 시스템 or 고객을 나타냄

어떻게 기능을 구현 할건지는 파악 불가능 → 각 유스케이스(기능)마다 유스케이스 기술서( Usecase Description) 작성한다

**Usecase Description**

기능이 발동되기 위한 사전 조건

기능이 끝나고 난 후 반드시 수행해야하는 사후 조건 들어있음

**Usecase Test**

Usecase Scenario 보고 한다 Usecase가 어떻게 흘러가는지 시나리오 보고 함

정상흐름(Normal Flow): 유스케이스가 정상적으로 수행되는 흐름

대안흐름(Alternative Flow):  정상흐름 아니지만 다른 선택적인 방법으로 실행되는거

예외 흐름(Exceptional Flow): 작업흐름중 예기치 못한 상황으로 인하여 시스템이 어떻게 종료되는지

**상위설계, 하위설계**

상위설계: 아키텍처 설계

하위설계(상세설계) : 소스코드 설계, 자료구조, 알고리즘 설계

**ISO 26262 아키텍처 설계 원칙**

> **상위설계**

1d: Strong cohesion within each software componet

1e: Loose coupling between software components

**코드 모듈화 하기 → 응집도 up, 결합도 down**

응집도 : 같은 기능을 하는 애들끼리 뭉치고

결합도 : 함수끼리 얽힌걸 끊어내기

> **하위설계**

포인터 제한적으로 쓰기

전역변수 줄이기

변수 초기화 하기

재귀함수 사용 금지

casting 금지

등등….

**SW 이중화 다중화**

> 이중화(Redundancy)

함수를 복사하여 붙여넣었을때 input에 대한 결과가 같은지 보는것

결함이 복사될수도 있음

> 다향성(diversity)

기능이 같은 함수의 알고리즘을 다르게 사용함

input과 output은 같음

**구현**

코드작성 + 디버깅 + 단위,통합 테스트

개발자별 코딩스타일 다르기 때문에 코딩표준 기준으로 개발(ISO26262, 회사 코딩 스타일 지침서…)

## 형상관리

------

**형상관리(버전관리)**

무결성: 제외된 요구사항 or 다른 요구사항 없는지 확인

추적성: 양방향 추적성 즉 상위, 하위 단계로의 연결이 표시되어야함

기업에서 필수적으로 하는 process

**CCB(Configuration Control Board, 형상 통제 위원회)**

형상 항목의 변경을 수락 또는 거절

베이스라인 수립 여부

승인된 변경에 대한 책임 및 보증

베이스라인의 변경 요청이 필요한 경우, 이에 대한 검토 및 승인

**Baseline**

Read Only 상태

Write 불가능(CCB의 허락 있어야 변경 가능)

소프트웨어 개발 하나의 완전한 산출물로써 쓰여질 수 있는 상태의 집합

공식적인 변경 통제 절차에 의해서만 변경될 수 있는 상태

**※Release 란?**

소프트웨어 제품의 사용 가능한 상태에 대한 버전

**ISO26262의 UML**

The paradigm of model based development does not depend on the Language

Formal : Z언어

Semi-Formal : UML

**SW Quality**

> 제품의 품질

제품 자체가 가지는 품질 소비자가 요구하는 바에 얼마나 부합하는지 예)ISO26262  …

> 프로세스의 품질 :  제품을 만드는 사람들의 수준

예) A-SPICE …

**SW제품의 품질**

사용자, 상품 기획팀 둘다 만족시켜야 한다

요구사항 명세서와 실제 고객의 요구사항과 100프로 일치하는 경우는 거의 없다

V & V : Verification and Validation

V&V 활동이 강한 모델: V모델

**Verification vs Validation**

> Verification

SW요구사항 **명세서**에 따라 만들어지고 있는지 확인

> Validation

**고객이 실질적으로 의도**한 환경이나 사용 목적에 맞게 만들어지는지 확인

## SW테스팅

------

**SW테스트 종류**

정적 vs 동적 : SW실행여부

> 정적인 방법(Static)

SW실행하지 않고 결함 찾아냄

여러 사람들이 모여 SW검토, 정적 검증 도구 이용

예) ….

> 동적인 방법(Dynamic)

SW실행하여 결함을 찾아냄

**결함을 최대한 많이 발견**할 목적으로 테스트

디버깅 이용(우리가 흔히 알고있는 테스트)

종류) white, black box test

## Testing vs Debugging

------

**Testing**

알려지지 않은 숨겨진 결함의 발견

개발자 + 외부의 제 3자 가 수정

Re-Test : 결함을 수정했을때 한번더 확인함

**Debugging**

이미 알고 있는 결함의 수정

결함의 정확한 위치 파악, 결함의 이유 파악

개발자가 수정
