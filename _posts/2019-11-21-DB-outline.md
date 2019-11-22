---
title: "정보처리기사 실기 데이터베이스 요약"
date: 2019-11-21
last_modified_at: 2019-11-22T16:34:09

toc_sticky: false

categories:
 - Database
tags:
 - Database
 - cheatsheet
 - 정보처리기사
---

본 글은 정보처리기사 2019년 3회차를 응시하기 위해 데이터베이스 과목을 공부하며 기록한 요약본입니다.

내용은 다음을 참고하여 작성하였으며 2020년 부터는 정처기 시험이 개편된다곤 하지만 DB의 전체적인 흐름을 파악하는데 도움이 되고자 업로드 하였습니다.

## References
> [\[정보처리기사실기\] 요약 정리본 - Gyoogle(규글)](https://kim6394.tistory.com/74?category=659595)
>
> [정보처리 실기_데이터베이스 - YouTube](https://www.youtube.com/playlist?list=PLimVTOIIZt2aP6msQIw0011mfVP-oJGab)
>


## 1. 데이터베이스(DB)의 개념
### 1.1. 정의
특정 조직의 업무를 수행하는데 필요한 상호 관련된 데이터들의 모임

1. 통합 데이터(Integrated Data)
: 검색 효율성을 위해 중복을 최소화한 데이터 eg) 클라우드

2. 저장 데이터(Stored Data)
: 컴퓨터가 접근 가능한 매체에 저장된 데이터

3. 운영 데이터(Operational Data)
: 조직의 목적을 위해 존재가치가 확실하고 반드시 필요한 데이터

4. 공유 데이터(Shared Data)
: 여러 응용프로그램이 공동을 사용하는 데이터

### 1.2. 특징
1. 실시간 접근성(Real Time Accessiblity)
: 사용자 질의에 즉시 처리하여 응답

2. 계속적인 변화(Continuous Evolution)
: 삽입/수정/삭제를 통해 항상 최근의 정확한 데이터를 동적으로 유지

3. 내용에 의한 참조(Content Reference)
: 데이터 참조 시 튜플이 주소나 위치가 아닌 데이터 내용에 따라 참조

4. 동시 공유(Concurrent Sharing)
: 여러 사용자가 동시에 원하는 데이터를 공용

5. 데이터 논리적/물리적 독립성(Independence)
: 

### 1.3. 데이터 언어: DBMS와의 통신수단
1. DDL(Data Definition Language, 데이터 정의어)
: DB를 구축하거나 변경할 목적으로 사용하는 언어

2. DML(Data Manipulation Language, 데이터 조작어)
: 응용프로그램과 DBMS 사이의 인터페이스를 위한 언어

3. DCL(Data Control Language, 데이터 제어어)
: 보안/권한제어, 무결성, 회복, 병행제어를 위한 언어

### 1.4. 데이터베이스 사용자
1. DBA(Database Administrator)
: DDL과 DCL을 통해 DB를 정의하고 제어하는 사람/그룹

2. 데이터 관리자(Data Administrator)
: 기업 또는 조직 전반에 있는 데이터에 대한 관리 총괄/통제

3. 데이터 설계자(Data Architect)
: 기업 업무 수행에 필요한 데이터 구조를 체계적으로 정의하는 사람

4. 응용프로그래머(Application Programmer)
: 호스트 프로그래밍언어에 DML을 삽입하여 DB에 접근하는 사람

5. 일반 사용자(End User)
: 질의어(query)를 통해 DBMS에 접근하는 사람

DA(Data Administrator, Architect)가 data 자체를 관리하고 설계하는 설계자, 관리자라면 DBA는 DA가 설계한 구조를 바탕으로 DB를 구축하는 기술자이다.


## 2. 데이터베이스 관리 시스템(DBMS; Database Management System)
### 2.1. 개념
사용자와 데이터베이스 사이에서 사용자의 요구에 따라 정보를 생성해 주고 데이터베이스를 관리해주는 소프트웨어
eg) MySQL, ORACLE, ...

###	2.2. 특징
1. 중복의 최소화
: 

