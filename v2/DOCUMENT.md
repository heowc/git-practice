# Git

## 시작하기

### 버전 관리란?

- 파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템
- 로컬 버전 관리(VCS) -> 중앙 집중식 버전 관리(CVCS) -> __분산 버전 관리 시스템 (DVCS)__

### 짧게 보는 Git의 역사

- 특징
	- 빠른 속도
	- 단순한 구조
	- 비선형적인 개발(수천 개의 동시 다발적인 브랜치)
	- 완벽한 분산
	- Linux 커널 같은 대형 프로젝트에도 유용할 것

### Git 기초

- 데이터를 파일 시스템 스냅샷으로 취급하고 크기가 아주 작다.
- 파일이 달라지지 않으면 파일에 대한 링크만 저장한다.
- 거의 모든 명령이 로컬 파일과 데이터만 사용한다.
- 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다.
- 파일을 Committed, Modified, Staged 과 같은 세 가지 상태로 관리한다.
	- Committed : 데이터가 로컬 데이터베이스에 안전하게 저장 됐다는 것
	- Modified : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않을 것
	- Staged : 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태인 것

### 최소 설정

- 환경 설정을 해줘야 한다. 업그레이드해도 유지된다.
- `git config`으로 내용을 확인하고 변경할 수 있다.
- `--global`으로 공통적인 사용자 정보를 설정할 수 있다.
- `core.editor`으로 해당 설정에 대한 편집기를 변경 할 수 있다.
- `git config --list`으로 설정한 모든 값을 보여준다.

### CLI

### Git 설치하기

- Linux (Fedora)
	```text
	$ sudo yum install git-all
	```
- Linux (Ubuntu)
	```text
	$ sudo apt-get install git-all
	```
- Mac
	- http://git-scm.com/download/mac

- Windows
	1. http://git-scm.com/download/win
	2. http://windows.github.com

## Git 의 기초

### Git 저장소 만들기

- 기존 디렉토리를 Git 저장소로 만들기
	- `git init`
	- .git 이라는 하위 디렉토리 생성

- 기존 저장소를 Clone 하기
	- `git clone [url]`
	- 프로젝트의 히스토리를 전부 받아온다.
	- https, git, ssh 프로토콜 지원

### 수정하고 저장소에 저장하기

- 워킹 디렉토리(Check out 한 디렉토리)의 모든 파을은 크게 Tracked 와 Untracked 로 나뉜다.
	- Tracked : 이미 스냅샷에 포함돼 있던 파일
		- Unmodified : 수정하지 않음
		- Modified : 수정함
		- Staged : 커밋으로 저장소에 기록
	- Untracked : 워킹 디렉토리에 있는 파일 중 스냅샷에도 Staging Area 에도 포함되지 않은 파일

- 파일 상태 확인하기
	- `git status`

- 파일 새로 추적하기
	- `git add {file}`

- Modified 파일 Stage 하기
	- `git add`는 파일을 새로 추적할 떄도 사용하고 수정한 파일을 Staged 만들 때도 사용한다.

- 파일 상태 짧게 확인하기
	- `git status -s` 또는 `git status --short`
	- 새로운 파일은 ?? 표기
	- Staged 상태로 추가한 파일 중 새로 생성한 파일 앞에는 A 표기
	- 수정한 파일 앞에는 M 표

- 파일 무시하기
	- .gitignore 생성
	- 파일 패턴을 적음
		- 빈 라인이나 #으로 시작하는 라인 무시
		- 표준 Glob 패턴 사용

- Staged 와 Unstaged 상태의 변경 내용 보기
	- `git diff`
	- Staging Area 에 넣은 파일의 변경 부분을 보고 싶으면 `git diff --staged`
	- `git diff --staged` 와 `git diff --cached` 동일 옵션

- 변경사항 커밋하기
	- `git commit`
	- 메시지를 인라인 첨부 `git commit -m "{메시지}"`

- Staging Area 생략하기
	- Tracked 상태의 파일을 자동으로 Staging Area에 넣음. `git commit -a`

