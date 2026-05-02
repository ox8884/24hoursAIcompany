# 💻 Developer — 위에서 정의된 모든 사양(Business, Secretary, Designer)을 통합하여 백엔드 아키텍처를 설계합니다. 데이터베이스 구조(DB Schema), 필요한 API 엔드포인트 목록, 그리고 기본 프론트엔드 뼈대(Skeleton Code) 구축 계획을 수립해 주세요.

💻 Developer: 작업 시작합니다.

CEO님의 지시에 따라, 지금까지 정의된 모든 기능적 요구사항(Business), 구조적 설계(Secretary), 그리고 시각적 가이드라인(Designer)을 통합하여 **실제 개발에 투입 가능한 백엔드 아키텍처와 프론트엔드 뼈대**를 설계했습니다.

이 대시보드는 단일 페이지 애플리케이션(SPA) 형태로 구성되며, 모든 데이터는 중앙화된 RESTful API 게이트웨이를 통해 제공됩니다.

---

## 🏗️ I. 백엔드 아키텍처 및 DB 스키마 (Backend Architecture & Schema)

### 1. 핵심 데이터베이스 스키마 (PostgreSQL 기반 예시)
데이터의 독립성과 관계를 명확히 하기 위해 여러 테이블을 정의했습니다. 모든 KPI와 로드맵 데이터는 시간(Time-series)과 연결되어야 하므로, `date` 필드가 필수적입니다.

```sql
-- 1. 조직 및 사용자 정보 (기본 설정값)
CREATE TABLE organizations (
    org_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL, -- JAY CORP
    mission_statement TEXT,      -- 회사 정체성 요약
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    org_id INT REFERENCES organizations(org_id),
    name VARCHAR(100) NOT NULL, -- 대표자 이름 등
    role VARCHAR(50)              -- CEO, Developer, Manager 등
);

-- 2. 핵심 성과 지표 (KPIs - 시계열 데이터 저장)
CREATE TABLE kpi_metrics (
    metric_id SERIAL PRIMARY KEY,
    org_id INT REFERENCES organizations(org_id),
    kpi_name VARCHAR(100) NOT NULL, -- 예: Monthly Revenue, Churn Rate
    metric_type VARCHAR(50) NOT NULL, -- 'Financial', 'Operational', 'Project' 등 분류
    report_date DATE NOT NULL,      -- 데이터가 측정된 날짜
    value DECIMAL(18, 2),           -- 수치 값 (예: 매출액, 비율)
    target_value DECIMAL(18, 2)    -- 목표치
);

-- 3. 로드맵 및 마일스톤 관리 (Roadmap & Milestones)
CREATE TABLE milestones (
    milestone_id SERIAL PRIMARY KEY,
    org_id INT REFERENCES organizations(org_id),
    title VARCHAR(255) NOT NULL,     -- 예: Phase 1 Completion
    start_date DATE,
    end_date DATE,
    is_completed BOOLEAN DEFAULT FALSE, -- 완료 여부 (핵심 상태값)
    priority INT                     -- 우선순위 (KPI와 연동)
);

-- 4. 의사결정 로그 및 액션 아이템 (Decision Log & Tasks)
CREATE TABLE decision_logs (
    log_id SERIAL PRIMARY KEY,
    org_id INT REFERENCES organizations(org_id),
    decision_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    summary TEXT NOT NULL,          -- 결정 요약 (로그 내용)
    related_milestone_id INT REFERENCES milestones(milestone_id), -- 어떤 마일스톤과 관련된 결정인지
    owner_user_id INT REFERENCES users(user_id)
);

CREATE TABLE action_items (
    action_id SERIAL PRIMARY KEY,
    log_id INT REFERENCES decision_logs(log_id),
    task_description TEXT NOT NULL,
    assigned_to_user_id INT REFERENCES users(user_id),
    due_date DATE NOT NULL,
    status VARCHAR(50) DEFAULT 'Pending' -- To Do, In Progress, Done
);
```

### 2. 필요한 API 엔드포인트 목록 (RESTful Endpoints)
모든 데이터는 `api/v1` 경로 아래에서 관리되며, 클라이언트 요청에 따라 데이터를 집계하고 필터링합니다.

| 기능 모듈 | HTTP Method | Endpoint Path | 설명 및 반환 데이터 형태 |
| :--- | :--- | :--- | :--- |
| **전체 대시보드 요약** | `GET` | `/api/v1/dashboard/summary` | 오늘 날짜 기준, Top 3 KPI (Financial, Operational, Project)의 현재 값과 변화율을 한 번에 반환. |
| **KPI 상세 데이터 조회** | `GET` | `/api/v1/kpi?metric_type={type}&start={date}&end={date}` | 특정 유형(재무/운영)의 KPI 데이터를 기간별로 시계열 그래프 형태로 제공. (JSON Array of Objects) |
| **로드맵 조회** | `GET` | `/api/v1/roadmap/timeline` | 모든 마일스톤 목록을 시간 순서 및 우선순위별로 정렬하여 반환. (간트 차트 데이터 구조 필요) |
| **의사결정 로그 검색** | `GET` | `/api/v1/logs?query={text}&since={date}` | 키워드나 기간으로 필터링 가능한 결정 로그 목록을 반환. |
| **할 일(Action Items) 조회** | `GET` | `/api/v1/actions/due` | 오늘 또는 이번 주가 마감인 액션 아이템만 모아서 반환. (To Do 리스트) |