2. 데이터 공동이용(공유성)
: DBMS는 특정 프로그램에 종속되지 않고 여러 응용 시스템들에게 데이터를 줄 수 있어야 한다.

3. 데이터 일관성
: 

4. 데이터 무결성 유지(정확성/정합성)
: 

5. 데이터 보안 유지
: 

###	2.3. 장단점
1. 장점
: 논리적/물리적 독립성 보장, 공동 이용, 표준화, 무결성 유지, 실시간 처리, 중복 방지, 통합관리, 일관성 유지, 보안 유지, 
최신 유지

2. 단점
: 전문가 부족, 과부화, 전산화 비용 증가, 백업과 회복 어려움, 시스템 복잡

DSMS(Data Stream Management System)
: 통신 상 데이터 스트리밍을 통해 데이터를 처리하고 관리하는 시스템으로 동적인 데이터를 처리하는 관리 시스템


## 3. 스키마(Schema)
### 3.1. 정의
데이터베이스의 구조와 계약조건에 관한 전반적인 명세

###	3.2. 스키마 3계층
1. 외부 스키마(External Schema)
: 사용자나 응용프로그래머가 각 개인 입장에서 필요로 하는 DB의 논리적 구조를 정의한다.
: 사용자 입장에서 보여지는 구조

2. 개념 스키마(Conceptual Schema)
: 개체간 관계와 제약 조건을 나타내고, 접근권한, 보안정책, 무결성 규정에 관한 명세를 정의한다.
: 전체적인 DB의 구조, 제약조건, 보안과 관련된 모든 요소들

3. 내부 스키마(Internal Schema)
: DB의 물리적 구조 정의
: 물리적으로 저장돼 있는 데이터의 유형이나 크기와 관련된 구조


## 4. 데이터베이스 설계
###	4.1. 정의
DB Schema를 개발하는 과정이며 데이터 모델링이라고도 한다.

현실 세계의 정보들을 컴퓨터에 체계적으로 표현하기 위해서 단순화, 추상화 형태로 나타낸 개념적 모형

###	4.2. 구성 요소
구조(Structure) / 연산(Operation) / 제약조건(Constraint)	

###	4.3. 개념적 데이터 모델
1. 요구조건 분석
: 요구조건 명세서 작성

2. 개념적 설계
: 개념스키마 설계, 트랜잭션 모델링, ER모델링. 
: _무엇을 데이터베이스화 할 것이냐?(DA의 역할)_

3. 논리적 설계
: 논리 스키마 설계, 트랜잭션 인터페이스 설계

4. 물리적 설계
: 물리적 구조의 데이터(내부 스키마)로 변환

5. 데이터베이스 구현
: DDL로 데이터베이스 생성, 트랜잭션 생성


<br>
<br>

# 개념적 데이터베이스 모델링
## 5. ER모델
대표적인 개념적 데이터 모델(Peter Chen, 1976)

###	5.1. 개체(Entity)
DB화 하려는 대상

Entity Type - Entity Instance

###	5.2. 속성(Attribute)
1. 단순 속성 vs 복합 속성
    - 단순 속성 (simple attribute)
    : 더 이상 분해할 수 없는 속성 eg) 이름

    - 복합 속성 (composit attribute)
    : 단순 속성으로 분해할 수 있는 속성 eg) 주소

2. 단일값 속성 vs 다중값 속성
    - 단일값 속성(single value attribute)
    : 하나의 값을 갖는 속성

    - 다중값 속성(multiple value attribute)
    : 여러 값을 갖는 속성 -> 정규화를 통해 가급적이면 분해해야 한다.

3. 저장속성 vs 유도 속성
    - 저장 속성(Stored Attribute)
    : 유도 속성 계산을 위해 사용된 속성 eg) 강사 입문 년도

    - 유도 속성(Derived Attribute)
    : 다른 속성 값으로 부터 유도되어 결정되는 속성 eg) 강사 경력
	
###	5.3. 관계 타입(Relationship Type) / 관계의 카디널리티(Relationship Cardinality)
1. 1:1관계
: 관계에 참여하고 있는 두 개체 타입이 모두 하나씩의 개체 occurence를 갖는 관계

