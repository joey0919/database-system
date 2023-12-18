# Ch7 Relational Database Design

## What About Smaller Schemas?
>   * inst_dept로 시작했다고 가정하면, 이것을 instructor와 department로 분해해야 하는지 어떻게 알 수 있을까?
>   * 이를 함수 종속으로 나타낸다: dept_name -> building, budget
>   * inst_dept에서는 dept_name이 후보키가 아니기 때문에 department의 building, budget이 반복될 수 있다.
>   * 이는 inst_dept를 분해해야 하는 필요성을 나타낸다.
>   * 손실이 있는 분해
>
>   ![Alt text](image-104.png)

## Example of Lossless-Join Decomposition (무손실 조인 분해의 예)
>   ![Alt text](image-105.png)

## First Normal Form (첫 번째 정규형)
>   * 도메인은 원자적이라면, 해당 도메인의 요소들이 분할될 수 없는 단위로 간주된다.
>   * 비-원자적 도메인의 예시
>       * 이름 집합, 복합 속성과 같은 것들
>       * CS101같은 식별 번호, 이는 부분으로 나눌 수 있는 경우
>   * 관계형 스키마 R이 첫 번째 정규형에 속한다면, R의 모든 속성의 도메인은 원자적이라는 의미이다.
>   * 예시: 각 고객에 대해 저장된 계좌 집합 및 각 계좌에 대해 저장된 소유자 집합과 같은 경우
>   * 우리는 모든 관계가 첫 번째 정규형에 속한다고 가정

## Functional Dependencies
>   * 법적 관계 집합에 대한 제약
>   * 특정 속성 세트의 값이 다른 속성 세트의 값을 고유하게 결정하도록 요구
>   * functional dependencies는 key의 개념을 일반화 한것
>   * functional dependencies란 R이라는 relation schema에 대해 α ⊆ R이고 β ⊆ R일 때, 모든 합법적인 관계 r(R)에 대해 어떤 두 튜플 t1과 t2가 r 내에서 속성 α에서 일치한다면, 속성 β에서도 일치한다는 것을 의미한다. 즉, t1[α]=t2[α]⟹t1[β]=t2[β]
>   * 예시로 r(A, B) 관계 스키마와 다음과 같은 r의 인스턴스를 고려해봅시다. 이 인스턴스에 대해 A → B는 성립하지 않지만, B → A는 성립한다.
>   * 이 맥락에서 A → B가 성립하지 않는다는 것은 A의 값이 같은 두 튜플이 B의 값에서 다를 수 있다는 것을 의미합니다. 반면에 B → A가 성립한다는 것은 B의 값이 같은 두 튜플이 A의 값에서도 같다는 것을 의미한다.

 ## Full/Partial Function Dependency (전체/부분 기능 종속성)
 >  * Full Functional Dependency (FFD)
 >      * R에서 속성 집합 Y가 속성집합 X에 함수적으로 종속되어 있지만, 속성집합 X의 전체가 아닌 일부분에는 종속되지 않음을 의미
 >      * 예: (고객아이디, 이벤트번호) -> 당첨여부
 >  * Partial Functional Dependency (PFD)
 >      * R에서 속성집합 Y가 속성집합 X의 전체가 아닌 일부분에도 함수적으로 종속됨을 의미
 >      * 예: (고객아이디, 이벤트번호) -> 고객이름

 ## Repetition -> Anomaly
 >  * 이상(anomaly)현상
 >      * 불필요한 데이터 중복으로 인해 테이블에 대한 데이터 삽입,수정,삭제 연산을 수행할 때 발생할 수 있는 부작용
 >  * Type of Anomalies
 >      * Insert Anomaly: 새 데이터를 삽입하기 위해 불필요한 데이터도 함께 삽입해야 하는 문제
 >      * Update Anomaly: 중복 tuple 중 일부만 변경하면 데이터가 불일치 하게되는 문제
 >      * Delete Anomaly: tuple을 삭제하면 다른 필요한 데이터까지 함께 삭제되는 데이터 손실 문제

 ## Insert Anomaly
 >  * 아직 이벤트에 참여하지 않은 아이디가 "melon"이고 이름이 "성원용", 등급이 "gold"인 신규 고객의 데이터는 이벤트참여 릴레이션에 삽입할 수 없음.
 >  * 삽입하려면 실제로 참여하지 않은 임시 이벤트번호(혹은 NULL)를 삽입해야 함
 >
 >  ![Alt text](image-106.png)

 ## Update Anomaly
 >  * 아이디가 "apple"인 고객의 등급이 "gold"에서 "vip"로 변경되었는데, 일부 tuple에 대해서만 등급이 수정된다면, "apple" 고객이 서로 다른 등급을 가지는 모순이 발생