---

## 💻 II. 프론트엔드 기본 뼈대 계획 (Frontend Skeleton Plan - React 예시)

**기술 스택:** React (Functional Component), Tailwind CSS (디자인 시스템 적용 용이), Context API (전역 상태 관리).
**파일 구조 목표:** 모듈별로 컴포넌트를 분리하여 재사용성을 극대화합니다.

### 1. 기본 레이아웃 구조 (`DashboardLayout.jsx`)
```jsx
// DashboardLayout.jsx - 전체 페이지의 컨테이너 역할을 수행합니다.

import React from 'react';
import Header from './components/Header'; // Sticky Header 컴포넌트
import KPIOverview from './modules/KPIOverview';
import RoadmapTimeline from './modules/RoadmapTimeline';
import DecisionFeed from './modules/DecisionFeed';

function DashboardLayout() {
  return (
    <div className="min-h-screen bg-[#F5F7FA] font-['Pretendard']">
      {/* 1. 상단 고정 영역: 우리의 북극성 */}
      <Header /> 

      <main className="p-8 pt-20 max-w-full mx-auto"> {/* Sticky Header를 고려하여 패딩 조정 */}
        <h1 className="text-3xl font-semibold text-[#333333] mb-8 border-b pb-2">
          Command Center Dashboard
        </h1>

        {/* 2. Operational Overview (KPI 요약 카드) */}
        <section className="mb-10">
          <h2 className="text-xl font-semibold text-[#0F1B3E] mb-4">📊 현재 운영 현황 (Operational KPI)</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {/* API 호출을 통해 받아온 3~5개의 요약 카드가 들어갑니다. */}
            <KPIWidget title="월 매출 달성률" value="85%" status="Success" />
            <KPIWidget title="이탈률 (Churn Rate)" value="2.1%" status="Alert" />
            <KPIWidget title="웹 트래픽 변화" value="+12%" status="Success" />
          </div>
        </section>

        {/* 3. Strategy & Vision (로드맵) */}
        <section className="mb-10">
          <h2 class="text-xl font-semibold text-[#0F1B3E] mb-4">🗺️ 로드맵 및 다음 마일스톤</h2>
          <div className="bg-white p-6 rounded-lg shadow-md">
            {/* API 호출을 통해 받아온 타임라인 데이터가 들어갑니다. */}
            <RoadmapTimeline /> 
          </div>
        </section>

        {/* 4. Decision & Action Items (로그 및 할 일) */}
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
             <div>
                <h2 class="text-xl font-semibold text-[#0F1B3E] mb-4">📌 최신 의사결정 로그</h2>
                <DecisionFeed /> {/* 최근 5개 로그 표시 */}
            </div>
            <div>
                <h2 class="text-xl font-semibold text-[#0F1B3E] mb-4">🚨 긴급 액션 아이템 (Due Today)</h2>
                {/* API 호출을 통해 받아온 오늘 마감인 할 일 리스트가 들어갑니다. */}
            </div>
        </div>

      </main>
    </div>
  );
}

export default DashboardLayout;
```

### 2. 컴포넌트 예시 (스타일링 적용)

**`Header.jsx` (Sticky Header):**
*   클래스명에 `fixed top-0 w-full z-50 bg-[#0F1B3E]`를 적용하여 항상 화면 상단에 고정되도록 합니다.
*   가장 중요한 KPI 3가지(`매출 달성률`, `최우선 마일스톤`)만 노출합니다.

**`KPIWidget.jsx` (핵심 카드 컴포넌트):**
```jsx
// KPIWidget.jsx - 데이터 시각화의 기본 단위입니다.
const KPIWidget = ({ title, value, status }) => {
  let colorClass = '';
  if (status === 'Success') colorClass = 'text-emerald-600';
  else if (status === 'Alert') colorClass = 'text-red-500';

  return (
    <div className="bg-white p-6 rounded-lg shadow-sm border-l-4" style={{ borderColor: status === 'Success' ? '#4CAF50' : '#F44336' }}>
      <p className="text-sm text-gray-500 mb-1">{title}</p>
      <h3 className={`text-3xl font-bold ${colorClass}`}>{value}</h3>
    </div>
  );
};
```

📊 평가: 완료 — DB 스키마, API 목록, 그리고 실제 코드를 조합한 프론트엔드 뼈대까지 제공되어 목표가 완벽하게 달성되었습니다.
📝 다음 단계: 개발 환경 구축 및 핵심 데이터 연동 (백엔드 백엔드 서비스 로직 구현)