- 파일 삭제하기
	- Tracked 상태의 파일을 삭제. `git rm`
	- 삭제한 파일은 Staged 상태가 된다.

- 파일 이름 변경하기
	- `git mv {file_from} {file_to}`

### 커밋 히스토리 조회 하기

- `git log`
- SHA-1 체크섬, 저자 이름, 저자 이메일 커밋한 날짜 커밋 메시지 보여줌
- 각 커밋의 diff : `git log -p`
- 각 커밋의 통계 정보 : `git log --stat`
- 히스토리 형식 변경 : `git log --pretty={oneline|short|full|fuller|format}`
	- format 옵션(%H, %h, %T, %t, %P, %p, %an, %ae, %ad, %ar, %cn, %ce, %cd, %cr, %s)
- 브랜치와 머지 히스터리 정보를 아스키 그래프 : `git log --graph`
- 히스토리 갯수 : `git log -{n}`
- 히스토리 시간 : `git log --since | --after` | `git log --until | --before`
	```text
	n주 동안 : {n}.weeks
	절대 날짜 : "2017-08-23"
	상대 날짜 : "2 years 1 day 3 minutes ago"
	```
- 히스토리 저자 : `git log --author`
- 히스토리 키워드 : `git log --grep`

	> 저자와 키워드 모두 만족하는 커밋을 찾으려면 `--all-match` 옵션을 사용

- 히스토리 내용 : `git log --S{word}`

### 되돌리기

- 완료된 커밋 수정 : `git commit --amend`
- 아래 명령어는 하나의 커밋으로 기록
	```text
	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend
	```
- 파일 Unstage 상태로 변경 : `git reset HEAD {file}`

	> `git reset` 명령은 위험하기 때문에 되도록이면 사용하지 않는다.

- Modified 파일 되돌리기 : `git checkout -- {file}`

	> `git checkout` 명령은 위험하기 때문에 되도록이면 사용하지 않는다.

- Stash와 Branch를 사용하자.

### 리모트 저장소

- 리모트 저장소 확인 : `git remote`
- 단축이름과 URL 포함 : `git remote -v`
- 리모트 저장소 추가 : `git remote add {단축 이름} {url}`
- 리모트 저장소 가져오기(no merge) : `git fetch {단축 이름}`
- 리모트 저장소 가져오기(merge) : `git pull {단축 이름}`
- 리모트 저장소 공유하기 : `git push {단축 이름} {브랜치 이름}`
- 리모트 저장소 살펴보기 : `git remote show {단축 이름}`
- 리모트 저장소 이름 변경 : `git remote rename {단축 이름} {새 단축 이름}`
- 리모트 저장소 삭제 : `git remote remove {단축 이름}` | `git remote rm {단축 이름}`

### 태그

- 태그 조회 : `git tag`
- 태그 조회 (검색 패턴) : `git tag -l "{*word*}"`
- 태그 정보와 커밋 정보 확인 : `git show`
- 태그 붙이기
	1. Lightweight 태그
		- 특정 커밋에 대한 포인터
		- `git tag {tag}`
	2. Annotated 태그
		- 태그 만든 사람 이름, 이메일 등록 날짜 메시지 저장
		- `git tag -a {tag} -m "{message}"`
- 나중에 태그하기 : `git tag -a {tag} {체크섬}`
- 태그 공유하기 : `git push {단축 이름} {tag}`
- 태그 공유하기(복수) : `git push {단축 이름} --tags`

	> `git push` 명령은 자동으로 리모스 서버에 태그를 전송하지 않는다.

- 태그 checkout 하기 : `git checkout -b {new branch name} {tag}`

	> 태그는 브랜치와 달리 머킷을 바꿀 수 없기 때문에 새로운 브랜치를 만들어 작업해야 한다.

### Git Alias

- Git 명령어를 alias로 만들 수 있다.
- 예를 들면, `git commit` 을 `git ci` 로 대체 할 수 있다.
	```text
	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status
	```

## Git 브랜치

### 브랜치란 무엇인가?

