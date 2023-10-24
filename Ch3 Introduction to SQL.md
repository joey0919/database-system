# Ch3 Introduction to SQL 

## DDL Types in SQL
>   * char(n): 사용자가 지정한 길이의 고정 길이 문자열 n
>   * varchar(n): 사용자가 지정한 최대 길이를 갖는 가변 길이 문자열 n
>   * int: 정수(기계에 따라 달라지는 정수의 유한 하위 집합)
>   * smallint: 작은 정수
>   * numeric(p, d): 사용자가 지정한 정밀도를 갖는 고정점 수 p, 소수점 오른쪽 자리 d
>   * real, double precision: 기계에 따라 정밀도가 달라지는 부동 소수점 및 배정밀도 부동 소수점 숫자
>   * float(n): 사용자가 지정한 정밀도 이상의 부동 소수점 숫자 n

## Create Table Construct
>   * SQL create table
>       ```
>       CREATE TABLE r (
>           ID          char(5),
>           name        varchar(20),
>           dept_name   varchar(20),
>           salary      numeric(8,2)
>       )
>       ```

## Integrity Constraints in Create Table (테이블 생성의 무결성 제약 조건)
>   * Types of integrity constrains
>       * primary key ()
>       * foreign key () references r
>       * not null
>   * SQL은 무결성 제약 조건을 위반하는 데이터베이스 업데이트 방지한다.
>       ```
>       CREATE TABLE instructor (
>           ID          char(5),
>           name        varchar(20) not null,
>           dept_name   varchar(20),
>           salary      numeric(8,2),
>           primary     key(ID)
>           foreign key (dept_name) references department
>       );
>       ```
>       ```
>       CREATE TABLE student (
>           ID          varchar(5),
>           name        varchar(20), not null,
>           dept_name   varchar(20),
>           tot_cred    numeric(3, 0),
>           primary key (ID),
>           foreign key (dept_name) references department
>       );
>       ```
>       ```
>       CREATE TABLE takes (
>           ID          varchar(5),
>           course_id   varchar(8),
>           sec_id      varchar(8),
>           semester    varchar(6),
>           year        numeric(4, 0),
>           grade       varchar(2),
>           primary key (ID, course_id, sec_id, semester, year),
>           foreign key (ID) references student,
>           foreign key (course_id, sec_id, semester, year) references section
>       );
>       ```

## Updates to tables
>   * Drop Table
>       * Drop Table r
>   * Alter
>       * Alter Table r add A D
>       * Alter Table r drop A

