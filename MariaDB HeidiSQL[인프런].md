# MariaDB / HeidiSQL - 인프런 무료강의

[[무료] MariaDB 클라이언트 개발, HeidiSQL - 인프런 | 강의](https://www.inflearn.com/course/mariadb-heidisql-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EA%B0%9C%EB%B0%9C/dashboard)

# 접속 세션 및 테이블 생성

---

- mariadb 설치, heidisql 연동, 테이블 생성
- 쿼리 던지고 결과 확인하기(실행은 ; 단위로)

# 결과셋(격자행) 내보내기

---

- 격자행(결과셋) 내보내기 : html,excel(.csv) -> 쿼리를 통해 테이블에 대한 쿼리 출력값을 원하는
- 파일로 출력

# 데이터베이스 → SQL

---

- 데이터베이스 sql로 내보내기 : 테이블에서 테이블로 내보내어 백업 테이블 만들기

# excel(.csv) 내용을 데이터베이스로 가져오기

---

# 행들을 ctrl,shift로 선택하여 각 형식으로 복붙하기

---

- 복사(마지막에 선택한 행을 테이블에 복사), copy as는 다른 것!
- 복잡한 쿼리를 보기 좋게 보고 싶다면 '쿼리'-'SQL 재구성'

# 로그창 활용 / 텍스트문자열 찾기

---

- 쿼리 및 테이블 생성등 모든 명령어들을 기록한 로그(맨 아래 패널)를 파일로 저장하기
- Ctrl+f로 검색 이후 다음 검색 결과를 보려면 f3을 누른다
- '검색'-'서버 내에서 텍스트 찾기' : 찾을 텍스트를 입력한 뒤, 일치 유형으로 와일드카드 타입을 설정하여 중간에 껴있는 내용까지 찾을 건지 설정하여 검색한다.// 데이터베이스에서는 *대신 %을 사용한다
- 로그창 청소 단축키 : Ctrl+Q

# 데이터 탭 활용

---

- 테이블 데이터 섹터에서 행 추가/삭제/편집/필터
- SELECT 문에서 LIMIT문을 걸면 ('도구'-'격자서식'-'페이지당최대행수') 지정한 행수만큼의 행들만을 보여주고 다음버튼을 통해 다음페이지들을 보여줌

# 스니펫 활용

---

- 쿼리문들을 스니펫으로 저장하여 자주 사용하는 쿼리문들을 등록할 수 있음
- 쿼리내역을 통해 그 동안 사용했던 쿼리문들을 조회할 수 있음

# 통한 쿼리 프로파일링

---

MySQL에서 쿼리가 처리되는 동안 각 단계별 작업에 시간이 얼마나 걸렸는지 확인 할 수 있는 기능을 제공하며 쿼리 프로파일링(Query Profiling) 기능을 제공하고 있습니다.

- 각 이벤트에 대한 데이터를 캡처하고 파일이나 테이블에 저장하여 나중에 분석할 수 있습니다. 예를 들어 프로덕션 환경을 모니터링하여 어느 저장 프로시저가 너무 늦게 실행되어 성능을 떨어뜨리고 있는지 볼 수 있습니다. SQL Server 프로파일러는 다음과 같은 작업에 사용됩니다.
- 문제가 발생한 원인을 찾기 위해 문제 쿼리 실행
- 실행이 느린 쿼리를 찾고 진단
- 문제가 발생한 일련의 Transact-SQL 문 포착. 그런 다음 저장된 추적을 사용하여, 문제를 진단할 수 있는 테스트 서버에서 문제를 복제할 수 있습니다.
- SQL Server의 인스턴스에서 수행되는 동작을 감사하는 기능을 지원합니다. 감사는 보안 관리자가 나중에 검토할 수 있도록 보안 관련 동작을 기록합니다.

쿼리 프로파일링(Query Profiling) 는 MySQL 5.1 이상에서 부터 지원 합니다.

기본적으로 활성화돼 있지 않기 때문에 사용시 먼저 프로파일링 기능을 활성화 해야 합니다.

# 프로파일링 활성화

---

쿼리 프로파일링를 사용하기 위해서는 먼저 활성화가 필요 합니다.

활성화는 아래와 같이 수행하면 됩니다.

```sql
mysql> set profiling=1;
```

## 프로파일링 사용

---

**show profiles**

쿼리 프로파일링의 쿼리 목록은 아래와 같이 수행할 수 있습니다.

```sql
mysql> show profiles;
+----------+------------+----------------------------------------+
| Query_ID | Duration   | Query                                  |
+----------+------------+----------------------------------------+
|        1 | 0.00019375 | select *                               |
|          |            | from employees                         |
|          |            | where emp_no=10001                     |
|        2 | 0.00212875 | select count(*)                        |
|          |            | from employees                         |
|          |            | where emp_no between 10001 and 12000   |
|          |            | group by first_name                    |
+----------+------------+----------------------------------------+
```

**profiling_history_size**profiling 정보는 모두 저장되는 것이 아니고 profiling_history_size 에 설정된 만큼 저장이 됩니다.

```sql
mysql> show variables like '%profiling_history_size%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| profiling_history_size | 15    |
+------------------------+-------+
```

기본 값으로 최근 15개의 쿼리에 대해서 저장 되며 최대 100까지 설정이 가능 합니다.

## 특정 쿼리의 상세 프로파일링 정보를 조회

---

특정 쿼리의 상세 프로파일링 정보를 조회하려면 for query [쿼리번호] 를 사용 하면 됩니다.

위에서 언급한 쿼리번호 는 위의 show profiles 에서 조회한 Query_ID 입니다.

```sql
mysql> **SELECT** **STATE**, **FORMAT**(**DURATION**, 6) **AS** **DURATION
FROM** INFORMATION_SCHEMA.**PROFILING
WHERE** QUERY_ID = 1 **ORDER BY** **SEQ**;
```

```sql
mysql> show profile for query 1;
+----------------------+----------+
| Status               | Duration |
+----------------------+----------+
| starting             | 0.000049 |
| checking permissions | 0.000005 |
| Opening tables       | 0.000014 |
| init                 | 0.000018 |
| System lock          | 0.000007 |
| optimizing           | 0.000006 |
| statistics           | 0.000044 |
| preparing            | 0.000007 |
| executing            | 0.000002 |
| Sending data         | 0.000010 |
| end                  | 0.000003 |
| query end            | 0.000005 |
| closing tables       | 0.000005 |
| freeing items        | 0.000010 |
| cleaning up          | 0.000011 |
+----------------------+----------+
```

## ****쿼리 프로파일링 비활성화****

---

쿼리 프로파일일 비활성화는 다시 0 으로 입력 하면 됩니다.

```sql
mysql> set profiling=0;

mysql> show variables like '%profiling%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| profiling              | OFF   |
+------------------------+-------+
```

# [SET문을 통한 변수 설정하여 쿼리](https://lightblog.tistory.com/190)

---

test 라는 테이블에서 어제자 데이터를 출력해야 하는 상황을 생각해 보자.

```sql
SELECT * FROM test WHERE date >= '2018-06-18'  and  date < '2018-06-19';
```

대략 위와 같이 쿼리를 구성하면 원하는 자료를 출력할 수 있을 것이다.

하지만 위와 같은 작업을 매일 반복해야 한다면 변수를 쓰면 좋을 것 같은 생각이 든다.

쿼리에도 변수를 사용할 수 있을까?

가능하다. SET 명령을 사용하며 변수 앞에는 @이 붙는다.

```sql
SET @var = 1;
SELECT * FROM test WHERE date >= subdate(curdate(), @var)  and  date < subdate(curdate(), @var-1);
```

위와 같이 쿼리를 구성하면 @var 의 값만 변경하여 원하는 효과를 얻을 수 있다.

다만 쿼리가 두 개로 나누어지므로 매번 두 개의 쿼리를 각각 한 번씩 실행시켜 주어야 한다는 점은 조금 아쉽다.

[MySQL 메뉴얼](https://dev.mysql.com/doc/refman/8.0/en/user-variables.html)에 따르면 @var := 1 과 같이 콜론과 이퀄을 이용해 변수를 세팅하고 SELECT 문 안쪽에 한 번에 작성할 수도 있다고는 한다.

```sql
SELECT @var := 1, * FROM test WHERE date >= subdate(curdate(), @var) and date < subdate(curdate(), @var-1);
```

대략 위와 같은 문법이 되는 셈, 하지만 메뉴얼에서는 이렇게 사용하는 것을 권장하지 않으며,

이렇게 사용할 경우 아마도 의도한 결과를 얻겠지만(might get) 그 정확성을 보장할 수는 없다고(not guaranteed) 한다.

# PRIMARY키 중복되는 경우에 대한 INSERT문

---

- **INSERT IGNORE**

```sql
mysql> insert into insert_ign values(1,1),(2,2),(3,3);
// INSERT INTO [TABLE명](변경 하고자 하는 컬럼) VALUES ();

// 출처: https://smartjuho.tistory.com/entry/INSERT-IGNORE-REPLACE [개발잡동사니:티스토리]
```

**기본 INSERT쿼리를 실행하는 데, 중복 키가 발생할 경우 구문 오류는 발생하지 않는다.**

**// PRIMARY 키 중복나면 오류창만 뜨고 그대로 진행**

대신 0 rows affected를 확인할 수 있다.

존재하지 않는 ROW에 대해서는 신규로 입력할 수 있지만, 기존 ROW를 업데이트할 수는 없다.

그러므로 ROW데이터의 갱신은 필요치 않고, 단순히 없을 때에만 INSERT 하는 작업에 알맞다.

(Auto_Increment 값은 변하지 않는다.)

하지만, 여기서 디벨롭하여 rows affected value를 체크하여 INSERT 성공 건수가 없다면,

이미 로우가 존재하는 걸로 판단 후 UPDATE 쿼리를 수행할 수 있다.

- **REPLACE INTO**

```sql
mysql> replace into insert_ign values(1,10),(2,20),(3,30);
// REPLACE INTO [TABLE명](변경 하고자 하는 컬럼) VALUES ()

// 출처: https://smartjuho.tistory.com/entry/INSERT-IGNORE-REPLACE [개발잡동사니:티스토리]
```

**갱신(수정)하고자 하는 ROW데이터가 있으면 UPDATE, 없으면 INSERT 한다.
// PRIMARY 키 중복나면 해당 값 삭제 후 진행**

REPLACE INTO는 내부적으로 무조건 PK값에 해당하는 ROW를 DELETE 하고 INSERT 한다.

(성능은그닥그닥다그닥)

1000개의 row일 때 2000번의 쿼리 실행

로직단에서 분기 처리하는 것과 비교했을 때, 코드를 간결하게 할 수 있다.

또한, ROW가 많아질수록 이점이 있는데 REPLACE INTO는 Multiplerow가 가능하다.

(물론, BULK INSERT도 가능하지만 UPDATE는 Multiplerow가 안되므로)

**주의할 점**

1. Not Null 필드인 컬럼이 존재할 때

(변경하고자 하는 컬럼)을 명시할 때 Not Null인 컬럼과 value를 지정해주지 않을 경우

field doesn't have a default value 오류를 만난다.

당연한 부분인데, ROW를 DELETE 한 후에 INSERT 하는데 기존 데이터가 어디 있겠나

~~해당 컬럼에 default value를 선언해주어도 REPLACE INTO에서는 지정이 안된다.~~

(예외: auto_increment 속성)

2. Auto_Increment 컬럼

DELETE후 INSERT이니 auto_increment가 1씩 증가하므로 해당 필드가 레코드 식별에 큰 역할인지 확인이 필요하다.

- **INSERT IGNORE** vs **REPLACE INTO**

REPLACE INTO는 ROW가 있든, 없든 알아서 지우고 INSERT 하기에 편리할 수 있다.

하지만 성능상으로 봤을 때, 우리는 ROW가 있을때엔 INSERT 없을 때엔 UPDATE로 작업을 나누어,

한 개의 데이터는 한 개의 작업을 하도록 해야 한다.

당장 하나의 데이터일 땐 2번의 작업(DELETE, INSERT)이지만, 천 건, 만 건, 몇 만건이 되었을 때

불필요한 작업으로 인한 성능 저하는 당연한 수순