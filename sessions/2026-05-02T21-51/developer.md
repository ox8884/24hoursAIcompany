# 💻 Developer — 실제 HTML 코드를 작성하되, 이 코드는 '뼈대(Scaffold)' 역할만 수행하도록 한다. 데이터가 들어갈 위치(예: 사용자 프로필, 메뉴 정보)에는 실제 데이터를 대신하는 플레이스홀더 주석(<!-- API Placeholder: ... -->)을 명확히 삽입하여, 이 초안이 아직 동적이지 않은 구조체임을 시각적으로 증명해야 한다.

💻 Developer: 작업 시작합니다.

CEO님의 지시를 반영하여, 비록 정적인 '뼈대(Scaffold)' 코드이지만, 시스템의 구조와 데이터 흐름을 시각적으로 이해할 수 있는 `index.html` 초안을 작성했습니다.

이 코드는 **단순히 레이아웃과 섹션 구분**만을 위한 것이며, 말씀해주신 대로 모든 동적 콘텐츠 영역에는 해당 데이터를 가져올 API 엔드포인트가 필요함을 명확하게 표시하는 주석(Placeholder)을 삽입하였습니다. 이 구조체는 백엔드에서 데이터가 채워질 '빈 캔버스' 역할을 수행합니다.

다음은 `index.html` 파일의 전체 내용입니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Celina 창업 플랜: 의사결정 지원 대시보드 (Scaffold)</title>
    <style>
        /* 기본 스타일링 - 가독성 위주로 최소화 */
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; padding: 0; background-color: #f4f7fa; color: #333; }
        header { background-color: #2c3e50; color: white; padding: 20px 40px; display: flex; justify-content: space-between; align-items: center; }
        header h1 { margin: 0; font-size: 1.8em; }
        nav a { color: #ecf0f1; text-decoration: none; margin-left: 25px; font-weight: bold; transition: color 0.3s; }
        nav a:hover { color: #3498db; }
        .container { padding: 30px 40px; max-width: 1400px; margin: 20px auto; }
        h2 { border-bottom: 2px solid #3498db; padding-bottom: 5px; color: #2c3e50; margin-top: 40px; }

        /* 섹션 레이아웃 */
        .grid-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; margin-bottom: 30px; }
        .card { background-color: white; padding: 25px; border-radius: 8px; box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08); border-left: 5px solid #3498db; }
        .placeholder { color: #aaa; font-style: italic; background-color: #f9f9f9; padding: 10px; border-radius: 5px; margin-top: 10px;}

        /* 로그 테이블 스타일 */
        #decision-log table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        #decision-log th, #decision-log td { padding: 12px; text-align: left; border-bottom: 1px solid #eee; }
        #decision-log th { background-color: #ecf0f1; color: #34495e; }

        /* 다음 액션 플랜 (가장 중요) */
        #next-action-plan .card { border-left-color: #2ecc71; box-shadow: 0 4px 10px rgba(46, 204, 113, 0.2); }
    </style>
</head>
<body>

    <!-- ======================================================= -->
    <!-- HEADER (브랜딩 및 네비게이션) -->
    <!-- ======================================================= -->
    <header>
        <h1>Celina 창업 플랜 관리 시스템</h1>
        <nav>
            <a href="#dashboard">대시보드</a>
            <a href="#goals">목표 현황</a>
            <a href="#log">의사결정 로그</a>
            <a href="#action-plan">다음 액션</a>
        </nav>
    </header>

    <!-- ======================================================= -->
    <!-- MAIN CONTAINER (핵심 대시보드) -->
    <!-- ======================================================= -->
    <div class="container">

        <!-- 1. 핵심 지표 및 요약 섹션 (KPI Dashboard) -->
        <section id="dashboard">
            <h2>📊 종합 현황 대시보드</h2>
            <p>현재 프로젝트의 가장 중요한 핵심 성과 지표(KPI)를 한눈에 보여줍니다.</p>

            <div class="grid-container">
                <!-- KPI Card 1 -->
                <div class="card">
                    <h3>🔥 목표 달성률</h3>
                    <div class="placeholder"><!-- API Placeholder: 공동 목표 대비 현재 진행 상황 (%) 및 그래프 데이터 --><br>예시: 45% (진행 중)</div>
                    <p style="font-size: 0.9em; color: #7f8c8d;">[데이터 소스: Company Goals API]</p>
                </div>

                 <!-- KPI Card 2 -->
                <div class="card">
                    <h3>💰 예상 매출 흐름</h3>
                    <div class="placeholder"><!-- API Placeholder: 월별/분기별 재무 예측 데이터 (차트 필요) --><br>최근 추이 분석 결과가 들어갑니다.</div>
                    <p style="font-size: 0.9em; color: #7f8c8d;">[데이터 소스: Financial Model API] </p>
                </div>

                 <!-- KPI Card 3 -->
                <div class="card">
                    <h3>✅ 핵심 마일스톤</h3>
                    <div class="placeholder"><!-- API Placeholder: 다음 달까지 반드시 완료해야 할 필수 작업 목록 (To-Do List) --><br>우선순위 순으로 정렬됩니다.</div>
                    <p style="font-size: 0.9em; color: #7f8c8d;">[데이터 소스: Task Management API] </p>
                </div>
            </div>
        </section>

        <!-- 2. 다음 액션 플랜 엔진 (Decision Support System Core) -->
        <section id="next-action-plan">
            <h2>🚀 최적의 다음 행동 제안 (Next Action Plan Engine)</h2>
            <p>현재 상황을 진단하고, 공동 목표 달성을 위해 가장 효과적인 '다음 스텝'을 AI가 자동으로 추천합니다.</p>

            <div class="card" style="margin-top: 20px;">
                <h4>💡 시스템 분석 결과</h4>
                <p class="placeholder"><!-- API Placeholder: 현재 상태 진단 요약 (예: "법인 설립 서류 검토가 가장 시급함") --> </p>
                
                <h4>🎯 추천 액션 플랜</h4>
                <div style="background-color: #e6f7ff; padding: 15px; border-radius: 5px;">
                    <strong>[추천 내용]</strong> SBA/VA 컨설턴트와 만나 법인 형태 및 공식 비즈니스 플랜 최종 확정. (로그 [2026-05-02])<br>
                    <strong>[필요 리소스]</strong> 계약서 초안, 예상 자본금 데이터.<br>
                    <!-- API Placeholder: 추천 액션의 상세 가이드라인 및 필요한 문서 목록 -->
                </div>

                <h4>🗓️ 실행 기한</h4>
                <div class="placeholder"><!-- API Placeholder: 해당 액션을 위한 권장 시작일과 종료일 --><br>예시: 2026년 5월 10일 ~ 5월 17일 --></div>
            </div>
        </section>


        <!-- 3. 의사결정 로그 (Decision Log) -->
        <section id="log">
            <h2>📜 회사 주요 의사결정 및 학습 기록</h2>
            <p>시간 순서대로 누적되는 모든 중요한 결정과 회의 내용을 확인하여, 비즈니스의 신뢰도와 일관성을 유지합니다.</p>

            <div id="decision-log">
                <table>
                    <thead>
                        <tr>
                            <th>일시</th>
                            <th>주제/결정 내용 요약</th>
                            <th>핵심 결정 사항</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- API Placeholder: 최신 의사결정 로그 5개 목록 -->
                        <tr>
                            <td class="placeholder">2026-05-02</td>
                            <td class="placeholder">웹사이트 구축 로드맵 수립 및 아키텍처 설계 확정.</td>
                            <td>의사결정 지원 시스템(DSS) 역할 정의.</td>
                        </tr>
                        <tr>
                            <td class="placeholder">2026-05-02</td>
                            <td class="placeholder">기술 구현 난이도 설명 및 MVP 범위 제한 합의.</td>
                            <td>핵심 데이터 소스 3가지 연동 우선순위 확정.</td>
                        </tr>
                        <!-- ... 더 많은 로그가 API를 통해 동적으로 로드됩니다. -->
                    </tbody>
                </table>
            </div>
        </section>

        <!-- 4. 공동 목표 및 KPI (Goal Tracking) -->
        <section id="goals">
            <h2>🎯 장기/단기 공동 목표 추적</h2>
            <p>현재 진행하는 모든 작업이 궁극적인 비전과 어떻게 연결되는지 추적합니다.</p>

             <div class="grid-container">
                <!-- Goal Card 1: 장기 목표 -->
                <div class="card" style="border-left-color: #e74c3c;">
                    <h3>📈 장기 비전 (1년)</h3>
                    <ul>
                        <li>[공동 목표 API Placeholder]: 유튜브 구독자 10만 달성</li>
                        <li>[공동 목표 API Placeholder]: 월 수익 500만원 달성</li>
                    </ul>
                </div>
                 <!-- Goal Card 2: 단기 목표 -->
                <div class="card" style="border-left-color: #f39c12;">
                    <h3>🗓️ 단기 실행 계획 (1개월)</h3>
                    <ul>
                        <li>[공동 목표 API Placeholder]: 영상 4개 업로드</li>
                        <li>[공동 목표 API Placeholder]: 릴스 12개 게시</li>
                    </ul>
                </div>
             </div>
        </section>

    </div> <!-- /.container -->


</body>
</html>
```

📊 평가: 완료 — CEO가 요청한 '뼈대' 역할의 HTML 스캐폴드를 완성했으며, 모든 동적 영역에 플레이스홀더 주석을 명확하게 삽입하여 요구사항을 충족했습니다.
📝 다음 단계: 이 구조를 기반으로 실제 데이터 연동(백엔드/API) 설계를 진행하거나, 특정 섹션(예: 의사결정 로그 테이블)의 상세 UI 컴포넌트 디자인을 착수해야 합니다.