- 코드를 통째로 복사하고 나서 원래 코드와 상관없이 독립적으로 개발을 진행 하는 것
- Git의 브랜치는 매우 가볍다.
- Git 데이터는 스냅샷을 이용하여 기록한다.
- Git 저장소에 파일을 blob이라고 부른다.
- 브랜치를 만들어 작업하고 Merge 하는 방법을 권장
- Git의 브랜치는 커밋 사이를 가볍게 이동할 수 있는 포인터 같은 것 이다.
- 새 브랜치 생성 : `git branch {name}`
	- 새로 만든 브랜치는 지금 작업하고 있던 마지막 커밋을 가리킨다.
- 현재 작업 중인 브랜치를 가리키는 특수한 포인터인 HEAD가 있다.
	- 브랜치 커밋 내용 확인 : `git log --decorate`
- 브랜치 이동하기 : `git checkout {name}`

### 브랜치와 Merge의 기초

- 브랜치 생성 및 checkout : `git checkout -b {name}`
- 브랜치 삭제 : `git branch -d {name}`
- 브랜치 합침 : `git merge {name}`
- Fast forward 방식 Merge : Merge를 하였으나, Merge 과정 없이 그저 최신 커밋으로 이동
- 3-way Merge : 결과를 별도의 커밋으로 만들어서 가리키도록 이동 (Merge 커밋)
- merge 충돌 시, `git status`명령으로 확인하면 unmerged 상태로 표기
	- `======` 위쪽은 HEAD 내용, 아래쪽은 해당 브랜치 내용
	- `git mergetool`명령으로 충돌 해결
	- 충돌 해결 후, `git commit`에는 해당 내용을 자세헤게 기록된다.

### 브랜치 관리

- 브랜치 목록 : `git branch` (현재 checkout 브랜치는 앞에 * 표기)
- 마지막 커밋 표기 : `git branch -v`
- merge 필터링 : `git branch --(merged|no-merged)`
- 브랜치 강제 삭제 : `git branch -D {name}` (merge 하지 않은 브랜치는 `-d`로 삭제 되지 않는다.)

### 브랜치 워크플로

- 브랜치를 만들고 Merge 하는 Flow 방식(?)
- LongRunning 브랜치
	- 안정적인 `master` 브랜치를 두고 개발 브랜치를 만들어 Merge 하는 방식
	- 규모가 크고 복잡한 프로젝트에 유용
- 토픽 브랜치
	- 프로젝트 크기에 상관없음
	- 한 가지 주제나 작업을 위한 짧은 호흡의 브랜치

### 리모트 브랜치

- 리모트 Refs는 리모트 저장소에 있는 포인터인 레퍼런스이다. (브랜치, 태그 등등)
- 모든 리모트 Refs 조회 : `git ls-remote {branch name}`
- 모든 리모트 브랜치와 그 정보 조회 : `git remote show {branch name}`
- 보통 리모트 Refs보다는 리모트 트래킹 브랜치(리모트 브랜치를 추적하는 브랜치, 일종의 북마크)를 사용한다.
- 리모트 트래킹 브랜치를 로컬 브랜치로 Checkout 하면 자동으로 `트래킹 브랜치`가 만들어진다.
	- 서버 저장소에서 `master` 브랜치를 clone 하면, git에서 `orgin/master`의 트래킹 브랜치인 `master` 브랜치를 만든다.
- 리모트의 특정 브랜치를 추적 : `git branch -(u|set-upstream-to)`
- 추적 브랜치 확인 : `git branch -vv`
- `git pull` = `git fetch` + `git merge`
- 리모트 브랜치 삭제 : `git push {리모트 저장소} --delete {branch name}`

### Rebase 하기

- 브랜치 합치는 방법
	1. Merge
	2. Rebase
- rebase : 한 브랜치에서 변경된 사항을 다른 브랜치로 적용하는 방식
	1. 합칠 브랜치를 어딘가에 임시로 저장해놓는다.
	2. rebase 브랜치가 합칠 브랜치가 가리키는 커밋을 가리키게 한다.
	3. 합칠 브랜치를 Fast forward 시킨다.