2. 1:N관계
: 관계에 참여하고 있는 두 개체 타입 중 한 개체 타임은 여러 개의 개체 occurence, 다른 한 개체 타입은 하나의 개채 occurence를 갖는 관계

3. N:M관계
: 관계에 참여하고 있는 두 개체 타입 모두 여러가지의 개체 occurence를 갖는 관계

###	5.4. 필수/선택 참여 관계
필수참여 관계(Mandatory Membership)
: 반드시 대응되는 개체가 있어야 한다.	\|

선택참여 관계(Optional Membership)
: 반드시 대응되는 개체가 없어도 된다. 	O

###	5.5. 종속 관계(Independant Relationship)
식별관계 
: 외래 식별자가 주 식별자로 존재하는 관계(B는 A에 의존적)... _실선_

비식별관계
: 외래 식별자가 일반속성으로 존재하는 관계(A와 B는 독립적)... _점선_

###	5.6. 존재 종속 관계(Existance Dependance)
주 개체(dominant entity, strong entity)
: B개체의 존재를 결정하는 A개체

종속 개체(subdominate entity, weak entity
: A개체에 의해 존재가 결정되는 B개체

###	5.7. ISA관계: 상속관계


## 6. 관계형데이터 모델
테이블 또는 Relation(데이터를 원자 값으로 갖는 이차원의 테이블)의 구조로 표현하는 논리적 데이터 모델

###	6.1. 릴레이션의 구조
1. 행(Row)
: 

2. 열(Column)
: 

3. Relation Scheme
: 표의 구조

4. Relation Instance
: 릴레이션에 실제 입력된 데이터 한 줄

5. Tuple
: 하나의 행
: Relation Instance - 한 줄 단위의 데이터 그 자체 / Tuple - 하나의 행

6. Cardinality
: 입력된 튜플의 개수

7. 속성(Atrribute)
: 릴레이션을 구성하는 각각의 열 개체의 특성이나 상태, 열(Column)로 입력된다.

8. 차수(Degree)
: attribute/column의 수

9. Domain
: 속성이 취할 수 있는 같은 타입의 원자값들의 집합. 정의된 속성은 반드시 해당 도메인 내의 값을 취한다. eg)학년의 도메인: 1~6
		
###	6.2. 릴레이션의 특성
1. 한 릴레이션에 포함된 튜플들은 모두 상이하다.(중복 금지)

2. 한 릴레이션에 포함된 튜플 사이에는 순서가 없다.

3. 한 릴레이션을 구성하는 속성의 이름(열 제목)은 유일해야 한다.

4. 한 릴레이션을 구성하는 속성 사이에는 순서가 없다.

5. 모든 속성값은 논리적으로 더 이상 분해할 수 없는 값인 원자값이어야 한다.


###	6.3. ER스키마의 관계 스키마 사상(Mapping Rule)
ER model(개념적 모델) -> Relation Schema(논리적 모델)

1. 1:1관계
: Relation A(B)의 기본키 -> B(A)의 외래키

2. 1:N관계
: Relation A의 기본키 -> B의 외래키

3. N:M관계
: Relation A, B 기본키를 포함한 별도 릴레이션 C를 표현하며 이 때 C는 교차 엔티티(교차 릴레이션) 이다.


###	6.4. 교차 관계(Intersection Relationship)
관계를 테이블로 표현하여 다대다 관계를 보여주는 테이블

<br><br>
# 논리적 데이터베이스 모델링
## 7. 키(key)
### 7.1. 정의
주어진 릴레이션에서 모든 인스턴스 가운데 유일함(Uniqueness)을 보장해주는 하나 이상의 애트리뷰트의 집합이다.

튜플을 유일하게 식별할 수 있는 속성의 집합

튜플을 검색하거나 정렬할 때 튜플을 서로 구분할 수 있는 기준이 되는 속성

### 7.2. 특성
1. 유일성(Uniqueness)
: 

2. 최소성(minimality)
: 최소한의 속성으로 원하는 튜플을 찾아야 한다.

### 7.3. 키의 종류
1. 슈퍼키(Super Key)
: 튜플을 유일하게 구분하기 위해 한 개 이상의 속성들로 이루어진 키. 유일성을 만족하지만 최소성을 만족하지 못함

