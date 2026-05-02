# 💻 Developer — 디자이너가 만든 와이어프레임 컨셉을 기반으로, 실제로 작동하는 MVP(Minimum Viable Prototype)를 위한 기술 스택과 API 연동 우선순위를 정의하세요. 특히 데이터는 단순 정보 나열이 아닌 '상호작용적 차트' 형태로 제공되어야 하며, 핵심 변수 변화에 따라 인터랙티브하게 값이 바뀌도록 구현 계획을 수립해주세요.

💻 Developer: 작업 시작합니다.

## ⚙️ Jaywave DSS MVP 구축 계획 (Technical Architecture Proposal)

CEO님의 지시에 따라, Designer가 제시한 와이어프레임 컨셉을 실제 작동하는 MVP(Minimum Viable Prototype)로 전환하기 위한 기술 스택, 데이터 모델, 그리고 핵심 인터랙션 로직의 구현 계획을 수립했습니다. 단순 정보 나열이 아닌, **'변수에 반응하여 시각화되는 의사결정 도구'**를 목표로 합니다.

---

### 1. 💻 추천 기술 스택 (Technology Stack)

| 영역 | 선택 기술 | 이유 및 역할 |
| :--- | :--- | :--- |
| **Frontend (Client)** | **Next.js (React)** | 최신 웹 개발 트렌드에 적합하며, 서버 측 렌더링(SSR)과 클라이언트 사이드 상호작용을 모두 처리할 수 있어 동적 차트 구현에 유리합니다. |
| **Backend (API/Logic)** | **Node.js (Express Framework)** | JavaScript 기반으로 프론트엔드와 언어 통일성이 높아 개발 속도가 빠릅니다. 복잡한 비즈니스 로직(금융 시뮬레이션)을 처리하는 API 게이트웨이 역할을 합니다. |
| **Database** | **PostgreSQL** | 구조화된 데이터(법규, 금융 수치, 시장 지표 등)를 저장하고, 트랜잭션 무결성이 중요하므로 가장 안정적입니다. |
| **Charting/Visualization** | **Recharts / Chart.js** | React 환경에서 사용하기 쉽고, 상호작용성(Tooltip, Hover effects)이 높은 차트 라이브러리입니다. |

### 2. 🌐 핵심 API 엔드포인트 및 데이터 모델 정의 (API & Schema Design)

시스템의 모든 인터랙티브 요소는 다음 세 가지 핵심 API를 중심으로 데이터를 가져오고 처리합니다.

#### A. `GET /api/status/legal-compliance`
*   **기능:** 현재 법적 기반 상태(LLC, S-Corp 등)와 필수 서류 목록을 조회합니다.
*   **데이터 구조 예시 (JSON):**
    ```json
    {
      "current_status": "Pending",
      "best_option": "S-Corp Hybrid Model",
      "required_documents": ["EIN Confirmation", "TX Business License Form"],
      "risk_factors": ["State Tax Changes"]
    }
    ```

#### B. `POST /api/simulation/financial-projection` (⭐가장 중요)
*   **기능:** 사용자가 입력한 핵심 변수(Seed Capital, 예상 매출 증가율, 운영 비용 등)를 받아 미래의 재무 상태를 시뮬레이션하고 결과를 차트 데이터로 반환합니다. **이것이 '상호작용적 차트'의 원천입니다.**
*   **요청 바디 (Request Body):**
    ```json
    {
      "seed_capital": 50000, // [사용자 입력]
      "monthly_overhead": 12000, // [상수 값 혹은 변동 가능]
      "market_growth_rate": 0.05, // [사용자 슬라이더/입력]
      "years_to_project": 3
    }
    ```
*   **응답 바디 (Response Body - Chart Data):**
    ```json
    {
      "projection_period": ["Year 1", "Year 2", "Year 3"],
      "net_profit": [45000, 89000, 150000], // 차트 데이터 1: 수익 변화
      "cash_reserve": [12000, 35000, 75000]  // 차트 데이터 2: 현금 보유량 변화
    }
    ```

#### C. `GET /api/market/competitor-analysis`
*   **기능:** Celina 지역 경쟁사 데이터를 조회하여, 핵심 지표(평균 임차료, 고객 유입 경로 POI)를 비교합니다.
*   **데이터 구조 예시 (JSON):**
    ```json
    [
      {"name": "Cafe A", "avg_rent_sqft": 20, "poi_score": 75},
      // ... 등
    ]
    ```

### 3. ✨ 핵심 상호작용 로직 구현 계획 (Interaction Logic Implementation)

**목표:** 사용자가 특정 변수(예: 시드 자본금 또는 시장 성장률)를 변경할 때, 관련된 모든 차트와 요약 지표가 실시간으로 재계산되어야 합니다.

