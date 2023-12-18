# Ch6 Database Design Using the E-R Model

## Design Phases (설계 단계)
>   * 초기 단계 - 데이터베이스 사용자의 데이터 요구 사항을 완전히 특성화 한다.
>   * 두 번째 단계 - 데이터 모델 선택
>       * 선택한 데이터 모델의 개념 적용
>       * 이러한 요구 사항을 데이터베이스의 개념적 스키마로 변환
>       * 완전히 개발된 개념 스키마는 기업의 기능적 요구 사항을 나타낸다
>           * 데이터에 대해 수행될 작업(또는 트랜잭션)의 종류를 설명한다
>       * 최종단계 - 추상 데이터 모델에서 데이터베이스 구현으로 이동
>           * 논리적 설계 - 데이터베이스 스키마를 결정한다. 데이터베이스 설계에서는 관계 스키마의 좋은 컬랙션을 찾아야 한다.
>               * 비즈니스 결정 - 데이터베이스에 어떤 속성을 기록해야 합니까?
>               * 컴퓨터 과학 결정 - 어떤 관계 스키마를 가져야 하며 다양한 관계 스키마에 속성을 어떻게 배포해야 합니까?
>           * 물리적 디자인 - 데이터베이스의 물리적 레이아웃 결정

## Design Alternatives (디자인 대안)
>   * 데이터베이스 스키마를 설계할 때 다음 두 가지 주요 함정을 피해야 한다.
>       * 중복성: 잘못된 설계로 인해 정보가 반복될 수 있다.
>           * 정보의 중복 표현으로 인해 다양한 정보 사본 간의 데이터 불일치가 발생할 수 있다.
>       * 불완전성: 잘못된 설계로 인해 기업의 특정 측면을 모델링하기가 어렵거나 불가능해질 수 있다.
>   * 나쁜 디자인을 피하는 것만으로 충분하지 않다. 우리가 선택해야 할 좋은 디자인이 많이 있을 수 있다.

## Design Approaches (설계 접근 방식)
>   * 엔터티 관계 모델
>       * 기업을 다음의 집합으로 모델링한다. 엔터티, 관계
>           * 엔터티(Entity): 기업 내에서 다른 객체와 구별되는 '사물' 또는 '객체'
>               * 속성의 집합으로 설명됨
>           * 관계(Relationship): 여러 엔터티 간의 연결
>       * 도식적으로 다음과 같이 표현된다. entity-relationship 다이어그램
>   * 정규화 이론
>       * 어떤 디자인이 나쁜지 공식화하고 테스트

## ER model -- Database Modeling
>   * ER 데이터 모델은 세가지 기본 개념을 사용한다.
>       * 엔터티 세트
>       * 관계(relationship) 세트
>       * 속성