2. 후보키(Candidate Key)
: 튜플을 유일하게 구분할 수 있는 최소 슈퍼키. 한 릴레이션에서 유일성과 최소성을 모두 만족

3. 기본키(primary key)
: 후보키 중에서 대표로 지정된 키. 중복값이나 NULL값을 가질 수 없음 

4. 대체키(alternate key)
: 후보키 중에서 기본키를 제외한 나머지 후보키. 보조키라고도 불린다. 후보키 - 기본키 = 대체키

5. 외래키(foreign key)
: 다른 릴레이션의 기본키를 참조하는 속성 또는 속성들의 집합

6. 복합키(Composit Key)
: 2개 이상의 속성을 조합하여 만든 키


## 8.무결성 제약(Integrity Constraint)
### 8.1. 무결성
DB에 저장된 값과 그것이 표현하는 현실세계 실제 값이 일치하는 정확성. 데이터의 정확성/정합성

### 8.2. 제약조건(Constraint)
DB의 정확성을 보장하기 위해 정확하지 않은 데이터가 저장되는 것을 방지하기 위한 제약조건

### 8.3. 종류
1. 개체 무결성 제약
: 한 릴레이션의 기본키를 구성하는 어떠한 속성값도 NULL이나 중복값을 가질 수 없다.

2. 참조 무결성 제약
: 외래키 값은 NULL 또는 참조 릴레이션 기본키 값과 동일해야 함. FK를 통해 찾아갔는데 없으면 안된다.

3. 도메인 무결성 제약
: 주어진 속성의 값이 도메인에 속한 것이어야 함
: (도메인 - 속성값이 가질 수 잇는 범위, 원자값들의 집합)


### 8.4. 무결성 제약을 위한 DBMS의 옵션
1. restrict - no action

2. cascade - 연쇄 삭제 연쇄 수정

3. set null - 널값으로 수정

4. set default - 기본값으로 수정


## 9. 이상(Anomaly)
### 9.1. 개념
데이터 중복으로 인해 릴레이션 조작 시 예상하지 못한 곤란한 현상이 발상

이상은 속성들 간에 존재하는 여러 종류의 종속 관계를 하나의 릴레이션에 표현할 때 발생

### 9.2. 종류
1. 삽입 이상(Insertion Anomaly)
: 데이터 삽입 시 의도하지 않는 값들로 인해 삽입이 불가능한 현상

2. 삭제 이상(deletion Anomaly)
: 한 튜플 삭제 시 의도하지 않은 값들도 같이 삭제되는 현상

3. 갱신 이상(Update Anomaly)
: 갱신 시 일부 튜플의 정보만 갱신 되어 정보의 불일치성이 생기는 현상



## 10. 함수적 종속성(Functional Dependency)
### 10.1. 정의
X -> Y 'X이면 Y이다'

X는 결정자(Determinant) / Y는 종속자(Dependent)

### 10.2. 정규형

함수적 종속 | 정규형
:-------------:|:--------:
완전 함수적 종속 | 2NF
부분 함수적 종속 | 2NF
이행 함수적 종속 | 3NF
다치 종속 | 4NF
조인 종속 | 5NF

### 10.3. 완전함수적종속 / 부분함수적종속
복합키로 기본키가 구성되어 있을 때,

키를 구성하는 모든 속성을 참조해서 특정 속성을 조회할 수 있다면 완전함수적종속이고 일부 속성만을 참조해서 특정 속성을 조회할 수 있다면 부분함수적종속이다.

학번, 과목코드 -> 학과, 성명

학번, 과목코드 -> 학과 / 학번 -> 성명

부분함수적종속을 제거하고 완전함수적종속으로 바꾼다.

### 10.4. 이행함수적종속
A->B->C

학번 -> 주민번호 -> 성명

학번 -> 주민번호 / 주민번호 -> 성명

### 10.5. 다치종속(MVD, Multi Value Dependent)
A ->> B

과목 ->> 교제\|교수

과목 -> 교제 / 과목 -> 교수

### 10.6. 조인 종속
위조튜플


## 11. 정규화(normalization)
비정규 릴레이션(개념스키마) -> 정규화된 릴레이션