| 인터랙션 트리거 | 발생 이벤트 | 처리 로직 (Backend/Frontend) | 결과 출력 (Visualization) |
| :--- | :--- | :--- | :--- |
| **자본금 변경** | 사용자가 `seed_capital` 슬라이더 값을 수정하고 '계산' 버튼 클릭. | 1. Frontend가 새 값을 API에 전송 (`POST /api/simulation/...`). <br> 2. Backend는 이 값으로 재무 모델을 실행하여 새로운 연도별 순이익 데이터를 생성. | **[Primary Chart]** 수익성 곡선 전체가 부드럽게 업데이트됨. **[Summary Card]** '최대 생존 기간' 지표가 즉시 변경되어 표시됨. |
| **시장 성장률 변화** | 사용자가 예상 시장 성장률(%)을 수정하고 재계산 실행. | 1. Backend는 이 비율을 반영하여 운영 비용 대비 매출 증가분을 재계산. | **[Secondary Chart]** 총매출액과 현금 흐름의 상승 곡선이 변경됨. **[Textual Alert]** "성장률 X% 달성을 위해 추가적으로 필요한 마케팅 예산 Y달러"라는 경고 팝업 생성. |
| **법적 구조 전환** | 사용자가 법인 형태를 'LLC'에서 'S-Corp'로 변경하고 확인 버튼 클릭. | 1. Frontend가 `GET /api/status/legal-compliance?type=SCORP`를 호출하여 세금 및 보고서 의무에 대한 새로운 데이터를 로드. | **[Fixed Top Bar]** "최적의 법인 구조" 섹션의 설명과 필수 서류 리스트 자체가 동적으로 변경됨. |

### 4. 📜 코드 스캐폴드 (Proof of Concept: Financial Simulation)

가장 핵심적인 '재무 시뮬레이션' 기능을 구현하기 위한 Next.js 컴포넌트와 API 로직의 기본 골격(Scaffold)을 제시합니다. 이는 실제 개발 착수를 위한 기반입니다.

#### A. `pages/dashboard/index.tsx` (Frontend - React Component Skeleton)
```tsx
// 📁 pages/dashboard/index.tsx - Next.js Page Scaffold
import React, { useState } from 'react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip } from 'recharts';

interface FinancialData {
  name: string; // 연도
  netProfit: number;
  cashReserve: number;
}

export default function Dashboard() {
  const [seedCapital, setSeedCapital] = useState(50000);
  const [growthRate, setGrowthRate] = useState(0.05);
  const [chartData, setChartData] = useState<FinancialData[]>([]);

  // 💡 핵심 상호작용 로직: 버튼 클릭 시 API 호출 및 상태 업데이트
  const handleSimulationRun = async () => {
    console.log("Running simulation with:", { seedCapital, growthRate });
    try {
      // TODO: 실제 백엔드 API 엔드포인트로 변경 필요 (예: fetch('/api/simulation/financial-projection', ...))
      const response = await new Promise(resolve => setTimeout(() => resolve([
        { name: 'Year 1', netProfit: 45000, cashReserve: 12000 },
        { name: 'Year 2', netProfit: 89000, cashReserve: 35000 },
        { name: 'Year 3', netProfit: 150000, cashReserve: 75000 },
      ]));
      setChartData(response); // 차트 데이터 업데이트
    } catch (error) {
      console.error("Simulation Failed:", error);
    }
  };

  return (
    <div className="p-8 bg-white shadow-lg rounded-xl">
      <h2 className="text-2xl font-bold mb-6 text-gray-700">💰 재무 예측 시뮬레이션</h2>
      
      {/* 🎯 변수 입력 섹션 (Interactive Controls) */}
      <div className="grid grid-cols-3 gap-4 mb-8 p-4 border rounded-lg bg-gray-50">
        <div>
          <label className="block text-sm font-medium text-gray-700">시드 자본금 ($)</label>
          <input type="number" value={seedCapital} onChange={(e) => setSeedCapital(parseFloat(e.target.value))} className="mt-1 block w-full border p-2 rounded"/>
        </div>
        <div>
          <label className="block text-sm font-medium text-gray-700">예상 성장률 (%)</label>
          {/* TODO: 슬라이더 컴포넌트 사용 권장 */}
          <input type="number" value={growthRate * 100} onChange={(e) => setGrowthRate(parseFloat(e.target.value) / 100)} className="mt-1 block w-full border p-2 rounded"/>
        </div>
        <button onClick={handleSimulationRun} className="col-span-3 bg-blue-600 text-white py-2 rounded hover:bg-blue-700 transition">
          🚀 시뮬레이션 실행 및 재계산 (API 호출)
        </button>
      </div>

      {/* 📈 상호작용적 차트 영역 */}
      <div className="h-[400px] border p-4 rounded-lg bg-gray-50">
        <LineChart width={1000} height={350} data={chartData}>
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis dataKey="name" />
          <YAxis yAxisId="netProfit" orientation="left" domain={[0, 'auto']} />
          <Tooltip formatter={(value) => `$${Math.round(value).toLocaleString()}`}/>
          {/* netProfit에 마우스를 올리면 상세 정보 표시 (상호작용성 구현) */}
          <Line yAxisId="netProfit" type="monotone" dataKey="netProfit" stroke="#8884d8" /> 
          {/* cashReserve도 추가하여 두 개의 데이터 라인 비교 가능하게 설계 */}
        </LineChart>
      </div>

    </div>
  );
}
```

---
📊 평가: 완료 — 기술 스택, API 명세, 상호작용 로직 및 코드 스캐폴드까지 제공되어 MVP 구축의 청사진이 완성되었음.
📝 다음 단계: 백엔드 개발 환경 세팅 및 핵심 시뮬레이션 로직 (Financial Projection) 구현에 착수