- 보다 깔끔한 히스토리
- rebase : `git rebase {베이스 브랜치} {브랜치}`
- 브랜치의 브랜치를 다른 브랜치에 적용시키는 방법 : `git rebase --onto {다른 브랜치} {브랜치} {브랜치의 브랜치}`
- 이미 저장소에 push한 커밋을 rebase하면 안된다.
- rebase한 것을 rebase 하기 : `git pull --rebase`

## Git 서버

### 프로토콜

- 리모트 저장소는 일반적으로 워킹 디렉토리가 없는 Bare 저장소 이다.
	- Bare 저장소 : 일반 프로젝트에서 `.git` 디렉토리만 있는 저장소
- 프로토콜
	- Local, HTTP, SSH, Git 등의 프로토콜 지원
	- 로컬 프로토콜
		- 리모트 저장소가 단순히 디스크의 다른 디렉토리에 존재
		- 파일경로
			1. 직접 쓸 때 : 복사나 하드링크 사용
			2. `file://` : 별도의 프로세스를 생성하여 처리
		- 장점
			1. 간단하다.
			2. 타저장소 가져오기 용이
		- 단점
			1. 디렉토리 공유 불편
			2. 마운트 중에는 빠르지 않다.
			3. 우발적 사고에 보호되지 않는다.
	- HTTP 프로토콜
		- 스마트 HTTP : SSH나 Git 프로토콜처럼 통신
		- 멍청한 HTTP : 원격 저장소를 파일 건네주는 웹 서버로 취급
	- SSH 프로토콜
		- Git의 대표 프로토콜
		- 아무런 외부 도구 없이 Git 서버 구축 가능
		- 장점
			1. 상대적으로 설정이 쉽다.
			2. 보안에 안전하다.
			3. 전송 시, 데이터를 가능한한 압축하기 때문에 효율적이다.
		- 단점
			1. 익명 접근 불가능
	- Git 프로토콜
		- Git에 포함된 데몬을 사용하는 것
		- 9418 포트 사용
		- SSH 프로토콜과 비슷한 서비스 제공(인증 메커니즘 X)
		- 장점
			1. 전송 속도가 가장 빠르다.
		- 단점
			1. 인증 메커니즘이 없다.

### 서버에 Git 설치하기

- 서버에 Git 설치하기
	- Bare 저장소 만듬
	- `git clone --bare project project.git`
	- `cp -Rf project/.git project.git` 과 동일
- 서버에 Bare 저장소 넣기
	- 도메인프로토콜 설정
	- SSH 접속 가능한 Git 저장소 : `scp -r project.git user@git.example.com:/srv/git`
- SSH 접속
	1. 각각의 계정 기입
	2. 서버마다 git 계정 만듬
	3. LDAP 서버 이용

### SSH 공개키 만들기

- SSH 공개키 만들기
	1. 키 존재여부 확인
		- 기본적으로 `~/.ssh` 디렉토리에 저장
		- `.pub`은 공개키 다른 파일은 개인키
	2. (`.ssh` 디렉토리가 없거나 해당 파일들이 없다면) 생성
		- `ssh-keygen`으로 생성

### 서버 설정하기

- git 계정 추가 및 `.ssh` 디렉토리 안에 `authorized_keys` 파일 추가
- 공개키 `authorized_keys` 파일에 추가 : `cat {.pub} >> ~/ .ssh/authorized_keys`
- ~~

### Git 데몬

- 인증 기능이 없는 Git 저장소를 만들 수 있는 가장 빠른 방법
- `git daemon --reuseaddr --base-path=/srv/git/ /srv/git/`
	- `--reuseraddr` : 서버가 기존의 연결이 타임아웃될 때까지 기다리지 말고 바로 재시작하게 하는 옵션
	- `--base-path` : clone 시, 전체 경로 사용하지 않도록 하는 옵션
- 우분투 기준으로 Upstart 스크립트를 이용하여 Git 데몬을 실행 : `/etc/init/local-git-daemon.conf`