테이블을 무손실분해하여 이상 현상 발생 가능성을 줄이는 것
### 11.1. 무손실 분해(Lossless Decomposition)
분해된 두 릴레이션을 조인하면 원래의 릴레이션에 들어 있는 정보를 완전하게 얻을 수 있다.

여기서 손실이란 정보의 손실을 뜻한다.

정보의 손실은 원래의 릴레이션을 분해한 후에 생성된 릴레이션들을 조인한 결과에 들어 있는 정보가 원래의 릴레이션에 들어 있는 정보보다 적거나 많은 것을 모두 포함한다.	

### 11.2. 제1정규형(1NF)
릴레이션에 속한 모든 도메인이 원자값(Atomic Value)

모든 열과 행의 중복지점에는 한 개의 값(single value)를 가진다.

### 11.3. 제2정규형(2NF) 
키가 아닌 모든 속성들이 기본키에 완전 함수 종속

### 11.4. 제3정규형(3NF) 
키가 아닌 모든 속성들이 기본키에 이행적으로 함소종속 되지 않는 릴레이션

### 11.5. BCNF(Boyce-Codd NF) 
릴레이션의 모든 결정자가 후보키인 릴레이션 강한 제3정규형(3.5NF)리아고도 함

### 11.6. 제4정규형(4NF) 
릴레이션 R에서 다치종속 A ->-> B 가 성립하는 경우 다치종속(MVD)의 제거

### 11.7. 제5정규형(5NF) 
조인종속성 이용



<br><br>
# 물리적 데이터베이스 모델링
## 12. 물리적 데이터베이스 모델링
### 12.1. 정의
저장구조와 접근경로의 설계

데이터베이스의 쿼리와 트랜잭션들을 분석

역정규화

### 12.2. 물리적 DB 설계 시 고려사항
응답 시간의 최소화

저장 공간의 효율화

트랜잭션 처리도

### 12.3. 물리적 DB 설계의 기능
저장 레코드 양식 설계 -> type과 data size를 정한다.

레코드 집중의 분석 및 설계 -> 자주 함께 사용되는 데이터끼리는 가까운 곳에 저장한다.



## 13. 반정규화/역정규화(De-normalization)
### 13.1. 정의
시스템의 성능향상과 개발/운영의 단순화를 위해 기존 설계를 재구성하는 것

데이터의 정합성과 데이터의 무결성을 우선으로 할지, 데이터베이스 구성의 단순화와 성능을 우선으로 할지 결정해야 함

트랜잭션 발생량이 많아 시스템 성능에 크게 영향을 주는 테이블만을 대상으로 한다.

### 13.2. 반정규화 과정
1. 반정규화 대상 조사
: 범위처리 빈도수 조사
: 테이블 조인 개수

2. 다른 방법 검토
: 뷰(View) 테이블
: 클러스터링 적용
: 인덱스의 조정

3. 반정규화 적용
: 테이블 반정규화
: 속성의 반정규화
: 관계의 반정규화

### 13.3. 반정규화 대상 조사
1. 상관 모델링
: 트랜잭션의 빈도수를 예상하기 위해 업무 프로세스와 데이터베이스의 상관성을 설계하는 작업
: 업무가 처리되는 과정에 따라 데이터가 어떻게 영향을 받고 있는지 분석하여 설계

2. CRUD MATRIX
: CRUD 4가지 유형으로 업무가 진행되는 절차에 따른 데이터의 상관관계를 분석

### 13.4. TABLE 반정규화
정규화 과정에 의해 분리된 두 테이블에 많은 트랜잭션이 발생하여 JOIN 연산으로 인해 시스템 저하가 일어날 수 있으므로이런 경우 두 테이블을 병합

TABLE 분할 - 수직적 분할 / 수평적 분할
1. 테이블의 수직적 분할(Vertical Partitioning)
: 트랜잭션이 집중 발생하는 속성들을 따로 뽑아서 분할한다.

2. 테이블의 수평적 분할(Horizontal Partitioning)
: 천만 개의 데이터를 백만개 데이터 10개로 자른다.

