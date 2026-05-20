<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Polarización de la Luz — Simulador Completo</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
<style>
:root {
  --ink: #0f0e0c;
  --paper: #faf7f2;
  --cream: #f0ebe0;
  --gold: #c8922a;
  --gold-light: #e8b84a;
  --blue: #1a4a7a;
  --red: #8a2020;
  --green: #1a5a2a;
  --muted: #7a7268;
  --border: #d8d0c0;
  --panel: #f5f0e6;
}
* { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior: smooth; }
body {
  background: var(--paper);
  color: var(--ink);
  font-family: 'DM Mono', monospace;
  font-size: 13px;
  line-height: 1.6;
}

/* ── NAV ── */
nav {
  position: sticky; top: 0; z-index: 100;
  background: var(--ink);
  display: flex; align-items: center;
  padding: 0 28px;
  height: 52px;
  gap: 0;
}
.nav-title {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: var(--paper);
  font-size: 1.05rem;
  margin-right: auto;
  letter-spacing: 0.01em;
}
.nav-title span { color: var(--gold-light); font-style: normal; }
.nav-links { display: flex; gap: 2px; }
.nav-link {
  color: #8a8278;
  padding: 8px 14px;
  cursor: pointer;
  font-size: 0.62rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  border-radius: 3px;
  transition: all 0.15s;
  border: none; background: none;
}
.nav-link:hover, .nav-link.active { color: var(--gold-light); background: rgba(200,146,42,0.1); }

/* ── SECTIONS ── */
section {
  min-height: 100vh;
  padding: 52px 0 0;
  display: none;
}
section.visible { display: block; }

.sec-header {
  padding: 40px 52px 28px;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: flex-end; justify-content: space-between;
  flex-wrap: wrap; gap: 12px;
}
.sec-number {
  font-size: 0.6rem; letter-spacing: 0.2em;
  color: var(--gold); text-transform: uppercase;
  margin-bottom: 6px;
}
.sec-title {
  font-family: 'Playfair Display', serif;
  font-size: 2rem; font-weight: 700;
  line-height: 1.1;
}
.sec-desc {
  max-width: 480px;
  color: var(--muted);
  font-size: 0.75rem;
  line-height: 1.7;
}

/* ── LAYOUT ── */
.two-col {
  display: grid;
  grid-template-columns: 1fr 360px;
  min-height: calc(100vh - 180px);
}
.main-panel { padding: 32px 40px; border-right: 1px solid var(--border); }
.side-panel { padding: 28px 24px; background: var(--panel); display: flex; flex-direction: column; gap: 20px; overflow-y: auto; }

/* ── CARDS ── */
.card {
  background: var(--paper);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 16px;
}
.card-title {
  font-size: 0.6rem; text-transform: uppercase;
  letter-spacing: 0.14em; color: var(--gold);
  margin-bottom: 10px; padding-bottom: 8px;
  border-bottom: 1px solid var(--border);
}

/* ── SLIDERS ── */
.ctrl { display: flex; flex-direction: column; gap: 5px; margin-bottom: 12px; }
.ctrl-head { display: flex; justify-content: space-between; font-size: 0.72rem; }
.ctrl-head .lbl { color: var(--ink); }
.ctrl-head .val { font-weight: 500; }
.val-blue { color: var(--blue); }
.val-red  { color: var(--red); }
.val-green{ color: var(--green); }
input[type=range] {
  -webkit-appearance: none; width: 100%; height: 3px;
  background: var(--border); border-radius: 2px; cursor: pointer; outline: none;
}
input[type=range].r-blue::-webkit-slider-thumb { background: var(--blue); }
input[type=range].r-red::-webkit-slider-thumb  { background: var(--red); }
input[type=range].r-green::-webkit-slider-thumb{ background: var(--green); }
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none; width: 14px; height: 14px;
  border-radius: 50%; background: var(--gold);
  box-shadow: 0 1px 4px rgba(0,0,0,0.25); cursor: grab;
}

