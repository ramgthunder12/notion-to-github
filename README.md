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
.
├── notion-sync/                  # Notion → GitHub 변환 관련 디렉토리
│   ├── notion_to_md.py           # 실행 스크립트 (Notion 페이지를 Markdown으로 변환)
│   └── .env                      # 로컬 전용 환경변수 파일 (OUTPUT_DIR 설정)
└── .github/
    └── workflows/                # GitHub Actions 워크플로 디렉토리
        └── notion-sync.yml       # Notion Sync 자동 실행 설정 파일


