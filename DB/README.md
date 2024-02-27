[노션으로 보기](https://cumbersome-juniper-780.notion.site/1-ac0e644f027343f5823719fc7132f5c6?pvs=4)

## 1주차 이슈

<details><summary>접기/펼치기</summary>

### DB를 설계할 때 가장 중요한 것에 대해 설명해주세요

<details><summary>답변</summary>
    
#### 데이터 무결성을 보장해야 합니다.

- 무결성 : 데이터의 정확성, 일관성, 유효성이 유지되는 것을 의미합니다.
- 정확성 : 데이터의 중복이나 누락이 없는 상태
- 일관성 : 데이터가 변하지 않는 상태를 보장

*데이터베이스에 데이터 무결성을 설계하지 않는다면, 테이블에 중복된 데이터가 존재하고 부모와 자식 데이터 간의 관계가 깨지고 잦은 에러와 재개발 비용 발생 등과 같은 문제가 발생할 수 있다.*

#### 데이터 무결성 제약조건 종류

- 개체 무결성 :
    - **기본 키**에 관련된 무결성 / 기본키 : 레코드를 구별하기 위해 후보키 중에 선택된 식별자 키
    - 테이블은 기본 키를 지정하고 그에따른 무결성 원칙을 지켜야한다.
    - 기본 키는 Null이 올 수 없다.
    - 하나의 테이블에 동일한 기본 키를 갖을 수 없다.
- 참조 무결성 :
    - **외래 키**에 관련된 무결성 / 외래키 : 한테이블의 키 중에서 다른 테이블의 레코드를 유일하게 식별할 수 있는 키
    - 외래키는 Null이거나 참조 릴레이션의 기본키 값과 동일해야 한다.
    - 외래 키 속성은 참조할 수 없는 값을 지닐 수 없다 -> 외래키 속성 값이 상위 테이블의 인스턴스에 반드시 존재하거나 Null 이어야한다.
- 도메인 무결성 :
    - 필드의 무결성을 보장하기 위함
    - 필드의 타입과 관련한 사항을 정의하고 올바른 데이터가 입력되었는지 확인
- Null 무결성 :
    - 특정 속성 값이 Null이 될 수 없게 하는 조건
- 고유 무결성
    - 테이블의 특정 속성에 대해 각 레코드들이 갖는 값들이 서로 달라야 하는 조건
- 키 무결성 :
    - 하나의 테이블에는 적어도 하나의 키가 존재해야 하는 조건
- 관계 무결성 :
    - 테이블에 한 레코드의 삽입 가능 여부 확인하는 조건다른 테이블과의 관계에 대한 적절성 여부를 지정한 조건

#### 무결성 제약조건의 장단점?

#### 무결성 보장 방법은?

</details>

--- 

### DB의 TCL에 대해 설명해주세요

<details><summary>답변</summary>

#### Transaction Control Language

- COMMIT / SAVEPOINT / ROLLBACK / TRUNCATE

#### COMMIT

- 하나의 트랜잭션을 물리적인 데이터베이스에 적용하는 작업으로, 마지막 COMMIT 시점 이후 실행한 트랜잭션 결과를 데이터베이스에 영구적으로 저장한다.
- 마지막으로 COMMIT한 시점까지만 ROLLBACK이 가능하다.
    - ex) COMMIT;

#### SAVEPOINT

- SAVEPOINT를 지정하면, ROLLBACK시 해당 지정 위치로 복원이 가능하여 SAVEPOINT 명령어로 지점을 지정, ROLLBACK 명령어로 복원한다.
    - ex) SAVEPOINT SV1;

#### ROLLBACK

- 데이터의 수정, 삭제, 삽입 등의 작업을 한 후 원래의 상태로 되돌릴 때 사용하는 작업이다.
- COMMIT 이후에는 ROLLBACK하더라도 이전으로 돌아갈 수 없다.
    - ex) ROLLBACK;
    - ex) ROLLBACK TO SV1;

#### TRUNCATE (DDL)

- 지정된 테이블의 모든 데이터를 삭제한다.
- 자동으로 COMMIT 되므로 ROLLBACK 할 수 없다.
    - ex) TRUNCATE TABLE table_name;