/* ── METRICS ── */
.metrics { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.metric {
  background: var(--cream); border: 1px solid var(--border);
  border-radius: 4px; padding: 9px 12px;
}
.metric .m-lbl { font-size: 0.58rem; text-transform: uppercase; letter-spacing: 0.1em; color: var(--muted); margin-bottom: 3px; }
.metric .m-val { font-size: 1.05rem; font-weight: 500; font-family: 'Playfair Display', serif; }

/* ── INFO CALLOUT ── */
.callout {
  border-left: 3px solid var(--gold);
  background: rgba(200,146,42,0.06);
  padding: 12px 16px;
  border-radius: 0 4px 4px 0;
  font-size: 0.72rem;
  line-height: 1.7;
  color: #3a3020;
}
.callout.blue  { border-color: var(--blue);  background: rgba(26,74,122,0.06); color: #0a1a30; }
.callout.red   { border-color: var(--red);   background: rgba(138,32,32,0.06); color: #2a0808; }
.callout.green { border-color: var(--green); background: rgba(26,90,42,0.06);  color: #081808; }
.callout b { font-weight: 500; }

/* ── FORMULA ── */
.formula-display {
  background: var(--ink); color: var(--paper);
  border-radius: 6px; padding: 16px 20px;
  text-align: center; font-size: 1.1rem;
  letter-spacing: 0.06em;
  font-family: 'Playfair Display', serif;
  font-style: italic;
}
.formula-display .sub { font-size: 0.65rem; color: #8a8278; margin-top: 6px; font-family: 'DM Mono'; font-style: normal; letter-spacing: 0.1em; }

/* ── INTENSITY BAR ── */
.ibar-wrap { display: flex; align-items: center; gap: 10px; }
.ibar { flex: 1; height: 10px; background: var(--border); border-radius: 5px; overflow: hidden; }
.ibar-fill { height: 100%; border-radius: 5px; transition: width 0.12s, background 0.12s; }
.ibar-val { width: 42px; text-align: right; font-size: 0.7rem; font-weight: 500; }

/* ── TABS ── */
.tabs { display: flex; gap: 2px; margin-bottom: 20px; }
.tab {
  padding: 7px 16px; font-size: 0.65rem; letter-spacing: 0.1em;
  text-transform: uppercase; cursor: pointer; border-radius: 3px;
  border: 1px solid var(--border); background: transparent; color: var(--muted);
  transition: all 0.12s;
}
.tab:hover { color: var(--ink); border-color: var(--ink); }
.tab.active { background: var(--ink); color: var(--paper); border-color: var(--ink); }

/* ── CONCEPT SECTION SPECIFIC ── */
.concept-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 20px; padding: 32px 40px;
}
.concept-card {
  border: 1px solid var(--border); border-radius: 8px;
  padding: 24px; background: var(--paper);
}
.concept-card h3 {
  font-family: 'Playfair Display', serif;
  font-size: 1.1rem; margin-bottom: 10px;
  padding-bottom: 8px; border-bottom: 1px solid var(--border);
}
.concept-card p { font-size: 0.72rem; color: #3a3028; line-height: 1.8; margin-bottom: 10px; }
.concept-canvas { width: 100%; border-radius: 4px; background: var(--cream); border: 1px solid var(--border); display: block; }

/* ── GRAPH ── */
#graphCanvas { width: 100%; border-radius: 4px; background: var(--cream); border: 1px solid var(--border); display: block; }

/* ── PARADOX SECTION ── */
.paradox-layout { display: grid; grid-template-columns: 1fr 320px; min-height: calc(100vh - 180px); }
.paradox-main { padding: 32px 40px; border-right: 1px solid var(--border); }
.paradox-side { padding: 28px 24px; background: var(--panel); }

/* steps */
.step-list { display: flex; flex-direction: column; gap: 12px; margin-top: 16px; }
.step {
  display: flex; gap: 14px; align-items: flex-start;
  padding: 12px; border: 1px solid var(--border); border-radius: 5px;
  cursor: pointer; transition: all 0.15s; background: var(--paper);
}
.step:hover { border-color: var(--gold); background: rgba(200,146,42,0.04); }
.step.active { border-color: var(--gold); background: rgba(200,146,42,0.08); }
.step-num {
  width: 28px; height: 28px; border-radius: 50%;
  background: var(--border); display: flex; align-items: center; justify-content: center;
  font-size: 0.7rem; font-weight: 500; flex-shrink: 0; color: var(--muted);
}
.step.active .step-num { background: var(--gold); color: white; }
.step-content h4 { font-size: 0.75rem; font-weight: 500; margin-bottom: 3px; }
.step-content p { font-size: 0.68rem; color: var(--muted); line-height: 1.5; }

/* ── QUIZ ── */
.quiz-section { padding: 32px 40px; }
.quiz-q { margin-bottom: 28px; }
.quiz-q h3 { font-size: 0.85rem; font-weight: 500; margin-bottom: 12px; }
.quiz-opts { display: flex; flex-direction: column; gap: 6px; }
.quiz-opt {
  padding: 9px 14px; border: 1px solid var(--border); border-radius: 4px;
  cursor: pointer; font-size: 0.72rem; transition: all 0.12s;
  background: var(--paper);
}
.quiz-opt:hover { border-color: var(--gold); }
.quiz-opt.correct { background: rgba(26,90,42,0.1); border-color: var(--green); color: var(--green); }
.quiz-opt.wrong   { background: rgba(138,32,32,0.1); border-color: var(--red);   color: var(--red); }
.quiz-feedback { font-size: 0.68rem; margin-top: 8px; padding: 8px 12px; border-radius: 4px; display: none; }
.quiz-feedback.show { display: block; }
.quiz-feedback.ok  { background: rgba(26,90,42,0.08); color: var(--green); }
.quiz-feedback.bad { background: rgba(138,32,32,0.08); color: var(--red); }

/* ── SCROLLBAR ── */
::-webkit-scrollbar { width: 5px; }
::-webkit-scrollbar-track { background: var(--panel); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
</style>
</head>
<body>

<!-- ════ NAV ════ -->
<nav>
  <span class="nav-title">Polarización de la <span>Luz</span></span>
  <div class="nav-links">
    <button class="nav-link active" onclick="showSection('exp',this)">① Experimento</button>
    <button class="nav-link" onclick="showSection('concept',this)">② Conceptos</button>
    <button class="nav-link" onclick="showSection('malus',this)">③ Ley de Malus</button>
    <button class="nav-link" onclick="showSection('paradox',this)">④ Paradoja</button>
    <button class="nav-link" onclick="showSection('quiz',this)">⑤ Evaluación</button>
  </div>
</nav>

<!-- ════════════════════════════════════════════
     SECCIÓN 1 — EXPERIMENTO INTERACTIVO
════════════════════════════════════════════ -->
<section id="sec-exp" class="visible">
  <div class="sec-header">
    <div>
      <div class="sec-number">01 — Experimento</div>
      <div class="sec-title">Laboratorio Virtual</div>
    </div>
    <div class="sec-desc">Arrastra los polarizadores para rotarlos. Observa cómo cambia la intensidad de la luz en tiempo real según la Ley de Malus.</div>
  </div>

  <div class="two-col">
    <div class="main-panel">
      <!-- MODE TABS -->
      <div class="tabs">
        <button class="tab active" onclick="setExpMode(1,this)">1 Polarizador</button>
        <button class="tab" onclick="setExpMode(2,this)">2 Polarizadores</button>
        <button class="tab" onclick="setExpMode(3,this)">3 Polarizadores</button>
      </div>
      <canvas id="expCanvas" style="width:100%;border-radius:6px;background:var(--cream);border:1px solid var(--border);display:block;cursor:default;"></canvas>
      <div id="expCallout" class="callout" style="margin-top:16px;"></div>
    </div>

    <div class="side-panel">
      <div class="card">
        <div class="card-title">Controles</div>
        <div class="ctrl">
          <div class="ctrl-head"><span class="lbl">Polarizador 1 (θ₁)</span><span class="val val-blue" id="lbl1">0°</span></div>
          <input type="range" class="r-blue" id="sl1" min="0" max="180" value="0" oninput="sliderUpdate()">
        </div>
        <div class="ctrl" id="ctrl2wrap">
          <div class="ctrl-head"><span class="lbl">Polarizador 2 (θ₂)</span><span class="val val-red" id="lbl2">90°</span></div>
          <input type="range" class="r-red" id="sl2" min="0" max="180" value="90" oninput="sliderUpdate()">
        </div>
        <div class="ctrl" id="ctrl3wrap" style="display:none">
          <div class="ctrl-head"><span class="lbl">Polarizador 3 (θ₃)</span><span class="val val-green" id="lbl3">45°</span></div>
          <input type="range" class="r-green" id="sl3" min="0" max="180" value="45" oninput="sliderUpdate()">
        </div>
      </div>

      <div class="card">
        <div class="card-title">Intensidades</div>
        <div class="metrics" id="metricsGrid"></div>
      </div>

      <div class="card">
        <div class="card-title">Intensidad Final</div>
        <div class="ibar-wrap">
          <div class="ibar"><div class="ibar-fill" id="ibarFill" style="width:50%;background:var(--gold);"></div></div>
          <span class="ibar-val" id="ibarVal">50%</span>
        </div>
      </div>

      <div class="formula-display" id="formulaBox">
        <div id="formulaText">I₁ = I₀ / 2</div>
        <div class="sub" id="formulaSub">Primer polarizador: luz no polarizada → polarizada</div>
      </div>
    </div>
  </div>
</section>

<!-- ════════════════════════════════════════════
     SECCIÓN 2 — CONCEPTOS
════════════════════════════════════════════ -->
<section id="sec-concept">
  <div class="sec-header">
    <div>
      <div class="sec-number">02 — Conceptos</div>
      <div class="sec-title">¿Qué es la Polarización?</div>
    </div>
    <div class="sec-desc">La luz es una onda electromagnética transversal. La polarización describe la dirección de oscilación del campo eléctrico.</div>
  </div>

  <div class="concept-grid">
    <div class="concept-card">
      <h3>Luz No Polarizada</h3>
      <p>La luz natural (sol, lámparas) oscila en <b>todas las direcciones</b> perpendiculares a su propagación. No hay una dirección preferida del campo eléctrico <b>E</b>.</p>
      <canvas class="concept-canvas" id="unpolarCanvas" height="140"></canvas>
      <p style="margin-top:10px">Al pasar por un polarizador lineal, <b>sólo sobrevive la componente paralela</b> al eje del filtro. La intensidad se reduce a la mitad: <b>I = I₀/2</b></p>
    </div>
    <div class="concept-card">
      <h3>Luz Polarizada Linealmente</h3>
      <p>Tras el primer polarizador, el campo <b>E</b> oscila en una única dirección. Esta es la <b>dirección de polarización</b>.</p>
      <canvas class="concept-canvas" id="polarCanvas" height="140"></canvas>
      <p style="margin-top:10px">Un segundo polarizador sólo transmite la <b>proyección</b> de este campo sobre su propio eje. De ahí la Ley de Malus.</p>
    </div>
    <div class="concept-card">
      <h3>Eje del Polarizador</h3>
      <p>Un polarizador tiene un <b>eje de transmisión</b> (o eje óptico). Sólo pasa la componente del campo <b>E</b> paralela a dicho eje.</p>
      <canvas class="concept-canvas" id="axisCanvas" height="140"></canvas>
      <p style="margin-top:10px">La componente transmitida es <b>E·cos(θ)</b>, y como la intensidad es proporcional a E², resulta <b>I = I₀·cos²(θ)</b>.</p>
    </div>
    <div class="concept-card">
      <h3>Tipos de Polarización</h3>
      <p><b>Lineal:</b> E oscila en una dirección fija. <b>Circular:</b> E rota con amplitud constante. <b>Elíptica:</b> E rota con amplitud variable.</p>
      <canvas class="concept-canvas" id="typesCanvas" height="140"></canvas>
      <p style="margin-top:10px">En este laboratorio trabajamos con <b>polarización lineal</b>, la más fácil de producir y demostrar con filtros polarizadores.</p>
    </div>
  </div>
</section>

<!-- ════════════════════════════════════════════
     SECCIÓN 3 — LEY DE MALUS
════════════════════════════════════════════ -->
<section id="sec-malus">
  <div class="sec-header">
    <div>
      <div class="sec-number">03 — Ley de Malus</div>
      <div class="sec-title">I = I₀ · cos²(θ)</div>
    </div>
    <div class="sec-desc">Etienne-Louis Malus (1809) descubrió que la intensidad transmitida depende del cuadrado del coseno del ángulo entre los ejes de polarización.</div>
  </div>

  <div class="two-col">
    <div class="main-panel">
      <canvas id="malusCanvas" style="width:100%;border-radius:6px;background:var(--cream);border:1px solid var(--border);display:block;" height="320"></canvas>
      <div style="margin-top:20px;">
        <canvas id="graphCanvas" height="200"></canvas>
      </div>
    </div>

    <div class="side-panel">
      <div class="card">
        <div class="card-title">Parámetros</div>
        <div class="ctrl">
          <div class="ctrl-head"><span class="lbl">Ángulo θ entre polarizadores</span><span class="val val-blue" id="malusTheta">0°</span></div>
          <input type="range" class="r-blue" id="malusSl" min="0" max="180" value="0" oninput="updateMalus()">
        </div>
        <div class="ctrl">
          <div class="ctrl-head"><span class="lbl">Intensidad inicial I₀</span><span class="val" id="malusI0">100%</span></div>
          <input type="range" id="malusI0Sl" min="10" max="100" value="100" oninput="updateMalus()">
        </div>
      </div>

      <div class="card">
        <div class="card-title">Cálculo en Vivo</div>
        <div id="malusCalc" style="font-size:0.72rem;line-height:2;"></div>
      </div>

      <div class="card">
        <div class="card-title">Casos Notables</div>
        <div style="display:flex;flex-direction:column;gap:6px;">
          <div class="callout blue" onclick="setMalusAngle(0)" style="cursor:pointer"><b>θ = 0°</b> → cos²(0°) = 1 → I = I₀ (máximo)</div>
          <div class="callout" onclick="setMalusAngle(45)" style="cursor:pointer"><b>θ = 45°</b> → cos²(45°) = 0.5 → I = I₀/2</div>
          <div class="callout red" onclick="setMalusAngle(90)" style="cursor:pointer"><b>θ = 90°</b> → cos²(90°) = 0 → I = 0 (extinción)</div>
        </div>
        <p style="font-size:0.63rem;color:var(--muted);margin-top:10px;">Haz clic en un caso para verlo</p>
      </div>

      <div class="callout">
        <b>Demostración vectorial:</b> La amplitud transmitida es <b>E₀·cos(θ)</b>. La intensidad es proporcional a la amplitud al cuadrado: <b>I ∝ E² = E₀²·cos²(θ) = I₀·cos²(θ)</b>
      </div>
    </div>
  </div>
</section>

<!-- ════════════════════════════════════════════
     SECCIÓN 4 — PARADOJA DEL 3er POLARIZADOR
════════════════════════════════════════════ -->
<section id="sec-paradox">
  <div class="sec-header">
    <div>
      <div class="sec-number">04 — Paradoja</div>
      <div class="sec-title">El Tercer Polarizador</div>
    </div>
    <div class="sec-desc">Dos polarizadores cruzados a 90° extinguen la luz. Pero insertar un tercero entre ellos puede hacer que la luz reaparezca. ¿Cómo es posible?</div>
  </div>

  <div class="paradox-layout">
    <div class="paradox-main">
      <canvas id="paradoxCanvas" style="width:100%;border-radius:6px;background:var(--cream);border:1px solid var(--border);display:block;" height="260"></canvas>

      <div style="margin-top:20px;">
        <div class="ctrl">
          <div class="ctrl-head"><span class="lbl">Ángulo del polarizador intermedio (θ₂)</span><span class="val val-red" id="paradoxLbl">45°</span></div>
          <input type="range" class="r-red" id="paradoxSl" min="0" max="180" value="45" oninput="updateParadox()">
        </div>
      </div>

      <div id="paradoxCallout" class="callout" style="margin-top:16px;"></div>

      <canvas id="paradoxGraph" style="margin-top:20px;width:100%;border-radius:4px;background:var(--cream);border:1px solid var(--border);display:block;" height="160"></canvas>
    </div>

    <div class="paradox-side">
      <div class="card-title" style="margin-bottom:12px;">Guía Paso a Paso</div>
      <div class="step-list">
        <div class="step active" id="pstep0" onclick="setParadoxStep(0,this)">
          <div class="step-num">1</div>
          <div class="step-content">
            <h4>Dos polarizadores cruzados</h4>
            <p>P₁ = 0°, P₃ = 90°. La luz se extingue completamente. I = 0.</p>
          </div>
        </div>
        <div class="step" id="pstep1" onclick="setParadoxStep(1,this)">
          <div class="step-num">2</div>
          <div class="step-content">
            <h4>Insertar P₂ a 45°</h4>
            <p>Añadir P₂ = 45° entre P₁ y P₃. ¡La luz reaparece con I = 12.5%!</p>
          </div>
        </div>
        <div class="step" id="pstep2" onclick="setParadoxStep(2,this)">
          <div class="step-num">3</div>
          <div class="step-content">
            <h4>¿Por qué funciona?</h4>
            <p>P₂ re-polariza la luz en una nueva dirección. P₃ ya no extingue esa nueva polarización.</p>
          </div>
        </div>
        <div class="step" id="pstep3" onclick="setParadoxStep(3,this)">
          <div class="step-num">4</div>
          <div class="step-content">
            <h4>Ángulo óptimo</h4>
            <p>El máximo de transmisión ocurre cuando P₂ = 45°. Explora otros ángulos con el slider.</p>
          </div>
        </div>
      </div>

      <div class="card" style="margin-top:16px;">
        <div class="card-title">Fórmula Combinada</div>
        <div class="formula-display" style="font-size:0.8rem;padding:12px;">
          I = (I₀/2)·cos²(θ₂)·cos²(90°−θ₂)
          <div class="sub">θ₂ = ángulo de P₂ respecto a P₁</div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ════════════════════════════════════════════
     SECCIÓN 5 — EVALUACIÓN
════════════════════════════════════════════ -->
<section id="sec-quiz">
  <div class="sec-header">
    <div>
      <div class="sec-number">05 — Evaluación</div>
      <div class="sec-title">Pon a Prueba tu Comprensión</div>
    </div>
    <div class="sec-desc">Responde las siguientes preguntas para verificar que entendiste los conceptos de polarización.</div>
  </div>

  <div class="quiz-section">
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:28px;">

      <div class="quiz-q" id="q1">
        <h3>1. Si la luz no polarizada pasa por un solo polarizador lineal, ¿qué fracción de la intensidad original se transmite?</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q1',this,'wrong')">a) La intensidad no cambia (I = I₀)</div>
          <div class="quiz-opt" onclick="answer('q1',this,'correct')">b) La mitad (I = I₀/2)</div>
          <div class="quiz-opt" onclick="answer('q1',this,'wrong')">c) Un cuarto (I = I₀/4)</div>
          <div class="quiz-opt" onclick="answer('q1',this,'wrong')">d) Cero (I = 0)</div>
        </div>
        <div class="quiz-feedback" id="fb1"></div>
      </div>

      <div class="quiz-q" id="q2">
        <h3>2. Dos polarizadores están cruzados a 90°. ¿Qué ocurre con la intensidad de la luz transmitida?</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q2',this,'wrong')">a) Se transmite la mitad de la intensidad</div>
          <div class="quiz-opt" onclick="answer('q2',this,'wrong')">b) Se transmite el 25% de la intensidad</div>
          <div class="quiz-opt" onclick="answer('q2',this,'correct')">c) La luz se extingue completamente (I = 0)</div>
          <div class="quiz-opt" onclick="answer('q2',this,'wrong')">d) No hay cambio en la intensidad</div>
        </div>
        <div class="quiz-feedback" id="fb2"></div>
      </div>

      <div class="quiz-q" id="q3">
        <h3>3. Según la Ley de Malus, si θ = 60° entre dos polarizadores, ¿cuánto vale I/I₀?</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q3',this,'wrong')">a) 0.75</div>
          <div class="quiz-opt" onclick="answer('q3',this,'correct')">b) 0.25</div>
          <div class="quiz-opt" onclick="answer('q3',this,'wrong')">c) 0.50</div>
          <div class="quiz-opt" onclick="answer('q3',this,'wrong')">d) 0.866</div>
        </div>
        <div class="quiz-feedback" id="fb3"></div>
      </div>

      <div class="quiz-q" id="q4">
        <h3>4. En la paradoja del tercer polarizador, ¿por qué reaparece la luz al insertar P₂ entre P₁ (0°) y P₃ (90°)?</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q4',this,'wrong')">a) P₂ amplifica la intensidad de la luz</div>
          <div class="quiz-opt" onclick="answer('q4',this,'wrong')">b) P₂ cancela el efecto de P₁</div>
          <div class="quiz-opt" onclick="answer('q4',this,'correct')">c) P₂ re-polariza la luz en una dirección intermedia que P₃ puede transmitir parcialmente</div>
          <div class="quiz-opt" onclick="answer('q4',this,'wrong')">d) P₂ elimina la polarización produciendo luz no polarizada</div>
        </div>
        <div class="quiz-feedback" id="fb4"></div>
      </div>

      <div class="quiz-q" id="q5">
        <h3>5. ¿En qué ángulo del polarizador intermedio se obtiene la máxima transmisión en la configuración de 3 polarizadores (P₁=0°, P₃=90°)?</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q5',this,'wrong')">a) 30°</div>
          <div class="quiz-opt" onclick="answer('q5',this,'correct')">b) 45°</div>
          <div class="quiz-opt" onclick="answer('q5',this,'wrong')">c) 60°</div>
          <div class="quiz-opt" onclick="answer('q5',this,'wrong')">d) 90°</div>
        </div>
        <div class="quiz-feedback" id="fb5"></div>
      </div>

      <div class="quiz-q" id="q6">
        <h3>6. La luz polarizada circularmente se diferencia de la lineal en que…</h3>
        <div class="quiz-opts">
          <div class="quiz-opt" onclick="answer('q6',this,'wrong')">a) No tiene campo magnético asociado</div>
          <div class="quiz-opt" onclick="answer('q6',this,'correct')">b) El vector E rota continuamente con amplitud constante al propagarse</div>
          <div class="quiz-opt" onclick="answer('q6',this,'wrong')">c) Solo existe en medios sólidos</div>
          <div class="quiz-opt" onclick="answer('q6',this,'wrong')">d) No puede producirse con polarizadores lineales</div>
        </div>
        <div class="quiz-feedback" id="fb6"></div>
      </div>

    </div>

    <div id="quizResult" style="display:none;margin-top:32px;" class="callout">
      <b id="quizScore"></b><br><span id="quizMsg"></span>
    </div>
  </div>