## Basic Query Structure
>   ![image-36](https://github.com/joey0919/database-system/assets/87600842/a658348c-a781-49d3-923b-d2feb3ef0430)

## The select Clause
>   * 모든 강사의 학과명을 찾아 중복 제거
>       ```
>       SELECT DISTINCT dept_name
>       FROM instructor
>       ```
>   * 중복을 제거하지 않고 모두 지정
>       ```
>       SELECT all dept_name
>       from instructor
>       ```
>   * SELECT 절의 별표는 "모든 속성"을 나타낸다
>       ```
>       SELECT *
>       FROM instructor
>       ```
>   * 산술식 포함
>       ```
>       SELECT ID, name, salary/12
>       FROM instructor
>       ```
>   * 컬럼 이름 변경하여 출력
>       ```
>       SELECT ID, name, salary/12 as monthly_salary
>       FROM instructor
>       ```

## Where 절
>   표현
>   ```
>   SELECT name
>   FROM instructor
>   WHERE dept_name = 'Comp. Sci.'
>   ```
> 논리 연결의 피연산자는 비교 연산자 <, <= ,> ,>= ,= 및 <>를 포함하는 표현식일 수 있다.
>   ```
>   SELECT name
>   FROM instructor
>   WHERE dept_name = 'Comp. Sci.' and salary > 80000
>   ```

## FROM 절
>   * 데카르트 곱 찾기 instructor X teaches
>       ```
>       SELECT *
>       FROM instructor, teaches
>       ```

## Joins
>   * 특정 강좌를 가르친 모든 강사의 이름과 강좌 ID를 찾는다.
>       ```
>       SELECT name, course_id
>       FROM instructor, teaches
>       WHERE instrucor.ID = teaches.ID
>       ```
>   * 미술부에서 어떤 과목을 가르쳤던 모든 강사의 이름과 Course_id를 찾는다.
>       ```
>       SELECT name, course_id
>       FROM instructor, teaches
>       WHERE instructor.ID = teaches.ID and instructor.dept_name = 'Art'

## The Rename Operation
>   * SQL에서는 다음을 사용하여 관계 및 속성의 이름을 바꿀 수 있다.
>       ```
>       old-name as new-name
>       ```
>   * 'Comp. Sci'이름을 가진 강사보다 연봉이 높은 강사를 찾는다.
>       ```
>       SELECT distinct T.name
>       FROM instructor as T, instructor as S
>       WHERE T.salary > S.salary and S.dept_name = 'Comp. Sci.'
>       ```

## Self Join Example
>   * 예제
>
>       ![image-37](https://github.com/joey0919/database-system/assets/87600842/c641a1fd-9e62-4410-a2ac-750e0649edd5)
>   *  Find the supervisor of “Bob”
>   *  Find the supervisor of the supervisor of “Bob”
>   *  Can you find ALL the supervisors (direct and indirect) of “Bob”?
>   ```
>   SELECT supervisor 
>   FROM emp-super
>   WHERE person = 'Bob'
>   ```
>   ```
>   SELECT supervisor
>   FROM emp-super A, emp-super B
>   WHERE A.supervisor = B.person
>   and A.person = 'Bob'
>   ```   

## String Operations
>   * %: % 문자는 모든 하위 문자열과 일치
>   * _: _문자는 모든 문자와 일치
>       ```
>       SELECT name
>       FROM instructor
>       WHERE name like '%dar%'
>   * \: escape 문자
>   * ||: 연결 사용
>   * upper to lower case (and vice versa)
>   * 문자열 길이 찾기, 하위 문자열 추출 등

## Ordering the Display of Tuples
>   * 모든 강사의 이름을 알파벳 순으로 나열
>       ``` 
>       SELECT DISTINCT name
>       FROM instructor
>       ORDER BY name
>       ``` 
>   * 내림차순
>       ``` 
>       SELECT DISTINCT name
>       FROM instructor
>       ORDER BY name desc
>       ``` 

## Where Clause Predicates
>   * SQL에는 between 연산자가 있다
>       ```
>       SELECT name
>       FROM instructor, teaches
>       WHERE salary between 90000 and 100000
>       ``` 
>   * Tuple
>       ``` 
>       SELECT name, course_id
>       FROM instructor, teaches
>       WHERE (instructor.ID, dept_name) = (teaches.ID, 'Biology')
>       ``` 

## Set Operations
>   * union
>   * intersect
>   * except
>   * union all
>   * intersect all
>   * except all

## Null Values
>   * 예:
>       * 5 + null returns null
>       ``` 
>       SELECT name
>       FROM instructor
>       WHERE salary is null
>       ``` 
>       ``` 
>       SELECT name
>       FROM instructor
>       WHERE salary is not null
>       ``` 

## Aggregate Functions
>   * avg
>   * min
>   * max
>   * sum
>   * count

## Aggregate Function Examples
>   ``` 
>   SELECT avg(salary)
>   FROM instructor
>   WHERE dept_name = 'Comp. Sci.';
>   ``` 
>   ``` 
>   SELECT count(distinct ID)
>   FROM teaches
>   WHERE semester = 'Spring' and year = 2018;
>   ``` 
>   ``` 
>   SELECT count(*)
>   FROM course;
>   ``` 

## Group By
>   ``` 
>   SELECT dept_name, avg(salary) as avg_salary
>   FROM instructor
>   GROUP by dept_name;
>   ``` 

## Having
>   ``` 
>   SELECT dept_name, avg(salary) as avg_salary
>   FROM instructor
>   GROUP BY dept_name
>   having avg(salary) > 42000;
>   ``` 

## Set Membership
>   * 2017년 가을과 2018년 봄에 제공되는 강좌를 찾아라
>       ``` 
>       SELECT distinct course_id
>       FROM section
>       WHERE semester = 'Fall' and year = 2017 and
>       course_id in (SELECT course id 
>                     FROM section
>                     WHERE semester = 'Spring' and year = 2018);
>       ``` 
>   * 2017년 가을에는 제공되지만 2018년 봄에는 제공되지 않는 강좌 찾기
>       ``` 
>       SELECT distinct course_id
>       FROM section
>       WHERE semester 'Fall' and year = 2017 and
>       course_id not in (SELECT course_id
>                         FROM section
>                         WHERE semester = 'Spring' and year = 2018);
>        ``` 
>   * 이름이 "Mozart"도 "Eistein" 이 아닌 모든 강사의 이름을 지정하라
>       ``` 
>       SELECT DISTINCT name
>       FROM instructor
>       WHERE name not in ("Mozart", "Eistein")
>       ``` 
>   * 강사가 가르치는 과목을 이수한 학생의 총 수를 구한다.
>       ``` 
>       SELECT count(distinct ID)
>       FROM takes
>       WHERE (course_id, sec_id, semester, year) in
>               (SELECT course_id, semester, year
>                FROM teaches
>                WHERE teaches.ID = '10101');
>       ``` 

## Set Comparison - "some"
>   * 생물학과의 일부 강사보다 급여가 높은 강사의 이름을 찾아라
>       ``` 
>       SELECT distinct T.name
>       FROM instructor as T, instructor as S
>       WHERE T.salary > S.salary and S.dept_name = 'Biology'
>       ```
>   * some을 사용하는 동일한 쿼리
>       ```
>       SELECT name
>       FROM instructor
>       WHERE salary > some (SELECT salary
>                            FROM instructor
>                            Name dept name = 'Biology')
>       ```

## Set Comparison - "all"
>   * 생물학과의 모든 강사의 급여보다 급여가 더 많은 강사의 이름을 찾아보시오.
>       ```
>       SELECT name
>       FROM instructor
>       WHERE salary > all (SELECT salary
>                           FROM instructor
>                           WHERE dept_name = 'Biology')
>       ```

## EXISTS
>   * 생물학과에서 제공되는 모든 과목을 수강한 모든 학생을 찾는다.
>       ```
>       SELECT course_id
>       FROM section as S
>       WHERE semester = 'Fall' and year = 2017 and
>       exists (SELECT *
>               FROM section as T
>               WHERE semester = 'Spring' and year = 2018
>               AND S.course_id = T.course_id);
>       ```

## unique
>   * 2017년에 최대 1회 제공되었던 강좌를 모두 찾아보시오
>       ```
>       SELECT T.course_id
>       FROM course as T
>       WHERE unique (SELECT R.course_id
>                     FROM section as R
>                     WHERE T.course_id = R.course_id
>                     AND R.year = 2017)
>       ```

## Subqueries in the From Clause
>   * 평균 급여가 $42,000보다 큰 부서의 평균 강사 급여를 찾아보시오.
>       ```
>       SELECT dept_name, avg_salary
>       FROM (SELECT dept_name, avg(salary) as avg_salary
>             FROM instructor
>             GROUP BY dept_name)
>       WHERE avg_salary > 42000
>   * 위 쿼리를 작성하는 또 다른 방법
>      ```
>      SELECT dept_name, avg_salary
>      FROM (SELECT dept_name, avg(salary)
>            FROM instructor
>            GROUP BY dept_name)
>            AS dept_avg (dept_name, avg_salary)
>       WHERE avg_salary > 42000

## With Clause
>   * 예산이 최대인 부서를 모두 찾아보시오
>       ```
>       WITH max_budget (value) as
>       (SELECT MAX(budget)
>       FROM department)
>       SELECT department.name
>       FROM department, max_budget
>       WHERE department.budget = max_budget.value
>       ```

## Complex Queries using With Clause
>   * 전체 급여가 전체 부서의 총 급여 평균보다 큰 부서를 모두 찾아보시오
>       ```
>       WITH dept_total(dept_name, value) as
>       (SELECT dept_name, SUM(salary)
>       FROM instructor
>       GROUP BY dept_name),
>       dept_total_avg(value) as
>       (SELECT avg(value)
>       FROM dept_total)
>
>       SELECT dept_name
>       FROM dept_total, dept_total_avg
>       WHERE dept_total_value > dept_total_avg.value
>       ```

## Scalar Subquery
>   * 각 학과의 강사 수와 함께 모든 학과를 나열
>       ```
>       SELECT dept_name,
>           (SELECT count(*)
>           FROM instructor
>           WHERE department.dept_name = instructor.dept_name)
>           AS num_instructors
>       FROM department
>       ```

## Delete
>   * 모든 강사 삭제
>       ```
>       DELETE FROM instructor
>       ```
>   * 재무 부서의 모든 강사 삭제
>       ```
>       DELETE FROM instructor
>       WHERE dept_name = 'Finance'
>       ```
>   * Watson 건물에 위치한 부서와 연관된 강사에 대한 강사 관계의 모든 튜플을 삭제
>       ```
>       DELETE FROM instructor
>       WHERE dept_name in (SELECT dept_name
>                           FROM department
>                           WHERE building = 'Watson')
>       ```
>   * 강사 평균 급여보다 급여가 적은 강사 삭제
>       ```
>       DELETE FROM instructor
>       WHERE salary > (SELECT AVG(salary)
>                       FROM instructor)
>       ```
>   * 문제: 튜플을 삭제할 때 강사, 평균 급여가 변경된다.
>   * 삭제할 모든 튜플을 찾는다.
>   * 다음으로 위에서 찾은 모든 튜플을 삭제한다.

## Insertion
>   * 추가
>       ```
>       INSERT INTO course
>       VALUSE ('CS-437', 'Database Systems', 'Comp. Sci.', 4)
>       ```
>   * equal
>      ```
>      INSERT INTO course (course_id, title, dept_name, credits)
>      VALUES ('CS-437', 'Database Systems', 'Comp. Sci.', 4)
>      ```
>   * 144 학점 이상을 취득한 음악학과의 각 학생의 급여 $18,000의 음악학과 강사로 만드십시오.
>       ```
>       INSERT INTO instructor
>           SELECT ID, name, dept_name, 18000
>           FROM student
>           WHERE dept_name = 'Music' AND total_cred > 144
>       ```

## Update
>   * 모든 강사에게 급여 5% 인상
>       ```
>       UPDATE instructor
>       SET salary = salary*1.05
>       ```
>   * 연봉 7만 미만 강사 연봉 5% 인상
>       ```
>       UPDATE instructor
>       SET salary = salary*1.05
>       WHERE salary < 70000
>       ```
>   * 급여가 평균보다 적은 강사에게 급여를 5% 인상
>       ```
>       UPDATE instructor
>       SET salary = salary*1.05
>       WHERE salary < (SELECT AVG(salary) FROM instructor)
>       ```
>   * 급여가 $100,000 이상인 강사의 급여를 3% 인상하고, 기타 모든 강사의 급여를 5% 인상
>       ```
>       UPDATE instructor
>       SET salary = salary*1.03
>       WHERE salary <= 100000;
>
>       UPDATE instructor
>       SET salary = salary*1.05
>       WHERE salary > 100000
>       ```

## Case Statement for Conditional Updates
>   * 이전과 똑같은 쿼리
>       ```
>       UPDATE instructor
>       SET salary = CASE 
>                       WHEN salary <= 100000 THEN salary*1.05
>                       ELSE salary*1.03
>                    END
>      ```

## Update with Scalar Subqueries
>   * 모든 학생의 tot_creds 값을 다시 계산하고 업데이트 한다.
>       ```
>       UPDATE student S
>       SET tot_cred = (select sum(credits)
>                       FROM takes, course
>                       WHERE takes.course_id = course.course_id and
>                       takes.grade <> 'F' and
>                       takes.grade is not null);
?       ```

