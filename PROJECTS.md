# Bo-sung 포트폴리오 프로젝트 소개

> GitHub(`github.com/Bo-sung`) 저장소 전체(54개)를 조회한 뒤, 저장소 용량(size)·커밋 이력을 기준으로
> **어느 정도 진행도가 있는 프로젝트만** 남겼습니다. 거의 빈 저장소(0~수 KB, 뼈대만 있는 수준)는 전부 제외했습니다.
> 설명(description)이 비어 있는 저장소는 이름 기반으로 추정했으니 `[TODO]` 표시된 부분은 실제 내용에 맞게 보완해 주세요.
> 기존 이력서(`bluearchive_portfolio_boseong-1.pdf`, 블루아카이브 서버 프로그래머 지원용)를 참고해 일부 항목의 상세 내용을 보강했습니다.

---

## 경력 요약 (이력서 기준)

**Funigloo | 게임 프로그래머** (2022.01 ~ 2024.04, 2년 3개월)
- 클라이언트 개발 2년 3개월 — **라그나로크 라비린스 NFT** (동시접속 5~600명 규모 라이브 서비스): 글로벌/동남아 서비스 운영, 신규 콘텐츠 3종·채팅 필터 시스템 개발, 앱·에셋 번들 빌드/배포, NFT 연동 시스템 유지보수, 이벤트 시스템 테이블화, 화웨이 결제 연동, 기획자용 엑셀→테이블 자동 추출 툴 개발
- 서버 개발 8개월(병행) — **라그나로크 20Heroes**: 신규 프로젝트 오픈 전후 서버 개발/운영, 재화·배틀패스·원정대 시스템 구현, GM 툴 개발·유지보수, DB 쿼리 최적화, Java 백엔드 + MySQL + Redis + Rocky Linux 운영
- ※ 프로젝트명·동접 규모 출처: `5469640_서보성_자기소개서_marp.md` (수업 과제로 작성한 Marp 슬라이드 자소서) — 공개 문서에 실명 기재 가능한지 확인 필요

**기술 스택**: C#(주 언어)·Java·C++ / Unity(2년 실무)·Unreal Engine 5.6 / ASP.NET Core·C# 소켓 서버·Java 백엔드 / MySQL·Redis / TCP·UDP 소켓 프로그래밍 / Git·GitHub·Naver Cloud SourceCommit·자체 구축 Git 서버

---

## 1. 게임 클라이언트/서버 프로젝트

