<p align="right"><b>한국어</b> | <a href="README.en.md">English</a></p>
<p align="center">
  <img src="docs/images/logo.png" width="120" alt="Senvas"/>
</p>
<h1 align="center">Senvas HMI</h1>
<p align="center">AI 와 함께 만드는 <b>산업용 HMI</b> 개발 플랫폼 — 설계부터 터치 패널 배포까지</p>
<p align="center">
  <a href="https://github.com/going-kr/Release.Senvas/releases/latest"><b>⬇ Senvas-win-Setup.exe 내려받기</b></a>
  &nbsp;·&nbsp; Windows 10/11 x64 &nbsp;·&nbsp; 한국어 UI
</p>

---

이 저장소는 **설치 파일과 자동 업데이트 산출물만** 배포합니다. 소스 코드는 비공개로 관리됩니다.

> ℹ️ 릴리스 페이지 하단의 **"Source code (zip / tar.gz)"** 는 GitHub 가 모든 릴리스에 자동으로 붙이는 **이 배포 저장소(README·이미지)의 스냅샷**이며, Senvas 소스 코드가 아닙니다.

## Senvas 는 무엇인가

**Senvas** 는 [Going](https://github.com/going-kr) 의 산업용 HMI 개발 도구입니다. 감시·제어 화면을 **`.gudx` 비주얼 디자이너**로 그리고, 통신·로직·데이터는 **C#** 으로 채우며, 결과물은 **Senvas 터치 패널**(linux-arm64)로 한 번의 클릭에 배포됩니다. 가장 큰 특징은 **AI 워크플로우**입니다 — Senvas 가 내장한 MCP 서버에 Claude Code · Codex CLI 같은 AI 코딩 도구가 붙어, 요구사항 인터뷰부터 화면 설계·계획·구현·검증까지 프로젝트 전체를 만들어 내고, 그 뒤에는 같은 도구가 수정 요청을 처리하는 어시스턴트로 동작합니다.

<p align="center"><img src="docs/images/ui.png" width="900" alt="UI 디자이너"/></p>

## 작업 흐름

1. **새 프로젝트** — 화면 크기(예: 1280×800)와 테마를 고르면 `.senvas` 프로젝트와 AI 컨텍스트(`CLAUDE.md` / `AGENTS.md`)가 만들어집니다.
2. **AI 생성** — AI 페이지에서 *클로드 콘솔* 또는 *코덱스 콘솔* 을 열고 만들고 싶은 HMI 를 한 문단으로 설명합니다. AI 는 **인터뷰 → 설계 → 계획 → 구현 → 검증** 순서로 진행하며, 설계와 계획 단계에서는 반드시 멈춰 사용자의 승인을 기다립니다.
3. **편집** — UI 페이지에서 화면을 손보고, Code 페이지에서 C# 을 직접 고칩니다. AI 는 이후에도 어시스턴트로 수정 요청을 처리합니다.
4. **Build / Run** — 코드 생성(MakeCode) → `dotnet build` → Windows 에서 바로 실행해 확인합니다.
5. **Deploy** — 같은 네트워크의 Senvas 터치 패널을 자동 검색해 업로드·설치·시작까지 한 번에 수행합니다.

## 주요 기능

### AI 워크플로우 (MCP)
Senvas 는 **MCP 서버**(`http://localhost:5283`)를 내장하고, 설치된 **Claude Code · Codex CLI** 에 자동 등록합니다. AI 는 도구 호출로 `.gudx` 검증, 코드 생성(MakeCode), 이미지·폰트 리소스 추가, 데이터 저장소 설정, 진행 상태·산출물 등록을 수행합니다. 함께 설치되는 **약 300 편의 지식 문서**(컨트롤·통신·데이터·디자인·코드 패턴)와 워크플로우 문서를 읽고 작업하므로 결과가 Senvas 의 규칙에 맞게 나옵니다. 검증 단계에서는 화면을 PNG 로 렌더링해 **AI 가 직접 눈으로 보고** 레이아웃 문제를 찾아 고칩니다(시각 검증).

**AI 페이지**는 현재 모드·단계·상태, 인터뷰 체크리스트, 설계 문서·구현 계획 같은 산출물을 한눈에 보여 줍니다. Senvas 안에 채팅창은 없습니다 — 대화는 익숙한 AI CLI 에서 하고, Senvas 는 진행 상황과 결과를 보여 줍니다.

<p align="center"><img src="docs/images/ai.png" width="900" alt="AI 진행 상태"/></p>
<p align="center"><img src="docs/images/mcp-clients.png" width="445" alt="AI 클라이언트 자동 등록"/></p>

### UI 디자이너 — `.gudx` 화면 설계
도구상자의 컨테이너·컨트롤·이미지 캔버스·플로우 시스템 부품을 캔버스에 배치하고 속성 창에서 다듬습니다. 탐색기에서 Master·Pages·Windows 를 오가고, 테마 편집기로 색상 체계를, 리소스 패널로 이미지·폰트를 관리합니다. 탱크·펌프·밸브·배관 같은 **FlowSystem** 부품은 파이프 연결 도구로 이어 P&ID 풍 공정 화면을 만들 수 있고, 게이지·미터·트렌드/시간 그래프·막대/원형 그래프·램프·스위치·데이터그리드 등 HMI 에 필요한 컨트롤이 기본으로 들어 있습니다.

### 코드 — 생성 코드와 사용자 코드의 분리
빌드 때마다 `.gudx` 에서 `*.Designer.cs` · `design.json` · `GlobalUsings.cs` 를 다시 생성하고(MakeCode), 통신·데이터·로직은 `DeviceManager` · `DataManager` · `LogicManager` 의 사용자 partial 파일에 씁니다. 생성 파일은 덮어쓰지만 사용자 코드는 건드리지 않습니다. Code 페이지의 편집기에서 바로 고치거나 **IDE 로 열기**로 외부 IDE 를 쓸 수 있습니다. 생성 프로젝트는 .NET 8 기반 OpenTK·SkiaSharp 앱(Going.UI)이며 Windows 와 linux-arm64 에서 같은 코드로 동작합니다.

<p align="center"><img src="docs/images/code.png" width="900" alt="코드 편집"/></p>

### 통신
Modbus RTU/TCP(마스터·슬레이브), MQTT, LS 산전 CNet, 미쓰비시 MC 프로토콜 라이브러리와 사용 패턴 문서가 함께 제공되어 AI 또는 사용자가 `DeviceManager` 에 바로 작성합니다. Senvas 터치 패널의 시리얼 포트(RS-232 1ch · RS-485 3ch) 배정 규칙도 지식 문서에 포함되어 인터뷰 단계에서 자동으로 반영됩니다.

### 데이터 저장 (선택)
이력·설정 같은 데이터를 남겨야 하면 MCP 도구가 **EF Core + SQLite** 를 프로젝트에 붙이고(패키지 추가·`dotnet-ef` 로컬 도구 고정) 마이그레이션까지 생성합니다. 여러 `DbContext` 도 지원합니다.

### 빌드 · 실행 · 배포
- **Build**: 저장 → MakeCode → `dotnet build`. 로그가 창에 실시간으로 흐릅니다.
- **Run**: 빌드 결과를 Windows 에서 바로 실행해 화면을 확인합니다.
- **Deploy**: `dotnet publish`(linux-arm64) → 압축 → 검색된 **Senvas 터치 패널**로 업로드·설치·시작. 장치는 mDNS 로 자동 검색되며 첫 연결 때 장치 토큰을 한 번 입력합니다. Deploy 페이지에서 장치의 웹 UI 열기, 장치에서 실행/중지도 할 수 있습니다.

<p align="center"><img src="docs/images/run.png" width="700" alt="Windows 에서 실행한 HMI 앱"/></p>

## 구성품

| 구성 | 실행 위치 | 역할 |
|---|---|---|
| **Senvas 에디터** (이 저장소에서 배포) | Windows PC | 프로젝트·UI 디자이너·코드 편집·AI 워크플로우(MCP)·빌드·실행·배포 |
| **Senvas 터치 패널** | 현장 (linux-arm64 패널) | 배포된 HMI 앱을 실행. 에디터와 같은 네트워크에서 mDNS 로 검색 |

패널 쪽 소프트웨어는 이 저장소에서 배포하지 않습니다. 패널 없이도 에디터에서 만들고 Windows 에서 실행해 볼 수 있습니다.

## 요구 사항

| 구분 | 요구 사항 |
|---|---|
| OS | Windows 10/11 x64 (에디터는 self-contained 로 별도 .NET 런타임 설치 불필요) |
| **.NET 8 SDK** | Build · Run · Deploy 가 `dotnet` 을 호출하고 생성 프로젝트가 `net8.0` 을 대상으로 하므로 **직접 설치**해야 합니다 → [.NET 8 SDK 다운로드](https://dotnet.microsoft.com/download/dotnet/8.0). `dotnet` 경로는 **설정 › 빌드**에서 바꿀 수 있습니다 |
| AI CLI | **Claude Code** 또는 **Codex CLI** 중 하나 이상(AI 워크플로우용). 설치돼 있으면 Senvas 가 시작 시 MCP 서버를 자동 등록합니다 |
| Node.js + qmd | AI 의 지식 검색 엔진. 첫 실행 온보딩이 `winget` 으로 Node.js LTS 를, `npm` 으로 `qmd` 를 설치하고 검색 인덱스를 배치합니다(건너뛰기 가능) |
| 배포 대상 | Senvas 터치 패널 + 같은 네트워크(mDNS) + 장치 토큰 |

## 설치 (Windows)

1. [최신 릴리스](https://github.com/going-kr/Release.Senvas/releases/latest) 에서 **`Senvas-win-Setup.exe`** 를 내려받아 실행합니다.
2. 설치기는 자체 서명 인증서로 서명되어 있어 Windows SmartScreen 경고가 뜰 수 있습니다 → **"추가 정보" → "실행"**.
3. 사용자 계정 아래(`%LocalAppData%\Senvas`)에 설치되며 관리자 권한이 필요 없습니다. 실행에 필요한 .NET 런타임이 포함되어 있어 따로 설치할 것이 없습니다.
4. 첫 실행 시 온보딩이 Node.js + qmd(지식 검색) 설치를 안내합니다. 나중에 해도 되지만 AI 워크플로우 품질을 위해 권장합니다.
5. **.NET 8 SDK** 가 없으면 [여기](https://dotnet.microsoft.com/download/dotnet/8.0)서 SDK(x64)를 설치합니다. 없으면 **Build 가 실패**합니다(편집·저장은 가능). 설치 확인: `dotnet --list-sdks`.
6. Claude Code 또는 Codex CLI 를 설치해 두면 **설정 › AI 클라이언트**에 "등록됨"으로 표시됩니다.

## 업데이트

설치된 앱은 시작 시 새 버전을 조용히 확인하고 변경 내용을 보여 준 뒤 적용합니다. 언제든 **도움말 › 업데이트 확인** 으로 직접 확인할 수 있으며(업데이트 적용 시 앱이 재시작되므로 작업을 저장하세요). 이 페이지에서 다시 내려받을 필요가 없습니다.

## 릴리스 파일 안내

| 파일 | 용도 |
|---|---|
| `Senvas-win-Setup.exe` | **처음 설치 시 받는 파일** |
| `Senvas-x.y.z-full.nupkg`, `*-delta.nupkg` | 자동 업데이트 패키지 — 직접 받지 않음 |
| `RELEASES`, `releases.win.json`, `assets.win.json` | 자동 업데이트 메타데이터 |
| (자동) `Source code (zip)`, `Source code (tar.gz)` | GitHub 가 자동으로 붙이는 **이 배포 저장소의 스냅샷**. Senvas 소스 코드가 아니며 받을 필요 없음 |

> `v1.1.x` 태그는 이전 세대 제품 **SenvasHMI** 의 릴리스입니다. 새로 설치하는 경우 최신 `v2.x` 를 받으세요. 두 제품은 별개로 설치되며 서로 업데이트되지 않습니다.

## 문의

버그·요청은 이 저장소의 [Issues](https://github.com/going-kr/Release.Senvas/issues) 에 남겨 주세요.

---

© Going. All rights reserved. 바이너리는 있는 그대로 제공됩니다. [NOTICE.md](NOTICE.md) 참고.
