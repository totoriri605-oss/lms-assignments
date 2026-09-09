# Chapter 03 확장 실습 답안 템플릿

> **과제:** PostgreSQL과 DBeaver로 실습 환경 검증하기  
> **사용 방법:** 이 파일을 내려받아 본인의 GitHub 저장소에 `chapter03_answer.md`라는 이름으로 저장한 뒤 실습하면서 바로 작성합니다.  
> **제출 방법:** LMS에는 파일을 직접 업로드하지 않고, **본인 GitHub 저장소의 `chapter03_answer.md` 파일 URL**을 제출합니다.

---

## 제출 전 보안 주의

이 과제 파일과 캡처 화면에는 다음 정보를 올리지 않습니다.

```text
실제 PostgreSQL 비밀번호
전체 DB 접속 URL
API Key / Token
개인정보
공개할 필요가 없는 사내 서버 주소
```

LMS에서 제출자를 확인할 수 있으므로 공개 저장소의 답안 파일에 학번이나 실명을 반드시 적을 필요는 없습니다.

```text
GitHub 계정 또는 별칭:
과제 작성일:
사용한 AI 도구:
```

---

# 1. PostgreSQL과 DBeaver 환경 확인

## 1-1. 내 환경

| 항목 | 작성 내용 |
| --- | --- |
| 운영체제 |  |
| PostgreSQL 버전 |  |
| DBeaver 버전 |  |
| Host | 비밀정보가 아니라면 기록, 아니면 `localhost`/`마스킹` |
| Port |  |
| Database |  |
| Username | 필요하면 마스킹 |

> 비밀번호는 기록하지 않습니다.

## 1-2. PostgreSQL과 DBeaver 역할 설명

```text
PostgreSQL은:데이터를 저장하고 SQL을 실행하는 관계형 데이터베이스 관리 시스템(DBMS)이다.

DBeaver는: PostgreSQL 같은 데이터베이스에 연결해서 SQL을 작성하고 실행 결과를 확인하는 클라이언트 프로그램이다.

두 프로그램의 차이는:PostgreSQL = 실제로 데이터를 저장하고 처리하는 프로그램
DBeaver = 그 PostgreSQL을 사람이 편하게 다루도록 도와주는 프로그램이다.
```

---

# 2. 연결 테스트와 첫 SQL

## 2-1. DBeaver 연결 결과

- [ ] PostgreSQL 연결 유형 선택
- [ ] Host 확인
- [ ] Port 확인
- [ ] Database 확인
- [ ] Username 확인
- [ ] Test Connection 성공

### 연결 성공 화면

권장 이미지 경로:

```text

```

![현재 DB/사용자/스키마/search_path 확인](./images/step03_location_check.png)

## 2-2. 첫 SQL 실행

```sql
SELECT 1 + 1 AS result;
```

실행 전 예상:

```text
2
```

실제 결과:

```text
2
```

이 결과가 의미하는 것:

```text
DBeaver에서 작성한 SQL이 PostgreSQL에 정상적으로 전달되었고,
PostgreSQL이 계산한 결과가 다시 DBeaver에 표시되는 것을 확인했다.
```

---

# 3. 현재 연결 위치를 SQL로 검증

다음 SQL을 실행합니다.

```sql
SELECT version();
SELECT current_database();
SELECT current_user;
SELECT current_schema();
SHOW search_path;
SHOW transaction_read_only;
SHOW TimeZone;
```

## 3-1. 결과 기록

| 확인 항목 | 실제 결과 | 내가 이해한 의미 |
| --- | --- | --- |
| `version()` |18.6|  |
| `current_database()` |postgres|  |
| `current_user` |postgres|  |
| `current_schema()` |public|  |
| `search_path` |public, "$user"|  |
| `transaction_read_only` |off|  |
| `TimeZone` |Asia/Seoul|  |

## 3-2. 반드시 설명할 것

### DBeaver 연결 이름과 `current_database()`는 왜 같은 개념이 아닌가요?

