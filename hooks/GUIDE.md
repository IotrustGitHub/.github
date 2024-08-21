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

## Hooks를 로컬 레포지토리 전체에 적용하기

- `.githooks` 폴더를 생성한다.
  - `.githooks` 폴더는 레포지토리 안에 생성하지 않고 일반적인 폴더에 생성한다.
  - hooks 폴더에 있는 모든 파일을 `.githooks` 폴더에 복사한다.
- 아래와 같은 명령을 실행하면 git를 사용하는 모든 명령은 생성한 폴더에 있는 hooks 파일을 실행한다.

```bash
git config --global core.hooksPath .githooks(새로 생성한 폴더 경로)

# get 명령으로 정상적으로 설정되었는지 확인한다.
git config --get core.hooksPath
```
