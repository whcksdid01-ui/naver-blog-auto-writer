# 블로그_제작 프로젝트 정리

## 프로젝트 개요
기존 `C_Blog` 실행파일 세트의 원본 소스가 없는 상태에서, 확인된 UI/입력 형식/동작 흐름을 바탕으로 C# WinForms 프로젝트를 새로 복원한 작업입니다.

현재 목표는 다음과 같습니다.
- 계정 데이터와 상품 데이터를 불러와 관리
- 설정값 저장/불러오기
- Selenium 기반 브라우저 실행
- 계정별 프로필(profile) 재사용
- 향후 블로그 글쓰기 페이지 진입 및 작성 기능 확장

## 개발 환경
- Language: C#
- UI: WinForms
- Runtime: `net10.0-windows`
- IDE: Visual Studio
- Browser Automation: Selenium WebDriver + ChromeDriver

## 현재까지 구현된 기능

### 1. 프로젝트 기본 구조 생성
새 WinForms 프로젝트를 생성하고, 기능별로 Models / Services 구조를 나누어 정리했습니다.

### 2. 데이터 모델 구현
다음 모델을 생성했습니다.

- `Account`
  - `UserId`
  - `Password`

- `ProductData`
  - `Title`
  - `DecorateTitle`
  - `FirstLine`
  - `ImageUrl`
  - `ProductUrl`

- `AppSettings`
  - `ApiKey`
  - `AiModel`
  - `TotalCount`
  - `GroupCount`
  - `UseTitleKeyword`
  - `UseTetheringChange`
  - `UseProxyHotkeyVpn`
  - `ResourceFolder`
  - `NoticeFolder`
  - `FooterEmailText`

### 3. 파서 구현
`DataParserService`에서 다음 형식을 처리하도록 구현했습니다.

- 계정 데이터  
  `아이디/비번`

- 상품 데이터  
  `제목〓타이틀〓본문첫줄〓이미지URL〓링크`

추가 규칙:
- `타이틀`은 빈칸 허용
- `본문첫줄`이 비면 `제목`을 fallback으로 사용

### 4. 설정 저장/불러오기
`SettingsService`를 통해 `setting.txt` 기반 저장/불러오기를 구현했습니다.

현재 가능한 동작:
- 폼 값 읽기
- 설정 파일 저장
- 설정 파일 불러오기
- 프로그램 시작 시 `setting.txt` 자동 로드 시도

### 5. 필수값 검증
`ValidationService`를 통해 실행 전 필수값 검증을 구현했습니다.

검사 항목:
- API Key
- 리소스 폴더
- 상단 공지 폴더
- 하단 이메일 입력
- 계정 데이터 존재 여부
- 상품 데이터 존재 여부

### 6. 로그 출력 기능
`LogService`를 통해 로그창에 아래 형식으로 출력하도록 구성했습니다.

- `[DEBUG]`
- `[INFO]`
- `[WARN]`
- `[ERROR]`

### 7. UI 기본 연결
`Form1.cs`, `Form1.Designer.cs`를 정리하고 아래 컨트롤을 연결했습니다.

- `rtbAccounts`
- `rtbProducts`
- `rtbLog`
- `txtApiKey`
- `txtAiModel`
- `txtTotalCount`
- `txtGroupCount`
- `txtResourceFolder`
- `txtNoticeFolder`
- `txtFooterEmail`
- `chkUseTitleKeyword`
- `chkUseTetheringChange`
- `chkUseProxyHotkeyVpn`
- `btnLoadSettings`
- `btnSaveSettings`
- `btnStart`
- `btnStop`

### 8. 파일 불러오기 기능
계정/상품 데이터를 파일에서 불러와 각각의 RichTextBox에 채우는 기능을 구현했습니다.

- 계정 파일 불러오기
- 상품 파일 불러오기
- 로그창에 성공/실패 기록

### 9. Selenium 기초 연결
`BrowserService`를 추가하여 Selenium 기반 브라우저 실행 골격을 구성했습니다.

