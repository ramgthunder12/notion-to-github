# 📦 Notion to GitHub Sync

Notion Database 페이지를 GitHub 저장소에 Markdown 형식으로 자동 백업하는 도구입니다.
GitHub Actions를 이용해 매일 정해진 시간에 자동 실행되며, 수동 실행을 통해 테스트를 지원합니다.

## 📝 프로젝트 소개

- 설명  
    이 프로젝트는 Notion Database → GitHub Markdown 자동 백업 도구입니다.
- 만든 계기  
    이 프로젝트는 Notion에 작성한 글을 GitHub에도 다시 작성해야 하는 이중 작업 때문에 발생하는 시간과 비용 낭비 문제를 해결하기 위해 만들어졌습니다.
    

## ✨ 기능

### 페이지 자동 백업
Notion 데이터베이스에 작성된 모든 페이지를 Markdown 파일로 변환해 GitHub 저장소에 자동 저장합니다.

### 파일명 정리
페이지 제목을 파일명으로 사용하며, `\ / * ? : " < > |` 같은 특수문자를 제거하고 저장됩니다.

### 자동 업데이트
Notion 내용이 수정되면 GitHub 저장소도 자동으로 갱신되어 항상 최신 상태를 유지합니다.

### 에러 알림
동기화 과정에서 오류가 발생하면 등록된 이메일로 알림을 받아 문제를 바로 확인할 수 있습니다.

### 텍스트 스타일 지원
볼드체, 이탤릭체, 코드, 취소선, 밑줄 등 Notion에서 작성한 다양한 서식이 그대로 반영됩니다.

### 블록 요소 지원
제목(Heading), 문단, 체크박스, 인용구, 코드 블록, 토글, 리스트 등 다양한 블록 형식이 Markdown으로 변환됩니다.

### 자동 실행
매일 한국 시간 기준 오전 8시에 자동으로 백업이 실행됩니다.  
기본으로 오전 8시에 동작하며, 수동 실행을 통해 백업이 정상적으로 작동하는지 직접 테스트해볼 수 있습니다.

### 수동 실행
필요할 때는 GitHub Actions에서 직접 실행해 즉시 백업을 진행할 수 있습니다.


## 📖 사용방법  