### 스마트 HTTP

- 기본적인 Git 서버 실행(v1.6.6+) : `git-http-backend`
- CGI 서버 연동(ex. apache)
	- CGI 란?
		```text
		공용 게이트웨이 인터페이스(Common Gateway Interface; CGI)라고도 하는 CGI는 http 통신규약을 사용하는 웹서버의 80번 포트를 통해서 웹 서버에 질의(query; 물어보는 것)하는 일련의 데이타 스트링을 보내서 서버에 문의를 하면, 웹 서버는 임의의 처리를 거친 가공된 데이타를 웹 클라이언트에 보내주는 것으로서 "문의"와 "답변"을 주고받는 통신 및 처리규약
		```

### GitWeb

- Git은 웹에서 저장소를 조회할 수 있는 GitWeb이라는 CGI 스크립트를 제공
- `lighttpd`나 `webrick`같은 경량 웹 서버가 설치돼 있어야 사용 가능
- `git instaweb --httpd=(lighttpd|webrick)` (default : lighttpd)

### GitLab

- 널리 사용하는 서버 중 하나
- 데이터베이스와 따로 연동해야하는 웹 애플리케이션
- 설치
	- `https://bitnami.com/stack/gitlab`
- 관리자
	- 기본 사용자이름은 `admin@local.host`, 암호는 `5iveL!fe`이다
- ~~

### 또 다른 선택지, 호스팅

- Git 서버 직접 운영 부담 → Git 호스팅 서비스
- Github : 가장 큰 Git 호스팅 서비스


## 분산 환경에서의 Git

### 분산 환경에서의 워크플로

- Git은 분산형이다.
- 저장소 운영방식 비교
	1. 중앙집중식 워크플로
		- 하나의 저장소에 여러 개발자가 push&merge
		- 작은 팀에서 유용
	2. Intergration-Manager 워크플로
		- 리모트 저장소를 여러 개 운영
		- 대표하는 공식 저장소가 존재
		- Intergration-Manager는 기여자의 저장소를 리모트 저장소로 등록하고, 로컬에서 기여물을 테스트하고, 프로젝트 메인브랜치에 Merge하고, 그 내용을 다시 프로젝트 메인 저장소에 Push
	3. Dictator and Lieutenants 워크플로
		- 리모트 저장소를 여러 개 운영하는 방식을 변형한 구조
		- 수백 명의 개발자가 참여하는 아주 큰 프로젝트에 이용
		- Lieutenants : 여러 명의 Intergration-Manager
		- Benevolent Dictator : Lieutenants의 최종 관리자

### 프로젝트에 기여하기

- Git은 유연하게 설계되어 여러 가지 방식이 존재
- 여러 변수 존재
	1. 활발히 활동하는 개발자의 수 → 최신 코드 유지 어려움
	2. 프로젝트에서 선택한 워크플로
	3. 접근 권한
- 커밋 가이드라인
	- https://github.com/git/git/blob/master/Documentation/SubmittingPatches 참고
	- 여러가지 이슈를 하나의 커밋에 담지 않는다.
	- 좋은 커밋 메시지를 작성해야 한다.

### 프로젝트 관리하기

- 운영 방법
	1. `format-patch` 명령으로 생성한 Patch를 이메일로 받아서 프로젝트에 Patch를 적용
	2. 프로젝트의 다른 리모트 저장소로부터 변경 내용을 Merge
- 토픽 브랜치에서 일하기
	- 메인 브랜치 통합 전 임시 브랜치에서 통합 테스트
- 이메일로 받은 Patch를 적용하기
	- 우선 토픽 브랜치에 Patch 적용
	- Patch 적용 방법 : `git apply` `git am`
