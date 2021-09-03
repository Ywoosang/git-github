# Git&GitHub

- 협업을 위해 필요로 하는 git 과 github 개념을 빠르게 정리한 저장소입니다.
- 추가로 정리가 필요한 개념이 있다면 **issue** 로 남겨 주세요. 
 
- [origin,remote 개념](#origin-remote---)
- [SSH 원격 접속](#ssh------)
- [파일 상태](#-----)
- [브랜치 기본](#------)
  * [브랜치 만들기 및 이동](#------------)
  * [remote 에서 브랜치 받아오기](#remote------------)
  * [HEAD](#head)
  * [로그 조회](#-----)
  * [merge](#merge)
  * [브랜치 삭제](#------)
- [브랜치 종류](#------)
  * [Main branch](#main-branch)
  * [Supporting branch](#supporting-branch)
- [clone, push, pull](#clone--push--pull)
  * [브랜치 목록 조회](#---------)
  * [clone](#clone)
  * [push](#push)
  * [pull](#pull)
  * [remote 에 업로드한 파일 삭제](#remote-------------)

## origin,remote 개념

remote : 원격 저장소     
local : 내 컴퓨터 로컬 저장소  
origin : 깃허브 원격 저장소 주소  

ex) origin/master :  원격 저장소에 있는 master 브랜치
  
commit, push, pull 등 커맨드 통해 remote repository 와 협업한다. 
  
## SSH 원격 접속

매번 원격 저장소에 파일을 올리기 위해서 아이디와 비밀번호를 입력해 계정 주인임을 인증했다. SSH 원격 접속을 이용해 현재 사용하고 있는 기기를 깃허브에 등록한다면 번거로움을 줄일 수 있다. 

SSH : Private Key 와 public Key 를 한 쌍으로 묶어 컴퓨터를 인증하는 방식이다. 
  
엔터를 눌러 SSH 키를 생성한다. 
```
cd ~
ssh-keygen
```
생성된 키가 있는 경로로 이동한다. 
```bash
cd ~/.ssh
``` 
cat 명령어를 이용해 파일 내용을 확인한다
ssh-rsa 부터 끝까지 복사한다. 
``` bash
cat id_rsa.pub
```
**setting -> SSH and GPG Keys -> New  key -> Add Key** 로 키를 추가한다. 깃허브의 SSH 주소만 알고 있으면 로그인 정보를 입력하지 않고도 그 저장소에 접속할 수 있다. 

## 파일 상태

untracked unmodified modified staged
  
untracked : 처음 만들어 올리지 않은 상태    
staged : add 명령어로 스테이지에 올라간 상태  
unmodified : 커밋해서 변경 내용이 없는 상태  
modified : 커밋 후 코드를 변경한 상태  

보통 untracked 상태에서 처음 add 하고 그 다음 add 와 commit 을 반복한다. 

## 브랜치 기본


개별 브랜치를 만들어 독립적인 내용을 작업한다. 

**브랜치 분기 -> 작업 -> 병합** 

마스터브랜치에서 분기하기 전 적어도 하나 이상 커밋이 존재해야 한다. 


### 브랜치 만들기 및 이동
```bash
# 브랜치 생성
$ git branch <branch-name>
# 브랜치 생성 후 해당 브랜치로 이동
$ git checkout -b <branch-name>
# 브랜치 내역 확인
$ git branch
# 이미 존재하는 브랜치로 이동
$ git checkout <branch-name>
```

브랜치 내에서 작업 add, commit 후 병합한다.

### 브랜치 목록 조회

로컬에 있는 브랜치 목록을 조회한다. 
```bash
$ git branch
``` 
remote 저장소에 있는 브랜치 목록을 조회한다
```bash
$ git branch -r
```
모든 브랜치 목록을 조회한다.
```bash
$ git branch -a
```

### remote 에서 브랜치 받아오기

원격 저장소에서 작업을 한 이후에 로컬에 받아오기 위해서 git pull 명령을 사용한다. 하지만 원격 저장소에서 새로운 Branch를 생성하였을 경우에는 git pull 명령을 통해서도 Branch 정보를 받아올 수 없다. 이 경우에는 git pull이 아닌 다음의 명령을 수행한다.
```bash
$ git remote update
``` 
이때 원하는 저장소의 브랜치만 가져오고 싶다면 `-t` 옵션과 원격 저장소의 branch 이름을 입력한다. 로컬에 동일한 이름의 브랜치가 생성되고 해당 브랜치로 checkout 된다. 

```bash
$ git checkout -t <origin-branch-name>
```

### 브랜치 이름 변경

로컬에 있는 브랜치 이름을 변경하려면 git branch의 m 옵션을 사용한다
```bash
$ git branch -m <old-name> <new-name>
``` 
원격 저장소의 브랜치 이름을 변경하려면 로컬에서 브랜치 이름 변경한 후 새로운 이름의 브랜치로 push 한다. 원격 저장소를 확인해보면 새로운 이름의 브랜치가 하나 늘어난 것을 확인할 수 있다. 예전 이름 브랜치를 `--delete` 옵션을 통해 삭제하면 이름이 변경된 효과를 얻을 수 있다. 
```bash
$ git push origin -u <new-name>
$ git push origin --delete <old-name>
``` 

원격 저장소의 기본 브랜치 이름을 변경하고 싶을 경우 `Settings -> Branches` 메뉴에 들어가 이름을 변경한다. 로컬에 저장소의 이름도 같이 바꿔줘야 한다.
```bash
git branch -m <old-name> <new-name>
git fetch origin
git branch -u origin/<new-name> <new-name>
```

 


### HEAD 

HEAD 는 현재 작업트리가 어떤 버전을 기반으로 작업중인지 가리킨다. 작업 트리란 현재 작업중인 폴더를 의미한다. 작업트리에서 변경 내용을 add 하고 commit 한다.
다음 명령어로 커밋 내역을 확인해 보자
```bash
$ git long --oneline
```

ex) git log --oneline 으로 HEAD 를 조회해보았을때 다음과 같이 나타났다고 하자.
HEAD 가 가리키는 master 브랜치에서 first 커밋을 기반으로 작업중이다. 여기서 커밋 해시는 57ce9e0 이고 커밋메시지는 first 다. 
```bash 
$ 57ce9e0 (HEAD -> master) first 
``` 


### 로그 조회

로그를 조회할때 여러 옵션을 함께 사용할 수 있다.  

별도 옵션 없이 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다.
```bash
$ git log
```  
`--patch` 옵션은 각 커밋의 diff 결과를 보여준다.`-p` 로 축약해 사용 가능하다. 
```bash
$ git log -p
```  
`stat` 옵션으로 각 커밋이 어떤 파일에서 몇 줄을 추가 또는 삭제했는지를 확인할 수 있다. 
```bash
$ git log
```  
`--graph` 옵션을 이용해 모든 브랜치의 분기점 및 최근 커밋내역을 그래프로 확인할 수 있다. 
```bash
$ git log --oneline --branches --graph
```
`--branches` 옵션을 이용하면 현재 위치한 브랜치의 커밋내역 뿐만 아니라 다른 브랜치의 커밋내역도 같이 보여준다. 개인적으로 `--oneline` 옵션과 같이 사용하면 한눈에 보기 편하다. 
```bash
$ git log
```  
### merge

작업이 완료되었다면 merge 할 수 있다. merge 하고 싶은 브랜치로 이동해 merge 할 대상 브랜치를 지정하는게 핵심이다.
```bash
# 브랜치 이동
$ git checkout <branch-name>
# merge
$ git merge <branch-name>
``` 
ex) main 브랜치에 dev 브랜치를 merge 한다.
```bash
$ git checkout main
$ git merge dev
``` 

merge 는 항상 성공하는 명령어는 아니다. merge 도중 브랜치들에서 동일 부분을 서로 다르게 수정했다면 conflict 가 발생한다. 이 경우 자동으로 merge 가 되지 않는다. 생각해보면 다른사람이 이미 변경해 놓은 내용을 상의없이 내 코드로 덮어쓰는 것이니, 자동으로 merge 되면 위험한 것이다.  
충돌이 생긴 내용은 수동으로 충돌부분을 해결해야 하는데 이 경우 코드 에디터상 변경 부분이 표시된다. 해당 내용을 직접 수정한 후 commit 하면 merge 가 성공적으로 이루어진다. 

merge 도중 conflict 가 일어났을때 merge 를 원하지 않는다면  취소할 수 있다. 
```bash
$ git merge --abort
```

### 브랜치 삭제

병합이 끝났거나 더 이상 해당 브랜치가 필요가 없다 판단되면 삭제한다. 
```bash
$ git branch -D <branch-name>
```

## 브랜치 종류

### Main branch

메인 브랜치에는 master 브랜치와 develop 브랜치가 있다.

#### 제품으로 출시될 수 있는 master branch

제품으로 출시될 수 있는 브랜치다. 사용자에게 배포 가능한 상태만을 관리해야 한다. 배포를 관리하기 위한 브랜치이므로 함부로 master 브랜치에 병합(merge) 하지 않는다. 항상 master 브랜치에서 작업하고 있는 건 아닌지 확인한다.

#### 다음 출시 버전을 개발하는 develop branch 

기능 개발을 위한 브랜치들을 병합하기 위해 사용한다. 모든 기능이 추가되고 버그가 수정되어 배포 가능한 안정적인 상태라면 develop 브랜치를 mater 브랜치에 merge(병합)한다. 평소에는 이 브랜치를 기반으로 개발을 진행한다. 

### Supporting branch

팀원간 브랜치를 나눠 개발을 할 경우 supproting 브랜치들을 이용한다. 메인 브랜치와 달리 개발이 끝난 후 제거된다.

#### 기능을 개발하는 feature branch


feature 브랜치는 새로운 기능 개발 및 버그 수정이 필요할 때마다 develop 브랜치로부터 분기한다. feature 브랜치에서의 작업은 기본적으로 공유할 필요가 없기 때문에 자신의 로컬 저장소에서 관리한다. 개발이 완료되면 develop 브랜치로 merge하여 다른 사람들과 공유한다. 

**develop 브랜치에서 새로운 기능 개발을 위해 feature 브랜치를 분기 -> 새로운 기능에 대한 작업을 수행 -> 작업이 끝나면 develop 브랜치로 merge -> 더 이상 필요하지 않은 feature 브랜치는 삭제**


어떤 이름도 가능하다. 단, master, develop, release-..., hotfix-... 같은 이름은 사용할 수 없다.

feature/기능요약 형식으로 관리한다. 
ex) feature/comment
  
####  출시 버전을 준비하는 release branch

release 브랜치는 배포를 위한 최종적인 버그 수정, 문서 추가 등 배포와 직접적으로 관련된 작업을 수행한다. 이러한 작업들 이외에 release 브랜치에 새로운 기능을 추가로 merge하지 않는다

**develop 브랜치에서 배포 작업을 위해 release 브랜치를 분기 -> master 브랜치에 merge -> develop 브랜치에 merge**

release-RB_... 또는 release-... 또는 release/...같은 이름이 일반적이다.  
ex) realease-1.1

#### 출시 버전에서 발생한 버그를 수정하는 hotfix branch

배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, master 브랜치에서 분기하는 브랜치다. develop 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어려우므로 바로 배포가 가능한 master 브랜치에서 직접 브랜치를 만들어 필요한 부분만 수정한 후 다시 master 브랜치에 병합하여 이를 배포한다.

**배포한 버전에 긴급하게 수정할 부분 발생 -> master브랜치에서 hotfix브랜치 분기 -> 문제가 되는 부분 수정후 master브랜치에 merge -> develop 브랜치에 merge**

hotfix-... 와 같은 이름이 일반적이다.  
ex) hotfix-1.2.1 

## clone, push, pull 

### clone

저장소 주소를 클립보드에 복사한 후 해당 저장소 코드가 위치하길 바라는 곳에서 터미널에 다음을 입력한다. 
ex) 
```bash
$ git clone git@github.com:Ywoosang/git-github.git
```
이때 해당 브랜치의 코드만 remote 에서 가져오는 것이다.   
필요한 브랜치가 있다면 추가로 원격 브랜치 목록을 조회하고 가져온다. 

### push

지역 저장소의 브랜치를 원격 저장소 브랜치에 연결한다. 처음 push 할 때 한 번만 사용한다. 
```bash 
$ git push -u origin <branch-name>
``` 
다음은 로컬에서 커밋을 진행하고 push 로 remote 에 업로드한다. 

```bash
$ git push
``` 


로컬에서 만든 브랜치를 remote 에 추가할 수 있다. 기본적으로 로컬에서 브랜치를 만들면 remote 에 추가가 되지 않으므로 추가를 원한다면 직접 올린다. 
```bash
$ git push origin <branch-name> 
```

### pull

pull 명령어는 원격 저장소의 소스를 가져온다. 해당 소스가 현재 내 소스보다 더 최신 버전이라고 하면 지금의 버전을 해당 소스에 맞춰 올린다. merge 명령어를 사용하는 것이다.


다른 브랜치에서 pull 을 받는 것은 merge 하는 것과 동일하다는 것에 주의하자. 
```bash
# master 브랜치에서 develop 브랜치를 pull 한다.
$ git pull origin develop
``` 

### remote 에 업로드한 파일 삭제

local 에서 해당 파일을 삭제한 뒤 add, commit 후 remote 에 push 한다.

### 





