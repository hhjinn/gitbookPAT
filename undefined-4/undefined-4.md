# 마이그레이션

기존에 사용 중인 데이터베이스를 OwlDB에서 사용하기 위해 마이그레이션 기능을 제공합니다. 마이그레이션 진행 전 호환성 평가를 위한 Analyzer와, 이관을 진행하는 Migrator 기능이 있습니다. 데이터베이스 운영 상태가 `Running`일 때만 Analyzer와 Migrator 수행이 가능합니다. 단, Disaster Recovery (DR) 구성인 데이터베이스는 마이그레이션 기능을 제공하지 않습니다. 마이그레이션을 진행하려면 DR 구성을 해제하거나 다른 데이터베이스를 선택해 주십시오.

{% hint style="info" %} **참고** 호환성 분석 및 마이그레이션 기능은 현재 Tibero에 한하여 Oracle만 지원합니다. '[지원 범위 및 사양](#JKEmRa7PUCKiF4VoFAVy)' 페이지를 참고하시기 바랍니다. {% endhint %}

# 지원 범위 및 사양

OwlDB에서 제공하는 마이그레이션 기능의 지원 범위와 상세 정보를 확인합니다.

## **지원 데이터베이스**

| 소스 데이터베이스 | 타겟 데이터베이스 |
|-----------|-----------|
| Oracle 11g, 12c, 18c, 19c | Tibero 7  |

{% hint style="info" %} **참고** 현재 CDB 이관만 지원하며, 추후 PDB 이관 지원 예정입니다. {% endhint %}

### **지원 오브젝트**

OwlDB 마이그레이션은 모든 Independent Object와 Dependent Object를 한번에 이관합니다. 개별 Object만 선택적으로 이관하거나 제외하는 기능은 지원하지 않습니다.

| Oracle | Tibero | 비고  |
|--------|--------|-----|
| Constraint | Constraint | Primary Key, Foreign Key, Check, Ref Constraint에 대해 이관을 지원함<br>• Primary Key index/constraint는 모두 constraint로 처리함<br>• Check constraint의 표현식은 Oracle의 DD에 저장된 문장을 이용해 DDL을 생성함 |
| Index  | Index  | • R-TREE 미지원<br>• Domain Index 미지원 |
| Materialized | Materialized | -   |
| Materialized View Log | Materialized View Log | -   |
| Privilege | Privilege | -   |
| PSM    | PSM    | -   |
| Role   | Role   | -   |
| Schema | Schema | -   |
| Sequence | Sequence | -   |
| Synonym | Synonym | -   |
| Table  | Table  | • Nested Table 미지원<br>• XML Table 미지원 |
| Tablespace | Tablespace | 테이블 스페이스의 크기는 20% 증대하여 이관함 -> 데이터 이관 시 용량이 TO-BE에서보다 커질 수 있기 때문 |
| View   | View   | -   |

### **데이터 변환 타입**

Oracle에서 Tibero로 이관할 때 변환되는 데이터 타입에 대해 안내합니다.

| Oracle | Tibero |
|--------|--------|
| blob   | BLOB   |
| binary_float | BINARY_FLOAT |
| binary_double | BINARY_DOUBLE |
| character | CHAR   |
| clob   | CLOB   |
| date   | DATE   |
| interval day to second | INTERVAL DAY(2) TO SECOND(6) |
| interval year to month | INTERVAL YEAR(2) TO MONTH |
| long   | LONG   |
| long raw | LONG RAW |
| nchar  | NCHAR  |
| nclob  | NCLOB  |
| number | NUMBER |
| nvarchar2 | NVARCHAR2 |
| rowid  | ROWID  |
| time   | TIME   |
| timestamp | TIMESTAMP |
| timestamp with time zone | TIMESTAMP WITH TIME ZONE |
| timestamp with local time zone | TIMESTAMP(6) WITH LOCAL TIME ZONE |
| varchar | VARCHAR |
| varchar2 | VARCHAR2 |
| xmltype | XMLTYPE |


---

# Analyzer


1. **OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Analyzer** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 호환성을 평가할 데이터베이스를 선택합니다.
3. **분석** 버튼을 클릭합니다.
4. 호환성을 평가할 소스 데이터베이스(이하 소스 데이터베이스)의 정보를 입력합니다.

| 항목  | 설명  |
|-----|-----|
| Title\* | 데이터베이스 호환성 평가 제목 |
| Type\* | 소스 데이터베이스의 엔진 유형 |
| ID\* | 소스 데이터베이스의 사용자 ID |
| Password\* | 소스 데이터베이스의 사용자 PW |
| Address\* | 소스 데이터베이스의 IP 주소 이름 |
| Port\* | 소스 데이터베이스의 포트 번호 |
| SID\* | 소스 데이터베이스의 SID |

\*표기는 필수 입력 항목을 의미합니다.


1. **분석** 버튼을 클릭합니다.
2. **OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Analyzer > 상태** 클릭 시, 진행 정보를 확인할 수 있습니다.

## **Analyzer 결과**

**OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Analyzer > Analyzer Title** 클릭 시, Analyzer 결과를 확인할 수 있습니다.


---

# Migrator


1. **OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Migrator** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 이관을 수행할 데이터베이스를 선택합니다.
3. **이관** 버튼을 클릭합니다.
4. 이관을 진행할 소스 데이터베이스의 정보를 입력합니다.

{% tabs %} {% tab title="Data Connection" %} 마이그레이션 대상이 되는 소스 데이터베이스에 접속을 수행합니다.

| 항목  | 설명  |
|-----|-----|
| Title\* | 데이터베이스 이관 제목 |
| Type\* | 소스 데이터베이스의 엔진 유형 |
| ID\* | 소스 데이터베이스의 사용자 ID |
| Password\* | 소스 데이터베이스의 사용자 PW |
| Host\* | 소스 데이터베이스의 IP 주소 이름 |
| Port\* | 소스 데이터베이스의 포트 번호 |
| SID\* | 소스 데이터베이스의 SID |
| Target Database\* | 타겟 데이터베이스의 별칭 |

\*표기는 필수 입력 항목을 의미합니다. {% endtab %} {% tab title="Type Conversion" %} 소스 데이터베이스의 데이터 타입 변환에 관한 정보를 조회합니다.

{% hint style="info" %} **참고** 타입이 변환된 데이터 유형은 주황색 하이라이팅으로 강조하여 표현합니다. {% endhint %} {% endtab %} {% tab title="Summary" %} 앞 단계에서 입력한 정보를 요약해서 제공합니다. {% endtab %} {% endtabs %}


1. **이관** 버튼을 클릭합니다.
2. **OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Migrator > 상태** 클릭 시, 진행 정보를 확인할 수 있습니다.

## **Migrator 결과**

**OwlDB 콘솔 화면 > 관리 > 마이그레이션 > Migrator > Migrator Title** 클릭 시, **Migrator 결과**를 확인할 수 있습니다.