포함된 항목:
- 계정별 profile 폴더 경로 생성
- 계정별 고정 viewport 반환
- Chrome 실행
- 네이버 메인 이동
- 로그인 상태 확인용 골격
- 블로그 글쓰기 페이지 이동용 골격
- 브라우저 종료 함수

## 현재 프로젝트 구조

```text
블로그_제작/
  Models/
    Account.cs
    ProductData.cs
    AppSettings.cs

  Services/
    DataParserService.cs
    SettingsService.cs
    ValidationService.cs
    LogService.cs
    BrowserService.cs

  Form1.cs
  Form1.Designer.cs
```

## UI 기준 현재 동작 흐름
1. 프로그램 실행
2. `setting.txt` 자동 로드 시도
3. 계정 파일 / 상품 파일 불러오기
4. 설정값 입력 또는 불러오기
5. `동작` 클릭
6. 필수값 검증
7. 계정/상품 데이터 파싱
8. 로그 출력
9. Selenium 브라우저 실행 단계로 확장 가능

## 현재 확인된 입력 형식

### 계정 데이터
```text
id1/pass1
id2/pass2
```

### 상품 데이터
```text
상품명〓꾸밈말〓본문첫줄〓https://이미지주소.jpg〓https://naver.me/xxxxx
```

## 확인된 동작 규칙
- `타이틀`은 꾸밈말 의미
- `profile` 폴더는 계정별 브라우저 프로필 저장 용도
- `blog` 폴더는 합성 이미지 저장 용도로 정리됨
- 이후 발행 단계 목표는 `상품리뷰` 주제 기준

## 현재 상태 요약
현재 프로젝트는 아래까지 완료된 상태입니다.

- 빌드 성공
- 실행 가능
- 설정 저장/불러오기 가능
- 계정/상품 파일 불러오기 가능
- 로그 출력 가능
- Start 버튼에서 입력 검증 및 파싱 가능
- Selenium 브라우저 서비스 기본 구조 추가

즉, **프로그램 골격 + 데이터 입력/검증/로그/기본 브라우저 연결까지 정리된 상태**입니다.

## 아직 미완성인 부분
다음 단계에서 이어서 구현해야 할 항목입니다.

1. BrowserService와 Start 흐름 완전 연결
2. 로그인된 profile 기준 글쓰기 페이지 진입 안정화
3. 제목 입력
4. 본문 입력
5. 인용구 삽입
6. 구분선 삽입
7. 링크 카드 삽입
8. 사진 업로드
9. 동영상 업로드
10. 발행 전 최종 검증
11. 주제 `상품리뷰` 선택
12. 발행 처리

## 이번 작업에서 제외된 항목
아래 항목은 구현하지 않았습니다.

- 기기값/환경값 변경
- 다른 컴퓨터처럼 보이게 하는 설정
- 입력/클릭을 사람처럼 보이게 만드는 방식
- 로그인 자동 처리
- 탐지 회피성 동작

## 트러블슈팅 기록
진행 중 겪었던 대표 이슈:

### 디자이너 깨짐 문제
원인:
- `Form1.Designer.cs` 이벤트 연결과 `Form1.cs` 함수가 맞지 않음
- 예: `label1_Click` 연결 잔존

정리 방향:
- Designer 이벤트 연결과 Code-behind 함수 이름을 일치시키거나
- 불필요한 연결은 제거

### namespace / nullable 경고
- 프로젝트 namespace와 코드 namespace를 일치시켜 해결
- 속성 기본값(`string.Empty`) 추가로 nullable 경고 해결

## 제출용 한 줄 요약
원본 실행파일 기반 구조를 분석해, C# WinForms 프로젝트로 재구성하는 과정에서 **기본 UI, 설정 관리, 데이터 파싱, 검증, 로그, 파일 불러오기, Selenium 브라우저 연결 골격**까지 구현한 상태입니다.
