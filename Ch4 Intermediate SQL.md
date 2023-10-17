# Ch4 Intermediate SQL

## Joined Relations
>   * 조인 작업은 두개의 관계를 취하여 결과적으로 다른 관계를 반환
>   * from 절에서 하위 쿼리 표현식으로 사용됨
>   * 세가지 유형의 조인
>       * Natural join
>       * Inner join
>       * Outer join

## Natural Join in SQL
>   * Natural Join은 모든 공통 속성에 대해 동일한 값을 가진 튜플을 일치시키고 각 공통 열의 복사본을 하나만 유지한다.
>   * 강사의 이름과 강사가 가르친 강좌의 강좌 ID를 나열하시오.
>       ```
>       SELECT name, course_id
>       FROM students, takes
>       WHERE student.ID = takes.ID;
>       ```
>   * Natural join 구조를 사용하는 SQL의 동일한 쿼리
>       ```
>       SELECT name, course_id
>       FROM students NATURAL JOIN takes;
>       ```

## Dangerous in Natural Join
>   * 동일한 이름을 가진 관련 없는 속성 주의
>   * Correct version
>       ```
>       SELECT name, title
>       FROM student natural join takes, course
>       WHERE takes.course_id = course.course_id;
>       ```
>   * Incorrect version
>       ```
>       SELECT name, title
>       FROM student natural join takes natural join course;
>       ```
>   * 이 쿼리는 학생이 자신의 학과가 아닌 다른 학과에서 과목을 수행하는 경우 모든(학생이름, 과목제목) 쌍을 생략한다.

## Outer Join
>   * 정보 손실을 방지하는 조인 작업의 확장
>   * 조인을 계산한 다음 조인결과에 대해 다른 관계의 튜플과 일치하지 않는 한 관계의 튜플을 추가한다.
>   * null values 사용
>   * 세가지 형태의 외부 조인
>       * left outer join
>       * right outer join
>       * full outer join