### 13.5. Column(속성)의 반정규화
1. 중복 컬럼 방법
: 해당 테이블에서 자주 사용하는 다른 테이블의 컬럼을 해당 테이블에도 복사한다.

2. 파생 컬럼 추가
: 필요에 의해 특정 속성값으로 만들어지는 파생 컬럼을 추가


## 14. View 설계
### 14.1. View
가상테이블
### 14.2. DDL
```sql
CREATE VIEW name(attr1, attr2, ...) AS
SELECT attr, ...
FROM table, ...
WHERE condition
```


## 15. 인덱스(Index) 설계
### 15.1. 정의
데이터베이스에서 원하는 데이터를 좀더 빨리 찾아줄 수 있도록 데이터의 위치정보를 모아놓은 개체

항상 정렬 상태로 유지되며 성능 향상에 기여한다.

수정이 자주 발생하지 않는 컬럼을 인덱스로 선정한다.

### 15.2. 데이터 검색 방법
1. FTS(Full Table SCAN)

2. INDEX SCAN

### 15.3. 인덱스의 종류
1. Clustered INDEX
: 인덱스 기준으로 데이터를 정렬
: 검색 속도가 빠르고 범위 조회(Range Query)에서 빠르다
: 한 테이블에 한 Clustered INDEX만 생성 가능
: 기본키를 만들면 일반적으로 기본키에 Clustered INDEX가 생성됨
: 루트 -> 리프

2. Non Clustered INDEX
: 데이터 페이지의 데이터 그대로 위치 정보를 인덱스로 구성
: 별도 인덱스 페이지가 생성
: 검색 속도가 느리며 범위 조회를 할 경우 거의 인덱스의 도움을 받을 수 없다.
: 여러 개의 인덱스 생성 가능
: 더 많은 공간 차지
: 루트 -> 리프 -> 데이터 페이지

### 15.4. 선택성(Selectivity)
선택될 수 있는 빈도

분포도가 높으면 선택성이 낮아진다.



## 16. 관계 데이터 연산
SOC(Structure, Operation, Constraint)

### 16.1. 관계 대수(Relational Algebra)
원하는 정보와 그 정보를 어떻게 유도하는가를 기술하는 절차적인 언어

일반 집합 연산자 / 순수 관계 연산자

어떻게?

1. 일반 관계 연산자

    UNION / INTERSECTION / DIFFERENCE / CARTESIAN PRODUCT

2. 순수 관계 연산자

    SELECT / PROJECT / JOIN / Theta JOIN / Equi JOIN / Natural JOIN / Outer JOIN / DIVISION	

    SELECT
    : 튜플의 수평적 부분집합

    PROJECT
    : 튜플의 수직적 부분집합

### 16.2. 관계해석 Relational Calculus
원하는 정보가 무엇이라는것만 정의하는 비절차적 언어



<br><br>
# DB 구축 - SQL
## 17. DDL(Data Definition Language)
Metadata in System Catalog = Data Dictionary

### 17.1. CREATE
```sql
CREATE DOMAIN GENDER CHAR(2)
DEFAULT '여'
CONSTRAINT VALID-GENDER CHECK (VALUE IN ('남', '여'))
```
```sql
CREATE TABLE 학생(
학번 CHAR(15), 이름 VARCHAR(15) NOT NULL, ... , 성별 GENDER, 생년월일 DATE,
PRIMARY KEY(학번), 
UNIQUE(전화번호), 
FOREIGN KEY(학과) REFERENCES 학과(학과코드) ON DELETE DASCADE ON UPDATE CASCADE, 
CONSTRAINT 학년제약 CHECK(학년 >= 1 AND 학년 <= 4));
```
```sql
ON DELETE {CASCADE | SET NULL | SET DEFAULT | NO ACTION | RESTRICT}
```
```sql
ON UPDATE {CASCADE | SET NULL | SET DEFAULT | NO ACTION | RESTRICT}
```
```sql
CREATE VIEW 컴공학생(학번, 이름, 학과)
AS SELECT 학번, 이름, 학과
FROM 학생
WHERE 학과 = '컴공'
[WITH CHECK OPTION];
```
```sql
CREATE UNIQUE INDEX 학번_idx
ON 학생(학번 ASC);
```

