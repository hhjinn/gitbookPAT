OwlDB에서 운영 중인 데이터베이스의 저장 공간(테이블스페이스·데이터 파일)을 조회·생성·수정·삭제합니다. 하나의 테이블스페이스는 여러 개의 데이터 파일을 포함할 수 있습니다.

{% hint style="info" %}
**참고**

- 데이터베이스 상태가 `Running`인 경우에만 테이블스페이스 조회·수정·삭제가 가능합니다.
- DB 엔진이 OpenSQL로 설정된 경우에는 이 페이지 대신 데이터베이스 관리 화면이 표시됩니다.
{% endhint %}

> 📷 **이미지** 이미지

# 테이블스페이스 조회

1. **OwlDB 콘솔 화면 > 관리 > 테이블스페이스** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 테이블스페이스를 조회할 데이터베이스를 선택합니다.
3. 테이블스페이스 목록을 조회합니다. 유형으로 목록을 필터링할 수 있습니다. 이름으로 직접 검색할 수 있습니다.

<table><thead><tr><th>항목</th><th>설명</th></tr></thead><tbody><tr><td>Name</td><td>테이블스페이스 이름</td></tr><tr><td>Type</td><td>테이블스페이스 유형<ul><li><strong>Permanent</strong>: 영구적인 데이터를 저장하며 일반적으로 가장 많이 사용하는 유형</li><li><strong>Temporary</strong>: 임시 데이터를 저장하며 세션이 종료되거나 작업 완료 시 데이터 삭제</li><li><strong>Undo</strong>: 수정된 데이터에 대해 저장</li></ul></td></tr><tr><td>Status</td><td>테이블스페이스 상태<ul><li><strong>ONLINE</strong>: 데이터베이스에 정상적으로 연결되어 사용할 수 있는 상태</li><li><strong>OFFLINE</strong>: 데이터베이스에 연결이 끊긴 상태 (테이블스페이스에 저장된 객체에 접근 불가)</li></ul></td></tr><tr><td>Used/Total Size</td><td><ul><li><strong>Used Size</strong>: 해당 테이블스페이스에 속한 데이터 파일이 사용 중인 공간의 총합</li><li><strong>Total Size</strong>: 해당 테이블스페이스에 속한 데이터 파일이 할당받은 공간의 총합</li></ul></td></tr><tr><td>Total/Max Size</td><td><ul><li><strong>Total Size</strong>: 해당 테이블스페이스에 속한 데이터 파일이 할당받은 공간의 총합</li><li><strong>Max Size</strong>: 해당 테이블스페이스에 속한 데이터 파일이 최대로 할당받을 수 있는 공간의 총합</li></ul></td></tr><tr><td>Logging</td><td>로깅 여부</td></tr><tr><td>Allocation Type</td><td>Extent 할당 방식<ul><li><strong>SYSTEM</strong>: 시스템 요구에 따라 동적으로 extent 크기를 할당하는 방식</li><li><strong>UNIFORM</strong>: 동일한 크기의 Extent를 사용하여 오브젝트를 저장하는 방식</li></ul></td></tr><tr><td>Next Extent</td><td>다음 할당 Extent 크기</td></tr></tbody></table>

---

# 테이블스페이스 생성

1. **OwlDB 콘솔 화면 > 관리 > 테이블스페이스** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 테이블스페이스를 생성할 데이터베이스를 선택합니다.
3. **생성** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**

데이터 블록 크기 8KB 기준, UNIFORM SIZE를 128KB 보다 작게 설정하더라도 Extent 최소 크기인 128KB로 설정됩니다.
{% endhint %}

---

# 테이블스페이스 수정

1. **OwlDB 콘솔 화면 > 관리 > 테이블스페이스** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 테이블스페이스를 수정할 데이터베이스를 선택합니다.
3. **수정** 버튼을 클릭합니다.

---

# 테이블스페이스 삭제

1. **OwlDB 콘솔 화면 > 관리 > 테이블스페이스** 메뉴로 이동합니다.
2. **DB Alias** 드롭다운 버튼을 클릭하여 테이블스페이스를 삭제할 데이터베이스를 선택합니다.
3. **삭제** 버튼을 클릭합니다.

---

# 데이터 파일 조회

1. '[테이블스페이스 조회](#테이블스페이스-조회)'를 참고하여 데이터 파일을 조회할 데이터베이스를 선택합니다.
2. **Tablespace** 라디오 버튼을 클릭하여 데이터 파일을 조회할 테이블스페이스를 선택합니다.
3. 데이터 파일 목록을 조회합니다. 자동 확장 여부로 목록을 필터링할 수 있습니다. 데이터 파일 이름으로 직접 검색할 수 있습니다.

<table><thead><tr><th>항목</th><th>설명</th></tr></thead><tbody><tr><td>Tablespace Name</td><td>테이블스페이스 이름</td></tr><tr><td>Name</td><td>데이터 파일 이름</td></tr><tr><td>Online Status</td><td>데이터 파일 상태<ul><li><strong>SYSOFF</strong>: 시스템 오프라인 파일</li><li><strong>SYSTEM</strong>: 시스템 온라인 파일</li><li><strong>OFFLINE</strong>: 오프라인 상태</li><li><strong>ONLINE</strong>: 온라인 상태</li><li><strong>RECOVER</strong>: 복구가 필요한 상태</li><li><strong>AVAILABLE</strong>: 사용 가능한 상태</li></ul></td></tr><tr><td>Used/Total Size</td><td><ul><li><strong>Used Size</strong>: 해당 데이터 파일이 사용 중인 공간의 용량</li><li><strong>Total Size</strong>: 해당 데이터 파일이 할당받은 공간의 용량</li></ul></td></tr><tr><td>Total/Max Size</td><td><ul><li><strong>Total Size</strong>: 해당 데이터 파일이 할당받은 공간의 용량</li><li><strong>Max Size</strong>: 해당 데이터 파일이 최대로 할당받을 수 있는 공간의 용량</li></ul></td></tr><tr><td>Auto Extend</td><td>자동 확장 여부</td></tr><tr><td>Next</td><td>다음 확장 용량</td></tr><tr><td>Physical Reads</td><td>물리적 읽기 횟수</td></tr><tr><td>Physical Writes</td><td>물리적 쓰기 횟수</td></tr><tr><td>Single Block Reads</td><td>단일 블록 읽기 횟수</td></tr></tbody></table>

---

# 데이터 파일 생성

1. '[데이터 파일 조회](#데이터-파일-조회)'를 참고하여 데이터 파일을 생성할 테이블스페이스를 선택합니다.
2. **생성** 버튼을 클릭합니다.

---

# 데이터 파일 수정

1. '[데이터 파일 조회](#데이터-파일-조회)'를 참고하여 데이터 파일을 수정할 테이블스페이스를 선택합니다.
2. **수정** 버튼을 클릭합니다.

---

# 데이터 파일 삭제

1. '[데이터 파일 조회](#데이터-파일-조회)'를 참고하여 데이터 파일을 삭제할 테이블스페이스를 선택합니다.
2. **삭제** 버튼을 클릭합니다.
