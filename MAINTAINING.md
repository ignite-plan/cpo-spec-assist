# 🛠️ 수정·관리 가이드

스킬 내용을 고치거나 새 규칙을 넣고 싶을 때 따라 하시는 문서입니다. 코딩 경험이 없으셔도 **Claude에게 시키면서** 진행하실 수 있습니다.

## 🧭 어디를 고치나요

| 바꾸고 싶은 것 | 고칠 파일 |
|---|---|
| 목업 규칙 (색, 부품, 캔버스 등) | `skills/cpo-mockup/SKILL.md` |
| description 문체·구성·관용 표현 | `skills/cpo-description/SKILL.md` |
| 세션 시작 때 Claude에게 주는 메모 | `hooks/session-note.txt` |
| 플러그인 버전·설명 | `.claude-plugin/plugin.json` |

## 🔁 수정 순서

```mermaid
flowchart LR
    A[저장소 받기] --> B[파일 수정]
    B --> C[내 PC에서 시험]
    C --> D[버전 올리기]
    D --> E[커밋·푸시]
    E --> F[팀원이 업데이트]
```

### 1. 저장소 받기

```text
git clone https://github.com/ignite-plan/cpo-spec-assist.git
cd cpo-spec-assist
```

### 2. 파일 수정

이 폴더에서 Claude Code를 열고 요청하셔도 됩니다.

```text
skills/cpo-description/SKILL.md 의 "문체" 절에서 ~를 ~로 바꿔 줘.
```

스킬 파일 맨 위의 `---`로 둘러싼 부분(`name`, `description`)은 Claude가 **언제 이 스킬을 쓸지** 판단하는 근거입니다. 의미를 바꾸는 경우가 아니라면 건드리지 마세요.

### 3. 내 PC에서 시험

저장소 폴더에서 아래처럼 실행하면, 설치하지 않고 수정한 내용을 바로 적용해서 시험할 수 있습니다.

```text
claude --plugin-dir .
```

그 상태에서 "CPO 요건이야. 목업 그려줘"처럼 요청해 보시고 규칙이 반영됐는지 확인하세요.
형식이 깨지지 않았는지는 `claude plugin validate .`로 확인합니다.

### 4. 버전 올리기

`.claude-plugin/plugin.json`의 `version`을 올려 주세요(예: `1.0.0` → `1.0.1`). **버전을 올리지 않으면 팀원에게 업데이트가 전달되지 않을 수 있습니다.**

| 변경 | 올리는 자리 |
|---|---|
| 오타, 문구 다듬기 | 맨 끝 자리 (1.0.0 → 1.0.1) |
| 규칙 추가·수정 | 가운데 자리 (1.0.0 → 1.1.0) |
| 스킬 추가·삭제 등 큰 변경 | 맨 앞 자리 (1.0.0 → 2.0.0) |

### 5. 커밋·푸시

```text
git add -A
git commit -m "무엇을 바꿨는지 한 줄"
git push
```

### 6. 팀원에게 반영

팀원은 아래를 실행하고 Claude Code를 다시 시작하면 됩니다.

```text
/plugin update cpo-spec-assist@cpo-spec-assist
```

## ➕ 새 스킬을 추가하려면

1. `skills/<스킬이름>/SKILL.md`를 만들고, 맨 위에 `name`과 `description`을 작성합니다.
2. 세션 시작 메모에도 반영하려면 `hooks/session-note.txt`에 한 줄을 추가합니다.
3. `README.md`의 표에 한 줄을 추가합니다.
4. 위 3~6단계를 따릅니다.

기존 스킬(`cpo-mockup`)을 본보기로 삼으시면 됩니다.

## ✅ 규칙을 고칠 때 지킬 것

- 작업 방식이나 설정이 충돌하면 **기획자의 방식이 우선**이라는 원칙을 유지합니다.
- 요건에 없는 값은 지어내지 않고 `{미정}`으로 남기는 원칙을 유지합니다.
- 커밋 메시지에는 작업 내용만 작성합니다.

## 🚑 문제가 생기면

| 증상 | 확인할 것 |
|---|---|
| 푸시했는데 팀원에게 반영이 안 됨 | `version`을 올렸는지, 팀원이 `/plugin update` 후 재시작했는지 |
| 스킬이 아예 안 불림 | `/skills`에서 켜져 있는지(✔), `SKILL.md` 맨 위 `name`·`description`이 있는지 |
| 형식 오류 | `claude plugin validate .` 결과 확인 |
| 잘못 올린 변경 되돌리기 | `git revert <커밋>` 후 푸시하고 버전을 올립니다 |
