<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Quiz – Les conflits en entreprise</title>
  <style>
    :root{
      --bg:#0f172a;          /* slate-900 */
      --card:#111827ee;      /* gray-900 */
      --text:#e5e7eb;        /* gray-200 */
      --muted:#9ca3af;       /* gray-400 */
      --accent:#22c55e;      /* green-500 */
      --accent-2:#60a5fa;    /* blue-400 */
      --danger:#ef4444;      /* red-500 */
      --warning:#f59e0b;     /* amber-500 */
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, "Helvetica Neue", Arial, "Apple Color Emoji", "Segoe UI Emoji";
      background: radial-gradient(1200px 600px at 80% -20%, #1e293b33, transparent),
                  radial-gradient(1200px 600px at -10% 120%, #0ea5e955, transparent),
                  var(--bg);
      color:var(--text);
      min-height:100dvh; display:flex; align-items:center; justify-content:center; padding:24px;
    }
    .app{width: min(900px, 100%);}
    .card{
      background: linear-gradient(180deg, #0b1222, #0b1222) padding-box,
                  linear-gradient(120deg, #22c55e55, #60a5fa55) border-box;
      border:1px solid transparent; border-radius:24px; box-shadow: 0 10px 40px #0008; padding:28px;
    }
    h1{font-size:clamp(22px, 3.5vw, 34px); margin:0 0 8px; letter-spacing:.4px}
    .subtitle{color:var(--muted); margin:0 0 20px}
    .controls{display:flex; gap:12px; flex-wrap:wrap; align-items:center; margin:12px 0 22px}
    button{
      border:none; border-radius:14px; padding:12px 16px; font-weight:600; cursor:pointer;
      background:linear-gradient(180deg, #22c55e, #16a34a); color:#04110a; transition:transform .06s ease, filter .2s ease; box-shadow:0 6px 18px #16a34a44;
    }
    button.secondary{background:linear-gradient(180deg, #60a5fa, #3b82f6); color:#041021; box-shadow:0 6px 18px #3b82f644}
    button.ghost{background:#1f293799; color:var(--text); box-shadow:none; border:1px solid #334155}
    button:active{transform:translateY(1px)}
    .meta{display:flex; gap:16px; flex-wrap:wrap; color:var(--muted); font-size:14px}
    .pill{border:1px solid #334155; padding:6px 10px; border-radius:999px}
    .question{margin:18px 0 10px; font-weight:700}
    .qcard{background:#0b1222; border:1px solid #1f2937; border-radius:18px; padding:18px; margin:14px 0}
    .options{display:grid; gap:10px; margin-top:10px}
    label.opt{
      display:flex; gap:10px; align-items:flex-start; border:1px solid #1f2937; background:#0e162b; border-radius:12px; padding:12px 14px; cursor:pointer; transition:border-color .15s ease, background .15s ease;
    }
    label.opt:hover{border-color:#334155; background:#101b33}
    .opt input{margin-top:3px}
    .actions{display:flex; gap:10px; justify-content:flex-end; margin-top:14px}
    .hidden{display:none}
    .score{font-weight:800}
    .explain{color:#cbd5e1; font-size:14px; margin-top:8px; border-left:3px solid #334155; padding-left:10px}
    .correct{outline:2px solid #16a34a66}
    .wrong{outline:2px solid #ef444466}
    .footer-note{color:#9ca3af; font-size:12px; margin-top:12px}
    .result{
      background: linear-gradient(180deg, #04101d, #0b1222) padding-box,
                  linear-gradient(120deg, #f59e0b55, #22c55e55) border-box;
      border:1px solid transparent; border-radius:18px; padding:18px; margin-top:16px
    }
    @media (prefers-reduced-motion:no-preference){
      .fade{animation:fade .4s ease both}
      @keyframes fade{from{opacity:0; transform:translateY(4px)} to{opacity:1; transform:none}}
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="card fade">
      <h1>Quiz – Les conflits en entreprise</h1>
      <p class="subtitle">Testez vos connaissances : causes, impacts et gestion des conflits (exemples GE Healthcare inclus).</p>

      <div class="controls">
        <button id="startBtn">Démarrer le quiz</button>
        <button id="resetBtn" class="ghost hidden">Recommencer</button>
        <button id="showAnswersBtn" class="secondary hidden">Voir les réponses</button>
        <div class="meta">
          <span class="pill">Questions: <span id="qCount">0</span></span>
          <span class="pill">Score: <span id="score">0</span></span>
          <span class="pill">Temps: <span id="timer">00:00</span></span>
        </div>
      </div>

      <div id="quizContainer" class="hidden"></div>

      <div id="result" class="result hidden"></div>
      <p class="footer-note">Astuce: Répondez d'abord, puis cliquez sur « Voir les réponses » pour afficher les corrections et explications.</p>
    </div>
  </div>

  <script>
    const questions = [
      {
        q: "Le conflit en entreprise est…",
        opts: [
          "Toujours nuisible et à éviter absolument",
          "Naturel et parfois utile s'il est bien géré",
          "Synonyme d'échec managérial"
        ],
        answer: 1,
        explain: "Le conflit est inévitable dans toute organisation et peut devenir un levier d'amélioration s'il est géré de manière constructive."
      },
      {
        q: "Laquelle n'est PAS une cause fréquente de conflit ?",
        opts: [
          "Objectifs divergents",
          "Manque d'information",
          "Transparence parfaite des processus"
        ],
        answer: 2,
        explain: "La transparence tend à réduire les malentendus, donc ce n'est pas une cause habituelle de conflit."
      },
      {
        q: "Quel type de conflit oppose deux services (ex: Marketing vs R&D) ?",
        opts: [
          "Conflit intragroupe",
          "Conflit intergroupes",
          "Conflit interpersonnel"
        ],
        answer: 1,
        explain: "Intergroupes = entre des équipes/entités différentes au sein de l'organisation."
      },
      {
        q: "Parmi ces effets, lequel peut être POSITIF ?",
        opts: [
          "Baisse de productivité",
          "Turnover",
          "Innovation par confrontation des idées"
        ],
        answer: 2,
        explain: "Le conflit peut stimuler la créativité et amener des solutions nouvelles."
      },
      {
        q: "Exemple GE Healthcare: IRM en panne vs solution provisoire – la clé de résolution a été :",
        opts: [
          "Ignorer la panne et attendre",
          "Décision technique validée et communication claire",
          "Mettre toutes les demandes en pause"
        ],
        answer: 1,
        explain: "Validation technique + communication = remise en service rapide et sécurisée."
      },
      {
        q: "Exemple GE Healthcare: priorisation échographes en maternité – bonne pratique ?",
        opts: [
          "Traiter par ordre d'arrivée uniquement",
          "Prioriser l'impact médical (maternités) puis organiser le reste",
          "Prioriser les clients les plus proches géographiquement"
        ],
        answer: 1,
        explain: "On priorise selon l'impact patient/clinique, puis on planifie les autres interventions."
      },
      {
        q: "Outil de prévention des conflits :",
        opts: [
          "Management participatif et canaux d'écoute",
          "Suppression des réunions",
          "Rotation de tout le personnel chaque semaine"
        ],
        answer: 0,
        explain: "La prévention passe par l'écoute, la clarté des objectifs et l'inclusion."
      },
      {
        q: "Quel dispositif formalise QUI fait QUOI et QUAND ?",
        opts: [
          "Jeu de rôle",
          "Tableau de responsabilités / RACI",
          "Afterwork"
        ],
        answer: 1,
        explain: "Les matrices RACI/Responsabilités clarifient rôles, livrables et échéances."
      },
      {
        q: "Quand utiliser la médiation ?",
        opts: [
          "Lorsque les parties ne parviennent pas à se parler sereinement",
          "Toujours, dès le premier désaccord",
          "Jamais, la médiation est une perte de temps"
        ],
        answer: 0,
        explain: "La médiation est utile quand la communication directe est rompue ou trop chargée émotionnellement."
      },
      {
        q: "Après un conflit, clarifier les rôles permet…",
        opts: [
          "D'augmenter les zones grises",
          "De réduire les malentendus et accélérer l'exécution",
          "D'éviter toute prise d'initiative à l'avenir"
        ],
        answer: 1,
        explain: "Des rôles clairs = moins de frictions, plus d'efficacité."
      }
    ];

    // State
    let started = false;
    let submitted = false;
    let startTime = 0; let timerInterval = null;

    const quizContainer = document.getElementById('quizContainer');
    const startBtn = document.getElementById('startBtn');
    const resetBtn = document.getElementById('resetBtn');
    const showAnswersBtn = document.getElementById('showAnswersBtn');
    const qCountEl = document.getElementById('qCount');
    const scoreEl = document.getElementById('score');
    const timerEl = document.getElementById('timer');
    const resultEl = document.getElementById('result');

    qCountEl.textContent = questions.length;

    startBtn.addEventListener('click', () => {
      if (started) return;
      started = true;
      submitted = false;
      scoreEl.textContent = '0';
      buildQuiz();
      quizContainer.classList.remove('hidden');
      resetBtn.classList.remove('hidden');
      showAnswersBtn.classList.remove('hidden');
      startBtn.classList.add('hidden');
      resultEl.classList.add('hidden');
      startTimer();
      window.scrollTo({top:0, behavior:'smooth'});
    });

    resetBtn.addEventListener('click', () => {
      stopTimer(); timerEl.textContent = '00:00';
      quizContainer.innerHTML = '';
      started = false; submitted = false;
      startBtn.classList.remove('hidden');
      resetBtn.classList.add('hidden');
      showAnswersBtn.classList.add('hidden');
      resultEl.classList.add('hidden');
      scoreEl.textContent = '0';
      window.scrollTo({top:0, behavior:'smooth'});
    });

    showAnswersBtn.addEventListener('click', () => {
      if (!started) return;
      const {score} = grade(true);
      showSummary(score);
    });

    function buildQuiz(){
      const frag = document.createDocumentFragment();
      questions.forEach((item, idx) => {
        const q = document.createElement('div');
        q.className = 'qcard';
        q.innerHTML = `
          <div class="question">Q${idx+1}. ${item.q}</div>
          <div class="options">
            ${item.opts.map((opt,i)=>`
              <label class="opt">
                <input type="radio" name="q${idx}" value="${i}" />
                <span>${opt}</span>
              </label>
            `).join('')}
          </div>
          <div class="explain hidden">${item.explain}</div>
        `;
        frag.appendChild(q);
      });

      // Submit row
      const actions = document.createElement('div');
      actions.className = 'actions';
      const submitBtn = document.createElement('button');
      submitBtn.textContent = 'Valider mes réponses';
      actions.appendChild(submitBtn);
      frag.appendChild(actions);

      submitBtn.addEventListener('click', () => {
        const {score} = grade(false);
        showSummary(score);
      });

      quizContainer.appendChild(frag);
    }

    function grade(reveal){
      const cards = [...document.querySelectorAll('.qcard')];
      let score = 0;
      cards.forEach((card, idx) => {
        const selected = card.querySelector('input[type=radio]:checked');
        const selectedIndex = selected ? parseInt(selected.value, 10) : -1;
        const labels = card.querySelectorAll('label.opt');
        labels.forEach(l => l.classList.remove('correct','wrong'));
        if (selectedIndex === questions[idx].answer){
          score++;
          if (reveal) labels[selectedIndex]?.classList.add('correct');
        } else if (selectedIndex >= 0 && reveal){
          labels[selectedIndex]?.classList.add('wrong');
          labels[questions[idx].answer]?.classList.add('correct');
        }
        const ex = card.querySelector('.explain');
        if (reveal) ex.classList.remove('hidden');
      });
      scoreEl.textContent = String(score);
      submitted = true;
      return {score};
    }

    function showSummary(score){
      stopTimer();
      const total = questions.length;
      const pct = Math.round((score/total)*100);
      let mood = '🟢 Solide !';
      if (pct < 50) mood = '🟠 En progrès';
      if (pct < 30) mood = '🔴 À réviser';
      resultEl.innerHTML = `
        <div><strong>Résultat:</strong> <span class="score">${score}/${total}</span> (${pct}%) – ${mood}</div>
        <div style="margin-top:8px;color:var(--muted)">Cliquez sur « Voir les réponses » pour afficher les corrections détaillées question par question.</div>
      `;
      resultEl.classList.remove('hidden');
    }

    function startTimer(){
      startTime = Date.now();
      timerInterval = setInterval(()=>{
        const s = Math.floor((Date.now()-startTime)/1000);
        const mm = String(Math.floor(s/60)).padStart(2,'0');
        const ss = String(s%60).padStart(2,'0');
        timerEl.textContent = `${mm}:${ss}`;
      }, 250);
    }
    function stopTimer(){
      clearInterval(timerInterval); timerInterval = null;
    }
  </script>
</body>
</html>