</details>

---

### Delete, Truncate, Drop의 차이점에 대해 설명해주세요

<details><summary>답변</summary>

#### DELETE

- WHERE 절을 사용하여 테이블에 있는 데이터를 하나하나 선택하여 제거
- WHERE 절을 사용하지 않더라도 내부적으로 일일히 하나씩 제거
- 속도가 늦어서 원하는 데이터만 삭제할 때 사용
- COMMIT 전이라면 ROLLBACK 명령어를 통해 되돌릴 수 있음

#### TRUNCATE

- 전체 데이터를 한 번에 삭제하는 방식
- 자동 COMMIT이 되는 명령어 이므로, 지운 후엔 되돌릴 수 없다.
- TABLE을 CREATE TABLE한 시점과 같이 되돌림.

#### DROP

- 인덱스를 포함한 테이블 자체를 완전히 제거
- 자동 COMMIT이 되는 명령어 이므로, 지운 후엔 되돌릴 수 없다.

</details>

---

### 테이블 파티셔닝이 필요한 이유와 범위에 대해 설명해주세요

<details><summary>답변</summary>

- 데이터가 일정 규모를 넘어서면 인덱스 조정이나 쿼리 변경만으로는 한계가 있고, 적절한 인덱스가 생성되었다고 해도 탐색의 효율성이 떨어져 시간이 오래 걸립니다.
- 이 때, 파티션을 적용하여 테이블을 파티션이라는 단위로 나누어 관리하게 된다면 쿼리를 튜닝하지 않고도 성능을 끌어올릴 수 있습니다.

### 장점

- 가용성 : 파티셔닝을 통해 전체 데이터 훼손 가능성이 줄어듬.
- 관리용이성 : 큰 테이블들을 제거
- 성능 : 특정 DML과 Query에 대한 성능 향상, 대용량 Data일수록 효율적임.

### 단점

- Table 간의 Join 비용이 증가
- 테이블과 인덱스를 별도로 파티션이 불가능하다.

### 파티셔닝의 범위

1. Range Partitioning (범위 분할)
    - 분할 키 값이 범위 내에 있는지 여부로 구분한다.
    - 가장 단순하고, 시간이 단축되지만, 특정 파티션에 데이터가 몰릴 수 있다.
2. Hash Partitioning (해시 분할)
    - 해시 함수의 값에 따라 파티션에 포함할 지 여부를 결정한다.
    - 성능 향상이 주 목적이다.
    - 특정 데이터가 어느 파티션에 있는지 판단이 불가능하다.
3. List Partitioning (리스트 분할)
    - 특정 컬럼의 특정 값을 기준으로 파티셔닝을 하는 방식이다.
        - ex) 아시아 - [한국, 중국, 일본]
4. Composite Partitioning (합성 분할)
    - 여러 기법을 결합해 사용한다.

</details>

---

### 조인에 대해 설명해주세요

<details><summary>답변</summary>

#### 조인이란?

- 두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법
- 테이블을 연결하기 위해서는 적어도 하나의 칼럼을 서로 공유하고 있어야 하므로, 이를 이용하여 데이터 검색에 활용한다.

#### 조인의 종류

- INNER JOIN
    - 교집합 : 두 테이블에서 일치하는 값을 가진 행을 반환한다.
- LEFT OUTER JOIN
    - 왼쪽 테이블의 모든 행과 오른쪽 테이블의 일치하는 행을 반환한다. 일치하는 항목이 없으면 오른쪽 테이블의 열에 대해 null 값을 반환한다.
- RIGHT OUTER JOIN
    - 오른쪽 테이블의 모든 행과 왼쪽 테이블의 일치하는 행을 반환한다. 일치하는 항목이 없으면 왼쪽 테이블의 열에 대해 null 값을 반환한다.
- FULL OUTER JOIN
    - 합집합 : 두 테이블의 모든 행을 반환한다. 일치하는 항목이 없으면 일치하지 않는 테이블의 열에 대해 null 값을 반환한다.
