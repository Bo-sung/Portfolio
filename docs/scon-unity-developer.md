# 서보성 | 유니티 게임 개발자 포트폴리오 (SCON 지원)

> Unity 라이브 서비스 2년 3개월 — 콘텐츠 개발부터 3개 마켓 출시·운영, 긴급 대응까지 상용 서비스의 전 과정을 경험했고,
> 지금은 **AI 기반 에셋·콘텐츠 생성 파이프라인**을 직접 구축하고 있습니다.
> 리깅된 3D 모델과 애니메이션 데이터를 다루는 도구를 만들어 온 만큼, 버추얼 캐릭터 도메인과의 접점이 넓습니다.

📧 ssbss0329@gmail.com · 🐙 [github.com/Bo-sung](https://github.com/Bo-sung) · [종합 포트폴리오](../README.md)

---

## 한눈에 보기

| # | 항목 | 무엇을 | 공고와의 접점 |
| --- | --- | --- | --- |
| 1 | **실무** (Funigloo, 2년 3개월) | Unity 라이브 게임 개발·운영 | 게임 개발·출시 / 콘텐츠 운영 / 라이브 대응 |
| 2 | **AI 파이프라인 툴 연작** (개인, 공개) | 에셋·콘텐츠 생성 파이프라인 5종 | **AI 활용 개발 / 파이프라인 최적화 (우대: AI 기반 에셋 생성 파이프라인 구축)** |
| 3 | **Titan Slayer** (팀 4인, 리드) | Unity FPS 보스전 | Unity 게임 개발, 팀 리드·일정 관리 |
| 4 | **폴리글롯** (팀 5인, 캡스톤) | Unity XR 실시간 번역기 | 외부 SDK 추상화·교체 대응력 |
| 5 | **CardFlip** (개인) | Unity 6 연출 데모 | 최신 Unity(6/URP) 사용, 연출 구현 |
| 6 | **서버 역량** (개인·팀) | C# 게임 서버 2종 | 라이브 운영 이해의 깊이 (서버까지 아는 클라이언트) |

---

## 1. 실무 — Unity 라이브 서비스 (Funigloo, 2022.01~2024.04, 산업기능요원)

- **글로벌/동남아 라이브 모바일 게임** 클라이언트 개발·운영 (퇴사 시까지 지속)
- **상용 출시·운영 경험**: Google Play, App Store, 화웨이 앱갤러리 — 3개 마켓의 등록·심사 대응·업데이트, **마켓별 SDK 세팅**(화웨이 신규 출시 시 결제 연동 직접 구현)
- **콘텐츠 운영**: 신규 콘텐츠·이벤트 다수 출시, 기존 이벤트 시스템 **테이블화**로 콘텐츠 추가 파이프라인 개선
- **빌드/배포**: Android/iOS 앱 빌드, Unity 에셋번들 빌드·CDN 배포
- **라이브 대응**: 퇴근 직전 접수된 유저 간 거래 취약점 버그를 밤샘 분석으로 수정, 익일 긴급점검 배포
- **PM 병행(후반기)**: 시즌 이벤트·상품 패키지 설계, NFT 펫 리뉴얼 — 개발과 운영·사업 양쪽 언어를 씀
- **소규모(3인) 신규 프로젝트**: 기존 코드의 SDK 의존부를 추상화해 NHN GAMEBASE·IronSource 등으로 교체 — 외부 SDK/협력사 대응의 기술적 기반
- 서버 파트 8개월 병행: 원정대 탐사 시스템, GM툴 개발·운영 (Java+MySQL+Redis)

---

## 2. AI 파이프라인 툴 연작 — 에셋·콘텐츠 생성 파이프라인 구축 (개인, 전부 공개)

아트 리소스가 부족한 개발 환경의 병목을 AI로 풀기 위해 직접 시도·연구했고, 체감한 한계를 하나씩 메우는 과정에서 만들어진 도구들입니다.
**주요 AI 서비스를 종류별로 유료 구독해 비교했고, 로컬 LLM 구동용 장비(NVIDIA DGX Spark)까지 도입해 검증했습니다.**

| 툴 | 해결한 문제 |
| --- | --- |
| [SpriteForge](https://github.com/Bo-sung/SpriteForge) | AI 생성 스프라이트의 프레임 간 일관성 붕괴 → **리깅된 3D 모델(FBX/GLB)을 오프스크린 렌더링해 픽셀아트 스프라이트 시트로 변환** — 본 이름 기반 크로스 스켈레톤 리타겟팅, 장비 부착, 팔레트 양자화. CLI + 라이브 프리뷰 GUI |
| [ComfyUI-FBX-ControlNet-Converter](https://github.com/Bo-sung/ComfyUI-FBX-ControlNet-Converter) | 생성 영상·포즈의 프레임 간 흔들림 → **FBX 애니메이션을 실제 스켈레톤 기반 ControlNet 패스**(OpenPose/depth/normal/canny)로 변환해 시간적으로 안정된 AI 영상 생성. C#/OpenGL 네이티브 렌더러 내장, Mixamo 리타겟팅 지원 |
| [html_to_ugui](https://github.com/Bo-sung/html_to_ugui) | AI로 뽑은 UI 시안이 엔진 리소스로 이어지지 않음 → **HTML/CSS 시안을 Unity UGUI 프리팹으로 자동 변환**, 반복 구조는 재사용 프리팹으로 추출 |
| [AgentManager](https://github.com/Bo-sung/AgentManager) | AI 코딩 에이전트 다중 운용의 충돌·비용 → **git worktree 격리 병렬 관리**, 로컬 LLM 번역 레이어로 토큰 절감, Diff 리뷰 워크플로 |
| [ai-workspace-template](https://github.com/Bo-sung/ai-workspace-template) | 다중 AI 워커 협업 규칙 부재 → 역할·권한 거버넌스 템플릿 |

**버추얼 캐릭터 도메인과의 접점**: SpriteForge와 ComfyUI 컨버터는 모두 **리깅·스켈레톤·애니메이션 데이터를 파싱해 렌더링하는 파이프라인**입니다. 버추얼 캐릭터(3D 모델 + 모션 데이터)를 다루는 환경에서 그대로 이어지는 기술 기반입니다.

---

## 3. Titan Slayer — Unity FPS 보스 배틀 (팀 4인, 프로젝트 리드)

| | |
| --- | --- |
| 기간 | 2025.09 ~ 2025.12 |
| 역할 | **프로젝트 리드** (저학년 다수 팀) |
| 저장소 / 영상 | [GitHub](https://github.com/Bo-sung/BasicGameEngine_4_teamP) · [시연 영상](https://youtu.be/FhHfEcHxENo) |

- 아키텍처 설계·모듈 정의, 마일스톤·일정 조율, 작업 분배, 코드 리뷰 — 일정 지연 시 직접 구현으로 리스크 흡수, **기한 내 완성**
- 구현: FPS 캐릭터 제어, 무기 시스템, **상태 머신 기반 보스 AI**, 전투 시스템, HUD, **오브젝트 풀링 최적화**

## 4. 폴리글롯 — Unity XR 실시간 번역기 (팀 5인, 캡스톤)

- Meta Quest 3 + Google Cardboard **2개 XR 플랫폼 동시 지원**
- **인터페이스 기반 SDK 추상화**: 번역(Papago 메인 → ML Kit 온디바이스 폴백 자동 전환), STT(한국어 ReturnZero / 다국어 Google Cloud) — 외부 서비스 의존성을 교체 가능하게 설계
- 단종된 Cardboard SDK와 최신 Unity의 호환성 대응 — 외부 SDK 이슈를 직접 풀어본 경험

## 5. CardFlip — Unity 6 연출 데모 (개인)

- [저장소](https://github.com/Bo-sung/CardFlip) · Unity 6(URP), uGUI/TMP, iTween
- 트윈 연출(2단계 카드 플립, easeOutBack 순차 등장, 애니메이션 중 입력 차단)에 집중한 초단기 완성작
- 순수 C# Presenter(MVP)로 로직·UI 분리 — 최신 Unity 버전 사용 이력

## 6. 서버 역량 — 라이브 운영을 깊이 이해하는 클라이언트

- [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server): C#/.NET 10, 커스텀 바이너리 TCP 프로토콜, 인증(Redis 1회용 토큰)-매칭-게임세션(프로세스 동적 spawn) 3단 구조를 단독 설계·구현
- [InterplanetaryOnline](https://github.com/Bo-sung/InterPlanetery_Server): 팀 4인 RTS의 서버 메인 — 락스텝 결정론적 동기화, 하트비트 모니터링
- 실무 서버 8개월 + 개인 프로젝트로, **클라이언트-서버 간 프로토콜 협의와 라이브 이슈의 양쪽 사정**을 모두 이해합니다

---

## 기반 역량

- **Unity**: 2년 3개월 실무 (UGUI, 에셋번들, 테이블 데이터 시스템, Object Pooling) + Unity 6/URP 개인 프로젝트
- **언어**: C#(주), Java, C++ / **엔진**: Unity, Unreal Engine 5.6(학생 주도 게임잼 우승작)
- **마켓/SDK**: Google Play·App Store·화웨이 앱갤러리 출시/운영, 결제·로그인 SDK 연동, SDK 의존부 추상화 설계
- **AI**: ComfyUI 워크플로·커스텀 노드 개발, Stable Diffusion 계열 에셋 생성, LLM 도구 개발, 로컬 LLM 인프라(DGX Spark)
- **개발 도구 제작**: 기획자용 엑셀→테이블 자동 추출 툴(실무), 맵툴 2종, 위 AI 파이프라인 연작 — **팀의 병목을 도구로 푸는 습관**
