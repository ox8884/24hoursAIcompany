# 🎨 Designer — 위에서 정의된 정보 아키텍처(Secretary)와 비즈니스 요구사항(Business)을 바탕으로, 전문적이고 신뢰감 있는 브랜드 톤에 맞는 웹사이트의 메인 페이지 및 모듈별 UI/UX 디자인 시안(색상 팔레트, 타이포그래피 포함)을 제작해 주세요.

🎨 Designer: 작업 시작합니다.

요청하신 '의사결정 지원 대시보드'는 단순한 웹페이지를 넘어 **조직의 기억(Institutional Memory)**이자 **실시간 지휘 센터(Command Center)** 역할을 해야 합니다. 따라서 디자인은 트렌디함보다는 신뢰성, 가독성, 그리고 정보 구조에 초점을 맞춰야 합니다.

전문적인 컨설팅/금융 SaaS 플랫폼에서 볼 수 있는 '데이터 기반', '미니멀하지만 기능이 명확한' 톤앤매너를 적용하여 UI/UX 시안을 구성했습니다.

---

## 🎨 JAY CORP 대시보드 디자인 시스템 (Design System) 정의

### 1. 컬러 팔레트 (Color Palette)
| 용도 | 색상명 | HEX 코드 | RGB 값 | 설명 및 활용처 |
| :--- | :--- | :--- | :--- | :--- |
| **Primary** (핵심) | Deep Navy Blue | `#0F1B3E` | (15, 27, 62) | 배경(Dark Mode 옵션), 헤더 바, 핵심 로고. **신뢰성/권위** 부여. |
| **Secondary** (강조) | Electric Teal | `#4A90E2` | (74, 144, 226) | KPI 상승 화살표, 액션 버튼(CTA), 현재 포커스 모듈 하이라이트. **집중/행동 유도**. |
| **Accent** (보조) | Muted Gray | `#B0BEC5` | (176, 190, 197) | 구분선(Divider), 비활성화된 버튼, 경고 표시 배경. |
| **Background** (기본) | Soft White | `#F5F7FA` | (245, 247, 250) | 메인 콘텐츠 영역 배경. 눈의 피로도를 낮추는 색상. |
| **Text/Primary** | Charcoal Gray | `#333333` | (51, 51, 51) | 본문 텍스트, 제목. 순수 검정(`\#000000`)보다 부드러움. |
| **Status: Success** | Emerald Green | `#4CAF50` | (76, 175, 80) | 목표 달성, 긍정적인 KPI 변화. |
| **Status: Alert** | Warning Red | `#F44336` | (244, 67, 54) | 위험 경고, 실패한 마일스톤, 즉각적 수정 필요 항목. |

### 2. 타이포그래피 (Typography)
*   **시스템:** 산세리프(Sans-serif). 가독성이 최우선입니다.
*   **메인 서체 (Primary Font):** Pretendard / Inter (웹폰트 권장)
    *   *(이유: 다양한 크기와 무게에서 균일하고 깔끔한 느낌을 주며, 데이터 대시보드에 가장 적합합니다.)*
*   **제목(H1-H3) 크기 및 굵기:**
    *   H1 (대시보드 제목): Pretendard Bold, 28px, `#333333`
    *   H2 (섹션 제목): Pretendard SemiBold, 20px, Primary Color (`#0F1B3E`)
    *   Body Text: Pretendard Regular, 14px, `#333333`

### 3. UI/UX 핵심 컴포넌트 (Components)
1.  **Card:** 모든 정보 블록은 `Soft White` 배경의 직사각형 카드로 분리합니다. (미묘한 그림자: `box-shadow: 0 2px 4px rgba(0,0,0,0.05);`)
2.  **KPI Gauge/Metric:** 숫자와 변화율을 명확히 분리하고, 변화 방향에 따라 Status Color를 적용합니다. (예: $1.2M <span style="color:#4CAF50;">▲+8%</span>)
3.  **Timeline:** 점(Dot)과 연결선(Line)으로 구성된 간트 차트 형태를 사용하며, 마일스톤의 중요도에 따라 원의 크기를 다르게 합니다.

---

## 💻 JAY CORP 대시보드 메인 페이지 시안 (The Command Center View)

**(전체 레이아웃: Clean White Background (`#F5F7FA`) 위에서 Deep Navy Blue(`\#0F1B3E`)와 Electric Teal(`\#4A90E2`)가 대비되도록 설계)**

### 🧭 [Sticky Header/Status Bar]
*   **디자인:** `Deep Navy Blue` 배경. 모든 정보는 흰색 또는 밝은 회색 텍스트로 배치하여 명도 대비를 극대화합니다.
*   **요소:**
    1.  **(좌측)** 로고 + 회사명 (흰색).
    2.  **(중앙)** 현재 모드 및 대표자 상태 표시: `CEO Focus: [Roadmap Phase 2 - Legal Structuring]` (Electric Teal 하이라이트 박스).
    3.  **(우측)** 알림 아이콘, 사용자 프로필 사진 (미니멀한 아이콘 중심).

### 📈 Section I: KPI 요약 대시보드 (Top Metrics)
*   **배치:** 4개의 등폭 카드(Grid Layout), 최상단에 배치하여 시선이 바로 가도록 합니다.
*   **컨셉:** 각 카드는 제목, 핵심 수치, 변화율을 담습니다.
    *   **[카드 예시: 매출 목표 달성률]**
        *   **(제목):** Monthly Revenue Target (H3)
        *   **(수치):** `$1.2M` (Pretendard Bold, 36px)
        *   **(변화율):** <span style="color:#4CAF50;">▲ +8%</span> vs Last Month (Electric Teal/Green 강조)

### 🗺️ Section II: 핵심 로드맵 및 액션 플랜 (The Journey Map)
*   **디자인:** 중앙을 가로지르는 시간 흐름(Time Axis)의 간트 차트 형태.
*   **컨셉:** 목표는 큰 원으로, 현재 주간 할 일은 작은 사각형 박스로 명확히 구분합니다.
    *   **(Visualization):** 연도 → 분기 → 월 순서로 흐르는 라인.
    *   **(Focus Point):** 가장 가까운 마일스톤(예: '법적 구조 확정')에 `Electric Teal`의 테두리를 적용하고, 이 모듈 아래에 상세한 **Task Checkbox List**를 배치합니다.

### 💰 Section III: 재무 및 리스크 트래커 (Financial & Risk Tracker)
*   **배치:** 좌우 분할 레이아웃 (왼쪽: 그래프 / 오른쪽: 목록).
*   **좌측 (그래프):** LTV/CAC 추이 그래프 (라인 차트). `Deep Navy Blue` 배경에, 목표선은 점선(`Muted Gray`)으로 표시합니다.
*   **우측 (리스크 로그):** 'Critical Alert' 리스트를 상단에 배치하고, 위험도(Red/Yellow) 태그를 사용합니다.

---
📊 평가: 완료 — 모든 모듈별 디자인 토큰(색상 코드, 폰트명, 레이아웃 구조)을 구체적으로 정의하여 즉시 개발 가능한 수준의 결과물을 도출했습니다.
📝 다음 단계: 이 디자인 시스템 기반으로 실제 와이어프레임(Figma 또는 Sketch 파일 형태)을 제작하고, 각 모듈에 들어갈 예시 데이터(Dummy Data)를 채워 넣는 작업을 진행해야 합니다.
