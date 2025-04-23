# project-flow-start-issue

## 개요

이 액션은 브랜치가 생성될 때 연관된 이슈를 GitHub Project의 지정된 상태(Status)로 업데이트합니다.

---

## ✨ 기능 설명

- 브랜치 이름에서 이슈 번호를 추출합니다.
- 해당 이슈가 등록된 GitHub Project에서 Item을 찾아 상태(Status)를 업데이트합니다.
- 사용자 소유(User-owned)와 조직 소유(Organization-owned) 프로젝트 모두 지원합니다.

---

## 🧾 입력값

| 이름             | 필수 여부 | 기본값        | 설명                                                             |
| ---------------- | --------- | ------------- | ---------------------------------------------------------------- |
| `github_token`   | 예        | –             | `project` 권한이 포함된 GitHub Token (예: PAT 또는 GITHUB_TOKEN) |
| `project_owner`  | 예        | –             | 프로젝트를 소유한 사용자 또는 조직의 이름                        |
| `project_number` | 예        | –             | 프로젝트 번호 (ID가 아님)                                        |
| `target_column`  | 아니오    | `In Progress` | 초기로 설정할 상태(Status) 컬럼 이름                             |

---

## ⚠️ 사용 주의사항

### 브랜치 명에 이슈 번호가 포함되어야 합니다.

이슈 번호가 포함된 브랜치 이름을 사용해야 액션이 정상적으로 작동합니다. (ex: feature/#12)

### `on: create` 트리거 주의

이 액션은 `create` 이벤트를 기반으로 작동합니다. 따라서 태그 또는 릴리즈가 생성될 때도 트리거될 수 있으므로, 아래 조건을 추가하여 **브랜치 생성 시에만 실행**되도록 해야 합니다:

```yaml
if: github.ref_type == 'branch' && contains(github.ref, 'feature/~~~')
```

## 🔁 사용 예시

```yaml
on:
  create:

jobs:
  start-issue:
    if: github.ref_type == 'branch' && contains(github.ref, 'feature')
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Start Issue
        uses: jinhokim98/project-flow-start-issue@v1
        with:
          github_token: ${{ secrets.PERSONAL_TOKEN }}
          project_owner: your-org
          project_number: 42
          target_column: 'In Progress'
```

## Overview

This GitHub Action updates the status of a related issue in a GitHub Project when a new branch is created.

---

## ✨ Features

- Extracts the issue number from the branch name.
- Finds the corresponding project item and updates its status.
- Supports both user-owned and organization-owned projects.

---

## 🧾 Inputs

| Name             | Required | Default       | Description                                                         |
| ---------------- | -------- | ------------- | ------------------------------------------------------------------- |
| `github_token`   | Yes      | –             | GitHub Token with `project` permissions (e.g., PAT or GITHUB_TOKEN) |
| `project_owner`  | Yes      | –             | The owner of the project (user or organization)                     |
| `project_number` | Yes      | –             | The project number (not the project ID)                             |
| `target_column`  | No       | `In Progress` | The name of the column to set as the initial status                 |

---

## ⚠️ Notes

### The branch name must contain the issue number

This action extracts the issue number from the branch name.  
Make sure your branch naming convention includes the issue number. (e.g., `feature/#12`)

### Be cautious with the `on: create` trigger

This action is triggered by the `create` event, which also fires when tags or releases are created.  
To ensure it only runs when a branch is created, add the following condition:

```yaml
if: github.ref_type == 'branch' && contains(github.ref, 'feature/~~~')
```

## 🔁 Usage Example

```yaml
on:
  create:

jobs:
  start-issue:
    if: github.ref_type == 'branch' && contains(github.ref, 'feature')
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Start Issue
        uses: jinhokim98/project-flow-start-issue@v1
        with:
          github_token: ${{ secrets.PERSONAL_TOKEN }}
          project_owner: your-org
          project_number: 42
          target_column: 'In Progress'
```
