# 서보성 | 게임 프로그래머 포트폴리오

> Unity 클라이언트 2년 3개월 + 서버 개발 실무 경험을 보유한 게임 프로그래머입니다.
> 라이브 서비스 운영 경험을 바탕으로 클라이언트-서버 전반을 아우르는 개발을 지향하며,
> C# 기반 서버(ASP.NET Core, 순수 소켓)와 Unreal Engine까지 영역을 넓혀가고 있습니다.
> **새 기술이 등장하면 직접 검증해 보고 쓸 만하면 실전에 투입하는 것을 즐깁니다** — VR, 메타버스, NFT, AI까지.

|          |                                          |
| -------- | ---------------------------------------- |
| 📧 Email  | ssbss0329@gmail.com                      |
| 🐙 GitHub | [github.com/Bo-sung](https://github.com/Bo-sung) |

### 📌 포지션별 상세 포트폴리오

- **[게임 서버 프로그래머](docs/server-programmer.md)** — 분산 서버, 락스텝 동기화, 인증/세션 설계 중심
- **[게임 클라이언트 프로그래머](docs/client-programmer.md)** — 라이브 서비스 실무, UE5/Unity, 게임잼 우승작 중심
- **[크래프톤 정글 게임랩 지원용](docs/krafton-jungle-gamelab.md)** — 8년 성장 서사와 완주 이력 중심

---

## 🛠 기술 스택

| 구분 | 기술 |
| --- | --- |
| **언어** | C# (주 언어), Java, C++ |
| **게임 엔진** | Unity (2년 실무), Unreal Engine 5.6 |
| **서버 / DB** | ASP.NET Core, C# 소켓 서버, Java 백엔드(실무) · MySQL, Redis |
| **네트워크** | TCP/UDP 소켓 프로그래밍, 커스텀 바이너리 프로토콜 |
| **인프라** | Rocky Linux(실무), CentOS 7, Git 서버 자체 구축 — 고교 시절부터 자가 서버 운영 (Dell PowerEdge R300 → 현재는 Mac mini 2대로 홈서버 운영 중) |
| **플랫폼** | Google Play, App Store, 화웨이 앱갤러리 출시/운영 |

---

## 🔭 신기술 검증 사이클

새 패러다임이 등장하면 **"일단 만들어보고 판단한다"**를 반복해 왔습니다. 검증에서 끝나지 않고, 필요하면 도구까지 만듭니다.

| 시기 | 기술 | 검증 방식 |
| --- | --- | --- |
| VR/AR | Meta Quest 3, Google Cardboard | VR/AR 팀 프로젝트 → 캡스톤 "폴리글롯"(AR 실시간 번역기)으로 2개 XR 플랫폼 동시 개발 |
| 메타버스 | Unreal Engine | 메타버스 컨셉 프로토타입 제작 시도 |
| NFT | 블록체인 연동 | 실무에서 NFT 연동 게임의 라이브 서비스 유지보수 담당 |
| AI | LLM, 이미지 생성 | 주요 AI 서비스를 종류별로 유료 구독해 비교하고, **로컬 LLM용 장비(NVIDIA DGX Spark)까지 도입**해 **아트 리소스 제작에 직접 적용·연구** → 체감한 한계를 메우는 **부산물로 툴 제작**: [SpriteForge](https://github.com/Bo-sung/SpriteForge)(3D→픽셀아트), [ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter)(3D→ControlNet), [html_to_ugui](https://github.com/Bo-sung/html_to_ugui)(디자인→UGUI), [AgentManager](https://github.com/Bo-sung/AgentManager)(멀티 에이전트 관리) |

---

## 💼 경력

### Funigloo | 게임 프로그래머 (2022.01 ~ 2024.04, 2년 3개월, 산업기능요원)

- **클라이언트** (2년 3개월): 글로벌/동남아 라이브 서비스 운영, 앱·에셋번들 빌드/배포, 신규 콘텐츠 및 이벤트 개발, 이벤트 시스템 테이블화, 화웨이 결제 연동, 기획자용 엑셀→테이블 자동 추출 툴 개발
- **PM 업무 병행** (후반기): 시즌 이벤트·상품 패키지 설계, 신규 콘텐츠 개발, NFT 펫 리뉴얼 등 담당
- **서버** (8개월 병행): 신규 프로젝트 오픈 전후 서버 개발/운영, 재화·배틀패스·원정대 탐사 시스템 구현, GM 툴 유지보수 — Java + MySQL + Redis + Rocky Linux
- 3인 소규모 프로젝트에서 아동 타겟 게임의 SDK 의존부 추상화·교체(NHN GAMEBASE, IronSource) 담당

---

## 🚀 대표 프로젝트

### 1. InterplanetaryOnline — 멀티플레이어 RTS (서버 메인)

| | |
| --- | --- |
| 기간 | 2025.09 ~ 2025.12 |
| 역할 | **서버 전체 개발** + 클라이언트 네트워크 계층 |
| 팀 | 4명 |
| 기술 | C#, Unity, MySQL, TCP/UDP |
| 저장소 | [InterPlanetery_Server](https://github.com/Bo-sung/InterPlanetery_Server) |

실시간 멀티플레이어 전략 시뮬레이션. 인증(로그인/세션), 방 관리, **락스텝(lockstep) 결정론적 동기화**, 하트비트 연결 모니터링을 직접 설계·구현했습니다. 입력 버퍼링과 지연 보상으로 동기화를 안정화했고, 팀원들이 네트워크 세부사항을 몰라도 쓸 수 있도록 네트워크 매니저 API를 추상화해 제공했습니다. (테스트 시 최대 동시 접속 8명)

### 2. BattleShip — 커스텀 프로토콜 분산 게임 서버

| | |
| --- | --- |
| 역할 | 개인 프로젝트 |
| 기술 | C#/.NET 10, MySQL, Redis, 순수 TcpListener |
| 저장소 | [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server) · [BattleShip_Client](https://github.com/Bo-sung/BattleShip_Client) |

HTTP 없이 **6바이트 length-prefix 바이너리 TCP 프로토콜**을 직접 설계한 3단 분산 서버입니다. Auth(MySQL 로그인 + Redis TTL 30초 1회용 토큰) → Lobby(방 매칭) → **GameSession 독립 프로세스 동적 spawn**(포트 풀 7010~7200) 구조로, Watchdog 좀비 프로세스 정리, 결과 전송 ACK 재시도, SSH 배포 스크립트까지 게임 서버 인프라 전반을 구현했습니다.

### 3. ASPAuthServer — ASP.NET Core 인증 서버 + WPF GM 툴

| | |
| --- | --- |
| 역할 | 개인 프로젝트 |
| 기술 | ASP.NET Core 8.0, MySQL, Redis, WPF(MVVM) |
| 저장소 | [ASPAuthServer](https://github.com/Bo-sung/ASPAuthServer) |

RESTful 설계 원칙을 준수한 인증 서버(회원가입/로그인/세션, 로그인 실패 잠금 정책, Repository·Options·DI 패턴)와, 이를 관리하는 WPF GM 툴(Access/Refresh 토큰 자동 갱신, 사용자 관리, 실시간 로그 뷰어)을 함께 구현했습니다. 설계 문서를 먼저 쓰고 구현하는 프로세스로 진행했습니다.

### 4. Creavas — 빙벽 등반 생존 게임 🏆 교내 게임잼 우승

| | |
| --- | --- |
| 기간 | 2025.06.28 ~ 29 (1박 2일) |
| 역할 | 게임 시스템 개발 (데이터 테이블, 맵툴, 아이템) |
| 기술 | Unreal Engine 5.6, Blueprint |

"얼음" 주제 게임잼에서 점프킹을 모티브로 한 발사형 이동(결과물은 Getting Over It에 가까운 감각)의 빙벽 등반 생존 게임으로 우승했습니다. 스테이지별 난이도·아이템 데이터 테이블화, 빠른 스테이지 제작용 맵툴, 아이템(핫팩/피켈)·체온 시스템을 담당 — 1박 2일 안에 콘텐츠를 빠르게 찍어낼 수 있는 파이프라인을 만드는 데 집중했습니다.

### 5. AI 개발 도구 시리즈 (2026)

프로그래머로서 부족한 **아트 리소스를 AI로 보완**하려고 직접 시도·연구하는 과정에서, 현재 AI가 아트 리소스 제작에서 갖는 한계들을 체감했고 — 그 **한계를 하나씩 메우는 과정의 부산물**로 만들어진 툴들입니다.

- 스프라이트 애니메이션의 프레임 일관성 부족 → **SpriteForge** (3D 모델을 렌더링해 변환하므로 일관성 보장)
- 생성 영상·포즈의 프레임 간 흔들림 → **ComfyUI-FBX-ControlNet-Converter** (실제 스켈레톤 기반 컨디셔닝)
- AI가 뽑은 UI 시안이 엔진 리소스로 이어지지 않음 → **html_to_ugui** (시안을 UGUI 프리팹으로 직접 변환)

- **[AgentManager](https://github.com/Bo-sung/AgentManager)** — 여러 AI 코딩 에이전트(Claude Code, Codex 등)를 git worktree로 격리해 병렬 관리하는 .NET 10 WPF 플랫폼. 로컬 LLM 번역 레이어로 클라우드 토큰 절감
- **[SpriteForge](https://github.com/Bo-sung/SpriteForge)** — 리깅된 3D 모델(FBX/GLB)을 픽셀아트 스프라이트 시트로 변환하는 파이프라인 (OpenGL 오프스크린 렌더링 → 팔레트 양자화 → 시트 패킹)
- **[html_to_ugui](https://github.com/Bo-sung/html_to_ugui)** — HTML/CSS 디자인을 헤드리스 Chromium으로 렌더링해 Unity UGUI 프리팹으로 변환, 반복 구조는 자동으로 재사용 프리팹화
- **[ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter)** — 3D 애니메이션을 ComfyUI 안에서 ControlNet 패스(OpenPose/depth/normal 등)로 변환하는 커스텀 노드. C#/OpenGL 네이티브 렌더러 내장

---

## 📂 기타 프로젝트

| 분류 | 프로젝트 | 설명 |
| --- | --- | --- |
| 서버 학습 | [GameServerP_Personal](https://github.com/Bo-sung/GameServerP_Personal) | 소켓 틱택토 → 3-Tier 분산 아키텍처(Auth/Lobby/Game + ENet UDP)로 이어지는 학습 모노레포 |
| 모작 | [skullike-project](https://github.com/Bo-sung/skullike-project) | "Skul: The Hero Slayer" 모작 — 스컬 투척/순간이동/돌진 3종 액션 |
| 모작 | [tricky-towers](https://github.com/Bo-sung/tricky-towers) | 물리 퍼즐 "Tricky Towers" 모작 — 낙하 중 트리거→착지 시 물리 전환 |
| 러너 | [Project_Runner](https://github.com/Bo-sung/Project_Runner) | 2D 무한 러너 — Factory 패턴, 오브젝트 재활용 맵 생성 리팩터링 |
| 연출 데모 | [CardFlip](https://github.com/Bo-sung/CardFlip) | 트윈 연출 시연용 초단기 소품 — MVP 패턴 + iTween 플립/패널 연출 |
| 툴 | [Tools/MapEditor](https://github.com/Bo-sung/Tools) | WinForms 2D 타일맵 에디터 |
| 기획 문서 | [FantasyDeffence_Docs](https://github.com/Bo-sung/FantasyDeffence_Docs) | 서브컬처 디펜스 RPG 기획서 (문서 등급제 관리) |
| 기획 문서 | [ZDeffencePlan](https://github.com/Bo-sung/ZDeffencePlan) | 포스트 아포칼립스 "폐교 생존 게임" 기획 + 개발 로드맵 |
| 기획 문서 | [BBF 분석](https://github.com/Bo-sung/BBF_-) | 실무 프로젝트 SDK/에셋 교체 분석 문서 — AI 도구 이전부터의 마크다운 문서화 습관 |
| 물리 구현 | WPF 마리오 (2021) | WPF로 충돌 처리·게임 루프를 직접 구현 — AABB, 스윕 충돌(터널링 해결), 15ms 게임 루프 ([스크린샷](assets/mario_1.png) · [클래스 다이어그램](assets/mario_class_diagram.png)) |

> 비공개(NCloud SourceCommit) 프로젝트: Unity VR/AR 프로젝트 **VRAR_Final**, Papago/ML Kit 폴백 구조의 AR 번역 앱 **MRTranslator("폴리글롯")** — 공개 준비 중

---

## 🎓 학력

**계명대학교** — 컴퓨터공학과(주전공) · 게임소프트웨어공학과(부전공), 2026.02 졸업 예정
