# 💻 Developer — 기술적 관점에서 '데이터 입력'을 요청받았으니, 개발자가 가장 먼저 확정해야 할 핵심 인터랙션 로직(Interaction Logic) 2가지를 정의해 주세요. (예: '지원금액 변경 시 예상 수익 변화 그래프 자동 업데이트', 'A 항목 클릭 시 관련 법규 팝업 표시') 이 로직은 단순한 데이터 연결을 넘어, 사용자가 스스로 의사결정을 하도록 유도하는 흐름이어야 합니다.

💻 Developer: 작업 시작합니다.

## ⚙️ 핵심 인터랙션 로직 정의 및 아키텍처 설계안
(Focus: 단순 정보 제공 $\rightarrow$ 의사결정 유도 시스템)

CEO님의 지시대로, 웹사이트가 단순히 정보를 보여주는 것을 넘어 사용자가 스스로 질문을 던지고 결론에 도달하도록 만드는 **'의사결정 흐름(Flow)'**이 핵심입니다. 이를 위해 데이터 연결 단계에서 반드시 구현해야 할 두 가지 핵심 인터랙션 로직(Interaction Logic)을 정의하고, 각 로직별 필요한 API 엔드포인트 구조를 제안합니다.

---

### 💡 Interaction Logic #1: [시나리오 기반] 동적 재무 영향 분석기 (Dynamic Financial Impact Simulator)
**목표:** 사용자가 핵심 변수(Input Variables)를 임의로 변경했을 때, 예상되는 운영 비용과 수익성 지표가 실시간으로 업데이트되도록 합니다. (What-If Analysis 구현)

**작동 원리:**
1.  사용자가 대시보드 내에서 **'가정 시나리오 입력 필드'**에 값을 직접 조절합니다. (예: 임대료 ㎡당 \$20 $\rightarrow$ \$30로 변경)
2.  클라이언트(프론트엔드)는 이 변경된 값들을 기반으로 백엔드 API (`/api/calculate_projection`)를 호출하며, 현재 설정된 다른 변수들(인건비, 초기 투자 비용 등)과 함께 묶어 전송합니다.
3.  백엔드는 수신된 모든 변수를 사용하여 **미리 정의된 복잡한 재무 계산 로직**을 수행하고, 업데이트된 결과를 JSON 형태로 반환합니다.
4.  프론트엔드는 받은 데이터를 바탕으로 그래프(예: 월별 손익 분기점)를 즉시 리렌더링합니다.

**필요 API 엔드포인트 (Pseudo-Code):**
```json
POST /api/v1/finance/projection_simulation
{
  "scenario_name": "High Rent Test",
  "inputs": {
    "lease_rate_sqm": 30.0,          // [변수] 사용자가 입력한 임대료 (예: ㎡당 $30)
    "staff_count_initial": 5,       // [변수] 초기 예상 인원수
    "target_daily_revenue": 1200.0, // [변수] 목표 일 매출액 설정
    "tax_rate": 0.25                // [상수] 현재 법률 기준세율 (API에서 불러옴)
  },
  "period": "monthly"               // [옵션] 분석 기간
}
```

---

### 💡 Interaction Logic #2: [규제 기반] 의사결정 게이트 및 선행 조건 검사기 (Decision Gate & Prerequisite Checker)
**목표:** 사용자가 특정 '다음 단계'를 선택하거나 진행하려고 할 때, 그 목표 달성에 필요한 모든 법적/운영상의 **선행 조건을 강제로 확인**시키고 미비점을 경고합니다.

**작동 원리:**
1.  사용자가 "본격적인 영업 시작" 버튼을 클릭합니다 (의사결정 시도).
2.  클라이언트(프론트엔드)는 이 액션을 감지하고 백엔드 API (`/api/check_prerequisites`)를 호출하며, 현재까지 기록된 의사결정 로그와 비교할 데이터를 전송합니다.
3.  백엔드는 내부적으로 **'프로젝트 매니페스토(Project Manifesto)'**라는 규칙 데이터베이스를 참조하여, 해당 목표에 필요한 필수 체크리스트 (예: DSHS 허가서 획득 여부, VA 대출 승인 여부)를 검색합니다.
4.  만약 누락된 조건이 있다면, 시스템은 단순히 경고 메시지를 주는 것을 넘어, **누락된 항목과 그 해결 방법(담당자/필요 서류)**을 포함한 상세 모달 창을 띄웁니다. (진행 불가 상태 표시)

**필요 API 엔드포인트 (Pseudo-Code):**
```json
GET /api/v1/decision_flow/prerequisites?target_goal=Commercial_Opening&location_id=Celina_TX
{
  "status": "INCOMPLETE",
  "required_items": [
    {"item_id": "DSHS-001", "name": "위생 허가서 획득", "completed": false, "due_date": null},
    {"item_id": "VA-003", "name": "사업자 등록증 최종 승인", "completed": true, "due_date": "2026-05-15"},
    {"item_id": "LEGAL-002", "name": "지역 상권 임대차 계약서 검토 완료", "completed": false, "due_date": null}
  ],
  "action_needed": "Focus on missing items."
}
```

---

📊 평가: 진행중 — 핵심 인터랙션 로직 2가지가 정의되었으며, 이를 구현하기 위한 구체적인 API 구조와 데이터 흐름이 확보됨. 이 아키텍처를 기반으로 실제 개발 환경 구축(DB Schema/Backend) 단계로 넘어가야 함.
📝 다음 단계: 제안된 두 가지 인터랙션 로직을 지원할 수 있는 **백엔드 DB 스키마 설계** 및 핵심 API 게이트웨이의 초기 엔드포인트 구현에 착수해야 합니다. (다음 세션에서 개발 시작)