- 리모트 브랜치로부터 통합하기
- 기여물 통합하기
 	- Merge하는 워크플로
		- 가장 간단한 시나리오
		- `master` 브랜치에 merge할 때마다 해당 토픽 브랜치를 삭제
	- 대규모 Merge 워크플로
		- LongRunning 브랜치를 4개 운영(`master`, `next`, `pu`, `maint`)
		- 토픽 브랜치가 안정화되면 `next`, 개선이 필요하면 `pu`
		- 마지막 릴리즈 `maint`
	- Rebase와 Cherry-Pick 워크플로
		- 히스토리를 한 줄로 관리하기 위해 `master` 브랜치를 기반으로 Rebase
- Rerere
	- 수시로 Merge나 Rebase를 한다거나 오랫동안 유지되는 토픽브랜치는 쓸 때 `rerere` 사용
	- 글로버 설정 변경 : `git config --global rerere.enabled (true/false)`
	- `git rerere`
- 릴리즈 버전에 태그 달기
- 빌드넘버 만들기
	- `git describe`
- 릴리즈 준비하기
	- `git archive`
- Shortlog 보기
	- 이메일로 프로젝트의 변경 사항을 알릴 때, 지난 릴리즈 이후 변경 사항 목록 가져오기
	- `git Shortlog`

## GitHub

### 계정 만들고 설정하기

- 가장 큰 Git 저장소 호스트
- 많은 오픈소스 프로젝트는 GitHub을 이용해서 Git 호스팅, 이슈 트래킹, 코드 리뷰, 등등의 일을 한다.
- 관리, Git 저장소 사용법, 프로젝트 기여, GitHub 인터페이스
- 계정 만들기
	- https://github.com
- SSH 사용하기
	- 개인의 공개키 등록
	- `~/.ssh/id_rsa.pub` 파일 내용 복사&붙여넣기
- 아바타
	- Profile 변경 가능
- 사용자 이메일 주소
	- Git 커밋에 있는 이메일 주소를 보고 사용자 식별
	- 여러 Email 등록 가능
- 투팩터 인증
	- 더 안전한 보안을 위해서 "2FA" 인증 설정
		1. OTP
		2. SMS

### GitHub 프로젝트에 기여하기

- 프로젝트 Fork 하기
	- GitHub이 프로젝트를 통째로 복사해준다.
- GitHub 플로우
	- Pull Request가 중심인 협업 워크플로를 위주로 설계(Fork 해서 프로젝트에 기여하는 방법)
		1. `master`에서 토픽 브랜치를 만든다.
		2. 무엇인가를 수정해서 커밋한다.
		3. 자신의 GitHub 프로젝트에 브랜치를 Push 한다.
		4. GibHub에 Pull Request를 연다.
		5. 토론하면서 그에 따라 계속 커밋한다.
		6. 프로젝트 소유자는 Pull Request를 Merge 하고 닫는다.
	- 기본적으로 `Intergration-Manager 워크플로`와 동일
	- Pull Request 만들기
		1. Fork한 개인 저장소를 로컬에 Clone한다.
		2. 무슨 일인지 설명이되는 이름의 토픽 브랜치를 만든다.
		3. 코드를 수정한다.
		4. 잘 고쳤는지 확인한다.
		5. 토픽 브랜치에 커밋한다.
		6. GitHub의 개인 저장소에 토픽 브랜치를 Push 한다.
		7. 원 저장소에 Pull Request를 만들어 작성한다.
- 팁
	- 이슈와 연동(`#{issue number}`)
	- Markdown, GItHub Flavored Markdown

### GibHub 프로젝트 관리하기

- 새 저장소 만들기
	- `New repository`
- 동료 추가하기
	- [Setting] - [Collaborators]
- Pull Request 관리하기
 	- 이메일 알림
	- Pull Request로 함께 일하기
	- 멘션과 알림(`@`)
	- 알림 페이지
- 특별한 파일
	- README
		- `.md`, `.asciidoc` 자동 렌더링
		- 필요한 정보 제공
			- 무슨 프로젝트인지
			- 설정하고 설치하는 방법
			- 사용법과 실행 결과에 대한 예제
			- 프로젝트의 라이센스
			- 기여하는 방법
	- CONTRIBUTING
		- 프로젝트 기여 방법과 Pull Request 규칙