### 📝 설문 참여 안내  
이 도구를 사용하기 전 아래 설문에 참여해주세요.  
[**👉 설문지 바로가기**](https://docs.google.com/forms/d/e/1FAIpQLSe3cP1dzmnMDuKH6M5NMfJI5lD7JHMYyW2tYokaSCW7cU5M4A/viewform?usp=dialog)

설문 순서  
1. 사전 문답에 답변합니다.  
2. README의 자세한 설정 방법에 따라 도구를 설정하고 사용합니다.  
3. 사용 후 나머지 문답을 작성해 제출합니다.


### ⏩ 큰흐름
1. GitHub 코드 가져오기 → 레포지토리를 클론
2. Notion 설정 → 데이터베이스 및 API 토큰 준비
3. GitHub 설정 → GitHub Actions 및 Secrets 설정

### 📂 디렉토리 구조
```
.
├── notion-sync/                  # Notion → GitHub 변환 관련 디렉토리
│   ├── notion_to_md.py           # 실행 스크립트 (Notion 페이지를 Markdown으로 변환)
│   └── .env                      # 로컬 전용 환경변수 파일 (OUTPUT_DIR 설정)
├── .github/
│   └── workflows/                # GitHub Actions 워크플로 디렉토리
│       └── notion-to-github.yml       # Notion Sync 자동 실행 설정 파일
└── images/                       # README 설명 이미지 디렉토리
```

### 🔍 자세한 설정법
<details>
<summary>1. 🐙 GitHub 코드 가져오기 (Fork)</summary>

1. Fork 버튼 클릭 (Create new fork)  
    Notion-to-Github 레파지토리 상단에 fork 버튼 누르기  
    ![1 GitHub 코드 가져오기 (fork)_1 Fork 버튼 클릭(Create new fork).png](./images/1%20GitHub%20코드%20가져오기%20(fork)_1%20Fork%20버튼%20클릭(Create%20new%20fork).png)
2. Owner(계정) 선택하기
    - Owner : Notion-to-Github 레파지토리를 저장 할 계정을 선택  
    ![1 GitHub 코드 가져오기 (fork)_2 계정 선택하기.png](./images/1%20GitHub%20코드%20가져오기%20(fork)_2%20계정%20선택하기.png)
3. Create fork 클릭
</details>

<details>
<summary>2. 🗂 Notion 설정</summary>

#### (1) 데이터베이스 생성  

1. Notion 회원 가입하기
2. `/데이터베이스` 입력 → 데이터베이스-**전체 페이지** 선택(Table-Fullpage)  
[데이터베이스-전체페이지 생성 참고](https://m.blog.naver.com/freewill_life/221928219258)  
![(1) 데이터베이스 생성_1 데이터베이스 입력-전체 페이지 생성.png](./images/(1)%20데이터베이스%20생성_1%20데이터베이스%20입력-전체%20페이지%20생성.png)
3. 데이터 베이스 생성 완료
4. 데이터베이스 이름 작성 및 테스트 페이지 작성  
![(1)데이터베이스 생성_4데이터베이스 이름 작성 및 테스트 페이지 작성](./images/(1)데이터베이스%20생성_4데이터베이스%20이름%20작성%20및%20테스트%20페이지%20작성.png)  

#### (2) Database ID 가져오기  

1. 데이터베이스에 가서 창닫기 버튼 옆 공유 클릭
2. 링크 복사 클릭
3. https://www.notion.so/**DATABASE_ID**?v=~~~  
4. DATABASE_ID 부분의 텍스트 복사 후 메모장에 잠시 보관해 두기
    - 메모장에 “DATABASE_ID : (여기에 붙여 넣기)” 정리 해두기  
    - [GitHub Secret Variables 설정 시 필요함](#1-GitHub-Secret-Variables-설정)  

#### (3) Notion API 생성

1. [Notion Integration 페이지](https://www.notion.so/my-integrations) 접속 후 로그인  
2. 새 API 통합 클릭  
![(3) Notion API 생성_2 새 API 통합 개발 클릭.png](./images/(3)%20Notion%20API%20생성_2%20새%20API%20통합%20개발%20클릭.png)
3. 새 API 통합 작성  
    - API 통합 이름 : `to-github`(추천)
    (⚠️ `notion` 이라는 단어는 포함 불가)
    - 관련 워크스페이스 : 데이터베이스가 있는 워크스페이스
    - 유형: `private`  
    ![(3)Notion API 생성_3 새 API 통합작성.png](./images/(3)Notion%20API%20생성_3%20새%20API%20통합작성.png)
4. “API 통합이 생성되었습니다.” API 통합 설정 구성 클릭  
![(3)Notion API 생성_4 API통합이 생성되었습니다.png](./images/(3)Notion%20API%20생성_4%20API통합이%20생성되었습니다.png)
5. 저장 후 API Key 복사
    - “표시하기” 클릭 → “복사” 클릭
    - 메모장에 “NOTION_API_KEY : (여기에 붙여 넣기)” 정리 해두기
    - [GitHub Secret Variables 설정 시 필요함](#1-GitHub-Secret-Variables-설정)  
    ![(3)Notion API 생성_5 저장 후 API key 복사.png](./images/(3)Notion%20API%20생성_5%20저장%20후%20API%20key%20복사.png)  
    
#### (4) GitHub 연결 (DB 공유)

1. 사용할 데이터베이스에서 창닫기 버튼 아래 `…` 클릭  
![(4) GitHub 연결 (DB 공유)_1사용할 데이터베이스에서 창닫기 버튼 아래클릭.png](./images/(4)%20GitHub%20연결%20(DB%20공유)_1사용할%20데이터베이스에서%20창닫기%20버튼%20아래클릭.png)  
2. 연결 클릭
3. [내가 만든 Notion API 이름(`to-github` 추천)](#3-Notion-API-생성) 검색 후 클릭  
![(4) Github 연결(DB공유)_3 내가만든 API이름 검색.png](./images/(4)%20Github%20연결(DB공유)_3%20내가만든%20API이름%20검색.png)

</details>

<details>
<summary>3. 🐙 GitHub 설정</summary>

#### (1) GitHub Secret Variables 설정

1. Notion to GitHub 코드를 Fork한 레포지토리에서 상단 탭의 Settings 클릭  
    <details>
    <summary>용어 설명</summary>

    - 클론(Clone) : 프로젝트 전체를 내 로컬 저장소(내 컴퓨터)에 복사  
    - 포크(Fork) : 프로젝트 전체를 내 원격 저장소(GitHub 계정)에 복사  
    - 레포지토리(Repository) : GitHub 저장소  
    - 탭(Tab) : GitHub 상단의 메뉴 항목  
    </details>

2. 좌측 메뉴에서 Security → Secrets and variables → Actions 클릭  
3. New repository secret 버튼 클릭  
4. Name과 Secret 입력
    - Name : 작성 해야 할 이름 (환경 변수명)(반드시 일치해야 함 → 오타 주의)
    - Secret : 복사 붙여 넣기 할 실제 값 (작성 후 다시 확인 불가 → 복사 저장 권장)
        
        <details>
        <summary>🔑 필수로 만들어야할 Secret 변수 목록</summary>
                
        1. name : NOTION_API_KEY  
            secret : Notion에서 발급받은 API 키  
            ➜[NOTION_API_KEY 발급방법](#3-Notion-API-생성)
        
        2. name : DATABASE_ID  
            secret : Notion 데이터베이스 고유 번호  
            ➜[DATABASE_ID 가져오는 방법](#2-Database-ID-가져오기)    
        
        3. name : SENDER_EMAIL  
            secret : 송신 이메일 주소 (예: [ramgthunder12@gmail.com](mailto:ramgthunder12@gmail.com))                
        
        4. name : EMAIL_PASSWORD  
            secret : 송신 이메일 계정 비밀번호 (예: NotionToGithub9080$)
        
        5. name : RECEIVER_EMAIL  
            secret : 오류 알림을 받을 수신 이메일 주소 (예: [ramgthunder12@gmail.com](mailto:ramgthunder12@gmail.com))      
        
        6. name : GH_TOKEN  
            secret : GitHub에서 push/commit 권한을 가진 Personal Access Token
        
            <details>
            <summary>GitHub Personal Access Token 발급 방법</summary>

            1. 프로필 아이콘 → Settings
            2. 좌측 하단 Developer settings
            3. Personal Access Tokens → Tokens (classic)  
            4. Generate new token (classic) 클릭
            5. Note에 GH_TOKEN 입력
            6. 권한 체크
                - workflow (GitHub Actions)
                - write:packages (GitHub Package)
                - admin:repo_hook
                - delete_repo
                ![(1) GitHub Secret Variables 설정_3 Name과 Secret 입력_GitHub Personal Access Token 발급 방법_권한체크_1.png](./images/(1)%20GitHub%20Secret%20Variables%20설정_3%20Name과%20Secret%20입력_GitHub%20Personal%20Access%20Token%20발급%20방법_권한체크_1.png)  
                ![(1) GitHub Secret Variables 설정_3 Name과 Secret 입력_GitHub Personal Access Token 발급 방법_권한체크_2.png](./images/(1)%20GitHub%20Secret%20Variables%20설정_3%20Name과%20Secret%20입력_GitHub%20Personal%20Access%20Token%20발급%20방법_권한체크_2.png)
            7. Generate token 클릭
            8. 복사 후 메모장에 저장  
            ![(1)GitHub Secret Variables 설정_3 Name과 Secret 입력_GitHub Personal Access Token 발급 방법_8 복사 후 메모장에 저장.png](./images/(1)GitHub%20Secret%20Variables%20설정_3%20Name과%20Secret%20입력_GitHub%20Personal%20Access%20Token%20발급%20방법_8%20복사%20후%20메모장에%20저장.png)
            </details>
            </details>  

    5. Add secret 버튼 클릭

#### (2) .env 설정
- 파일 위치 : [./notion-syn/.env](./notion-sync/.env)  
- 설정 방법 : (파일 위치 클릭)`.env` 파일에서 ./NotionTIL 글씨 중 NotionTIL만 지우고 원하는 파일명 작성(노션의 데이터베이스 페이지들이 들어갈 파일명)  
- 필수 값  
![(2)env설정_설정방법.png](./images/(2)env설정_설정방법.png)
  - OUTPUT_DIR : Notion 페이지를 저장할 폴더명 (Notion 데이터베이스 이름과 동일하게 설정 권장)  


#### (3) 추가 설정
<details>
<summary>⏰ 시간 설정</summary>

- 파일 위치 : [./.github/workflows/workflow_dispatch.yml](./.github\workflows\notion-to-github.yml)  
- 설정 방법 : 7번 줄의 23을 원하는 시간으로 변경  
  - GitHub Actions는 UTC 기준으로 동작  
  - 예: 23시 (UTC) = 한국 시간 오전 8시  
  - 원하는 한국 시간 -9시간 값으로 설정  
</details>
<details>
<summary>🕹️ 수동 시작</summary>  

1. 포크한 레포지토리에서 상단 탭 Actions 클릭  
![(3)추가설정_수동시작_1.png](./images/(3)추가설정_수동시작_1.png)  
2. 왼쪽 목록에서 Notion to GitHub Commit 선택  
![(3)추가설정_수동시작_2.png](./images/(3)추가설정_수동시작_2.png)  
3. Run workflow 클릭  
![(3)추가설정_수동시작_3.png](./images/(3)추가설정_수동시작_3.png)  
4. 초록색 Run workflow 버튼 클릭  
![(3)추가설정_수동시작_4.png](./images/(3)추가설정_수동시작_4.png)  
5. 실행 목록에 초록색 동그라미🟢 가 보이면 완료  
![(3)추가설정_수동시작_5.png](./images/(3)추가설정_수동시작_5.png)  
6. 레포지토리에서 OUTPUT_DIR에 입력한 폴더가 생성됐는지 확인 → 폴더에 들어가 Notion 데이터베이스 페이지들이 정상 저장됐는지 확인  
![(3)추가설정_수동시작_6.png](./images/(3)추가설정_수동시작_6.png)  

</details>

</details>

## 🧩 문제 해결 FAQ

- **Settings 탭이 안 보일 때**
  - 오른쪽 상단 `...` 클릭 또는 전체화면 전환  

- **저장 한도**
  - GitHub 리포지토리: 1GB 이하 권장 (100MB 이상은 LFS 필요)  

- **토큰 만료**  
  GitHub 가입 시 사용한 이메일로 만료 알림이 오면,  
  새 토큰을 생성해 다시 등록해야 합니다. [GitHub Secret Variables 설정 방법](#1-GitHub-Secret-Variables-설정)



## 🚀 향후 계획 (Roadmap)

- [ ] 코드 블록 내부 텍스트 변환 개선
- [x] 설문 질문 작성  

문의: **ramgthunder12@gmail.com**
