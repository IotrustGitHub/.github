# IoTrustGitHub 레포지토리 생성 가이드

이 가이드 문서는 IotrustGitHub 내에서 새로운 레포지토리(Repository)를 생성하는 과정과 방법을 설명합니다.

## ✅ 전제 조건

- GitHub 계정이 있어야 합니다.
- 회사 GitHub 조직의 멤버여야 합니다.
- 레포지토리 생성 권한이 있어야 합니다.
  - 기본 값: 멤버인 경우, 생성 가능.

## ✅ 레포지토리 생성 방법

1. GitHub에 로그인한 상태로 [IotrustGitHub 페이지](https://github.com/IotrustGitHub)에 접속합니다.
2. [Repositories 페이지](https://github.com/orgs/IotrustGitHub/repositories)의 우측 상단 `New repository` 버튼을 클릭합니다.

![IotrustGitHub-2](https://github.com/user-attachments/assets/cd5a20f0-c7db-42ee-a13a-abf6debfabaf)

3. 레포지토리 **이름**을 입력합니다.
   - 레포지토리 이름은 케밥 케이스(kebab case)를 사용하여 명명합니다. (예: `github-project-name`)
4. 설명을 추가합니다. (선택사항)
5. 공개 여부를 **비공개**로 설정합니다.
6. `README.md` 파일 생성 여부를 선택합니다.
7. `.gitignore` 파일과 라이선스를 선택합니다. (선택사항)
8. `Create repository` 버튼을 클릭하여 생성합니다.

![IotrustGitHub-3](https://github.com/user-attachments/assets/a0c36e84-4feb-4e22-aec5-2aaa8211a3df)

## ✅ 레포지토리 설정 방법

### 1) 팀 접근 권한 설정

1. 생성된 레포지토리의 `Settings` 탭으로 이동합니다.

![IotrustGitHub-4](https://github.com/user-attachments/assets/636838fd-629f-445c-944c-8e68899c37d3)

2. `Access` 카테고리의 `Collaborators and teams` 설정으로 이동합니다.
3. `Manage access` 섹션에서 팀이나 개인 단위로 동료의 GitHub 계정을 초대합니다.

![IotrustGitHub-5](https://github.com/user-attachments/assets/ee6f470c-fe7f-48fb-8c4b-988820895cd3)

4. 적절한 권한 수준(Read, Triage, Write, Maintain, Admin)을 설정합니다.
   - `Read`: 읽기, 토론 참여 가능
   - `Triage`: 쓰기 권한은 없지만, 이슈와 Pull Request 참여 가능
   - `Write`: 소스 코드를 push 및 이슈, Pull Request 작성 가능
   - `Maintain`: Action 등의 중요한 설정 권한은 없으나 해당 레포지토리를 전반적으로 관리 가능
   - `Admin`: 모든 작업을 포함하여 프로젝트에 대한 전체 권한 보유

![IotrustGitHub-5-2](https://github.com/user-attachments/assets/6b1b99a2-59a5-4939-a9a6-ef79be1b4213)

### 2) 브랜치 보호 규칙 설정

1. `Code and automation` 카테고리의 `Branches` 설정으로 이동합니다.
2. `Add classic branch protection rule` 버튼을 클릭하여 새 규칙을 추가합니다.
   - 현재 가이드 문서의 경우, classic 버전으로 예시를 작성되었으며, `Add branch ruleset`이 더 익숙하시다면 해당 규칙 설정을 사용하셔도 괜찮습니다.

![IotrustGitHub-6](https://github.com/user-attachments/assets/fb43ff20-6f41-4a9d-94dd-0927f3278799)

3. 보호할 브랜치 패턴을 입력합니다.

   - 브랜치 하나에 적용하고자 한다면, 브랜치 이름으로 작성합니다. (예: `main`)
   - 특정 패턴의 모든 브랜치에 적용하고자 한다면, 패턴을 작성합니다. (예: `feature/*`)

4. 필요한 보호 규칙을 설정합니다.
   - `Require a pull request before merging`: PR 작성 필수(직접적인 push 불가능)
   - `Require approvals` - `Required number of approvals before merging`: N명 이상의 승인을 받아야 머지 가능
   - 위 두 항목은 필수로 설정해주시고, 이외의 항목은 각 레포지토리 성격에 맞춰 설정합니다.

![IotrustGitHub-7](https://github.com/user-attachments/assets/906cf9be-b1fb-4c9c-8595-3aa32b7cc88f)

### 3) 슬랙 봇으로부터 알림 받을 채널 설정

1. `Code and automation` 카테고리의 `Custom properties` 설정으로 이동합니다.
2. 해당 레포지토리의 Pull Request 알림을 연동할 슬랙 채널 ID를 입력하고 `Save`합니다.

![IotrustGitHub-8](https://github.com/user-attachments/assets/6e1c473b-662e-4d32-8f98-5bfbf54ad54a)

#### 💡 슬랙 채널 ID 확인하는 방법

1. 슬랙 앱 좌측의 채널 목록에서 확인하고자 하는 채널을 우클릭하여 `채널 세부정보 보기`를 클릭합니다.
2. 채널 세부정보 모달 창 하단에서 `채널 ID`를 확인 및 복사할 수 있습니다.

![IotrustGitHub-9](https://github.com/user-attachments/assets/2ee598dc-2265-4a0a-85c2-ff85d368d8dd)

## ✅ 초기 파일 추가

- `README.md`: 프로젝트 개요, 설치 방법, 사용법 등을 설명하는 파일을 추가합니다.

## ✅ 주의사항

- 민감한 정보(API 키, 비밀번호 등)를 레포지토리에 커밋하지 않도록 주의하세요.
- 코딩 컨벤션과 브랜치 규칙을 준수하세요.
