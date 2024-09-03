# Git Hooks 설정 가이드

이 가이드 문서는 Git Hooks를 이용하여 커밋 메시지와 브랜치 이름 제한

## ✅ 전제 조건

- git은 2.22 버전 이상을 사용해야 합니다.

## ✅ 각 Hooks 설명

- `commit-msg`: 커밋 메시지 규칙을 강제하는 Hook 스크립트입니다.
- `pre-commit`: 커밋이 생성되기 전에 실행되는 Hook 스크립트로, 코드 린팅과 포맷팅 검사에 유용합니다.
- `pre-push`: 커밋을 원격 저장소에 푸시하기 전에 실행되는 Hook 스크립트로, 테스트 실행이나 기타 검사에 유용합니다.

## ✅ 사용 예시

1. `commit-msg`

   ```shell
   $ git commit -m "잘못된 메시지"
   Error: commit 메시지 형식이 잘못 되었습니다..
   commit msg는 #{이슈번호}: {내용} 형식으로 작성해주세요.
   예: #21: stack prepareTransaction 구현
   ```

2. `pre-commit`

   - 정상적인 commit msg임에도 branch 이름이 잘못되면 에러
     발생
   - branch name : bug/test

   ```shell
   git commit -m "#21: test"
   Error: 브랜치 이름이 잘못 되었습니다.
   브랜치 이름은 다음과 같은 패턴들로 시작해야 합니다.: feature/#{이슈번호}_, bug/#{이슈번호}_, hotfix/#{이슈번호}_, release/v{버전}_
   예: feature/#21_stack_transaction
   ```

3. tag name

   - tag 설정 후 push 할때 설정된 tag name이 규칙에 맞지 않으면 에러가 발생됩니다.

   ```shell
   git push origin bug/#00_test
   Error: tag 이름이 잘못 되었습니다.
   tag 이름은 다음 형식 중 하나로 작성해주세요:
   1. vX.X.X-pre
   2. vX.X.X-prod
   3. vX.X.X-stage
   4. vX.X.X-dev
   5. vX.X.X-dev-wepin
   6. vX.X.X-dev-dcent
   7. vX.X.X-prod-wepin
   8. vX.X.X-prod-dcent
   9. vX.X.X-v1-dev
   10. vX.X.X-v1-stage
   11. vX.X.X-v1-prod
   예:
   1. v1.0.0-pre
   2. v1.0.0-prod-wepin
   ```

## ✅ Hooks를 로컬 레포지토리에 적용하기

- `hooks` 폴더에 있는 모든 스크립트 파일을 레포지토리의 .git/hooks에 복사하면 commit 시에 자동으로 동작한다.
- macOS와 linux에서는 `hooks` 폴더에 있는 모든 파일을 실행 가능하게 설정해야 합니다.
  - `chmod a+x *`

## ✅ Hooks를 로컬 레포지토리 전체에 적용하기

- `.githooks` 폴더를 생성한다.
  - `.githooks` 폴더는 레포지토리 안에 생성하지 않고 일반적인 폴더에 생성한다.
  - hooks 폴더에 있는 모든 파일을 `.githooks` 폴더에 복사한다.
- 아래와 같은 명령을 실행하면 git를 사용하는 모든 명령은 생성한 폴더에 있는 hooks 파일을 실행한다.

```bash
git config --global core.hooksPath .githooks(새로 생성한 폴더 경로)

# get 명령으로 정상적으로 설정되었는지 확인한다.
git config --get core.hooksPath
```

## 💡 Windows에서 한글 깨짐 오류 해결 방법

### Case 1) `git log` 사용 시

#### 오류 예시

![예시 사진](https://github.com/user-attachments/assets/b2a5fc13-33d5-4b35-a851-5e628e6028b5)

- 처음에 아무런 설정이 되어 있지 않을 때, git을 사용하는 경우 터미널에서 한글이 깨져보이는 경우가 발생합니다.

#### **해결 방법**

- 이 문제는 Git이 UTF-8 인코딩을 제대로 처리하지 못해서 발생하는 것으로, 아래 방법으로 해결할 수 있습니다.

  ```powershell
  # 현재 git config 상태 확인
  git config --global -l

  # i18n.commitencoding과 i18nlogoutputencoding 항목값을 utf-8로 수정
  git config --global i18n.commitencoding "UTF-8"
  git config --global i18n.logoutputencoding "UTF-8"
  ```

### Case 2) `git commit` 사용 시

#### 오류 예시

```powershell
PS C:\Users\user\Documents\GitHub\dcent-hybrid-webview-> git commit -m "임시  커밋"

[feature/#39_refactoring_to_common_component e1db12e5] ?꾩떆떆꾩
```

- 커밋 메시지를 남겼을 때 위와 같이 한글이 깨져보이는 경우가 발생합니다.

#### 해결 방법

- 이 문제는 PowerShell의 인코딩 기본값이 UTF-8이 아니기 때문에 발생하는 것으로, 아래 방법으로 해결할 수 있습니다.

1. **해결 방법 1)**

   ```powershell
   # OutputEncoding을 UTF-8로 수정
   $OutputEncoding = [console]::InputEncoding = [console]::OutputEncoding = New-Object System.Text.UTF8Encoding
   ```

- 다만 **해결 방법 1)은 경우 터미널을 킬 때마다 계속 다시 입력해줘야 하므로, 터미널이 켜질 때마다 자동으로 실행되게 설정합니다.**

2. **해결 방법 2) ← `추천`** ✨

   1. PowerShell 프로필이 존재하는지 확인합니다. 다음 명령어를 실행하세요:

      ```powershell
      Test-Path $PROFILE
      ```

   2. 만약 `False`가 반환되면, 프로필을 생성해야 합니다. 다음 명령어로 생성할 수 있습니다:

      ```powershell
      New-Item -Path $PROFILE -Type File -Force
      ```

   3. 프로필 파일을 편집하기 위해 경로 이동 후 설정 파일(`Microsoft.PowerShell_profile.ps1`)을 실행합니다:

      ```powershell
      # 해당 파일이 있는 경로로 이동
      cd C:\Users\{PC 유저 이름(각 PC마다 다름)}\Documents\WindowsPowerShell

      # 메모장 실행
      notepad $PROFILE
      ```

   4. 메모장이 열리면, 다음 줄을 추가합니다:

      ```powershell
      $OutputEncoding = [console]::InputEncoding = [console]::OutputEncoding = New-Object System.Text.UTF8Encoding
      ```

   5. 파일을 저장하고 메모장을 닫습니다.
   6. 그렇게 하면 PowerShell이 켜질 때마다 자동 실행됩니다.