- CROSS JOIN
    - 데카르트 JOIN : 모든 경우의 수를 전부 표현해주는 방식이다. A가 3개이고 B가 4개이면 총 12개의 데이터가 검색된다.
- SELF JOIN
    - 자기자신과 자기자신을 조인하는 것이다.
    - 자신이 갖고 있는 칼럼을 다양하게 변형시켜 활용할 때 자주 사용한다.

</details>

---

### 관계형 DB와 비관계형 DB에 대해 설명해주세요

<details><summary>답변</summary>

### SQL vs NoSQL

- SQL : Structured Query Language의 줄임말로, 구조화된 쿼리 언어

### 1. SQL (관계형 DB)

#### (1) 예시

- MySQL

#### (2) 데이터 저장

- 데이터를 테이블에 레코드로 저장한다.
- 데이터를 정해진 스키마에 따라 테이블에 저장한다. 정해진 스키마를 따르지 않으면 레코드를 테이블에 추가할 수 없다.
- 각 테이블마다 명확하게 정의된 구조가 있다. 해당 구조는 필드 이름과 데이터 유형으로 정의된다.
- 관계를 통해 데이터를 여러 테이블에 분산하여 저장한다. 관계를 활용하여 데이터의 중복을 방지한다.

#### (3) 쿼리

- 테이블의 형식과 테이블 간의 관계에 맞춰서 데이터를 요청해야 한다.
- 따라서 정보 요청 시 SQL과 같이 구조화된 쿼리 언어를 사용한다.

#### (4) 확장성

- 데이터 저장 방식으로 인해 일반적으로 수직적 확장만 가능하다.
- 수직적 확장 : 단순히 데이터베이스 서버의 성능을 향상시키는 것 (예를 들어, CPU를 업그레이드)

#### (5) 장점

- 스키마가 명확하게 정의되어 데이터 무결성이 보장된다.
- 관계를 통해 각 데이터를 중복없이 한 번만 저장할 수 있다.

#### (6) 단점

- 엄격한 스키마를 따라야 하므로 덜 유연하다. 데이터 스키마를 사전에 계획하고 알려야 하며, 나중에 수정하기 힘들다.
- 관계를 맺고 있어서 조인문이 많은 복잡한 쿼리가 만들어질 수 있다.

### 2. NoSQL (비관계형 DB)

#### (1) 예시

- MongoDB

#### (2) 데이터 저장

- 레코드를 문서(document)라고 부른다. 문서를 json과 비슷한 형태로 가지고 있다.
- 스키마가 없다. 즉, 다른 구조의 데이터를 같은 컬렉션에 추가할 수 있다.
- 관계가 없다. 데이터를 여러 테이블에 나누어담지 않고, 관련된 데이터를 동일한 컬렉션에 함께 넣는다.
- 이렇게 하면 데이터가 중복되어 서로 영향을 줄 위험이 있다. 따라서 조인을 잘 사용하지 않고 자주 변경되지 않는 데이터인 경우에 NoSQL을 사용하면 효율적이다.

#### (3) 쿼리

- 데이터 그룹 자체를 조회하는 것에 초점을 두고 있다.
- 따라서 구조화되지 않은 쿼리 언어로도 데이터 요청이 가능하다.

#### (4) 확장성

- 수직적 및 수평적 확장이 모두 가능하다. 따라서 애플리케이션이 발생시키는 모든 읽기/쓰기 요청을 처리할 수 있다.
- 일반적으로 수평적으로 확장한다.
- 수평적 확장 : 더 많은 서버를 추가하고 데이터베이스를 전체적으로 분산시키는 것, 따라서 하나의 데이터베이스에서 작동하지만 여러 호스트에서 작동하게 된다.

#### (5) 장점

- 스키마가 없으므로 유연하다. 언제든지 저장된 데이터를 조정하고 새로운 필드를 추가할 수 있다.
- 데이터는 애플리케이션이 필요로 하는 형식으로 저장된다. 따라서 데이터를 읽어오는 속도가 빨라진다.

#### (6) 단점

