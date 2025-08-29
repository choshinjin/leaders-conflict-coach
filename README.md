   <!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <title>리더 코칭 챗봇 — 리더의 대화, 더 나은 해답으로</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!-- 외부 CSS/폰트 없음: FOUC 근본 차단 -->

  <style>
    /* ========= Base / Reset ========= */
    :root{
      --c-bg:#f7f8fb; --c-surface:#ffffff; --c-primary:#1f2a44; --c-accent:#3b82f6;
      --c-support:#6b7280; --c-border:#e6e8ef; --c-danger:#b91c1c; --c-warn:#d97706; --c-info:#0ea5e9;
      --shadow:0 10px 24px rgba(15,23,42,.06);
    }
    *,*::before,*::after{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, "Apple SD Gothic Neo", "Malgun Gothic", Arial, sans-serif;
      color:#0f172a; background:var(--c-bg);
      text-rendering:optimizeLegibility;
      -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
    }
    img{max-width:100%; display:block}
    button,select,textarea{font:inherit}

    /* ========= Utilities ========= */
    .container{max-width:1100px; margin:0 auto; padding:0 16px}
    .surface{background:var(--c-surface); border:1px solid var(--c-border); border-radius:14px; box-shadow:var(--shadow)}
    .row{display:grid; gap:12px}
    .row-2{grid-template-columns:1fr 1fr}
    .row-5{grid-template-columns:repeat(5,1fr)}
    @media (max-width:900px){ .row-2{grid-template-columns:1fr} .row-5{grid-template-columns:repeat(2,1fr)} }
    .px-16{padding-left:16px; padding-right:16px}
    .py-8{padding-top:8px; padding-bottom:8px}
    .py-12{padding-top:12px; padding-bottom:12px}
    .p-12{padding:12px}
    .p-16{padding:16px}
    .mb-8{margin-bottom:8px}
    .mb-12{margin-bottom:12px}
    .mt-12{margin-top:12px}
    .mt-16{margin-top:16px}
    .muted{color:var(--c-support)}
    .title{color:var(--c-primary); font-weight:800}
    .divider{height:1px; background:var(--c-border)}
    .two-lines{display:-webkit-box; -webkit-line-clamp:2; -webkit-box-orient:vertical; overflow:hidden}

    /* ========= Header ========= */
    header{background:#fff; border-bottom:1px solid var(--c-border)}
    .title-chip{
      display:inline-block; padding:6px 10px; border-radius:10px;
      background:linear-gradient(180deg, #eef2ff, #f5f7ff); border:1px solid #e0e7ff;
      box-shadow:0 2px 6px rgba(59,130,246,.08);
      font-weight:900; color:var(--c-primary)
    }
    .btn{padding:8px 12px; border-radius:10px; font-weight:700; border:none; cursor:pointer}
    .btn-primary{background:var(--c-primary); color:#fff}
    .btn-info{background:var(--c-info); color:#fff}
    .btn-accent{background:var(--c-accent); color:#fff}
    .btn-danger{background:var(--c-danger); color:#fff}
    .btn:active{transform:translateY(1px)}

    /* ========= Case buttons ========= */
    .cases{display:grid; gap:10px}
    .case{
      display:flex; flex-direction:column; gap:2px; padding:10px; border-radius:12px;
      background:#fff; border:1px solid var(--c-border); transition:box-shadow .18s, transform .08s, border-color .18s;
    }
    .case:hover{box-shadow:var(--shadow)}
    .case--active{
      border-color:#93c5fd; box-shadow:0 8px 20px rgba(59,130,246,.12), 0 0 0 3px rgba(59,130,246,.12);
      transform:scale(1.01); background:linear-gradient(0deg,#fff,#fff), linear-gradient(120deg, rgba(59,130,246,.06), rgba(59,130,246,0));
    }
    .state-dot{width:8px; height:8px; border-radius:999px; background:#e5e7eb; display:inline-block; margin-right:8px}
    .case--active .state-dot{background:#3b82f6}

    /* ========= Forms ========= */
    label{font-weight:700; color:var(--c-primary)}
    select,textarea{
      width:100%; border:1px solid var(--c-border); border-radius:10px; padding:10px 12px; background:#fff;
      outline:none
    }
    select:focus,textarea:focus{box-shadow:0 0 0 3px rgba(59,130,246,.15); border-color:#93c5fd}

    /* ========= Badges ========= */
    .badge{padding:4px 8px; border-radius:999px; font-weight:800; font-size:12px}
    .badge-case{background:#eef2ff; color:#3730a3; border:1px solid #c7d2fe}
    .badge-high{background:var(--c-danger); color:#fff}
    .badge-medium{background:var(--c-warn); color:#fff}
    .badge-low{background:var(--c-info); color:#fff}
    .badge-none{background:#e5e7eb; color:#334155}

    /* ========= Charts (fixed height to avoid jumps) ========= */
    .chart-card{padding:10px}
    .chart-wrap{height:160px; position:relative}
    .chart-wrap canvas{position:absolute; inset:0; width:100% !important; height:100% !important}

    /* ========= Footer ========= */
    footer{color:var(--c-support); font-size:12px; text-align:center; padding:8px 0}
  </style>
</head>
<body>
  <!-- Header -->
  <header>
    <div class="container px-16 py-12" style="display:flex;justify-content:space-between;align-items:center;gap:12px;">
      <div>
        <div class="title-chip">리더 코칭 챗봇 — 리더의 대화, 더 나은 해답으로</div>
        <div class="muted" style="margin-top:4px; font-size:13px;">
          평가 · 보상 · 승진 · 동료 갈등 · 노무 이슈 면담 시뮬레이션
        </div>
      </div>
      <div style="display:flex; gap:8px;">
        <button id="resetBtn" class="btn btn-info" type="button">초기화</button>
        <button id="exportBtn" class="btn btn-primary" type="button">기록 내보내기</button>
      </div>
    </div>
  </header>

  <main class="container px-16" style="padding-top:14px; padding-bottom:14px;">
    <!-- 상단: 실습 -->
    <section class="row" style="gap:14px;">
      <!-- 케이스 선택 -->
      <div class="cases row row-5">
        <button class="case" data-case="평가" type="button" aria-pressed="false">
          <div style="display:flex;align-items:center;">
            <span class="state-dot"></span><span class="title" style="font-size:15px;">평가</span>
          </div>
          <span class="muted" style="font-size:12px;">불만/이의제기</span>
        </button>
        <button class="case" data-case="보상" type="button" aria-pressed="false">
          <div style="display:flex;align-items:center;">
            <span class="state-dot"></span><span class="title" style="font-size:15px;">보상</span>
          </div>
          <span class="muted" style="font-size:12px;">연봉/인상</span>
        </button>
        <button class="case" data-case="승진" type="button" aria-pressed="false">
          <div style="display:flex;align-items:center;">
            <span class="state-dot"></span><span class="title" style="font-size:15px;">승진</span>
          </div>
          <span class="muted" style="font-size:12px;">기준/누락</span>
        </button>
        <button class="case" data-case="갈등" type="button" aria-pressed="false">
          <div style="display:flex;align-items:center;">
            <span class="state-dot"></span><span class="title" style="font-size:15px;">동료 갈등</span>
          </div>
          <span class="muted" style="font-size:12px;">협업/분쟁</span>
        </button>
        <button class="case" data-case="노무" type="button" aria-pressed="false">
          <div style="display:flex;align-items:center;">
            <span class="state-dot"></span><span class="title" style="font-size:15px;">노무</span>
          </div>
          <span class="muted" style="font-size:12px;">법적 리스크</span>
        </button>
      </div>

      <!-- 시나리오 -->
      <div class="surface p-16">
        <div style="display:flex;flex-wrap:wrap;gap:10px;align-items:center;justify-content:space-between">
          <div style="display:flex;align-items:center;gap:8px;">
            <h2 class="title" style="font-size:18px;">시나리오</h2>
            <span id="selectedCaseBadge" class="badge badge-case" style="display:none;"></span>
          </div>
          <label style="display:flex;align-items:center;gap:8px;font-size:14px;">
            <span class="muted">상황</span>
            <select id="scenarioType" style="min-width:240px">
              <option value="">케이스를 먼저 선택하세요</option>
            </select>
          </label>
        </div>
        <div class="divider mt-12 mb-12"></div>
        <p id="scenarioText" class="muted">케이스를 선택하세요.</p>
      </div>

      <!-- 리더 답변 입력 -->
      <div class="surface p-16">
        <label for="leaderInput">리더 답변 입력</label>
        <div class="muted" style="margin-top:6px; font-size:13px;">
          흐름: <span style="font-weight:700;background:#f1f5f9;border-radius:999px;padding:2px 8px;">공감</span>
          → <span style="font-weight:700;background:#f1f5f9;border-radius:999px;padding:2px 8px;">기준/원칙</span>
          → <span style="font-weight:700;background:#f1f5f9;border-radius:999px;padding:2px 8px;">HR 절차 안내</span>
        </div>
        <textarea id="leaderInput" rows="4" class="mt-12" placeholder="예) 말씀해주셔서 감사합니다. 그렇게 느끼실 수 있습니다. 기준에 따라 확인 후 안내드리겠습니다."></textarea>
        <div style="display:flex;align-items:center;gap:8px;margin-top:10px;">
          <button id="coachBtn" class="btn btn-primary" type="button">코칭 받기</button>
          <button id="copyBestBtn" class="btn btn-accent" type="button" style="display:none;">모범 대화 복사</button>
          <span id="toast" class="muted"></span>
        </div>
      </div>

      <!-- 피드백 -->
      <div id="feedbackPanel" class="surface p-16" style="display:none;">
        <div style="display:flex;align-items:center;justify-content:space-between;">
          <div class="title" style="font-size:16px;">코칭 피드백</div>
          <span id="riskBadge" class="badge">RISK</span>
        </div>

        <div class="row row-2 mt-16">
          <div class="surface p-12">
            <div class="title" style="font-size:14px;">잘한 점</div>
            <p id="fbPositive" class="mt-12"></p>
          </div>
          <div class="surface p-12">
            <div class="title" style="font-size:14px;">보완할 점</div>
            <p id="fbImprovement" class="mt-12"></p>
          </div>
        </div>

        <div class="surface p-12 mt-12">
          <div class="title" style="font-size:14px;">모범 대화</div>
          <p id="fbBest" class="mt-12"></p>
        </div>
        <div class="surface p-12 mt-12">
          <div class="title" style="font-size:14px;">다음 학습 제안</div>
          <p id="fbNext" class="mt-12"></p>
        </div>
        <div class="mt-12">
          <button id="hrBtn" class="btn btn-danger" type="button" style="display:none;">HR 연결 가이드 확인</button>
        </div>
      </div>
    </section>

    <!-- 하단: 대시보드 & 보조 -->
    <section class="row" style="gap:14px; margin-top:14px;">
      <div class="surface p-16">
        <div class="title" style="font-size:16px;">학습 대시보드</div>
        <div class="row row-2 mt-12">
          <div class="surface chart-card">
            <div class="muted" style="font-weight:600; font-size:13px; margin-bottom:6px;">리스크 분포(도넛)</div>
            <div class="chart-wrap"><canvas id="riskDonut"></canvas></div>
          </div>
          <div class="surface chart-card">
            <div class="muted" style="font-weight:600; font-size:13px; margin-bottom:6px;">케이스별 시도(막대)</div>
            <div class="chart-wrap"><canvas id="caseBar"></canvas></div>
          </div>
        </div>
      </div>

      <div class="row row-2">
        <div class="surface p-16">
          <div class="title" style="font-size:16px;">히스토리 (최근 5건)</div>
          <ul id="historyList" class="mt-12" style="list-style:none;padding:0;margin:0;display:grid;gap:10px;"></ul>
        </div>
        <div class="surface p-16">
          <div class="title" style="font-size:16px;">HR 퀵가이드</div>
          <ul class="mt-12 muted" style="font-size:14px; padding-left:16px;">
            <li>• <b>이의제기:</b> 절차·기한·서류 안내 후 HR 경로 접수</li>
            <li>• <b>갈등조정:</b> 당사자 분리 → 사실확인 → 중립 조정 → 후속관리</li>
            <li>• <b>즉시 HR:</b> 차별/괴롭힘/보복/노동청/법률 언급</li>
          </ul>
        </div>
      </div>
      <footer>© Woongjin · Leader’s Conflict Coach</footer>
    </section>
  </main>

  <!-- 유일한 외부 의존성: Chart.js (defer로 레이아웃 영향 최소화) -->
  <script defer src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <script>
  // ===== 시나리오 데이터 =====
  const scenarios = {
    "평가":[
      {key:"객관기준요청",text:"직원: 이번 평가 결과가 납득되지 않습니다. 구체적인 기준과 근거를 설명해 주세요."},
      {key:"동료비교불만",text:"직원: 저보다 성과 낮은 동료가 더 높은 등급이라니요? 형평성이 없어요."},
      {key:"피드백부족",text:"직원: 평가 피드백이 모호했습니다. 무엇을 개선해야 하는지 모르겠어요."},
      {key:"편향의심",text:"직원: 팀 내 편향과 선호가 반영된 것 같습니다. 공정성 검토가 필요해요."},
      {key:"목표설정문제",text:"직원: 목표가 중간에 바뀌었는데 그게 반영된 건가요?"}
    ],
    "보상":[
      {key:"시장평균이슈",text:"직원: 업계 평균 대비 제 연봉이 낮습니다. 시장 보정이 필요한 것 아닌가요?"},
      {key:"성과대비보상",text:"직원: 성과 대비 인상률이 너무 적습니다. 동기부여가 되지 않아요."},
      {key:"보상구조불만",text:"직원: 개인 성과보다 회사 실적 가중치가 너무 커서 불공정해요."},
      {key:"이직시사",text:"직원: 타사 제안을 받고 있어요. 보상 재검토 가능합니까?"},
      {key:"생활비압박",text:"직원: 최근 물가와 생활비가 크게 올라서 실제 체감 보상이 줄었습니다. 회사에서 생활 안정 지원 방안은 없나요?"}
    ],
    "승진":[
      {key:"승진누락",text:"직원: 저는 왜 승진에서 탈락했나요? 기준이 불투명합니다."},
      {key:"비교승진",text:"직원: 저보다 영향력이 적은 동료가 승진했습니다. 납득이 안 됩니다."},
      {key:"역량요건",text:"직원: 승진 요건(리더십/조직기여)을 어떻게 충족해야 하나요?"},
      {key:"평판이슈",text:"직원: 익명 피드백/평판이 작용했다는 말이 있는데 사실인가요?"},
      {key:"로드맵요청",text:"직원: 다음 승진을 위해 구체적인 개발 로드맵을 제시해 주세요."}
    ],
    "갈등":[
      {key:"업무분장",text:"직원: ○○씨가 업무를 분담하지 않아 제가 과중합니다."},
      {key:"커뮤니케이션",text:"직원: 팀 내 소통 방식이 공격적이라 협업이 어렵습니다."},
      {key:"성과공헌",text:"직원: 제 아이디어가 공로 인정 없이 사용됐습니다."},
      {key:"역할경계",text:"직원: 제 역할 범위를 넘어 지시가 반복돼 불만이 큽니다."},
      {key:"장기갈등",text:"직원: 장기간 누적된 갈등이 있어 중재가 필요합니다."}
    ],
    "노무":[
      {key:"차별의심",text:"직원: 특정 집단에 대한 차별이 있다고 느낍니다. 확인해 주세요."},
      {key:"괴롭힘의심",text:"직원: 반복적인 모욕적 언행이 있었습니다. 괴롭힘에 해당한다고 봅니다."},
      {key:"보복우려",text:"직원: 문제 제기 이후 불이익이 있을까 걱정됩니다."},
      {key:"노동청언급",text:"직원: 필요하다면 노동청에 신고하겠습니다."},
      {key:"업무외연락강요",text:"직원: 업무 시간 외 메신저·전화 응답을 강요받습니다. 워라밸이 무너져 힘듭니다."},
      {key:"원격근무감시",text:"직원: 재택 중 카메라 상시 켜두라고 요구받습니다. 과도한 감시 아닌가요?"},
      {key:"SNS발언논란",text:"직원: 제 SNS 글 때문에 징계 검토 얘기가 나옵니다. 사생활 침해 아닌가요?"},
      {key:"사적메신저지시",text:"직원: 카톡 같은 사적 메신저로 업무 지시를 계속 받습니다. 공식 채널로만 하면 안 되나요?"}
    ]
  };

  // ===== 상태 =====
  const state = {
    currentCase: null,
    riskCounts: { HIGH:0, MEDIUM:0, LOW:0, NONE:0 },
    caseCounts: { "평가":0, "보상":0, "승진":0, "갈등":0, "노무":0 }
  };

  // ===== DOM =====
  const scenarioType = document.getElementById("scenarioType");
  const scenarioText = document.getElementById("scenarioText");
  const selectedCaseBadge = document.getElementById("selectedCaseBadge");
  const leaderInput = document.getElementById("leaderInput");
  const toast = document.getElementById("toast");
  const coachBtn = document.getElementById("coachBtn");
  const copyBestBtn = document.getElementById("copyBestBtn");
  const fbPanel = document.getElementById("feedbackPanel");
  const fbPositive = document.getElementById("fbPositive");
  const fbImprovement = document.getElementById("fbImprovement");
  const fbBest = document.getElementById("fbBest");
  const fbNext = document.getElementById("fbNext");
  const riskBadge = document.getElementById("riskBadge");
  const hrBtn = document.getElementById("hrBtn");
  const historyList = document.getElementById("historyList");

  // ===== 차트 =====
  let riskChart, caseChart;
  function badgeClass(level){
    return level==="HIGH" ? "badge badge-high" : level==="MEDIUM" ? "badge badge-medium"
         : level==="LOW" ? "badge badge-low" : "badge badge-none";
  }

  function initCharts(){
    const donutCtx = document.getElementById("riskDonut").getContext("2d");
    const barCtx = document.getElementById("caseBar").getContext("2d");
    if(riskChart) riskChart.destroy();
    if(caseChart) caseChart.destroy();

    riskChart = new Chart(donutCtx, {
      type: "doughnut",
      data: {
        labels: ["HIGH","MEDIUM","LOW","NONE"],
        datasets: [{
          data: [state.riskCounts.HIGH, state.riskCounts.MEDIUM, state.riskCounts.LOW, state.riskCounts.NONE],
          backgroundColor: ["#b91c1c","#d97706","#0ea5e9","#cbd5e1"], borderWidth:0
        }]
      },
      options: {
        responsive:true, maintainAspectRatio:false, animation:false,
        plugins:{ legend:{ position:'bottom', labels:{ boxWidth:12, font:{size:11} } } },
        layout:{ padding:4 }
      }
    });

    caseChart = new Chart(barCtx, {
      type: "bar",
      data: {
        labels: Object.keys(state.caseCounts),
        datasets: [{ label:"시도", data:Object.values(state.caseCounts), backgroundColor:"#1f2a44", borderWidth:0 }]
      },
      options: {
        responsive:true, maintainAspectRatio:false, animation:false,
        plugins:{ legend:{ display:false } },
        scales:{
          x:{ ticks:{ font:{ size:11 } }, grid:{ display:false } },
          y:{ beginAtZero:true, ticks:{ stepSize:1, font:{ size:11 } }, grid:{ color:'#eef2f7' } }
        },
        layout:{ padding:4 }
      }
    });
  }

  // ===== 히스토리 =====
  const STORAGE_KEY = "lcc_history_nofouc";
  const loadHistory = ()=>{ try{ return JSON.parse(localStorage.getItem(STORAGE_KEY)||"[]"); }catch(e){ return []; } };
  const saveHistory = (items)=> localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
  const pushHistory = (entry)=>{ const arr = loadHistory(); arr.unshift(entry); saveHistory(arr.slice(0,30)); };
  function renderHistory(){
    const items = loadHistory();
    historyList.innerHTML = "";
    items.slice(0,5).forEach(it=>{
      const badge = badgeClass(it.Feedback.Risk.level);
      const li = document.createElement("li");
      li.className = "surface p-12";
      li.innerHTML = `
        <div style="display:flex;justify-content:space-between;align-items:center;">
          <div class="title" style="font-size:14px;">${it.case} · ${it.scenarioKey}</div>
          <span class="${badge}">${it.Feedback.Risk.level}</span>
        </div>
        <div class="muted two-lines" style="margin-top:6px; font-size:13px;">${it.UserInput}</div>
      `;
      historyList.appendChild(li);
    });
  }

  // ===== 케이스 선택 =====
  const caseButtons = Array.from(document.querySelectorAll(".case"));
  function setActiveCaseButton(btn){
    caseButtons.forEach(b=>{ b.classList.remove("case--active"); b.setAttribute("aria-pressed","false"); });
    btn.classList.add("case--active"); btn.setAttribute("aria-pressed","true");
  }
  function handleCaseSelect(btn){
    state.currentCase = btn.dataset.case;
    setActiveCaseButton(btn);
    scenarioType.innerHTML = `<option value="">상황을 선택하세요</option>`;
    scenarios[state.currentCase].forEach((s, idx)=>{
      const preview = s.text.replace(/^직원:\s?/, "").slice(0,28) + (s.text.length>28?"...":"");
      scenarioType.insertAdjacentHTML("beforeend", `<option value="${idx}">[${s.key}] ${preview}</option>`);
    });
    selectedCaseBadge.textContent = state.currentCase;
    selectedCaseBadge.style.display = "inline-block";
    scenarioText.textContent = "상황을 선택하세요.";
  }
  caseButtons.forEach(btn=>{
    btn.addEventListener("click", ()=>handleCaseSelect(btn));
    btn.tabIndex = 0;
    btn.addEventListener("keydown", e=>{ if(e.key==="Enter"||e.key===" "){ e.preventDefault(); handleCaseSelect(btn);} });
  });
  scenarioType.addEventListener("change", ()=>{
    const idx = scenarioType.value;
    scenarioText.textContent = (!state.currentCase||idx==="") ? "상황을 선택하세요." : scenarios[state.currentCase][Number(idx)].text;
  });

  // ===== 코칭 (오프라인 폴백) =====
  function localCoach(){
    const t = (leaderInput.value||"") + " " + (scenarioText.textContent||"");
    const hasEmp = /감사|공감|이해|그럴 수|경청/.test(t);
    const badTone = /너|네가|당연히|원래|말이 안돼|무조건/.test(t);
    const risk = /신고|노동청|차별|괴롭힘/.test(t) ? "HIGH" : /보복|부당|불이익/.test(t) ? "MEDIUM" : "LOW";
    return {
      positive: hasEmp ? "공감과 경청이 잘 드러납니다." : "공감 문장을 먼저 제시하면 효과적입니다.",
      improvement: badTone ? "단정적 표현을 줄이고 근거/절차 중심으로 설명하세요." : "기준·근거 제시와 HR 절차 안내를 병행하세요.",
      riskLevel: risk,
      riskNotes: risk==="HIGH" ? "고위험 키워드: 즉시 HR 연결" : risk==="MEDIUM" ? "중위험: 신중한 표현·절차 병행" : "낮은 리스크: 기록 유지 권장",
      bestPractice: "말씀해 주셔서 감사합니다. 그렇게 느끼실 수 있습니다. 공지된 기준과 기록을 근거로 사실관계를 확인하겠습니다. 필요 시 HR 절차(이의제기/조정)도 안내드리겠습니다.",
      nextStep: "같은 케이스의 다른 상황을 연습하고, 모범 문장을 자신의 표현으로 재구성해 보세요."
    };
  }

  document.getElementById("coachBtn").addEventListener("click", async ()=>{
    const scenario = scenarioText.textContent.trim();
    const input = leaderInput.value.trim();
    if(!state.currentCase){ toast.textContent="케이스를 먼저 선택하세요."; return; }
    if(scenarioType.value===""){ toast.textContent="상황을 선택하세요."; return; }
    if(!input){ toast.textContent="답변을 입력하세요."; return; }

    coachBtn.disabled = true; coachBtn.textContent = "코칭 중..."; toast.textContent="피드백 생성 중...";
    try{
      // (외부 API 제거: 100% 로컬 폴백 → 깜빡임/지연 최소)
      const feedback = localCoach();

      fbPanel.style.display = "block";
      fbPositive.textContent = feedback.positive;
      fbImprovement.textContent = feedback.improvement;
      fbBest.textContent = feedback.bestPractice;
      fbNext.textContent = feedback.nextStep;
      riskBadge.textContent = feedback.riskLevel;
      riskBadge.className = "badge " + (feedback.riskLevel==="HIGH" ? "badge-high" : feedback.riskLevel==="MEDIUM" ? "badge-medium" : feedback.riskLevel==="LOW" ? "badge-low" : "badge-none");
      hrBtn.style.display = feedback.riskLevel==="HIGH" ? "inline-block" : "none";

      copyBestBtn.style.display = "inline-block";
      copyBestBtn.onclick = async ()=>{
        try{
          if(navigator.clipboard?.writeText) await navigator.clipboard.writeText(feedback.bestPractice);
          else { const ta=document.createElement("textarea"); ta.value=feedback.bestPractice; document.body.appendChild(ta); ta.select(); document.execCommand("copy"); document.body.removeChild(ta); }
          toast.textContent="모범 대화가 복사되었습니다."; setTimeout(()=>toast.textContent="",1200);
        }catch{ toast.textContent="복사 실패"; }
      };

      // 상태 → 차트 갱신
      state.riskCounts[feedback.riskLevel] = (state.riskCounts[feedback.riskLevel]||0) + 1;
      state.caseCounts[state.currentCase] = (state.caseCounts[state.currentCase]||0) + 1;
      initCharts();

      // 히스토리
      const idx = Number(scenarioType.value);
      const key = scenarios[state.currentCase][idx].key;
      pushHistory({
        Scenario: scenario, UserInput: input,
        Feedback:{ Positive:feedback.positive, Improvement:feedback.improvement,
          Risk:{ level:feedback.riskLevel, notes:feedback.riskNotes, escalateToHR:(feedback.riskLevel==="HIGH") },
          BestPractice:feedback.bestPractice },
        NextStep: feedback.nextStep, case: state.currentCase, scenarioKey: key, time: Date.now()
      });
      renderHistory();

    }finally{
      coachBtn.disabled = false; coachBtn.textContent = "코칭 받기";
      if(!toast.textContent.includes("실패")) setTimeout(()=>toast.textContent="",1800);
    }
  });

  // HR 가이드 모달(간단)
  document.getElementById("hrBtn").addEventListener("click", ()=>{
    const modal = document.createElement("div");
    modal.style.cssText = "position:fixed;inset:0;background:rgba(0,0,0,.4);display:flex;align-items:center;justify-content:center;z-index:50";
    modal.innerHTML = '<div class="surface" style="padding:16px; max-width:360px; text-align:center;"><div class="title" style="font-size:16px; margin-bottom:8px;">HR 연결 가이드</div><p class="muted" style="font-size:14px;">즉시 HR에 사실관계 보고 → 공식 절차(이의제기/조사) 안내 → 문서화/기록 유지 → 2차 피해 방지 조치</p><div style="margin-top:12px;"><button id="closeModal" class="btn btn-primary" type="button">확인</button></div></div>';
    document.body.appendChild(modal);
    modal.querySelector("#closeModal").addEventListener("click", ()=> document.body.removeChild(modal));
  });

  // 초기화/내보내기
  document.getElementById("resetBtn").addEventListener("click", ()=>{
    leaderInput.value = "";
    fbPanel.style.display = "none";
    copyBestBtn.style.display = "none";
    toast.textContent="";

    localStorage.removeItem(STORAGE_KEY);
    renderHistory();

    state.riskCounts = { HIGH:0, MEDIUM:0, LOW:0, NONE:0 };
    state.caseCounts = { "평가":0, "보상":0, "승진":0, "갈등":0, "노무":0 };
    initCharts();
  });
  document.getElementById("exportBtn").addEventListener("click", ()=>{
    const data = loadHistory();
    const blob = new Blob([JSON.stringify(data, null, 2)], {type:"application/json"});
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    a.download = `leader-conflict-coach-history-${new Date().toISOString().slice(0,10).replaceAll("-","")}.json`;
    a.click();
    URL.revokeObjectURL(a.href);
  });

  // 첫 렌더(Chart.js 로드 후)
  window.addEventListener("load", ()=>{
    renderHistory();
    if(window.Chart){ initCharts(); }
  });
  </script>
</body>
</html>