```text
DBeaver 연결 이름은 사용자가 알아보기 쉽게 붙인 이름일 뿐이고,
current_database()는 PostgreSQL 서버가 실제로 현재 연결되어 있다고 알려주는 데이터베이스 이름이다.
따라서 DBeaver 연결 이름과 실제 접속 데이터베이스 이름은 서로 다를 수 있다.
```

### `current_schema()`와 `search_path`는 어떤 관계가 있나요?

```text
search_path는 스키마 이름을 생략했을 때 PostgreSQL이 어떤 스키마 순서로 객체를 찾을지 정하는 경로이다.
current_schema()는 그 search_path 안에서 현재 실제로 사용할 수 있는 첫 번째 스키마를 보여준다.
따라서 두 값은 서로 관련되어 있지만 같은 개념은 아니다.
```

### `transaction_read_only = off`라는 결과만으로 모든 테이블을 만들 권한이 있다고 단정할 수 있나요?

```text
아니다.
transaction_read_only = off는 현재 세션이 읽기 전용으로 강제되어 있지 않다는 뜻이다.
하지만 이것만으로 모든 데이터베이스, 스키마, 테이블에 CREATE나 INSERT 같은 권한이 있다는 뜻은 아니다.
실제 권한은 데이터베이스, 스키마, 테이블별로 따로 확인해야 한다.
```

## 3-3. 증거 화면

권장 경로:

```text
assignments/chapter03/images/step03_location_check.png
```

![현재 DB/사용자/스키마/search_path 확인](./images/step03_location_check.png)

---

# 4. `ai_database_book` 데이터베이스 확인

## 4-1. 현재 데이터베이스

```sql
SELECT current_database();
```

실제 결과:

```text
postgres
```

- [ ] 결과가 `ai_database_book`이다.
- [ ] 다른 DB라면 올바른 연결로 전환했다.

## 4-2. 연결을 바꾼 뒤 다시 검증

```text
전환 전 데이터베이스:postgres
전환 후 데이터베이스:ai_database_book
전환 여부를 판단한 근거:SELECT current_database();를 다시 실행했을 때
결과가 ai_database_book으로 나오는 것을 확인했다.
```

### 화면에서 보이는 연결 이름만 믿지 않고 SQL을 다시 실행해야 하는 이유

```text
DBeaver에 표시되는 연결 이름은 사용자가 붙인 이름일 수 있기 때문에
실제로 어떤 데이터베이스에 연결되어 있는지 확실하지 않을 수 있다.

따라서 화면에 보이는 이름만 믿지 않고
SELECT current_database();를 다시 실행해서
현재 실제 연결된 데이터베이스를 확인해야 한다.
```

---

# 5. SQL 실행 범위 실험

SQL Editor에 다음 세 문장을 입력합니다.

```sql
SELECT 'A' AS step;
SELECT 'B' AS step;
SELECT 'C' AS step;
```

## 5-1. 한 문장 실행

```text
내가 실행한 문장:SELECT 'A' AS step;
실제 결과:A
```

## 5-2. 선택 영역 실행

```text
선택한 문장:
SELECT 'A' AS step;
SELECT 'B' AS step;
실제 결과:A와 B문장이 실행되었다.
```

## 5-3. 전체 스크립트 실행

```text
실제 결과:실제 결과:
A, B, C 세 문장이 모두 실행되었다.
결과 탭 또는 실행 순서에서 관찰한 점:
SQL을 어디까지 선택해서 실행했느냐에 따라 실제 실행되는 범위가 달라진다.
```

## 5-4. 결과 해석

```text
한 문장 실행과 전체 스크립트 실행의 차이:
한 문장 실행은 현재 선택한 SQL 한 문장만 실행하지만,
전체 스크립트 실행은 작성된 여러 SQL 문장을 모두 실행한다.

변경 SQL에서 실행 범위를 잘못 선택하면 위험한 이유:SELECT만 실행하려고 했는데 UPDATE나 DELETE까지 함께 실행되면
원하지 않는 데이터 수정이나 삭제가 발생할 수 있기 때문이다.
따라서 실행 전에 선택된 SQL 범위를 반드시 확인해야 한다.
```

### 증거 화면

권장 경로:

```text
assignments/chapter03/images/step05_execution_scope.png
```

