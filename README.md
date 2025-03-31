<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>App5 Integrada – Retina + OCT</title>
  <style>
    /* ================== ESTILOS GENERALES ================== */
    body {
      margin: 0;
      padding: 0;
      background: #000;
      overflow: hidden;
      font-family: Arial, sans-serif;
      color: white;
    }
    #container {
      position: relative;
      width: 100vw;
      height: 100vh;
      perspective: 1200px;
    }
    /* CONTENEDOR DEL OJO (RETINA CENTRAL) */
    #eye-chamber {
      position: absolute;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .retina-container {
      position: relative;
      width: 80vmin;
      height: 80vmin;
      transform-style: preserve-3d;
      perspective: 1200px;
      border-radius: 50%;
      overflow: hidden;
      transition: transform 0.3s ease;
    }
    .retina-sphere {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: radial-gradient(circle at center, #400000 0%, #300000 40%, #200000 70%, #100000 90%);
      transform: rotateX(20deg) translateZ(50px);
      box-shadow: inset 0 0 150px rgba(200,0,0,0.3),
                  inset 0 0 50px rgba(255,0,0,0.2),
                  0 0 100px rgba(0,0,0,0.9);
      position: relative;
    }
    .retina-texture {
      position: absolute;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at 50% 50%, rgba(255,200,200,0.3) 0%, rgba(255,150,150,0.2) 80%),
                  url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="none" stroke="rgba(255,100,100,0.2)" stroke-width="1"/></svg>');
      background-size: 100%, 20px 20px;
      opacity: 0.9;
      border-radius: 50%;
      mix-blend-mode: multiply;
    }
    .macula {
      position: absolute;
      width: 80px;
      height: 80px;
      background: radial-gradient(circle at center, rgba(255,220,180,0.5) 0%, rgba(255,200,150,0.3) 70%, transparent 100%);
      border-radius: 50%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 2;
    }
    .blood-vessels {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 3;
      pointer-events: none;
    }
    .retina-nerve {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200"><path d="M0,0 Q50,50 100,0 T200,0" stroke="rgba(255,255,255,0.05)" stroke-width="2" fill="none"/></svg>') center/cover;
      pointer-events: none;
      opacity: 0.5;
      z-index: 4;
    }
    .optic-disc {
      position: absolute;
      width: 70px;
      height: 70px;
      background-color: rgba(200,100,100,0.3);
      border-radius: 50%;
      top: 50%;
      left: 25%;
      transform: translate(-50%, -50%);
      box-shadow: inset 0 0 15px rgba(200,50,50,0.4), 0 0 20px rgba(200,50,50,0.3);
      z-index: 5;
    }
    .optic-cup {
      position: absolute;
      width: 30px;
      height: 30px;
      background-color: rgba(200,50,50,0.4);
      border-radius: 50%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      box-shadow: inset 0 0 10px rgba(150,30,30,0.5);
      z-index: 6;
    }
    /* CAMPO DE VISIÓN – Endoiluminador */
    #light-mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.85);
      mask-image: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), transparent var(--light-size, 80px), black calc(var(--light-size, 80px) + 40px));
      -webkit-mask-image: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), transparent var(--light-size, 80px), black calc(var(--light-size, 80px) + 40px));
      transition: all 0.3s ease;
      border-radius: 50%;
      pointer-events: none;
      z-index: 7;
    }
    #light-reflection {
      position: absolute;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), rgba(255,255,220,0.9) calc(var(--light-size, 80px)*0.2), rgba(255,200,150,0.5) calc(var(--light-size, 80px)*0.4), transparent var(--light-size, 80px));
      opacity: 0;
      transition: opacity 0.3s ease;
      border-radius: 50%;
      pointer-events: none;
      z-index: 8;
    }
    /* INSTRUMENTOS QUIRÚRGICOS */
    #vitrectome, #end-grasper, #laser-probe {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      display: none;
    }
    #vitrectome {
      width: 20px;
      height: 100px;
      background: linear-gradient(to bottom, #ccc, #fff);
      border: 1px solid #aaa;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }
    #vitrectome::after {
      content: '';
      position: absolute;
      bottom: -5px;
      left: 50%;
      transform: translateX(-50%);
      width: 12px;
      height: 12px;
      background: radial-gradient(circle, #fff, #ccc);
      border-radius: 50%;
    }
    #end-grasper {
      width: 18px;
      height: 90px;
      background: linear-gradient(to bottom, #ddd, #eee);
      border: 1px solid #bbb;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0,0,0,0.5);
    }
    #laser-probe {
      width: 4px;
      height: 80px;
      background: linear-gradient(to bottom, #f00, #ff0);
      border-radius: 2px;
      box-shadow: 0 0 5px rgba(255,0,0,0.8);
    }
    /* MINI MAPA (igual que en app5 original) */
    #miniMapContainer {
      position: absolute;
      top: 20px;
      right: 20px;
      width: 300px;
      height: 225px;
      overflow: hidden;
      border-radius: 10px;
      z-index: 2000;
    }
    #eyeCrossSection {
      width: 100%;
      height: 100%;
      display: block;
    }
    /* PANEL DE PARÁMETROS: se ubica ordenadamente debajo del mini mapa (derecha) */
    .control-panel {
      position: absolute;
      top: 250px;
      right: 20px;
      width: 300px;
      background: rgba(10,10,10,0.85);
      padding: 15px;
      border-radius: 8px;
      z-index: 5000;
      font-size: 14px;
    }
    .control-panel .vital-sign {
      display: flex;
      justify-content: space-between;
      margin: 8px 0;
    }
    .control-panel .vital-label {
      color: #ccc;
    }
    .control-panel .vital-value {
      font-family: 'Courier New', monospace;
      font-weight: bold;
    }
    .normal { color: #2ecc71; }
    .warning { color: #f39c12; }
    .danger { color: #e74c3c; }
    #iop-gauge {
      width: 100%;
      height: 8px;
      background: linear-gradient(to right, #2ecc71 0%, #2ecc71 30%, #f39c12 30%, #f39c12 70%, #e74c3c 70%, #e74c3c 100%);
      border-radius: 4px;
      margin-top: 5px;
      overflow: hidden;
    }
    #iop-level {
      height: 100%;
      width: 50%;
      background: rgba(255,255,255,0.3);
      border-radius: 4px;
      transition: width 0.5s ease;
    }
    /* PANEL DE INSTRUMENTOS - CENTRADO COMO EN app.html */
    .instrument-panel {
      position: absolute;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.7);
      padding: 15px;
      border-radius: 10px;
      z-index: 5000;
    }
    .toggle-btn {
      background: #1e3a8a;
      color: white;
      padding: 8px 12px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 0.9em;
      margin: 0 5px;
    }
    .toggle-btn.active {
      background: #3b82f6;
      box-shadow: 0 0 10px #3b82f6;
    }
    /* JOYSTICKS: INTERCAMBIADOS: endoiluminador a la izquierda, vitrectomo a la derecha */
    .joystick-container {
      position: absolute;
      bottom: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      z-index: 3000;
    }
    #joystick-light-container {
      left: 20px;
      right: auto;
    }
    #joystick-vitrectomo-container {
      right: 20px;
      left: auto;
    }
    .joystick {
      width: 100px;
      height: 100px;
      background: rgba(255,255,255,0.1);
      border: 2px solid rgba(255,255,255,0.3);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      touch-action: none;
      position: relative;
    }
    .joystick .joystick-handle {
      width: 40px;
      height: 40px;
      background: rgba(255,255,255,0.5);
      border-radius: 50%;
      position: absolute;
      transition: transform 0.1s ease;
    }
    /* EFECTOS ADICIONALES */
    .laser-spot {
      width: 25px;
      height: 25px;
      background: radial-gradient(circle, rgba(255,50,50,0.8), transparent 70%);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: laser-fade 2.5s ease-out forwards;
    }
    @keyframes laser-fade {
      0% { opacity: 1; transform: scale(1); }
      100% { opacity: 0; transform: scale(0.3); }
    }
    .laser-burn {
      width: 6px;
      height: 6px;
      background: radial-gradient(circle, white 0%, rgba(255,255,255,0.6) 60%, transparent 80%);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
    }
    .vitreous-particle {
      width: 5px;
      height: 5px;
      background: rgba(255,255,255,0.9);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: float-particle 1.5s ease-out forwards;
    }
    @keyframes float-particle {
      100% { transform: translate(var(--tx, 0px), var(--ty, 0px)); opacity: 0; }
    }
    .injection-bubble {
      position: absolute;
      background: rgba(200,230,255,0.7);
      border-radius: 50%;
      pointer-events: none;
      z-index: 3;
      filter: blur(1px);
      animation: bubble-float 4s ease-in-out forwards;
    }
    @keyframes bubble-float {
      100% { 
        transform: translate(calc(var(--tx)*1px), calc(var(--ty)*1px));
        opacity: 0;
      }
    }
    /* CLASE PARA EFECTO PEELING (en el OCT) */
    .peel-remove {
      transition: transform 1s ease, opacity 1s ease;
      transform: translateY(-20px);
      opacity: 0;
    }
    /* ================== DIBUJO OCT ================== */
    /* Se utiliza el mismo SVG que en practica6 pero se modifica el contenedor */
    #oct-container {
      position: absolute;
      top: 10px;
      left: 10px;
      z-index: 1000;
      width: 400px;  /* 50% del ancho original */
      height: 350px; /* Extendido en altura para abarcar más hacia abajo */
    }
    #oct-container svg {
      width: 100%;
      height: 100%;
    }
    /* Posicionamiento de las etiquetas dentro del OCT (calculado en base al viewBox 800×500) */
    .oct-label {
      position: absolute;
      color: white;
      background: rgba(0,0,0,0.6);
      padding: 2px 4px;
      border-radius: 3px;
      font-size: 12px;
      pointer-events: none;
      transform: translate(-50%, -50%);
    }
    /* Se asignan porcentajes aproximados para que se ubiquen dentro del dibujo, según practica6 */
    #label-mli    { left: 81.25%; top: 28%; }
    #label-cfnr   { left: 81.25%; top: 32%; }
    #label-ccg    { left: 81.25%; top: 37%; }
    #label-cpi    { left: 81.25%; top: 42%; }
    #label-cni    { left: 81.25%; top: 47%; }
    #label-cpe    { left: 81.25%; top: 52%; }
    #label-cne    { left: 81.25%; top: 57%; }
    #label-mle    { left: 81.25%; top: 61%; }
    #label-isos   { left: 81.25%; top: 63.4%; }
    #label-epr    { left: 81.25%; top: 69%; }
    #label-mbruch { left: 76.25%; top: 71%; }
    #label-coro   { left: 78.75%; top: 76%; }
    #label-fov    { left: 50%;    top: 40%; }
  </style>
