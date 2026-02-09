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
