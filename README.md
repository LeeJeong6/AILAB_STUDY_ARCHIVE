# Lab Seminar Archive

**연구실 세미나 자료를 관리하는 웹 기반 아카이브 시스템**

GitHub Pages를 통해 운영되며, 브라우저의 IndexedDB를 활용하여 서버 없이 완전히 클라이언트 측에서 작동합니다.

---

## 📋 목차

- [주요 기능](#-주요-기능)
- [페이지 구성](#-페이지-구성)
- [카테고리](#-카테고리)
- [기능 상세 설명](#-기능-상세-설명)
- [사용 방법](#-사용-방법)
- [데이터 저장](#-데이터-저장)
- [기술 스택](#-기술-스택)
- [브라우저 호환성](#-브라우저-호환성)
- [배포 방법](#-배포-방법)
- [주의사항](#-주의사항)

---

## ✨ 주요 기능

### 📤 파일 업로드 및 관리
- PDF, PPT, PPTX 파일 업로드
- 메타데이터 입력 (날짜, 발표자, 제목, 설명)
- 파일 다운로드 및 브라우저에서 바로 열기
- 세션별 상세 페이지

### 🎙️ 음성 녹음
- 브라우저에서 직접 오디오 녹음
- 카테고리별 녹음 저장
- 녹음 시간 표시 및 실시간 모니터링

### 🔄 드래그 앤 드롭 순서 변경
- **메인 페이지**: 카테고리별 최신 3개 항목의 순서 조정
- **카테고리 페이지**: 모든 항목의 순서 자유롭게 조정
- 변경된 순서는 IndexedDB에 영구 저장

### 🗑️ 삭제 기능
- 메인 페이지에서 직접 삭제
- 카테고리 페이지에서 세션 삭제
- 삭제 전 확인 다이얼로그

### 🔄 실시간 동기화
- **BroadcastChannel API** 사용
- 여러 탭/창에서 동시에 열어도 자동 동기화
- 카테고리 페이지에서의 변경사항이 메인 페이지에 즉시 반영
- 지원 이벤트: 업로드, 삭제, 순서 변경

---

## 🗂️ 페이지 구성

```
lab-seminar-v2/
├── index.html                    # 메인 페이지 (홈)
├── category.html                 # 카테고리별 목록 페이지
├── session.html                  # 세션 상세 페이지
└── README.md                     # 프로젝트 문서
```

### 1. 메인 페이지 (index.html)

**기능:**
- 3개 카테고리 개요 카드
- 카테고리별 최신 3개 항목 미리보기
- 🎙️ 음성 녹음 시작 버튼
- 각 항목의 드래그 앤 드롭 순서 변경
- View Session / Delete 버튼

**사용자 동작:**
- 카테고리 카드 클릭 → 해당 카테고리 페이지로 이동
- 미리보기 항목 클릭 → 세션 상세 페이지로 이동
- View Session 버튼 → 세션 상세 페이지로 이동
- Delete 버튼 → 항목 삭제 (확인 후)
- 드래그 핸들(⋮⋮) → 순서 변경

### 2. 카테고리 페이지 (category.html)

**기능:**
- 선택한 카테고리의 모든 세션 표시
- 📤 Upload New Material 버튼
- 파일 업로드 폼 (토글 가능)
- 드래그 앤 드롭으로 세션 순서 변경
- View Session / Delete Session 버튼
- Open File / Download 버튼 (업로드된 파일의 경우)

**업로드 폼 필드:**
- Date (날짜)
- Presenter (발표자)
- Title (제목)
- Description (설명)
- File (PDF, PPT, PPTX)

**사용자 동작:**
- Upload 버튼 → 파일 업로드 폼 표시/숨김
- 폼 작성 후 Upload 버튼 → 세션 생성
- View Session → 세션 상세 페이지
- Delete Session → 세션 삭제 (확인 후)
- Open File → 새 탭에서 파일 열기
- Download → 파일 다운로드
- 드래그 핸들(⋮⋮) → 순서 변경

### 3. 세션 상세 페이지 (session.html)

**기능:**
- 세션 제목, 날짜, 발표자 정보
- 카테고리 배지
- 파일 정보 및 다운로드/열기 버튼
- AI 요약 플레이스홀더 (향후 구현 예정)
- 상세 설명(Description) 표시

**사용자 동작:**
- Back 버튼 → 카테고리 페이지 또는 홈으로 돌아가기
- Open File → 새 탭에서 파일 열기
- Download File → 파일 다운로드
- Download Text → 텍스트 파일 다운로드 (녹음의 경우)

---

## 📁 카테고리

시스템은 3개의 주요 카테고리로 구성됩니다:

### 1. 📄 Paper Seminar
논문 발표 및 리뷰 자료

### 2. 💻 Vibe Coding
코딩 세션 및 구현 데모 자료

### 3. 📐 Math Study
수학 개념 설명 및 증명 자료

---

## 🔍 기능 상세 설명

### 1. 파일 업로드 시스템

**지원 파일 형식:**
- PDF (`.pdf`)
- PowerPoint (`.ppt`, `.pptx`)

**업로드 프로세스:**
1. 카테고리 페이지에서 "Upload New Material" 클릭
2. 폼 작성 (모든 필드 필수)
3. Upload 버튼 클릭
4. IndexedDB에 파일과 메타데이터 저장
5. 업로드 폼 자동 닫힘 (1.5초 후)
6. 다른 탭의 메인/카테고리 페이지에 자동 반영

**저장되는 데이터:**
```javascript
{
  id: 'uuid',
  category: 'paper' | 'vibe' | 'math',
  date: '2026-03-24',
  presenter: '발표자 이름',
  title: '세션 제목',
  description: '상세 설명',
  fileName: 'file.pdf',
  fileBlob: Blob,
  kind: 'upload',
  order: 0,
  createdAt: '2026-03-24T10:00:00.000Z'
}
```

### 2. 음성 녹음 시스템

**녹음 프로세스:**
1. 메인 페이지에서 "🎙️ Start Study Session" 클릭
2. 마이크 권한 허용
3. 녹음 시작 (시간 표시)
4. "Stop Recording" 버튼 클릭
5. 카테고리 선택 모달 표시
6. 카테고리 선택 후 "Save" 클릭
7. IndexedDB에 오디오 파일과 텍스트 파일 저장

**저장되는 데이터:**
```javascript
{
  id: 'uuid',
  category: 'paper' | 'vibe' | 'math',
  date: '2026-03-24',
  presenter: 'Study Session',
  title: 'Study Session - 2026. 3. 24.',
  description: '녹음 정보 텍스트',
  fileName: 'study-session-2026-03-24T10:00:00.000Z.webm',
  fileBlob: Blob (audio/webm),
  textFileName: 'study-session-2026-03-24T10:00:00.000Z.txt',
  textBlob: Blob (text/plain),
  kind: 'recording',
  createdAt: '2026-03-24T10:00:00.000Z'
}
```

### 3. 드래그 앤 드롭 순서 변경

**메인 페이지:**
- 각 카테고리별로 최신 3개 항목만 표시
- 같은 카테고리 내에서만 순서 변경 가능
- 드래그 중 시각적 피드백 (반투명, 테두리 강조)

**카테고리 페이지:**
- 해당 카테고리의 모든 항목 표시
- 자유롭게 순서 변경 가능
- 드래그 핸들(⋮⋮) 표시

**순서 저장 방식:**
- `order` 속성에 0부터 시작하는 인덱스 저장
- 순서 변경 시 모든 항목의 `order` 업데이트
- 정렬 우선순위: order > date > createdAt

### 4. 삭제 기능

**삭제 가능한 위치:**
- 메인 페이지: 각 미리보기 카드의 Delete 버튼
- 카테고리 페이지: 각 세션 카드의 Delete Session 버튼

**삭제 프로세스:**
1. Delete 버튼 클릭
2. 확인 다이얼로그 표시
3. "확인" 클릭 시 IndexedDB에서 영구 삭제
4. UI 자동 갱신
5. 다른 탭에 변경사항 브로드캐스트

**주의:** 삭제된 데이터는 복구할 수 없습니다.

### 5. 실시간 동기화 시스템

**BroadcastChannel API 사용:**
```javascript
const syncChannel = new BroadcastChannel('lab-seminar-sync');
```

**동기화되는 이벤트:**
- `entry-added`: 새 세션 업로드 또는 녹음 저장
- `entry-deleted`: 세션 삭제
- `entry-reordered`: 순서 변경

**동작 방식:**
1. 한 탭에서 작업 수행 (업로드/삭제/순서변경)
2. `broadcastChange()` 함수로 메시지 전송
3. 다른 탭의 `syncChannel.onmessage` 리스너가 수신
4. 해당 페이지 자동 갱신 (`renderPreviewBoard()` 또는 `renderEntries()`)

**지원 브라우저:**
- Chrome/Edge 54+
- Firefox 38+
- Safari 15.4+

---

## 🚀 사용 방법

### 로컬에서 실행

1. **HTTP 서버 실행**

```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000

# Node.js (http-server 설치 필요)
npx http-server -p 8000
```

2. **브라우저에서 접속**

```
http://localhost:8000
```

### 기본 워크플로우

#### 1. 파일 업로드
1. 카테고리 선택 (Paper Seminar / Vibe Coding / Math Study)
2. "Upload New Material" 클릭
3. 폼 작성 후 Upload
4. 메인 페이지에서 자동으로 확인 가능

#### 2. 음성 녹음
1. 메인 페이지에서 "🎙️ Start Study Session" 클릭
2. 녹음 진행 후 Stop
3. 카테고리 선택
4. 저장

#### 3. 순서 변경
1. 드래그 핸들(⋮⋮) 클릭 후 드래그
2. 원하는 위치에 드롭
3. 자동 저장

#### 4. 세션 삭제
1. Delete 버튼 클릭
2. 확인 다이얼로그에서 "확인"
3. 자동으로 목록에서 제거

---

## 💾 데이터 저장

### IndexedDB 구조

**데이터베이스 이름:** `lab-seminar-archive-db`
**버전:** 1
**Object Store:** `uploads`

**인덱스:**
- `category`: 카테고리별 검색
- `date`: 날짜별 정렬
- `order`: 순서 정렬

**Primary Key:** `id` (UUID)

### 데이터 스키마

```typescript
interface Entry {
  id: string;                    // UUID
  category: 'paper' | 'vibe' | 'math';
  date: string;                  // YYYY-MM-DD
  presenter: string;
  title: string;
  description: string;
  fileName: string;
  fileBlob: Blob;
  textFileName?: string;         // 녹음의 경우
  textBlob?: Blob;               // 녹음의 경우
  kind: 'upload' | 'recording';
  order?: number;                // 순서 (0부터 시작)
  createdAt: string;             // ISO 8601
}
```

### 저장 위치

모든 데이터는 브라우저의 IndexedDB에 저장됩니다:

**경로 (Chrome/Edge):**
```
C:\Users\[사용자]\AppData\Local\Google\Chrome\User Data\Default\IndexedDB
```

**경로 (Firefox):**
```
C:\Users\[사용자]\AppData\Roaming\Mozilla\Firefox\Profiles\[프로필]\storage\default
```

---

## 🛠️ 기술 스택

### 프론트엔드
- **HTML5**: 구조 및 마크업
- **CSS3**: 스타일링 및 레이아웃
  - CSS Variables
  - Flexbox & Grid
  - 반응형 디자인 (모바일 지원)
- **Vanilla JavaScript (ES6+)**
  - Async/Await
  - Promises
  - Arrow Functions
  - Template Literals
  - Destructuring

### 브라우저 API
- **IndexedDB**: 로컬 데이터 저장
- **MediaRecorder API**: 오디오 녹음
- **BroadcastChannel API**: 탭 간 통신
- **Blob API**: 파일 처리
- **File API**: 파일 업로드
- **URL.createObjectURL()**: 파일 미리보기

### 폰트
- **Google Fonts**
  - Roboto (라틴 문자)
  - Noto Sans KR (한글)

---

## 🌐 브라우저 호환성

### 완벽 지원
- ✅ Chrome/Edge 54+
- ✅ Firefox 38+
- ✅ Safari 15.4+
- ✅ Opera 41+

### 부분 지원
- ⚠️ Safari 15.4 미만: BroadcastChannel API 미지원 (실시간 동기화 불가)
- ⚠️ IE 11: 미지원 (ES6+ 문법 사용)

### 필수 기능 확인
```javascript
// IndexedDB
if (!window.indexedDB) {
  alert('Your browser does not support IndexedDB');
}

// MediaRecorder API
if (!window.MediaRecorder) {
  alert('Your browser does not support audio recording');
}

// BroadcastChannel API
if (!window.BroadcastChannel) {
  console.warn('BroadcastChannel not supported - real-time sync disabled');
}
```

---

## 🌍 배포 방법

### GitHub Pages

1. **저장소 생성**
   - GitHub에서 새 저장소 생성
   - Public 또는 Private (Pro 계정 필요)

2. **파일 업로드**
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/[사용자명]/[저장소명].git
git push -u origin main
```

3. **GitHub Pages 활성화**
   - 저장소 → Settings → Pages
   - Source: "Deploy from a branch"
   - Branch: `main` / `(root)`
   - Save

4. **접속**
```
https://[사용자명].github.io/[저장소명]/
```

### Netlify / Vercel

정적 사이트 호스팅 서비스를 사용할 수도 있습니다:

**Netlify:**
1. 저장소 연결
2. Build command: (비워둠)
3. Publish directory: `.`
4. Deploy

**Vercel:**
1. 저장소 연결
2. Framework Preset: "Other"
3. Output Directory: (비워둠)
4. Deploy

---

## ⚠️ 주의사항

### 데이터 백업

**중요:** IndexedDB 데이터는 브라우저에 저장되므로 다음 경우 데이터가 손실될 수 있습니다:

- 브라우저 캐시 삭제
- 브라우저 재설치
- 시크릿/프라이빗 모드 종료
- 저장 공간 부족 시 자동 삭제

**권장사항:**
- 중요한 파일은 별도로 백업하세요
- 정기적으로 파일 다운로드하여 저장하세요
- 향후 데이터 내보내기/가져오기 기능 구현 예정

### 파일 크기 제한

브라우저 IndexedDB는 일반적으로 다음 제한이 있습니다:

- **Chrome/Edge**: 사용 가능한 디스크 공간의 ~60%
- **Firefox**: 사용 가능한 디스크 공간의 ~50%
- **Safari**: 약 1GB

**권장사항:**
- 파일 크기는 50MB 이하로 유지
- 대용량 파일은 외부 저장소 사용 고려

### 마이크 권한

음성 녹음 기능은 마이크 권한이 필요합니다:

1. 브라우저에서 권한 요청 시 "허용" 클릭
2. HTTPS 연결 또는 localhost에서만 작동
3. HTTP 사이트에서는 마이크 권한 거부됨

**GitHub Pages는 자동으로 HTTPS를 제공합니다.**

### 브라우저 간 데이터 공유

IndexedDB는 브라우저별로 독립적으로 관리됩니다:

- Chrome에서 업로드한 데이터는 Firefox에서 보이지 않음
- 같은 브라우저의 다른 프로필도 별도 저장소 사용

---

## 🔮 향후 개선 계획

### 1. AI 요약 기능 (선택적)
- 업로드된 파일 자동 분석
- 요약문 생성 및 세션 페이지에 표시
- 키워드 추출

### 2. 검색 기능
- 제목, 발표자, 설명으로 검색
- 날짜 범위 필터
- 카테고리 필터

### 3. 태그 시스템
- 세션에 키워드 태그 추가
- 태그별 필터링 및 그룹핑
- 태그 클라우드

### 4. 데이터 백업/복원
- IndexedDB → JSON 내보내기
- JSON → IndexedDB 가져오기
- 파일 일괄 다운로드 (ZIP)

### 5. 고급 정렬 옵션
- 날짜순, 제목순, 발표자순
- 오름차순/내림차순

### 6. 사용자 설정
- 테마 변경 (다크 모드)
- 카테고리별 색상 커스터마이징
- 한 페이지에 표시할 항목 수 설정

### 7. 세션 편집 기능
- 메타데이터 수정
- 파일 교체

---

## 📄 라이선스

© 2026 KMU AI Lab. All rights reserved.

---

## 👥 기여

KMU AI Lab 연구실 세미나 아카이브 프로젝트입니다.

**개발:**
- 연구실 구성원들의 협업으로 개발됨

**문의:**
- [KMU AI Lab 홈페이지](https://ailab.kookmin.ac.kr/)

---

## 📞 지원

문제가 발생하거나 개선 제안이 있으시면:

1. GitHub Issues에 등록
2. 연구실 내부 채널을 통해 문의
3. 직접 수정 후 Pull Request

---

**🎉 Lab Seminar Archive를 사용해 주셔서 감사합니다!**