## Left Outer Join
>   * course natural left outer join prereq
>   
>       ![image-38](https://github.com/joey0919/database-system/assets/87600842/4640ea90-044a-4c0c-948f-023014bb55de)
>   * In relational algebra:
>   
>       ![image-39](https://github.com/joey0919/database-system/assets/87600842/454d6c52-57c3-48bc-bac4-c0fea69cb8c8)


## Right Outer Join
>   * course natural right outer join prereq
>   
>       ![image-40](https://github.com/joey0919/database-system/assets/87600842/cd5cbd4c-0244-4418-874f-5eaab86673df)
>   * In relational algebra
>  
>       ![image-41](https://github.com/joey0919/database-system/assets/87600842/be1dae3d-2df1-4c51-a410-1721badae305)


## Full Outer Join
>   * course natural full outer join prereq
>   
>       ![image-42](https://github.com/joey0919/database-system/assets/87600842/dbb7e338-9964-48a8-acf4-c41c546edf2c)
>   * In relational algebra:
>
>       ![image-43](https://github.com/joey0919/database-system/assets/87600842/d791e60a-4cb9-46e8-948a-b25a120fdd67)

## Joined Types and Conditions
>   * Join 작업은 두 개의 관계를 취하여 결과적으로 다른 관계를 반환
>   * 이러한 추가 작업은 일반적으로 하위 쿼리 표현식으로 FROM 절에서 사용
>   
>       ![image-44](https://github.com/joey0919/database-system/assets/87600842/e3b1cfde-a937-4a99-a20f-37790784a9ce)

## Joined Relations - Examples
>   * course natural right outer join prereq
>   
>       ![image-45](https://github.com/joey0919/database-system/assets/87600842/a082731c-75cd-4f39-ad19-b03ff3d013e3)
>   * course full outer join prereq using (course_id)
>       
>       ![image-46](https://github.com/joey0919/database-system/assets/87600842/fba11328-ec62-4f0d-bb59-9e053251ec12)
>   * course inner join prereq on course.course_id = prereq.course_id
>   
>       ![image-47](https://github.com/joey0919/database-system/assets/87600842/9378c13b-6438-4af3-a265-e5e9c70a00a4)
>   * course left outer join prereq on course.course_id = prereq.course_id
>       ![image-48](https://github.com/joey0919/database-system/assets/87600842/b6a37ade-2753-4411-8604-49501932eda2)
>   * course natural right outer join prereq
>       
>       ![image-49](https://github.com/joey0919/database-system/assets/87600842/4f87679d-4ad9-485c-ae5c-178323528b92)
>   * course full outer join prereq using (course_id)
>   
>       ![image-50](https://github.com/joey0919/database-system/assets/87600842/71e7c7c6-1d0b-4656-83e2-7617d7a0d86b)


## View
>   * 가상테이블
>   * 독립성: 테이블 구조가 변경되어도 뷰에서 조회하는 컬럼이나 컬럼명, 테이블명 등이 그대로라면, 뷰를 사용하고 있는 응용 프로그램은 변경하지 않아도 된다.
>   * 편리성: 자주 사용되는 복잡한 쿼리를 미리 뷰로 정의해두면, 추후 쿼리는 간단한 형태로 표현, 조회 등이 가능하다.
>   * 보안성: 사용자의 권한에 따라 열람 가능한 데이터를 다르게 할 수 있다. 권한에 따라 확인 가능한 컬럼을 정의하여 뷰를 생성하면, 기본 테이블 모든 정보 노출 없이 접근 제어를 할 수 있다.
>   * 월급 없는 강사들의 모습
>       ```
>       CREATE view faculty as
>       SELECT ID, name, dept_name
>       FROM instructor
>       ```
>   * 생물학과의 모든 강사 찾기
>       ```
>       SELECT name
>       FROM faculty
>       WHERE dept_name = 'Biology'
>       ```
>   * 부서 금여 총액 보기 만들기
>       ```
>       CREATE view departments_total_salary(dept_name, total_salary) as
>           SELECT dept_name, sum(salary)
>           FROM instructor
>           GROUP BY dept_name
>       ```

## Views Defined Using Other Views
>   ```
>   CREATE view physics_fall_2017 as
>       SELECT course.course_id, sec_id, building, room_number
>       FROM course, section
>       WHERE course.course_id = section.course_id
>           and course.dept_name = 'Physics'
>           and section.semester = 'Fall'
>           and section.year = '2017';
>   ```
>   ```
>   CREATE view physics_fall_2017_waston as
>       SELECT course_id, room_number
>       FROM physics_fall_2017
>       WHERE building = 'Waton';

## Materialized Views (구체화된 뷰)
>   * 특정 데이터베이스 시스템에서는 뷰 관계를 물리적으로 저장할 수 있다.
>   * 쿼리에 사용된 관계가 업데이트되면 구체화된 뷰 결과가 최신이 아님.

## Update of a View
>   * faculty view에 새 튜플을 추가
>       ```
>       INSERT into faculty
>       values('30765', '그린', '음악', null);
>       ```

## View Updates in SQL
>   * 대부분의 SQL 구현은 단순 뷰에서만 업데이트를 허용
>       * FROM 절에 데이터베이스 관계가 하나만 있다.
>       * SELECT 절에서는 관계의 속성 이름만 포함되며 표현식, 집계 또는 DISTINCT 가 없다
>       * 목록에 나열되지 않은 모든 속성은 SELECT절에서 NULL로 설정될 수 있다.
>       * 쿼리에 GROUP BY 혹은 HAVING이 없어야 한다.

## Transactions
>   * transaction은 쿼리 및 업데이트 문으로 구성되며 작업의 단위이다
>   * SQL 표준은 SQL 문이 실행될 때 transaction이 암시적으로 시작되도록 정의
>   * Transaction은 다음 문 중 하나로 끝내야 한다.
>       * Commit work
>           * Transaction에 의해 수행된 업데이트는 데이터베이스에서 영구적이게 된다.
>       * Rollback work
>           * 트랜잭션의 SQL 문에 의해 수행된 모든 업데이트가 실행 취소된다.
>   * Atomic transaction
>       * 완전히 실행되거나 전혀 발생하지 않은 것처럼 롤백된다.
>   * 동시 트랜잭션으로부터 격리

## Constraints on a Single Relation (단일 관계에 대한 제약)
>   * not null
>   * primary key
>   * unique
>       * 후보키를 생성, 후보키는 null이 허용됨
>   * check

## The check clause
>   * 예: 학기가 가을, 겨울, 봄 또는 여름 중 하나인지 확인
>       ```
>       CREATE table section
>       (course_id varchar(8),
>       sec_id varchar(8),
>       semester varchar(6)
>       year(numeric(4,0)),
>       building varchar(15),
>       room_number varchar(7),
>       time_slot_id varchar(4)
>       primary key (course_id, sec_id, semester, year),
>       check(semester in ('Fall', 'Winter', 'Spring', 'Summer')))

## Referential Integrity (참조 무결성)
>   * foreign key (dept_name) references department
>   * foreign key (dept_name) references department(dept_name)

## Cascading Actions in Referential Integrity
>   * 참조 무결성 제약 조건이 위반되면 일반적인 절차를 위반을 초래한 작업을 거부하는 것
>   * 삭제 또는 업데이트의 경우 대안은 cascade로 배열하는 것
>       ```
>       CREATE TABLE course (
>           ...
>           dept_name varchar(20),
>           foreign key (dept_name) references department,
>               on delete cascade,
>               on update cascade,
>            ...
>       )
>       ```
>   * cascade 대신 사용
>       * set null
>       * set default

## Built-in Data Types in SQL (SQL에 내장된 데이터 유형)
>   * date
>       * Example: date '2005-7-27
>   * time
>       * time '09:00:30' time '09:00:30.75'
>   * timestamp
>       * Example: timestamp '2005-7-27 09:00:30.75'
>   * interval
>       * Example: interval '1' day
>       * 날짜/시간/타임스탬프 값을 다른 값에서 빼면 간격 값이 제공됨
>       * 날짜/시간/타임스탬프 값에 간격 값을 추가할 수 있다.

## Large-Object Types
>   * 대형 개체(사진, 비디오, CAD 파일 등)는큰 물체:
>       * blob: 이진 대형 객체(Binary Large Object) - 객체는 해석되지 않은 이진 데이터의 대규모 모음입니다(해석은 데이터베이스 시스템 외부의 응용 프로그램에 맡겨짐).
>       * clob: 문자 대형 객체 - 객체는 문자 데이터의 대규모 모음입니다.
>   * 쿼리가 대형 개체를 반환하면 대형 개체 자체가 아닌 포인터를 반환

## Index Creation
>   * 많은 쿼리는 테이블에 있는 레코드 중 작은 부분만 참조
>   * 시스템이 특정 값을 가진 레코드를 찾기 위해 모든 레코드를 읽는 것은 비효율적이다.
>   * Index 관계의 속성에 대한 데이터 구조는 데이터베이스 시스템이 관계의 모든 튜플을 검색하지 않고도 해당 속성에 대해 지정된 값을 갖는 관계에서 해당 튜플을 효율적으로 찾을 수 있도록 하는 데이터 구조
>   * create index name on relation-name (attribute);
>       ![image-51](https://github.com/joey0919/database-system/assets/87600842/cf47e981-48aa-4cc7-9988-1ec3ecfe5fb8)
