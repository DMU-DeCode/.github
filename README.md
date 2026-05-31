# UPM (Universal PC Manager)

[![UPM Banner](https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=200&section=header&text=UPM%20Universal%20PC%20Manager&fontSize=45&fontColor=FFFFFF&fontAlignY=38&desc=Desktop%20Resource%20Optimization%20%7C%20Hardware%20Control%20%7C%20Cloud%20Management&descSize=16&descAlignY=58)](https://github.com/your-org/upm)

<br/>

## 프로젝트 개요

UPM은 **Windows PC의 리소스를 실시간 모니터링·최적화**하고, 직접 제작한 **아두이노 스트림덱(물리 하드웨어)**과 **모바일 앱**으로 언제 어디서든 PC를 제어할 수 있는 통합 관리 플랫폼입니다.

> 아두이노 통신 방식을 무선(MQTT) → **유선(USB Serial)**로 전환하여 **Zero-Latency** 하드웨어 제어를 실현하고,
> 크라우드소싱 기반 통계 분석으로 **전 세계 유저 데이터를 활용한 지능형 프로세스 최적화**를 구현합니다.

<br/>

## 프로젝트 소개

| 기능 | 설명 |
|------|------|
| 🖥️ **실시간 PC 모니터링** | CPU / RAM / GPU 사용률 및 프로세스 정보를 1초 단위로 수집·시각화 |
| 🎛️ **아두이노 스트림덱** | USB Serial 직결로 Zero-Latency 하드웨어 제어 및 LCD 실시간 렌더링 |
| ☁️ **크라우드소싱 최적화** | 전 세계 UPM 유저 데이터 분석 → 프로세스 블랙/화이트리스트 자동 배포 |
| 📱 **모바일 원격 제어** | 외부에서도 MQTT를 통해 즉각적인 PC 제어 명령 전송 |
| 📊 **리소스 히스토리** | 모바일 대시보드에서 과거 리소스 사용량 그래프 확인 |

<br/>

## 팀 멤버 소개

| **이름 (역할)** | **이름 (역할)** | **이름 (역할)** | **이름 (역할)** |
|:---:|:---:|:---:|:---:|
| <img src="https://github.com/identicons/member1.png" width="80"> | <img src="https://github.com/identicons/member2.png" width="80"> | <img src="https://github.com/identicons/member3.png" width="80"> | <img src="https://github.com/identicons/member4.png" width="80"> |
| [@GitHub아이디](https://github.com/) | [@GitHub아이디](https://github.com/) | [@GitHub아이디](https://github.com/) | [@GitHub아이디](https://github.com/) |
| 팀장(PM) / 데스크톱 앱 | 아두이노 / 하드웨어 | 백엔드 서버 | 모바일 / 프론트 |

<br/>

## 개발 환경

- **Desktop**: ![C#](https://img.shields.io/badge/C%23-239120?style=flat&logo=csharp&logoColor=white) ![.NET](https://img.shields.io/badge/.NET_8-512BD4?style=flat&logo=dotnet&logoColor=white) ![WPF](https://img.shields.io/badge/WPF-0078D6?style=flat&logo=windows&logoColor=white)

- **Hardware**: ![Arduino](https://img.shields.io/badge/Arduino-00979D?style=flat&logo=arduino&logoColor=white) ![C++](https://img.shields.io/badge/C++-00599C?style=flat&logo=cplusplus&logoColor=white)

- **Back-end**: ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)

- **Database**: ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)

- **Messaging**: ![MQTT](https://img.shields.io/badge/MQTT-660066?style=flat&logo=eclipsemosquitto&logoColor=white)

- **협업 툴**: ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-ffffff?style=flat&logo=notion&logoColor=black) ![Discord](https://img.shields.io/badge/Discord-5865F2?style=flat&logo=discord&logoColor=white)

- **디자인**: ![Figma](https://img.shields.io/badge/Figma-F24E1E?style=flat&logo=figma&logoColor=white)

<br/>

## 프로젝트

### 시스템 구성도

```
┌─────────────────────────────────────────────────────────────────────┐
│                  Tier 3 : 리모트 모바일 클라이언트                    │
│              [ 모바일 대시보드 & 컨트롤러 (Web / App) ]               │
│            REST GET (통계 조회) │ MQTT Publish (원격 제어)            │
└──────────────────────┬──────────────────────┬───────────────────────┘
                       │                      │
┌──────────────────────▼──────────────────────▼───────────────────────┐
│                      Tier 2 : 서버 인프라                             │
│    [ FastAPI 글로벌 최적화 서버 ]    [ Eclipse Mosquitto MQTT 브로커 ] │
│               MySQL DB                                               │
└──────────────────────┬──────────────────────────────────────────────┘
                       │  HTTP (REST) + MQTT Subscribe
┌──────────────────────▼──────────────────────────────────────────────┐
│                 Tier 1 : 엣지 PC & 로컬 하드웨어                      │
│  [ UPM 데스크톱 앱 (C#, WPF) ] ──USB Serial── [ Arduino 스트림덱 ]   │
│    CPU/RAM/GPU 텔레메트리 수집       Zero-Latency  LCD 실시간 렌더링   │
└─────────────────────────────────────────────────────────────────────┘
```

<br/>

### API 데이터 명세 (JSON)

> 시스템 간 모든 데이터는 **camelCase** 규칙을 따릅니다.

**`POST /api/v1/metrics`** — 텔레메트리 전송 (Desktop → Server)
```json
{
  "machineId": "UPM_PC_01",
  "cpuUsagePercent": 45.2,
  "ramUsagePercent": 68.5,
  "gpuTemperatureCelsius": 72.0,
  "timestamp": "2025-06-01T12:00:00Z",
  "topProcesses": [
    { "processId": 14502, "processName": "chrome", "memoryWorkingSetBytes": 1048576000, "isSystemCritical": false }
  ]
}
```

**`GET /api/v1/optimization/rules`** — 최적화 룰셋 수신 (Server → Desktop)
```json
{
  "version": "v1.0.5",
  "blackList": ["bloatware.exe", "adware_updater.exe"],
  "whiteList": ["svchost", "explorer", "System", "csrss"]
}
```

**USB Serial (BaudRate: 115200)** — LCD 모니터링 출력 (Desktop → Arduino)
```json
{ "cpu": 45, "ram": 68, "gpuTemp": 72, "status": "NORMAL" }
```

**USB Serial / MQTT `upm/command`** — 제어 명령 (Arduino or Mobile → Desktop)
```json
{ "actionType": "SHORTCUT", "actionData": "CTRL+C" }
```
> `actionType` : `"SHORTCUT"` | `"SHUTDOWN"` | `"BOOST"`

<br/>

## 주요 기능

### 🖥️ Tier 1 — 데스크톱 앱 & 아두이노

**UPM 데스크톱 앱** `C# / WPF / .NET 8`
- CPU, RAM, GPU 사용률 및 프로세스 정보 실시간 수집 및 LiveCharts 시각화
- 아두이노와 USB COM Port 직결 — 1초마다 상태 전송 및 명령 수신
- 백그라운드에서 MQTT 브로커 구독(Subscribe) 대기

**UPM 스트림덱** `Arduino / C++`
- PC에서 수신한 JSON 데이터를 파싱해 LCD에 60fps급 실시간 렌더링
- 물리 버튼 클릭 시 시리얼 포트를 통해 단축키 / 종료 / 부스트 명령 전달

### ☁️ Tier 2 — 서버 인프라

**글로벌 최적화 서버** `Python / FastAPI / MySQL`
- 수백 대 PC의 텔레메트리 데이터를 비동기(Async) 방식으로 수집·저장
- 크라우드소싱 통계 분석으로 프로세스 블랙/화이트리스트 생성 및 각 PC에 배포

**원격 제어 중계기** `Eclipse Mosquitto (MQTT)`
- 외부 네트워크에서 모바일 ↔ 데스크톱 간 초경량 명령 메시지 중계

### 📱 Tier 3 — 모바일 클라이언트

**모바일 대시보드 & 컨트롤러** `Web / App`
- FastAPI REST 요청으로 내 PC의 과거 리소스 사용량 그래프 조회
- MQTT Publish로 외부에서 즉각적인 PC 제어 명령 실행

<br/>

## 프로젝트 계획서 PPT

[📎 UPM 프로젝트 계획서.pptx](#)

## 프로젝트 최종 발표 PPT

[📎 UPM 최종 발표.pptx](#)

## 시연 영상

[![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](#)

[▶️ UPM 시연 영상 보러가기](#)

<br/>

## 느낀점

### 팀원 이름

- (여기에 느낀점을 작성해주세요)

### 팀원 이름

- (여기에 느낀점을 작성해주세요)

### 팀원 이름

- (여기에 느낀점을 작성해주세요)

### 팀원 이름

- (여기에 느낀점을 작성해주세요)

---

[![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=120&section=footer)](https://github.com/your-org/upm)
