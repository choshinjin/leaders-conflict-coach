<!DOCTYPE html>
<html lang="ko" class="no-fouc">
<head>
  <meta charset="utf-8" />
  <title>리더 코칭 챗봇 — 리더의 대화, 더 나은 해답으로</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!-- Google Fonts (깜빡임 최소화: display=swap) -->
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;600;700;900&display=swap" rel="stylesheet"/>

  <!-- Tailwind (CDN) -->
  <script src="https://cdn.tailwindcss.com"></script>

  <style>
    /* 초기 깜빡임 방지: 모든 리소스 로딩 완료 전까지 화면 숨김 */
    html.no-fouc { visibility: hidden; }
    :root{
      --c-bg:#f6f7fb; --c-surface:#ffffff; --c-primary:#1f2a44; --c-accent:#3b82f6;
      --c-support:#6b7280; --c-border:#e6e8ef; --c-danger:#b91c1c; --c-warn:#d97706;
      --c-info:#0ea5e9; --shadow:0 10px 24px rgba(15,23,42,.06);
    }
    html,body{font-family:'Noto Sans KR', system-ui, -apple-system, Segoe UI, Roboto, sans-serif; color:#0f172a;}
    .bg-app{background:var(--c-bg);}
    .text-primary{color:var(--c-primary);}
    .text-support{color:var(--c-support);}
    .surface{background:var(--c-surface); border:1px solid var(--c-border); border-radius:16px; box-shadow:var(--shadow);}
    .header{backdrop-filter: blur(8px); background:rgba(255,255,255,.7); border-bottom:1px solid var(--c-border);}
    .btn{padding:.6rem 1rem; border-radius:12px; font-weight:700; letter-spacing:.2px; transition:transform .05s ease, opacity .15s;}
    .btn:active{transform:translateY(1px);}
    .btn-primary{background:var(--c-primary); color:#fff;}
    .btn-accent{background:var(--c-accent); color:#fff;}
    .btn-info{background:var(--c-info); color:#fff;}
    .btn-danger{background:var(--c-danger); color:#fff;}
    .badge{padding:.25rem .6rem; border-radius:999px; font-weight:800; font-size:.85rem;}
    .badge-high{background:var(--c-danger); color:#fff;}
    .badge-medium{background:var(--c-warn); color:#fff;}
    .badge-low{background:var(--c-info); color:#fff;}
    .badge-none{background:#e5e7eb; color:#334155;}
    .badge-case{background:#eef2ff; color:#3730a3; border:1px solid #c7d2fe;}
    .card-title{font-weight:800; color:var(--c-primary); letter-spacing:.2px;}
    .muted{color:#94a3b8;}
    .kv{display:inline-block; padding:.25rem .6rem; border-radius:999px; background:#f1f5f9; color:#334155; font-weight:700; font-size:.82rem;}
    textarea, select{border:1px solid var(--c-border); border-radius:12px; padding:.7rem .9rem;}
    .hint{font-size:.85rem; color:#6b7280;}
    .list-dot li{position:relative; padding-left:1rem;}
    .list-dot li::before{content:"•"; position:absolute; left:0; color:var(--c-accent);}
    .grid-cases .case{
      display:flex; flex-direction:column; gap:.25rem; padding:1rem; border-radius:14px; background:#fff;
      border:1px solid var(--c-border); transition:box-shadow .18s ease, transform .08s ease, border-color .18s ease;
    }
    .grid-cases .case:hover{box-shadow:var(--shadow);}
    /* 선택 상태 강조 */
    .grid-cases .case.case--active{
      border-color:#93c5fd;
      box-shadow:0 10px 26px rgba(59,130,246,.15), 0 0 0 3px rgba(59,130,246,.15);
      transform:scale(1.01);
      background:linear-gradient(0deg,#ffffff, #ffffff), linear-gradient(120deg, rgba(59,130,246,.06), rgba(59,130,246,0));
    }
    .grid-cases .case .state-dot{
      width:8px; height:8px; border-radius:999px; background:#e5e7eb; display:inline-block; transition:background .18s ease;
    }
    .grid-cases .case.case--active .state-dot{ background:#3b82f6; }
    .divider{height:1px; background:var(--c-border);}

    /* 차트 영역 높이를 고정해 초기 레이아웃 점프 방지 */
    .chart-wrap{min-height:240px; position:relative;}
    .chart-wrap canvas{position:absolute; inset:0;}
  </style>
</head>

<body class="bg-app">
  <!-- 헤더 -->
  <header class="header sticky top-0 z-20">
    <div class="mx-auto max-w-7xl px-4 py-4 flex items-center justify-between">
      <div>
        <h1 class="text-xl md:text-2xl font-extrabold text-primary">리더 코칭 챗봇 — 리더의 대화, 더 나은 해답으로</h1>
        <p class="text-sm md:text-base text-support">평가 · 보상 · 승진 · 동료 갈등 · 노무 이슈 면담 시뮬레이션</p>
      </div>
      <div class="flex gap-2">
        <button id="resetBtn" type="button" class="btn btn-info">초기화</button>
        <button id="exportBtn" type="button" class="btn btn-primary">기록 내보내기</button>
      </div>
    </div>
  </header>

  <main class="mx-auto max-w-7xl px-4 py-6 grid grid-cols-1 lg:grid-cols-12 gap-6">
    <!-- 좌측 메인 -->
    <section class="lg:col-span-8 space-y-6">
      <!-- 케이스 선택 -->
      <div class="grid grid-cols-2 md:grid-cols-5 gap-3 grid-cases" role="tablist" aria-label="케이스 선택">
        <button data-case="평가" class="case" role="tab" aria-pressed="false" type="button">
          <div class="flex items-center gap-2">
            <span class="state-dot" aria-hidden="true"></span>
            <span class="font-bold text-primary">평가</span>
          </div>
          <span class="muted text-xs">불만/이의제기</span>
        </button>
        <button data-case="보상" class="case" role="tab" aria-pressed="false" type="button">
          <div class="flex items-center gap-2">
            <span class="state-dot" aria-hidden="true"></span>
            <span class="font-bold text-primary">보상</span>
          </div>
          <span class="muted text-xs">연봉/인상</span>
        </button>
        <button data-case="승진" class="case" role="tab" aria-pressed="false" type="button">
          <div class="flex items-center gap-2">
            <span class="state-dot" aria-hidden="true"></span>
            <span class="font-bold text-primary">승진</span>
          </div>
          <span class="muted text-xs">기준/누락</span>
        </button>
        <button data-case="갈등" class="case" role="tab" aria-pressed="false" type="button">
          <div class="flex items-center gap-2">
            <span class="state-dot" aria-hidden="true"></span>
            <span class="font-bold text-primary">동료 갈등</span>
          </div>
          <span class="muted text-xs">협업/분쟁</span>
        </button>
        <button data-case="노무" class="case" role="tab" aria-pressed="false" type="button">
          <div class="flex items-center gap-2">
            <span class="state-dot" aria-hidden="true"></span>
            <span class="font-bold text-primary">노무</span>
          </div>
          <span class="muted text-xs">법적 리스크</span>
        </button>
      </div>

      <!-- 시나리오 카드 -->
      <div class="surface p-5">
        <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-3">
          <div class="flex items-center gap-3">
            <h2 class="card-title text-lg">시나리오</h2>
            <span id="selectedCaseBadge" class="badge badge-case hidden"></span>
          </div>
          <div class="flex items-center gap-3">
            <label class="text-sm flex items-center gap-2">
              <span class="text-support">상황</span>
              <select id="scenarioType" class="min-w-[280px]">
                <option value="">케이스를 먼저 선택하세요</option>
              </select>
            </label>
          </div>
        </div>
        <div class="divider my-3"></div>
        <p id="scenarioText" class="text-support">케이스를 선택하세요.</p>
      </div>

      <!-- 리더 답변 입력 -->
      <div class="surface p-5">
        <label for="leaderInput" class="block card-title">리더 답변 입력</label>
        <p class="hint mt-1">권장 흐름: <span class="kv">공감</span> → <span class="kv">기준/원칙</span> → <span class="kv">HR 절차 안내</span></p>
        <textarea id="leaderInput" rows="4" class="mt-3 w-full" placeholder="예) 말씀해주셔서 감사합니다. 그렇게 느끼실 수 있습니다. 기준에 따라 확인 후 안내드리겠습니다."></textarea>
        <div class="mt-4 flex items-center gap-2">
          <button id="coachBtn" type="button" class="btn btn-primary">코칭 받기</button>
          <button id="copyBestBtn" type="button" class="btn btn-accent hidden">모범 대화 복사</button>
          <span id="toast" class="hint"></span>
        </div>
      </div>

      <!-- 피드백 -->
      <div id="feedbackPanel" class="surface p-5 hidden">
        <div class="flex items-center justify-between">
          <h3 class="card-title text-lg">코칭 피드백</h3>
          <span id="riskBadge" class="badge">RISK</span>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
          <div class="surface p-4">
            <div class="card-title">잘한 점</div>
            <p id="fbPositive" class="mt-2"></p>
          </div>
          <div class="surface p-4">
            <div class="card-title">보완할 점</div>
            <p id="fbImprovement" class="mt-2"></p>
          </div>
        </div>

        <div class="surface p-4 mt-4">
          <div class="card-title">모범 대화</div>
          <p id="fbBest" class="mt-2"></p>
        </div>

        <div class="surface p-4 mt-4">
          <div class="card-title">다음 학습 제안</div>
          <p id="fbNext" class="mt-2"></p>
        </div>

        <div class="mt-4">
          <button id="hrBtn" type="button" class="btn btn-danger hidden">HR 연결 가이드 확인</button>
        </div>
      </div>

      <!-- 학습 대시보드 -->
      <div class="surface p-5">
        <h3 class="card-title text-lg">학습 대시보드</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-4">
          <div class="surface p-4 chart-wrap">
            <div class="text-support font-semibold mb-2">리스크 분포(도넛)</div>
            <canvas id="riskDonut"></canvas>
          </div>
          <div class="surface p-4 chart-wrap">
            <div class="text-support font-semibold mb-2">케이스별 시도(막대)</div>
            <canvas id="caseBar"></canvas>
          </div>
        </div>
      </div>
    </section>

    <!-- 우측 사이드 -->
    <aside class="lg:col-span-4 space-y-6">
      <div class="surface p-5">
        <h3 class="card-title text-lg">HR 퀵가이드</h3>
        <ul class="list-dot mt-3 space-y-2">
          <li><b>이의제기:</b> 공식 절차·기한·서류 안내 후 HR 경로 접수</li>
          <li><b>갈등조정:</b> 당사자 분리 → 사실확인 → 중립적 조정 → 후속관리</li>
          <li><b>HR 연결 기준:</b> 차별/괴롭힘/보복/노동청/법률 언급 시 즉시 HR</li>
        </ul>
      </div>

      <div class="surface p-5">
        <h3 class="card-title text-lg">리스크 배지</h3>
        <div class="mt-3 flex flex-wrap gap-2">
          <span class="badge badge-high">HIGH</span>
          <span class="badge badge-medium">MEDIUM</span>
          <span class="badge badge-low">LOW</span>
          <span class="badge badge-none">NONE</span>
        </div>
      </div>

      <div class="surface p-5">
        <h3 class="card-title text-lg">히스토리 (최근 10건)</h3>
        <ul id="historyList" class="mt-3 space-y-3 text-support"></ul>
      </div>

      <footer class="muted text-center py-2">© Woongjin · Leader’s Conflict Coach</footer>
    </aside>
  </main>

  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <script>
    // 모든 리소스 로딩 완료 후 화면 표시(FOUC 방지)
    window.addEventListener('load', () => {
      document.documentElement.classList.remove('no-fouc');
    });

    /* =========== 1) 시나리오 =========== */
    const scenarios = {
      "평가": [
        {key:"객관기준요청", text:"직원: 이번 평가 결과가 납득되지 않습니다. 구체적인 기준과 근거를 설명해 주세요."},
        {key:"동료비교불만", text:"직원: 저보다 성과 낮은 동료가 더 높은 등급이라니요? 형평성이 없어요."},
        {key:"피드백부족", text:"직원: 평가 피드백이 모호했습니다. 무엇을 개선해야 하는지 모르겠어요."},
        {key:"편향의심", text:"직원: 팀 내 편향과 선호가 반영된 것 같습니다. 공정성 검토가 필요해요."},
        {key:"목표설정문제", text:"직원: 목표가 중간에 바뀌었는데 그게 반영된 건가요?"}
      ],
      "보상": [
        {key:"시장평균이슈", text:"직원: 업계 평균 대비 제 연봉이 낮습니다. 시장 보정이 필요한 것 아닌가요?"},
        {key:"성과대비보상", text:"직원: 성과 대비 인상률이 너무 적습니다. 동기부여가 되지 않아요."},
        {key:"보상구조불만", text:"직원: 개인 성과보다 회사 실적 가중치가 너무 커서 불공정해요."},
        {key:"이직시사", text:"직원: 타사 제안을 받고 있어요. 보상 재검토 가능합니까?"},
        {key:"생활비압박", text:"직원: 최근 물가와 생활비가 크게 올라서 실제 체감 보상이 줄었습니다. 회사에서 생활 안정 지원 방안은 없나요?"}
      ],
      "승진": [
        {key:"승진누락", text:"직원: 저는 왜 승진에서 탈락했나요? 기준이 불투명합니다."},
        {key:"비교승진", text:"직원: 저보다 영향력이 적은 동료가 승진했습니다. 납득이 안 됩니다."},
        {key:"역량요건", text:"직원: 승진 요건(리더십/조직기여)을 어떻게 충족해야 하나요?"},
        {key:"평판이슈", text:"직원: 익명 피드백/평판이 작용했다는 말이 있는데 사실인가요?"},
        {key:"로드맵요청", text:"직원: 다음 승진을 위해 구체적인 개발 로드맵을 제시해 주세요."}
      ],
      "갈등": [
        {key:"업무분장", text:"직원: ○○씨가 업무를 분담하지 않아 제가 과중합니다."},
        {key:"커뮤니케이션", text:"직원: 팀 내 소통 방식이 공격적이라 협업이 어렵습니다."},
        {key:"성과공헌", text:"직원: 제 아이디어가 공로 인정 없이 사용됐습니다."},
        {key:"역할경계", text:"직원: 제 역할 범위를 넘어 지시가 반복돼 불만이 큽니다."},
        {key:"장기갈등", text:"직원: 장기간 누적된 갈등이 있어 중재가 필요합니다."}
      ],
      "노무": [
        {key:"차별의심", text:"직원: 특정 집단에 대한 차별이 있다고 느낍니다. 확인해 주세요."},
        {key:"괴롭힘의심", text:"직원: 반복적인 모욕적 언행이 있었습니다. 괴롭힘에 해당한다고 봅니다."},
        {key:"보복우려", text:"직원: 문제 제기 이후 불이익이 있을까 걱정됩니다."},
        {key:"노동청언급", text:"직원: 필요하다면 노동청에 신고하겠습니다."},
        {key:"업무외연락강요", text:"직원: 업무 시간 외 메신저·전화 응답을 강요받습니다. 워라밸이 무너져 힘듭니다."},
        {key:"원격근무감시", text:"직원: 재택 중 카메라 상시 켜두라고 요구받습니다. 과도한 감시 아닌가요?"},
        {key:"SNS발언논란", text:"직원: 제 SNS 글 때문에 징계 검토 얘기가 나옵니다. 사생활 침해 아닌가요?"},
        {key:"사적메신저지시", text:"직원: 카톡 같은 사적 메신저로 업무 지시를 계속 받습니다. 공식 채널로만 하면 안 되나요?"}
      ]
    };

    /* =========== 2) 상태/DOM =========== */
    let currentCase = null;
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

    /* =========== 3) 차트/히스토리 =========== */
    let riskChart, caseChart;
    const riskCounts = { HIGH:0, MEDIUM:0, LOW:0, NONE:0 };
    const caseCounts = { "평가":0, "보상":0, "승진":0, "갈등":0, "노무":0 };

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
            data: [riskCounts.HIGH, riskCounts.MEDIUM, riskCounts.LOW, riskCounts.NONE],
            backgroundColor: ["#b91c1c","#d97706","#0ea5e9","#cbd5e1"],
            borderWidth: 0
          }]
        },
        options: { plugins:{ legend:{ labels:{ font:{family:'Noto Sans KR'} } } } }
      });

      caseChart = new Chart(barCtx, {
        type: "bar",
        data: {
          labels: Object.keys(caseCounts),
          datasets: [{ label:"시도 횟수", data:Object.values(caseCounts), backgroundColor:"#1f2a44" }]
        },
        options: {
          plugins:{ legend:{ labels:{ font:{family:'Noto Sans KR'} } } },
          scales:{ x:{ ticks:{ font:{family:'Noto Sans KR'} } }, y:{ beginAtZero:true, ticks:{ font:{family:'Noto Sans KR'} } } }
        }
      });
    }

    const STORAGE_KEY = "lcc_history_v2";
    function loadHistory(){ try{ return JSON.parse(localStorage.getItem(STORAGE_KEY)||"[]"); }catch(e){ return []; } }
    function saveHistory(items){ localStorage.setItem(STORAGE_KEY, JSON.stringify(items)); }
    function pushHistory(entry){ const arr = loadHistory(); arr.unshift(entry); saveHistory(arr.slice(0,50)); }
    function renderHistory(){
      const items = loadHistory();
      historyList.innerHTML = "";
      items.slice(0,10).forEach(it=>{
        const badge = it.Feedback.Risk.level==="HIGH" ? "badge badge-high"
                   : it.Feedback.Risk.level==="MEDIUM" ? "badge badge-medium"
                   : it.Feedback.Risk.level==="LOW" ? "badge badge-low" : "badge badge-none";
        const li = document.createElement("li");
        li.className = "surface p-3";
        li.innerHTML = `
          <div class="flex items-center justify-between">
            <div class="font-semibold text-primary">${it.case} · ${it.scenarioKey}</div>
            <span class="${badge}">${it.Feedback.Risk.level}</span>
          </div>
          <div class="mt-1 text-support text-sm">${it.UserInput}</div>
        `;
        historyList.appendChild(li);
      });
    }

    /* =========== 4) 상호작용 + Active State =========== */
    const caseButtons = Array.from(document.querySelectorAll(".grid-cases .case"));

    function setActiveCaseButton(btn){
      caseButtons.forEach(b=>{
        b.classList.remove("case--active");
        b.setAttribute("aria-pressed","false");
      });
      btn.classList.add("case--active");
      btn.setAttribute("aria-pressed","true");
    }

    function handleCaseSelect(btn){
      currentCase = btn.dataset.case || btn.querySelector(".font-bold")?.textContent?.trim();
      setActiveCaseButton(btn);
      populateScenarioOptions();
      selectedCaseBadge.textContent = currentCase;
      selectedCaseBadge.classList.remove("hidden");
    }

    caseButtons.forEach(btn=>{
      btn.addEventListener("click", ()=>handleCaseSelect(btn));
      btn.setAttribute("tabindex","0");
      btn.addEventListener("keydown",(e)=>{
        if(e.key==="Enter"||e.key===" "){
          e.preventDefault(); handleCaseSelect(btn);
        }
      });
    });

    function populateScenarioOptions(){
      scenarioType.innerHTML = "";
      if(!currentCase){
        scenarioType.innerHTML = `<option value="">케이스를 먼저 선택하세요</option>`;
        scenarioText.textContent = "케이스를 선택하세요.";
        return;
      }
      scenarioType.insertAdjacentHTML("beforeend", `<option value="">상황을 선택하세요</option>`);
      scenarios[currentCase].forEach((s, idx)=>{
        const preview = s.text.replace(/^직원:\s?/, "").slice(0,28) + (s.text.length>28?"...":"");
        scenarioType.insertAdjacentHTML("beforeend", `<option value="${idx}">[${s.key}] ${preview}</option>`);
      });
      scenarioText.textContent = "상황을 선택하세요.";
    }

    scenarioType.addEventListener("change", ()=>{
      const idx = scenarioType.value;
      scenarioText.textContent = (!currentCase||idx==="") ? "상황을 선택하세요." : scenarios[currentCase][Number(idx)].text;
    });

    function badgeClass(level){
      return level==="HIGH" ? "badge badge-high"
           : level==="MEDIUM" ? "badge badge-medium"
           : level==="LOW" ? "badge badge-low" : "badge badge-none";
    }

    /* =========== 5) 안전한 코칭 생성 (Gemini 없으면 로컬 폴백) =========== */
    async function callGeminiSafe(prompt){
      const apiKey = ""; // 필요 시 여기에 키 입력. 없으면 로컬 규칙 기반 폴백 사용.
      if(!apiKey){ return localCoach(prompt); }

      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
      const payload = {
        contents:[{role:"user", parts:[{text:prompt}]}],
        generationConfig:{
          responseMimeType:"application/json",
          responseSchema:{
            type:"OBJECT",
            properties:{
              positive:{type:"STRING"},
              improvement:{type:"STRING"},
              riskLevel:{type:"STRING","enum":["HIGH","MEDIUM","LOW","NONE"]},
              riskNotes:{type:"STRING"},
              bestPractice:{type:"STRING"},
              nextStep:{type:"STRING"}
            },
            required:["positive","improvement","riskLevel","riskNotes","bestPractice","nextStep"]
          }
        }
      };

      try{
        const res = await fetch(apiUrl,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify(payload)});
        if(!res.ok) throw new Error(`API ${res.status}`);
        const json = await res.json();
        const txt = json?.candidates?.[0]?.content?.parts?.[0]?.text;
        return JSON.parse(txt);
      }catch(e){
        console.warn("Gemini 호출 실패, 로컬 폴백 사용:", e);
        return localCoach(prompt);
      }
    }

    // 로컬 규칙 기반 코칭(오프라인/키 없음/에러 시)
    function localCoach(prompt){
      const t = leaderInput.value.trim() + " " + scenarioText.textContent.trim();
      const hasEmp = /감사|공감|이해|그럴 수|경청/.test(t);
      const badTone = /너|네가|당연히|원래|말이 안돼|무조건/.test(t);
      const risk = /신고|노동청|차별|괴롭힘/.test(t) ? "HIGH"
                 : /보복|부당|불이익/.test(t) ? "MEDIUM" : "LOW";

      const positive = hasEmp ? "공감 표현과 경청으로 신뢰 형성이 잘 되었습니다." : "첫 응답을 시도하신 점은 좋습니다. 공감 문장을 먼저 제시하면 더 효과적입니다.";
      const improvement = badTone ? "단정적·감정적 표현을 줄이고 객관 기준과 절차 중심으로 설명하세요." : "근거(기록·지표) 제시와 HR 절차(이의제기/조정) 안내를 병행하세요.";
      const bestPractice = "말씀해 주셔서 감사합니다. 그렇게 느끼실 수 있습니다. 사전에 공지된 기준과 기록을 근거로 사실관계를 확인해 드리겠습니다. 필요한 경우 HR 절차(이의제기/조정)도 안내하겠습니다.";
      const riskNotes = risk==="HIGH" ? "고위험 키워드 감지: 즉시 HR 경로로 연결 필요" :
                        risk==="MEDIUM" ? "중위험: 신중한 표현·절차 병행 권장" : "즉시 리스크 낮음: 기록·후속 점검 권장";
      const nextStep = "같은 케이스의 다른 상황을 연습하거나 모범 문장을 자신의 말로 재구성해 보세요.";

      return {positive, improvement, riskLevel:risk, riskNotes, bestPractice, nextStep};
    }

    coachBtn.addEventListener("click", async ()=>{
      toast.textContent = "";
      if(!currentCase){ toast.textContent="케이스를 먼저 선택하세요."; return; }
      if(scenarioType.value===""){ toast.textContent="상황을 선택하세요."; return; }
      const scenario = scenarioText.textContent.trim();
      const input = leaderInput.value.trim();
      if(!input){ toast.textContent="답변을 입력하세요."; return; }

      // 로딩 상태
      coachBtn.disabled = true;
      coachBtn.textContent = "코칭 중...";
      toast.textContent = "피드백을 생성 중입니다...";

      const userPrompt = `
        시나리오: "${scenario}"
        리더 답변: "${input}"
        위 대화에 대해 HR 코칭 피드백을 JSON으로 반환.
      `;

      try{
        const feedback = await callGeminiSafe(userPrompt);

        fbPanel.classList.remove("hidden");
        fbPositive.textContent = feedback.positive;
        fbImprovement.textContent = feedback.improvement;
        fbBest.textContent = feedback.bestPractice;
        fbNext.textContent = feedback.nextStep;
        riskBadge.textContent = feedback.riskLevel;
        riskBadge.className = badgeClass(feedback.riskLevel);
        hrBtn.classList.toggle("hidden", feedback.riskLevel!=="HIGH");

        copyBestBtn.classList.remove("hidden");
        copyBestBtn.onclick = async ()=>{
          try{
            if(navigator.clipboard?.writeText){
              await navigator.clipboard.writeText(feedback.bestPractice);
            }else{
              const ta=document.createElement("textarea");
              ta.value=feedback.bestPractice; document.body.appendChild(ta);
              ta.select(); document.execCommand("copy"); document.body.removeChild(ta);
            }
            toast.textContent="모범 대화가 복사되었습니다.";
          }catch{ toast.textContent="복사에 실패했습니다. 수동으로 복사하세요."; }
          setTimeout(()=>toast.textContent="",1500);
        };

        // 차트/히스토리 갱신
        riskCounts[feedback.riskLevel] = (riskCounts[feedback.riskLevel]||0) + 1;
        caseCounts[currentCase] = (caseCounts[currentCase]||0) + 1;
        initCharts();

        const idx = Number(scenarioType.value);
        const key = scenarios[currentCase][idx].key;
        const entry = {
          Scenario: scenario, UserInput: input,
          Feedback: { Positive: feedback.positive, Improvement: feedback.improvement,
            Risk: { level: feedback.riskLevel, notes: feedback.riskNotes, escalateToHR:(feedback.riskLevel==="HIGH") },
            BestPractice: feedback.bestPractice
          },
          NextStep: feedback.nextStep, case: currentCase, scenarioKey: key, time: Date.now()
        };
        pushHistory(entry); renderHistory();

      }catch(e){
        console.error(e);
        toast.textContent = "피드백 생성에 실패했습니다. 다시 시도해 주세요.";
      }finally{
        coachBtn.disabled = false;
        coachBtn.textContent = "코칭 받기";
        if(!toast.textContent.includes("실패")) setTimeout(()=>toast.textContent="",2000);
      }
    });

    // HR 가이드 버튼(간단 모달)
    document.getElementById("hrBtn").addEventListener("click", ()=>{
      const modal = document.createElement("div");
      modal.className = "fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center z-50";
      modal.innerHTML = `
        <div class="bg-white p-6 rounded-lg shadow-xl max-w-sm mx-auto text-center">
          <h4 class="font-bold text-lg mb-3">HR 연결 가이드</h4>
          <p class="text-gray-700 text-sm">
            즉시 HR에 사실관계 보고 → 공식 절차(이의제기/조사) 안내 → 문서화/기록 유지 → 2차 피해 방지 조치
          </p>
          <button id="closeModal" type="button" class="btn btn-primary mt-4">확인</button>
        </div>`;
      document.body.appendChild(modal);
      modal.querySelector("#closeModal").addEventListener("click", ()=> document.body.removeChild(modal));
    });

    // 초기화/내보내기
    document.getElementById("resetBtn").addEventListener("click", ()=>{
      leaderInput.value = "";
      fbPanel.classList.add("hidden");
      copyBestBtn.classList.add("hidden");
      toast.textContent="";
      // 차트 데이터 초기화
      for (const k in riskCounts) riskCounts[k]=0;
      for (const k in caseCounts) caseCounts[k]=0;
      initCharts();
      // 히스토리 초기화
      localStorage.removeItem(STORAGE_KEY);
      renderHistory();
    });

    document.getElementById("exportBtn").addEventListener("click", ()=>{
      const data = loadHistory();
      const blob = new Blob([JSON.stringify(data, null, 2)], {type:"application/json"});
      const a = document.createElement("a");
      const date = new Date().toISOString().slice(0,10).replaceAll("-","");
      a.href = URL.createObjectURL(blob);
      a.download = `leader-conflict-coach-history-${date}.json`;
      a.click(); URL.revokeObjectURL(a.href);
    });

    // 초기 구동
    (function init(){ renderHistory(); initCharts(); })();
  </script>
</body>
</html>
