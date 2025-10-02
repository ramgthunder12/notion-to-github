# 📦 Notion to GitHub Sync

Notion Database 페이지를 GitHub 저장소에 Markdown 형식으로 자동 백업하는 도구입니다.
GitHub Actions를 이용해 매일 정해진 시간에 자동 실행되며, 수동 실행도 지원합니다.

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

### 수동 실행
필요할 때는 GitHub Actions에서 직접 실행해 즉시 백업을 진행할 수 있습니다.

## 📖 사용방법  

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
└── .github/
    └── workflows/                # GitHub Actions 워크플로 디렉토리
        └── notion-sync.yml       # Notion Sync 자동 실행 설정 파일
```

### 🔍 자세한 설정법
<details>
<summary>1. 🐙 GitHub 코드 가져오기 (Fork)</summary>

1. Fork 버튼 클릭 (Create new fork)  
    Notion-to-Github 레파지토리 상단에 fork 버튼 누르기
2. 계정 선택하기
    - Owner : 소유할 계정을 선택  
3. Create fork 클릭
</details>

<details>
<summary>2-1. 🗂 Notion 설정</summary>

#### (1) 데이터베이스 생성
1. Notion 회원 가입하기
2. `/데이터베이스` 입력 → 데이터베이스 전체 페이지 선택
3. 생성 완료
4. 데이터베이스 이름 작성 및 테스트 페이지 작성  

#### (2) Database ID 가져오기
1. 설정 클릭(새로 만들기 옆 에 위치한 버튼)  
2. 데이터 소스 관리 클릭
3. 해당 데이터베이스의 `…` 클릭  
4. 데이터 소스 ID 복사(Database ID)   
    메모장에 붙여넣어 잠시 보관해 두기 

</details>

<details>
<summary>2-2. 🗂 Notion API 생성</summary>

1. [Notion Integration 페이지](https://www.notion.so/my-integrations) 접속 후 로그인  
2. 새 API 통합 클릭  
3. 새 API 통합 작성
   - API 통합 이름 : `to-github`  
     (⚠️ `notion` 이라는 단어는 포함 불가)  
   - 관련 워크스페이스 : 데이터베이스가 있는 워크스페이스 
   - 유형: `private`  
4. 저장 후 API Key 복사  
   - “표시하기” 클릭 → “복사” 클릭  
</details>

<details>
<summary>2-3. 🗂 GitHub 연결 (DB 공유)</summary>

1. 사용할 DB 맨 오른쪽 위 `…` 클릭(공유 ☆ ...)  
2. 연결 클릭  
3. 방금 만든 API 통합(`to-github`) 검색 후 클릭  
4. 연결 확인  

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
    - Name : 환경 변수명 (반드시 일치해야 함 → 오타 주의)  
    - Secret : 실제 값 (작성 후 다시 확인 불가 → 복사 저장 권장)  
5. Add secret 버튼 클릭  

<details>
<summary>🔑 필수 Secret 변수</summary>

- NOTION_API_KEY : Notion에서 발급받은 API 키  
- DATABASE_ID : Notion 데이터베이스 고유 번호  
- SENDER_EMAIL : 송신 이메일 주소 (예: ramgthunder12@gmail.com)  
- EMAIL_PASSWORD : 송신 이메일 계정 비밀번호 (예: NotionToGithub9080$)  
- RECIVER_EMAIL : 오류 알림을 받을 수신 이메일 주소 (예: ramgthunder12@gmail.com)  
- GH_TOKEN : GitHub에서 push/commit 권한을 가진 Personal Access Token  

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
7. Generate token 클릭  
8. 복사 후 메모장에 저장  
</details>
</details>

---

#### (2) .env 설정
- 파일 위치 : 프로젝트 루트  
- 설정 방법 : `.env` 파일에서 TILDB 글씨를 지우고 원하는 파일명 작성  
- 필수 값  
  - OUTPUT_DIR : Notion 페이지를 저장할 폴더명 (Notion 데이터베이스 이름과 동일하게 설정 권장)  

---

#### (3) 추가 설정
<details>
<summary>⏰ 시간 설정</summary>

- 파일 위치 : `.github/workflows/workflow_dispatch.yml`  
- 설정 방법 : 7번 줄의 23을 원하는 시간으로 변경  
  - GitHub Actions는 UTC 기준으로 동작  
  - 예: 23시 (UTC) = 한국 시간 오전 8시  
  - 원하는 한국 시간 -9시간 값으로 설정  
</details>
</details>
