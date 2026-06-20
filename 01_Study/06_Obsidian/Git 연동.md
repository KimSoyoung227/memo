#Obsidian #Git

# GitHub 에서 Repository 생성
Obsidian과 연동할 Repository는 기존에 존재하는 Repo를 사용해도 되고 새롭게 생성해도 된다.
나는 memo 이름의 Repo를 새로 만들었다.
![[스크린샷 2026-06-20 오후 5.50.27 1.png]]

# Git 설치
Obsidian에서 작업하기 전에 로컬에 Git이 설치되어 있는지 확인한다.
###### 터미널에서 Git이 설치되어 있는지 확인하기
```
git --version # 버전 정보가 출력되면 Git이 설치된 상태
```

만약 `git: command not found` 에러가 나오면 git이 설치되지 않은 상태이므로 git을 설치해준다.
###### 맥OS에서 Homebrew를 이용하여 Git 설치하기
```
brew install git
```

# GitHub Repo 연동하기(터미널)
GitHub Repo 연동하는 작업은 터미널을 이용하는 것을 추천한다. 처음부터 Obsidian 플러그인으로 설정하려고 하면 중간에 발생하는 에러를 분석하기가 어렵기 때문이다.

나는 Obsidian Vault 를 Private 와 Public으로 구분한 후 Public/06_Daily/의 하위 디렉토리 및 파일만 Git 과 연결하고자 한다.
```
Obsidian Vault/
├── .obsidian
├── Private
└── Public
    └── 06_Daily
	    └── 01_Study
		    └── 02_AI
```

###### 터미널에서 GitHub와 연동할 최상위 디렉토리로 이동하기
```
cd .../Obsidian Valt/Public/06_Daily
```

###### Git 초기화하기
```
git init
```

 에러가 없다면 `.git` 파일이 생성되는 것을 확인할 수 있다.
```
Obsidian Vault/
├── .obsidian
├── Private
└── Public
    └── 06_Daily
	    ├── .git. #.git 파일이 생성된다.
	    └── 01_Study
		    └── 02_AI
```

GitHub Repo가 Private하면 괜찮지만 Public하면 개인정보나 불필요한 파일들은 업로드 자체를 제외시켜주는 것이 좋다. 해당 세팅을 위해 `.gitignore` 파일을 생성한다. 
###### 터미널에서 `.gitignore` 파일 만들기
```
touch .gitignore
```

만약 Obsidian Valt 디렉토리 자체를 Git과 연동할 경우, `.obsidian` 디렉토리를 제외하는 것을 추천한다.

>
> **Q. `.obsidian` 파일에는 무엇이 저장되는가?**
> A.  테마, 플러그인, 창 배치, 개인 설정 등이 들어있는데 특히 `workspace.json`에는 내가 최근 오픈한 파일경로 및 파일명, 탭 정보, 사이드바 상태 등이 저장된다. 잘못하면 개인정보가 유출될 수 있다. `plugins` 는 임시 파일, 캐시, 버전 정보가 저장되고 있어 불필요한 파일을 Git에 업로드할 필요는 없다.
> 
> 굳이 `.obsidian` 디렉토리를 git과 연동할 예정이라면 아래의 파일 및 디렉토리는 `.gitignore`에 추가하는 것을 권장한다.
>
> ```text
> .obsidian/  
>├── workspace.json  
>├── workspace-mobile.json  
>├── graph.json  
>├── cache/  
>└── plugins/*/data.json
> ```


나는 Public/06_Daily/ 하위 경로만 Git과 연결하므로`.obsidian` 디렉토리를 추가하지 않았다.
###### vi 편집기로 `.gitignore` 수정
```
.DS_Store # 맥북 메타데이터 파일. 윈도우는 thumbs.db 파일과 동일
*.pdf # pdf확장자는 저작권으로 업로드 제외하였다.
```

GitHub 기본 연동 작업을 진행한다. 이 부분은 사람들마다 방법이 다르기 때문에 각자의 방법대로 연동하면 될 듯 하다.

> git을 처음 설치한 경우, username과 email을 추가 설정해야 한다. 보안 인증도 추가 설정할 수 있다.
> 여기서는 git 기본 설정 작업은 생략한다.
###### 터미널에서 GitHub 연동 작업 및 테스트하기
```
git status
git remote add origin <Repo URL>
git branch -M main
git add <업로드파일>
git commit -m <커밋메시지>
git push origin main
git pull origin main
```

만약 `git push`와 `git pull`이 터미널에서 정상적으로 수행되었다면 이제 Obsidian에서 Git 플러그인을 설치해보자.

# Obsidian Git 플러그인 설치
'Obsidian 설정 모달창-옵션-커뮤니티 플러그인 메뉴-커뮤니티 플러그인' 설정의 `탐색` 버튼을 클릭한다.
![[스크린샷 2026-06-20 오후 7.08.51.png]]

검색창에 `Git`을 입력하면 `Git`이라는 플러그인이 나올 것이다. 이것을 클릭한다.
![[스크린샷 2026-06-20 오후 7.09.06.png]]

`설치` 버튼을 클릭한다.
![[스크린샷 2026-06-20 오후 7.09.12.png]]

Obsidian이 플러그인 설치를 완료하면  Git 옆에 '설치됨' 태그가 생성된다.
이 태그가 확인이 되면 `활성화`버튼을 클릭한다.
![[스크린샷 2026-06-20 오후 7.09.20.png]]

활성화가 완료되면 Obsidian을 재실행한다.

창 왼쪽 측면에 세로로 긴 툴 바를 `리본`이라고 칭하는데, `리본`에 Git 아이콘이 추가된 것을 확인할 수 있다.
![[스크린샷 2026-06-20 오후 7.11.54 1.png]]

리본메뉴에 추가된 Git 아이콘을 클릭하면 오른쪽 네비게이션 바에 `Source control` 창이 열린다.![[스크린샷 2026-06-20 오후 7.12.01.png]]

`Refresh`아이콘을 클릭하면 Git과 연동되어 수정되거나 Stage에 올라간 파일들을 확인할 수 있다.
![[스크린샷 2026-06-20 오후 7.15.31.png]]

이제는 터미널에서 명령어를 입력하지 않고 Obsidian에서 마우스를 딸깍하면서 Git에 데이터를 업로드할 수 있다.