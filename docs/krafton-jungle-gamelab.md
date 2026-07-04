# 서보성 | 크래프톤 정글 게임랩 지원 포트폴리오

> 2017년 C 콘솔 텍스트 RPG에서 시작해 8년째 게임을 만들고 있습니다.
> 라이브 서비스 실무 2년 3개월(클라이언트+서버), 게임잼 우승, 팀 리드 완주 경험과
> 클라·서버·툴·기획을 아우르는 제작 폭을 프로젝트로 정리했습니다.

📧 ssbss0329@gmail.com · 🐙 [github.com/Bo-sung](https://github.com/Bo-sung) · [종합 포트폴리오](../README.md)

---

## 한눈에 보기

| # | 프로젝트 | 무엇을 | 역할 | 핵심 포인트 |
| --- | --- | --- | --- | --- |
| 1 | **Creavas** (팀 4인, 무박 2일) | 빙벽 등반 생존 게임 | 게임 시스템 | 🏆 게임잼 우승, 프로토타입 후 방향 피봇, 유일한 UE 팀 |
| 2 | **Titan Slayer** (팀 4인) | FPS 보스전 | **프로젝트 리드** | 보스 AI 상태 머신, 일정·작업 관리, 기한 내 완성 |
| 3 | **폴리글롯** (팀 5인, 캡스톤) | AR 실시간 번역기 | 개발 | XR 2개 플랫폼 동시 지원, SDK 추상화+폴백 설계 |
| - | **실무** (Funigloo, 2년 3개월) | 글로벌 라이브 서비스 | 클라 + 서버 + 후반 PM | 출시·운영·긴급 대응 전 사이클 경험 |
| 4 | **BattleShip** (개인, 완성) | 3단 분산 게임 서버 | 단독 | 커스텀 TCP 프로토콜, 세션 프로세스 동적 spawn |
| 5 | **WPF 마리오** (개인) | 물리·게임루프 직접 구현 | 단독 | AABB·스윕 충돌, 15ms 게임 루프 — 엔진 없는 기본기 |
| 6 | **AI 파이프라인 툴 연작** (개인) | 개발 생산성 도구 5종 | 단독 | 3D→픽셀아트, 3D→ControlNet, HTML→UGUI, 에이전트 관리 |

---

## 1. Creavas — 빙벽 등반 생존 게임 🏆 게임잼 우승 (팀 4인)

| | |
| --- | --- |
| 기간 | 2025.06.28 ~ 29 (무박 2일 교내 게임잼) |
| 역할 | 게임 시스템 개발 |
| 기술 | Unreal Engine 5.6, Blueprint — **참가팀 중 유일한 언리얼 팀** |
| 성과 | **우승** |

"얼음" 주제. 빙벽을 오르며 체온(체력 100)이 소진되기 전에 목표 높이에 도달하는 생존 게임.

- **결정적 피봇**: 초기 기획은 단순 플랫포머 → 빠른 프로토타이핑 결과 "너무 단순" 판단 → **'Getting Over It' 방식 발사형 이동으로 전환**하며 게임성이 획기적으로 개선, 우승으로 이어짐
- 담당 구현: 아이템 시스템(핫팩 — 체온 손실 방지 / 피켈 — 붙잡기 포인트 생성), 체온 시스템, 난이도 구성
- **잼 이후 확장**: 확장성을 고려한 데이터 테이블 + 엑셀 기반 레벨 에디터를 추가 구현 — 기획자가 직접 맵을 만들 수 있는 구조로 발전

## 2. Titan Slayer — FPS 보스 배틀 (팀 4인, 프로젝트 리드)

| | |
| --- | --- |
| 기간 | 2025.09 ~ 2025.12 |
| 역할 | **프로젝트 리드** (저학년 다수 팀) |
| 기술 | Unity, C# |
| 저장소 | [BasicGameEngine_4_teamP](https://github.com/Bo-sung/BasicGameEngine_4_teamP) |

- 리드 역할: 전체 아키텍처 설계·모듈 정의, 마일스톤·일정 조율, 팀원 역량별 작업 분배, Unity/코딩 가이드, 코드 리뷰
- 직접 구현: FPS 캐릭터 제어, 무기(발사/재장전/조준), **상태 머신 기반 보스 AI**, 전투 시스템(히트/데미지/체력), HUD, 오브젝트 풀링 최적화
- 일정 지연 시 직접 구현으로 리스크를 흡수하며 **기한 내 완성**

## 3. 폴리글롯 — AR 실시간 번역기 (팀 5인, 캡스톤)

| | |
| --- | --- |
| 역할 | 개발 |
| 기술 | Unity, Meta Quest 3 + Google Cardboard |
| 외부 서비스 | Papago, Google ML Kit, ReturnZero, Google Cloud STT |

외국어 수업 소통 문제를 AR 자막 번역으로 해결하는 졸업 프로젝트. XR 2개 플랫폼(고급형 Quest 3 / 저가형 Cardboard) 동시 지원.

- **인터페이스 기반 SDK 추상화 + 폴백**: Papago(메인) 실패 시 ML Kit(온디바이스)로 자동 전환, 한국어 STT는 ReturnZero·다국어는 Google STT 분기
- 2개 플랫폼 동시 개발 과정에서 공통 모듈 경계 문제를 겪고 구조를 조정 — 팀 아키텍처 합의의 중요성을 배운 프로젝트

## 실무 — Funigloo 라이브 서비스 (2022.01~2024.04, 2년 3개월, 산업기능요원)

- 글로벌/동남아 라이브 서비스: 유지보수, 신규 콘텐츠·이벤트 출시, 앱·에셋번들 빌드/배포, 화웨이 마켓 신규 런칭(결제 연동부터)
- 기획서→프로토타입→테스트→배포→CS 버그 리포트→재현·수정→긴급 패치의 전체 사이클 반복 경험
- **후반기 PM 업무 병행**: 시즌 이벤트·상품 패키지 설계, 신규 콘텐츠, NFT 펫 리뉴얼
- 서버 파트 8개월 병행: 재화/배틀패스/원정대 탐사 시스템, GM 툴, Java+MySQL+Redis+Rocky Linux
- **3인 소규모 프로젝트**(기획1·아트1·개발1): 아동 타겟 벽돌깨기 — SDK 의존부 추상화 후 NHN GAMEBASE·IronSource 교체 담당
- 긴급 대응 사례: 유저 간 거래 취약점 버그를 밤샘 분석으로 수정, 익일 긴급점검 배포
- 기획자용 엑셀→테이블 자동 추출 툴 제작

## 4. BattleShip — 3단 분산 게임 서버 (개인, 완성)

| | |
| --- | --- |
| 기술 | C#/.NET 10, MySQL, Redis, 순수 TcpListener |
| 저장소 | [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server) |

- HTTP 없이 **6바이트 length-prefix 바이너리 TCP 프로토콜** 직접 설계
- Auth(MySQL + Redis TTL 30초 1회용 토큰) → Lobby(방 매칭) → **GameSession 독립 프로세스 동적 spawn**(포트 풀 7010~7200)
- Watchdog 좀비 프로세스 정리, 결과 ACK 3회 재시도, 게임 룰 JSON 외부화, SSH 배포 스크립트 — 인증부터 배포까지 인프라 전 과정 단독 구현

## 5. WPF 마리오 — 물리·게임 루프 직접 구현 (개인, 2021)

| | |
| --- | --- |
| 기술 | C#, WPF(.NET Framework) — 엔진·물리 라이브러리 미사용 |

- **AABB 충돌 감지**, 스윕(sweep) 영역 검사로 **터널링(총알 관통) 해결**, 겹침 위치 보정, 원점 간 벡터로 충돌 방향 판정
- 15ms 타이머 게임 루프 — 유니티 프레임 흐름을 참고한 Physics()→Update() 순서
- [스크린샷](../assets/mario_1.png) · [충돌 다이어그램](../assets/mario_collision_diagram.png) · [클래스 다이어그램](../assets/mario_class_diagram.png)

## 6. AI 파이프라인 툴 연작 (개인, 2026)

유료 구독으로 AI 활용법을 탐구하다, 필요한 도구를 직접 만드는 단계까지 온 연작입니다.

| 툴 | 기능 |
| --- | --- |
| [SpriteForge](https://github.com/Bo-sung/SpriteForge) | 리깅된 3D 모델(FBX/GLB) → **픽셀아트 스프라이트 시트** 자동 변환 (OpenGL 렌더링 → 팔레트 양자화 → 시트 패킹, CLI+GUI) |
| [ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter) | 3D 애니메이션 → **ControlNet 패스**(OpenPose/depth/normal/canny) — 실제 스켈레톤 기반이라 프레임 간 흔들림 없는 AI 영상 생성 |
| [html_to_ugui](https://github.com/Bo-sung/html_to_ugui) | AI로 뽑은 HTML/CSS UI 시안 → **Unity UGUI 프리팹** 자동 변환, 반복 구조 재사용 프리팹화 |
| [AgentManager](https://github.com/Bo-sung/AgentManager) | 여러 AI 코딩 에이전트를 **git worktree 격리로 병렬 관리**하는 WPF 플랫폼 — 로컬 LLM 번역 레이어, Diff 리뷰, 워커 위임 |
| [ai-workspace-template](https://github.com/Bo-sung/ai-workspace-template) | 다중 AI 워커 협업 시의 **역할·권한 거버넌스** 템플릿 |

팀 환경에서는 그대로 생산성 도구가 됩니다 — 아트 리소스가 부족한 팀에서 AI 파이프라인으로 비주얼 공백을 메우고 반복 작업을 자동화할 수 있습니다.

---

## 성장 연대기

```
2017  C 텍스트 RPG            — 프로그래밍 입문작
2018  러닝게임                 — Unity 입문
2019  맵툴                     — 첫 외부 개발 도구 (WinForms)
2020  Skul 모작                — Git 버전관리 도입
2021  WPF 마리오               — 물리엔진 바닥부터 구현
2022  Funigloo 입사            — 라이브 서비스 실무 (산업기능요원)
2024  퇴사, 복학               — C# 서버 독학 전환
2025  게임잼 우승, 캡스톤, 팀 리드
2026  AI 파이프라인 툴 연작     — 개발 생산성 도구로 확장
```

- 고등학생 때 마인크래프트 서버 운영을 위해 리눅스 독학 → 현재 실물 서버(Dell PowerEdge R300, CentOS 7) 자가 운영
- 모작 연작으로 역분석 훈련: [Skul: The Hero Slayer](https://github.com/Bo-sung/skullike-project)(2020), [Tricky Towers](https://github.com/Bo-sung/tricky-towers)(2019), [CardFlip](https://github.com/Bo-sung/CardFlip)(2026, 초단기 완성 연습)
- 서버 학습 연작: [ASPAuthServer](https://github.com/Bo-sung/ASPAuthServer)(인증 서버+GM툴) → [authLobbyGame](https://github.com/Bo-sung/GameServerP_Personal)(3-Tier 분산 설계, 설계 문서 9종)