>   ![Alt text](image-107.png)

 ## Delete Anomaly
 >  * 아이디가 "orange"인 고객이 이벤트 참여를 취소해 관련 튜플을 삭제하게 되면 이벤트 참여와 관련이 없는 고객아이디, 고객이름, 등급데이터까지 손실됨.
 >  ![Alt text](image-108.png)

 ## Goals of Normalization
 >  * 정규화는 데이터베이스 설계에서 중복을 최소화 하고 데이터의 일관성을 유지하기 위한 과정으로, 관계형 데이터베이스에서는 함수 종속성(FD)이나 다른 종속성 규칙을 기반으로 한다.
 >  * 따라서 주어진 관계 스키마 R과 함수 종속성 집합 F에 대해, 우리는 R이 "good" form인지 여부를 판단해야한다. 만약 R이 "good" form이 아니라면 이를 {R1, R2, ..., Rn}의 관계 스키마 집합으로 분해해야 한다. 각각의 분해된 관계 스키마는 "good" form이어야 한다.
 >  * 이것은 일반적으로 정규형에 따라 진행되며, 대표적으로 제1정규형, 제2정규형, 제3정규형 등이 있다. 이러한 정규형들은 데이터 중복을 방지하고 데이터 구조를 간결하게 만들어 데이터베이스의 일관성과 성능을 향상시킨다.

 ## Normal Form(NF, 정규형)
>   * 릴레이션이 Normalization된 정도
>   * 각 정규형 마다 제약조건이 존재
>       * 정규형의 차수가 높아질수록 요구되는 제약조건이 많아지고 엄격해짐
>   * 릴레이션의 특성을 고려하여 적합한 정규형을 선택

## 제 1 정규형 (1NF, First Normal Form)
>   * 릴레이션의 모든 속성이 더는 분해되지 않는 원자 값(atomic value)만 가지면 제 1 정규형을 만족함
>   * 제 1 정규형을 만족해야 관계 데이터베이스의 릴레이션이 될 자격이 있음
>   ![Alt text](image-109.png)
>   ![Alt text](image-110.png)
>   ![Alt text](image-111.png)

## 제 2 정규형 (2NF, Second Normal Form)
>   * 릴레이션이 제 1 정규형에 속하고 primary key가 아닌 모든 속성이 primary key에 FFD되면 제 2 정규형을 만족함
>   ![Alt text](image-112.png)
>   ![Alt text](image-113.png)

## 제 3 정규형 (3NF, Third Normal Form)
>   * 릴레이션이 제 2 정규형에 속하고, primary key가 아닌 모든 속성이 primary key에 transitivity 성질이 없으면 제 3 정규형을 만족함

## 보이스/코드 정규형(BCNF, Boyce/Codd Normal Form)
>   * 필요성
>       * 하나의 릴레이션에 여러 개의 candidate key가 존재하는 경우, 제 3 정규형까지 모두 만족해도 이상 현상이 발생할 수 있다.
>   * 의미: 강한 제 3 정규형(strong 3NF)
>       * Candidate key를 여러 개 가지고 있는 릴레이션에 발생할 수 있는 이상 현상을 해결하기 위해 제 3 정규형 보다 좀 더 엄격한 제약조건을 제시
>       * 보이스/코드 정규형에 속하는 모든 릴레이션은 제 3 정규형에 속하지만, 제 3 정규형에 속하는 모든 릴레이션이 보이스/코드 정규형에 속하는 것은 아니다.
>   ![Alt text](image-114.png)
>   ![Alt text](image-115.png)
>   ![Alt text](image-116.png)

## Lossless Decomposition (무손실 분해)
>   * R = (R1, R2)인 경우, 모든 가능한 관계 r에 대해 r = ∏R1 (r ) ∏R2 (r )이 성립해야 합니다. 즉, R의 모든 속성을 R1과 R2로 분해하였을 때, 원래의 관계 r을 복원할 수 있어야 한다
>   * 또한, R을 R1과 R2로 분해하는 것이 손실 없는 결합(lossless join)이 되려면, 다음 중 하나의 함수 종속성이 FDs(FD 집합)에 포함되어 있어야 한다.
>       * R1 ∩ R2 → R1
>       * R1 ∩ R2 → R2
>   * 즉, R1과 R2의 교집합이 R1이나 R2 중 어느 하나의 슈퍼키를 형성하면, R을 R1과 R2로 나누는 것이 손실 없는 결합이 된다.
>   * 예를 들어, inst_dept (ID, name, salary, dept_name, building, budget) 관계는 다음의 두 관계로 손실 없이 분해될 수 있다.
>       * instructor (ID, name, dept_name, salary)
>       * department (dept_name, building, budget)


## Design Goals
>   관계형 데이터베이스 디자인의 목표는 다음과 같다.
>       * 3NF or BCNF
>       * Lossless join decomposition

## Overall Database Design Process
>   * 스키마 R이 주어진 것으로 간주한다.
>       * 스키마 R은 개체-관계(E-R) 다이어그램을 테이블로 변환할 때 생성될 수 있다. 각 개체와 관계가 테이블로 매핑되어 R이라는 스키마가 형성된다.
>       * R은 관심 속성을 모두 포함하는 단일한 관계일 수 있다. 이를 "universal relation"이라고 부르기도 한다.
>       * 정규화는 스키마 R을 더 작은 관계로 나누는 과정이다. 이를 통해 데이터 중복을 최소화하고 무결성을 유지한다.
>       * R은 어떤 ad hoc(임의의) 디자인 결과로 생성되었을 수 있다. 이것은 초기에는 특별한 설계에 따라 만들어진 관계일 수 있으며, 그 후에는 정규화 등의 과정을 통해 품질을 향상시킬 수 있다.
