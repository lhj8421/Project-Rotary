# Project-Rotary


## 1. 프로젝트 소개
* 4가지 하드웨어 장치의 커널 드라이버를 통한 센서 데이터 수집 및 OLED 디스플레이 제어로 동작

### 주요 기능
- **실시간 온습도 모니터링 (DHT11)**
- **정확한 시간 관리 (DS1302 RTC)**
- **직관적인 시간 편집 UI (Rotary Encoder + FSM)**
- **OLED 실시간 데이터 시각화**

**개발 기간**: 2025.12.24 ~ 2025.12.29
**개발 환경**: Raspberry Pi / Linux Kernel Driver / C / Multithreading  



## 2. 시스템 아키텍처
### 🏗️ 전체 시스템 구조
<img width="951" height="523" alt="스크린샷 2026-01-07 104811" src="https://github.com/user-attachments/assets/a93c304b-dcb9-450b-9448-0a5df7b16f20" />

**동작 원리**
센서 스레드 → read() → parse → Shared_data_t → OLED 출력