![SQL 실행 범위 비교](./images/step05_execution_scope.png)

---

# 6. 제공된 환경 확인 SQL 실행

Public 저장소의 Chapter 03 파일을 사용합니다.

```text
code/chapter03/setup_check.sql
code/chapter03/setup_validate_local.sql
```

## 6-1. `setup_check.sql`

실행 결과에서 확인한 항목:

```text.
PostgreSQL 버전:18.6
현재 DB:ostgres
현재 사용자:postgres
현재 스키마:public
search_path:public, "$user"
읽기 전용 여부:off
TimeZone:Asia/Seoul
1 + 1 결과:2
public 스키마 존재 여부: 있음 (true)
public USAGE 권한: 있음 (true)
public CREATE 권한: 있음 (true)
```

### 이 파일을 여러 번 실행해도 비교적 안전한 이유

```text
setup_check.sql은 현재 환경을 확인하기 위한 SELECT와 SHOW 중심의 조회 SQL로 구성되어 있고,
데이터를 수정하거나 삭제하는 DROP, DELETE, UPDATE, INSERT 같은 변경 SQL이 없기 때문에
여러 번 실행해도 비교적 안전하다.
```

## 6-2. `setup_validate_local.sql`

```text
실행 결과:Chapter 03 recommended local environment validation passed

PASS / FAIL:PASS
```
실패했다면 실패 항목:없음

```text

```

그 실패가 실제 문제인지 환경 차이인지 판단한 근거:

```text
실패 항목이 없었고, 권장 로컬 환경 검증이 정상적으로 통과했기 때문에
현재 실습 환경이 Chapter 03의 권장 조건을 만족한다고 판단했다.
```

---

# 7. 안전한 오류 진단 실습

실제 오류가 있었다면 그 오류를 사용합니다. 오류가 없었다면 **데이터를 삭제하거나 서버를 강제로 중지하지 말고**, 안전한 SQL 문법 오류를 하나 만들어 관찰합니다.

예:

```sql
SELEC 1;
```

> 오류를 확인한 뒤 올바른 `SELECT 1;`로 복구합니다.

## 7-1. 오류 기록

```text
오류 메시지 핵심 문장:syntax error at or near "SELEC"


내가 먼저 생각한 원인 1:SQL 명령어의 철자가 잘못되었을 가능성이 있다고 생각했다.


내가 먼저 생각한 원인 2:
PostgreSQL 서버 연결에 문제가 있을 가능성도 생각했다.

실제로 확인한 방법:
오류 메시지에서 SELEC 부분을 확인하고,
SELECT의 마지막 T가 빠진 것을 확인했다.
또한 기존 연결이 정상적으로 유지되는지도 확인했다.

실제 원인:
SELECT를 SELEC로 잘못 입력한 SQL 문법 오류였다.

수정한 내용:
SELEC 1;을 SELECT 1;로 수정했다.
```

## 7-2. 수정 후 재검증

```sql
SELECT 1;
SELECT current_database();
```

```text
재검증 결과:poostgres
```

## 7-3. 오류를 유형으로 분류

- [ ] 서버 실행 문제
- [ ] Host 문제
- [ ] Port 문제
- [ ] Database 문제
- [ ] Username/인증 문제
- [o] SQL 문법 문제
- [ ] 권한 문제
- [ ] 기타

선택 이유:

```text
오류 메시지에 syntax와 SELEC가 표시되었고,
SELECT의 철자를 수정한 뒤 정상적으로 실행되었기 때문에
SQL 문법 문제라고 판단했다.
```

---

# 8. AI를 오류 분석 보조 도구로 사용

## 8-1. AI에게 전달한 프롬프트

비밀번호·개인정보·전체 접속 URL은 제거하고 기록합니다.

```text
나는 PostgreSQL과 DBeaver를 처음 배우는 학생입니다.

다음 SQL을 실행했습니다.

SELEC 1;

실행 후 syntax error가 발생했습니다.

오류를 하나의 원인으로 바로 단정하지 말고,
다음 순서로 설명해 주세요.

1. 오류 메시지에서 확인되는 사실
2. 가능한 원인 후보
3. 안전하게 확인하는 방법
4. 수정 방법
5. 수정 후 다시 확인할 방법

비밀번호나 개인정보는 포함하지 않았습니다.
```

