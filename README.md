# mi-juego <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-9725841734787321"
     crossorigin="anonymous"></script>
<style>
  :root {
    --bg: #14100c;
    --card-bg: #211a13;
    --text: #f5ede1;
    --muted: #ab9d89;
    --accent: #e08a3e;
    --accent-2: #4f9d6e;
    --border: #362b1e;
    --wrong: #c1503f;
  }
  * { box-sizing: border-box; }
  html, body { margin: 0; min-height: 100%; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px 14px calc(20px + env(safe-area-inset-bottom, 0px));
  }
  .ad-slot {
    width: 100%;
    max-width: 480px;
    min-height: 60px;
    margin: 0 0 14px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.75rem;
    color: var(--muted);
    border: 1px dashed var(--border);
    border-radius: 10px;
  }
  .app { width: 100%; max-width: 480px; }
  header { text-align: center; margin-bottom: 14px; }
  header h1 { font-size: 1.4rem; margin: 0 0 4px; }
  header p { margin: 0; color: var(--muted); font-size: 0.85rem; }
  .card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 22px;
  }
  .cat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  .cat-btn {
    padding: 16px 10px;
    border-radius: 12px;
    border: 1px solid var(--border);
    background: #2a2118;
    color: var(--text);
    font-size: 0.95rem;
    font-weight: 600;
    cursor: pointer;
  }
  .cat-btn:hover { border-color: var(--accent); }
  .progress {
    display: flex; justify-content: space-between;
    font-size: 0.8rem; color: var(--muted); margin-bottom: 8px;
  }
  .bar { height: 6px; background: var(--border); border-radius: 4px; overflow: hidden; margin-bottom: 18px; }
  .bar-fill { height: 100%; background: var(--accent); transition: width .3s; }
  .question { font-weight: 600; font-size: 1.08rem; margin-bottom: 18px; line-height: 1.4; }
  .option {
    display: block; width: 100%; text-align: left;
    padding: 13px 14px; margin-bottom: 9px; border-radius: 10px;
    border: 1px solid var(--border); background: #2a2118;
    color: var(--text); font-size: 0.98rem; cursor: pointer;
  }
  .option.correct { background: color-mix(in srgb, var(--accent-2) 25%, #2a2118); border-color: var(--accent-2); }
  .option.wrong { background: color-mix(in srgb, var(--wrong) 25%, #2a2118); border-color: var(--wrong); }
  .option:disabled { cursor: default; }
  #feedback { min-height: 20px; font-weight: 600; margin-bottom: 4px; }
  button.primary {
    margin-top: 14px; width: 100%; padding: 13px; border: none;
    border-radius: 10px; background: var(--accent); color: #1a1206;
    font-size: 1rem; font-weight: 700; cursor: pointer;
  }
  button.primary:disabled { opacity: 0.4; }
  button.secondary {
    margin-top: 10px; width: 100%; padding: 12px; border: 1px solid var(--border);
    border-radius: 10px; background: transparent; color: var(--text); font-size: 0.95rem; cursor: pointer;
  }
  .score-big { text-align: center; font-size: 2.4rem; font-weight: 700; color: var(--accent); margin: 10px 0; }
  .center { text-align: center; }
  input[type="text"] {
    width: 100%; padding: 12px; border-radius: 10px; border: 1px solid var(--border);
    background: #2a2118; color: var(--text); font-size: 1rem; margin-top: 10px;
  }
  table.ranking { width: 100%; border-collapse: collapse; margin-top: 12px; font-size: 0.9rem; }
  table.ranking th, table.ranking td { text-align: left; padding: 8px 6px; border-bottom: 1px solid var(--border); }
  table.ranking th { color: var(--muted); font-weight: 600; }
  .hidden { display: none; }
</style>
</head>
<body>

<!-- Espacio de anuncio (arriba) - reemplaza data-ad-client y data-ad-slot con los tuyos -->
<div class="ad-slot">
  <ins class="adsbygoogle"
       style="display:block; width:100%;"
       data-ad-client="ca-pub-TUNUMERO"
       data-ad-slot="TU-SLOT-ID"
       data-ad-format="auto"></ins>
</div>

<div class="app">
  <header>
    <h1>🧠 Trivia Pro</h1>
    <p>Elige categoría, compite por el primer lugar</p>
  </header>

  <div class="card">

    <!-- Vista: selección de categoría -->
    <div id="categoryView">
      <p class="center" style="margin-top:0;color:var(--muted);">Elige una categoría</p>
      <div class="cat-grid" id="catGrid"></div>
      <button class="secondary" id="showRankingBtn">🏆 Ver ranking</button>
    </div>

    <!-- Vista: quiz -->
    <div id="quizView" class="hidden">
      <div class="progress">
        <span id="progressLabel"></span>
        <span id="scoreLabel"></span>
      </div>
      <div class="bar"><div class="bar-fill" id="barFill"></div></div>
      <div class="question" id="questionText"></div>
      <div id="optionsContainer"></div>
      <div id="feedback"></div>
      <button class="primary" id="nextBtn" disabled>Siguiente</button>
    </div>

    <!-- Vista: guardar puntaje -->
    <div id="saveView" class="hidden center">
      <p>¡Terminaste!</p>
      <div class="score-big" id="finalScore"></div>
      <p id="resultMsg" style="color:var(--muted);"></p>
      <input type="text" id="nameInput" placeholder="Escribe tu nombre" maxlength="18">
      <button class="primary" id="saveScoreBtn">Guardar en el ranking</button>
      <button class="secondary" id="backToCatBtn">Volver a categorías</button>
    </div>

    <!-- Vista: ranking -->
    <div id="rankingView" class="hidden">
      <p class="center" style="margin-top:0;color:var(--muted);">🏆 Mejores puntajes (en este dispositivo)</p>
      <table class="ranking" id="rankingTable">
        <thead><tr><th>#</th><th>Nombre</th><th>Categoría</th><th>Puntos</th></tr></thead>
        <tbody id="rankingBody"></tbody>
      </table>
      <button class="secondary" id="backFromRankingBtn">Volver</button>
    </div>

  </div>
</div>
<!-- Espacio de anuncio (abajo) -->
<div class="ad-slot" style="margin-top:14px;">
  <ins class="adsbygoogle"
       style="display:block; width:100%;"
       data-ad-client="ca-pub-TUNUMERO"
       data-ad-slot="TU-SLOT-ID"
       data-ad-format="auto"></ins>
</div>

<script>
(function(){
  try { (window.adsbygoogle = window.adsbygoogle || []).push({}); } catch(e) {}
  try { (window.adsbygoogle = window.adsbygoogle || []).push({}); } catch(e) {}
})();

const BANK = {
  "Ciencia": [
    { q: "¿Cuál es el planeta más grande del sistema solar?", options: ["Tierra","Júpiter","Saturno","Marte"], correct: 1 },
    { q: "¿Cuál es el metal líquido a temperatura ambiente?", options: ["Hierro","Mercurio","Plomo","Aluminio"], correct: 1 },
    { q: "¿Qué gas respiramos principalmente del aire?", options: ["Oxígeno","Nitrógeno","Dióxido de carbono","Hidrógeno"], correct: 1 },
    { q: "¿Cuántos huesos tiene el cuerpo humano adulto?", options: ["186","206","226","246"], correct: 1 },
    { q: "¿Qué científico propuso la teoría de la relatividad?", options: ["Newton","Einstein","Galileo","Darwin"], correct: 1 }
  ],
  "Historia": [
    { q: "¿En qué año llegó el humano a la Luna?", options: ["1965","1969","1971","1975"], correct: 1 },
    { q: "¿Quién fue el primer presidente de EE.UU.?", options: ["Lincoln","Washington","Jefferson","Adams"], correct: 1 },
    { q: "¿En qué año cayó el Muro de Berlín?", options: ["1985","1987","1989","1991"], correct: 2 },
    { q: "¿Qué civilización construyó Machu Picchu?", options: ["Maya","Azteca","Inca","Olmeca"], correct: 2 },
    { q: "¿Qué imperio construyó el Coliseo?", options: ["Griego","Romano","Egipcio","Persa"], correct: 1 }
  ],
  "Deportes": [
    { q: "¿Cada cuántos años son los Juegos Olímpicos de verano?", options: ["2","3","4","5"], correct: 2 },
    { q: "¿Cuántos jugadores tiene un equipo de fútbol en cancha?", options: ["9","10","11","12"], correct: 2 },
    { q: "¿En qué deporte se usa un 'birdie'?", options: ["Tenis","Golf","Bádminton","Squash"], correct: 2 },
    { q: "¿Cuántos sets se necesitan ganar en tenis masculino Grand Slam?", options: ["2","3","4","5"], correct: 1 },
    { q: "¿Qué país ha ganado más Mundiales de fútbol?", options: ["Alemania","Argentina","Brasil","Italia"], correct: 2 }
  ],
  "Entretenimiento": [
    { q: "¿Quién pintó la Mona Lisa?", options: ["Miguel Ángel","Rafael","Leonardo da Vinci","Donatello"], correct: 2 },
    { q: "¿Qué banda cantó 'Bohemian Rhapsody'?", options: ["The Beatles","Queen","Pink Floyd","Led Zeppelin"], correct: 1 },
    { q: "¿Cuál es el estudio de animación detrás de 'Toy Story'?", options: ["DreamWorks","Illumination","Pixar","Blue Sky"], correct: 2 },
    { q: "¿Quién dirigió la trilogía original de 'Star Wars'?", options: ["Spielberg","Lucas","Cameron","Scorsese"], correct: 1 },
    { q: "¿En qué ciudad se ambienta la serie 'Friends'?", options: ["Los Ángeles","Chicago","Nueva York","Boston"], correct: 2 }
  ],
  "Geografía": [
    { q: "¿Cuál es el océano más grande?", options: ["Atlántico","Índico","Ártico","Pacífico"], correct: 3 },
    { q: "¿Cuál es el río más largo del mundo?", options: ["Nilo","Amazonas","Yangtsé","Misisipi"], correct: 1 },
    { q: "¿Qué país tiene más habitantes actualmente?", options: ["China","EE.UU.","India","Indonesia"], correct: 2 },
    { q: "¿Cuál es el desierto más grande del mundo?", options: ["Sahara","Gobi","Antártida","Kalahari"], correct: 2 },
    { q: "¿En qué continente está Egipto?", options: ["Asia","África","Europa","Oceanía"], correct: 1 }
  ]
};

let currentCategory = "";
let currentQuestions = [];
let idx = 0, score = 0, answered = false;

const catGrid = document.getElementById('catGrid');
const categoryView = document.getElementById('categoryView');
const quizView = document.getElementById('quizView');
const saveView = document.getElementById('saveView');
const rankingView = document.getElementById('rankingView');

Object.keys(BANK).forEach(cat => {
  const b = document.createElement('button');
  b.className = 'cat-btn';
  b.textContent = cat;
  b.onclick = () => startQuiz(cat);
  catGrid.appendChild(b);
});

function startQuiz(cat) {
  currentCategory = cat;
  currentQuestions = BANK[cat];
  idx = 0; score = 0;
  categoryView.classList.add('hidden');
  saveView.classList.add('hidden');
  rankingView.classList.add('hidden');
  quizView.classList.remove('hidden');
  loadQuestion();
}

const questionText = document.getElementById('questionText');
const optionsContainer = document.getElementById('optionsContainer');
const feedback = document.getElementById('feedback');
const nextBtn = document.getElementById('nextBtn');
const progressLabel = document.getElementById('progressLabel');
const scoreLabel = document.getElementById('scoreLabel');
const barFill = document.getElementById('barFill');

function loadQuestion() {
  answered = false;
  nextBtn.disabled = true;
  feedback.textContent = '';
  const item = currentQuestions[idx];
  questionText.textContent = item.q;
  progressLabel.textContent = `${currentCategory} · Pregunta ${idx+1} de ${currentQuestions.length}`;
  scoreLabel.textContent = `Puntos: ${score}`;
  barFill.style.width = `${(idx / currentQuestions.length) * 100}%`;
  optionsContainer.innerHTML = '';
  item.options.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.className = 'option';
    btn.textContent = opt;
    btn.onclick = () => pick(i);
    optionsContainer.appendChild(btn);
  });
}

function pick(i) {
  if (answered) return;
  answered = true;
  const item = currentQuestions[idx];
  [...optionsContainer.children].forEach((b, j) => {
    b.disabled = true;
    if (j === item.correct) b.classList.add('correct');
    else if (j === i) b.classList.add('wrong');
  });
  if (i === item.correct) { score++; feedback.textContent = '¡Correcto! 🎉'; }
  else { feedback.textContent = 'Incorrecto'; }
  scoreLabel.textContent = `Puntos: ${score}`;
  nextBtn.disabled = false;
}

nextBtn.onclick = () => {
  idx++;
  if (idx < currentQuestions.length) loadQuestion();
  else finishQuiz();
};

function finishQuiz() {
  quizView.classList.add('hidden');
  saveView.classList.remove('hidden');
  document.getElementById('finalScore').textContent = `${score}/${currentQuestions.length}`;
  const pct = score / currentQuestions.length;
  let msg = pct === 1 ? '¡Perfecto! 🏆' : pct >= 0.6 ? '¡Buen trabajo!' : 'Sigue practicando.';
  document.getElementById('resultMsg').textContent = msg;
}

function getRanking() {
  try { return JSON.parse(localStorage.getItem('trivia_ranking') || '[]'); }
  catch(e) { return []; }
}
function saveRanking(list) {
  try { localStorage.setItem('trivia_ranking', JSON.stringify(list)); } catch(e) {}
}

document.getElementById('saveScoreBtn').onclick = () => {
  const name = document.getElementById('nameInput').value.trim() || 'Anónimo';
  const list = getRanking();
  list.push({ name, category: currentCategory, score, total: currentQuestions.length, date: Date.now() });
  list.sort((a,b) => (b.score/b.total) - (a.score/a.total));
  saveRanking(list.slice(0, 20));
  showRanking();
};

document.getElementById('backToCatBtn').onclick = backToCategories;
document.getElementById('showRankingBtn').onclick = showRanking;
document.getElementById('backFromRankingBtn').onclick = backToCategories;

function backToCategories() {
  saveView.classList.add('hidden');
  rankingView.classList.add('hidden');
  quizView.classList.add('hidden');
  categoryView.classList.remove('hidden');
}

function showRanking() {
  categoryView.classList.add('hidden');
  saveView.classList.add('hidden');
  rankingView.classList.remove('hidden');
  const list = getRanking();
  const body = document.getElementById('rankingBody');
  body.innerHTML = '';
  if (list.length === 0) {
    body.innerHTML = '<tr><td colspan="4" style="color:var(--muted);">Aún no hay puntajes guardados</td></tr>';
    return;
  }
  list.forEach((r, i) => {
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${i+1}</td><td>${r.name}</td><td>${r.category}</td><td>${r.score}/${r.total}</td>`;
    body.appendChild(tr);
  });
}
</script>
</body>
</html>
