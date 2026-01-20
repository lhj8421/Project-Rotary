# Project-Rotary


## 1. 프로젝트 소개
* 4가지 하드웨어 장치의 커널 드라이버를 통한 센서 데이터 수집 및 OLED 디스플레이 제어로 동작

### 프로젝트 개요
* 본 프로젝트는 임베디드 리눅스 환경에서 다양한 하드웨어(OLED, DHT11, DS1302, Rotary Encoder)를 제어하기 위한 **리눅스 캐릭터 디바이스 드라이버(Linux Character Device Driver)**를 구현한 결과물입니다.

* 애플리케이션이 하드웨어 복잡성을 신경 쓰지 않고, 표준 파일 입출력 함수를 통해 센서를 제어할 수 있도록 커널 모듈을 설계했습니다.

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


## 3. 멀티스레드 데이터 수집

> ### **중앙 집중식 데이터 관리를 통한 안정적인 동기화**

### 🔄 Application 내부 구조

<img width="934" height="520" alt="스크린샷 2026-01-07 105046" src="https://github.com/user-attachments/assets/b9b427b5-b804-4f62-a247-7ddcb038ee79" />

> ### **중앙 집중식 데이터 관리를 통한 안정적인 동기화**
**계층 구조**
- **Application Layer**: 멀티스레드 기반 센서 데이터 수집
- **Kernel Layer**: 4개의 커널 드라이버 모듈
  - `oled.ko` - I2C 기반 SSD1306 제어
  - `dht11.ko` - GPIO 기반 온습도 센서
  - `ds1302.ko` - GPIO 기반 RTC
  - `rotary.ko` - GPIO 기반 로터리 엔코더
- **Hardware Layer**: I2C/GPIO 컨트롤러를 통한 하드웨어 제어

**동작 원리 :**
센서 스레드 → read() → parse → Shared_data_t → OLED 출력

**각 스레드 역할**
- **DS1302 Thread**: `/dev/ds1302` → 시간 정보 읽기
- **DHT11 Thread**: `/dev/dht11` → 온습도 정보 읽기
- **Rotary Thread**: `/dev/rotary` → 사용자 입력 감지
- **OLED Thread**: 공유 데이터 → `/dev/oled` → 화면 출력

## 4. FSM 기반 시간 편집 모드

> ### **Rotary Encoder 3가지 입력으로 직관적인 UI 구현**

### 🎛️ 상태 전이 다이어그램

**입력 이벤트**
- **CLICK**: 다음 필드로 이동
- **CW (시계방향)**: 값 증가
- **CCW (반시계방향)**: 값 감소

**상태 흐름**  
<img width="942" height="515" alt="image" src="https://github.com/user-attachments/assets/98cdec48-ad19-4c6a-958a-bd8b496e422b" />


**편집 과정**
1. 정상 화면에서 CLICK → 편집 모드 진입
2. CW/CCW로 현재 필드 값 조정
3. CLICK으로 다음 필드 이동
4. 마지막 필드(초) 편집 완료 후 자동 복귀



## 6. 하드웨어 구성

> ### **실제 구현 및 연결**

> ### 📸 실제 동작 모습
<img width="695" height="338" alt="스크린샷 2026-01-07 093118" src="https://github.com/user-attachments/assets/76a4e05c-00c2-4804-96b1-2a391cf64d0a" />

### 🔌 핀 연결도
<img width="942" height="529" alt="스크린샷 2026-01-07 104756" src="https://github.com/user-attachments/assets/b8673759-36e0-4a82-a0a3-3daee0ffae88" />