## 8-2. AI 답변 검토

| AI가 제안한 확인 방법 | 실제로 확인했는가? | 결과 | 수용 / 수정 / 거절 |
| --- | --- | --- | --- |
| SQL 철자 확인 | 예 | SELEC에서 T가 빠진 것을 확인함 | 수용 |
| SELECT 1;로 수정 후 실행 | 예 | 결과 1이 정상 출력됨 | 수용 |
| current_database()로 연결 상태 재확인 | 예 | 현재 데이터베이스 이름이 정상 출력됨 | 수용 |

### AI가 오류 원인을 너무 빨리 단정한 부분이 있었나요?

```text
이번 오류에서는 SELEC라는 잘못된 철자가 명확하게 보여 문법 오류 가능성이 높았다.
하지만 AI의 설명만으로 바로 확정하지 않고 실제 SQL 철자와 실행 결과를 확인했다.
```

### 오류 메시지와 실제 환경 중 무엇을 확인해서 최종 판단했나요?

```text
오류 메시지에 syntax error와 SELEC가 표시된 것을 확인했고,
SELECT 1;로 수정한 뒤 정상 실행되는 것을 확인했다.
또한 current_database()가 정상 실행되는 것을 보고 서버 연결 문제는 아니라고 판단했다.
```

### AI 활용에서 가장 유용했던 점

```text
오류의 가능한 원인을 정리하고,
초보자가 무엇부터 확인해야 하는지 순서를 제시해 준 점이 가장 유용했다.
```

### AI 답변을 그대로 실행하지 않고 확인해야 하는 이유

```text
AI는 내 실제 실행 환경을 완전히 알 수 없고 잘못된 원인을 제안할 수도 있기 때문이다.
특히 데이터 삭제, 권한 변경, 재설치 같은 명령은 문제가 더 커질 수 있으므로
AI의 제안을 실제 오류 메시지와 실행 결과를 통해 하나씩 검증해야 한다.
```

---

# 9. Chapter 01~02 개인 서비스와 연결

앞에서 선택한 개인 서비스가 PostgreSQL을 사용한다고 가정합니다.

```text
서비스 이름:
PMU 고객관리 서비스

사용할 데이터베이스 이름 후보:
ai_database_book

사용할 스키마 이름 후보:
pmu_project

앞으로 만들고 싶은 테이블 후보 3개:

1. clients
2. treatments
3. retouch_records
```

### 아직 SQL을 만들지 않고 이름과 역할만 정하는 이유

```text
아직 테이블의 구조와 관계를 충분히 정하지 않았기 때문이다.
먼저 어떤 데이터를 관리할지와 각 테이블의 역할을 생각한 뒤,
이후 Chapter에서 SQL과 데이터 모델링을 배운 후 구조를 확정하는 것이 더 안전하다고 생각했다.
```

### Chapter 02에서 정리했던 한 행의 의미 중 수정할 부분이 있나요?

```text
고객 테이블의 한 행은 고객 한 명,
시술 테이블의 한 행은 한 번의 시술 기록,
리터치 기록 테이블의 한 행은 한 번의 리터치 기록으로 구분하면 더 명확하다고 생각했다.
```

---

# 10. 초보자용 연결 가이드 작성

친구가 자신의 PC에서 같은 실습을 시작한다고 가정합니다. 아래 순서를 자신의 말로 작성합니다.