</head>
<body>
  <!-- CONTENEDOR PRINCIPAL -->
  <div id="container">
    <!-- MINI MAPA -->
    <div id="miniMapContainer">
      <svg id="eyeCrossSection" viewBox="0 0 800 600" preserveAspectRatio="xMidYMid meet">
        <defs>
          <radialGradient id="bgGradient" cx="50%" cy="50%" r="70%">
            <stop offset="0%" stop-color="#1E293B" />
            <stop offset="100%" stop-color="#0F172A" />
          </radialGradient>
          <radialGradient id="lensGradient" cx="50%" cy="50%" r="50%">
            <stop offset="0%" stop-color="#E0F7FA" stop-opacity="0.9" />
            <stop offset="70%" stop-color="#B2EBF2" stop-opacity="0.8" />
            <stop offset="100%" stop-color="#80DEEA" stop-opacity="0.7" />
          </radialGradient>
          <filter id="dropShadow" x="-20%" y="-20%" width="140%" height="140%">
            <feGaussianBlur in="SourceAlpha" stdDeviation="8" />
            <feOffset dx="0" dy="4" result="offsetblur" />
            <feComponentTransfer>
              <feFuncA type="linear" slope="0.3" />
            </feComponentTransfer>
            <feMerge>
              <feMergeNode />
              <feMergeNode in="SourceGraphic" />
            </feMerge>
          </filter>
        </defs>
        <rect width="800" height="600" fill="url(#bgGradient)" />
        <ellipse cx="400" cy="150" rx="100" ry="65" fill="url(#lensGradient)" filter="url(#dropShadow)" />
        <ellipse cx="400" cy="150" rx="95" ry="60" fill="none" stroke="#B3E5FC" stroke-width="2" />
        <circle cx="400" cy="150" r="55" fill="none" stroke="url(#irisGradient)" stroke-width="15" />
        <circle cx="400" cy="150" r="30" fill="#000000" />
        <circle cx="400" cy="300" r="250" fill="none" stroke="#E0E0E0" stroke-width="4" />
        <circle cx="400" cy="300" r="240" fill="#400000" opacity="0.8" />
        <circle id="miniCenter" cx="400" cy="300" r="8" fill="#00f7ff" />
        <line id="probeLight" x1="225" y1="100" x2="350" y2="300" stroke="#FFFFFF" stroke-width="9" stroke-linecap="round" />
        <line id="probeLightInner" x1="225" y1="100" x2="350" y2="300" stroke="#FFFFFF" stroke-width="6" stroke-linecap="round" />
        <line id="probeForceps" x1="575" y1="100" x2="450" y2="300" stroke="#FFFFFF" stroke-width="9" stroke-linecap="round" />
        <line id="probeForcepsInner" x1="575" y1="100" x2="450" y2="300" stroke="#FFFFFF" stroke-width="6" stroke-linecap="round" />
      </svg>
    </div>
    
    <!-- PANEL DE INSTRUMENTOS - CENTRADO COMO EN app.html -->
    <div class="instrument-panel">
      <button class="toggle-btn" id="btn-vitrectomo">Vitrectomo</button>
      <button class="toggle-btn" id="btn-peeling">Peeling</button>
      <button class="toggle-btn" id="btn-laser">Láser</button>
      <button class="toggle-btn" id="btn-injection">Inyección</button>
    </div>
    
    <!-- PANEL DE PARÁMETROS -->
    <div class="control-panel">
      <div class="vital-sign">
        <span class="vital-label">Presión Intraocular:</span>
        <span class="vital-value normal" id="iop-value">16 mmHg</span>
      </div>
      <div id="iop-gauge">
        <div id="iop-level"></div>
      </div>
      <div class="vital-sign">
        <span class="vital-label">Vitreo Removido:</span>
        <span class="vital-value" id="vitreous-progress">0%</span>
      </div>
      <div class="vital-sign">
        <span class="vital-label">Estado Retina:</span>
        <span class="vital-value normal" id="retina-status">Estable</span>
      </div>
    </div>
    
    <!-- JOYSTICKS -->
    <div id="joystick-light-container" class="joystick-container">
      <div id="joystick-light" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container z-control">
        <label for="endo-z-slider">Endoiluminador Z (tamaño luz):</label>
        <input type="range" id="endo-z-slider" min="0" max="200" value="50">
      </div>
    </div>
    <div id="joystick-vitrectomo-container" class="joystick-container">
      <div id="joystick-vitrectomo" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container z-control">
        <label for="vitrectomo-z-slider">Vitrectomo Z:</label>
        <input type="range" id="vitrectomo-z-slider" min="-250" max="-50" value="-150">
        <button id="btn-precionar">Precionar</button>
      </div>
    </div>
    
    <!-- CONTENEDOR DEL OJO (RETINA CENTRAL) -->
    <div id="eye-chamber">
      <div class="retina-container" id="retina">
        <div class="retina-sphere">
          <div class="retina-texture"></div>
          <div class="retina-nerve"></div>
          <div class="optic-disc">
            <div class="optic-cup"></div>
          </div>
          <div class="macula"></div>
          <svg class="blood-vessels" viewBox="0 0 600 600">
            <path d="M100,300 Q250,200 300,300 T500,300" fill="none" stroke="#8b0000" stroke-width="2" stroke-opacity="0.7"/>
            <path d="M100,280 Q250,400 300,280 T500,280" fill="none" stroke="#8b0000" stroke-width="1.5" stroke-opacity="0.6"/>
            <path d="M300,100 Q200,250 300,300 T300,500" fill="none" stroke="#8b0000" stroke-width="1.8" stroke-opacity="0.7"/>
          </svg>
        </div>
        <div id="light-mask"></div>
        <div id="light-reflection"></div>
        <div id="vitrectome" class="instrument"></div>
        <div id="end-grasper" class="instrument"></div>
        <div id="laser-probe" class="instrument"></div>
      </div>
    </div>
    
    <!-- CONTENEDOR DEL OCT (50% del tamaño original, extendido en altura) -->
    <div id="oct-container">
      <svg class="oct-scan" viewBox="0 0 800 500" preserveAspectRatio="xMidYMid meet">
        <!-- Fondo negro -->
        <rect width="800" height="500" fill="black"/>
        <!-- Capa de vítreo -->
        <path d="M0,120 C100,110 200,105 300,110 C400,115 500,110 600,105 C700,100 800,110 800,120" stroke="rgba(0,0,255,0.1)" stroke-width="2" fill="none"/>
        <!-- MLI - Membrana Limitante Interna -->
        <g id="mli-group">
          <path id="mli-layer" d="M0,140 C100,130 200,125 300,135 C400,145 500,140 600,130 C700,120 800,140 800,150" stroke="rgba(255,255,255,0.9)" stroke-width="2" fill="none"/>
        </g>
        <!-- CFNR -->
        <path d="M0,160 C100,150 200,145 300,155 C400,165 500,160 600,150 C700,140 800,160 800,170" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,160 C100,150 200,145 300,155 C400,165 500,160 600,150 C700,140 800,160 800,170" stroke="none" fill="rgba(200,220,255,0.3)"/>
        <!-- CCG -->
        <path d="M0,180 C100,170 200,165 300,175 C400,185 500,180 600,170 C700,160 800,180 800,190" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,160 C100,150 200,145 300,155 C400,165 500,160 600,150 C700,140 800,160 800,170 L800,190 C700,180 600,190 500,200 C400,205 300,195 200,185 C100,170 0,180 0,180 Z" stroke="none" fill="rgba(150,180,255,0.2)"/>
        <!-- CPI -->
        <path d="M0,200 C100,190 200,185 300,195 C400,205 500,200 600,190 C700,180 800,200 800,210" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,180 C100,170 200,165 300,175 C400,185 500,180 600,170 C700,160 800,180 800,190 L800,210 C700,200 600,210 500,220 C400,225 300,215 200,205 C100,190 0,200 0,200 Z" stroke="none" fill="rgba(120,150,220,0.2)"/>
        <!-- CNI -->
        <path d="M0,230 C100,220 200,215 300,225 C400,235 500,230 600,220 C700,210 800,230 800,240" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,200 C100,190 200,185 300,195 C400,205 500,200 600,190 C700,180 800,200 800,210 L800,240 C700,230 600,240 500,250 C400,255 300,245 200,235 C100,220 0,230 0,230 Z" stroke="none" fill="rgba(100,130,200,0.25)"/>
        <!-- CPE -->
        <path d="M0,250 C100,240 200,235 300,245 C400,255 500,250 600,240 C700,230 800,250 800,260" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,230 C100,220 200,215 300,225 C400,235 500,230 600,220 C700,210 800,230 800,240 L800,260 C700,250 600,260 500,270 C400,275 300,265 200,255 C100,240 0,250 0,250 Z" stroke="none" fill="rgba(120,140,180,0.2)"/>
        <!-- CNE -->
        <path d="M0,280 C100,270 200,265 300,275 C400,285 500,280 600,270 C700,260 800,280 800,290" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,250 C100,240 200,235 300,245 C400,255 500,250 600,240 C700,230 800,250 800,260 L800,290 C700,280 600,290 500,300 C400,305 300,295 200,285 C100,270 0,280 0,280 Z" stroke="none" fill="rgba(150,150,150,0.3)"/>
        <!-- MLE -->
        <path d="M0,290 C100,280 200,275 300,285 C400,295 500,290 600,280 C700,270 800,290 800,300" stroke="rgba(255,255,255,0.9)" stroke-width="1" fill="none"/>
        <!-- SI (Segmentos Internos) -->
        <path d="M0,310 C100,300 200,295 300,305 C400,315 500,310 600,300 C700,290 800,310 800,320" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,290 C100,280 200,275 300,285 C400,295 500,290 600,280 C700,270 800,290 800,300 L800,320 C700,310 600,320 500,330 C400,335 300,325 200,315 C100,300 0,310 0,310 Z" stroke="none" fill="rgba(200,180,120,0.3)"/>
        <!-- SE (Segmentos Externos) -->
        <path d="M0,330 C100,320 200,315 300,325 C400,335 500,330 600,320 C700,310 800,330 800,340" stroke="rgba(255,255,255,0.8)" stroke-width="1" fill="none"/>
        <path d="M0,310 C100,300 200,295 300,305 C400,315 500,310 600,300 C700,290 800,310 800,320 L800,340 C700,330 600,340 500,350 C400,355 300,345 200,335 C100,320 0,330 0,330 Z" stroke="none" fill="rgba(220,180,100,0.35)"/>
        <!-- Unión IS/OS -->
        <path d="M0,322 C100,312 200,307 300,317 C400,327 500,322 600,312 C700,302 800,322 800,332" stroke="rgba(255,255,255,0.9)" stroke-width="2" fill="none"/>
        <!-- EPR -->
        <path d="M0,350 C100,340 200,335 300,345 C400,355 500,350 600,340 C700,330 800,350 800,360" stroke="rgba(255,255,255,0.9)" stroke-width="2" fill="none"/>
        <path d="M0,330 C100,320 200,315 300,325 C400,335 500,330 600,320 C700,310 800,330 800,340 L800,360 C700,350 600,360 500,370 C400,375 300,365 200,355 C100,340 0,350 0,350 Z" stroke="none" fill="rgba(100,80,60,0.5)"/>
        <!-- M.Bruch -->
        <path d="M0,360 C100,350 200,345 300,355 C400,365 500,360 600,350 C700,340 800,360 800,370" stroke="rgba(255,255,255,0.9)" stroke-width="2" fill="none"/>
        <!-- Coroides -->
        <path d="M0,400 C100,390 200,385 300,395 C400,405 500,400 600,390 C700,380 800,400 800,410" stroke="rgba(255,255,255,0.5)" stroke-width="1" fill="none"/>
        <path d="M0,360 C100,350 200,345 300,355 C400,365 500,360 600,350 C700,340 800,360 800,370 L800,410 C700,400 600,410 500,420 C400,425 300,415 200,405 C100,390 0,400 0,400 Z" stroke="none" fill="rgba(80,50,40,0.4)"/>
        <!-- Fóvea -->
        <path d="M350,140 C380,170 420,170 450,140" stroke="rgba(255,255,255,0.6)" stroke-width="1" fill="none"/>
        <path d="M350,160 C380,190 420,190 450,160" stroke="rgba(255,255,255,0.6)" stroke-width="1" fill="none"/>
        <path d="M350,180 C380,210 420,210 450,180" stroke="rgba(255,255,255,0.6)" stroke-width="1" fill="none"/>
        <path d="M350,200 C380,230 420,230 450,200" stroke="rgba(255,255,255,0.6)" stroke-width="1" fill="none"/>
        <path d="M350,220 C380,250 420,250 450,220" stroke="rgba(255,255,255,0.6)" stroke-width="1" fill="none"/>
      </svg>
      <!-- Etiquetas dentro del OCT -->
      <div id="label-mli"    class="oct-label">MLI</div>
      <div id="label-cfnr"   class="oct-label">CFNR</div>
      <div id="label-ccg"    class="oct-label">CCG</div>
      <div id="label-cpi"    class="oct-label">CPI</div>
      <div id="label-cni"    class="oct-label">CNI</div>
      <div id="label-cpe"    class="oct-label">CPE</div>
      <div id="label-cne"    class="oct-label">CNE</div>
      <div id="label-mle"    class="oct-label">MLE</div>
      <div id="label-isos"   class="oct-label">IS/OS</div>
      <div id="label-epr"    class="oct-label">EPR</div>
      <div id="label-mbruch" class="oct-label">M.Bruch</div>
      <div id="label-coro"   class="oct-label">Coro</div>
      <div id="label-fov"    class="oct-label">Fóv</div>
    </div>
  </div>
  
  <!-- ALERTAS -->
  <div class="alert-modal" id="alert-modal" style="display:none; position:fixed; top:50%; left:50%; transform:translate(-50%,-50%); background:rgba(20,0,0,0.9); padding:20px; border-radius:8px; z-index:10000;">
    <div class="alert-title" id="alert-title" style="color:#f44; font-size:18px; margin-bottom:10px;">Alerta</div>
    <div class="alert-message" id="alert-message" style="margin-bottom:15px;"></div>
    <button class="alert-btn" onclick="closeAlert()" style="padding:8px 15px; background:#800; color:white; border:none; border-radius:4px; cursor:pointer;">Aceptar</button>
  </div>
  
  <!-- ================== SCRIPTS ================== -->
  <script>
    /* Función para remover la línea MLI del OCT con efecto peel */
    function peelMLI_OCT() {
      var mliGroup = document.getElementById('mli-group');
      var labelMLI = document.getElementById('label-mli');
      if(mliGroup) {
        mliGroup.classList.add('peel-remove');
        setTimeout(function(){ mliGroup.remove(); }, 1000);
      }
      if(labelMLI) {
        labelMLI.classList.add('peel-remove');
        setTimeout(function(){ labelMLI.remove(); }, 1000);
      }
    }
    
    /* VARIABLES GLOBALES */
    let lightJoystickX = 50, lightJoystickY = 50;
    let vitrectomoJoystickX = 50, vitrectomoJoystickY = 50;
    let currentDepth = parseInt(document.getElementById('vitrectomo-z-slider').value);
    let activeInstrument = null;
    let iop = 16; // mmHg
    let vitreousRemoved = 0;
    let retinaStatus = 'Estable';
    let peelAccumulated = 0;
    const peelStep = 30;
    let tearPoints = [];
    
    /* FUNCIONES DE ALERTA */
    function showAlert(title, message) {
      document.getElementById('alert-title').innerText = title;
      document.getElementById('alert-message').innerText = message;
      document.getElementById('alert-modal').style.display = 'block';
    }
    function closeAlert() {
      document.getElementById('alert-modal').style.display = 'none';
    }
    
    /* Función toggle para instrumentos */
    function toggleInstrument(btnId, instrumentId) {
      const btn = document.getElementById(btnId);
      const instr = document.getElementById(instrumentId);
      if(btn.classList.contains('active')) {
        btn.classList.remove('active');
        if(instr) instr.style.display = 'none';
        if(btnId === 'btn-peeling') {
          const mem = document.getElementById('membrane');
          if(mem) mem.remove();
        }
      } else {
        btn.classList.add('active');
        if(instr) instr.style.display = 'block';
        activeInstrument = instrumentId;
        if(btnId === 'btn-peeling') {
          initPeeling();
        }
      }
    }
    document.getElementById('btn-vitrectomo').addEventListener('click', function(){
      toggleInstrument('btn-vitrectomo', 'vitrectome');
    });
    document.getElementById('btn-peeling').addEventListener('click', function(){
      toggleInstrument('btn-peeling', 'end-grasper');
    });
    document.getElementById('btn-laser').addEventListener('click', function(){
      toggleInstrument('btn-laser', 'laser-probe');
    });
    document.getElementById('btn-injection').addEventListener('click', function(){
      performInjection();
    });
    
    /* Botón PRECIONAR */
    document.getElementById('btn-precionar').addEventListener('click', () => {
      if(activeInstrument === 'laser-probe'){
         const instrument = document.getElementById('laser-probe');
         const retinaRect = document.getElementById('retina').getBoundingClientRect();
         const instRect = instrument.getBoundingClientRect();
         const x = instRect.left + instRect.width/2 - retinaRect.left;
         const y = instRect.top + instRect.height/2 - retinaRect.top;
         const syntheticEvent = { clientX: retinaRect.left + x, clientY: retinaRect.top + y };
         laserFunction(syntheticEvent);
      } else if(activeInstrument === 'vitrectome'){
         const instrument = document.getElementById('vitrectome');
         const retinaRect = document.getElementById('retina').getBoundingClientRect();
         const instRect = instrument.getBoundingClientRect();
         const x = instRect.left + instRect.width/2 - retinaRect.left;
         const y = instRect.top + instRect.height/2 - retinaRect.top;
         const syntheticEvent = { clientX: retinaRect.left + x, clientY: retinaRect.top + y };
         vitrectomyFunction(syntheticEvent);
      } else if(activeInstrument === 'end-grasper'){
         const membrane = document.getElementById('membrane');
         const peelCanvas = document.getElementById('peelCanvas');
         const pinza = document.getElementById('pinza');
         if(membrane && peelCanvas && pinza) {
            const memRect = membrane.getBoundingClientRect();
            const centerX = memRect.width/2;
            const centerY = memRect.height/2;
            const radius = memRect.width/2;
            pinza.setAttribute("cx", centerX + radius);
            pinza.setAttribute("cy", centerY);
            const tearPath = document.getElementById('tearPath');
            const d = `M ${centerX} ${centerY} L ${centerX + radius} ${centerY}`;
            tearPath.setAttribute("d", d);
            activarDesgarroPeel(membrane, pinza, peelCanvas);
         }
      }
    });
    
    /* Actualización de parámetros (vitals) */
    function updateVitals() {
      iop += (Math.random() - 0.5) * 0.2;
      if(activeInstrument === 'vitrectome') iop -= 0.1;
      document.getElementById('iop-value').innerText = iop.toFixed(1) + " mmHg";
      document.getElementById('iop-level').style.width = Math.min(100, Math.max(0, iop)) + "%";
      document.getElementById('vitreous-progress').innerText = Math.min(100, vitreousRemoved).toFixed(0) + "%";
      document.getElementById('retina-status').innerText = retinaStatus;
    }
    setInterval(updateVitals, 1000);
    
    /* Actualización de posición de instrumentos */
    function updateVitrectomoPosition(normX, normY) {
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const maxOffset = retinaRect.width / 2;
      const offsetX = (normX - 50) / 50 * maxOffset;
      const offsetY = (normY - 50) / 50 * maxOffset;
      const zVal = document.getElementById('vitrectomo-z-slider').value;
      if(activeInstrument) {
        const instr = document.getElementById(activeInstrument);
        if(instr) {
          instr.style.transform = `translate(calc(-50% + ${offsetX}px), calc(-50% + ${offsetY}px)) translateZ(${zVal}px)`;
        }
      }
      vitrectomoJoystickX = normX;
      updateMiniLeftLine(normX, normY);
    }
    function updateEndoLightEffect(normX, normY) {
      document.documentElement.style.setProperty('--light-x', normX + '%');
      document.documentElement.style.setProperty('--light-y', normY + '%');
      document.getElementById('light-reflection').style.opacity = 0.7;
      const zVal = parseInt(document.getElementById('endo-z-slider').value);
      document.getElementById('light-mask').style.transform = `translateZ(${zVal}px)`;
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const minLightSize = 10;
      const maxLightSize = retinaRect.width;
      const newLightSize = minLightSize + (maxLightSize - minLightSize) * (zVal / 200);
      document.documentElement.style.setProperty('--light-size', newLightSize + 'px');
      lightJoystickX = normX;
      updateMiniRightLine(normX, normY);
    }
    function updateMiniLeftLine(normX, normY) {
      const defaultTipX = 350;
      const defaultTipY = 300;
      const scaleX = 2.5;
      const scaleY = 1.8;
      const offsetX = (normX - 50) * scaleX;
      const offsetY = (normY - 50) * scaleY;
      let tipX = defaultTipX + offsetX;
      let tipY = defaultTipY + offsetY;
      const miniLeft = document.getElementById('probeLight');
      const miniLeftInner = document.getElementById('probeLightInner');
      miniLeft.setAttribute('x2', tipX);
      miniLeftInner.setAttribute('x2', tipX);
      miniLeft.setAttribute('y2', tipY);
      miniLeftInner.setAttribute('y2', tipY);
    }
    function updateMiniRightLine(normX, normY) {
      const defaultTipX = 450;
      const defaultTipY = 300;
      const scaleX = 2.5;
      const scaleY = 1.8;
      const offsetX = (normX - 50) * scaleX;
      const offsetY = (normY - 50) * scaleY;
      let tipX = defaultTipX + offsetX;
      let tipY = defaultTipY + offsetY;
      const miniRight = document.getElementById('probeForceps');
      const miniRightInner = document.getElementById('probeForcepsInner');
      miniRight.setAttribute('x2', tipX);
      miniRightInner.setAttribute('x2', tipX);
      miniRight.setAttribute('y2', tipY);
      miniRightInner.setAttribute('y2', tipY);
    }
    function initJoystick(joystickElement, updateCallback) {
      const handle = joystickElement.querySelector('.joystick-handle');
      const rect = joystickElement.getBoundingClientRect();
      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const maxDistance = rect.width / 2;
      let dragging = false;
      function pointerDown(e) {
        dragging = true;
        joystickElement.setPointerCapture(e.pointerId);
      }
      function pointerMove(e) {
        if (!dragging) return;
        const bounds = joystickElement.getBoundingClientRect();
        const x = e.clientX - bounds.left;
        const y = e.clientY - bounds.top;
        let deltaX = x - centerX;
        let deltaY = y - centerY;
        const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY);
        if (distance > maxDistance) {
          const angle = Math.atan2(deltaY, deltaX);
          deltaX = Math.cos(angle) * maxDistance;
          deltaY = Math.sin(angle) * maxDistance;
        }
        handle.style.transform = `translate(${deltaX}px, ${deltaY}px)`;
        let normalizedX = ((deltaX + maxDistance) / (2 * maxDistance)) * 100;
        let normalizedY = ((deltaY + maxDistance) / (2 * maxDistance)) * 100;
        updateCallback(normalizedX, normalizedY);
      }
      function pointerUp(e) {
        dragging = false;
        handle.style.transform = `translate(0px, 0px)`;
        updateCallback(50, 50);
      }
      joystickElement.addEventListener('pointerdown', pointerDown);
      joystickElement.addEventListener('pointermove', pointerMove);
      joystickElement.addEventListener('pointerup', pointerUp);
      joystickElement.addEventListener('pointerleave', pointerUp);
    }
    const joystickVitrectomo = document.getElementById('joystick-vitrectomo');
    initJoystick(joystickVitrectomo, updateVitrectomoPosition);
    const joystickLight = document.getElementById('joystick-light');
    initJoystick(joystickLight, updateEndoLightEffect);
    document.getElementById('vitrectomo-z-slider').addEventListener('input', function(){
      currentDepth = parseInt(this.value);
      updateVitrectomoPosition(vitrectomoJoystickX, vitrectomoJoystickX);
    });
    document.getElementById('endo-z-slider').addEventListener('input', function(){
      updateEndoLightEffect(lightJoystickX, lightJoystickY);
    });
    
    function laserFunction(e) {
      const retina = document.getElementById('retina');
      const rect = retina.getBoundingClientRect();
      const laserSpot = document.createElement('div');
      laserSpot.className = 'laser-spot';
      laserSpot.style.left = (e.clientX - rect.left - 12) + 'px';
      laserSpot.style.top = (e.clientY - rect.top - 12) + 'px';
      retina.appendChild(laserSpot);
      const burnMark = document.createElement('div');
      burnMark.className = 'laser-burn';
      burnMark.style.left = (e.clientX - rect.left - 3) + 'px';
      burnMark.style.top = (e.clientY - rect.top - 3) + 'px';
      retina.appendChild(burnMark);
      setTimeout(() => { laserSpot.remove(); }, 2500);
    }
    function vitrectomyFunction(e) {
      const retina = document.getElementById('retina');
      const rect = retina.getBoundingClientRect();
      const particleCount = 10;
      for (let i = 0; i < particleCount; i++) {
        const particle = document.createElement('div');
        particle.className = 'vitreous-particle';
        particle.style.left = (e.clientX - rect.left + (Math.random()*20 - 10)) + 'px';
        particle.style.top = (e.clientY - rect.top + (Math.random()*20 - 10)) + 'px';
        particle.style.setProperty('--tx', Math.random()*40 - 20);
        particle.style.setProperty('--ty', Math.random()*40 - 20);
        retina.appendChild(particle);
        setTimeout(() => particle.remove(), 1500);
      }
      vitreousRemoved = Math.min(100, vitreousRemoved + 0.8);
      document.getElementById('vitreous-progress').innerText = `${vitreousRemoved.toFixed(0)}%`;
    }
    function performInjection() {
      showAlert("FASE 4: Inyección", "Inyectando solución salina equilibrada...");
      const retina = document.getElementById('retina');
      retina.style.background = `
        radial-gradient(circle at 35% 45%, rgba(100,150,255,0.2) 0%, rgba(50,100,255,0.3) 70%, rgba(20,50,255,0.2) 100%),
        repeating-linear-gradient(45deg, rgba(100,150,255,0.1) 0px, rgba(100,150,255,0.1) 1px, transparent 1px, transparent 10px)`;
      for (let i = 0; i < 30; i++) {
        setTimeout(() => {
          const bubble = document.createElement('div');
          bubble.className = 'injection-bubble';
          bubble.style.left = `${50 + Math.random()*20}%`;
          bubble.style.top = `${50 + Math.random()*20}%`;
          bubble.style.width = `${5 + Math.random()*15}px`;
          bubble.style.height = bubble.style.width;
          bubble.style.setProperty('--tx', Math.random()*100 - 50);
          bubble.style.setProperty('--ty', Math.random()*100 - 50);
          retina.appendChild(bubble);
          setTimeout(() => bubble.remove(), 4000);
        }, i*150);
      }
      setTimeout(() => {
        showAlert("Procedimiento Completado", "Vitrectomía posterior finalizada con éxito");
      }, 5000);
    }
    
    /* FUNCIONALIDAD DE PEELING: crea overlay en retina y sincroniza con el OCT */
    function initPeeling() {
      if(document.getElementById('membrane')) return;
      const retina = document.getElementById('retina');
      // Crear overlay circular en la retina
      const membrane = document.createElement('div');
      membrane.id = 'membrane';
      membrane.style.position = 'absolute';
      membrane.style.top = '50%';
      membrane.style.left = '50%';
      membrane.style.width = '50%';
      membrane.style.height = '50%';
      membrane.style.background = 'radial-gradient(circle at center, rgba(255,255,255,0.3) 0%, rgba(255,255,255,0.1) 60%, transparent 100%)';
      membrane.style.borderRadius = '50%';
      membrane.style.transform = 'translate(-50%, -50%)';
      membrane.style.cursor = 'grab';
      retina.appendChild(membrane);
      // Agregar canvas SVG para simular el peeling
      const peelCanvas = document.createElementNS("http://www.w3.org/2000/svg", "svg");
      peelCanvas.setAttribute('id', 'peelCanvas');
      peelCanvas.setAttribute('style', 'position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:auto;');
      membrane.appendChild(peelCanvas);
      const defs = document.createElementNS("http://www.w3.org/2000/svg", "defs");
      const mask = document.createElementNS("http://www.w3.org/2000/svg", "mask");
      mask.setAttribute("id", "peelMask");
      const fullCircle = document.createElementNS("http://www.w3.org/2000/svg", "circle");
      fullCircle.setAttribute("cx", "50%");
      fullCircle.setAttribute("cy", "50%");
      fullCircle.setAttribute("r", "50%");
      fullCircle.setAttribute("fill", "white");
      mask.appendChild(fullCircle);
      defs.appendChild(mask);
      peelCanvas.appendChild(defs);
      membrane.style.webkitMaskImage = "url(#peelMask)";
      membrane.style.maskImage = "url(#peelMask)";
      const tearPath = document.createElementNS("http://www.w3.org/2000/svg", "path");
      tearPath.setAttribute('id', 'tearPath');
      peelCanvas.appendChild(tearPath);
      const pinza = document.createElementNS("http://www.w3.org/2000/svg", "circle");
      pinza.setAttribute('id', 'pinza');
      pinza.setAttribute('cx', '50%');
      pinza.setAttribute('cy', '50%');
      pinza.setAttribute('r', '7');
      pinza.setAttribute('fill', 'green');
      peelCanvas.appendChild(pinza);
      let isDragging = false;
      tearPoints = [];
      pinza.addEventListener('mousedown', (event) => {
        if(event.button !== 0) return;
        isDragging = true;
        tearPoints = [];
        const pt = getSVGPoint(event, peelCanvas);
        tearPoints.push([pt.x, pt.y]);
        pinza.style.cursor = 'grabbing';
      });
      peelCanvas.addEventListener('mousemove', (event) => {
        if(!isDragging) return;
        const pt = getSVGPoint(event, peelCanvas);
        pinza.setAttribute('cx', pt.x);
        pinza.setAttribute('cy', pt.y);
        tearPoints.push([pt.x, pt.y]);
        updateTearPath(tearPoints, tearPath);
        const memRect = membrane.getBoundingClientRect();
        const center = { x: memRect.width / 2, y: memRect.height / 2 };
        const current = { x: pt.x, y: pt.y };
        const dist = Math.hypot(current.x - center.x, current.y - center.y);
        const radius = memRect.width / 2;
        if(dist >= radius) {
          activarDesgarroPeel(membrane, pinza, peelCanvas);
          isDragging = false;
        }
      });
      document.addEventListener('mouseup', () => {
        if(isDragging) {
          isDragging = false;
          pinza.style.cursor = 'grab';
          tearPoints = [];
          updateTearPath([], tearPath);
        }
      });
    }
    function getSVGPoint(event, svg) {
      let pt = svg.createSVGPoint();
      pt.x = event.clientX;
      pt.y = event.clientY;
      return pt.matrixTransform(svg.getScreenCTM().inverse());
    }
    function updateTearPath(points, pathElement) {
      if(points.length === 0) {
        pathElement.setAttribute('d', '');
        return;
      }
      let d = `M ${points[0][0]} ${points[0][1]}`;
      for(let i = 1; i < points.length; i++){
        d += ` L ${points[i][0]} ${points[i][1]}`;
      }
      pathElement.setAttribute('d', d);
    }
    function activarDesgarroPeel(membrane, pinza, peelCanvas) {
      peelAccumulated += peelStep;
      const tearPathEl = document.getElementById('tearPath');
      const d = tearPathEl.getAttribute('d');
      if(d) {
        const mask = peelCanvas.querySelector("mask#peelMask");
        const hole = document.createElementNS("http://www.w3.org/2000/svg", "path");
        hole.setAttribute("d", d);
        hole.setAttribute("fill", "black");
        mask.appendChild(hole);
        updateTearPath([], tearPathEl);
        tearPoints = [];
        if(peelAccumulated >= 100) {
          showAlert("Peeling Completado", "La membrana ha sido removida. Proceda al siguiente paso.");
          // Remover overlay en retina
          membrane.remove();
          // En el OCT, remover la línea MLI con efecto peel
          peelMLI_OCT();
        }
      } else {
        membrane.remove();
        showAlert("Peeling Completado", "La membrana ha sido removida. Proceda al siguiente paso.");
        peelMLI_OCT();
      }
      peelAccumulated = 0;
      pinza.style.cursor = 'grab';
    }
  </script>
</body>
</html>
