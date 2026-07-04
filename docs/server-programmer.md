# 서보성 | 게임 서버 프로그래머 포트폴리오

> Unity 클라이언트 2년 3개월 + 서버 개발 8개월(병행)의 라이브 서비스 실무 경험 위에,
> C# 기반 서버(순수 소켓·ASP.NET Core)를 독학으로 쌓아올린 서버 프로그래머 지망생입니다.
> "게임의 핵심 로직은 서버에 있다"는 확신으로 클라이언트에서 서버로 전향했습니다.

📧 ssbss0329@gmail.com · 🐙 [github.com/Bo-sung](https://github.com/Bo-sung) · [종합 포트폴리오](../README.md)

---

## 한눈에 보기

| # | 프로젝트 | 무엇을 | 역할 | 핵심 기술 포인트 |
| --- | --- | --- | --- | --- |
| 1 | **BattleShip** (개인) | 3단 분산 게임 서버 | 설계~배포 전 과정 | 커스텀 바이너리 TCP 프로토콜, 세션 프로세스 동적 spawn, Redis 1회용 토큰 |
| 2 | **InterplanetaryOnline** (팀 4인) | 멀티플레이어 RTS 서버 | 서버 메인 | 락스텝 결정론적 동기화, 입력 버퍼링·지연 보상, 동접 8명 검증 |
| 3 | **ASPAuthServer** (개인) | 인증 서버 + GM 툴 | 단독 | ASP.NET Core RESTful, Redis 세션, 토큰 자동 갱신 GM 툴 |
| 4 | **authLobbyGame** (개인, 진행 중) | 3-Tier 분산 아키텍처 | 단독 | TCP+UDP(ENet) 하이브리드, 설계 문서 9종 선행 |
| - | **실무** (Funigloo, 2년 3개월) | 라이브 서비스 | 클라 2년 3개월 + 서버 8개월 | Java+MySQL+Redis 운영, 원정대 탐사 시스템 |

---

## 서버 관련 실무 경험 (Funigloo, 2022.01~2024.04, 산업기능요원)

- **신규 프로젝트 서버 개발/운영 8개월**: 오픈 직전부터 오픈 후 3개월까지 참여
- **원정대 탐사 시스템**(보유 캐릭터 기반 보상 콘텐츠) 개발 — 클라이언트 개발자와의 협업으로 진행, 서버 개발자의 입장에 직접 서보며 양쪽 관점을 모두 체득
- 재화 시스템, 배틀패스 구현, GM 툴 유지보수 및 신규 기능 개발
- **서버 간 동기화, Redis 캐시 활용, 게임 DB 설계, 프로토콜 구성** 등 게임 서버 핵심 개념을 실제 서비스 환경에서 학습
- **Java 백엔드 + MySQL(유저 데이터·게임 로직) + Redis(세션·캐싱) + Rocky Linux 운영**
- 회사에서 더 성장하고 싶었지만, 복학과 함께 **서버를 더 깊이 공부하기 위해 퇴사를 선택** — 이후 C# 서버 독학 연작(아래 프로젝트들)으로 이어짐

---

## 1. BattleShip — 커스텀 바이너리 프로토콜 3단 분산 서버 (개인)

| | |
| --- | --- |
| 역할 | 개인 프로젝트 (설계~배포 전 과정) |
| 기술 | C# / .NET 10, 순수 TcpListener/TcpClient, MySQL(MySqlConnector), Redis(StackExchange.Redis) |
| 저장소 | [BattleShip_Server](https://github.com/Bo-sung/BattleShip_Server) · [BattleShip_Client](https://github.com/Bo-sung/BattleShip_Client) |

HTTP 프레임워크 없이 **순수 TCP 소켓 위에 바이너리 프로토콜을 직접 설계**한 멀티플레이어 배틀십 서버.
인증-매칭-게임세션-정산-배포까지 게임 서버 인프라의 한 사이클을 전부 직접 구현하는 것이 목표였습니다.

### 아키텍처

```
클라이언트
   │  ①로그인(MySQL) → 1회용 토큰 발급(Redis, TTL 30초)
AuthServer (:7001)
   │  ②토큰 검증 → 방 매칭
LobbyServer (:7002)
   │  ③양측 Ready → 게임 세션 프로세스 동적 spawn
GameSession (동적 포트 7010~7200, 독립 프로세스)
   │  ④세션이 Lobby로 역접속 → 룰 수신 → 게임 진행 → 결과 ACK 전송
   └→ MySQL 전적 저장
```

### 프로토콜 설계

- **6바이트 length-prefix 헤더**: Length / PacketId / Sequence
- PacketId 대역 분리: 1xxx(인증) / 2xxx(로비) / 3xxx(게임) / 9xxx(공통)
- `C_`(클라→서버) / `S_`(서버→클라) / `SS_`(서버 간) 패킷 명명 규칙
- 5개 프로젝트가 공유하는 `BattleShip.Common` 프로토콜 라이브러리 — Unity 클라이언트에는 DLL로 배포

### 구현 상세

- **1회용 토큰 인증**: Redis TTL 30초, 사용 즉시 무효화 — 세션 서버가 인증 서버에 의존하지 않는 구조
- **게임 세션 동적 spawn**: 매칭 성사 시 독립 프로세스로 세션 실행, 포트 풀 관리, SessionReady 레이스 컨디션 방지
- **Watchdog**: 무응답 30초 시 좀비 프로세스 자동 정리
- **신뢰성**: 게임 결과 전송에 ACK + 3회 재시도
- **게임 룰 JSON 외부화**: 코드 수정 없이 룰 변경, standalone 모드(`--config classic`) 지원
- **운영 도구**: CLI 테스트 클라이언트, paramiko 기반 SSH 배포 스크립트

### 배운 것 / 한계

- 프로세스 격리형 세션 서버의 장점(장애 격리)과 비용(IPC·포트 관리 복잡도)을 모두 체감
- 비밀번호가 현재 평문 비교 상태 — 해싱 도입이 다음 과제 (README에도 명시)

---

## 2. InterplanetaryOnline — 락스텝 동기화 멀티플레이어 RTS (팀 4인, 서버 메인)

| | |
| --- | --- |
| 개발 기간 | 2025.09.29 ~ 2025.12.13 |
| 역할 | **서버 전체 개발(메인) + 클라이언트 네트워크 계층** |
| 기술 | C#, Unity, MySQL, TCP/UDP |
| 저장소 | [InterPlanetery_Server](https://github.com/Bo-sung/InterPlanetery_Server) |

실시간 멀티플레이어 전략 시뮬레이션(RTS). 서버 전체를 혼자 담당하고, 클라이언트에는 네트워크 매니저/핸들러를
구현해 다른 팀원들이 게임 로직에만 집중할 수 있게 했습니다.

### 서버 구현 (메인 담당)

- **인증**: 로그인, 회원가입, 세션 관리
- **방 관리**: 방 생성/입장/나가기, 빈방 자동 정리·재사용
- **락스텝(lockstep) 동기화**: 결정론적 동기화 방식으로 멀티플레이어 구현
- **하트비트**: 클라이언트-서버 연결 상태 실시간 모니터링
- **DB**: MySQL 기반 유저 접속 정보·테이블 데이터 관리
- **프로토콜**: TCP/UDP 소켓 기반 통신 프로토콜 설계·구현

### 클라이언트 네트워크 계층

- 네트워크 매니저(연결 관리·패킷 송수신 인터페이스), 패킷 핸들러(파싱·처리)
- **추상화된 API 제공으로 팀원들이 네트워크 세부사항 없이 게임 로직 구현** — 게임 로직과 네트워크의 분리

### 기술적 도전과 해결

| 과제 | 해결 |
| --- | --- |
| 락스텝 동기화의 네트워크 지연 처리 | 입력 버퍼링 + 지연 보상 알고리즘으로 안정적 동기화 달성 |
| 빈방 자동 정리·재사용 | 방 상태 관리 시스템으로 서버 리소스 효율화 |
| 클라이언트 연결 끊김 감지 | 하트비트 기반 실시간 모니터링 + 자동 재연결 |
| 팀원용 네트워크 인터페이스 설계 | 네트워크 매니저 API 추상화로 게임 로직과 네트워크 분리 |

### 성과

- 서버 전체 아키텍처 설계·구현 완료, 테스트 시 **최대 동시 접속 8명** 안정 처리
- 락스텝 동기화를 통한 완전한 게임 상태 일치

---

## 3. ASPAuthServer — RESTful 인증 서버 + WPF GM 툴 (개인)

| | |
| --- | --- |
| 역할 | 개인 프로젝트 (퇴사 후 C# 서버 전향 학습) |
| 기술 | ASP.NET Core 8.0, C# 12, MySQL 8, Redis, WPF(.NET 8) + ModernWpfUI |
| 저장소 | [ASPAuthServer](https://github.com/Bo-sung/ASPAuthServer) |

실무(Java 백엔드)에서 느낀 서버 개발 흥미를 C# 역량으로 전환하기 위해 **인증 서버와 GM 툴을 세트로** 설계·구현.
설계 문서 5종(AUTH_SERVER_DESIGN, API_DOCUMENTATION, DESIGN_DOCUMENT, LOG_SYSTEM_DESIGN, PERFORMANCE_GUIDE)을 먼저 쓰고 구현하는 프로세스로 진행했습니다.

### AuthServer

- 회원가입 / 로그인 / 로그아웃 / 토큰 검증, DB 자동 초기화
- **RESTful 설계 원칙 준수**: 리소스 중심 URL(`GET /api/auth/users/{id}`), 상태 코드 구분, 생성 시 `201 Created` + `Location` 헤더
- **보안 정책**: 로그인 실패 3회 시 15분 계정 잠금, Game/Admin JWT 분리 설계
- **세션**: Redis 기반, TTL + 슬라이딩 만료, 로그아웃 시 즉시 무효화
- Repository 패턴, Options 패턴(+ValidateOnStart), DI 구성

### GMTool (WPF)

- **MVVM**(CommunityToolkit.Mvvm) + DI 구조
- **토큰 자동 갱신**: `DelegatingHandler`로 401 감지 시 Refresh Token 재발급 후 재요청
- 사용자 관리: 목록(페이지네이션/검색/필터), 상세, 잠금/해제, 비밀번호 초기화, 세션 강제 종료
- **실시간 로그 뷰어**: 전 화면 하단 고정 — API 호출/에러/사용자 액션 추적, 레벨 필터·검색
- 성능 고려: DataGrid 가상화, 페이지네이션, 이펙트 최소화

서버(API)와 운영 도구(GM 툴)를 한 세트로 보는 **라이브 서비스 관점**이 이 프로젝트의 핵심입니다.

---

## 4. authLobbyGame — 3-Tier 분산 아키텍처 설계 (개인, 진행 중)

| | |
| --- | --- |
| 기술 | ASP.NET Core(인증) + 순수 C# 소켓(로비/게임) + ENet-CSharp(UDP), MySQL + Redis |
| 저장소 | [GameServerP_Personal](https://github.com/Bo-sung/GameServerP_Personal) |

인증(HTTP/JWT) → 로비(TCP) → 게임(TCP+UDP) 서버를 분리한 **범용 분산 게임 서버 아키텍처** 설계·구현 프로젝트.

- **하이브리드 프로토콜**: TCP(중요 이벤트·채팅) + UDP/ENet(위치 동기화·실시간 상태)
- **이중 DB**: MySQL(영구 저장 — 유저/전적/통계) + Redis(세션·매칭큐·방 정보·리더보드·서버 상태)
- **장르 독립적 설계**: game_mode·JSON stats로 FPS/MOBA/퍼즐 어디든 적용 가능한 범용 구조
- **자동 리소스 관리**: 비활성 방 주기 스캔 제거, 세션 TTL, 게임 서버 하트비트 중단 시 자동 제외
- **설계 문서 9종 선행 작성**: ARCHITECTURE, NETWORK_PROTOCOL, DATABASE_SCHEMA, REDIS_DATA_STRUCTURES, SERIALIZATION_SPEC, SYSTEM_DIAGRAMS(다이어그램 15개) 등
- 구현 진행: LobbyServer(Room/RoomManager/ClientSession), CommonLib(Redis 래퍼 — 세션/캐시/PubSub/RateLimiter, Protocol, Command 패턴)
- 같은 저장소에 **소켓 틱택토 → 패킷/프로토콜 분리 → 분산 아키텍처**로 이어지는 학습 계단이 그대로 남아 있음

---

## 기반 역량

- **네트워크**: TCP/UDP 소켓, 커스텀 바이너리 프로토콜 설계, 락스텝/하트비트/ACK 재시도 패턴 구현 경험
- **DB**: MySQL 스키마 설계·쿼리 최적화, Redis 자료구조 활용(세션/매칭큐/리더보드/PubSub/RateLimiter)
- **인프라**: Rocky Linux(실무)·CentOS 7 운영, 고교 시절부터 실물 서버(Dell PowerEdge R300)로 게임 서버 자가 운영, 자체 Git 서버 구축
- **운영 감각**: 2년 3개월 라이브 서비스에서 긴급 이슈 대응·핫픽스 배포·QA 협업 경험
- **신기술 검증 습관**: VR/AR(캡스톤 XR 번역기), NFT(실무 연동 서비스 유지보수), AI(유료 구독 탐구 → [AgentManager](https://github.com/Bo-sung/AgentManager) 등 툴 직접 제작)까지 — 새 기술을 직접 검증한 뒤 실전 투입하는 사이클을 반복해 왔습니다. 서버 분야에서도 새로운 스택을 빠르게 흡수해 검증할 수 있습니다
