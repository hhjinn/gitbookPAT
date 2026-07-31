# 비정상 노드 조치 가이드

## 노드 간 연관 관계 인식 실패

데이터베이스 탐색 과정에서 일부 노드 간의 연관 관계를 정확히 파악하지 못하는 경우가 발생할 수 있습니다. 이 경우 아래 방법으로 수동 설정을 진행합니다.

1. 연관된 모든 DB 노드의 Agent 설치 경로로 이동합니다.
2. 각 노드에서 `db_scan.info` 파일을 생성합니다.
3. 파일에 동일한 TSC ID를 입력합니다.

```
tsc_id={고유한 숫자 값}
```

예를 들어 4개의 노드로 구성된 TSC 클러스터의 경우 아래와 같이 설정합니다.

```
Node 1의 Agent 경로/db_scan.info → tsc_id=262
Node 2의 Agent 경로/db_scan.info → tsc_id=262
Node 3의 Agent 경로/db_scan.info → tsc_id=262
Node 4의 Agent 경로/db_scan.info → tsc_id=262
```

{% hint style="info" %}
**참고**

- TSC ID는 고유한 값을 사용해야 합니다.
- 동일한 DB 구성에 속한 모든 노드는 반드시 같은 TSC ID를 사용해야 합니다.
- 설정 완료 후 DB 스캔을 재시도하면 클러스터 구성이 정상적으로 인식됩니다.
{% endhint %}