### BattleShip (클라이언트 · 서버)
- [BattleShip_Client](https://github.com/Bo-sung/BattleShip_Client) (298KB, C#) — Unity 클라이언트. 서버 프로젝트(`BattleShip.Common`)를 `dotnet build`로 빌드해 나온 DLL을 `Assets/Plugins/`에 넣어 공유하는 구조. 네트워킹/UI/게임로직 스크립트로 폴더 분리
- [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server) (144KB, C#) — **.NET 10 기반 3단 분산 게임 서버** (코드 분석으로 확인)
  - **커스텀 바이너리 TCP 프로토콜**: 6바이트 length-prefix 헤더(Length/PacketId/Sequence), PacketId 대역 분리(1xxx 인증/2xxx 로비/3xxx 게임/9xxx 공통), `C_`/`S_`/`SS_` 패킷 명명 규칙
  - **Auth(:7001) → Lobby(:7002) → GameSession(동적 포트 7010~7200)** 구조: AuthServer가 MySQL 로그인 후 Redis에 TTL 30초 1회용 토큰 발급 → LobbyServer가 검증·방 매칭 → 양측 Ready 시 **게임 세션을 독립 프로세스로 동적 spawn**
  - 세션은 Lobby로 역접속해 게임 룰(JSON 외부화)을 받아 턴제 배틀십 진행, 결과는 ACK 3회 재시도와 함께 Lobby로 전달해 MySQL 전적 저장
  - Watchdog(무응답 30초 시 프로세스 정리), CLI 테스트 클라이언트, paramiko 기반 SSH 배포 스크립트 포함 — 게임 서버 인프라 전반을 직접 구현

### InterPlanetery_Server — 이력서의 "InterplanetaryOnline" (멀티플레이어 RTS)
- [InterPlanetery_Server](https://github.com/Bo-sung/InterPlanetery_Server) (646KB, C#)
- 개발 기간 2025.09.29~2025.12.13, 팀 4명(조별 과제), 실시간 멀티플레이어 전략 시뮬레이션
- **역할**: 서버 전체 개발(메인 담당) + 클라이언트 네트워크 매니저/핸들러 구현
- **서버**: 로그인/회원가입/세션 인증, 빈방 자동 관리, 락스텝(lockstep) 동기화, 하트비트 기반 연결 모니터링, MySQL 유저·테이블 데이터 관리, TCP/UDP 프로토콜 설계
- **클라이언트**: 네트워크 매니저·패킷 핸들러 구현, 다른 팀원이 활용할 수 있도록 API 추상화
- **기술적 도전**: 입력 버퍼링/지연 보상으로 락스텝 동기화 안정화, 하트비트로 연결 끊김 자동 감지·재연결
- **성과**: 최대 동시 접속 8명 테스트, 완벽한 게임 상태 동기화

### ASPAuthServer
- [ASPAuthServer](https://github.com/Bo-sung/ASPAuthServer) (128KB, C#) — ASP.NET Core 8.0 기반 RESTful 인증 서버 + WPF GM 툴. 퇴사 후 학업과 병행해 C# 서버 개발 역량을 강화하기 위해 직접 설계·구현한 개인 프로젝트
- **AuthServer**: 회원가입/로그인/로그아웃, MySQL + Redis 세션 관리, 로그인 실패 3회 시 15분 계정 잠금, Repository·Options·DI 패턴 적용, RESTful 설계 원칙(리소스 중심 URL, `201 Created` + `Location` 헤더 등) 준수, DB 자동 초기화
- **GMTool**: AuthServer 관리용 WPF 데스크톱 앱. MVVM(CommunityToolkit.Mvvm) 구조, 관리자 로그인 → Access/Refresh 토큰 자동 갱신(`DelegatingHandler`로 401 감지 시 재발급), 사용자 목록/상세/잠금·해제/비밀번호 초기화/세션 강제종료, 대시보드 통계, **하단 고정 실시간 로그 뷰어**(API 호출·에러·사용자 액션 추적, 레벨 필터링) 자체 구현
- 설계 문서(AUTH_SERVER_DESIGN.md, API_DOCUMENTATION.md, DESIGN_DOCUMENT.md, LOG_SYSTEM_DESIGN.md, PERFORMANCE_GUIDE.md 등) 다수 포함 — 설계 단계부터 문서화하는 습관이 드러남

### GameServerP_Personal — 개인 C# 서버 학습 워크스페이스
- [GameServerP_Personal](https://github.com/Bo-sung/GameServerP_Personal) (450KB, C#) — 하나의 저장소에 난이도가 다른 여러 서버 프로젝트를 담은 학습용 모노레포
- **authLobbyGame** (가장 규모가 큰 하위 프로젝트): **인증 서버(ASP.NET Core) + 로비 서버 + 게임 서버**로 분리한 3-Tier 분산 게임 서버 아키텍처
  - TCP(중요 이벤트·채팅) + UDP(ENet-CSharp, 실시간 동기화) 하이브리드 프로토콜, MySQL(영구 저장) + Redis(세션·매칭큐·리더보드 등) 이중 DB
  - 게임 장르에 종속되지 않는 범용 설계(방/매칭/통계를 JSON으로 커스터마이즈), 비활성 방·세션 자동 정리, 서버 하트비트 기반 장애 대응
  - 문서 9종(ARCHITECTURE, NETWORK_PROTOCOL, DATABASE_SCHEMA, REDIS_DATA_STRUCTURES, SERIALIZATION_SPEC, SYSTEM_DIAGRAMS 등)과 함께 실제 구현 진행 중: LobbyServer의 Room/RoomManager/ClientSession, CommonLib의 Redis 래퍼(세션·캐시·PubSub·RateLimiter)·Protocol·Command 패턴(`MoveFleetCommand`, `ProduceFleetCommand` — 함대 이동 기반 RTS 커맨드로 InterplanetaryOnline과 유사한 설계)
  - ASPAuthServer의 초기 버전도 이 워크스페이스 안에 포함되어 있음(현재는 별도 저장소로 독립)
- **CommonServer**: TicTacToe를 소재로 LoginServer/GameServer를 분리해보는 학습용 프로젝트. 공통 패킷 매니저(`PacketManager`)로 서버 간 프로토콜 공유 구조 연습
- **TickTacToePVP**: 순수 소켓 기반 틱택토 PvP 서버/클라이언트 — 소켓 프로그래밍 기초를 다진 초기 프로토타입
- 전체적으로 "간단한 소켓 서버 → 패킷/프로토콜 분리 → 3-Tier 분산 아키텍처"로 이어지는 **학습 진행 과정**을 보여주는 저장소

---

## 2. 모작(클론) 프로젝트

- [Project_Runner](https://github.com/Bo-sung/Project_Runner) (약 42MB, C#) — "BoxRunner", Unity 2018.3 기반 2D 플랫포머 무한 러너 (코드 분석으로 확인)
  - 구 포트폴리오(2017-2021) 기준 **2018년, 유니티를 배우고 처음 만든 게임**. "플레이어가 화면 밖으로 밀려나면 사망 — 위치가 곧 체력"이라는 규칙. 개발 중 작업물 70%를 유실했다가 에셋스토어 리소스로 복구한 경험도 기록되어 있음
  - 스크린샷: [assets/runner_1.png](assets/runner_1.png) 외 3장
  - 자동 스크롤 맵에서 스페이스바 점프로 구멍·적 회피, Gun/Sword 아이템 획득 시 상체 애니메이터 교체(상·하체 분리 애니메이션)
  - 학습 의도가 뚜렷한 구조: `Player` 베이스 클래스를 `User`/`Enemy`가 상속, Container·Factory 패턴으로 맵/아이템/몬스터 분리, `Dictionary<KeyCode, Action>` 델리게이트 입력 테이블
  - 커밋 이력에서 맵 생성을 파괴-재생성 방식 → **오브젝트 재활용 + 숫자 패턴 문자열 기반 지형 생성**으로 리팩터링한 과정 확인 가능 (Grass/Sand/Cave 테마 전환 포함)
- [skullike-project](https://github.com/Bo-sung/skullike-project) (약 48MB) — **"Skul: The Hero Slayer" 모작** (2020). 플레이어 캐릭터 "LittleBone"이 머리(스컬)를 던지고, 던진 머리 위치로 순간이동하고, 회전 돌진 공격을 하는 3가지 스컬 액션이 핵심 메커니즘. 완/진/미(완료/진행중/미구현) 체계로 시스템별 진행 상황을 README에서 직접 트래킹(입력 처리, 맵 전환, HUD/인벤토리/게임오버 UI, 나무형 몬스터 Ent 계열 일부 구현·병사형 몬스터 미구현 등)
  - 구 포트폴리오 기준 **GitHub 버전 관리와 Rider IDE를 처음 도입한 프로젝트** — 기존 게임을 역분석하며 유니티를 깊게 파려는 목적. 미완성임을 스스로 명시
  - 스크린샷: [assets/skul_clone_1.png](assets/skul_clone_1.png) 외 2장 (+ 원작 비교용 [skul_original_ref.png](assets/skul_original_ref.png))
- [tricky-towers](https://github.com/Bo-sung/tricky-towers) (4.8MB, C#) — WeirdBeard의 물리 퍼즐 게임 "Tricky Towers" 모작, Unity 2017.4 프로토타입 (코드 분석으로 확인)
  - 핵심 메커니즘: 테트로미노가 **낙하 중에는 트리거+무중력으로 키보드 조작**을 받다가, 착지 순간 `Rigidbody2D` 중력·실제 콜라이더로 전환되어 물리적으로 기울고 무너지는 탑 쌓기
  - 7-bag 셔플 블록 큐, 다음 블록 프리뷰, A/D 이동·Q/E 회전·소프트드롭/투하 조작 구현
- [CardFlip](https://github.com/Bo-sung/CardFlip) (2.3MB, C#) — **트윈(tween) 연출 역량을 보여주기 위한 초단기 포트폴리오 소품** (Unity 6/URP, 2026.02)
  - 카드 짝 맞추기(8쌍 16장, 시도 20회)를 소재로 iTween 연출에 집중: 2단계 카드 플립(90도 회전 후 앞뒷면 교체), 결과 패널 순차 등장(easeOutBack 스케일), 애니메이션 중 입력 차단 처리
  - 짧은 제작 기간에도 로직은 MVP 패턴으로 분리: MonoBehaviour에 의존하지 않는 순수 C# `CardFlipPresenter`가 매칭 판정·상태 관리를 담당하고 이벤트로 View에 통보

---

## 3. AI / 개발 도구 프로젝트 (최근 작업, 2026년 상반기)

### [AgentManager](https://github.com/Bo-sung/AgentManager) (4.4MB, C#, 가장 최근 활동 2026-07-03)
여러 AI 코딩 에이전트를 하나의 창에서 관리하는 **.NET 10 WPF 데스크톱 플랫폼**.
- **멀티 엔진 관리**: Claude Code, Codex, Antigravity, Pi 네 가지 CLI 에이전트를 "단발 실행 + resume" 방식으로 구동하고, 세션마다 독립된 **git worktree**로 격리
- **번역 레이어**: 로컬 LLM(Ollama, 기본 exaone3.5:7.8b)이 한국어 입력을 영어로 번역해 클라우드에 보내고, 응답을 다시 한글로 되돌려 표시 — 토큰 소비 절감 목적
- **리뷰 페인**: 파일 변경사항을 Git Diff로 검토 후 Merge / Commit / Discard 선택
- **워커 위임**: 대형 모델이 소형 모델 워커에게 잔작업을 위임하는 반자동 흐름으로 비용 절감
- **권한 관리**: 엔진별 권한 모드를 위험도별 색상으로 표시, 도구 권한 요청을 가로채 승인/거부
- 헤드리스 Core를 분리해 CLI 프런트엔드와 로직 공유, 설정은 VS Code 스타일 `settings.json`

### [SpriteForge](https://github.com/Bo-sung/SpriteForge) (207KB, C#)
리깅된 3D 캐릭터 모델(FBX/GLB)을 **픽셀아트 스프라이트 시트로 변환**하는 다단계 파이프라인 툴.
- CLI(`spriteforge.exe`, `pixelart.exe`)와 라이브 프리뷰 WPF GUI(`SpriteForge.Gui`)가 하나의 라이브러리 공유
- 파이프라인: 오프스크린 고해상도 렌더링 → 모폴로지 정리 → 대표색 다운스케일 → 알파 이진화 → 엣지 팽창 → (선택)디더링 → 팔레트 양자화 → 재기 정리 → (선택)윤곽선
- 2/4/8방향 렌더링, JSON 매니페스트 기반 장비 부착, 본 이름 기반 크로스 스켈레톤 리타겟팅, Wu 색상 양자화 + Bayer/Floyd-Steinberg 디더링
- .NET 8 + Silk.NET(OpenGL/Assimp) + SkiaSharp, 픽셀아트 알고리즘은 unfake.js 기법을 응용

### [html_to_ugui](https://github.com/Bo-sung/html_to_ugui) (272KB, C#)
렌더링된 HTML/CSS/JS 디자인을 **Unity UGUI 프리팹**(스프라이트·폰트·색상·재사용 가능한 중첩 프리팹 포함)으로 변환하는 툴. "HTML→C# 컴파일러가 아니다"라고 명시할 만큼 UI 목업 변환에 특화.
- 2단계 구조: .NET 8 CLI가 헤드리스 Chromium으로 페이지를 렌더링·리소스 추출 → Unity 에디터 임포터가 중간 표현을 프리팹으로 변환
- 시맨틱 HTML 요소(div/span/heading/image/button), 절대 위치 RectTransform, border-radius, 인라인/외부 SVG 아이콘, CJK 지원 구글 폰트, 반복 구조 자동 감지 후 재사용 컴포넌트 프리팹화
- 원클릭 Unity 통합 / CLI 단독 실행 / CI 파이프라인 연동 세 가지 워크플로 지원
- 한계: box-shadow, gradient, filter, transform, 애니메이션 미지원 (HUD 목업 변환이 목적, 픽셀 퍼펙트 웹 재현 아님)

### [ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter) (197KB, C#)
리깅된 3D 애니메이션(FBX/GLB)을 ComfyUI 안에서 바로 **ControlNet 컨디셔닝 패스**(OpenPose, depth, normal, alpha, canny)로 변환하는 커스텀 노드.
- 내장된 네이티브 **C#/OpenGL CLI**가 리깅 평가·스키닝·렌더링을 수행 — Python 3D 스택이나 전처리기 추측 없이 실제 스켈레톤/메시 기반이라 기하학적으로 정확하고 시간적으로 안정적
- Mixamo 워크플로 지원(스킨 있는 모델 + 애니메이션 전용 클립을 본 이름으로 리타겟팅), 프레임 보간을 통한 자유로운 fps 리샘플링
- 자체 완결형(.NET 런타임 불필요) win-x64 바이너리를 Git LFS로 배포, Assimp/Silk.NET/SixLabors.ImageSharp 등 서드파티 라이브러리 사용 명시
- MIT 라이선스, ComfyUI-Yedp-Action-Director에서 아이디어만 착안(코드 재사용 없음)

### [ai-workspace-template](https://github.com/Bo-sung/ai-workspace-template) (925KB) — AI 에이전트 오케스트레이션 거버넌스 템플릿
전통적인 소프트웨어 프로젝트가 아니라, **여러 AI 에이전트가 하나의 저장소에서 협업할 때의 역할·권한 규칙**을 정의한 템플릿(`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.agents/`, `.shared-ai-tools/`)
- 데스크톱 AI CLI 세션을 **오케스트레이터**로 지정 — 작업 계획, 작업 분해, 입력 범위 선정, 워커 결과 검토, 최종 반영, Git 조작을 전담
- **로컬 LLM 워커**(Gemma/Exaone/Qwen): 문서 요약, 계획 정리, 초안 작성만 담당하며 소스코드 수정·아키텍처 결정 금지
- **경량 OpenAI 워커**(gpt-4-nano): 문법 검사·정규화만 수행, 최종 판단·코드 추론 제외
- 모든 워커는 단일 목적/토큰 제한(1,500~4,000)/읽기 전용 Git 권한 하에서만 동작 — Git 쓰기(commit/push/merge/rebase)는 워커에게 금지되고 오케스트레이터만 수행
- AgentManager(위 항목)와 사상적으로 맞닿아 있는, 멀티 에이전트 협업 설계에 대한 실험/선행 작업으로 보임

---

## 4. 기획 / 문서 프로젝트

- [FantasyDeffence_Docs](https://github.com/Bo-sung/FantasyDeffence_Docs) (955KB, HTML) — **서브컬처 디펜스 RPG** 기획 문서. 좌우 라인 기반 공격/방어 포지셔닝 전투, 영웅 덱(소환 유닛) 카드 시스템, "영지" 거점 건설, 스테이지 클리어·캐릭터 수집·거점 육성 세 축의 콘텐츠 루프. 문서를 정식/재검토 필요/초안/백로그로 등급화해 관리 — 전투·보스 AI·수익화는 재검토 단계, 장비·영지 세부·기술 아키텍처는 초안 단계
- [ZDeffencePlan](https://github.com/Bo-sung/ZDeffencePlan) (623KB, HTML) — **"폐교 생존 게임"**. (이름과 달리 좀비가 아니라) 차원 균열로 문명이 붕괴한 포스트 아포칼립스 세계관의 쿼터뷰 서바이벌. 폐교를 거점으로 생존자 7명을 관리하며 적 웨이브를 방어, 사망 캐릭터는 완전 소실, Football Manager식 8개 능력치 시스템, 차원 균열을 통한 이세계 캐릭터 소환. Unreal Engine 5.6(Blueprint 85% / C++ 15%) 기반으로 기획 완료 → 프로토타입(4~6주) → 알파(8~12주) → 베타(8~10주) 로드맵까지 수립, Git LFS + Discord/Trello 협업 체계 명시
- [BBF_-](https://github.com/Bo-sung/BBF_-) (약 117MB) — **"Block Breaker Friends(BBF)" 분석 문서**. 기존 SDK/에셋을 교체해 보유 IP("미니특공대" 등)를 접목한 신작을 만들기 위한 실무 분석 자료로, 기술 인프라(SDK 교체 여부), 용어/구조 정리, 수익화 전략(광고 배치, 상품 구성), 맵툴 분석, 진행 논의사항까지 섹션별로 문서화. AI 도구 도입 이전부터 마크다운 기반 문서화를 실천해온 근거 자료

---

## 5. 도구 / 유틸리티

### [Tools](https://github.com/Bo-sung/Tools) (493KB, C#)
- **MapEditor** (2019): C# WinForms 기반 2D 타일맵 에디터. 64x64 타일 단위로 던전형 타일셋(흙/돌바닥/용암/얼음/벌집/늪/무덤 등 200개 이상 타일 이미지)을 배치해 맵을 제작
  - `Form1`(에디터 UI) / `Map`(맵 데이터) / `ObjectFactories`(타일 오브젝트 생성) / `File_IO`(맵 저장·불러오기)로 구성
  - **제작 목적(구 포트폴리오에서 확인)**: "유니티 등 다른 게임 개발 시 도움이 될 맵 데이터 제작용 외부 툴을 만들자"에서 출발 — 파일 불러오기, 필드 크기 변경, 블록 리스트 선택, **결과물 TXT 출력** 기능
  - 스크린샷: [assets/maptool_main_1.png](assets/maptool_main_1.png), [maptool_wide.png](assets/maptool_wide.png) 외

---

## 초기 프로젝트 (2017~2021, 구 포트폴리오 발굴)

`G:\HDD_Backup\잡다한 추억\포트폴리오 1.pdf`(2017–2021 포트폴리오)에서 확인한 성장 과정 프로젝트들. 스크린샷은 `assets/`에 추출해 두었습니다.
연혁: 2017 텍스트게임 → 2018 러닝게임 → 2019 맵툴 → 2020 Skul 모작 → 2021 찌리리공 뒤집기 → 2021 WPF 마리오

### TextGame (2017, C) — 저장소 미확인
- C언어를 배우고 처음 만든 콘솔 텍스트 RPG. 콘솔 명령어와 구조체를 활용해 OOP 개념을 흉내낸 첫 작품
- 스크린샷: [assets/textgame_main.png](assets/textgame_main.png), [textgame_battle.png](assets/textgame_battle.png) 외

### 찌리리공 뒤집기 (2021, WPF) — 저장소 미확인
- 포켓몬 하트골드/소울실버의 금빛시티 미니게임(Voltorb Flip) 모작. **제작 3일** — 이후 보드 크기를 유연하게 조정 가능하도록 개편
- 스크린샷: [assets/voltorb_1.png](assets/voltorb_1.png) 외

### WPF 마리오 (2021, WPF/.NET Framework) — 저장소 미확인, 기술적으로 주목할 초기작
- WPF를 그래픽 엔진 삼아 **충돌 처리와 게임 루프를 직접 구현**한 물리 엔진 학습 프로젝트 (1-1 스테이지 구현)
- 구 포트폴리오에 구현 방식이 상세히 기술되어 있음:
  - AABB(Axis Aligned Bounding Box) 충돌 처리
  - **총알 관통(터널링) 문제 해결**: 현재 위치→이동 예상 위치의 스윕 영역을 만들어 교차 검사 후 표면 위치로 보정
  - 겹침 발생 시 겹친 영역의 너비/높이를 비교해 작은 축으로 밀어내는 위치 보정
  - 두 객체 원점 간 방향 벡터로 충돌 방향 감지
  - 15ms 타이머 기반 게임 루프 — 유니티 프레임 흐름을 참고해 Physics() → Update() 순서로 실행
  - 클래스 다이어그램 작성: [assets/mario_class_diagram.png](assets/mario_class_diagram.png)
- 스크린샷: [assets/mario_1.png](assets/mario_1.png) 외 4장, 충돌 다이어그램 [mario_collision_diagram.png](assets/mario_collision_diagram.png)
- ※ 물리/게임루프를 바닥부터 구현한 경험이라 "엔진에 의존하지 않는 기본기" 증빙으로 활용 가치 높음 — GitHub 업로드 권장

### 서버 인프라 자습 이력 (구 포트폴리오 자기소개에서 확인)
- 고등학교 시절부터 마인크래프트 서버를 구형 노트북으로 직접 구축·운영
- 이후 **Dell PowerEdge R300 실물 서버를 구비해 CentOS 7 리눅스 서버 운영**, AWS 등 클라우드 사용 경험 병행
- ※ 2026-07 기준: R300은 현재 미사용, **Mac mini 2대를 홈서버로 운영 중** (사용자 확인)
- GitHub의 `mcBackupTest1`, `Minecraft_Economy_CSharp` 저장소가 이 활동의 흔적 — 서버 프로그래머 지향의 뿌리가 학창 시절 인프라 자습에 있음을 보여주는 서사

---

## 6. 학교 수업 / 팀 프로젝트

- [ComputerGraphics1](https://github.com/Bo-sung/ComputerGraphics1) (13.6MB, C++) — 컴퓨터그래픽스 1학기 수업 실습 파일 백업용
- [ComputerGraphics2](https://github.com/Bo-sung/ComputerGraphics2) (약 26MB, C++) — 2025 2학기 컴퓨터그래픽스2 수업 실습 파일 백업용
- **스테레오 비전 기반 깊이맵 생성 알고리즘 비교 연구** (차량비전시스템 수업, 저장소 없음 — 로컬 문서)
  - Block Matching / SGBM / SGBM+WLS Filter 3종을 OpenCV(Colab)로 직접 구현·비교: Middlebury 표준 데이터셋 + 합성 데이터셋으로 정성·정량 평가, SGBM이 BM 대비 약 30% 정확도 향상 확인, 처리 시간·깊이 레벨 수 등 메트릭 분석
  - **전자공학회 논문지 양식으로 작성** (요약/Abstract/서론/방법론/실험/결론/참고문헌 10편) — 기술 문서 작성 역량 증빙 자료
  - 동일 내용을 마크다운(`depthmap_report_template.md`)으로도 관리 — 마크다운 문서화 습관의 또 다른 근거
  - 파일 위치: `G:\HDD_Backup\P_대학교 자료\비 프로그래밍\스테레오 비전 기반 깊이맵 생성 알고리즘 비교 연구*.pdf`
- [BasicGameEngine_4_teamP](https://github.com/Bo-sung/BasicGameEngine_4_teamP) (약 461MB, C#) — 이력서의 "**Titan Slayer**"(FPS 보스 배틀 게임)로 추정 · [시연 영상](https://youtu.be/FhHfEcHxENo)
  - 개발 기간 2025.09.29~2025.12.13, 팀 4명(저학년 다수 포함), Unity/C#
  - **역할**: 프로젝트 리드 — 시스템 아키텍처 설계, 일정 관리, 저학년 팀원 기술 지원, 작업 분배, 코드 리뷰, 일정 지연 시 직접 구현
  - **구현**: FPS 캐릭터 제어, 무기(발사/재장전/조준) 시스템, 상태 머신 기반 보스 AI, 히트/데미지/체력 전투 시스템, HUD, 오브젝트 풀링 최적화

---

## 7. NCloud SourceCommit 저장소 (사내/개인, 비공개)

GitHub이 아닌 Naver Cloud SourceCommit에 있던 저장소로, `G:\testbackup`에 클론하여 진행도(커밋 수·파일 수)를 확인했습니다.
GitHub에는 없는 저장소이므로, 포트폴리오에 포함하려면 공개 가능한 형태로 별도 저장소를 만들어 이전하는 작업이 필요합니다.

### MechTarkov (PMechTArkov_Origin + DMechTarkov + 로컬 백업 다수) — ⚠️ 포폴 제외 (사용자 결정: 1인 프로젝트 아님, 결과물 미완) · [시연 영상](https://youtu.be/sDrpmL388Ag)
- PMechTArkov_Origin — UE5 기반 게임 프로젝트 (8,512개 파일, 약 60GB). 2040년대 한국(경상북도·대구) 배경, 포스트 아포칼립스 세력 갈등을 다루는 **루터 슈터/익스트랙션 슈터**(Escape from Tarkov 계열) + 메카 컨셉. Lyra 프레임워크 기반으로 캐릭터·맵·에셋 다수 포함
- DMechTarkov — 위 프로젝트의 기획 문서(Obsidian 볼트). 장르 분석, 세계관, 프로젝트 규칙 등 상세 문서화 (49개 파일, 6커밋)
- **`G:\HDD_Backup\Git`에서 발견된 개발 이력** (2023년부터 이어진 장기 프로젝트임을 확인):
  - `mechtarkov_client` — **119커밋**(~2024.09), Lyra 애님 테스트·Project_MT·GAS 실험 등 실질적 개발 이력의 본체
  - `MechTarkov`/`MechTarkov11` — 2023년 초기 버전들 (3~4커밋)
  - `PmechTarkov_CPP` — **소스커밋에서 클론 실패했던 저장소의 로컬 백업(18GB, 14커밋, ~2025.03)**. 내용은 GASFPS(UE Gameplay Ability System 기반 FPS) — C++ 재구현 버전
- 2023 → 2024(클라이언트 집중) → 2025(C++/GAS 재구현) → PMechTArkov_Origin(에셋 통합)으로 이어지는 **2년 이상의 개인 대표 프로젝트**
- [TODO] 실제 플레이 가능한 빌드인지 확인, 여러 저장소 중 포폴에 내세울 대표 버전 선정

### VRAR_Final (SourceCommit 버전)
- Unity VR/AR 프로젝트, 2,806개 파일, 50커밋 — GitHub의 동명 저장소(`VRAR_Final`/`VRAR_Final2`)는 빈 껍데기였지만, 이 버전이 실제 완성본
- [TODO] VR/AR 기능·컨셉 설명 추가

### MRTranslator — 캡스톤 프로젝트 "폴리글롯" (증강현실 실시간 번역기)
- **캡스톤 6조(5인 팀: 서보성 외 4명) 졸업 프로젝트**, 1,362개 파일, 60커밋 (캡스톤 중간보고 pptx로 확인)
- **기획 배경**: 외국인 학생이 한국어 수업을, 한국 학생이 외국인 교수의 영어 수업을 따라가기 어려운 문제를 AR 실시간 번역으로 해결. 미래 AR 기기 대중화를 내다보고 현재 상용 기기로 구현
- **타깃 플랫폼 2종 동시 개발**: Meta Quest 3 (VR+MR) / Google Cardboard (저가형 휴대폰 VR)
- **인터페이스 기반 SDK 추상화 + 메인/폴백 구조**: `ITranslatorService`, `IAudioStreamWebService` 인터페이스로 외부 SDK를 추상화
  - 번역: **네이버 Papago API**(메인, 자연스러운 번역 품질로 채택) → 응답 실패 시 **Google ML Kit**(온디바이스, 폴백)으로 자동 전환하는 로직이 `TranslatorManager`에 구현되어 있음
  - 음성인식(STT): 한국어는 인식률이 가장 좋은 **Vito.ai ReturnZero**, 그 외 언어는 **Google Cloud Speech-to-Text**(WebSocket 스트리밍)로 분기하는 구조 설계
- **개발 중 겪은 문제(중간보고 기록)**: 2개 플랫폼 동시 개발 시 공통 모듈 경계 설정의 어려움, Cardboard SDK가 2020년 이후 업데이트 중단된 제약
- 관련 자료: `캡스톤6조 중간보고_수정.pptx` (G:\HDD_Backup\P_대학교 자료\비 프로그래밍\)

### Dangling (develop 브랜치) — 이력서의 "Creavas" (빙벽 등반 생존 게임, 🏆 교내 게임잼 우승작)
- Unreal Engine 프로젝트 `Dangling_Climb`, 687개 파일, 38커밋, 마지막 커밋 2025-06-29
- ※ `master` 브랜치는 빈 껍데기(커밋 1개)였고, 실제 내용은 `develop` 브랜치에 있음
- 개발 기간 2025.06.28~29 (1박 2일 게임잼), Unreal Engine 5.6 + Blueprint, **Dangling 교내 게임잼 우승**
- **컨셉**: "얼음"을 주제로 빙벽을 등반하며 체온(체력)이 소진되기 전 목표 높이에 도달하는 발사형 이동 생존 게임
- **결정적 피봇(사용자 정정)**: 초기 기획은 빙벽 구간에서 시점 전환되는 러너류 → 기획 구체화 단계에서 "너무 단순" 판단 → 핵심 키워드(빙벽 등반)만 남기고 점프킹 모티브 발사형 이동으로 과감히 전환(결과물은 Getting Over It에 가까움), 우승. 참가팀 중 유일한 언리얼 사용 팀
- **담당 역할(사용자 정정)**: 발사형 이동 조작, 맵툴 제작·적용을 포함해 **애니메이션 블루프린트 제외 구현 전반**
  - 아이템 시스템: 핫팩(체온 손실 방지), 피켈(붙잡기 포인트 생성)
  - 체온 시스템: 시간 경과에 따라 체력 100에서 점진적 감소
  - 데이터 테이블 + 엑셀 기반 레벨 에디터도 **잼 도중 구현**(사용자 확인) — 확장성을 고려한 구조로 스테이지를 빠르게 제작
- ※ 생성자 계정이 `dangling1`인 것은 팀 저장소를 공동 사용했기 때문으로 보임 (본인은 develop 브랜치에서 시스템 개발 담당)

---

## 이력서에는 있지만 저장소를 찾지 못한 프로젝트

이력서에 소개됐지만 온라인 저장소가 없던 프로젝트들의 **로컬 실물을 `G:\HDD_Backup`에서 찾았습니다.**

- **FogDepole** (좀비 서바이벌 FPS) — 2021년 2학기(약 4개월), Unreal Engine 4.26 + Blueprint, 2인 팀(개발 1명, 아트 1명), 본인 프로그래밍 전담. 좀비 AI, FPS 무기 시스템, 웨이브 기반 스테이지, 전투 메커니즘 구현
  - ✅ **실물 발견**: `G:\HDD_Backup\Unreal\FogDepole_Origin` (UE 프로젝트 전체 + README). 포폴 포함하려면 GitHub 업로드 필요
- **부위 탈락 시스템(PDSystem)** — 포폴에서는 제외하기로 했으나(사용자 결정), 실물은 `G:\HDD_Backup\Git\MechFPS_CPP\PDSystem`(PartDetachSystem + docs)에 존재. 소스커밋의 MechFPS_CPP는 빈 껍데기였지만 **로컬 백업은 16GB, 15커밋(~2025.09)의 실제 프로젝트**였음 — 재포함 여부는 추후 판단
- **Funigloo 실무 툴 실물**: `G:\HDD_Backup\Git\ExcelDataExporter`(FuniglooExportData) — 이력서의 "기획자용 엑셀→테이블 자동 추출 툴". 회사 코드라 공개는 불가능할 것으로 보이며 위치만 기록

---

## 제외한 저장소

- **진행도 낮음/뼈대 수준:** `DungeonDraft_Client`, `LOLFM`, `llm_wiki_code`, `SignanotaFanGameDocs`, `SignanotaFanGamePrj`
- **미완성으로 제외:** `FrontierBastion_client`, `FrontierBastion_battlecore`(+ 이미 빈 저장소였던 `FrontierBastion_server`) — 아직 완성되지 않은 프로젝트
- **제거 요청:** `Minecraft_Economy_CSharp`, `VRARProgramming_team1`
- **의미 없어진 개인용/참고용:** `receiptToxlsx`(의미 없어짐), `aspNet_samples`(개인 참고용), `funigloo-pw`(회사 업무 임시 보관용)
- **완전히 비어있음(0KB):** `FrontierBastion_server`, `Client`, `Dangling250630`, `VRAR_Final`, `kmu_Graphics_1`, `test`, `RandomDefence`, `Bo-sung`(프로필 설정), `-`
- **뼈대 수준(수 KB):** `AdvancedCopmuterGraphics`(1KB), `VRAR_Final2`(1KB), `TopViewShooterTemplate`(2KB), `my_ldea_wiki`(3KB), `GitFlowExample`(5KB), `ObsidianAI`(9KB), `Calculator_OOP`(15KB), `LeagueOfLegendCopy`(20KB), `quota-watch`(33KB)
- **테스트/백업 성격:** `mcBackupTest1`, `SimpleWebDocs`, `FlightShooterTemplate`
- **NCloud SourceCommit, 진행도 없음(커밋 1개·README뿐):** `MRTranslator_Docs`, `MechFPS_CPP`(단, 로컬 백업 `G:\HDD_Backup\Git\MechFPS_CPP`에는 16GB 실물 존재 — 부위탈락시스템 포함)
- **NCloud SourceCommit, 접근 실패:** `PMechTArkov_cpp` — 클론은 실패했으나 로컬 백업 `G:\HDD_Backup\Git\PmechTarkov_CPP`(18GB, 14커밋)로 내용 확인 완료

---

## 새로 작성해 푸시한 README (2026-07-04 완료)

README가 없거나 부실했던 4개 저장소를 코드 분석 후 한국어 README로 재작성해 GitHub에 푸시했습니다.

- [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server) — 3단 분산 서버 아키텍처, 프로토콜 명세, 실행 방법 포함 (※ 비밀번호 평문 비교 상태임을 정직하게 명시)
- [Project_Runner](https://github.com/Bo-sung/Project_Runner) — 구현 시스템·패턴·커밋 이력 요약 포함
- [tricky-towers](https://github.com/Bo-sung/tricky-towers) — 물리 전환 메커니즘 중심, 미완성 부분(UI 골격 등)도 명시
- [CardFlip](https://github.com/Bo-sung/CardFlip) — 트윈 연출 시연용 초단기 포폴 프레이밍, MVP 구조·게임 규칙 포함

---

## 참고한 대학 자료 (G:\HDD_Backup\P_대학교 자료\비 프로그래밍)

기존 포폴 항목의 추가 설명용으로 분석한 본인 작성 산출물:

- **캡스톤6조 중간보고_수정.pptx** → MRTranslator가 캡스톤 "폴리글롯"임을 확인, 기획 배경·플랫폼·문제점 반영 완료
- **스테레오 비전 깊이맵 비교 연구 (논문 양식 pdf + md)** → 학교 프로젝트 섹션에 신규 반영 완료
- **5469640_서보성_자기소개서_marp.md** → 실무 프로젝트명(라그나로크 라비린스 NFT / 20Heroes)과 동접 규모를 경력 요약에 반영 완료. Marp(마크다운 슬라이드) 활용 자체도 문서화 역량 증거
- **사용자 제공 자소서(2025)** → 추가 확보 정보: 산업기능요원 복무, 후반기 PM 업무(시즌 이벤트·상품 패키지·NFT 펫 리뉴얼), 밤샘 거래버그 픽스 에피소드, 3인 벽돌깨기 프로젝트(5~12세 타겟, SDK 추상화 — `G:\HDD_Backup\Git\BBF_Client`/`MFB_Client` 백업과 연결 추정), 원정대는 "탐사" 시스템, Creavas는 플랫포머→점프킹 모티브 발사형 이동 피봇(결과물은 Getting Over It에 가까움) 후 우승 + 테이블/엑셀 레벨 에디터도 잼 도중 구현(사용자 확인), 게임 철학(문명5·흥미로운 선택) — 각 문서에 반영 완료
- **알고리즘 과제 보고서 시리즈** (ChainedMatrix/Dijkstra/Floyd/Huffman/Kruskal/EditDistance 등, hwp+md) — C++ 구현 + 보고서. 알고리즘 기초 증빙용으로 보관만 (포폴 미기재)
- **포트폴리오 1.pdf** (`G:\HDD_Backup\잡다한 추억\`, 2017–2021 구 포트폴리오) → "초기 프로젝트" 섹션 신설, 프로젝트 스크린샷 29장을 `assets/`로 추출, Project_Runner·skullike·MapEditor 배경 정보 보강, 서버 인프라 자습 이력 확인
- 제외: `Depth Estimation.pptx`(교수 강의자료), 기타 수업 강의자료·행정 서류 일체

---

## 다음 단계 제안

1. 남은 `[TODO]` 항목(GameServerP_Personal 등)에 실제 설명·기술 스택·스크린샷 채우기
2. FogDepole 저장소 위치 확인 후 추가
3. 대표 프로젝트 3~5개(예: InterplanetaryOnline, Creavas, MechTarkov, AgentManager)를 상단에 하이라이트로 강조