- 유연성으로 인해 데이터 구조 결정을 미루게 될 수 있다.
- 데이터 중복을 계속 업데이트 해야한다.
- 데이터가 여러 컬렉션에 중복되어 있기 때문에, 수정 시 모든 컬렉션에서 수행해야 한다.

</details>

---

### 데이터베이스를 공격하는 해킹 기법은 뭐라고 하나요?

<details><summary>답변</summary>

#### SQL Injection ( SQL 삽입 공격 )

- 코드 주입 공격 기법에서 파생된 해킹 기법, 비교적 쉬운 공격 난이도에 비해 파급력이 치명적인 공격 중 하나
- 데이터베이스 시스템에 비정상적인 SQL 쿼리를 주입하여, 보안상 취약점을 이용해 데이터베이스를 조작하는 공격 기법
- 테스트를 통한 취약점 판별은 어렵다. 하지만 스캐닝 툴 & 코드 검증절차를 통해 취약점을 빠르게 판별 가능하다.

Ex)

1. INSERT INTO Students (이름) VALUES (”PUT NAME”);
2. PUT NAME 자리에 위조 코드 "Robert'); DROP TABLE Students;--” 라는 학생 이름을 넣는다.
3. INSERT INTO Students (이름) VALUES ('Robert'); DROP TABLE Students;—”);
4. 두 번째 줄에서 학생들의 데이터가 있는 테이블을 제거한다. 그리고 세 번째에서는 뒤에 오는 내용을 모두 주석 처리한다.
5. 이 명령문이 실행되면 최종적으로 모든 학생 기록이 삭제된다.

</details>

---

### 정규화 과정과 역 정규화에 대해 설명해주세요

<details><summary>답변</summary>
    
#### 정규화(Nomalization)

- RDBMS에서 중복을 최소화하도록 데이터를 구조화하는 프로세스를 말한다.

**기본형**

- 제 1 정규화 : 모든 속성의 도메인이 분해되지 않은 원자값으로만 이루어지도록 분해
- 제 2 정규화 : 기본키가 아닌 모든 속성이 기본키에 **완전 함수 종속**되도록 **부분 함수 종속**을 제거
- 제 3 정규화 : **이행적 함수 종속**
- BCNF(보이스 / 코드 정규화) : 함수 종속 관계에서 모든 결정자가 후보키인 경우

**고급형**

- 제 4 정규화 : 다치 종속 제거
- 제 5 정규화 : 조인 종속 제거

#### 장점

- 이상 현상이 발생하는 경우를 줄임

#### 단점

- 정규화 과정에서 여러 테이블로 나뉘어 연산 속도가 느려짐

#### 역정규화

- 정규화 과정에서 오히려 연산 속도가 느려지는 것을 개선하기 위해 중복을 허용하거나 분리된 데이터들을 합쳐 DB 성능을 향상 시킬 수 있게 구조화하는 것

</details>

---

### ORM vs SQL Mapper에 대해 설명해주세요

<details><summary>답변</summary>
    
### ORM

- Object Relational Mapping (객체 관계 매핑)
- Object와 DB 테이블을 매핑하여 데이터를 객체화하는 기술
- DBMS에 종속적이지 않고, 사용하고 있는 개발 언어에 더 가까운 모습을 보인다.
    - ex) JPA, Django ORM(Python)

#### 장점

- 객체지향 코드 작성 가능
- 유지보수성 증가
- DBMS에 대해 독립적

#### 단점

- 높은 학습 곡선
- 복잡한 쿼리 해결이 어려움

### SQL Mapper

- Object와 SQL의 필드를 매핑하여 데이터를 객체화하는 기술
- 객체와 테이블 간의 관계를 매핑하는 것이 아니다
- SQL문을 직접 작성하고, 쿼리 수행 결과를 어떠한 객체에 매핑할지 바인딩하는 방법
    - ex) SpringJdbcTemplate, MyBatis

#### 장점

- SQL 작성에 제약이 없음
- 다양한 테이블 조합 가능
- 학습이 쉬움

#### 단점

- 패러다임 불일치를 개발자가 해결해야함
- SQL 관련 유지보수성 낮음
- DBMS 종속적

jdbc → SQL Mapper → ORM의 형태로 발전하였음

