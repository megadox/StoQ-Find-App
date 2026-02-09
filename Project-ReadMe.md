StoQ는 일반 가정 및 사무실 환경에서 별도의 하드웨어 없이 스마트폰만으로 계층적 스토리지와 아이템을 관리하고, AR 기반으로 위치를 특정하는 지능형 재고 관리 솔루션입니다.  

### 1. 프로젝트 개요   
* 앱 이름: StoQ   
* 핵심 기능: 하우스-스토리지-아이템 계층 구조 관리, QR/바코드 인식, AR 기반 위치 마킹, 스마트폰 서버 모드.  
* 대상 사용자: 가정용(냉장고, 창고 정리), 사무실(비품 관리)

### 2. 시스템 아키텍처  
별도의 서버 구축 없이 사용자 기기간 P2P 연동을 지원하는 Hybrid-Local Architecture를 채택합니다.  

* Server Side (Master Device): Python (FastAPI) 기반의 로컬 API 서버 구동.  
* Client Side (User Device): Flutter 기반의 크로스 플랫폼 앱.  
* Communication: 로컬 네트워크(Wi-Fi) 내 REST API 통신.  
* Storage: SQLite를 이용한 오프라인 우선 데이터 관리.

### 3. 기술 스택 (Tech Stack)  
| 구분  | 선택 기술 | 사유 |
| ------------- | ------------- |------------- |
| Frontend  | Flutter (Dart)  | 단일 코드베이스로 iOS/Android 대응, ARCore/Kit 연동성 우수.  |
| Backend  | Python (FastAPI)  | 가벼운 실행 환경, 개발자님의 Python 숙련도 활용.  |
| Database  | SQLite  | 별도 서버 설정 없는 로컬 DB 관리.  |
| AR/Scanning  | ML Kit & ARCore  | 카메라 기반의 위치 특정 및 바코드 인식.  |

### 4. 데이터 모델 (ERD 초안)  
계층 구조와 아이템이 스토리지 역할을 할 수 있는 재귀적 구조를 가집니다.  

```
SQL

CREATE TABLE Nodes (
    id TEXT PRIMARY KEY,           -- 고유 식별자 (UUID)
    parent_id TEXT,                -- 부모 노드 ID (Nullable) 
    type TEXT CHECK(type IN ('HOUSE', 'STORAGE', 'ITEM')), -- 항목 타입 
    name TEXT NOT NULL,            -- 항목 이름 
    qr_code TEXT,                  -- 매핑된 QR 코드 값 
    ar_data JSON,                  -- AR 공간 좌표 (x, y, z)
    properties JSON,               -- 유통기한, 설명 등 가변 속성 
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP, [cite: 8]
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP [cite: 8]
);
```

### 5. 상세 기능 명세 (Work Items)  
🛠 Phase 1: 자산 관리 기반 (Core)  
* [ ] 계층형 CRUD: 하우스, 스토리지, 아이템의 추가/삭제/변경 인터페이스 구현.  
* [ ] 속성 상속 로직: 부모 스토리지의 설정이 하위 노드에 기본값으로 적용되는 엔진 개발.  
* [ ] 바코드/QR 스캐너: Google ML Kit을 이용한 인식 모듈 연동

🛠 Phase 2: 위치 특정 및 알림 (Smart)  
* [ ] AR 위치 마킹: 카메라 뷰에서 특정 좌표를 찍어 물리적 위치를 저장하는 기능.  
* [ ] 스마트 알림: 유통기한 임박 아이템 추출 및 로컬 푸시 알림 로직 구현.  
* [ ] 통합 검색: 이름/카테고리/위치 기반의 전역 검색 엔진 개발.

🛠 Phase 3: 서버 모드 및 연동 (Sync)  
* [ ] 서버 모드 활성화: 마스터 폰에서 FastAPI 서버를 백그라운드로 실행.  
* [ ] 기기 간 인증: PIN 번호 또는 QR 인증을 통한 클라이언트 접속 허가.  
* [ ] 실시간 동기화: 로컬 네트워크 내 데이터 수신 및 반영.  