## Entity Sets
>   * 엔티티는 실제 존재하는 객체이고 다른 객체와 구별될 수 있다.
>       * 예: 특정인, 회사, 행사, 공장
>   * 엔터티 세트는 동일한 속성을 공유하는 동일한 유형의 엔티티 집합이다.
>       * 예: 모든 사람, 회사, 나무, 휴일의 집합
>   * 엔티티는 속성 집합으로 표현된다. 즉, 엔터티 집합의 모든 구성원이 소유한 설명 속성
>       * 예: instructor = (ID, name, salary), course= (course_id, title, credits)
>   * 속성의 하위 집합은 기본 키 엔터티 세트 즉, 집합의 각  구성원을 고유하게 식별한다.
>   ![image-76](https://github.com/joey0919/database-system/assets/87600842/b4c80a55-0b27-4bae-a135-48196a5095fd)

## ER 다이어그램에서 엔터티 집합 표현
>   엔터티 집합은 다음과 같이 그래픽으로 표현될 수 있다.
>       * 직사각형은 엔터티 집합으로 나타낸다.
>       * 엔터티 직사각형 내부에 나열된 속성
>       * 밑줄은 기본 키 속성을 나타낸다.

## Relationship Sets
>   * relationship은 여러 엔터티 간의 연결이다.
>   * Example: 
>       ![image-77](https://github.com/joey0919/database-system/assets/87600842/a0d0f9e7-1dfc-4bf2-839e-ae2617593795)
>   * 예 관계 집합을 정의한다. 학생과 조언자 역할을 하는 강사 간의 연관성을 나타낸다.
>   * 그림으로 관련 엔터티 사이에 선을 그린다.
>   ![image-78](https://github.com/joey0919/database-system/assets/87600842/5c85ee3c-d951-4907-a32c-4bf5268544a3)

## ER 다이어그램을 통해 관계 집합 표현
>   * 다이아몬드는 관계 집합을 나타낸다.
>   ![image-79](https://github.com/joey0919/database-system/assets/87600842/0874c623-8897-4756-a4ff-c144e3ed88ff)
>   * 속성은 관계 세트와 연관될 수도 있다.
>   * 예를 들어, 엔터티 사이의 advisor 관계 집합은 강사를 설정한다. 그리고 학생은 학생이 언제인지 추적하는 속성 날짜를 가질 수 있다.
>   ![image-80](https://github.com/joey0919/database-system/assets/87600842/3e97fcbb-f907-4229-a474-537a7bdbdb3f)

## 속성이 있는 관계 세트
>   ![image-81](https://github.com/joey0919/database-system/assets/87600842/be1d2b97-9150-4e9b-a1ca-86176b66d5cd)

## Roles
>   * 관계의 엔터티 집합은 고유할 필요가 없다.
>       * 엔터티 집합의 각 발생은 관계에서 "역할"을 수행한다.
>   * 라벨 "course_id"와 "prereq_id"는 roles 라고 불린다

## Degree of a Relationship Set (관계 집합의 정도)
>   * Binary relationship
>       * 두 개의 엔터티 집합(또는 2차)을 포함한다.
>   * 데이터베이스 시스템의 대부분의 관계 세트는 바이너리이다.
>   * 세개 이상 엔터티 집합 간의 관계는 거의 없다. 대부분의 관계는 이분법적이다.
>       * 예: 학생들은 강사의 지도하에 연구과제를 한다.
>       * 관계 proj_guide는 강사, 학생, 프로젝트 간의 3자적인 관계이다.

## Non-binary Relationship Sets
>   * 대부분의 관계 세트는 바이너리이다.
>   * 관계를 이진법이 아닌 것으로 표현하는 것이 더 편리한 경우가 있다.
>   * 삼항 관계가 있는 E-R 다이어그램
>   ![image-82](https://github.com/joey0919/database-system/assets/87600842/e6900c7b-bf28-471c-b0e9-ff4463d35a09)

## Complex Attributes (복합 속성)
>   * 속성 유형
>       * Simple and Composite attributes
>       * Single-valued and multivalued attributes
>           * Example: multivalued attributes: phone_numbers
>       * Derived attributes (파생속성)
>           * 다른 속성으로부터 계산 가능
>           * 예: 나이, 생년월일 지정
>       * 도메인 - 각 속성에 허용되는 값 세트
>   ![image-83](https://github.com/joey0919/database-system/assets/87600842/35c8c75a-4afe-4b6d-b378-affb39d21d49)

## ER 다이어그램에서 복잡한 속성 표현
>   ![image-84](https://github.com/joey0919/database-system/assets/87600842/9fe7f8c3-49bb-4eed-bcb0-392d7f689b07)

## Mapping Cardinality Constraints (카디널리티 제약조건 매핑)
>   * 관계 집합을 통해 다른 엔터티와 연관될 수 있는 엔터티 수를 표현한다.
>   * 이진 관계 집합을 설명하는 데 가장 유용하다.
>   * 이진 관계 집합의 경우 매핑 카디널리티는 다음 유형 중 하나여야 한다.
>       * One to one
>       * One to many
>       * Many to one
>       * Many to Many

## 카디널리티 매핑
>   ![image-85](https://github.com/joey0919/database-system/assets/87600842/62165615-d887-40ad-8133-e32d178a7be0)
>   ![image-86](https://github.com/joey0919/database-system/assets/87600842/81a232b0-04b3-465f-8284-d4602d86762e)

## ER 다이어그램에서 카디널리티 제약 조건 표현
>   * 방향성 선 (->), 관계 세트와 엔터티 사이의 "one"을 의미
>   * 방향이 지정되지 않은 선(-), "many"를 의미
>   * One-to-one 관계 instructor, student
>       * 학생은 관계를 통해 최대 한 명의 강사와 연결된다.
>       * 학생은 stud_dep을 통해 최대 한 학과와 연결되어 있다.
>       ![image-87](https://github.com/joey0919/database-system/assets/87600842/dc2aeb6c-d737-4d29-8f6d-0853267af387)
>   * One-to-Many 관계
>
>       ![image-88](https://github.com/joey0919/database-system/assets/87600842/d0b2defe-5cc5-4461-924f-bc512b27072b)
>   * Many-to-One 관계
>
>       ![image-89](https://github.com/joey0919/database-system/assets/87600842/ea178e7e-891b-47c3-9cf9-40c15e1887fb)
>   * Many-to-Many 관계
>
>       ![image-90](https://github.com/joey0919/database-system/assets/87600842/fdc0023d-b8b2-4e6d-85be-8baf24963032)

## Total and Partial Participation (전체 및 부분 참여)
>   * 전체 참여(이중선으로 표시): 엔터티 집합의 모든 엔터티는 관계 집합의 적어도 하나의 관계에 참여
>   ![image-91](https://github.com/joey0919/database-system/assets/87600842/c2a56e32-bbad-4e3a-9a1c-d2080abd7369)
>   * 모든 학생은 관련강사가 있어야 한다.
>   * 부분 참여 일부 개체는 관계 집합의 어떤 관계에도 참여하지 않을 수 있다.

## Notation for Expressing More Complex Constraints(보다 복잡한 제약 조건을 표현하기 위한 표기법)
>   * 라인에는 다음 형식으로 표시된 관련 최소 및 최대 카디널리티가 있을 수 있다. l..h l은 최소값 h는 최대값이다.
>       * 최소값 1은 전체 참여를 나타낸다.
>       * 최대값 1은 엔터티가 최대 하나의 관계에 참여함을 나타낸다.
>       * 최대값 *은 제한이 없음을 나타낸다
>
>       ![image-92](https://github.com/joey0919/database-system/assets/87600842/70115b1b-8fb5-484b-aaaa-d06f7c999c5e)
>   * 강사는 0명 이상의 학생에게 조언을 할 수 있다. 학생에게는 1명의 강사가 있다 여러명의 강사를 가질 수 없다.    

## Primary Key
>   * 기본 키는 엔터티 관계를 구별하는 방법을 지정하는 방법을 제공한다.
>       * 엔터티 세트
>       * Relationship 세트
>       * 약한 엔터티 세트

## Primary key for Entity Sets
>   * 엔터티 속성 값은 엔터티를 고유하게 식별할 수 있어야 한다.
>       * 엔터티 세트의 두 엔터티는 모든 속성에 대해 정확히 동일한 값을 가질 수 없다.
>   * 엔터티의 키는 엔터티를 서로 구별하는 데 충분한 속성 집합이다.

## Primary Key for Relationship Sets (관계 집합의 기본 키)
>   * 관계 집합의 다양한 관계를 구별하기 위해 관계 집합에 있는 엔터티의 개별 기본 키를 사용한다.
>       * R을 개체 집합 E1, E2, ...En을 포함하는 관계 집합이라고 하자
>       * R의 기본 키는 다음의 기본 키들의 결합으로 구성된다. 엔터티 세트 E1, E2, ...En
>       * 관계 집합 R이 a1, a2, ...의 속성을 갖는 경우, 그러면 R의 기본 키는 a1, a2, ..., am 속성도 포함된다
>   * 예: relationship set "advisor".
>       * primary key는 instructor.ID and student.ID
>   relationship set의 기본 키 선택은 관계집합의 매핑 카디널리티에 따라 달라진다.

## Choice of Primary key of Binary Relationship (바이너리 관계를 위한 기본 키 선택)
>   * Many-to-Many relationships: 최소 슈퍼키를 기본키로 선택한다.
>   * One-to-Many relationships: "Many"측의 기본 키는 최소 슈퍼키이고 기본키로 사용된다.
>   * Many-to-One relationships: "Many" 측의 기본 키는 최소 슈퍼키이고 기본키로 사용된다.
>   * One-to-One relationships: 참여하는 엔터티 집합 중 하나의 기본 키는 최소 슈퍼키를 형성하며, 둘 중 하나를 기본 키로 선택할 수 있다.

## Weak Entity Sets (약한 엔터티 세트)
>   * 고유하게 식별되는 엔터티 course_id, semester, year, sec_id
>   * section entities는 course entities와 관련이 있다. relationship set로 sec_course를 생성한다고 가정해보자
>   * 그러나 sec_course의 정보가 중복되어 있다. 왜냐하면 이미 section에는 해당 section이 관련된 course를 식별하는 course_id 속성이 있기 때문이다
>   * 이렇게 하면 섹션과 코스 간의 관계가 속성으로 암시적으로 나타나게 되어 원하지 않는 결과가 된다.
>   * 중복성을 다루기 위한 대안적인 방법은 section 엔터티에 속성 course_id를 저장하지 않고, 나머지 속성 section_id, year 그리고 semester만 저장하는 것이다.
>   * 그러나 이렇게 하면 section 엔터티 집합은 특정 section 엔터티를 고유하게 식별하는데 충분한 속성이 없게 된다.
>   * 이 문제를 해결하기 위해 sec_course 관계를 특별한 관계로 취급하고, 이 겨우에는 section 엔터티를 고유하게 식별하는 데 필요한 course_id와 같은 추가 정보를 제공하는 관계로 취급한다.
>   * 약한 엔터티 집합은 다른 엔터티, 즉 식별 엔터티에 의존하는 엔터티 집합이다.
>   * 약한 엔터티에 기본 키를 연결하는 대신, 우리는 식별 엔터티와 식별자라고 불리는 추가 속성을 사용하여 약한 엔터티를 고유하게 식별한다.
>   * 약한 엔터티 집합 표현
>       * E-R 다이어그램에서 약한 엔터티 세트는 이중 직사각형을 통해 표시된다.
>       * 점선으로 설정된 약한 엔터티의 판별자에 밑줄을 긋는다.
>       * 약한 엔터티 집합을 식별하는 강한 엔터티 집합에 연결하는 관계집합은 이중 다이아몬드로 표시한다.
>       * ![image-93](https://github.com/joey0919/database-system/assets/87600842/f2f6dabf-ee1f-4df5-b13f-ca33a11e95d0)

## Reduction to Relation Schemas (관계 스키마로의 축소)
>   * 엔터티 집합과 관계 집합은 다음과 같이 균일하게 표현될 수 있다.
>   * E-R 다이어그램을 따르는 데이터베이스는 스키마 모음으로 표현될 수 있다.
>   * 각 엔터티 집합 및 관계 집합에는 해당 엔터티 집합 또는 관계 집합의 이름이 할당된 고유한 스키마가 있다.
>   * 각 스키마에는 고유한 이름을 가진 여러 컬럼(일반적으로 속성에 해당)이 있다.

## 엔터티 집합 표현
>   * 강력한 엔터티 집합은 동일한 속성을 가진 스키마로 축소된다.
>       * ![image-94](https://github.com/joey0919/database-system/assets/87600842/e8cf9bb4-0fac-497c-a3d8-631463fd4c43)
>   * 약한 엔터티 집합은 식별 가능한 강력한 엔터티 집합의 기본 키에 대한 컬럼을 포함하는 테이블이 된다.
>       * ![image-95](https://github.com/joey0919/database-system/assets/87600842/f6228906-1367-4265-81f6-08e3e4d09426)
>   * Example:
>       ![image-96](https://github.com/joey0919/database-system/assets/87600842/74886d15-b7cd-423e-92e7-558c03e4f781)

## 복합 속성을 사용한 엔터티 세트 표현
>   * 각 구성요소 속성에 대해 별도의 속성을 생성하여 복합속성을 평면화 한다.
>       * 예: 복합속성이 있는 주어진 엔터티 세트 instructor 구성 요소 특성이 있는 이름 first_name 그리고 last_name은 부속속성을 가지고 있다. 이 엔터티 집합에 해당되는 스키마는 name_first_name과 name_last_name을 갖는다.
>       * 모호성이 없으면 접두사 생략 가능
>   * 다중값 속성을 무시하면 확장된 instructor 스키마는 다음과 같다.
>
>   ![image-97](https://github.com/joey0919/database-system/assets/87600842/ca9e5b29-7997-4596-9a72-46582060c32d)
>
>   ![image-98](https://github.com/joey0919/database-system/assets/87600842/c86327e7-d828-4f55-a8e7-90468c898c5d)

## 다중값 특성을 가진 엔터티 집합 표현
>   * 하나의 엔터티 E의 다중 값 속성 M은 별도의 스키마 EM으로 표현된다.
>   * 예시: instructor 엔터티의 다중 값 속성인 phone_number는 다음과 같이 스키마에 의해 표현된다. inst_phone = (ID, phone_number)
>   * 다중 값 속성의 각 값은 스키마 EM상의 별도의 튜플에 매핑된다.
>   * 예를 들어, 기본 키가 22222이고 전화번호가 456-7890 및 123-4567인 강사 엔터티는 두 개의 튜플에 매핑된다.  (22222, 456-7890) 및 (22222, 123-4567)

## 관계 집합 표현
>   * 다대다 관계 집합은 참여하는 두 엔터티 집합의 기본 키에 대한 속성과 관계집합의 설명 속성을 포함하는 스키마로 표시된다.
>   * 예: 관계 집합에 대한 스키마 조언자
>   * ![image-100](https://github.com/joey0919/database-system/assets/87600842/1342f8eb-b0ed-4b95-81a4-b7e21ecf3881)

## 스키마 중복
>   * 다대일 및 일대다 관계 세트는 "1" 측의 기본 키를 포함하는 "다"측에 추가 속성을 추가하여 나타낼 수 있다.
>   * 예: 관계 집합에 대한 스키마를 생성하는 대신 inst_dept속성을 추가하여 instructor 엔터티 집합에서 파생된 스키마에 dept_name을 추가한다.
>   ![image-101](https://github.com/joey0919/database-system/assets/87600842/3951c643-04d2-4b9d-a005-949bb7d2e661)
>   * 일대일 관계 집합의 경우 어느 쪽이든 "다" 쪽 역할을 하도록 선택할 수 있다.
>       * 즉, 두 엔터티 집합에 해당하는 테이블 중 하나에 추가 속성을 추가할 수 있다.
>   * 만약 참여가 "many"쪽에서 부분적이라면 스키마를 "many"쪽에 해당하는 스키마에 추가속성으로 교체하는것은 null이 발생할 수 있다.

## E-R 표기법에 사용되는 기호 요약
>   ![image-102](https://github.com/joey0919/database-system/assets/87600842/3b1953fb-14a1-4673-af32-4adcb645021d)
>   ![image-103](https://github.com/joey0919/database-system/assets/87600842/f4f620df-a1bf-44cb-898d-089f2a92006f)