jdbc는 표준 API이며, 데이터베이스 트랜잭션 관리 작업을 수행함.
    
</details>

---

### Transaction에 대해 설명해주세요

<details><summary>답변</summary>

#### Transaction

- DB의 상태변화를 발생시키는 작업 단위
    - ex) 새로운 데이터 추가, 삭제를 통한 갱신
- SQL 질의어( SELECT, INSERT, DELETE, UPDATE ) 조합 → 데이터베이스 조작
    - 단, 작업의 단위는 1개가 아니라, 개발자가 설정한 SQL 질의 명령문들의 최적화된 조합

#### 작업 단위란?

- 작업의 단위 : Select + Insert , DB 상태변화를 발생시키는 최소 SQL문 집합
- ex)
    - 이용자가 게시글을 작성 후, 올리기 버튼을 눌렀다 → 게시글 데이터를 Insert 문으로 DB로 옮긴다.
    - 이용자는 최신글이 포함된 게시판을 본다 → 게시판을 최신 데이터로 Select 문으로 갱신한다.

</details>

---

### SQL 쿼리문의 실행 순서에 대해 설명하세요

<details><summary>답변</summary>

#### SQL 쿼리문의 실행 순서에 대해서 설명하세요

- 쿼리의 종류에는 SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, LIMIT 등이 있습니다. 이 때 쿼리의 실행 순서는 아래와 같습니다.
1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY
7. LIMIT
- 때문에 FROM 절에서 지정한 Table의 alias는 다른 모든 쿼리에서 사용이 가능하지만 Select 절에서 지정한 컬럼의 alias는 where 절에서 사용이 불가능합니다.
    - (예외, MySQL에서는 GROUP BY, HAVING에서 사용이 가능하도록 DBMS에서 알아서 해준다고 합니다.)

</details>

---

### **SQL 쿼리문에서 truncate 실제 사용 예시나 경험을 설명하세요**

<details><summary>답변</summary>

- Spring Boot 통합 테스트의 경우 실제 컨테이너를 올려서 실행되기 때문에 테스트의 기록이 데이터베이스에 남게 됩니다.
- 이때 키값을 자동 증가 값으로 설정할 경우 매번 테스트마다 키값이 증가하면서 테스트의 일관성이 깨질 수 있습니다.
- 이를 해결하기 위해 truncate를 사용하여 테스트의 일관성을 유지할 수 있습니다

</details>

---

### **ERD는 어떤 데이터 모델링 단계에 속하는지 말해주세요**
<details><summary>답변</summary>
    
# ERD란?

> 우리는 DB를 설계하기위해서 데이터 모델링이라는 과정을 거치게됩니다. 데이터 모델링이란 업무내용을 분석하고 약속된 표기법에 의해 표현하는 것을 의미합니다.
> 
> 
> 데이터 모델링 총 4가지의 절차로 이루어집니다.
> 
1. 업무파악(요구사항 수집 및 분석)
2. 필요한 데이터를 파악해서 관계를 설정하는 **개념적 데이터 모델링** : 추상적인 데이터( E-R Model)
    1. E-R 모델을 E-R Diagram으로 표현
3. 개념적 모델링을 표로 만들어내는 **논리적 데이터 모델링** : 구체적인 데이터
4. 이러한 과정을 거친 것을 실제 DB 테이블로 만들어내는 **물리적 데이터 모델링**

**이러한 과정에서 ERD는 논리적 데이터 모델링에 속하게됩니다.**

*추가적으로 DBMS는 2->3 과정으로 넘어갈때 선정한다고 합니다.*

ERD : Entity Relationship Diagram 으로 개체와 관계를 중심으로 표현해서 데이터베이스 구조를 한 눈에 쉽게 알아 보기위해 그려놓은 다이어그램을 의미합니다.

*다이어그램이란, 기호, 선 점 등을 이용해서 상호관계나 구조 등을 이해시키위한 그림을 의미합니다.*

</details>

---

### **ERD의 관계선 표기 방식에 대해 설명해주세요**
<details><summary>답변</summary>
    
### ERD : Entity Relation Diagram

- ERD의 표시방식은 점선과 실선이 존재

