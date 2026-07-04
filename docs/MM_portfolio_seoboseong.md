# MM_서보성 | 게임 프레임워크 프로그래머 포트폴리오

> 마비노기 모바일 게임 프레임워크 프로그래머 포지션 지원용 포트폴리오입니다.
> 아래의 모든 코드는 본인이 직접 작성했으며, 각 항목에 **어느 파일의 어떤 설계 결정을 봐주시면 되는지**를 함께 적었습니다.

📧 ssbss0329@gmail.com · 🐙 [github.com/Bo-sung](https://github.com/Bo-sung) · [종합 포트폴리오](../README.md)

---

## 개발 철학 — 예측 가능한 코드, 예측 가능한 작업

**1. 규칙을 문서로 먼저 세우고, 코드가 그 규칙을 따르게 합니다.**
프로젝트마다 코딩 스타일 가이드와 설계 문서를 먼저 작성합니다. 팀원(또는 미래의 나)이 코드를 열었을 때 놀라지 않는 것이 좋은 코드의 첫 조건이라고 생각합니다.

**2. 공유되는 것은 한 곳에만 존재해야 합니다.**
클라이언트와 서버가 프로토콜·데이터 구조를 각자 들고 있으면 반드시 어긋납니다. 공용 라이브러리(CommonLib)를 단일 원천으로 두고 양쪽이 참조하는 구조를 기본으로 합니다.

**3. 반복되는 작업은 도구로 바꿉니다.**
실무에서 기획자용 엑셀→게임 테이블 자동 추출 툴을 만들어 콘텐츠 추가 프로세스를 줄였고, 개인 작업에서는 AI 기반 에셋 생성 파이프라인(3D→픽셀아트, FBX→ControlNet)을 직접 구축했습니다. 사람이 실수할 수 있는 자리는 도구가 대신하게 만듭니다.

---

## 1. 대표 코드 — InterplanetaryOnline 서버 (팀 4인 RTS, 서버 메인 담당)

**저장소: [InterPlanetery_Server](https://github.com/Bo-sung/InterPlanetery_Server)** · C#, 순수 소켓, MySQL · 2025.09~12

실시간 멀티플레이어 RTS의 서버 전체와 클라이언트 네트워크 계층을 설계·구현했습니다.
개발 철학이 가장 잘 드러난 코드로, 아래 순서로 봐주시면 좋습니다.

### 이 코드에서 봐주시면 좋은 것들

| 파일 | 봐주실 포인트 |
| --- | --- |
| [`Docs/Guides/CodingStyle.md`](https://github.com/Bo-sung/InterPlanetery_Server/blob/master/Docs/Guides/CodingStyle.md) | 팀 프로젝트 시작 시 **코딩 스타일 가이드부터 작성** — 블록 스타일, 네이밍, 커밋 전 코드 정리 규칙 |
| [`Servers/CommonLib/Protocol.cs`](https://github.com/Bo-sung/InterPlanetery_Server/blob/master/Servers/CommonLib/Protocol.cs) | 프로토콜의 단일 원천. `AddParam` 플루언트 체이닝, `StateCode` 열거형으로 응답 상태 표준화 — 클라·서버가 같은 파일을 참조해 스펙 불일치를 구조적으로 차단 |
| [`Servers/CommonLib/Commands/`](https://github.com/Bo-sung/InterPlanetery_Server/blob/master/Servers/CommonLib/Commands/Command.cs) | 락스텝의 입력 단위를 **Command 패턴**으로 추상화 — `MoveFleetCommand`, `ProduceFleetCommand`가 직렬화 가능한 형태로 클라·서버를 오감 |
| [`Servers/BaseServer/Core/Game/Entities/Game.cs`](https://github.com/Bo-sung/InterPlanetery_Server/blob/master/Servers/BaseServer/Core/Game/Entities/Game.cs) | **락스텝 동기화의 심장** — 50ms 고정 틱(20TPS), 커맨드 버퍼링(`COMMAND_BUFFER_TICKS`)으로 네트워크 지연 흡수, 처리된 틱 추적으로 중복 실행 방지 |
| [`Servers/BaseServer/Core/Game/Session/ClientSession.cs`](https://github.com/Bo-sung/InterPlanetery_Server/blob/master/Servers/BaseServer/Core/Game/Session/ClientSession.cs) | 세션 수명 관리와 하트비트 기반 연결 모니터링 |
| [`Docs/`](https://github.com/Bo-sung/InterPlanetery_Server/tree/master/Docs) | 시스템 스펙, 아키텍처 다이어그램 3종, 프로토콜 비교 검토 문서, 클라이언트 구현 가이드 — **요구사항을 글로 정리해 개발에 반영하는 방식** 그대로 |

### 설계 배경 (요구사항 → 결정)

- **요구**: RTS 특성상 유닛 수가 많아 상태 전체 동기화는 대역폭이 감당 불가 → **결정**: 입력(Command)만 교환하는 락스텝, 모든 클라이언트가 같은 틱에 같은 명령을 실행해 상태 일치 보장
- **요구**: 네트워크 지연이 제각각인 클라이언트들의 명령을 같은 틱에 정렬 → **결정**: 수신 명령을 미래 틱(현재+버퍼)에 예약하는 입력 버퍼링
- **요구**: 팀원들이 네트워크를 몰라도 게임 로직을 만들 수 있어야 함 → **결정**: 클라이언트에 네트워크 매니저·핸들러를 만들어 제공, 게임 로직과 통신 계층 분리
- 성과: 최대 동시 접속 8명 테스트에서 상태 불일치 없이 동작

---

## 2. 보조 코드 — BattleShip 분산 서버 (개인, 완성)

**저장소: [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server)** · C#/.NET 10, MySQL, Redis

인증-매칭-게임세션-정산-배포까지 게임 서버 인프라 한 사이클을 단독으로 설계·구현했습니다.

- **6바이트 length-prefix 바이너리 TCP 프로토콜** 직접 설계: PacketId 대역 분리(1xxx 인증/2xxx 로비/3xxx 게임/9xxx 공통), `C_`/`S_`/`SS_` 패킷 명명 규칙 — 이름만 봐도 방향을 알 수 있게
- Auth(MySQL + Redis TTL 30초 1회용 토큰) → Lobby(방 매칭) → **GameSession 독립 프로세스 동적 spawn**(포트 풀 관리) — 세션 장애가 다른 게임에 전파되지 않는 격리 구조
- Watchdog(무응답 프로세스 자동 정리), 결과 전송 ACK 3회 재시도, 게임 룰 JSON 외부화(코드 수정 없는 룰 변경), SSH 배포 스크립트
- 5개 프로젝트가 공유하는 `BattleShip.Common` 프로토콜 라이브러리 — Unity 클라이언트에는 DLL로 배포 (철학 2의 다른 구현)

## 3. 시스템·자동화 이력

- **실무 (Funigloo, 2년 3개월, 클라이언트+서버)**: 기획자용 **엑셀→게임 테이블 자동 추출 툴** 제작, 기존 이벤트 시스템 테이블화로 콘텐츠 추가 파이프라인 개선, 앱·에셋번들 빌드/배포(Jenkins 사용), 3개 마켓(구글·애플·화웨이) 출시·SDK 세팅, 유저 간 거래 취약점 버그 밤샘 분석 후 익일 긴급점검 배포
- **AI 파이프라인 도구 (개인, 공개)**: [SpriteForge](https://github.com/Bo-sung/SpriteForge)(리깅된 3D 모델→픽셀아트 스프라이트 시트, OpenGL 오프스크린 렌더링), [ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter)(FBX 애니메이션→ControlNet 패스, 스켈레톤 기반 시간 안정성), [html_to_ugui](https://github.com/Bo-sung/html_to_ugui)(HTML 시안→Unity UGUI 프리팹), [AgentManager](https://github.com/Bo-sung/AgentManager)(AI 코딩 에이전트 git worktree 격리 병렬 관리)
- **문서화 이력**: 3-Tier 서버 설계 문서 9종 선행 작성([GameServerP_Personal/authLobbyGame](https://github.com/Bo-sung/GameServerP_Personal)), 실무 시절부터 마크다운 기반 분석·기획 문서 작성 습관

## 4. 기반 이해

- **엔진 밑단**: 엔진 없이 WPF로 충돌 처리(AABB, 스윕 기반 터널링 해결, 겹침 보정)와 게임 루프(고정 틱, Physics→Update 순서)를 직접 구현해 본 경험 — 프레임워크가 해주는 일의 원리를 바닥부터 이해
- **네트워크**: TCP/UDP 소켓, 커스텀 바이너리 프로토콜, 락스텝/하트비트/ACK 재시도 패턴 구현
- **플랫폼 폭**: Unity(실무)·UE5(학생 주도 게임잼 우승작)·WPF/WinForms 툴·ComfyUI(Python 노드)·Android ML Kit 온디바이스 연동 — 낯선 환경에 빠르게 진입해 결과물을 내는 패턴의 반복
