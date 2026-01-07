# Project-Rotary

## 📌 시스템 동작 방식
* 본 시스템은 4가지 하드웨어 장치의 커널 드라이버를 통한 센서 데이터 수집 및 OLED 디스플레이 제어로 동작

### 🔄 멀티스레드 기반 데이터 수집 및 동기화
**동작 순서 :**
Application → /dev 파일 접근(open) → 데이터 입출력(read/write) → 커널 드라이버 → 하드웨어 제어

**구조**
* 각 센서(DS1302, DHT11, Rotary Encoder, OLED)마다 독립적인 스레드 생성
* 디바이스 파일(/dev/ds1302, /dev/dht11, /dev/rotary, /dev/oled)을 통해 커널 드라이버와 통신
* 공유 구조체(Shared_data_t)에 모든 데이터 저장
  * 시간(time)
  * 온습도(temp, humi)
  * 로터리 입력(Click, CW, CCW)

 **스레드 동작 흐름:**
* 데이터 read → parse → 공유 구조체에 저장 → update flag 설정