### 17.2. ALTER
```sql
ALTER TABLE 학생 ADD/ALTER/DROP 연락처 VARCHAR(13)
```

### 17.3. DROP


## 18. DCL(Data Control Language)
### 18.1. COMMIT

### 18.2. ROLLBACK

### 18.3. GRANT
```sql
GRANT SELECT, DELETE ON STUDENT 
TO U1 WITH GRANT OPTION;
```

### 18.4. REVOKE
```sql
REVOKE SELECT, DELETE ON STUDENT
FROM U1 CASCADE;
```


## 19. DML(Data Manipulation Language)
### 19.1. SELECT
```sql
SELECT * 
FROM 학생;
```

```sql
SELECT DISTINCT 학과
FROM 학생;
```

```sql
SELECT 학번, 성명
FROM 학생
WHERE 학과 = '전기' AND 학년 = 4;
```

```sql
SELECT *
FROM 학생
WHERE 학과 IN ('기계', '컴퓨터') AND 학년 IS NULL;
```

```sql
SELECT *
FROM 학생
WHERE 성명 LIKE '이%';
```

```sql
SELECT *
FROM 수강
WHERE 성적 BETWEEN 90 AND 100;
```

```sql
SELECT *
FROM 수강
ORDER BY 학번 ASC, 과목코드 DESC;
```

```sql
SELECT 학번, AVG(성적) AS 평균성적
FROM 수강
GROUP BY 학번;
```

```sql
SELECT 과목코드, COUNT(*) AS 학생수
FROM 수강
GROUP BY 과목코드 HAVING COUNT(*) > 1;
```

```sql
SELECT 성명, 학과
FROM 학생
WHERE 학번 = (SELECT 학번 FROM 수강 WHERE 성적 = 100);
```

```sql
SELECT 학번, 성명
FROM 학생
WHERE 학번 IN (SELECT 학번 FROM 수강 WHERE 과목코드 = 'C001');
```

```sql
SELECT 학번,성명
FROM 학생
WHERE EXIST (SELECT * FROM 수강 WHERE 학번 = 학생.학번 AND 과목코드 = 'C001');
```

```sql
SELECT 학번 FROM 학생 WHERE 학년 = 1
UNION
SELECT 학번 FROM 수강 WHERE 과목코드 = 'C002';
```

```sql
SELECT 학번 FROM 학생
EXCEPT
SELECT DISTINCT(학번) FROM 수강;
```

```sql
SELECT 학생.학번, 학생.성명, 수강.과목코드
FROM 학생, 수강
WHERE 학생.학번 = 수강.학번 AND 학생.학과 = '전기';
```

```sql
SELECT A.학번, A.성명, B.과목코드
FROM 학생 A JOIN 수강 B ON (A.학번 = B.학번)
WHERE A.학과 = '전기';
```

```sql
SELECT A.학번, A.성명, B.과목코드
FROM 학생 A LEFT OUTER JOIN 수강 B ON (A.학번 = B.학번)
WHERE A.학과 = '전기';
```

### 19.2. INSERT
```sql
INSERT INTO 학생(학번, 성명) VALUES(500, 을지문덕);
```

```sql
INSERT INTO 학생 VALUES(600, 유관순, 체육, 3);
```

```sql
INSERT INTO 졸업예정자(학번, 성명, 학과)
SELECT 학번, 성명, 학과
FROM 학생
WHERE 학년 = 4;
```

### 19.3. DELETE
```sql
DELETE FROM 학생 WHERE 성명 = '홍길동';
```

```sql
DELETE FROM 학생;
```

### 19.4. UPDATE
```sql
UPDATE 학생 SET 학과 = '영어영문' WHERE 성명 = '이순신';
```


## 20. Trigger
```sql
CREATE TRIGGER 입고INS ON 입고 FOR INSERT
AS 
DECLARE @CODE CHAR(6), @QTY INT
SET @CODE = (SELECT 상품코드 FROM INSERTED)	
SET @QTY = (SELECT 입고수량 FROM INSERTED)
UPDATE 상품
SET 재고수량 = 재고수량 + @QTY
WHERE 상품코드 = @CODE
```	