- 프로젝트 관리
	- Pull Request 기본 브랜치 변경
	- 프로젝트 넘기기
		- [Setting] - [Opentions] - [Transfer Ownership]

### Organization 관리하기

- Organization 관리하기
	- 여러 명이 같은 프로젝트를 관리하는데 사용하는 그룹 계정
	- 사람들을 서브 그룹으로 나누어 관리하는 도구
- Organization 기초
	- `New organization`
- 팀
	- 저장소에서 함께 일하는 사람을 관리하는 효과적인 도구
- 감사 로그
	- 소유자는 Organization에서 일어나는 모든 정보를 알 수 있다.

### GitHub 스크립팅

- 타 서비스 통합
- 훅과 서비스 이용
- 서비스
	- CI, 버그 트래커, 이슈 트래커, 채팅, 문서 시스템 연동
	- GitHub에서 제공해주는 서비스가 있는지 확인
	- push email 전송, push jenkins test
- 훅
	- 서비스에 없는 사이트나 외부 서비스와 연동하고 싶거나 좀 더 세세한 설정을 하고 싶을 때 사용
	- URL로 HTTP 페이로드를 보내준다.
- GitHub API
	- 이벤트의 정보를 좀 더 자세히 알고 싶을 때 사용


## Git 도구

### 리버전 조회하기

- SHA-1 해쉬값으로 조회 : `git show {SHA-1}`
- Log 짧은 해시값 조회 : `git log --abbrev-commit --pretty=oneline`
- 해당 브랜치의 최근 커밋 조회 : `git show {branch name}`
- 브랜치와 HEAD가 가리켰던 커밋 로그 조회 : `git reflog`
	- 비슷한 기능
		1. `git show HEAD@{n}`
		2. `git show master@{yesterday}`
		3. `git log -g master`
	- Reflog의 일은 모두 로컬의 일이기 때문에 내 Reflog가 동료의 저장소에는 있을 수 없다.
- 계통 관계 가리키기 (이름뒤에 `^` 붙이기) : `git show HEAD^`
- 범위 커밋 카리키기
 	1. Double Dot
		- `git log master..exper`
		- `exper`브랜치 커밋들 중에서 아직 `master`브랜치에 Merge 안된 커밋 내용
	2. 세 개 이상의 Refs
		- `git log refA refB ^refC`
	3. Triple Dot
		- `git log master...exper`
		- 두 Refs 사이에 공통적인 커밋을 제외하고 서로 다른 커밋 내용

### 대화형 명령

- `git add -i(-interactive)`

### Stashing과 Cleaning

- stash :  Modified이면서 Tracked 상태인 파일과 Staging Area에 있는 파일들을 보관해두는 장소
- stash 저장 : `git stash` or `git stash save`
- stash 목록 : `git stash list`
- stash 가져오기 : `git stash apply`
	- `stash@{n}` : 해당 stash 적용
	- `--index` : 파일 적용
- stash 제거 : `git drop`
- stash 적용한 브랜치 만들기 : `git stash branch {branch name}`

- 워킹 디렉토링 청소하기 : `git clean (-x) (-f) (-d) (-n)`

### 내 작업에 서명하기

- GPG 이용하면 암호학적으로 안전하게 만들 수 있다.
- 개인키 설치 : `gpg --genkey`
- GPG 개인키 설정 : `git config --global user.signingkey {}`
- 태그 서명 : `git tag -s`
- 커밋 서명 : `git commit -S`
- v1.8.3 이후에는 `git merge`와 `git pull`을 GPG 서명 정보를 이용해 제한 할 수 있다.

### 검색

- `git grep` : 커밋 트리의 내용이나 워킹 디렉토리의 내용을 문자열이나 정규 표현식을 이용해 검색
	- `-n` : 라인 번호 출력
	- `--count` : 파일 갯수
	- `-p` : 함수 or 메소드
	- `--and` : 그리고
	- `--break`, `--heading` : 읽기 쉬운 형태 출력

