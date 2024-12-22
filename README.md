# ChikaChika 프로젝트

## 📚 프로젝트 소개
ChikaChika는 어린이를 대상으로 올바른 양치 습관을 형성할 수 있도록 도와주는 양치 도우미 서비스입니다. 이 서비스는 **WebSocket**과 **REST API**를 활용하여 양치 상태를 실시간으로 관리하고, 사용자에게 올바른 양치 순서를 안내합니다.

---

## 🔑 주요 기능

### 1. 양치 순서 및 상태 관리
- 각 양치 구역의 닦은 횟수에 따라 상태를 계산하여 실시간으로 업데이트합니다.
- 양치 진행 상태는 다음 네 가지 단계로 나뉩니다:
  - **None**: 양치를 시작하지 않음
  - **Little**: 일부만 닦음
  - **Medium**: 절반 이상 닦음
  - **Complete**: 완벽히 닦음

### 2. 양치 데이터 관리
- 양치 데이터를 세션에 저장하여 사용자가 양치 중단 후 다시 연결하더라도 진행 상황을 유지합니다.
- WebSocket을 통해 클라이언트와 서버 간 실시간 데이터 전송을 지원합니다.

### 3. REST API 제공
- 양치 상태 초기화, 양치 데이터 업데이트 등 다양한 동작을 지원하는 API를 제공합니다.

### 4. WebSocket 실시간 알림
- 양치 진행 상황에 따라 다음 구역을 안내하거나 완료 메시지를 클라이언트에 실시간으로 전송합니다.

---

## 📂 프로젝트 구조

### 주요 패키지

- **`controller`**
  - `ApiController`: REST API를 처리하며 클라이언트 요청에 대한 응답을 관리합니다.

- **`service`**
  - `TeethBrushedService`: 양치 상태 관리와 데이터 처리를 담당합니다.

- **`model`**
  - `TeethSource`: 클라이언트에서 양치 데이터를 전송할 때 사용되는 데이터 모델입니다.
  - `Teeth`: 서버에서 클라이언트로 응답을 보낼 때 사용되는 데이터 모델입니다.

- **`configuration`**
  - WebSocket 설정 및 초기 메시지 전송 기능을 제공합니다.

- **`util`**
  - `TeethSessionManager`: 사용자의 양치 데이터를 세션에 저장하고 관리합니다.

---

## 🔍 주요 클래스 설명

### `ApiController`
- 양치 관련 데이터를 처리하는 REST API를 제공합니다.

#### 제공 API:
- **`POST /api/reset`**: 양치 데이터를 초기화합니다.
- **`POST /api/teeth`**: 클라이언트에서 양치 데이터를 전송받아 서버에서 상태를 갱신하고 WebSocket으로 응답을 전송합니다.

### `TeethBrushedService`
- 양치 상태를 관리하고 데이터를 처리하는 핵심 서비스입니다.

#### 주요 메서드:
- `setTeethSectionBrushedLevel`: 특정 구역의 양치 상태를 업데이트합니다.
- `getTeethSectionBrushedLevel`: 현재 양치 상태를 반환합니다.
- `getNeedToBrushSectionIdx`: 다음에 닦아야 할 구역의 인덱스를 계산합니다.
- `resetBrushedInfo`: 양치 상태를 초기화합니다.

### `CustomWebSocketHandler`
- WebSocket 연결을 관리하며 클라이언트와 실시간으로 데이터를 주고받습니다.

#### 주요 메서드:
- `afterConnectionEstablished`: 클라이언트가 연결되면 초기 메시지를 전송합니다.
- `sendMessage`: 서버에서 생성된 메시지를 클라이언트에 전송합니다.

---

## 📄 API 명세

### **POST /api/reset**
- **설명**: 양치 데이터를 초기화합니다.

#### 응답:
- **성공**: 양치 데이터가 초기화됩니다.

---

### **POST /api/teeth**
- **설명**: 특정 구역의 양치 상태를 서버에 업데이트합니다.

#### Request Body:
```json
{
  "brushedSection": "UF_OUTSIDE",
  "brushedCnt": 3
}
