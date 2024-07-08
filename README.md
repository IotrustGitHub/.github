# Organization 전체에 Issue와 PR 템플릿 적용

# Git Hooks를 이용하여 commit msg와 branch 이름 제한
## 사용예
* commit message
```
$ git commit -m "잘못된 메시지"
Error: commit 메시지 형식이 잘못 되었습니다..
commit msg는 #{이슈번호}: {내용} 형식으로 작성해주세요.
예: #21: stack prepareTransaction 구현
```
* branch name
  - 정상적인 commit msg임에도 branch 이름이 잘못되면 에러 
  발생
  - branch name : bug/test
```
git commit -m "#21: test"
Error: 브랜치 이름이 잘못 되었습니다.
브랜치 이름은 다음과 같은 패턴들로 시작해야 합니다.: feature/#{이슈번호}_, bug/#{이슈번호}_, hotfix/#{이슈번호}_, release/v{버전}_
예: feature/#21_stack_transaction
```
## Hooks를 로컬 리포지토리에 적용하기
* hooks 폴더에 있는 모든 파일을 리포지토리의 .git/hooks에 복사하면 commit 시에 자동으로 동작한다.

## Hooks를 로컬 리포지토리 전체에 적용하기
* .githooks 폴더를 생성한다.
   - .githooks 폴더는 리포지토리 안에 생성하지 않고 일반적인 폴더에 생성한다.
   - hooks 폴더에 있는 모든 파일을 .githooks 폴더에 복사한다.
* 아래와 같은 명령을 실행하면 git를 사용하는 모든 명령은 생성한 폴더에 있는 hooks 파일을 실행한다.
```
git config core.hooksPath .githooks(새로 생성한 폴더 경로)
```
* get 명령으로 정상적으로 설정되었는지 확인한다.
```
git config --get core.hooksPath
```