```text
1. PostgreSQL 서버가 실행되는지 확인하는 방법:
Windows 서비스나 DBeaver 연결 테스트를 통해 PostgreSQL 서버가 실행 중인지 확인한다.

2. DBeaver에서 PostgreSQL 연결을 만드는 방법:
DBeaver에서 PostgreSQL 연결을 선택한 뒤 Host, Port, Database, Username, Password를 입력하고 Test Connection으로 연결이 되는지 확인한다.

3. Host / Port / Database / Username의 의미:
Host는 PostgreSQL 서버가 실행되는 위치이고,
Port는 PostgreSQL이 연결을 받는 번호이며,
Database는 서버 안에서 접속할 데이터베이스이고,
Username은 어떤 PostgreSQL 사용자로 접속할지를 의미한다.

4. ai_database_book에 연결되었는지 확인하는 방법:
DBeaver 화면의 연결 이름만 믿지 않고
SELECT current_database();를 실행해서 실제 결과가 ai_database_book인지 확인한다.

5. 현재 위치를 확인하는 SQL:
SELECT current_database();
SELECT current_user;
SELECT current_schema();
SHOW search_path;

6. 한 문장과 전체 스크립트 실행을 구분해야 하는 이유:
한 문장만 실행하면 원하는 SQL만 실행되지만,
전체 스크립트를 실행하면 작성된 여러 SQL 문장이 모두 실행될 수 있다.
UPDATE나 DELETE 같은 변경 SQL이 함께 있으면 원하지 않는 데이터 변경이 일어날 수 있으므로 실행 범위를 확인해야 한다.

7. 비밀번호를 GitHub나 AI 프롬프트에 넣으면 안 되는 이유:
비밀번호가 외부에 공개되면 다른 사람이 데이터베이스에 접근하거나 계정을 악용할 수 있기 때문이다.
따라서 비밀번호와 전체 접속 URL 같은 민감한 정보는 공개 저장소나 AI 프롬프트에 넣지 않아야 한다.
```

---

# 11. 최종 성찰

아래 문장은 반드시 본인의 말로 작성합니다.

```text
1. DBeaver와 PostgreSQL의 가장 중요한 차이는

   DBeaver는 데이터베이스에 연결해서 SQL을 작성하고 결과를 확인하는 클라이언트이고,
   PostgreSQL은 실제 데이터를 저장하고 SQL을 실행하는 DBMS라는 점이다.

2. 내가 지금 어느 데이터베이스에 연결되어 있는지 확인할 때

   화면 이름만 보지 않고 SELECT current_database();를 직접 실행해서 실제 연결된 데이터베이스를 확인해야 한다.

3. PostgreSQL 오류가 발생했을 때 가장 먼저 해야 할 일은

   오류 메시지를 먼저 읽고 어느 단계에서 문제가 발생했는지 확인한 뒤 원인 후보를 생각하는 것이다.

4. AI를 오류 해결에 사용할 때 가장 중요한 것은

   AI의 답변을 그대로 실행하지 않고 실제 오류 메시지와 내 환경의 실행 결과를 이용해 하나씩 검증하는 것이다.
```

---

# 12. 제출 체크리스트

- [ ] `chapter03_answer.md`의 빈 필수 항목을 작성했다.
- [ ] PostgreSQL과 DBeaver의 역할 차이를 설명했다.
- [ ] `current_database/current_user/current_schema/search_path`를 실제로 확인했다.
- [ ] `ai_database_book` 연결 여부를 SQL로 검증했다.
- [ ] SQL 실행 범위 세 가지를 비교했다.
- [ ] `setup_check.sql`을 실행했다.
- [ ] `setup_validate_local.sql` 결과를 확인했다.
- [ ] 오류 원인을 먼저 스스로 추정한 뒤 AI를 사용했다.
- [ ] AI 제안을 실제 환경에서 검증했다.
- [ ] 핵심 캡처 3~4장만 골라 넣었다.
- [ ] 캡처에 비밀번호·개인정보·전체 접속 URL이 없다.
- [ ] Markdown 이미지가 GitHub 웹 화면에서 실제로 보인다.
- [ ] 최종 답안 파일을 commit/push했다.

---

# 13. LMS 제출 URL

아래 형식의 **본인 GitHub 파일 URL**을 LMS에 제출합니다.

```text
https://github.com/<본인-GitHub-ID>/<본인-저장소>/blob/main/assignments/chapter03/chapter03_answer.md
```

내 제출 URL:

```text

```

> 저장소 메인 URL, 교수자 템플릿 URL, Raw URL이 아니라 **작성 완료된 본인 `chapter03_answer.md` 파일 화면 URL**을 제출합니다.