## 21. 내장형SQL(Embedded SQL)
삽입 SQL, 프로그램 언어에 삽입된 SQL


## 22. 커서(Cursor)
복수 개의 튜플에 접근 가능하기 위한 레코드 집합의 포인터

명령어
1. DECLARE
2. OPEN
3. FETCH
4. CLOSE

<br><br>
# 회복과 병행제어
## 23. 트랜잭션(Transaction)
DB에서 처리되는 작업의 단위로 연산의 집합으로 이루어져 있다.
### 23.1. 특징 - ACID
1. 원자성(Atomicity)
2. 일관성(Consistency)
3. 격리성(Isolation)
4. 영속성(Durability)

원자성(Atomicity)
: All or Nothing. 
: 트랜잭션 내의 모든 연산은 반드시 한꺼번에 완료되어야 하며 그렇지 못한 경우는 한꺼번에 취소되어야 한다.

일관성(Consistency)
: 트랜잭션이 그 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 변환한다.

격리성(독립성, Isolation)
: 트랜잭션 T1이 실행되는 도중 T2가 실행되면 안된다.

영속성(Durability)
: 트랜잭션에 의해서 생성된 결과는 계속 유지되어야 한다.

### 23.2. 연산
1. COMMIT
2. ROLLBACK

## 24. 회복(Recovery)
### 24.1. 장애의 유형
1. 실행장애
: ROLLBACK으로 종료된 경우

2. 트랜잭션 장애
: 계획되지 않은 비정상적인 종료들(overflow 등)

3. 시스템 장애
: 

4. 미디어 장애
: 저장장치의 문제

### 24.2. 중복 저장 기법
1. 덤프(Dump)
: 주기적으로 데이터베이스 전체를 다른 저장장치에 복제하는 기법

2. 로그(Log)
: 데이터베이스가 변경될 때마다 변경되는 데이터 항목의 이전 값과 이후 값을 별도로 기록하는 기법


### 24.3. 회복의 원리
1. shadowing 기법
: dump해놨던 데이터를 그대로 불러와서 원본 데이터를 복원한다.

2. redo / undo
    1. redo
    : 장애 발생 전 완료된 트랜잭션들을 대상으로 재수행한다.
    2. undo
    : 장애 발생 당시 실행 중이던 트랜잭션들의 실행을 되돌린다.

### 24.4. 회복 기법의 종류
1. 즉시 갱신(Immediate Update)
: 
2. 지연 갱신(Deferred Modification)
: No un-do, 변경 내용이 실제적으로 DB에 반영되지 않았기 때문에 로그만 삭제하고 끝난다.
3. 검사 시점(Check Point) 회복
: 


## 25. 병행 제어(Concurrency Control)
### 25.1. 갱신 분실(Lost Update)
두 개 이상의 트랜잭션이 같은 자료를 공유하여 갱신할 때 갱신 결과의 일부가 분실되는 현상이다.

race condition

### 25.2. 비완료 의존성(Uncommited Dependency)
하나의 트랜잭션 수행이 실패한 후 회복되기 전에 다른 트랜잭션이 실패한 갱신 결과를 참조하는 현상이다.

dirty read

### 25.3. 모순성(Inconsistency)

### 25.4. 연쇄 복귀(Cascade Rollback)
병행 수행되던 트랜잭션들의 하나에 문제가 생겨 Rollback하는다른 트랜잭션도 함께 Rollback되는 현상이다.

### 25.5. 병행제어의 정의
Concurrency가 가능하도록 해주는 작업

### 25.6. Scheduling
1. 직렬 스케줄(Serial Schedule)
2. 비직렬 스케줄(Nonserial Schedule)

### 25.7. 병행제어 기법의 종류
1. 로킹(Locking)
: deadlock의 위험이 있다.

2. 2단계 로킹(2PL; Two-Phase Locking)
: 성장(확장)단계(Growing Phase) - lock
: 축소 단계(Shrinking Phase) - unlock

3. 전용 로크(Exclusive Lock)
: rw 허용
    
    공용 로크(Shared Lock)
    : r허용 w비허용

4. 타임스탬프(Time Stamp)
: 