#### 실선(Identifying)

- A테이블과 B테이블이 식별관계임을 나타내며, 부모테이블[A 테이블]의 PK가 외래키로 자식테이블[B 테이블]의 PK에 포함되는 경우이다.
    - 즉, 부모-자식 관계로 볼 수 있다 (부모 테이블이 있어야 자식이 생긴다)

#### 점선(Non-Identifying)

- A테이블과 B테이블이 비식별관계임을 나타내며, 부모테이블[A 테이블]의 PK가 외래키로써, 자식테이블[B테이블]의 PK가 아닌 일반 속성이 되는 경우이다.
    - 즉, 부모, 자식 관계가 아닌 모든 경우로 볼 수 있다 (부모 테이블이 없어도 자식이 생기는 경우)

#### 0, 1, *

- 0 : 0개를 나타낸다.
- 1 : 1개를 나타낸다.
- ..* : n개 이상 존재할 수 있음을 나타낸다.

#### 관계차수 표현

- ㅡㅡO| : 0개 또는 1개
- ㅡㅡ∈ : 1개 이상
- ㅡㅡ|∈ : 1개 또는 1개 이상
- ㅡㅡO|∈ : 0개 또는 1개 또는 1개 이상

</details>

---

### **Cardinality에 대해서 자세히 설명해주세요**
<details><summary>답변</summary>

#### Cardinality가 무엇인지 설명해주세요.

- 관계 차수는 1:1, 1:N, N:M 등 하나의 개체에 몇 개의 개체가 대응되는지를 표현하는 것을 말한다. 즉, 테이블 간의 수적 관계를 명시하는 표현이다.
- 관계가 존재하는 두 엔티티 사이에 한 엔티티가 다른 엔티티 몇 개와 대응되는지 제약조건을 선을 그어서 표기한다.
- Cardinality는 한 개체에서 발생할 수 있는 발생 횟수를 정의하며, 다른 개체에서 발생할 수 있는 발생 횟수와 연관된다.

#### 관계에 어떤 종류가 있는지 예시와 함께 설명해주세요.

- (1) One-to-One Cardinality (1:1 관계)
    - 예를 들어, 학생 테이블과 신체정보 테이블의 관계가 해당된다. 한 명의 학생은 하나의 신체정보를 가지므로 학생과 신체정보는 1:1로 매칭되기 때문이다.
- (2) One-to-Many Cardinality (1:N 관계)
    - 예를 들어, 한 명의 학생은 여러 개의 취미를 가질 수 있다.
    - 두 엔티티 간에 1:N 관계가 성립할 때는 새발 표기로 표시한다.
- (3) Many-to-Many Cardinality (N:M 관계)
    - 제품 엔티티 입장에서, TV 제품은 삼성 티비, 애플 티비 같은 여러 제조업체 제품이 있을 수 있다. 이는 냉장고나 세탁기도 마찬가지이며, 여러 기업에서 자신만의 상표를 생산한다.
    - 제조업체 엔티티 입장에서, 삼성이나 애플 회사는 세탁기만 생산하는 것이 아니라 티비, 스마트폰 등 여러 종류의 제품을 생산한다.
    - 따라서 제품과 제조업체 관계는 N:M 관계가 된다.
    - 선의 양쪽 모두에 새발로 표기한다.

#### N:M 관계의 문제점과 그 해결 방법에 대해 설명해주세요.

- 두 엔티티가 N:M 관계에 있는 경우, 두 개의 엔티티만으로는 서로를 표현하기에 다소 부족하다.
- 특히 테이블에서 데이터가 중복되다보니 데이터를 확인할 때 애매한 상황이 발생하게 되고, 데이터를 수정할 때 더욱 많은 데이터를 수정해야 하는 문제점이 발생한다.
- 데이터 모델링에서는 N:M 관계를 완성되지 않은 모델로 간주하여, 두 엔티티의 관계를 1:N 또는 N:1로 조정하는 작업이 필요하다.
- 따라서 두 엔티티의 관련성을 표현하기 위해서는 중간에 또 다른 엔티티를 필요로 한다. 즉, N:M 관계를 각각 1:N과 1:M으로 풀어줄 수 있는 연관 테이블을 추가하는 방식으로 해결할 수 있다.
- 이 부분은 데이터 모델링에서 공식처럼 적용되는 규칙이다. ERD 프로그램에서 N:M을 잡게 된다면 자동으로 조정 작업이 행해지게 된다.

