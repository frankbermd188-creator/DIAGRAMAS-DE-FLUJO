# DIAGRAMAS-DE-FLUJO
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Diagramas de Flujo — Inspección No Destructiva</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=IBM+Plex+Sans:wght@300;400;600;700&family=IBM+Plex+Mono:wght@400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0c10;
    --surface: #111318;
    --border: #1e2330;
    --vt: #00c9a7;
    --pt: #f97316;
    --mt: #a78bfa;
    --ut: #38bdf8;
    --text: #e2e8f0;
    --muted: #64748b;
    --white: #ffffff;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'IBM Plex Sans', sans-serif;
    min-height: 100vh;
  }

  /* HEADER */
  header {
    text-align: center;
    padding: 60px 20px 40px;
    border-bottom: 1px solid var(--border);
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse 80% 60% at 50% 0%, rgba(0,201,167,0.07) 0%, transparent 70%);
    pointer-events: none;
  }
  .header-tag {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--vt);
    text-transform: uppercase;
    margin-bottom: 16px;
  }
  h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(36px, 7vw, 72px);
    letter-spacing: 4px;
    line-height: 1;
    color: var(--white);
    margin-bottom: 12px;
  }
  .subtitle {
    font-size: 14px;
    color: var(--muted);
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  /* NAV TABS */
  .tabs {
    display: flex;
    justify-content: center;
    gap: 0;
    padding: 40px 20px 0;
    flex-wrap: wrap;
  }
  .tab-btn {
    padding: 14px 32px;
    background: transparent;
    border: 1px solid var(--border);
    border-bottom: none;
    color: var(--muted);
    font-family: 'IBM Plex Mono', monospace;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 2px;
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    margin-bottom: -1px;
  }
  .tab-btn:hover { color: var(--white); }
  .tab-btn.active-vt  { color: var(--vt);  border-color: var(--vt);  background: rgba(0,201,167,0.06); }
  .tab-btn.active-pt  { color: var(--pt);  border-color: var(--pt);  background: rgba(249,115,22,0.06); }
  .tab-btn.active-mt  { color: var(--mt);  border-color: var(--mt);  background: rgba(167,139,250,0.06); }
  .tab-btn.active-ut  { color: var(--ut);  border-color: var(--ut);  background: rgba(56,189,248,0.06); }

  /* PANEL */
  .panel {
    display: none;
    max-width: 960px;
    margin: 0 auto;
    padding: 50px 20px 80px;
  }
  .panel.visible { display: block; }

  .panel-header {
    text-align: center;
    margin-bottom: 50px;
  }
  .technique-code {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 96px;
    line-height: 1;
    letter-spacing: 8px;
  }
  .technique-name {
    font-size: 18px;
    font-weight: 300;
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-top: 8px;
  }
  .technique-desc {
    font-size: 13px;
    color: var(--muted);
    max-width: 560px;
    margin: 16px auto 0;
    line-height: 1.7;
  }

  /* FLOWCHART */
  .flowchart {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0;
  }

  .node {
    width: 100%;
    max-width: 560px;
    padding: 18px 28px;
    border-radius: 4px;
    text-align: center;
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.5px;
    line-height: 1.5;
    position: relative;
    z-index: 1;
    transition: transform 0.15s;
  }
  .node:hover { transform: scale(1.015); }

  .node-start, .node-end {
    border-radius: 50px;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 13px;
    letter-spacing: 3px;
    text-transform: uppercase;
    padding: 16px 40px;
    width: auto;
    min-width: 200px;
  }
  .node-process {
    border-radius: 4px;
  }
  .node-decision {
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    padding: 34px 60px;
    font-size: 13px;
    font-weight: 700;
    width: 100%;
    max-width: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 110px;
  }
  .node-report {
    border-radius: 4px 4px 0 0;
    border-bottom: 3px solid;
  }

  /* ARROWS */
  .arrow {
    width: 2px;
    height: 32px;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .arrow::before {
    content: '';
    width: 2px;
    flex: 1;
  }
  .arrow::after {
    content: '▼';
    font-size: 12px;
    line-height: 1;
  }
  .arrow-label {
    position: absolute;
    right: calc(50% + 10px);
    top: 50%;
    transform: translateY(-50%);
    font-size: 11px;
    font-family: 'IBM Plex Mono', monospace;
    letter-spacing: 1px;
    white-space: nowrap;
  }
  .arrow-label-right {
    position: absolute;
    left: calc(50% + 10px);
    top: 50%;
    transform: translateY(-50%);
    font-size: 11px;
    font-family: 'IBM Plex Mono', monospace;
    letter-spacing: 1px;
    white-space: nowrap;
  }

  /* COLOR VARIANTS */
  .vt .node-start, .vt .node-end   { background: var(--vt); color: #003d32; border: 2px solid var(--vt); }
  .vt .node-process                { background: rgba(0,201,167,0.08); border: 1.5px solid rgba(0,201,167,0.3); color: var(--text); }
  .vt .node-decision               { background: rgba(0,201,167,0.12); border: none; color: var(--vt); outline: 2px solid rgba(0,201,167,0.4); outline-offset: -2px; }
  .vt .node-report                 { background: rgba(0,201,167,0.15); border: 1.5px solid var(--vt); border-bottom-color: var(--vt); color: var(--text); }
  .vt .arrow::before               { background: var(--vt); }
  .vt .arrow::after                { color: var(--vt); }
  .vt .arrow-label, .vt .arrow-label-right { color: var(--vt); }
  .vt .technique-code              { color: var(--vt); }

  .pt .node-start, .pt .node-end   { background: var(--pt); color: #3d1300; border: 2px solid var(--pt); }
  .pt .node-process                { background: rgba(249,115,22,0.08); border: 1.5px solid rgba(249,115,22,0.3); color: var(--text); }
  .pt .node-decision               { background: rgba(249,115,22,0.12); border: none; color: var(--pt); outline: 2px solid rgba(249,115,22,0.4); outline-offset: -2px; }
  .pt .node-report                 { background: rgba(249,115,22,0.15); border: 1.5px solid var(--pt); border-bottom-color: var(--pt); color: var(--text); }
  .pt .arrow::before               { background: var(--pt); }
  .pt .arrow::after                { color: var(--pt); }
  .pt .arrow-label, .pt .arrow-label-right { color: var(--pt); }
  .pt .technique-code              { color: var(--pt); }

  .mt .node-start, .mt .node-end   { background: var(--mt); color: #1e0050; border: 2px solid var(--mt); }
  .mt .node-process                { background: rgba(167,139,250,0.08); border: 1.5px solid rgba(167,139,250,0.3); color: var(--text); }
  .mt .node-decision               { background: rgba(167,139,250,0.12); border: none; color: var(--mt); outline: 2px solid rgba(167,139,250,0.4); outline-offset: -2px; }
  .mt .node-report                 { background: rgba(167,139,250,0.15); border: 1.5px solid var(--mt); border-bottom-color: var(--mt); color: var(--text); }
  .mt .arrow::before               { background: var(--mt); }
  .mt .arrow::after                { color: var(--mt); }
  .mt .arrow-label, .mt .arrow-label-right { color: var(--mt); }
  .mt .technique-code              { color: var(--mt); }

  .ut .node-start, .ut .node-end   { background: var(--ut); color: #001e3d; border: 2px solid var(--ut); }
  .ut .node-process                { background: rgba(56,189,248,0.08); border: 1.5px solid rgba(56,189,248,0.3); color: var(--text); }
  .ut .node-decision               { background: rgba(56,189,248,0.12); border: none; color: var(--ut); outline: 2px solid rgba(56,189,248,0.4); outline-offset: -2px; }
  .ut .node-report                 { background: rgba(56,189,248,0.15); border: 1.5px solid var(--ut); border-bottom-color: var(--ut); color: var(--text); }
  .ut .arrow::before               { background: var(--ut); }
  .ut .arrow::after                { color: var(--ut); }
  .ut .arrow-label, .ut .arrow-label-right { color: var(--ut); }
  .ut .technique-code              { color: var(--ut); }

  .panel-divider {
    width: 100%;
    max-width: 560px;
    height: 1px;
    background: var(--border);
    margin: 50px 0;
    align-self: center;
  }

  /* LEGEND */
  .legend {
    display: flex;
    justify-content: center;
    gap: 32px;
    flex-wrap: wrap;
    margin-top: 50px;
    padding: 24px;
    border: 1px solid var(--border);
    border-radius: 8px;
    background: var(--surface);
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 12px;
    color: var(--muted);
    font-family: 'IBM Plex Mono', monospace;
  }
  .legend-oval  { width:32px; height:18px; border-radius:50px; background:rgba(255,255,255,0.12); border:1.5px solid rgba(255,255,255,0.3); }
  .legend-rect  { width:32px; height:18px; border-radius:3px; background:rgba(255,255,255,0.06); border:1.5px solid rgba(255,255,255,0.2); }
  .legend-diam  { width:24px; height:24px; clip-path:polygon(50% 0%,100% 50%,50% 100%,0% 50%); background:rgba(255,255,255,0.1); }

  /* FOOTER */
  footer {
    text-align: center;
    padding: 32px;
    border-top: 1px solid var(--border);
    font-size: 11px;
    color: var(--muted);
    font-family: 'IBM Plex Mono', monospace;
    letter-spacing: 1px;
  }
</style>
</head>
<body>

<header>
 
  <h1>Diagramas de Flujo</h1>
  <div class="subtitle">Técnicas de Inspección · Procedimientos Estándar</div>
  <div class="subtitle">ALUMNOS: BERMUDEZ CRUZ DAVID ALONSO · HERNANDEZ RAMIREZ DIEGO IVAN · SANCHEZ HERNANDEZ AXEL OMAR </div>

</header>

<!-- TABS -->
<div class="tabs">
  <button class="tab-btn active-vt" onclick="show('vt',this)">VT — Visual</button>
  <button class="tab-btn" onclick="show('pt',this)">PT — Líquidos Penetrantes</button>
  <button class="tab-btn" onclick="show('mt',this)">MT — Partículas Magnéticas</button>
  <button class="tab-btn" onclick="show('ut',this)">UT — Ultrasonido</button>
</div>

<!-- ══════════════════════════════════════════════════════════ VT ══ -->
<div id="panel-vt" class="panel visible">
  <div class="panel-header vt">
    <div class="technique-code">VT</div>
    <div class="technique-name">Inspección Visual</div>
    <div class="technique-desc">Método más básico y económico. Detecta discontinuidades superficiales a simple vista o con ayuda óptica. Es el primer paso antes de cualquier otra END.</div>
  </div>
  <div class="flowchart vt">

    <div class="node node-start">INICIO</div>
    <div class="arrow"></div>

    <div class="node node-process">1. Revisión de planos, especificaciones y normas aplicables<br><small style="font-weight:300;font-size:12px">(ASME, AWS, API, ISO)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">2. Selección del tipo de inspección<br><small style="font-weight:300;font-size:12px">Directa · Remota · Translucida</small></div>
    <div class="arrow"></div>

    <div class="node node-process">3. Preparación y limpieza de la superficie<br><small style="font-weight:300;font-size:12px">Eliminar aceite, polvo, óxido, recubrimientos</small></div>
    <div class="arrow"></div>

    <div class="node node-process">4. Verificación de condiciones de iluminación<br><small style="font-weight:300;font-size:12px">Mín. 500 lux (general) · 1000 lux (crítico)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">5. Selección y calibración de herramientas ópticas<br><small style="font-weight:300;font-size:12px">Lupa · Boroscopio · Espejo · Cámara · Regla</small></div>
    <div class="arrow"></div>

    <div class="node node-process">6. Inspección sistemática de la zona de interés<br><small style="font-weight:300;font-size:12px">Recorrer en zigzag o cuadrícula según área</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Se detectan<br>discontinuidades?</div>

    <div style="display:flex; width:100%; max-width:560px; justify-content:space-between; align-items:flex-start; position:relative; height:60px;">
      <div style="width:2px; height:60px; background:var(--vt); margin-left:calc(50% - 1px); position:absolute;"></div>
      <!-- YES branch arrow left -->
      <div style="position:absolute; left:0; top:0; width:50%; height:60px; border-bottom:2px solid var(--vt); border-left:2px solid var(--vt);"></div>
      <!-- NO branch arrow right -->
      <div style="position:absolute; right:0; top:0; width:50%; height:60px; border-bottom:2px solid var(--vt); border-right:2px solid var(--vt);"></div>
      <span style="position:absolute;left:20px;top:36px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--vt)">SÍ</span>
      <span style="position:absolute;right:20px;top:36px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--vt)">NO</span>
    </div>

    <div style="display:flex; width:100%; max-width:680px; gap:40px; justify-content:center;">
      <div style="flex:1; display:flex; flex-direction:column; align-items:center; gap:0;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">7A. Caracterización de la discontinuidad<br><small style="font-weight:300;font-size:12px">Tipo · Dimensión · Ubicación · Orientación</small></div>
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">7B. Evaluación según criterio de aceptación</div>
        <div class="arrow"></div>
        <div class="node node-decision" style="max-width:220px; min-height:90px; padding:24px 30px; font-size:12px">¿Acepta<br>criterio?</div>
        <div class="arrow" style="height:20px"></div>
        <div class="node node-process" style="width:100%;font-size:13px">7C. Documentar rechazo<br>→ Reparación / Descarte</div>
      </div>
      <div style="flex:1; display:flex; flex-direction:column; align-items:center; gap:0;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">8. Superficie sin indicaciones relevantes</div>
        <div class="arrow" style="height:124px"></div>
      </div>
    </div>

    <div class="arrow"></div>
    <div class="node node-report">9. Elaboración del Reporte de Inspección<br><small style="font-weight:300;font-size:12px">Inspector · Fecha · Norma · Resultados · Fotografías</small></div>
    <div class="arrow"></div>
    <div class="node node-start">FIN</div>

  </div>
  <div class="legend">
    <div class="legend-item"><div class="legend-oval"></div>INICIO / FIN</div>
    <div class="legend-item"><div class="legend-rect"></div>PROCESO</div>
    <div class="legend-item"><div class="legend-diam"></div>DECISIÓN</div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════ PT ══ -->
<div id="panel-pt" class="panel">
  <div class="panel-header pt">
    <div class="technique-code">PT</div>
    <div class="technique-name">Líquidos Penetrantes</div>
    <div class="technique-desc">Detecta discontinuidades abiertas a la superficie en materiales no porosos. Basado en la acción capilar del líquido que penetra en grietas y se revela con un revelador.</div>
  </div>
  <div class="flowchart pt">

    <div class="node node-start">INICIO</div>
    <div class="arrow"></div>

    <div class="node node-process">1. Revisión de especificaciones y norma<br><small style="font-weight:300;font-size:12px">ASTM E165 · ASME Sec. V · EN ISO 3452</small></div>
    <div class="arrow"></div>

    <div class="node node-process">2. Selección del sistema penetrante<br><small style="font-weight:300;font-size:12px">Tipo I (Fluorescente) · Tipo II (Visible) / Método A, B, C, D</small></div>
    <div class="arrow"></div>

    <div class="node node-process">3. Pre-limpieza de la superficie<br><small style="font-weight:300;font-size:12px">Desengrasado · Decapado · Secado completo</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Superficie seca<br>y sin contaminantes?</div>
    <div class="arrow"><span class="arrow-label">NO → Relimpiar</span></div>

    <div class="node node-process">4. Aplicación del líquido penetrante<br><small style="font-weight:300;font-size:12px">Aspersión · Brocha · Inmersión</small></div>
    <div class="arrow"></div>

    <div class="node node-process">5. Tiempo de penetración (dwell time)<br><small style="font-weight:300;font-size:12px">5–60 min según norma, material y temperatura</small></div>
    <div class="arrow"></div>

    <div class="node node-process">6. Eliminación del exceso de penetrante<br><small style="font-weight:300;font-size:12px">Agua · Solvente · Emulsificante (según método)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">7. Secado de la superficie</div>
    <div class="arrow"></div>

    <div class="node node-process">8. Aplicación del revelador<br><small style="font-weight:300;font-size:12px">Húmedo · Seco · No acuoso · Película plástica</small></div>
    <div class="arrow"></div>

    <div class="node node-process">9. Tiempo de revelado<br><small style="font-weight:300;font-size:12px">≥ 10 min · no exceder 2× tiempo de penetración</small></div>
    <div class="arrow"></div>

    <div class="node node-process">10. Interpretación de indicaciones<br><small style="font-weight:300;font-size:12px">Luz blanca (≥1000 lux) o UV (≥1000 µW/cm²)</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Se detectan<br>indicaciones?</div>

    <div style="display:flex; width:100%; max-width:560px; justify-content:space-between; align-items:flex-start; position:relative; height:50px;">
      <div style="position:absolute; left:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--pt); border-left:2px solid var(--pt);"></div>
      <div style="position:absolute; right:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--pt); border-right:2px solid var(--pt);"></div>
      <span style="position:absolute;left:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--pt)">SÍ</span>
      <span style="position:absolute;right:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--pt)">NO</span>
    </div>

    <div style="display:flex; width:100%; max-width:680px; gap:40px; justify-content:center;">
      <div style="flex:1; display:flex; flex-direction:column; align-items:center; gap:0;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">11A. Evaluación de indicaciones<br><small style="font-weight:300;font-size:12px">Lineal · Redondeada · Falsa indicación</small></div>
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">Comparar con criterio de aceptación</div>
      </div>
      <div style="flex:1; display:flex; flex-direction:column; align-items:center; gap:0;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">11B. Sin indicaciones relevantes</div>
        <div class="arrow" style="height:76px"></div>
      </div>
    </div>

    <div class="arrow"></div>
    <div class="node node-process">12. Post-limpieza de la pieza inspeccionada</div>
    <div class="arrow"></div>
    <div class="node node-report">13. Elaboración del Reporte de Inspección<br><small style="font-weight:300;font-size:12px">Procedimiento · Materiales usados · Indicaciones · Aceptado/Rechazado</small></div>
    <div class="arrow"></div>
    <div class="node node-start">FIN</div>

  </div>
  <div class="legend">
    <div class="legend-item"><div class="legend-oval"></div>INICIO / FIN</div>
    <div class="legend-item"><div class="legend-rect"></div>PROCESO</div>
    <div class="legend-item"><div class="legend-diam"></div>DECISIÓN</div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════ MT ══ -->
<div id="panel-mt" class="panel">
  <div class="panel-header mt">
    <div class="technique-code">MT</div>
    <div class="technique-name">Partículas Magnéticas</div>
    <div class="technique-desc">Detecta discontinuidades superficiales y sub-superficiales en materiales ferromagnéticos. Los campos de fuga atraen partículas formando indicaciones visibles.</div>
  </div>
  <div class="flowchart mt">

    <div class="node node-start">INICIO</div>
    <div class="arrow"></div>

    <div class="node node-process">1. Verificación de material ferromagnético<br><small style="font-weight:300;font-size:12px">Acero carbono · Fundición · Acero de baja aleación</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Material es<br>ferromagnético?</div>
    <div class="arrow"><span class="arrow-label">NO → Usar PT o UT</span></div>

    <div class="node node-process">2. Revisión de norma aplicable<br><small style="font-weight:300;font-size:12px">ASTM E709 · ASME Sec. V Art. 7 · EN ISO 9934</small></div>
    <div class="arrow"></div>

    <div class="node node-process">3. Pre-limpieza de la superficie<br><small style="font-weight:300;font-size:12px">Remover escamas, óxido, pintura gruesa</small></div>
    <div class="arrow"></div>

    <div class="node node-process">4. Selección del método de magnetización<br><small style="font-weight:300;font-size:12px">Yugo · Bobina · Puntas de contacto · Conductor central</small></div>
    <div class="arrow"></div>

    <div class="node node-process">5. Selección del tipo de partículas<br><small style="font-weight:300;font-size:12px">Secas (coloreadas) · Húmedas (suspensión) · Fluorescentes</small></div>
    <div class="arrow"></div>

    <div class="node node-process">6. Verificación de campo magnético (gauss/Tesla)<br><small style="font-weight:300;font-size:12px">Gauss-metro · Indicadores de campo tipo Castrol</small></div>
    <div class="arrow"></div>

    <div class="node node-process">7. Magnetización de la pieza<br><small style="font-weight:300;font-size:12px">Corriente alterna (CA) o corriente directa (CD)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">8. Aplicación de partículas durante o después de la magnetización<br><small style="font-weight:300;font-size:12px">Técnica continua (CA) · Técnica residual (CD)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">9. Inspección bajo luz adecuada<br><small style="font-weight:300;font-size:12px">Luz blanca ≥1000 lux · UV ≥1000 µW/cm² (fluorescente)</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Existen indicaciones<br>relevantes?</div>

    <div style="display:flex; width:100%; max-width:560px; justify-content:space-between; align-items:flex-start; position:relative; height:50px;">
      <div style="position:absolute; left:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--mt); border-left:2px solid var(--mt);"></div>
      <div style="position:absolute; right:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--mt); border-right:2px solid var(--mt);"></div>
      <span style="position:absolute;left:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--mt)">SÍ</span>
      <span style="position:absolute;right:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--mt)">NO</span>
    </div>

    <div style="display:flex; width:100%; max-width:680px; gap:40px; justify-content:center;">
      <div style="flex:1; display:flex; flex-direction:column; align-items:center;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">10A. Clasificar indicación<br><small style="font-weight:300;font-size:12px">Superficial · Sub-superficial · Falsa</small></div>
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">Evaluar según criterio de aceptación normativo</div>
      </div>
      <div style="flex:1; display:flex; flex-direction:column; align-items:center;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">10B. Pieza sin indicaciones</div>
        <div class="arrow" style="height:76px"></div>
      </div>
    </div>

    <div class="arrow"></div>
    <div class="node node-process">11. Desmagnetización de la pieza<br><small style="font-weight:300;font-size:12px">Remanencia &lt; 2 Gauss (si especificado)</small></div>
    <div class="arrow"></div>
    <div class="node node-process">12. Post-limpieza y retiro de partículas</div>
    <div class="arrow"></div>
    <div class="node node-report">13. Elaboración del Reporte de Inspección<br><small style="font-weight:300;font-size:12px">Equipo · Campo aplicado · Indicaciones · Fotografías · Aceptado/Rechazado</small></div>
    <div class="arrow"></div>
    <div class="node node-start">FIN</div>

  </div>
  <div class="legend">
    <div class="legend-item"><div class="legend-oval"></div>INICIO / FIN</div>
    <div class="legend-item"><div class="legend-rect"></div>PROCESO</div>
    <div class="legend-item"><div class="legend-diam"></div>DECISIÓN</div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════ UT ══ -->
<div id="panel-ut" class="panel">
  <div class="panel-header ut">
    <div class="technique-code">UT</div>
    <div class="technique-name">Ultrasonido</div>
    <div class="technique-desc">Detecta discontinuidades internas y superficiales mediante ondas sonoras de alta frecuencia. Permite medir espesores y localizar defectos en profundidad con alta precisión.</div>
  </div>
  <div class="flowchart ut">

    <div class="node node-start">INICIO</div>
    <div class="arrow"></div>

    <div class="node node-process">1. Revisión de especificaciones y norma aplicable<br><small style="font-weight:300;font-size:12px">ASTM E317 · ASME Sec. V Art. 4/5 · EN ISO 16810</small></div>
    <div class="arrow"></div>

    <div class="node node-process">2. Selección del equipo y transductor<br><small style="font-weight:300;font-size:12px">Haz recto · Haz angular · Doble cristal · Inmersión · Phased Array</small></div>
    <div class="arrow"></div>

    <div class="node node-process">3. Selección del acoplante<br><small style="font-weight:300;font-size:12px">Agua · Gel · Aceite · Glicerina (según temperatura y posición)</small></div>
    <div class="arrow"></div>

    <div class="node node-process">4. Preparación de la superficie<br><small style="font-weight:300;font-size:12px">Rugosidad Ra &lt; 6.3 µm · Eliminar óxido o pintura gruesa</small></div>
    <div class="arrow"></div>

    <div class="node node-process">5. Calibración del equipo con bloque de referencia<br><small style="font-weight:300;font-size:12px">Block IIW · Block V1/V2 · Block de área de fondo plano</small></div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Calibración<br>correcta?</div>
    <div class="arrow"><span class="arrow-label">NO → Recalibrar</span></div>

    <div class="node node-process">6. Ajuste de sensibilidad y ganancia (dB)<br><small style="font-weight:300;font-size:12px">DAC · DGS · TCG según norma</small></div>
    <div class="arrow"></div>

    <div class="node node-process">7. Exploración (scanning) sistemática<br><small style="font-weight:300;font-size:12px">Traslación + rotación · Solapamiento ≥ 15% del transductor</small></div>
    <div class="arrow"></div>

    <div class="node node-process">8. Detección de indicaciones en pantalla A-scan / B-scan / C-scan</div>
    <div class="arrow"></div>

    <div class="node node-decision">¿Se detectan<br>ecos o indicaciones?</div>

    <div style="display:flex; width:100%; max-width:560px; justify-content:space-between; align-items:flex-start; position:relative; height:50px;">
      <div style="position:absolute; left:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--ut); border-left:2px solid var(--ut);"></div>
      <div style="position:absolute; right:0; top:0; width:50%; height:50px; border-bottom:2px solid var(--ut); border-right:2px solid var(--ut);"></div>
      <span style="position:absolute;left:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--ut)">SÍ</span>
      <span style="position:absolute;right:20px;top:30px;font-size:11px;font-family:'IBM Plex Mono',monospace;color:var(--ut)">NO</span>
    </div>

    <div style="display:flex; width:100%; max-width:680px; gap:40px; justify-content:center;">
      <div style="flex:1; display:flex; flex-direction:column; align-items:center;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">9A. Caracterización<br><small style="font-weight:300;font-size:12px">Amplitud · Profundidad · Extensión · Tipo</small></div>
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">9B. Comparar con nivel de referencia y criterio de aceptación</div>
        <div class="arrow"></div>
        <div class="node node-decision" style="max-width:220px; min-height:90px; padding:24px 30px; font-size:12px">¿Supera<br>nivel?</div>
        <div class="arrow" style="height:20px"></div>
        <div class="node node-process" style="width:100%;font-size:13px">Rechazar → Reparación o análisis adicional</div>
      </div>
      <div style="flex:1; display:flex; flex-direction:column; align-items:center;">
        <div class="arrow"></div>
        <div class="node node-process" style="width:100%">Sin indicaciones relevantes — Material aceptado</div>
        <div class="arrow" style="height:130px"></div>
      </div>
    </div>

    <div class="arrow"></div>
    <div class="node node-report">10. Elaboración del Reporte de Inspección<br><small style="font-weight:300;font-size:12px">Equipo · Frecuencia · Calibración · Indicaciones · Localización · Resultado</small></div>
    <div class="arrow"></div>
    <div class="node node-start">FIN</div>

  </div>
  <div class="legend">
    <div class="legend-item"><div class="legend-oval"></div>INICIO / FIN</div>
    <div class="legend-item"><div class="legend-rect"></div>PROCESO</div>
    <div class="legend-item"><div class="legend-diam"></div>DECISIÓN</div>
  </div>
</div>

<footer>Inspección No Destructiva · VT · PT · MT · UT · Diagramas de Procedimiento Estándar</footer>

<script>
const colors = { vt:'active-vt', pt:'active-pt', mt:'active-mt', ut:'active-ut' };

function show(id, btn) {
  // hide all panels
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('visible'));
  // deactivate all tabs
  document.querySelectorAll('.tab-btn').forEach(b => {
    b.className = 'tab-btn';
  });
  // show selected
  document.getElementById('panel-' + id).classList.add('visible');
  btn.classList.add(colors[id]);
}
</script>
</body>
</html>