- `git log` : Diff 내용을 검색하여 __언제__ 추가&변경 되었는지 검색
	- `-S` : 해당 문자열 검색
	- `-G` : 정규식 검색

- `git log -L` : 라인 히스토리 검색

### 히스토리 단장하기

- 중앙서버에 Push한 커밋은 절대 고치지 말아야 한다.
- 마지막 커밋 수정하기 : `git commit --amend`
- 커밋 메시지를 여러 개 수정하기 : `git rebase -i HEAD~{n}`
- 커밋 순서 바꾸기 : 대화형 Rebase 도구
- 커밋 합치기 : 대화형 Rebase 도구
- 히스토리 전체에 필요한 것만 골라내는데 사용하는 도구 : `filter-branch`
	- 모든 커밋에서 파일 제거
	- 하위 디렉토리를 루트 디렉토리로 만들기
	- 모든 커밋의 이메일 주소를 수정하기

### Reset 명확히 알고 가기

| 트리			| 역할								|
|---------------|-------------------------------------|
| HEAD			| 마지막 커밋 스냅샷, 다음 커밋의 부모 커밋 |
| Index			| 다음에 커밋할 스냅샷 					|
| 워킹 디렉토리	| 샌드박스 								|

- `reset`
	1. HEAD 이동 (`reset --soft`)
		- `git commit --amend`와 동일한 결과
	2. Index 업데이트 (`reset --mixed`)
		- 디폴트 옵션
		- Index를 현재 HEAD가 가리키는 스냅샷으로 업데이트
	3. 워킹 디렉토리 이동 (`reset --hard`)
		- 워킹 디렉토리까지 업데이트
- 합치기(Squash) : `git reset --soft HEAD~{n}`

### 고급 Merge

- Merge 충돌
	- Merge 하기 전에 워킹 디렉토리를 깔끔히 정리하는 것이 좋다.
- Merge 취소
	- `git merge --abort`
	- `git reset --hard HEAD`
- 공백 무시하기 : `git merge -X(ignore-space-change|ignore-all-space)`
- 커밋 되돌리기 : `git revert`
- 서브트리 Merge : `git read-tree`

### Rerere

- `"reuse recorded resolution"` : 기록한 해결책 재사용하기

### Git으로 버그 찾기

- `git blame` : 메소드의 각 라인을 누가 언제 마지막으로 고쳤는지 확인
- `git bisect` : 커밋 히스토리를 이진 탐색

### 서브모듈

- 서브 모듈 추가 : `git submodule add {url}`
- `.gitmodules`
- `git clone` 하면 서브모듈 디렉토리는 빈 디렉토리 이다.
	- `git submodule init`
	- `git submodule update`
	- `git clone --recursive`와 동일
- 서브 모듈 업데이트 : `git fetch` + `git merge` = `git submodule update --remote`
- 서브 모듈 커밋된 내용 확인 : `git diff --submodule`

### Bundle

- 데이터를 한 파일에 몰아넣는 것 : `git bundle`
	1. 네트워크 불통
	2. 보안상의 이유로 로컬 네트워크에 접속이 불가능할 때
	3. 통신 인터페이스 장비가 고장났을 때
	4. 공용 서버에 접근하지 못할 때
- Bundle 파일 생성 : `git bundle create`
	- `commits.bundle` 파일 생성
- Bundle 파일 검증 : `git bundle verify`

### Replace

- 히스토리에 저장된 Git의 개체는 기본적으로 변경이 불가능 하다. 하지만, 변경된 것 처럼 보이게 하는 기능 : `git replace`

### Credential 저장소

- 인증정보를 저장해두고 자동으로 입력해주는 시스템 제공
	- 기본 : 매번 사용자 이름과 임호 입력
	- "cache" : 메모리에 인증정보 저장. 15분 유지
	- "store" : Disk의 텍스트 파일로 저장 및 유지
		- 사용자 홈 디렉토리 아래에 일반 텍스트 파일로 저장되므로 보안에 취약
	- Mac은 `osxkeychain` 모드 사용(암호화)
	- Windows는 `wincred` Helper 사용(암호화)