</details>

---

### **반정규화가 무엇인지와 사용되는 이유에 대해 설명해 주세요**

<details><summary>답변</summary>
    
#### 반정규화란?

- 정규화된 엔티티, 속성, 관계에 대해 시스템의 성능향상과 개발과 운영의 단순화를 위해 중복, 통합, 분리 등을 수행하는 데이터 모델링의 기법
- 데이터 무결성이 깨질 수 있는 위험이 존재

#### 반정규화 절차

1. 대상 조사 및 검토
    - 데이터 처리 범위, 통계성 등을 확인하여 반정규화 대상 조사
2. 다른 방법 검토
    - 반정규화 수행 전 다른 방법 검토
    - 클러스터링, 뷰, 인덱스 튜닝, 응용 프로그램, 파티션
3. 반정규화 수행
    - 테이블, 칼럼, 관계 등을 반정규화 수행

#### 반정규화의 기법

1. 테이블 반정규화(테이블 수직, 수평 분할 / 테이블 병합)
2. 칼럼 반정규화(계산 값을 미리 계산하여 특정 칼럼에 추가)
3. 관계 반정규화

#### 반정규화의 필요성

- 데이터를 조회할 때 디스크 I/O량이 많은 경우 성능 저하
- 정규화로 인한 엔티티의 개수 증가, 관계가 많아짐에 따라 경로가 너무 멀어 조인으로 인한 성능이 저하되는 경우
- 칼럼을 계산하여 읽을 때 성능이 저하될 수 있음
- 특정 범위의 데이터만 자주 처리하는 경우
- 조회에 대한 처리성능이 중요한 경우, 반정규화를 통해 조회 성능 높이기 위해

#### 수행하지 않는 경우

- 성능이 저하된 데이터베이스가 생성될 수 있다.
- 구축단계나 시험단계에서 반정규화를 적용할 때 수정에 따른 노력비용이 많이 들게 된다.

</details>

---

### **부분 함수 종속, 이행적 종속, 다치 종속에 대해 설명해 주세요**

<details><summary>답변</summary>

#### 부분 함수 종속
- 기본키의 부분집합이 결정자가 되는 것
- ex) 수강강좌 테이블이 존재한다고 할 때
    - 수강강좌 테이블의 기본키는 *(학생번호, 강좌이름)*
    - *학생번호, 강좌이름*에 의해 *성적*을 결정할 수 있음
    - 그러나 *강의실*은 기본키의 부분집합인 *강좌이름*만으로 결정이 가능 → 부분 함수 종속 존재

#### 이행적 종속
- A→B, B→C가 성립할 때 A→C가 성립되는 관계를 이행적 종속이라고 함
- 테이블에 존재하는 필드의 부분 집합을 X와 Y라고 해보자
- X의 한 값이 Y에 속한 하나의 값에만 매핑이 될 경우 Y는 X에 함수 종속적이다 라고 하며, 이를 X→Y로 표기
    - 이때 X를 결정자, Y를 종속자 라고 한다
- ex) 계절학기 테이블
    - 학생번호 속성은 강좌이름을 결정하고, 강좌이름 속성은 수강료를 결정
    - 학생번호 속성이 이행적 종속으로 인해 수강료를 결정하게 된다 → 이행적 종속 존재

#### 다치 종속
- X→Y일 때 하나의 X값에 여러 개의 Y값이 존재하면 다치 종속성을 가진다 하고 X↠Y로 표시
    - 하나의 릴레이션 내에 두 개의 독립된 속성이 1:N의 관계를 갖는 것 
- ex) 학생 테이블
    - *학생번호*→*과목*일 때 하나의 학생번호에 여러 개의 과목이 존재
    - 3개의 속성 존재
    - 과목과 취미는 독립적이다

</details>

---

</details>