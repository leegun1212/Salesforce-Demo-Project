# Salesforce-Demo-Project









# Space Data Modeling 

- **Topic:** Custom Object (`Space__c`) vs. Standard Object (`Asset`)

---

## 1. 배경 및 문제 정의
* **요구사항:** 공유오피스 비즈니스 모델을 위한 **지점(Branch) - 층(Floor) - 호실(Room)**의 공간 정보 관리 필요.
* **비지니스 요구사항:** 단순한 공간 정보 저장을 넘어, 향후 해당 공간에서 발생하는 **시설 유지보수(Maintenance), 수리(Repair), 고객 민원(VoC)** 프로세스와의 유기적 연결이 필수적임.
* **문제 상황:** 초기 ERD 설계 단계에서는 `Space`라는 임의의 Custom Table로 구상함.

## 2. 검토 대안

### Option A: Custom Object (`Space__c`) 생성
* **장점:** 'Space'라는 직관적인 명칭 사용 가능. 필드 커스터마이징의 자유도 높음.
* **단점:**
    * `Case`나 `WorkOrder`와 연결하기 위해 별도의 Lookup 관계 설정 필요.
    * 계층 구조(Hierarchy) 구현을 위해 Self-lookup 로직을 별도로 개발해야 함.
    * 향후 Field Service(FSL) 확장 시 표준 기능과의 호환성이 떨어짐.

### Option B: Standard Object (`Asset`) 활용 
* **장점:** Salesforce Service Cloud의 표준 프로세스와 강력하게 결합됨.
* **단점:** 본래 '자산' 관리 목적이므로, '공간'으로 개념을 확장하여 사용하는 데 대한 팀원 설득 필요.

## 3.의사결정 및 근거

** 결정: Standard Object인 `Asset`을 사용하기로 결정함.**

**핵심 근거 :**

1.  **Service Cloud 프로세스 최적화 (Native Integration)**
    *   비즈니스의 핵심인 '시설물 수리 및 민원 관리'를 위해서는 `Case` 및 `WorkOrder`와의 연동이 필수적.
    *   `Asset`은 이미 이들과 **Native Relationship**으로 연결되어 있어, 별도의 관계 설정 개발 없이도 수리 이력 추적(History Tracking)이 가능함.

2.  **계층형 구조(Hierarchy) 기본 지원**
    *   **[지점 -> 층 -> 호실]**로 이어지는 트리 구조가 필요함.
    *   `Asset`은 자체적인 **'Asset Hierarchy'** 기능을 표준으로 제공함. 복잡한 개발 없이도 공간의 위계 구조를 시각화하고 관리할 수 있음.

3.  **확장성 및 기술 부채 최소화**
    *   향후 현장 수리 기사를 위한 **Field Service** 도입 시, `Asset`을 기준으로 작업 지시(Work Order)가 생성되므로 확장성에 매우 유리함.
    *   표준 기능을 사용함으로써 향후 플랫폼 업데이트 시 발생할 수 있는 호환성 문제(Technical Debt)를 사전에 차단함.

## 4. References
*   [Salesforce Help: What is an Asset?](https://help.salesforce.com/s/articleView?id=sf.assets_def.htm&type=5)
