이 페이지에서는 OwlDB Agent 연결과 DR 구성에 필요한 노드 간 준비 상태(UID/GID 일치, SSH 키 인증, 키 파일 일치성)를 확인합니다.

## 1. Agent 연결 확인

OwlDB UI에 접속 후 **탐색** 버튼을 눌러 Agent 연결 상태를 확인합니다.

```
http://[OwlDB 서버 IP]:[UI_PORT]/owldb/#/auth/login
```

## 2. UID/GID 일치 확인 (DR 구성 시)

각 노드에서 다음 명령을 실행하여 UID/GID가 **숫자까지 동일한지** 비교합니다.

```bash
# 각 노드에서 실행
id tibero
```

기대 결과 (모든 노드 동일):

```
uid=1100(tibero) gid=1100(dba) groups=1100(dba)
```

UID 또는 GID 숫자가 다르게 출력되는 경우, 해당 노드에서 데이터베이스 서버 공통 준비 사항의 OS 사용자 생성 절차에 따라 사용자를 재생성합니다.

## 3. SSH 키 인증 동작 확인 (DR 구성 시)

각 노드에서 tibero 계정으로 나머지 모든 노드에 SSH 접속을 시도하여, **비밀번호 없이** 접속되는지 확인합니다.

```bash
# Node1(10.10.0.11)에서 tibero 계정으로 실행
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Node2(10.10.0.12)에서 tibero 계정으로 실행
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.13

# Node3(10.10.0.13)에서 tibero 계정으로 실행
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.11
ssh -p 22 -i ~/.ssh/id_ed25519 -o BatchMode=yes tibero@10.10.0.12
```

`BatchMode=yes`는 비밀번호 prompt가 발생하면 즉시 실패하도록 강제하므로, 키 인증 실패를 명확히 검출할 수 있습니다.

## 4. 키 파일 일치성 확인 (DR 구성 시)

배포된 키 파일이 모든 노드에서 동일한지 해시로 검증합니다.

```bash
# 각 노드에서 tibero 계정으로 실행 후 결과 비교 (모든 노드 동일해야 함)
sha256sum ~/.ssh/id_ed25519
sha256sum ~/.ssh/id_ed25519.pub
```

해시가 노드 간에 다르게 출력되는 경우, 데이터베이스 서버 공통 준비 사항의 [키 셋 배포 단계](#id-4)를 다시 수행합니다.
