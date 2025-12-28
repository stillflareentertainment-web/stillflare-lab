<head>
  <meta charset="UTF-8" />
  <title>Stillflare MAAS Engine Console</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    /* ============= CORE THEME TOKENS ============= */
    :root {
      --maas-bg: #050816;
      --maas-panel: #0e1828;
      --maas-panel-soft: #121c30;
      --maas-border: #222e46;
      --maas-glow: #ff7a1a;
      --maas-glow-soft: rgba(255, 138, 64, 0.35);
      --maas-text-main: #f5f7ff;
      --maas-text-soft: #9aa3c0;
      --maas-accent-skill: #42b3ff;
      --maas-accent-var: #ff9a3b;
      --maas-radius: 14px;
      --maas-shadow: 0 18px 40px rgba(0, 0, 0, 0.85);
    }

    body {
      margin: 0;
      background: #050816;
    }

    #maas-console-root {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: radial-gradient(circle at top left, #1b2740 0, #050816 55%);
      padding: 32px 16px;
      color: var(--maas-text-main);
      display: flex;
      justify-content: center;
      min-height: 100vh;
      box-sizing: border-box;
    }

    .maas-console {
      max-width: 1080px;
      width: 100%;
      background: linear-gradient(145deg, #050a14, #050816 55%, #02040a 100%);
      border-radius: 24px;
      border: 1px solid #232d46;
      box-shadow: var(--maas-shadow);
      padding: 24px;
      display: grid;
      grid-template-columns: minmax(0, 1.1fr) minmax(0, 1.4fr);
      gap: 24px;
      position: relative;
      overflow: hidden;
    }

    .maas-console::before {
      content: "";
      position: absolute;
      inset: -20%;
      pointer-events: none;
      background:
        radial-gradient(circle at 15% 0%, rgba(255, 138, 64, 0.06) 0, transparent 55%),
        radial-gradient(circle at 90% 10%, rgba(90, 184, 255, 0.07) 0, transparent 60%);
      opacity: 0.9;
    }

    .maas-left,
    .maas-right {
      position: relative;
      z-index: 1;
    }

    /* ============= SLIDERS ============= */
    .maas-sliders-header {
      font-size: 0.85rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: var(--maas-text-soft);
      margin-bottom: 8px;
    }

    .maas-sliders {
      display: grid;
      grid-template-columns: repeat(7, minmax(0, 1fr));
      gap: 10px;
      background: var(--maas-panel);
      border-radius: var(--maas-radius);
      padding: 14px 14px 16px;
      border: 1px solid var(--maas-border);
    }

    .maas-slider {
      display: flex;
      flex-direction: column;
      align-items: stretch;
      gap: 4px;
    }

    .maas-slider-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.8rem;
    }

    .maas-slider-label {
      font-weight: 600;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--maas-text-soft);
    }

    .maas-slider-value {
      font-variant-numeric: tabular-nums;
      font-size: 0.78rem;
      padding: 2px 6px;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.03);
      border: 1px solid rgba(255, 255, 255, 0.04);
      color: var(--maas-text-main);
    }

    .maas-slider-input {
      -webkit-appearance: none;
      appearance: none;
      width: 100%;
      height: 120px;
      writing-mode: bt-lr;
      direction: rtl;
      background: transparent;
      margin: 4px 0;
    }

    .maas-slider-input::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 14px;
      height: 14px;
      border-radius: 4px;
      background: var(--maas-glow);
      box-shadow: 0 0 8px var(--maas-glow-soft);
      margin-top: -5px;
    }

    .maas-slider-input::-webkit-slider-runnable-track {
      width: 4px;
      height: 110px;
      background: linear-gradient(to top, #1f2840, #37456a);
      border-radius: 999px;
      border: 1px solid #252f4a;
    }

    .maas-slider-input::-moz-range-thumb {
      width: 14px;
      height: 14px;
      border-radius: 4px;
      background: var(--maas-glow);
      box-shadow: 0 0 8px var(--maas-glow-soft);
      border: none;
    }

    .maas-slider-input::-moz-range-track {
      width: 4px;
      height: 110px;
      background: linear-gradient(to top, #1f2840, #37456a);
      border-radius: 999px;
      border: 1px solid #252f4a;
    }

    .maas-slider-sub {
      font-size: 0.64rem;
      color: var(--maas-text-soft);
      line-height: 1.2;
    }

    /* ============= ENGINE CORE ============= */
    .maas-core {
      margin-top: 18px;
      background: radial-gradient(circle at center, #101a2c 0, #050816 80%);
      border-radius: 999px;
      border: 1px solid #28324c;
      padding: 22px;
      position: relative;
      overflow: hidden;
      box-shadow: 0 12px 28px rgba(0, 0, 0, 0.8),
        0 0 40px rgba(255, 138, 64, 0.12);
    }

    .maas-core-ring {
      position: absolute;
      border-radius: 999px;
      border: 2px solid rgba(255, 138, 64, 0.35);
      box-shadow: 0 0 14px rgba(255, 138, 64, 0.28);
      animation: maas-spin 16s linear infinite;
    }

    .maas-core-ring-outer {
      inset: 12px;
    }

    .maas-core-ring-middle {
      inset: 26px;
      animation-duration: 24s;
      border-style: dashed;
    }

    .maas-core-ring-inner {
      inset: 40px;
      animation-duration: 12s;
      border-color: rgba(66, 179, 255, 0.5);
    }

    .maas-core-center {
      position: relative;
      width: 64px;
      height: 64px;
      border-radius: 999px;
      margin: 16px auto 4px;
      background: radial-gradient(circle at 30% 30%, #ffd7ad 0, #ff7a1a 30%, #351814 75%);
      box-shadow:
        0 0 24px rgba(255, 138, 64, 0.8),
        inset 0 0 12px rgba(0, 0, 0, 0.7);
    }

    .maas-core-label {
      text-align: center;
      font-size: 0.7rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: var(--maas-text-soft);
    }

    @keyframes maas-spin {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(360deg);
      }
    }

    /* ============= GRAPH PANEL ============= */
    .maas-graph-panel {
      background: var(--maas-panel);
      border-radius: var(--maas-radius);
      border: 1px solid var(--maas-border);
      padding: 16px 18px 14px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.75);
    }

    .maas-graph-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.9rem;
      margin-bottom: 8px;
    }

    .maas-graph-legend {
      display: flex;
      gap: 10px;
      font-size: 0.75rem;
    }

    .maas-graph-legend span::before {
      content: "";
      display: inline-block;
      width: 10px;
      height: 3px;
      border-radius: 999px;
      margin-right: 4px;
    }

    .legend-skill::before {
      background: var(--maas-accent-skill);
    }

    .legend-var::before {
      background: var(--maas-accent-var);
    }

    #maas-graph {
      width: 100%;
      display: block;
      border-radius: 10px;
      background: #050816;
      border: 1px solid #252f4a;
    }

    .maas-current-metrics {
      margin-top: 10px;
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 10px;
    }

    .maas-metric {
      background: var(--maas-panel-soft);
      border-radius: 10px;
      border: 1px solid #252f4a;
      padding: 8px 10px;
    }

    .maas-metric-label {
      font-size: 0.73rem;
      color: var(--maas-text-soft);
      margin-bottom: 2px;
    }

    .maas-metric-value {
      font-variant-numeric: tabular-nums;
      font-size: 1rem;
    }

    .maas-metric-value.small {
      font-size: 0.76rem;
      line-height: 1.25;
    }

    /* ============= CONTROLS & BUTTONS ============= */
    .maas-controls {
      margin-top: 16px;
      background: linear-gradient(145deg, #0b1525, #050816);
      border-radius: var(--maas-radius);
      border: 1px solid var(--maas-border);
      padding: 16px 16px 12px;
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .maas-btn {
      border-radius: 999px;
      border: 1px solid transparent;
      padding: 10px 18px;
      font-size: 0.9rem;
      cursor: pointer;
      font-weight: 500;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: all 0.15s ease-out;
      font-family: inherit;
    }

    .maas-btn.primary {
      background: radial-gradient(circle at top left, #ffb46a 0, #ff7a1a 40%, #cc4f00 100%);
      color: #140600;
      box-shadow:
        0 0 18px rgba(255, 138, 64, 0.8),
        0 12px 26px rgba(0, 0, 0, 0.7);
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    .maas-btn.primary:hover {
      transform: translateY(-1px);
      box-shadow:
        0 0 22px rgba(255, 138, 64, 0.95),
        0 16px 28px rgba(0, 0, 0, 0.85);
    }

    .maas-pulse {
      animation: maas-pulse 0.3s ease-out;
    }

    @keyframes maas-pulse {
      0% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.03);
      }
      100% {
        transform: scale(1);
      }
    }

    .maas-secondary-row {
      display: flex;
      gap: 10px;
    }

    .maas-btn.secondary {
      flex: 1;
      background: #0a1426;
      border-color: #27324a;
      color: var(--maas-text-soft);
    }

    .maas-btn.secondary:hover {
      background: #121e34;
      border-color: #44537b;
    }

    .maas-btn.pdf {
      align-self: flex-start;
      background: #0b1426;
      border-color: #ff7a1a;
      color: var(--maas-text-main);
      font-size: 0.85rem;
    }

    .maas-btn.pdf:hover {
      background: #151f33;
      box-shadow: 0 0 16px rgba(255, 138, 64, 0.55);
    }

    .pdf-icon {
      position: relative;
      width: 18px;
      height: 20px;
      border-radius: 3px;
      background: #f5f5ff;
      display: inline-flex;
      align-items: flex-end;
      justify-content: center;
      padding: 2px 2px 3px;
      box-shadow: 0 0 6px rgba(0, 0, 0, 0.45);
    }

    .pdf-page {
      position: absolute;
      inset: 0;
      border-radius: 3px;
      background: linear-gradient(to bottom, #ffffff, #f1f1ff);
    }

    .pdf-graph {
      position: relative;
      width: 100%;
      height: 55%;
      border-radius: 2px;
      background: linear-gradient(to top right, #ff7a1a 0, #ffb46a 45%, #42b3ff 100%);
      clip-path: polygon(0% 80%, 20% 55%, 40% 65%, 65% 40%, 80% 55%, 100% 20%, 100% 100%, 0 100%);
    }

    /* ============= GAUGES ============= */
    .maas-gauges {
      display: grid;
      grid-template-columns: minmax(0, 0.9fr) minmax(0, 0.9fr);
      grid-template-rows: auto auto;
      gap: 10px;
    }

    .maas-gauge {
      background: var(--maas-panel-soft);
      border-radius: 10px;
      border: 1px solid #252f4a;
      padding: 8px 10px;
    }

    .maas-gauge-label {
      font-size: 0.75rem;
      color: var(--maas-text-soft);
      margin-bottom: 4px;
    }

    .maas-gauge-bar {
      position: relative;
      height: 7px;
      border-radius: 999px;
      background: #050816;
      overflow: hidden;
    }

    .maas-gauge-fill {
      position: absolute;
      inset: 0;
      width: 50%;
      border-radius: 999px;
    }

    .maas-gauge-fill.skill {
      background: linear-gradient(to right, #1b8af5, #42b3ff);
    }

    .maas-gauge-fill.var {
      background: linear-gradient(to right, #ff7a1a, #ffb46a);
    }

    .maas-gauge-value {
      margin-top: 4px;
      font-variant-numeric: tabular-nums;
      font-size: 0.9rem;
    }

    .maas-gauge-narrative {
      grid-column: 1 / span 2;
    }

    .maas-gauge-narrative-text {
      font-size: 0.78rem;
      line-height: 1.3;
      color: var(--maas-text-main);
    }

    .maas-footnote {
      font-size: 0.7rem;
      color: var(--maas-text-soft);
      margin-top: 2px;
    }

    /* ============= RESPONSIVE ============= */
    @media (max-width: 900px) {
      .maas-console {
        grid-template-columns: 1fr;
      }
      .maas-sliders {
        overflow-x: auto;
        padding-bottom: 12px;
      }
      .maas-sliders {
        grid-auto-flow: column;
        grid-auto-columns: 110px;
      }
    }

    @media (max-width: 600px) {
      #maas-console-root {
        padding: 16px 8px;
      }
      .maas-console {
        padding: 16px;
      }
      .maas-secondary-row {
        flex-direction: column;
      }
      .maas-gauges {
        grid-template-columns: 1fr;
      }
      .maas-gauge-narrative {
        grid-column: auto;
      }
    }
  </style>
</head>
<body>
  <div id="maas-console-root">
    <div class="maas-console">
      <!-- LEFT COLUMN – SLIDERS + ENGINE CORE -->
      <div class="maas-left">
        <div class="maas-sliders-header">MAAS Engine Sliders</div>
        <div class="maas-sliders">
          <!-- Each slider: label, value readout, input -->
          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">QS</span>
              <span class="maas-slider-value" data-output-for="qs">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="qs" class="maas-slider-input">
            <div class="maas-slider-sub">Quality of Signal</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">OI</span>
              <span class="maas-slider-value" data-output-for="oi">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="oi" class="maas-slider-input">
            <div class="maas-slider-sub">Outside Interference</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">DI</span>
              <span class="maas-slider-value" data-output-for="di">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="di" class="maas-slider-input">
            <div class="maas-slider-sub">Directional Instability</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">IC</span>
              <span class="maas-slider-value" data-output-for="ic">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="ic" class="maas-slider-input">
            <div class="maas-slider-sub">Internal Clarity</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">CC</span>
              <span class="maas-slider-value" data-output-for="cc">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="cc" class="maas-slider-input">
            <div class="maas-slider-sub">Channel Capacity</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">MF</span>
              <span class="maas-slider-value" data-output-for="mf">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="mf" class="maas-slider-input">
            <div class="maas-slider-sub">Mental Flexibility</div>
          </div>

          <div class="maas-slider">
            <div class="maas-slider-top">
              <span class="maas-slider-label">EP</span>
              <span class="maas-slider-value" data-output-for="ep">50</span>
            </div>
            <input type="range" min="0" max="100" value="50" id="ep" class="maas-slider-input">
            <div class="maas-slider-sub">Execution Pace</div>
          </div>
        </div>

        <!-- Engine core visual -->
        <div class="maas-core">
          <div class="maas-core-ring maas-core-ring-outer"></div>
          <div class="maas-core-ring maas-core-ring-middle"></div>
          <div class="maas-core-ring maas-core-ring-inner"></div>
          <div class="maas-core-center"></div>
          <div class="maas-core-label">ENGINE CORE</div>
        </div>
      </div>

      <!-- RIGHT COLUMN – GRAPH + CONTROLS -->
      <div class="maas-right">
        <!-- Graph + current readouts -->
        <div class="maas-graph-panel">
          <div class="maas-graph-header">
            <span>Skill% vs Variance%</span>
            <div class="maas-graph-legend">
              <span class="legend-skill">Skill%</span>
              <span class="legend-var">Variance%</span>
            </div>
          </div>
          <canvas id="maas-graph" width="420" height="180"></canvas>

          <div class="maas-current-metrics">
            <div class="maas-metric">
              <div class="maas-metric-label">Current Skill%</div>
              <div class="maas-metric-value" id="current-skill">50</div>
            </div>
            <div class="maas-metric">
              <div class="maas-metric-label">Current Variance%</div>
              <div class="maas-metric-value" id="current-var">50</div>
            </div>
            <div class="maas-metric">
              <div class="maas-metric-label">Narrative Guidance</div>
              <div class="maas-metric-value small" id="current-guidance">
                Stable – push with structure
              </div>
            </div>
          </div>
        </div>

        <!-- Control buttons -->
        <div class="maas-controls">
          <button class="maas-btn primary" id="run-session">
            Run Session / Compute MAAS
          </button>

          <div class="maas-secondary-row">
            <button class="maas-btn secondary" id="deep-session">
              Deep Session Lab
            </button>
            <button class="maas-btn secondary" id="quick-session">
              5-Minute Awareness Task
            </button>
          </div>

          <!-- Global gauges -->
          <div class="maas-gauges">
            <div class="maas-gauge">
              <div class="maas-gauge-label">Global Skill%</div>
              <div class="maas-gauge-bar">
                <div class="maas-gauge-fill skill" id="global-skill-bar"></div>
              </div>
              <div class="maas-gauge-value" id="global-skill-value">50</div>
            </div>

            <div class="maas-gauge">
              <div class="maas-gauge-label">Global Variance%</div>
              <div class="maas-gauge-bar">
                <div class="maas-gauge-fill var" id="global-var-bar"></div>
              </div>
              <div class="maas-gauge-value" id="global-var-value">50</div>
            </div>

            <div class="maas-gauge maas-gauge-narrative">
              <div class="maas-gauge-label">Narrative Guidance Feed</div>
              <div class="maas-gauge-narrative-text" id="global-guidance">
                Stable – push with structure. You have room to add challenge with clear rails.
              </div>
            </div>
          </div>

          <!-- PDF button (stubbed) -->
          <button class="maas-btn pdf" id="pdf-report">
            <span class="pdf-icon">
              <span class="pdf-page"></span>
              <span class="pdf-graph"></span>
            </span>
            <span>Generate PDF Report</span>
          </button>

          <div class="maas-footnote">
            MAAS is an HR-safe awareness console. Outputs describe patterns and options,
            not diagnoses or prescriptions.
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    (function () {
      const maxPoints = 24;

      const historySkill = [];
      const historyVar = [];

      let totalSkill = 0;
      let totalVar = 0;
      let sessions = 0;

      const $ = (sel) => document.querySelector(sel);
      const $$ = (sel) => Array.from(document.querySelectorAll(sel));

      const sliderIds = ["qs", "oi", "di", "ic", "cc", "mf", "ep"];

      const currentSkillEl = $("#current-skill");
      const currentVarEl = $("#current-var");
      const currentGuidanceEl = $("#current-guidance");

      const globalSkillBar = $("#global-skill-bar");
      const globalVarBar = $("#global-var-bar");
      const globalSkillValue = $("#global-skill-value");
      const globalVarValue = $("#global-var-value");
      const globalGuidanceEl = $("#global-guidance");

      const canvas = document.getElementById("maas-graph");
      const ctx = canvas.getContext("2d");

      const runBtn = document.getElementById("run-session");
      const deepBtn = document.getElementById("deep-session");
      const quickBtn = document.getElementById("quick-session");
      const pdfBtn = document.getElementById("pdf-report");

      function getSliderValues() {
        const values = {};
        sliderIds.forEach((id) => {
          const v = Number(document.getElementById(id).value);
          values[id] = v;
        });
        return values;
      }

      function updateSliderReadout(id, value) {
        const out = document.querySelector(
          '.maas-slider-value[data-output-for="' + id + '"]'
        );
        if (out) out.textContent = value;
      }

      $$(".maas-slider-input").forEach((input) => {
        input.addEventListener("input", (e) => {
          updateSliderReadout(e.target.id, e.target.value);
        });
      });

      function round1(x) {
        return Math.round(x * 10) / 10;
      }

      function computeMetrics() {
        const v = getSliderValues();

        const skillInputs = [v.qs, v.ic, v.cc, v.mf, v.ep];
        const varBaseInputs = [v.oi, v.di];

        const skillRaw =
          skillInputs.reduce((sum, x) => sum + x, 0) / skillInputs.length;

        const varBase =
          varBaseInputs.reduce((sum, x) => sum + x, 0) / varBaseInputs.length;

        const all = Object.values(v);
        const avgAll = all.reduce((a, b) => a + b, 0) / all.length;
        const spread =
          all.reduce((acc, x) => acc + Math.abs(x - avgAll), 0) / all.length;

        let varianceRaw = varBase + spread * 0.35;

        const skill = Math.max(0, Math.min(100, skillRaw));
        const variance = Math.max(0, Math.min(100, varianceRaw));

        return { skill: round1(skill), variance: round1(variance) };
      }

      function guidanceFor(skill, variance) {
        if (skill >= 70 && variance <= 30) {
          return "Stable – push with structure.";
        }
        if (skill >= 60 && variance > 30 && variance <= 55) {
          return "Capable but noisy – simplify lanes, protect focus.";
        }
        if (skill >= 40 && variance <= 40) {
          return "Building – keep reps light, prioritize repeatable wins.";
        }
        if (skill < 40 && variance <= 50) {
          return "Underpowered – lower expectations, rebuild from core basics.";
        }
        if (variance > 60 && skill >= 50) {
          return "Spiky performance – add buffers, slow the tempo.";
        }
        if (variance > 60 && skill < 50) {
          return "High volatility – shrink scope, stabilize one decision at a time.";
        }
        return "Mixed signal – log the session and adjust one variable, not all.";
      }

      function globalGuidance(skillGlobal, varGlobal) {
        if (!sessions || isNaN(skillGlobal) || isNaN(varGlobal)) {
          return "Run a few sessions to train the MAAS narrative guidance.";
        }
        if (skillGlobal >= 70 && varGlobal <= 30) {
          return "Overall stable pattern. You can safely add challenge with clear rails and recovery blocks.";
        }
        if (skillGlobal >= 60 && varGlobal <= 45) {
          return "Strong base with moderate noise. Tighten communication and scheduling to reduce friction.";
        }
        if (skillGlobal < 50 && varGlobal <= 50) {
          return "System is underpowered. Focus on sleep, food, and one high-impact routine before scaling.";
        }
        if (varGlobal > 60) {
          return "Chronic variance detected. Protect margins, reduce surprises, and create consistent start/stop rituals.";
        }
        return "Mixed global pattern. Keep logging sessions and watch for repeat situations that raise variance.";
      }

      function drawGraph() {
        const w = canvas.width;
        const h = canvas.height;

        ctx.clearRect(0, 0, w, h);

        const grad = ctx.createLinearGradient(0, 0, 0, h);
        grad.addColorStop(0, "#12172a");
        grad.addColorStop(1, "#050816");
        ctx.fillStyle = grad;
        ctx.fillRect(0, 0, w, h);

        ctx.strokeStyle = "#1f2941";
        ctx.lineWidth = 1;
        const horizontalLines = 5;
        for (let i = 0; i <= horizontalLines; i++) {
          const y = (h / horizontalLines) * i;
          ctx.beginPath();
          ctx.moveTo(30, y + 0.5);
          ctx.lineTo(w - 10, y + 0.5);
          ctx.stroke();
        }

        ctx.strokeStyle = "#2b3552";
        ctx.lineWidth = 1.2;
        ctx.beginPath();
        ctx.moveTo(30.5, 5);
        ctx.lineTo(30.5, h - 18);
        ctx.lineTo(w - 10, h - 18);
        ctx.stroke();

        function drawSeries(data, color) {
          if (!data.length) return;

          const maxCount = Math.max(historySkill.length, historyVar.length) || 1;
          const xStart = 30;
          const xEnd = w - 10;
          const yTop = 10;
          const yBottom = h - 20;
          const xStep =
            maxCount === 1 ? 0 : (xEnd - xStart) / (maxCount - 1);

          ctx.beginPath();
          data.forEach((val, i) => {
            const x = xStart + xStep * i;
            const y = yBottom - ((val / 100) * (yBottom - yTop));
            if (i === 0) ctx.moveTo(x, y);
            else ctx.lineTo(x, y);
          });

          ctx.strokeStyle = color;
          ctx.lineWidth = 2;
          ctx.stroke();
        }

        drawSeries(historySkill, "#42b3ff");
        drawSeries(historyVar, "#ff9a3b");
      }

      function updateUI(skill, variance) {
        currentSkillEl.textContent = skill.toFixed(1);
        currentVarEl.textContent = variance.toFixed(1);
        currentGuidanceEl.textContent = guidanceFor(skill, variance);

        historySkill.push(skill);
        historyVar.push(variance);
        if (historySkill.length > maxPoints) historySkill.shift();
        if (historyVar.length > maxPoints) historyVar.shift();

        sessions++;
        totalSkill += skill;
        totalVar += variance;

        const avgSkill = totalSkill / sessions;
        const avgVar = totalVar / sessions;

        globalSkillBar.style.width = Math.max(4, avgSkill) + "%";
        globalVarBar.style.width = Math.max(4, avgVar) + "%";
        globalSkillValue.textContent = avgSkill.toFixed(1);
        globalVarValue.textContent = avgVar.toFixed(1);
        globalGuidanceEl.textContent = globalGuidance(avgSkill, avgVar);

        drawGraph();
      }

      runBtn.addEventListener("click", () => {
        const { skill, variance } = computeMetrics();
        updateUI(skill, variance);

        runBtn.classList.add("maas-pulse");
        setTimeout(() => runBtn.classList.remove("maas-pulse"), 300);
      });

      deepBtn.addEventListener("click", () => {
        alert(
          "Deep Session Lab:\n\nUse this mode to log a longer session with notes.\nIn the full MAAS build, this button will open a structured form with segments, timestamps, and narrative fields."
        );
      });

      quickBtn.addEventListener("click", () => {
        alert(
          "5-Minute Awareness Task:\n\nSet a timer for 5 minutes.\nObserve your current environment, body, and task list.\nAdjust one slider that feels most accurate right now, then hit 'Run Session / Compute MAAS'."
        );
      });

      pdfBtn.addEventListener("click", () => {
        alert(
          "Generate PDF Report:\n\nIn the production version, this will create a brief HR-safe memo summarizing Skill%, Variance%, and guidance for the session window.\nFor now, treat this as a placeholder."
        );
      });

      (function initial() {
        const { skill, variance } = computeMetrics();
        updateUI(skill, variance);
      })();
    })();
  </script>
</body>
</html>
