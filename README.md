# Lab Seminar Archive - v2

연구실 세미나 자료를 관리하는 웹 기반 아카이브 시스템입니다.

왜 업데이트가 안되지?
## 주요 변경사항 (v2)

이 버전은 AI 요약 기능을 제거하고 세션 중심의 뷰 시스템으로 개편되었습니다.

### 변경된 기능

1. **메인 페이지 (index.html)**
   - 녹음 기능 유지
   - AI 요약 기능 제거
   - 녹음 내용을 텍스트 파일(.txt)로 저장
   - Recent Uploads 목록 클릭 시 세션 페이지로 이동

2. **카테고리 페이지 (category.html)**
   - 파일 업로드 기능 유지
   - AI 요약 기능 제거
   - "View Session" 버튼으로 세션 페이지 접근

3. **세션 페이지 (session.html) - 신규**
   - 업로드된 파일의 상세 정보 표시
   - 파일 다운로드 및 열기 기능
   - AI 요약 플레이스홀더 (향후 구현 예정)
   - 설명(Description) 섹션

## 파일 구조

```
lab-seminar-v2/
├── index.html                    # 메인 페이지
├── category.html                 # 카테고리별 목록 페이지
├── session.html                  # 세션 상세 페이지 (신규)
├── 2026-03-20-groupvit.html     # 샘플 세션 (정적 HTML)
└── README.md                     # 프로젝트 설명
```

## 기능

### 1. 세 가지 카테고리
- **Paper Seminar**: 논문 발표 및 요약
- **Vibe Coding**: 코딩 세션 및 구현 데모
- **Math Study**: 수학 개념 설명 및 증명

### 2. 녹음 기능
- 브라우저에서 직접 오디오 녹음
- 녹음 내용을 텍스트 파일로 저장
- IndexedDB에 로컬 저장

### 3. 파일 업로드
- PDF, PPT, PPTX 파일 업로드 지원
- 메타데이터 입력 (날짜, 발표자, 제목, 설명)
- 로컬 브라우저에 저장 (서버 불필요)

### 4. 세션 상세 보기
- 각 업로드된 파일마다 전용 세션 페이지
- 파일 미리보기 및 다운로드
- AI 요약 섹션 (향후 구현 예정)

## 사용 방법

### 로컬에서 실행

1. 프로젝트 폴더에서 간단한 HTTP 서버 실행:
```bash
# Python 3
python -m http.server 8000

# Node.js (http-server 설치 필요)
npx http-server -p 8000
```

2. 브라우저에서 접속:
```
http://localhost:8000
```

### GitHub Pages 배포

1. GitHub에 새 저장소 생성
2. 파일들을 저장소에 푸시
3. Settings > Pages에서 GitHub Pages 활성화
4. `main` 브랜치를 소스로 선택

## 데이터 저장

모든 데이터는 브라우저의 **IndexedDB**에 저장됩니다:
- 데이터베이스 이름: `lab-seminar-archive-db`
- 스토어: `uploads`
- 서버나 백엔드 불필요

> ⚠️ 주의: 브라우저 데이터를 삭제하면 모든 업로드된 자료가 사라집니다.

## 향후 개선 계획

1. **AI 요약 기능** (선택적)
   - 업로드된 파일을 AI가 분석하여 요약 생성
   - session.html에 요약 표시

2. **검색 기능**
   - 제목, 발표자, 날짜로 검색

3. **태그 시스템**
   - 키워드 태그로 분류 및 필터링

4. **데이터 백업/복원**
   - IndexedDB 데이터를 JSON으로 내보내기/가져오기

## 라이선스

© 2026 KMU AI Lab. All rights reserved.

## 기여

KMU AI Lab 연구실 세미나 아카이브 프로젝트입니다.
