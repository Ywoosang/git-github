# Git&GitHub

- 협업을 위해 필요로 하는 git 과 github 개념을 빠르게 정리한 저장소입니다.
- 추가로 정리가 필요한 개념이 있다면 **issue** 로 남겨 주세요. 
 

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
** setting -> SSH and GPG Keys -> New  key -> Add Key ** 로 키를 추가한다. 깃허브의 SSH 주소만 알고 있으면 로그인 정보를 입력하지 않고도 그 저장소에 접속할 수 있다. 

## 파일 상태

untracked unmodified modified staged

untracked : 처음 만들어 올리지 않은 상태
staged : add 명령어로 스테이지에 올라간 상태 
unmodified : 커밋해서 변경 내용이 없는 상태 
modified : 코드를 변경 

보통 untracked 상태에서 처음 add 하고 그 다음 add 와 commit 을 반복한다. 

## 브랜치

개별 브랜치를 만들어 독립적인 내용을 작업한다. 

**브랜치 분기 -> 작업 -> 병합** 

마스터브랜치에서 분기하기 전 적어도 하나 이상 커밋이 존재해야 한다. 

### 브랜치 만들기 및 이동
```bash
# 브랜치 생성
git branch <branch-name>
# 브랜치 생성 후 해당 브랜치로 이동
git checkout -b <branch-name>
# 브랜치 내역 확인
git branch
# 이미 존재하는 브랜치로 이동
git checkout <branch-name>
```

브랜치 내에서 작업 add, commit 후 병합한다.

### HEAD 

HEAD 는 현재 작업트리가 어떤 버전을 기반으로 작업중인지 가리킨다. 작업 트리란 현재 작업중인 폴더를 의미한다. 작업트리에서 변경 내용을 add 하고 commit 한다.
다음 명령어로 커밋 내역을 확인해 보자
```bash

```

ex) git log --oneline 으로 HEAD 를 조회해보았을때 다음과 같이 나타났다고 하자.
HEAD 가 가리키는 master 브랜치에서 first 커밋을 기반으로 작업중이다. 여기서 커밋 해시는 57ce9e0 이고 커밋메시지는 first 다. 
```bash 
57ce9e0 (HEAD -> master) first 
``` 


### 로그 조회

로그를 조회할때 여러 옵션을 함께 사용할 수 있다.  

별도 옵션 없이 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다.
```bash
git log
```  
`--patch` 옵션은 각 커밋의 diff 결과를 보여준다.`-p` 로 축약해 사용 가능하다. 
```bash
git log -p
```  
`stat` 옵션으로 각 커밋이 어떤 파일에서 몇 줄을 추가 또는 삭제했는지를 확인할 수 있다. 
```bash
git log
```  
`--graph` 옵션을 이용해 모든 브랜치의 분기점 및 최근 커밋내역을 그래프로 확인할 수 있다. 
```bash
git log --oneline --branches --graph
```
`--branches` 옵션을 이용하면 현재 위치한 브랜치의 커밋내역 뿐만 아니라 다른 브랜치의 커밋내역도 같이 보여준다. 개인적으로 `--oneline` 옵션과 같이 사용하면 한눈에 보기 편하다. 
```bash
git log
```  
### merge

작업이 완료되었다면 merge 할 수 있다. merge 하고 싶은 브랜치로 이동해 merge 할 대상 브랜치를 지정하는게 핵심이다.
```bash
# 브랜치 이동
git checkout <branch-name>
# merge
git merge <branch-name>
``` 
ex) main 브랜치에 dev 브랜치를 merge 한다.
```bash
git checkout main
git merge dev
``` 

merge 는 항상 성공하는 명령어는 아니다. merge 도중 브랜치들에서 동일 부분을 서로 다르게 수정했다면 conflict 가 발생한다. 이 경우 자동으로 merge 가 되지 않는다. 생각해보면 다른사람이 이미 변경해 놓은 내용을 상의없이 내 코드로 덮어쓰는 것이니, 자동으로 merge 되면 위험한 것이다.  
충돌이 생긴 내용은 수동으로 충돌부분을 해결해야 하는데 이 경우 코드 에디터상 변경 부분이 표시된다. 해당 내용을 직접 수정한 후 commit 하면 merge 가 성공적으로 이루어진다. 

merge 도중 conflict 가 일어났을때 merge 를 원하지 않는다면  취소할 수 있다. 
```bash
git merge --abort
```

### 브랜치 삭제

병합이 끝났거나 더 이상 해당 브랜치가 필요가 없다 판단되면 삭제한다. 
```bash
git branch -D <branch-name>
```

## clone, push, pull 

### 브랜치 목록 조회

로컬에 있는 브랜치 목록을 조회한다. 
```bash
git branch
``` 
remote 저장소에 있는 브랜치 목록을 조회한다
```bash
git branch -r
```
모든 브랜치 목록을 조회한다.
```bash
git branch -a
```

### clone

저장소 주소를 클립보드에 복사한 후 해당 저장소 코드가 위치하길 바라는 곳에서 터미널에 다음을 입력한다. 
ex) 
```bash
git clone git@github.com:Ywoosang/git-github.git
```
이때 해당 브랜치의 코드만 remote 에서 가져오는 것이다.   
필요한 브랜치가 있다면 추가로 원격 브랜치 목록을 조회하고 가져온다. 

### push

지역 저장소의 브랜치를 원격 저장소 브랜치에 연결한다. 처음 push 할 때 한 번만 사용한다. 
```bash 
git push -u origin <branch-name>
``` 
다음은 로컬에서 커밋을 진행하고 push 로 remote 에 업로드한다. 

```bash
git push
``` 


로컬에서 만든 브랜치를 remote 에 추가할 수 있다. 기본적으로 로컬에서 브랜치를 만들면 remote 에 추가가 되지 않으므로 추가를 원한다면 직접 올린다. 
```bash
git push origin <branch-name> 
```

### pull

로컬에서 작업하면서 clone 또는 pull 을 이용해 remote 브랜치를 가져오는 경우가 있다. 이때 원하는 저장소의 브랜치를 가져오고 싶다면 `-t` 옵션과 원격 저장소의 branch 이름을 입력한다. 로컬에 동일한 이름의 브랜치가 생성되고 해당 브랜치로 checkout 된다. 

```bash
git checkout -t <origin-branch-name>
```

다른 브랜치에서 pull 을 받는 것은 merge 하는 것과 동일하다는 것에 주의하자. 
```bash
# master 브랜치에서 develop 브랜치를 pull 한다.
git pull origin develop
``` 













