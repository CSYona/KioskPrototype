무인 셀프 계산 키오스크 시스템 (Prototype)
=============================



![Node.js](https://www.notion.so/image/attachment%3A1cab7827-3a8a-403f-b4e3-04256ef50988%3A73139655-C2DB-496C-AAE4-17733BDCACE2_4_5005_c.jpeg?table=block&id=2093114b-8b80-80ef-b42e-fa4eef3573e2&spaceId=882ec991-f563-4ea2-8f84-2f93c01b7280&width=2000&userId=&cache=v2) 

<img src="https://www.notion.so/image/attachment%3A9519bf0d-7794-4e13-bc4f-ff04054a9ea0%3A%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-06-12_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.09.35.png?table=block&id=2103114b-8b80-801e-84b9-e58ecc3fd61d&spaceId=882ec991-f563-4ea2-8f84-2f93c01b7280&width=2000&userId=&cache=v2" title="" alt="WebSocket" style="zoom:33%;"> 





## 프로젝트 개요

----------

편의점/마트의 무인 계산 시스템을 웹 기반으로 구현한 프로토타입 프로젝트입니다.
RFID 태그 기반 입장, AI 상품 인식, 자동 환불 계산 등의 기능을 시뮬레이션하며,실제 하드웨어 연동 전 소프트웨어 아키텍처 검증을 목표로 합니다.



### 프로젝트 배경

* **선결제 후 환불 방식**: 입장 시 10만원 선결제 → 구매 상품 자동 인식 → 잔액 환불
* **무인 시스템**: 직원 개입 없이 고객이 스스로 결제 완료
* **확장 가능한 설계**: 추후 AI 모델, DB, 실제 결제 시스템 연동 대비

### 프로젝트 상태

* **개발 기간**: 2025.05 ~ 2025.06 (약 1개월)
* **현재 상태**: 프로토타입 완료 (Mock 데이터 기반)
* **참여 형태**: 팀 프로젝트 중 백엔드 및 시스템 설계 담당
* **하차 사유**: 일본 취업 과정 프로그램 참여로 중도 이탈 (팀원들이 이후 AI 연동 등 지속 개발)

* * *

주요 구현 기능
----------

### 세션 관리 시스템

* UUID 기반 고유 세션 ID 생성
* 10분 자동 만료 타이머 (프론트/백엔드 동기화)
* 선결제 정보 JSON 파일 저장 및 관리

### WebSocket 실시간 통신

* 상품 인식 결과 즉시 전달
* 클라이언트-서버 양방향 메시지 처리
* HTTP 요청 없이 화면 전환 트리거

### 환불 계산 로직

* 선결제 금액(₩100,000) - 총 구매 금액 = 환불액
* 환불 내역 JSON 파일 기록
* 세션 기반 중복 환불 방지

### 5단계 UI 플로우

1. **초기 화면**: 시작하기 버튼
2. **장바구니 안내**: 상품 올리기 유도
3. **스캔 중**: 로딩 애니메이션 + 시뮬레이션 버튼
4. **영수증 출력**: 환불 완료 피드백
5. **종료 인사**: 자동 초기화 및 복귀

### Mock 데이터 시뮬레이션

* 10개 상품 샘플 데이터 (바코드, 가격 등)
* 무작위 3개 상품 선택하여 스캔 결과 생성
* AI 모델 없이도 전체 프로세스 테스트 가능

* * *

기술 스택
--------

### Backend

    Node.js v14+
    Express.js v4.18
    WebSocket (ws v8.13)
    UUID v9.0

### Frontend

    Vanilla JavaScript (ES6+)
    HTML5 / CSS3
    SessionStorage API

### Architecture

    MVC Pattern
    RESTful API
    WebSocket Protocol
    File-based Storage (JSON)

* * *

프로젝트 구조
----------

    kiosk/
    ├── app.js                    # 메인 서버 엔트리 포인트
    ├── controllers/
    │   └── kioskController.js    # 비즈니스 로직 (세션, 환불, 시뮬레이션)
    ├── routes/
    │   └── kioskRoutes.js        # API 라우팅
    ├── websocket/
    │   └── kioskSocket.js        # WebSocket 서버 설정
    ├── models/
    │   └── productModel.js       # (예비) 상품 데이터 모델
    ├── public/                   # 프론트엔드 정적 파일
    │   ├── index.html            # 키오스크 메인 UI
    │   ├── app.js                # 클라이언트 로직
    │   ├── style.css             # 스타일시트
    │   └── assets/               # 이미지 리소스
    │       ├── basket.png
    │       ├── receipt.png
    │       └── greeting.png
    ├── test/                     # 테스트 및 Mock 데이터
    │   ├── session-start.html    # 입장 시뮬레이션 페이지
    │   ├── mock-products.json    # 샘플 상품 데이터
    │   ├── paidSessions.json     # 결제 세션 로그
    │   └── refunds.json          # 환불 내역 로그
    ├── package.json
    └── README.md

* * *

설치 및 실행
----------

### 1. 저장소 클론 및 의존성 설치

    git clone [repository-url]
    cd kiosk
    npm install

### 2. 서버 실행

    node app.js

터미널에 다음 메시지가 표시되면 정상 실행된 것입니다:
    Kiosk server running at http://localhost:3000

### 3. 웹 브라우저에서 접속

**① 입장 시뮬레이션 (세션 시작)**
    http://localhost:3000/test/session-start.html

→ "입장" 버튼 클릭 시 세션 생성 후 키오스크 화면으로 자동 이동

**② 키오스크 메인 화면 (직접 접속 시)**
    http://localhost:3000/index.html

 세션 ID가 없으면 경고 메시지 표시됨

* * *

사용 방법
--------

### Step 1: 세션 시작

1. `http://localhost:3000/test/session-start.html` 접속
2. **"입장"** 버튼 클릭
3. 자동으로 키오스크 화면(`index.html`)으로 이동
   * URL에 `sessionId`와 `expireTime` 자동 포함

### Step 2: 키오스크 플로우 진행

1. **초기 화면**: "시작하기" 버튼 클릭
2. **장바구니 화면**: 3초 후 자동 전환
3. **스캔 화면**:
   * **"상품 인식 시뮬레이션"** 버튼 클릭 (실제 AI 대신)
   * WebSocket을 통해 서버에 요청 전송
4. **영수증 화면**: 자동 전환 (1초 대기)
5. **종료 화면**: 5초 후 초기 화면으로 복귀

### Step 3: 결과 확인

* **콘솔 로그**: 브라우저 개발자 도구에서 세션 정보, 스캔 결과 확인
* **파일 확인**:
  * `test/paidSessions.json`: 입장 세션 기록
  * `test/refunds.json`: 환불 내역 기록

* * *

📡 API 문서
---------

### 1. 세션 시작

    POST /api/session/start

**Response:**
    {
      "sessionId": "uuid-here",
      "paidAmount": 100000,
      "entryTime": "2025-06-12T09:00:00.000Z",
      "expireTime": "2025-06-12T09:10:00.000Z"
    }

### 2. 환불 처리

    POST /api/refund
    Content-Type: application/json
    
    {
      "sessionId": "uuid-here",
      "scannedTotal": 8650
    }

**Response:**
    {
      "message": "환불 완료",
      "refundAmount": 91350
    }

### 3. WebSocket 이벤트

**Client → Server:**
    {
      "action": "simulateScan"
    }

**Server → Client:**
    {
      "type": "scanResult",
      "items": [
        {
          "상품명": "크린랩 50M",
          "판매가격": 2300,
          ...
        }
      ],
      "totalPrice": 8650,
      "refund": 91350
    }

* * *

주요 구현 상세
-----------

### 세션 만료 처리

    // 백엔드: 10분 후 만료 시간 설정
    const expireTime = new Date(Date.now() + 10 * 60 * 1000);
    
    // 프론트엔드: 남은 시간 계산 후 타이머 설정
    const timeRemaining = expireTimestamp - Date.now();
    setTimeout(() => {
      alert("세션이 만료되었습니다.");
      sessionStorage.clear();
      goToScreen("screen-start");
    }, timeRemaining);

### WebSocket 실시간 통신

    // 서버: 상품 인식 결과 전송
    ws.send(JSON.stringify({
      type: "scanResult",
      items: [...],
      totalPrice: 8650,
      refund: 91350
    }));
    
    // 클라이언트: 수신 후 화면 전환
    socket.onmessage = (event) => {
      const data = JSON.parse(event.data);
      if (data.type === "scanResult") {
        goToScreen("screen-receipt");
      }
    };

### Mock 시뮬레이션 로직

    exports.simulateScan = () => {
      const mockProducts = JSON.parse(fs.readFileSync('mock-products.json'));
      const selectedItems = mockProducts.sort(() => 0.5 - Math.random()).slice(0, 3);
      const totalPrice = selectedItems.reduce((sum, item) => sum + item.판매가격, 0);
      return {
        items: selectedItems,
        totalPrice,
        refund: 100000 - totalPrice
      };
    };

* * *

화면 구성
--------

| 화면      | ID               | 설명            | 전환 조건        |
| ------- | ---------------- | ------------- | ------------ |
| 초기 화면   | `screen-start`   | 시작하기 버튼       | 버튼 클릭        |
| 장바구니 안내 | `screen-basket`  | 상품 올리기 유도     | 3초 자동        |
| 스캔 중    | `screen-scan`    | 로딩 + 시뮬레이션 버튼 | WebSocket 수신 |
| 영수증 출력  | `screen-receipt` | 환불 완료 표시      | 1초 자동        |
| 종료 인사   | `screen-goodbye` | 감사 인사         | 5초 후 초기화     |

* * *

프로토타입 한계 및 향후 개선
-------------------

### 현재 한계점

* ❌ Mock 데이터 기반 (실제 AI 인식 미연동)
* ❌ JSON 파일 저장 (DB 미사용)
* ❌ 실제 결제 시스템 미연동
* ❌ RFID 리더기 미연동
* ❌ 초음파 센서 미연동
* ❌ 프린터 연동 미구현

### 향후 개선 방향

    ✅ AI 상품 인식 모델 연동 (카메라 기반)
    ✅ PostgreSQL/MongoDB 도입
    ✅ 실제 PG사 연동 (토스페이먼츠, 이니시스 등)
    ✅ HTTPS + WSS 보안 통신
    ✅ 영수증 프린터 드라이버 연동
    ✅ 관리자 대시보드 구축

* * *

🧪 테스트 환경
---------

### 필수 요구사항

* Node.js v14 이상
* 웹 브라우저 (Chrome, Edge 권장)
* WebSocket 지원 브라우저

### 확인사항

1. **포트 충돌 확인**: 3000번 포트가 사용 중이면 `app.js`에서 변경
      const PORT = 3000; // → 다른 포트로 변경

2. **WebSocket 연결 실패 시**:
      // public/app.js 수정
      socket = new WebSocket("ws://localhost:3000");
      // → 포트 번호 확인 후 수정
   
   
   
   

* * *

기여자
------

* **최승연** - 백엔드 개발, 시스템 아키텍처 설계, WebSocket 구현

* **팀원 3명** - 프론트엔드 UI, AI 모델 개발 (이후 진행) 