</section>

<script>
// ═══════════════════════════════════════════════
// NAVIGATION
// ═══════════════════════════════════════════════
function showSection(id, btn) {
  document.querySelectorAll('section').forEach(s => s.classList.remove('visible'));
  document.getElementById('sec-' + id).classList.add('visible');
  document.querySelectorAll('.nav-link').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  if (id === 'concept') drawConceptCanvases();
  if (id === 'malus') { updateMalus(); }
  if (id === 'paradox') { updateParadox(); }
}

// ═══════════════════════════════════════════════
// SECTION 1 — EXPERIMENT
// ═══════════════════════════════════════════════
const expCanvas = document.getElementById('expCanvas');
const expCtx = expCanvas.getContext('2d');
let expMode = 1;
let expAngles = [0, 90, 45];
let expDragging = -1;
let expT = 0;

function setExpMode(n, btn) {
  expMode = n;
  document.querySelectorAll('.tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('ctrl2wrap').style.display = n >= 2 ? '' : 'none';
  document.getElementById('ctrl3wrap').style.display = n >= 3 ? '' : 'none';
  updateExp();
}

function sliderUpdate() {
  expAngles[0] = +document.getElementById('sl1').value;
  expAngles[1] = +document.getElementById('sl2').value;
  expAngles[2] = +document.getElementById('sl3').value;
  document.getElementById('lbl1').textContent = expAngles[0] + '°';
  document.getElementById('lbl2').textContent = expAngles[1] + '°';
  document.getElementById('lbl3').textContent = expAngles[2] + '°';
  updateExp();
}

function expIntensities() {
  const I0 = 1;
  const I1 = I0 * 0.5;
  if (expMode === 1) return { I0, I1, I2: null, I3: null, final: I1 };
  const d12 = (expAngles[1] - expAngles[0]) * Math.PI / 180;
  const I2 = I1 * Math.cos(d12) ** 2;
  if (expMode === 2) return { I0, I1, I2, I3: null, final: I2 };
  const d23 = (expAngles[2] - expAngles[1]) * Math.PI / 180;
  const I3 = I2 * Math.cos(d23) ** 2;
  return { I0, I1, I2, I3, final: I3 };
}

function updateExp() {
  const I = expIntensities();
  // Metrics
  const grid = document.getElementById('metricsGrid');
  const items = [
    { lbl: 'I₀ (entrada)', val: (I.I0 * 100).toFixed(0) + '%', color: '#3a3028' },
    { lbl: 'I₁ (tras P₁)', val: (I.I1 * 100).toFixed(1) + '%', color: '#1a4a7a' },
  ];
  if (expMode >= 2) items.push({ lbl: 'I₂ (tras P₂)', val: (I.I2 * 100).toFixed(1) + '%', color: '#8a2020' });
  if (expMode >= 3) items.push({ lbl: 'I₃ (tras P₃)', val: (I.I3 * 100).toFixed(1) + '%', color: '#1a5a2a' });
  grid.innerHTML = items.map(i => `<div class="metric"><div class="m-lbl">${i.lbl}</div><div class="m-val" style="color:${i.color}">${i.val}</div></div>`).join('');

  const pct = Math.round(I.final * 100);
  document.getElementById('ibarFill').style.width = pct + '%';
  const c = pct > 50 ? 'var(--gold)' : pct > 15 ? '#c06020' : 'var(--red)';
  document.getElementById('ibarFill').style.background = c;
  document.getElementById('ibarVal').textContent = pct + '%';

  // Formula
  let ftxt, fsub;
  if (expMode === 1) {
    ftxt = 'I₁ = I₀ / 2';
    fsub = 'Luz no polarizada → primer polarizador';
  } else if (expMode === 2) {
    const ang = Math.round(Math.abs(expAngles[1] - expAngles[0]));
    ftxt = `I₂ = I₁ · cos²(${ang}°) = ${(I.I2 * 100).toFixed(1)}%`;
    fsub = 'Ley de Malus aplicada al 2° polarizador';
  } else {
    const a12 = Math.round(Math.abs(expAngles[1] - expAngles[0]));
    const a23 = Math.round(Math.abs(expAngles[2] - expAngles[1]));
    ftxt = `I₃ = I₁·cos²(${a12}°)·cos²(${a23}°) = ${(I.I3 * 100).toFixed(1)}%`;
    fsub = 'Ley de Malus aplicada dos veces';
  }
  document.getElementById('formulaText').textContent = ftxt;
  document.getElementById('formulaSub').textContent = fsub;

  // Callout
  let msg = '';
  if (expMode === 1) {
    msg = `<b>Un polarizador</b>: La luz entra no polarizada y el filtro selecciona solo una dirección de oscilación del campo E. La intensidad se reduce exactamente a la <b>mitad</b>.`;
  } else if (expMode === 2) {
    const ang = Math.abs(expAngles[1] - expAngles[0]);
    if (ang >= 88 && ang <= 92) msg = `<b>🔴 Extinción total</b>: Los polarizadores están cruzados a ~90°. cos²(90°) = 0, por lo que no pasa luz.`;
    else if (ang <= 5) msg = `<b>🟢 Transmisión máxima</b>: Los ejes están alineados. cos²(0°) = 1, se transmite toda la intensidad I₁.`;
    else if (Math.abs(ang - 45) <= 5) msg = `<b>⚡ θ ≈ 45°</b>: Se transmite exactamente la <b>mitad</b> de I₁, es decir, 25% de I₀.`;
    else msg = `<b>Ley de Malus activa</b>: Con Δθ = ${Math.round(ang)}°, se transmite I₂ = I₁·cos²(${Math.round(ang)}°) = <b>${(expIntensities().I2 * 100).toFixed(1)}%</b> de I₀.`;
  } else {
    msg = `<b>Tres polarizadores</b>: El polarizador intermedio re-polariza la luz en una dirección nueva, permitiendo que el tercer filtro transmita algo. Esto es la <b>paradoja del tercer polarizador</b>.`;
  }
  document.getElementById('expCallout').innerHTML = msg;
}

// ─── DRAW EXPERIMENT ───
function resizeExpCanvas() {
  const rect = expCanvas.parentElement.getBoundingClientRect();
  expCanvas.width = rect.width - 80;
  expCanvas.height = 280;
}

function drawExp() {
  resizeExpCanvas();
  const W = expCanvas.width, H = expCanvas.height;
  expCtx.clearRect(0, 0, W, H);

  // Background
  expCtx.fillStyle = '#f5f0e6';
  expCtx.fillRect(0, 0, W, H);
  // Grid
  expCtx.strokeStyle = 'rgba(180,168,148,0.3)'; expCtx.lineWidth = 1;
  for (let x = 0; x < W; x += 35) { expCtx.beginPath(); expCtx.moveTo(x,0); expCtx.lineTo(x,H); expCtx.stroke(); }
  for (let y = 0; y < H; y += 35) { expCtx.beginPath(); expCtx.moveTo(0,y); expCtx.lineTo(W,y); expCtx.stroke(); }

  const I = expIntensities();
  const numEl = expMode + 2;
  const margin = 60;
  const step = (W - margin * 2) / (numEl - 1);
  const pos = Array.from({ length: numEl }, (_, i) => ({ x: margin + i * step, y: H / 2 }));

  const iSeq = [1, I.I1, I.I2, I.I3, I.final].filter(v => v !== null);
  const angSeq = [null, expAngles[0], expMode >= 2 ? expAngles[1] : null, expMode >= 3 ? expAngles[2] : null].filter((v, i) => i <= expMode);

  // Beams
  for (let s = 0; s < pos.length - 1; s++) {
    const x1 = pos[s].x + 30, x2 = pos[s + 1].x - 22;
    const bI = iSeq[s] ?? 0;
    if (bI < 0.002) continue;
    const bH = 6 + bI * 40;
    const alpha = 0.1 + bI * 0.55;
    const grd = expCtx.createLinearGradient(0, H/2 - bH, 0, H/2 + bH);
    grd.addColorStop(0, `rgba(255,200,40,0)`);
    grd.addColorStop(0.4, `rgba(255,180,20,${alpha * 0.6})`);
    grd.addColorStop(0.5, `rgba(255,220,80,${alpha})`);
    grd.addColorStop(0.6, `rgba(255,180,20,${alpha * 0.6})`);
    grd.addColorStop(1, `rgba(255,200,40,0)`);
    expCtx.fillStyle = grd;
    expCtx.fillRect(x1, H/2 - bH * 1.5, x2 - x1, bH * 3);
    drawExpWave(x1, x2, H/2, bI, s === 0 ? null : angSeq[s - 0] !== undefined ? (expAngles[s - 1] ?? 0) : 0, s === 0);
  }

  // Elements
  for (let i = 0; i < pos.length; i++) {
    const isFilter = i > 0 && i < pos.length - 1;
    if (i === 0) drawExpSource(pos[i].x, pos[i].y);
    else if (isFilter) drawExpFilter(pos[i].x, pos[i].y, expAngles[i - 1], i - 1);
    else drawExpScreen(pos[i].x, pos[i].y, I.final);
  }
}

function drawExpWave(x1, x2, cy, intensity, angle, isUnpol) {
  const amp = 5 + intensity * 22;
  const freq = 0.028;
  const ph = expT * 0.04;
  if (isUnpol) {
    for (let a = 0; a < 5; a++) {
      const r = (a / 5) * Math.PI;
      expCtx.strokeStyle = `rgba(160,120,20,0.18)`;
      expCtx.lineWidth = 1;
      expCtx.beginPath();
      for (let x = x1; x <= x2; x += 3) {
        const w = Math.sin((x - x1) * freq + ph) * amp;
        x === x1 ? expCtx.moveTo(x, cy + Math.cos(r) * w) : expCtx.lineTo(x, cy + Math.cos(r) * w);
      }
      expCtx.stroke();
    }
    return;
  }
  const cosA = Math.cos((angle || 0) * Math.PI / 180);
  expCtx.strokeStyle = `rgba(180,100,10,${0.35 + intensity * 0.45})`;
  expCtx.lineWidth = 1.5 + intensity;
  expCtx.beginPath();
  for (let x = x1; x <= x2; x += 2) {
    const w = Math.sin((x - x1) * freq + ph) * amp;
    x === x1 ? expCtx.moveTo(x, cy + cosA * w) : expCtx.lineTo(x, cy + cosA * w);
  }
  expCtx.stroke();
  for (let x = x1 + 25; x < x2; x += 50) {
    const w = Math.sin((x - x1) * freq + ph) * amp;
    const ey = cosA * w;
    if (Math.abs(ey) > amp * 0.25) {
      expCtx.strokeStyle = `rgba(160,80,0,${0.5 + intensity * 0.3})`;
      expCtx.lineWidth = 1.5;
      expCtx.beginPath(); expCtx.moveTo(x, cy); expCtx.lineTo(x, cy + ey); expCtx.stroke();
      const d = ey > 0 ? 1 : -1;
      expCtx.fillStyle = `rgba(160,80,0,${0.5 + intensity * 0.3})`;
      expCtx.beginPath(); expCtx.moveTo(x, cy + ey); expCtx.lineTo(x-3, cy + ey - d*6); expCtx.lineTo(x+3, cy + ey - d*6); expCtx.closePath(); expCtx.fill();
    }
  }
}

const FCOLS = [{ rim:'#1a4a7a', ax:'#2a7acc' }, { rim:'#8a2020', ax:'#cc4040' }, { rim:'#1a5a2a', ax:'#2a9a4a' }];

function drawExpFilter(x, cy, angle, idx) {
  const FH = 90, FW = 24;
  const col = FCOLS[idx];
  const rad = angle * Math.PI / 180;
  expCtx.save();
  expCtx.shadowColor = 'rgba(0,0,0,0.12)'; expCtx.shadowBlur = 8; expCtx.shadowOffsetY = 3;
  expCtx.strokeStyle = col.rim; expCtx.lineWidth = 2;
  expCtx.fillStyle = `${col.rim}18`;
  rrect(expCtx, x - FW/2, cy - FH/2, FW, FH, 4); expCtx.fill(); expCtx.stroke();
  expCtx.shadowBlur = 0; expCtx.shadowOffsetY = 0;
  // stripes
  expCtx.save(); expCtx.beginPath(); rrect(expCtx, x-FW/2, cy-FH/2, FW, FH, 4); expCtx.clip();
  expCtx.strokeStyle = `${col.rim}28`; expCtx.lineWidth = 1;
  for (let ly = cy - FH/2; ly < cy + FH/2; ly += 7) { expCtx.beginPath(); expCtx.moveTo(x-FW/2, ly); expCtx.lineTo(x+FW/2, ly); expCtx.stroke(); }
  expCtx.restore();
  // axis
  const aLen = 32;
  expCtx.strokeStyle = col.ax; expCtx.lineWidth = 2.5;
  expCtx.shadowColor = col.ax; expCtx.shadowBlur = 6;
  expCtx.beginPath();
  expCtx.moveTo(x - Math.sin(rad)*aLen, cy + Math.cos(rad)*aLen);
  expCtx.lineTo(x + Math.sin(rad)*aLen, cy - Math.cos(rad)*aLen);
  expCtx.stroke(); expCtx.shadowBlur = 0;
  expCtx.fillStyle = col.ax; expCtx.beginPath(); expCtx.arc(x, cy, 3, 0, Math.PI*2); expCtx.fill();
  // drag circle
  expCtx.strokeStyle = `${col.rim}60`; expCtx.lineWidth = 1.2; expCtx.setLineDash([3,4]);
  expCtx.beginPath(); expCtx.arc(x, cy, FH/2+10, 0, Math.PI*2); expCtx.stroke(); expCtx.setLineDash([]);
  // label
  expCtx.restore();
  expCtx.fillStyle = col.rim; expCtx.font = "bold 11px 'DM Mono'"; expCtx.textAlign = 'center';
  expCtx.fillText(`P${idx+1}`, x, cy + FH/2 + 16);
  expCtx.fillStyle = '#7a7268'; expCtx.font = "10px 'DM Mono'";
  expCtx.fillText(Math.round(angle) + '°', x, cy + FH/2 + 28);
}

function drawExpSource(x, cy) {
  const grd = expCtx.createRadialGradient(x, cy, 0, x, cy, 50);
  grd.addColorStop(0, 'rgba(255,210,60,0.5)'); grd.addColorStop(0.5, 'rgba(255,190,30,0.12)'); grd.addColorStop(1, 'transparent');
  expCtx.fillStyle = grd; expCtx.beginPath(); expCtx.arc(x, cy, 50, 0, Math.PI*2); expCtx.fill();
  expCtx.fillStyle = '#f5c030'; expCtx.strokeStyle = '#c09010'; expCtx.lineWidth = 2;
  expCtx.shadowColor = 'rgba(255,200,40,0.5)'; expCtx.shadowBlur = 14;
  expCtx.beginPath(); expCtx.arc(x, cy, 18, 0, Math.PI*2); expCtx.fill(); expCtx.stroke();
  expCtx.shadowBlur = 0;
  for (let i = 0; i < 8; i++) {
    const a = (i/8)*Math.PI*2;
    const r1 = 22, r2 = 30 + Math.sin(expT*0.06+i)*2;
    expCtx.strokeStyle = 'rgba(180,130,10,0.6)'; expCtx.lineWidth = 1.8;
    expCtx.beginPath(); expCtx.moveTo(x+Math.cos(a)*r1, cy+Math.sin(a)*r1); expCtx.lineTo(x+Math.cos(a)*r2, cy+Math.sin(a)*r2); expCtx.stroke();
  }
  expCtx.fillStyle = '#3a2a08'; expCtx.font = "bold 10px 'DM Mono'"; expCtx.textAlign = 'center';
  expCtx.fillText('I₀', x, cy+4);
  expCtx.fillStyle = '#7a7268'; expCtx.font = "10px 'DM Mono'";
  expCtx.fillText('Fuente', x, cy+38);
}

function drawExpScreen(x, cy, intensity) {
  const SH = 90;
  if (intensity > 0.01) {
    const grd = expCtx.createRadialGradient(x, cy, 0, x, cy, 60 * intensity);
    grd.addColorStop(0, `rgba(255,200,50,${intensity*0.6})`); grd.addColorStop(0.6, `rgba(255,180,20,${intensity*0.15})`); grd.addColorStop(1, 'transparent');
    expCtx.fillStyle = grd; expCtx.beginPath(); expCtx.arc(x, cy, 80, 0, Math.PI*2); expCtx.fill();
  }
  expCtx.fillStyle = '#2a2018'; expCtx.strokeStyle = '#3a3028'; expCtx.lineWidth = 1.5;
  expCtx.shadowColor = 'rgba(0,0,0,0.2)'; expCtx.shadowBlur = 6;
  rrect(expCtx, x-9, cy-SH/2, 18, SH, 3); expCtx.fill(); expCtx.stroke(); expCtx.shadowBlur = 0;
  if (intensity > 0.01) {
    const sh = 15 + intensity * 55;
    const grd2 = expCtx.createLinearGradient(0, cy-sh, 0, cy+sh);
    grd2.addColorStop(0,'transparent'); grd2.addColorStop(0.3,`rgba(255,210,60,${intensity*0.85})`); grd2.addColorStop(0.5,`rgba(255,240,130,${intensity})`); grd2.addColorStop(0.7,`rgba(255,210,60,${intensity*0.85})`); grd2.addColorStop(1,'transparent');
    expCtx.fillStyle = grd2; expCtx.fillRect(x-7, cy-sh, 14, sh*2);
  }
  expCtx.textAlign = 'center'; expCtx.fillStyle = '#7a7268'; expCtx.font = "10px 'DM Mono'";
  expCtx.fillText('Pantalla', x, cy+SH/2+16);
  expCtx.font = "bold 10px 'DM Mono'";
  expCtx.fillStyle = intensity > 0.4 ? '#c08010' : intensity > 0.1 ? '#7a6040' : '#4a4030';
  expCtx.fillText((intensity*100).toFixed(1)+'%', x, cy+SH/2+28);
}

function rrect(c, x, y, w, h, r) {
  c.beginPath(); c.moveTo(x+r,y); c.lineTo(x+w-r,y); c.quadraticCurveTo(x+w,y,x+w,y+r); c.lineTo(x+w,y+h-r); c.quadraticCurveTo(x+w,y+h,x+w-r,y+h); c.lineTo(x+r,y+h); c.quadraticCurveTo(x,y+h,x,y+h-r); c.lineTo(x,y+r); c.quadraticCurveTo(x,y,x+r,y); c.closePath();
}

// Drag interaction
function expFilterPos(idx) {
  const numEl = expMode + 2;
  const margin = 60;
  const W = expCanvas.width;
  const step = (W - margin * 2) / (numEl - 1);
  return { x: margin + (idx + 1) * step, y: expCanvas.height / 2 };
}

expCanvas.addEventListener('mousedown', e => {
  const r = expCanvas.getBoundingClientRect();
  const sx = expCanvas.width / r.width, sy = expCanvas.height / r.height;
  const mx = (e.clientX - r.left) * sx, my = (e.clientY - r.top) * sy;
  for (let i = 0; i < expMode; i++) {
    const p = expFilterPos(i);
    if (Math.hypot(mx - p.x, my - p.y) < 65) { expDragging = i; break; }
  }
});
expCanvas.addEventListener('mousemove', e => {
  const r = expCanvas.getBoundingClientRect();
  const sx = expCanvas.width / r.width, sy = expCanvas.height / r.height;
  const mx = (e.clientX - r.left) * sx, my = (e.clientY - r.top) * sy;
  if (expDragging >= 0) {
    const p = expFilterPos(expDragging);
    const ang = ((Math.atan2(my - p.y, mx - p.x) * 180 / Math.PI) % 180 + 180) % 180;
    expAngles[expDragging] = ang;
    document.getElementById(`sl${expDragging+1}`).value = ang;
    document.getElementById(`lbl${expDragging+1}`).textContent = Math.round(ang) + '°';
    updateExp();
  } else {
    let hit = false;
    for (let i = 0; i < expMode; i++) { const p = expFilterPos(i); if (Math.hypot(mx-p.x,my-p.y)<65){hit=true;break;} }
    expCanvas.style.cursor = hit ? 'grab' : 'default';
  }
});
expCanvas.addEventListener('mouseup', () => { expDragging = -1; expCanvas.style.cursor = 'default'; });
expCanvas.addEventListener('mouseleave', () => { expDragging = -1; });

// ═══════════════════════════════════════════════
// SECTION 2 — CONCEPTS
// ═══════════════════════════════════════════════
let conceptT = 0;
function drawConceptCanvases() {
  drawUnpolar();
  drawPolarized();
  drawAxis();
  drawTypes();
}

function drawUnpolar() {
  const c = document.getElementById('unpolarCanvas');
  c.width = c.offsetWidth; const W = c.width, H = c.height;
  const ctx2 = c.getContext('2d');
  ctx2.clearRect(0,0,W,H);
  ctx2.fillStyle = '#f0ebe0'; ctx2.fillRect(0,0,W,H);
  const cx = W/2, cy = H/2;
  // Multiple E vectors
  for (let i = 0; i < 8; i++) {
    const a = (i/8)*Math.PI*2;
    const len = 38;
    ctx2.strokeStyle = `hsl(${i*45},60%,45%)`; ctx2.lineWidth = 2;
    ctx2.beginPath(); ctx2.moveTo(cx - Math.cos(a)*len, cy - Math.sin(a)*len); ctx2.lineTo(cx + Math.cos(a)*len, cy + Math.sin(a)*len); ctx2.stroke();
    // arrowhead
    ctx2.fillStyle = `hsl(${i*45},60%,45%)`;
    const ax = cx + Math.cos(a)*len, ay = cy + Math.sin(a)*len;
    ctx2.beginPath(); ctx2.moveTo(ax, ay); ctx2.lineTo(ax-Math.cos(a+0.4)*8, ay-Math.sin(a+0.4)*8); ctx2.lineTo(ax-Math.cos(a-0.4)*8, ay-Math.sin(a-0.4)*8); ctx2.closePath(); ctx2.fill();
  }
  // Propagation arrow
  ctx2.strokeStyle = '#3a3028'; ctx2.lineWidth = 2; ctx2.fillStyle = '#3a3028';
  ctx2.beginPath(); ctx2.moveTo(cx - 55, cy + 50); ctx2.lineTo(cx + 55, cy + 50); ctx2.stroke();
  ctx2.beginPath(); ctx2.moveTo(cx+55, cy+50); ctx2.lineTo(cx+47, cy+44); ctx2.lineTo(cx+47, cy+56); ctx2.closePath(); ctx2.fill();
  ctx2.font = "10px 'DM Mono'"; ctx2.fillStyle = '#7a7268'; ctx2.textAlign = 'center'; ctx2.fillText('z (propagación)', cx, cy+68);
  ctx2.textAlign = 'left'; ctx2.fillText('E oscila en todas', 12, 20);
  ctx2.fillText('las direcciones', 12, 33);
}

function drawPolarized() {
  const c = document.getElementById('polarCanvas');
  c.width = c.offsetWidth; const W = c.width, H = c.height;
  const ctx2 = c.getContext('2d');
  ctx2.clearRect(0,0,W,H); ctx2.fillStyle = '#f0ebe0'; ctx2.fillRect(0,0,W,H);
  const cx = W/2, cy = H/2;
  // Single oscillation
  const len = 42;
  ctx2.strokeStyle = '#1a4a7a'; ctx2.lineWidth = 2.5;
  ctx2.beginPath(); ctx2.moveTo(cx, cy-len); ctx2.lineTo(cx, cy+len); ctx2.stroke();
  // arrows
  [[cx, cy-len, -1],[cx, cy+len, 1]].forEach(([ax,ay,d]) => {
    ctx2.fillStyle = '#1a4a7a';
    ctx2.beginPath(); ctx2.moveTo(ax, ay); ctx2.lineTo(ax-6, ay-d*10); ctx2.lineTo(ax+6, ay-d*10); ctx2.closePath(); ctx2.fill();
  });
  // Wave propagation
  ctx2.strokeStyle = '#c8922a'; ctx2.lineWidth = 1.5;
  ctx2.beginPath();
  for (let x = 10; x < W-10; x += 2) {
    const y = cy + Math.sin((x/W)*Math.PI*3) * 35;
    x===10 ? ctx2.moveTo(x,y) : ctx2.lineTo(x,y);
  }
  ctx2.stroke();
  ctx2.font = "10px 'DM Mono'"; ctx2.fillStyle = '#1a4a7a'; ctx2.textAlign = 'center';
  ctx2.fillText('E (vertical)', cx+50, cy);
  ctx2.fillStyle = '#7a7268';
  ctx2.fillText('Eje de polarización: 90°', cx, H-10);
}

function drawAxis() {
  const c = document.getElementById('axisCanvas');
  c.width = c.offsetWidth; const W = c.width, H = c.height;
  const ctx2 = c.getContext('2d');
  ctx2.clearRect(0,0,W,H); ctx2.fillStyle = '#f0ebe0'; ctx2.fillRect(0,0,W,H);
  const cx = W/2, cy = H/2;
  // Filter axis (45 deg)
  const axAngle = 45 * Math.PI/180;
  const axLen = 48;
  ctx2.strokeStyle = '#8a2020'; ctx2.lineWidth = 2; ctx2.setLineDash([5,3]);
  ctx2.beginPath(); ctx2.moveTo(cx-Math.cos(axAngle)*axLen, cy-Math.sin(axAngle)*axLen); ctx2.lineTo(cx+Math.cos(axAngle)*axLen, cy+Math.sin(axAngle)*axLen); ctx2.stroke(); ctx2.setLineDash([]);
  // Incoming E (vertical)
  ctx2.strokeStyle = '#1a4a7a'; ctx2.lineWidth = 2.5;
  const elen = 40;
  ctx2.beginPath(); ctx2.moveTo(cx, cy); ctx2.lineTo(cx, cy-elen); ctx2.stroke();
  ctx2.fillStyle='#1a4a7a'; ctx2.beginPath(); ctx2.moveTo(cx,cy-elen); ctx2.lineTo(cx-4,cy-elen+8); ctx2.lineTo(cx+4,cy-elen+8); ctx2.closePath(); ctx2.fill();
  // Projection
  const projLen = elen * Math.cos(45*Math.PI/180);
  ctx2.strokeStyle = '#1a7a3a'; ctx2.lineWidth = 2.5;
  ctx2.beginPath(); ctx2.moveTo(cx,cy); ctx2.lineTo(cx+Math.sin(axAngle)*projLen, cy-Math.cos(axAngle)*projLen); ctx2.stroke();
  // angle arc
  ctx2.strokeStyle = '#c8922a'; ctx2.lineWidth = 1.5;
  ctx2.beginPath(); ctx2.arc(cx, cy, 22, -Math.PI/2, -axAngle, false); ctx2.stroke();
  ctx2.font = "10px 'DM Mono'"; ctx2.fillStyle = '#c8922a'; ctx2.textAlign = 'left';
  ctx2.fillText('θ=45°', cx+18, cy-18);
  ctx2.fillStyle = '#1a4a7a'; ctx2.fillText('E₀', cx+3, cy-elen-4);
  ctx2.fillStyle = '#1a7a3a'; ctx2.fillText('E₀cosθ', cx+projLen*0.5+5, cy-projLen*0.5);
  ctx2.fillStyle = '#8a2020'; ctx2.textAlign='center'; ctx2.fillText('Eje del filtro', cx, H-10);
}

function drawTypes() {
  const c = document.getElementById('typesCanvas');
  c.width = c.offsetWidth; const W = c.width, H = c.height;
  const ctx2 = c.getContext('2d');
  ctx2.clearRect(0,0,W,H); ctx2.fillStyle = '#f0ebe0'; ctx2.fillRect(0,0,W,H);
  const types = [
    { label: 'Lineal', col: '#1a4a7a', draw: (cx,cy) => {
      ctx2.strokeStyle='#1a4a7a'; ctx2.lineWidth=2;
      ctx2.beginPath(); ctx2.moveTo(cx-30,cy-25); ctx2.lineTo(cx+30,cy+25); ctx2.stroke();
    }},
    { label: 'Circular', col: '#8a2020', draw: (cx,cy) => {
      ctx2.strokeStyle='#8a2020'; ctx2.lineWidth=2;
      ctx2.beginPath(); ctx2.arc(cx,cy,28,0,Math.PI*2); ctx2.stroke();
      ctx2.fillStyle='#8a2020'; ctx2.beginPath(); ctx2.arc(cx+28,cy,3,0,Math.PI*2); ctx2.fill();
    }},
    { label: 'Elíptica', col: '#1a5a2a', draw: (cx,cy) => {
      ctx2.strokeStyle='#1a5a2a'; ctx2.lineWidth=2;
      ctx2.beginPath(); ctx2.ellipse(cx,cy,30,18,0.3,0,Math.PI*2); ctx2.stroke();
    }},
  ];
  const section = W / 3;
  types.forEach((t, i) => {
    const cx = section * (i + 0.5), cy = H/2 - 10;
    t.draw(cx, cy);
    ctx2.fillStyle = t.col; ctx2.font = "10px 'DM Mono'"; ctx2.textAlign = 'center';
    ctx2.fillText(t.label, cx, cy + 48);
  });
}

// ═══════════════════════════════════════════════
// SECTION 3 — MALUS
// ═══════════════════════════════════════════════
const malusCanvas = document.getElementById('malusCanvas');
const malusCtx = malusCanvas.getContext('2d');
const graphCanvas2 = document.getElementById('graphCanvas');
const graphCtx = graphCanvas2.getContext('2d');

function setMalusAngle(a) {
  document.getElementById('malusSl').value = a;
  updateMalus();
}

function updateMalus() {
  const theta = +document.getElementById('malusSl').value;
  const I0pct = +document.getElementById('malusI0Sl').value / 100;
  document.getElementById('malusTheta').textContent = theta + '°';
  document.getElementById('malusI0').textContent = Math.round(I0pct*100) + '%';
  const I1 = I0pct * 0.5;
  const rad = theta * Math.PI / 180;
  const I2 = I1 * Math.cos(rad) ** 2;
  const pct2 = (I2 * 100).toFixed(2);
  document.getElementById('malusCalc').innerHTML = `
    <div>I₀ = <b>${Math.round(I0pct*100)}%</b></div>
    <div>I₁ = I₀/2 = <b>${(I1*100).toFixed(1)}%</b></div>
    <div>θ = <b>${theta}°</b></div>
    <div>cos(${theta}°) = <b>${Math.cos(rad).toFixed(4)}</b></div>
    <div>cos²(${theta}°) = <b>${(Math.cos(rad)**2).toFixed(4)}</b></div>
    <div style="border-top:1px solid var(--border);padding-top:6px;margin-top:4px;">I₂ = I₁ · cos²(θ) = <b style="color:var(--gold)">${pct2}%</b></div>
  `;
  drawMalus(theta, I0pct);
  drawGraph(theta, I0pct);
}

function drawMalus(theta, I0) {
  const r = malusCanvas.parentElement.getBoundingClientRect();
  malusCanvas.width = r.width - 80; malusCanvas.height = 320;
  const W = malusCanvas.width, H = malusCanvas.height;
  malusCtx.clearRect(0,0,W,H);
  malusCtx.fillStyle = '#f0ebe0'; malusCtx.fillRect(0,0,W,H);
  malusCtx.strokeStyle = 'rgba(180,168,148,0.3)'; malusCtx.lineWidth = 1;
  for (let x = 0; x < W; x += 40) { malusCtx.beginPath(); malusCtx.moveTo(x,0); malusCtx.lineTo(x,H); malusCtx.stroke(); }
  for (let y = 0; y < H; y += 40) { malusCtx.beginPath(); malusCtx.moveTo(0,y); malusCtx.lineTo(W,y); malusCtx.stroke(); }

  const cy = H/2;
  const I1 = I0 * 0.5;
  const rad = theta * Math.PI/180;
  const I2 = I1 * Math.cos(rad)**2;

  const positions = [
    { x: W*0.1, type:'source' },
    { x: W*0.35, type:'filter', angle: 0, idx: 0 },
    { x: W*0.65, type:'filter', angle: theta, idx: 1 },
    { x: W*0.9, type:'screen' },
  ];
  const iSeq = [I0, I1, I2];

  // Beams
  const segs = [[positions[0].x+28, positions[1].x-14, iSeq[0]], [positions[1].x+14, positions[2].x-14, iSeq[1]], [positions[2].x+14, positions[3].x-12, iSeq[2]]];
  segs.forEach(([x1,x2,bI], si) => {
    if (bI < 0.002) return;
    const bH = 6 + bI * 45;
    const alpha = 0.1 + bI * 0.6;
    const grd = malusCtx.createLinearGradient(0,cy-bH,0,cy+bH);
    grd.addColorStop(0,'rgba(255,200,40,0)'); grd.addColorStop(0.5,`rgba(255,220,80,${alpha})`); grd.addColorStop(1,'rgba(255,200,40,0)');
    malusCtx.fillStyle = grd; malusCtx.fillRect(x1, cy-bH*1.5, x2-x1, bH*3);
    // wave
    const angle = si === 0 ? null : si === 1 ? 0 : theta;
    expCtx.save && null;
    const c2 = malusCtx;
    const amp2 = 5 + bI*24, freq2=0.03, ph2=expT*0.04;
    if (si === 0) { // unpol
      for (let a=0;a<5;a++){const r2=(a/5)*Math.PI; c2.strokeStyle=`rgba(160,120,20,0.18)`; c2.lineWidth=1; c2.beginPath(); for(let x=x1;x<=x2;x+=3){const w=Math.sin((x-x1)*freq2+ph2)*amp2; x===x1?c2.moveTo(x,cy+Math.cos(r2)*w):c2.lineTo(x,cy+Math.cos(r2)*w);} c2.stroke(); }
    } else {
      const cosA = Math.cos(angle*Math.PI/180);
      c2.strokeStyle=`rgba(180,100,10,${0.3+bI*0.5})`; c2.lineWidth=1.5+bI;
      c2.beginPath(); for(let x=x1;x<=x2;x+=2){const w=Math.sin((x-x1)*freq2+ph2)*amp2; x===x1?c2.moveTo(x,cy+cosA*w):c2.lineTo(x,cy+cosA*w);} c2.stroke();
    }
  });

  positions.forEach(p => {
    if (p.type === 'source') drawMalusSource(malusCtx, p.x, cy, I0);
    else if (p.type === 'filter') drawMalusFilter(malusCtx, p.x, cy, p.angle, p.idx);
    else drawMalusScreen(malusCtx, p.x, cy, I2);
  });

  // Angle arc between filters
  const f1x = positions[1].x, f2x = positions[2].x;
  const midX = (f1x + f2x) / 2;
  malusCtx.strokeStyle = '#c8922a'; malusCtx.lineWidth = 1.5; malusCtx.setLineDash([4,3]);
  malusCtx.beginPath(); malusCtx.moveTo(f1x, cy - 55); malusCtx.lineTo(f1x, cy + 55); malusCtx.stroke();
  malusCtx.beginPath(); malusCtx.moveTo(f2x, cy - 55); malusCtx.lineTo(f2x, cy + 55); malusCtx.stroke();
  malusCtx.setLineDash([]);
  malusCtx.font = "bold 12px 'DM Mono'"; malusCtx.fillStyle = '#c8922a'; malusCtx.textAlign = 'center';
  malusCtx.fillText(`θ = ${theta}°`, midX, cy - 62);
}

function drawMalusSource(c, x, cy, I0) {
  c.fillStyle='#f5c030'; c.strokeStyle='#c09010'; c.lineWidth=2;
  c.shadowColor='rgba(255,200,40,0.4)'; c.shadowBlur=12;
  c.beginPath(); c.arc(x,cy,16,0,Math.PI*2); c.fill(); c.stroke(); c.shadowBlur=0;
  c.fillStyle='#3a2a08'; c.font="bold 10px 'DM Mono'"; c.textAlign='center'; c.fillText('I₀',x,cy+4);
  c.fillStyle='#7a7268'; c.font="10px 'DM Mono'"; c.fillText('Fuente',x,cy+32);
}

function drawMalusFilter(c, x, cy, angle, idx) {
  const col = FCOLS[idx]; const rad = angle*Math.PI/180;
  const FH=80, FW=22;
  c.strokeStyle=col.rim; c.lineWidth=2; c.fillStyle=`${col.rim}18`;
  c.shadowColor='rgba(0,0,0,0.1)'; c.shadowBlur=6; c.shadowOffsetY=2;
  rrect(c,x-FW/2,cy-FH/2,FW,FH,4); c.fill(); c.stroke(); c.shadowBlur=0; c.shadowOffsetY=0;
  c.save(); c.beginPath(); rrect(c,x-FW/2,cy-FH/2,FW,FH,4); c.clip();
  c.strokeStyle=`${col.rim}28`; c.lineWidth=1;
  for(let ly=cy-FH/2;ly<cy+FH/2;ly+=6){c.beginPath(); c.moveTo(x-FW/2,ly); c.lineTo(x+FW/2,ly); c.stroke();} c.restore();
  const aLen=28;
  c.strokeStyle=col.ax; c.lineWidth=2.5; c.shadowColor=col.ax; c.shadowBlur=5;
  c.beginPath(); c.moveTo(x-Math.sin(rad)*aLen,cy+Math.cos(rad)*aLen); c.lineTo(x+Math.sin(rad)*aLen,cy-Math.cos(rad)*aLen); c.stroke(); c.shadowBlur=0;
  c.fillStyle=col.rim; c.font="bold 11px 'DM Mono'"; c.textAlign='center'; c.fillText(`P${idx+1}`,x,cy+FH/2+16);
  c.fillStyle='#7a7268'; c.font="10px 'DM Mono'"; c.fillText(angle+'°',x,cy+FH/2+28);
}

function drawMalusScreen(c, x, cy, I) {
  if (I>0.01){const grd=c.createRadialGradient(x,cy,0,x,cy,55*I); grd.addColorStop(0,`rgba(255,200,50,${I*0.55})`); grd.addColorStop(1,'transparent'); c.fillStyle=grd; c.beginPath(); c.arc(x,cy,70,0,Math.PI*2); c.fill();}
  c.fillStyle='#2a2018'; c.strokeStyle='#3a3028'; c.lineWidth=1.5;
  rrect(c,x-8,cy-45,16,90,3); c.fill(); c.stroke();
  if(I>0.01){const sh=12+I*50; const g=c.createLinearGradient(0,cy-sh,0,cy+sh); g.addColorStop(0,'transparent'); g.addColorStop(0.5,`rgba(255,230,80,${I})`); g.addColorStop(1,'transparent'); c.fillStyle=g; c.fillRect(x-6,cy-sh,12,sh*2);}
  c.textAlign='center'; c.fillStyle='#7a7268'; c.font="10px 'DM Mono'"; c.fillText('Pantalla',x,cy+55);
  c.font="bold 10px 'DM Mono'"; c.fillStyle=I>0.4?'#c08010':I>0.1?'#7a6040':'#4a4030';
  c.fillText((I*100).toFixed(1)+'%',x,cy+67);
}

function drawGraph(theta, I0) {
  const r = graphCanvas2.parentElement.getBoundingClientRect();
  graphCanvas2.width = r.width - 80;
  const W = graphCanvas2.width, H = graphCanvas2.height;
  graphCtx.clearRect(0,0,W,H);
  graphCtx.fillStyle = '#f0ebe0'; graphCtx.fillRect(0,0,W,H);

  const pad = { l:48, r:16, t:16, b:36 };
  const gW = W - pad.l - pad.r, gH = H - pad.t - pad.b;

  // Grid
  graphCtx.strokeStyle = 'rgba(180,168,148,0.5)'; graphCtx.lineWidth = 1;
  for (let i=0;i<=4;i++){
    const y = pad.t + (i/4)*gH;
    graphCtx.beginPath(); graphCtx.moveTo(pad.l,y); graphCtx.lineTo(W-pad.r,y); graphCtx.stroke();
    graphCtx.fillStyle='#7a7268'; graphCtx.font="10px 'DM Mono'"; graphCtx.textAlign='right';
    graphCtx.fillText(((1-i/4)*100).toFixed(0)+'%', pad.l-6, y+4);
  }
  for(let a=0;a<=6;a++){
    const x=pad.l+(a/6)*gW;
    graphCtx.beginPath(); graphCtx.moveTo(x,pad.t); graphCtx.lineTo(x,pad.t+gH); graphCtx.stroke();
    graphCtx.fillStyle='#7a7268'; graphCtx.textAlign='center';
    graphCtx.fillText((a*30)+'°',x,pad.t+gH+14);
  }

  // Curve I0*cos²
  graphCtx.strokeStyle = '#c8922a'; graphCtx.lineWidth = 2.5;
  graphCtx.beginPath();
  for(let a=0;a<=180;a++){
    const x=pad.l+(a/180)*gW;
    const y=pad.t+gH-(I0*0.5*Math.cos(a*Math.PI/180)**2)*gH;
    a===0?graphCtx.moveTo(x,y):graphCtx.lineTo(x,y);
  }
  graphCtx.stroke();

  // Current point
  const I1 = I0*0.5; const I2=I1*Math.cos(theta*Math.PI/180)**2;
  const px=pad.l+(theta/180)*gW, py=pad.t+gH-I2*gH;
  graphCtx.fillStyle='#1a4a7a'; graphCtx.shadowColor='#1a4a7a'; graphCtx.shadowBlur=8;
  graphCtx.beginPath(); graphCtx.arc(px,py,5,0,Math.PI*2); graphCtx.fill(); graphCtx.shadowBlur=0;
  graphCtx.strokeStyle='rgba(26,74,122,0.4)'; graphCtx.lineWidth=1; graphCtx.setLineDash([3,3]);
  graphCtx.beginPath(); graphCtx.moveTo(px,py); graphCtx.lineTo(px,pad.t+gH); graphCtx.stroke();
  graphCtx.beginPath(); graphCtx.moveTo(px,py); graphCtx.lineTo(pad.l,py); graphCtx.stroke();
  graphCtx.setLineDash([]);

  // Axes labels
  graphCtx.fillStyle='#3a3028'; graphCtx.font="bold 10px 'DM Mono'"; graphCtx.textAlign='center';
  graphCtx.fillText('θ (grados)', pad.l+gW/2, H-2);
  graphCtx.save(); graphCtx.translate(12, pad.t+gH/2); graphCtx.rotate(-Math.PI/2);
  graphCtx.fillText('I₂ / I₀', 0, 0); graphCtx.restore();
  graphCtx.fillStyle='#c8922a'; graphCtx.font="10px 'DM Mono'";
  graphCtx.textAlign='left'; graphCtx.fillText('I₂ = I₁·cos²(θ)', pad.l+8, pad.t+14);
}

// ═══════════════════════════════════════════════
// SECTION 4 — PARADOX
// ═══════════════════════════════════════════════
const paradoxCanvas = document.getElementById('paradoxCanvas');
const paradoxCtx = paradoxCanvas.getContext('2d');
const paradoxGraph = document.getElementById('paradoxGraph');
const paradoxGCtx = paradoxGraph.getContext('2d');

function setParadoxStep(n, el) {
  document.querySelectorAll('.step').forEach(s => s.classList.remove('active'));
  el.classList.add('active');
  const angles = [45, 45, 45, 45];
  const slider = document.getElementById('paradoxSl');
  if (n === 0) { slider.value = 90; }
  else if (n === 1) { slider.value = 45; }
  else if (n === 2) { slider.value = 45; }
  else if (n === 3) { slider.value = 45; }
  updateParadox();
}

function updateParadox() {
  const theta2 = +document.getElementById('paradoxSl').value;
  document.getElementById('paradoxLbl').textContent = theta2 + '°';

  const I0 = 1;
  const I1 = I0 * 0.5; // after P1 (0 deg)
  const d12 = theta2 * Math.PI / 180;
  const I2 = I1 * Math.cos(d12) ** 2;
  const d23 = (90 - theta2) * Math.PI / 180;
  const I3 = I2 * Math.cos(d23) ** 2;

  // Callout
  let msg;
  if (theta2 <= 5 || theta2 >= 175) {
    msg = `<b>P₂ = ${theta2}°</b>: El polarizador intermedio está alineado con P₁. No re-polariza la luz en una dirección útil para P₃. I₃ ≈ 0.`;
  } else if (theta2 >= 85 && theta2 <= 95) {
    msg = `<b>P₂ = ${theta2}°</b>: El polarizador intermedio coincide con P₃. Toda la luz que pasa por P₂ es bloqueada por P₃. I₃ ≈ 0.`;
  } else if (Math.abs(theta2 - 45) <= 5) {
    msg = `<b>🌟 P₂ = ${theta2}°</b>: ¡Máxima transmisión paradójica! P₂ re-polariza la luz a 45°. P₃ puede transmitir la componente a 90°. I₃ = <b>${(I3*100).toFixed(2)}%</b> de I₀.`;
  } else {
    msg = `<b>P₂ = ${theta2}°</b>: La luz se transmite parcialmente. I₃ = <b>${(I3*100).toFixed(2)}%</b>. Prueba θ₂ = 45° para el máximo.`;
  }
  document.getElementById('paradoxCallout').innerHTML = msg;

  drawParadox(theta2, I0, I1, I2, I3);
  drawParadoxGraph(theta2);
}

function drawParadox(theta2, I0, I1, I2, I3) {
  const r = paradoxCanvas.parentElement.getBoundingClientRect();
  paradoxCanvas.width = r.width - 40; paradoxCanvas.height = 260;
  const W = paradoxCanvas.width, H = paradoxCanvas.height;
  paradoxCtx.clearRect(0,0,W,H);
  paradoxCtx.fillStyle = '#f0ebe0'; paradoxCtx.fillRect(0,0,W,H);
  paradoxCtx.strokeStyle = 'rgba(180,168,148,0.3)'; paradoxCtx.lineWidth = 1;
  for (let x=0;x<W;x+=40){paradoxCtx.beginPath();paradoxCtx.moveTo(x,0);paradoxCtx.lineTo(x,H);paradoxCtx.stroke();}
  for (let y=0;y<H;y+=40){paradoxCtx.beginPath();paradoxCtx.moveTo(0,y);paradoxCtx.lineTo(W,y);paradoxCtx.stroke();}

  const cy = H/2;
  const positions = [
    {x:W*0.08, type:'source'},
    {x:W*0.28, type:'filter', angle:0,   idx:0},
    {x:W*0.50, type:'filter', angle:theta2, idx:1},
    {x:W*0.72, type:'filter', angle:90,  idx:2},
    {x:W*0.92, type:'screen'},
  ];
  const iSeq = [I0, I1, I2, I3];
  const segments = [
    [positions[0].x+26, positions[1].x-13, iSeq[0], null, true],
    [positions[1].x+13, positions[2].x-13, iSeq[1], 0, false],
    [positions[2].x+13, positions[3].x-13, iSeq[2], theta2, false],
    [positions[3].x+13, positions[4].x-12, iSeq[3], 90, false],
  ];
  segments.forEach(([x1,x2,bI,angle,isUnpol]) => {
    if (bI < 0.002) return;
    const bH = 5 + bI*38;
    const alpha = 0.1 + bI*0.55;
    const grd = paradoxCtx.createLinearGradient(0,cy-bH,0,cy+bH);
    grd.addColorStop(0,'rgba(255,200,40,0)'); grd.addColorStop(0.5,`rgba(255,215,70,${alpha})`); grd.addColorStop(1,'rgba(255,200,40,0)');
    paradoxCtx.fillStyle=grd; paradoxCtx.fillRect(x1,cy-bH*1.5,x2-x1,bH*3);
    const c=paradoxCtx, amp=4+bI*20, freq=0.035, ph=expT*0.04;
    if(isUnpol){for(let a=0;a<4;a++){const ra=(a/4)*Math.PI; c.strokeStyle=`rgba(160,120,20,0.2)`; c.lineWidth=1; c.beginPath(); for(let x=x1;x<=x2;x+=3){const w=Math.sin((x-x1)*freq+ph)*amp; x===x1?c.moveTo(x,cy+Math.cos(ra)*w):c.lineTo(x,cy+Math.cos(ra)*w);} c.stroke();}}
    else{const cosA=Math.cos(angle*Math.PI/180); c.strokeStyle=`rgba(180,100,10,${0.3+bI*0.5})`; c.lineWidth=1.5+bI; c.beginPath(); for(let x=x1;x<=x2;x+=2){const w=Math.sin((x-x1)*freq+ph)*amp; x===x1?c.moveTo(x,cy+cosA*w):c.lineTo(x,cy+cosA*w);} c.stroke();}
  });
  positions.forEach(p => {
    if(p.type==='source') drawMalusSource(paradoxCtx,p.x,cy,I0);
    else if(p.type==='filter') drawMalusFilter(paradoxCtx,p.x,cy,p.angle,p.idx);
    else drawMalusScreen(paradoxCtx,p.x,cy,I3);
  });
}

function drawParadoxGraph(currentTheta) {
  const r = paradoxGraph.parentElement.getBoundingClientRect();
  paradoxGraph.width = r.width - 40; paradoxGraph.height = 160;
  const W = paradoxGraph.width, H = paradoxGraph.height;
  paradoxGCtx.clearRect(0,0,W,H);
  paradoxGCtx.fillStyle = '#f0ebe0'; paradoxGCtx.fillRect(0,0,W,H);

  const pad = {l:44,r:14,t:12,b:32};
  const gW = W-pad.l-pad.r, gH = H-pad.t-pad.b;

  paradoxGCtx.strokeStyle='rgba(180,168,148,0.5)'; paradoxGCtx.lineWidth=1;
  for(let i=0;i<=4;i++){const y=pad.t+(i/4)*gH; paradoxGCtx.beginPath(); paradoxGCtx.moveTo(pad.l,y); paradoxGCtx.lineTo(W-pad.r,y); paradoxGCtx.stroke();}
  for(let i=0;i<=6;i++){const x=pad.l+(i/6)*gW; paradoxGCtx.beginPath(); paradoxGCtx.moveTo(x,pad.t); paradoxGCtx.lineTo(x,pad.t+gH); paradoxGCtx.stroke();}

  // y labels
  paradoxGCtx.fillStyle='#7a7268'; paradoxGCtx.font="9px 'DM Mono'"; paradoxGCtx.textAlign='right';
  const maxI3 = 0.125;
  for(let i=0;i<=4;i++){paradoxGCtx.fillText(((1-i/4)*maxI3*100).toFixed(1)+'%',pad.l-4,pad.t+(i/4)*gH+4);}
  paradoxGCtx.textAlign='center';
  for(let i=0;i<=6;i++){paradoxGCtx.fillText((i*30)+'°',pad.l+(i/6)*gW,pad.t+gH+14);}

  // Curve: I3 = 0.5 * cos²(t) * cos²(90-t) = 0.5 * cos²(t) * sin²(t) = 0.125 * sin²(2t)
  paradoxGCtx.strokeStyle='#8a2020'; paradoxGCtx.lineWidth=2.5;
  paradoxGCtx.beginPath();
  for(let a=0;a<=180;a++){
    const t=a*Math.PI/180;
    const i3=0.5*Math.cos(t)**2*Math.sin(t)**2; // = 0.5*cos²θ*cos²(90-θ)
    const x=pad.l+(a/180)*gW;
    const y=pad.t+gH-(i3/maxI3)*gH;
    a===0?paradoxGCtx.moveTo(x,y):paradoxGCtx.lineTo(x,y);
  }
  paradoxGCtx.stroke();

  // Marker
  const t2=currentTheta*Math.PI/180;
  const i3m=0.5*Math.cos(t2)**2*Math.sin(t2)**2;
  const mpx=pad.l+(currentTheta/180)*gW, mpy=pad.t+gH-(i3m/maxI3)*gH;
  paradoxGCtx.fillStyle='#1a4a7a'; paradoxGCtx.shadowColor='#1a4a7a'; paradoxGCtx.shadowBlur=8;
  paradoxGCtx.beginPath(); paradoxGCtx.arc(mpx,mpy,5,0,Math.PI*2); paradoxGCtx.fill(); paradoxGCtx.shadowBlur=0;

  paradoxGCtx.fillStyle='#8a2020'; paradoxGCtx.font="9px 'DM Mono'"; paradoxGCtx.textAlign='left';
  paradoxGCtx.fillText('I₃ = (I₀/2)·cos²θ·sin²θ  [P₁=0°, P₃=90°]', pad.l+6, pad.t+14);
  paradoxGCtx.fillStyle='#3a3028'; paradoxGCtx.textAlign='center';
  paradoxGCtx.fillText('θ₂ (ángulo de P₂)', pad.l+gW/2, H-2);
}

// ═══════════════════════════════════════════════
// QUIZ
// ═══════════════════════════════════════════════
const feedbacks = {
  q1: { correct: '✓ Correcto. Un polarizador absorbe las componentes perpendiculares a su eje. La luz no polarizada tiene distribución uniforme, y en promedio el 50% es paralelo al eje.', wrong: '✗ Incorrecto. Recuerda: la luz no polarizada tiene componentes en todas las direcciones. Solo pasa la componente paralela al eje, que en promedio es la mitad.' },
  q2: { correct: '✓ Correcto. cos²(90°) = 0, por lo que I = I₁·0 = 0. Los polarizadores cruzados bloquean toda la luz.', wrong: '✗ Incorrecto. Aplica la Ley de Malus: I = I₁·cos²(90°) = I₁·0 = 0.' },
  q3: { correct: '✓ Correcto. cos(60°) = 0.5, cos²(60°) = 0.25. Entonces I₂/I₁ = 0.25, y como I₁ = I₀/2, I₂/I₀ = 0.125... pero la pregunta pide I/I₀ sin el primer filtro (es decir I₂/I₁), que es 0.25.', wrong: '✗ Incorrecto. cos(60°) = 1/2, cos²(60°) = 1/4 = 0.25. Recuerda: I = I₁·cos²(θ).' },
  q4: { correct: '✓ Correcto. P₂ crea una nueva dirección de polarización intermedia. La luz re-polarizada ya no es completamente perpendicular a P₃, por lo que una fracción puede pasar.', wrong: '✗ Incorrecto. La clave es que P₂ re-polariza la luz en una dirección intermedia. Con esta nueva dirección, P₃ ya no puede extinguir completamente la luz.' },
  q5: { correct: '✓ Correcto. A 45°, cos²(45°)·cos²(45°) = 0.5·0.5 = 0.25, lo que da I₃ = I₀/2·0.25 = I₀/8 = 12.5% — el máximo posible.', wrong: '✗ Incorrecto. En 45° se maximiza sin(2θ) = sin(90°) = 1. La fórmula I₃ ∝ sin²(2θ) alcanza su máximo cuando 2θ = 90°, es decir θ = 45°.' },
  q6: { correct: '✓ Correcto. En luz polarizada circularmente, el vector E rota a frecuencia ω describiendo un círculo. La amplitud es constante, solo cambia la dirección.', wrong: '✗ Incorrecto. En polarización circular, el campo E rota en un plano perpendicular a la propagación manteniendo amplitud constante.' },
};
let answers = {};
function answer(qid, el, type) {
  if (answers[qid]) return;
  answers[qid] = type;
  const opts = el.parentElement.querySelectorAll('.quiz-opt');
  opts.forEach(o => o.style.pointerEvents = 'none');
  el.classList.add(type);
  const fb = document.getElementById('fb'+qid.slice(1));
  fb.textContent = feedbacks[qid][type];
  fb.className = 'quiz-feedback show ' + (type === 'correct' ? 'ok' : 'bad');

  if (Object.keys(answers).length === 6) {
    const score = Object.values(answers).filter(v => v === 'correct').length;
    const res = document.getElementById('quizResult');
    res.style.display = 'block';
    res.className = 'callout ' + (score >= 5 ? 'green' : score >= 3 ? '' : 'red');
    document.getElementById('quizScore').textContent = `Resultado: ${score}/6`;
    const msgs = ['Revisa los conceptos — vuelve a las secciones anteriores.','Buen trabajo — repasa los casos que fallaste.','¡Muy bien!','¡Excelente! Dominas la polarización de la luz.'];
    document.getElementById('quizMsg').textContent = msgs[Math.min(3, Math.floor(score/2))];
  }
}

// ═══════════════════════════════════════════════
// ANIMATION LOOP
// ═══════════════════════════════════════════════
function loop() {
  expT++;
  drawExp();
  // animate malus and paradox only if visible
  if (document.getElementById('sec-malus').classList.contains('visible')) {
    const theta = +document.getElementById('malusSl').value;
    const I0 = +document.getElementById('malusI0Sl').value / 100;
    drawMalus(theta, I0);
  }
  if (document.getElementById('sec-paradox').classList.contains('visible')) {
    const t2 = +document.getElementById('paradoxSl').value;
    const I1=0.5, d12=t2*Math.PI/180, I2=I1*Math.cos(d12)**2, d23=(90-t2)*Math.PI/180, I3=I2*Math.cos(d23)**2;
    drawParadox(t2,1,I1,I2,I3);
  }
  requestAnimationFrame(loop);
}

// ── INIT ──
window.addEventListener('resize', () => {
  updateExp(); updateMalus(); updateParadox();
  if (document.getElementById('sec-concept').classList.contains('visible')) drawConceptCanvases();
});

updateExp();
loop();
</script>
</body>
</html>
