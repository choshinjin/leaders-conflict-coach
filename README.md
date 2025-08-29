<!DOCTYPE html>
<html lang="ko" class="no-fouc">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>웅진 신입사원 온보딩 교육</title>

  <!-- 1) TailwindCDN (개발용) : FOUC 최소화 위해 body 숨김 → load 이후 표시 -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- 2) Fonts 최적화 -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <!-- display=swap으로 폰트 로딩 중 깜빡임 최소화 -->
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet"/>

  <style>
    /* ====== 기초 스타일 ====== */
    html.no-fouc { visibility: hidden; } /* 초기 숨김 → onload때 해제 */
    body {
      font-family: 'Noto Sans KR', system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
      background-color: #E3F2FD;
      color: #1565C0;
      scroll-behavior: smooth; /* CSS만으로 부드러운 스크롤 */
    }
    .text-primary-blue { color: #1E88E5; }
    .text-dark-blue { color: #1565C0; }
    .text-light-blue { color: #BBDEFB; }
    .bg-primary-blue { background-color: #1E88E5; }
    .bg-dark-blue { background-color: #1565C0; }
    .bg-light-blue { background-color: #BBDEFB; }
    .bg-pastel-blue { background-color: #E3F2FD; }
    .bg-gradient-hero { background-image: linear-gradient(to bottom, #BBDEFB, #E3F2FD); }
    .btn-orange { background-color: #FFB300; }
    .btn-green { background-color: #43A047; }
    .text-orange { color: #FFB300; }
    .text-green { color: #43A047; }

    /* 모달 */
    .modal {
      display: none; position: fixed; z-index: 1000; inset: 0;
      width: 100%; height: 100%; overflow: auto;
      background-color: rgba(0,0,0,0.4);
      justify-content: center; align-items: center;
      animation: fadeIn .3s ease-out;
    }
    .modal-content {
      background: #fff; padding: 2.5rem; border-radius: 1rem;
      max-width: 90%; width: 500px; box-shadow: 0 4px 6px rgba(0,0,0,.1);
      animation: popIn .3s cubic-bezier(.175,.885,.32,1.275);
    }

    .section-card { margin-bottom: 2rem; }
    .section-title {
      font-size: 2rem; font-weight: 700; text-align: center; margin-bottom: 2rem;
      color: #1565C0; display: flex; align-items: center; justify-content: center; gap: 1rem;
    }

    .loading-spinner {
      border: 4px solid rgba(0,0,0,.1); border-left-color: #1E88E5;
      border-radius: 50%; width: 24px; height: 24px; animation: spin 1s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }
    @keyframes fadeIn { from {opacity:0} to {opacity:1} }
    @keyframes popIn { from {transform:scale(.9); opacity:0} to {transform:scale(1); opacity:1} }

    /* 유틸: 버튼 기본값(일부 브라우저에서 submit 방지) */
    button { all: unset; display: inline-block; }
    .btn {
      padding: .75rem 1.25rem; border-radius: 9999px; font-weight: 700;
      color: #fff; box-shadow: 0 4px 12px rgba(0,0,0,.08);
      transition: transform .05s ease, opacity .2s ease, background .2s ease;
      cursor: pointer;
    }
    .btn:active { transform: translateY(1px); }
  </style>
</head>

<body>
  <!-- Sticky Navigation -->
  <nav class="sticky top-0 z-50 bg-primary-blue shadow-lg">
    <div class="container mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center space-y-2 md:space-y-0">
      <a href="#hero" class="text-xl md:text-2xl font-bold text-white">Woongjin Onboarding</a>
      <div class="flex flex-wrap justify-center md:justify-end gap-2 text-sm md:text-base">
        <a href="#principles" class="text-white hover:text-light-blue transition">핵심 원칙</a>
        <a href="#basic-qualities" class="text-white hover:text-light-blue transition">기본 소양</a>
        <a href="#attitude" class="text-white hover:text-light-blue transition">기본 자세</a>
        <a href="#manners" class="text-white hover:text-light-blue transition">예절과 매너</a>
        <a href="#quiz" class="text-white hover:text-light-blue transition">O/X 퀴즈</a>
      </div>
    </div>
  </nav>

  <main class="container mx-auto px-4 py-8 md:py-16 space-y-16">
    <!-- Hero -->
    <section id="hero" class="text-center py-20 md:py-32 rounded-3xl bg-gradient-hero shadow-xl">
      <h1 class="text-3xl md:text-5xl font-bold text-primary-blue leading-tight mb-4">
        직장인이 아닌 <span class="text-dark-blue">웅진인</span>으로서,
      </h1>
      <p class="text-lg md:text-xl text-dark-blue max-w-2xl mx-auto mb-8 bg-light-blue py-2 px-4 rounded-full inline-block">
        <span class="font-bold text-primary-blue">함께 성장하는 새로운 여정이 시작됩니다.</span> ✈️
      </p>
    </section>

    <!-- Good People Core Principles -->
    <section id="principles" class="section-card">
      <h2 class="section-title">Good People 핵심 원칙</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div class="p-6 rounded-2xl bg-white shadow-lg">
          <h3 class="text-2xl font-bold mb-4"><span class="text-gray-500">직장인</span> vs <span class="text-dark-blue">웅진인</span></h3>
          <p class="text-lg text-gray-700">
            <span class="font-bold text-gray-500">직장인</span>은 단순히 '다니는 곳'이 있는 사람입니다. 급여와 복지를 중심으로 생각하며, 정해진 시간 동안 맡은 업무를 처리하는 것에 초점을 둡니다.
          </p>
          <p class="text-lg text-gray-700 mt-4">
            반면, <span class="font-bold text-primary-blue">웅진인</span>은 '<span class="font-bold text-orange">함께 성장하는 곳</span>'이 있는 사람입니다. 주인의식과 사명감을 가지고, 일의 의미와 가치를 찾으며 능동적으로 성장하는 자세를 갖춥니다.
          </p>
        </div>
        <div class="p-6 rounded-2xl bg-white shadow-lg">
          <h3 class="text-2xl font-bold mb-4">또또사랑</h3>
          <p class="text-lg text-gray-700 leading-relaxed mb-4">
            또또사랑은 변화, 일, 도전, 조직, 사회, 고객을 사랑하고,<br/>또 사랑하고, 또 사랑한다는 의미입니다.
          </p>
          <div class="space-y-4">
            <div class="flex items-start">
              <span class="text-3xl text-primary-blue mr-4">✨</span>
              <div>
                <h4 class="font-bold text-lg">고객중심</h4>
                <p class="text-gray-600 text-sm">고객의 입장에서 생각하고 장기적인 관계를 지향합니다.</p>
              </div>
            </div>
            <div class="flex items-start">
              <span class="text-3xl text-primary-blue mr-4">🤝</span>
              <div>
                <h4 class="font-bold text-lg">우리 임직원</h4>
                <p class="text-gray-600 text-sm">가장 중요한 자산은 사람, 임직원의 행복이 곧 경쟁력입니다.</p>
              </div>
            </div>
            <div class="flex items-start">
              <span class="text-3xl text-primary-blue mr-4">🌱</span>
              <div>
                <h4 class="font-bold text-lg">의미찾기</h4>
                <p class="text-gray-600 text-sm">"왜 이 일을 하는가"에 대한 답을 찾을 때 진정한 가치가 탄생합니다.</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 기본 소양 -->
    <section id="basic-qualities" class="section-card">
      <h2 class="section-title">사회인으로서 갖춰야 할 기본 소양</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="p-6 rounded-2xl bg-white shadow-lg">
          <h3 class="text-2xl font-bold mb-2 text-primary-blue">배려와 태도</h3>
          <ul class="list-disc pl-6 text-gray-700 space-y-1">
            <li>작은 배려가 큰 신뢰로 이어집니다.</li>
            <li>능력보다 중요한 것은 태도입니다.</li>
            <li>책임감 있게 행동하는 웅진인이 되세요.</li>
          </ul>
        </div>
        <div class="p-6 rounded-2xl bg-white shadow-lg">
          <h3 class="text-2xl font-bold mb-2 text-primary-blue">팀워크와 소통</h3>
          <ul class="list-disc pl-6 text-gray-700 space-y-1">
            <li>혼자 가면 빨리, 함께 가면 멀리 갑니다.</li>
            <li>열린 대화와 다양한 관점을 존중하세요.</li>
            <li>효과적인 소통이 성공적인 팀을 만듭니다.</li>
          </ul>
        </div>
        <div class="p-6 rounded-2xl bg-white shadow-lg">
          <h3 class="text-2xl font-bold mb-2 text-primary-blue">학습과 성장</h3>
          <ul class="list-disc pl-6 text-gray-700 space-y-1">
            <li>경쟁력은 끊임없는 학습에서 비롯됩니다.</li>
            <li>학습하는 전문인, 혁신하는 창의인이 되세요.</li>
            <li>의미를 찾는 과정 자체가 성장입니다.</li>
          </ul>
        </div>
      </div>
    </section>

    <!-- 신입사원 자세 -->
    <section id="attitude" class="section-card">
      <h2 class="section-title">신입사원의 기본 자세 & 태도</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="p-6 rounded-2xl bg-white shadow-lg text-center">
          <span class="text-4xl mb-2 block">🙇‍♂️</span>
          <h3 class="text-2xl font-bold mb-2">겸손과 배움</h3>
          <p class="text-base text-gray-700">모든 순간을 성장의 기회로 만드세요.</p>
        </div>
        <div class="p-6 rounded-2xl bg-white shadow-lg text-center">
          <span class="text-4xl mb-2 block">💪</span>
          <h3 class="text-2xl font-bold mb-2">실수는 성장</h3>
          <p class="text-base text-gray-700">실패를 두려워하지 않는 용기있는 자세가 필요합니다.</p>
        </div>
        <div class="p-6 rounded-2xl bg-white shadow-lg text-center">
          <span class="text-4xl mb-2 block">⭐</span>
          <h3 class="text-2xl font-bold mb-2">책임감과 주인의식</h3>
          <p class="text-base text-gray-700">내 일에 대한 주인의식이 성공을 만듭니다.</p>
        </div>
      </div>
    </section>

    <!-- 예절과 매너 -->
    <section id="manners" class="section-card">
      <h2 class="section-title">예절과 매너</h2>
      <div class="p-8 rounded-2xl bg-white shadow-lg space-y-4">
        <p class="text-lg text-gray-700 leading-relaxed">
          조직 문화의 핵심인 비즈니스 매너는 신뢰를 구축하고 원활한 협업을 이끄는 중요한 요소입니다. 특히 다음 4가지는 신입사원으로서 반드시 갖춰야 할 기본 소양입니다.
        </p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          <div class="text-center p-4 bg-gray-50 rounded-lg shadow-sm">
            <span class="text-4xl mb-2 block">⏰</span>
            <p class="font-bold text-2xl">시간 예절</p>
            <p class="text-base text-gray-600 mt-1">약속과 마감 시간을 지키는 것은 상대방에 대한 존중의 시작입니다.</p>
          </div>
          <div class="text-center p-4 bg-gray-50 rounded-lg shadow-sm">
            <span class="text-4xl mb-2 block">💬</span>
            <p class="font-bold text-2xl">커뮤니케이션 예절</p>
            <p class="text-base text-gray-600 mt-1">명확하고 간결한 소통은 업무 효율을 높이고 오해를 줄입니다.</p>
          </div>
          <div class="text-center p-4 bg-gray-50 rounded-lg shadow-sm">
            <span class="text-4xl mb-2 block">🤝</span>
            <p class="font-bold text-2xl">관계 예절</p>
            <p class="text-base text-gray-600 mt-1">경청, 감사 표현은 건강한 관계를 만들고 신뢰를 쌓습니다.</p>
          </div>
          <div class="text-center p-4 bg-gray-50 rounded-lg shadow-sm">
            <span class="text-4xl mb-2 block">🏢</span>
            <p class="font-bold text-2xl">공간 예절</p>
            <p class="text-base text-gray-600 mt-1">공용 공간을 깨끗이 사용하고 소음을 조절하는 것은 배려의 기본입니다.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- O/X 퀴즈 -->
    <section id="quiz" class="section-card">
      <h2 class="section-title">O/X 퀴즈</h2>
      <p class="text-lg text-dark-blue mb-8 text-center">총 10문제, 여러분의 지식을 확인해보세요!</p>

      <div id="quiz-container" class="space-y-6"></div>

      <div class="flex justify-center mt-8">
        <button id="submit-quiz" type="button"
          class="btn btn-orange hover:opacity-90 transition py-3 px-8 text-lg font-bold rounded-full shadow-lg opacity-50 cursor-not-allowed"
          disabled>결과 확인하기</button>
      </div>
    </section>

    <!-- Outro (optional) -->
    <section id="outro" class="text-center py-20 md:py-32 rounded-3xl bg-gradient-hero shadow-xl hidden">
      <h1 class="text-3xl md:text-5xl font-bold text-primary-blue leading-tight mb-4">사람 또 사람, 그리고 사랑!</h1>
      <h2 class="text-2xl md:text-3xl font-bold text-dark-blue leading-tight mb-8">그 놀라운 힘을 믿습니다.</h2>
      <p class="text-lg md:text-xl text-gray-700 max-w-2xl mx-auto">함께 성장하는 여정이 시작됩니다.</p>
    </section>

    <!-- 최종 화면 -->
    <section id="final-page" class="text-center py-20 md:py-32 rounded-3xl bg-gradient-hero shadow-xl hidden">
      <h1 class="text-3xl md:text-5xl font-bold text-primary-blue leading-tight mb-4">여러분 모두가 웅진에서 오래도록 성장하며,</h1>
      <h2 class="text-2xl md:text-3xl font-bold text-dark-blue leading-tight mb-8">서로에게 긍정적인 영향을 주는 동료가 되어주시길 바랍니다.</h2>
    </section>

    <!-- Footer -->
    <footer class="text-center py-8 text-sm text-gray-500">
      <p>본 교육 자료는 웅진의 신입사원 온보딩 프로그램을 위해 제작되었습니다.</p>
      <p>Copyright © 2025 Woongjin. All Rights Reserved.</p>
    </footer>
  </main>

  <!-- 결과 모달 -->
  <div id="result-modal" class="modal" aria-hidden="true">
    <div class="modal-content text-center space-y-4" role="dialog" aria-modal="true" aria-labelledby="modal-title">
      <h3 id="modal-title" class="text-3xl font-bold text-dark-blue">퀴즈 결과</h3>
      <p id="modal-score" class="text-5xl font-extrabold text-orange">0점</p>
      <p id="modal-message" class="text-xl text-gray-700"></p>
      <div class="flex justify-center mt-4 space-x-4">
        <button id="close-modal" type="button" class="btn bg-primary-blue hover:opacity-90 px-6 py-2 rounded-full">닫기</button>
        <button id="retry-quiz" type="button" class="btn btn-green hover:opacity-90 px-6 py-2 rounded-full">다시 풀기</button>
        <button id="exit-quiz" type="button" class="btn bg-light-blue hover:opacity-90 px-6 py-2 rounded-full text-blue-900">나가기</button>
      </div>
    </div>
  </div>

  <script>
    // ====== 0) 초기 FOUC 방지: 모든 리소스 로드 후 표시 ======
    window.addEventListener('load', () => {
      document.documentElement.classList.remove('no-fouc');
    });

    // ====== 1) 앵커 스크롤: CSS로 충분하여 JS 스크롤 로직 제거 ======
    // (중복 스크롤 로직이 있으면 순간 점프/리레이아웃이 생길 수 있어 제거)

    // ====== 2) 퀴즈 데이터 ======
    const quizData = [
      { question: "웅진의 핵심 가치는 '또또사랑'입니다.", answer: "O",
        explanation: "또또사랑은 변화, 일, 도전, 조직, 사회, 고객을 사랑하고, 또 사랑하고, 또 사랑한다는 의미입니다." },
      { question: "신입사원의 진정한 실력은 화려한 스펙에 있습니다.", answer: "X",
        explanation: "웅진에서 진정한 실력은 스펙이 아닌, 끊임없는 경험과 이를 통한 꾸준한 성장과 지혜를 의미합니다." },
      { question: "실수는 성장의 기회가 될 수 없습니다.", answer: "X",
        explanation: "실수는 성장의 발판이 될 수 있습니다. 실수를 통해 배우고 더 나은 방향으로 나아갈 수 있습니다." },
      { question: "업무의 성공 여부는 책임감과 주인의식에 달려있습니다.", answer: "O",
        explanation: "자신의 업무에 대한 책임감과 주인의식은 성공적인 결과를 이끌어내는 중요한 태도입니다." },
      { question: "효과적인 커뮤니케이션은 듣는 것보다 말하는 것이 더 중요합니다.", answer: "X",
        explanation: "효과적인 커뮤니케이션은 말하기와 듣기 모두 중요하며, 특히 상대방의 의견을 경청하는 자세가 필요합니다." },
      { question: "웅진인은 주어진 업무만 처리하는 직장인입니다.", answer: "X",
        explanation: "웅진인은 주어진 업무를 넘어 주인의식과 사명감을 가지고 능동적으로 일합니다." },
      { question: "문제 해결력은 신입사원에게 요구되는 핵심 역량 중 하나입니다.", answer: "O",
        explanation: "실무 기본 역량 중 하나로, 문제 해결 능력은 신입사원에게 매우 중요합니다." },
      { question: "개인적인 시간 관리는 직장 내 예절에 포함되지 않습니다.", answer: "X",
        explanation: "약속 시간 준수 등 시간 관리 능력은 직장 내 기본 예절에 속합니다." },
      { question: "꾸준한 학습 태도는 성장에 필수적입니다.", answer: "O",
        explanation: "꾸준한 학습은 변화하는 환경에 적응하고 개인의 역량을 지속적으로 발전시키는 데 필수적입니다." },
      { question: "협업 능력은 신입사원에게 중요하지 않습니다.", answer: "X",
        explanation: "협업은 팀워크를 통해 더 큰 성과를 창출하는 데 핵심적인 역량입니다." }
    ];

    // ====== 3) 상태 ======
    let answeredQuestions = new Set();
    const quizContainer = document.getElementById('quiz-container');
    const submitButton  = document.getElementById('submit-quiz');
    const modal         = document.getElementById('result-modal');
    const closeModalBtn = document.getElementById('close-modal');
    const retryQuizBtn  = document.getElementById('retry-quiz');
    const exitQuizBtn   = document.getElementById('exit-quiz');
    const finalPage     = document.getElementById('final-page');

    // ====== 4) 렌더 ======
    function renderQuiz() {
      quizContainer.innerHTML = '';
      answeredQuestions.clear();
      updateSubmitButtonState();

      quizData.forEach((item, index) => {
        const wrap = document.createElement('div');
        wrap.className = 'bg-white p-6 rounded-xl shadow-md space-y-4';
        wrap.innerHTML = `
          <p class="text-lg font-bold">Q${index + 1}. ${item.question}</p>
          <div class="flex flex-row justify-center gap-8">
            <div class="relative">
              <button type="button" class="quiz-option btn w-20 h-20 md:w-24 md:h-24 rounded-full font-bold transition flex items-center justify-center text-3xl md:text-4xl bg-gray-200 text-gray-700" data-answer="O" data-index="${index}">O</button>
              <span class="ringO absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-20 h-20 md:w-24 md:h-24 rounded-full border-4 border-transparent pointer-events-none transition"></span>
            </div>
            <div class="relative">
              <button type="button" class="quiz-option btn w-20 h-20 md:w-24 md:h-24 rounded-full font-bold transition flex items-center justify-center text-3xl md:text-4xl bg-gray-200 text-gray-700" data-answer="X" data-index="${index}">X</button>
              <span class="ringX absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-20 h-20 md:w-24 md:h-24 rounded-full border-4 border-transparent pointer-events-none transition"></span>
            </div>
          </div>
          <div id="feedback-${index}" class="text-base mt-2 hidden"></div>
        `;
        quizContainer.appendChild(wrap);
      });

      document.querySelectorAll('.quiz-option').forEach(btn=>{
        btn.addEventListener('click', handleAnswer);
      });
    }

    function updateSubmitButtonState(){
      if (answeredQuestions.size === quizData.length) {
        submitButton.classList.remove('opacity-50','cursor-not-allowed');
        submitButton.disabled = false;
      } else {
        submitButton.classList.add('opacity-50','cursor-not-allowed');
        submitButton.disabled = true;
      }
    }

    function handleAnswer(e){
      const btn = e.currentTarget;
      const idx = +btn.dataset.index;
      const container = btn.closest('.bg-white');
      const selected = btn.dataset.answer;
      const correct  = quizData[idx].answer;
      const feedback = document.getElementById(`feedback-${idx}`);
      const buttons  = container.querySelectorAll('.quiz-option');

      // 초기화
      buttons.forEach(b=>{
        b.classList.remove('bg-primary-blue','bg-orange','text-white');
        b.classList.add('bg-gray-200','text-gray-700');
        const ring = b.nextElementSibling;
        ring.classList.remove('border-primary-blue','border-orange');
        ring.classList.add('border-transparent');
      });

      if (selected === correct) {
        btn.classList.remove('bg-gray-200','text-gray-700');
        btn.classList.add('bg-primary-blue','text-white');
        btn.nextElementSibling.classList.add('border-primary-blue');
        feedback.innerHTML = `<span class="text-primary-blue font-bold">정답입니다!</span><br>${quizData[idx].explanation}`;

        buttons.forEach(b=>{ b.setAttribute('disabled','true'); b.classList.add('cursor-not-allowed'); });
        answeredQuestions.add(idx);
        updateSubmitButtonState();
      } else {
        btn.classList.remove('bg-gray-200','text-gray-700');
        btn.classList.add('bg-orange','text-white');
        btn.nextElementSibling.classList.add('border-orange');
        feedback.innerHTML = `<span class="text-orange font-bold">다시 한번 생각해봐요!</span>`;
      }
      feedback.classList.remove('hidden');
    }

    // 결과 모달
    submitButton.addEventListener('click', ()=>{
      let score = 0;
      const cards = document.querySelectorAll('#quiz-container > div');
      cards.forEach((card, i)=>{
        const corr = quizData[i].answer;
        const corrBtn = card.querySelector(`button[data-answer="${corr}"]`);
        if (corrBtn && corrBtn.classList.contains('bg-primary-blue')) score++;
      });
      const pct = (score / quizData.length) * 100;
      let msg = '';
      if (pct >= 90) msg = "👏 완벽해요! 핵심 가치를 완벽하게 이해하고 계시네요. 웅진의 훌륭한 인재가 되실 겁니다.";
      else if (pct >= 70) msg = "👍 잘했어요! 대부분의 내용을 잘 숙지하고 계십니다. 몇 가지 부분을 다시 확인하면 더욱 좋습니다.";
      else msg = "🌱 조금 더 노력해봐요! 이번 교육 내용을 다시 한번 살펴보는 것을 추천합니다. 함께 성장해 나가요!";

      document.getElementById('modal-score').innerText = `${score} / ${quizData.length} (${pct.toFixed(0)}%)`;
      document.getElementById('modal-message').innerText = msg;
      modal.style.display = 'flex';
      modal.setAttribute('aria-hidden','false');
    });

    document.getElementById('close-modal').addEventListener('click', ()=>{
      modal.style.display = 'none';
      modal.setAttribute('aria-hidden','true');
    });
    document.getElementById('retry-quiz').addEventListener('click', ()=>{
      modal.style.display = 'none';
      modal.setAttribute('aria-hidden','true');
      renderQuiz();
      document.getElementById('quiz').scrollIntoView({behavior:'smooth'});
    });
    document.getElementById('exit-quiz').addEventListener('click', ()=>{
      modal.style.display = 'none';
      modal.setAttribute('aria-hidden','true');
      finalPage.classList.remove('hidden');
      finalPage.scrollIntoView({behavior:'smooth'});
    });

    // 초기 렌더
    renderQuiz();
  </script>
</body>
</html>
