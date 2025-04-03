<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Simulador Quirúrgico Retiniano Avanzado</title>
  <style>
    /* ================== ESTILOS BASE ================== */
    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }
    
    body {
      margin: 0;
      padding: 0;
      background: #000;
      overflow: hidden;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      color: white;
      touch-action: manipulation;
    }
    
    #container {
      position: relative;
      width: 100vw;
      height: 100vh;
      perspective: 1200px;
    }
    
    /* ================== RETINA CENTRAL ================== */
    #eye-chamber {
      position: absolute;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      touch-action: none;
    }
    
    .retina-container {
      position: relative;
      width: 80vmin;
      height: 80vmin;
      max-width: 600px;
      max-height: 600px;
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
    
    /* ================== CAMPO DE VISIÓN – Endoiluminador ================== */
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
    
    /* ================== INSTRUMENTOS QUIRÚRGICOS ================== */
    #vitrectome, #end-grasper, #laser-probe {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      display: none;
      z-index: 10;
    }
    
    #vitrectome {
      width: 20px;
      height: 120px;
      background: linear-gradient(to bottom, #aaa, #ddd 20%, #fff 40%, #ddd 60%, #aaa 80%);
      border: 1px solid #999;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }
    
    #vitrectome::after {
      content: '';
      position: absolute;
      bottom: -8px;
      left: 50%;
      transform: translateX(-50%);
      width: 15px;
      height: 15px;
      background: radial-gradient(circle, #fff 0%, #ccc 50%, #999 100%);
      border-radius: 50%;
      box-shadow: 0 0 5px rgba(0,0,0,0.5);
    }
    
    #end-grasper {
      width: 18px;
      height: 110px;
      background: linear-gradient(to bottom, #bbb, #eee 20%, #fff 40%, #eee 60%, #bbb 80%);
      border: 1px solid #999;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0,0,0,0.5);
    }
    
    #end-grasper::after {
      content: '';
      position: absolute;
      bottom: -10px;
      left: 50%;
      transform: translateX(-50%);
      width: 12px;
      height: 8px;
      background: #fff;
      border-radius: 4px;
      box-shadow: 0 0 5px rgba(0,0,0,0.5);
    }
    
    #laser-probe {
      width: 4px;
      height: 100px;
      background: linear-gradient(to bottom, #f00, #ff0 30%, #f00 70%);
      border-radius: 2px;
      box-shadow: 0 0 10px rgba(255,0,0,0.8);
    }

    /* ================== NUEVOS ESTILOS PARA PEELING (de practica6) ================== */
    #peeling-container {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: none;
      z-index: 20;
    }

    #retina-canvas {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: 50%;
      box-shadow: 0 0 60px rgba(0, 100, 255, 0.3);
      cursor: crosshair;
      z-index: 21;
    }
    
    #peeling-hud {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0, 0, 0, 0.7);
      color: white;
      padding: 15px;
      border-radius: 10px;
      border-top: 3px solid #0099ff;
      max-width: 600px;
      text-align: center;
      z-index: 22;
    }
    
    #traction-meter {
      margin-top: 10px;
      height: 10px;
      background: #333;
      border-radius: 5px;
      overflow: hidden;
    }
    
    #traction-level {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #00ff00, #ffcc00, #ff0000);
      transition: width 0.1s;
    }
    
    .complication {
      position: absolute;
      background: rgba(200, 0, 0, 0.7);
      border-radius: 50%;
      pointer-events: none;
      transform: translate(-50%, -50%);
      z-index: 23;
    }
    
    #peeling-instrument-selector {
      position: absolute;
      top: 20px;
      right: 20px;
      background: rgba(0, 0, 0, 0.7);
      padding: 10px;
      border-radius: 10px;
      color: white;
      z-index: 24;
    }
    
    .peeling-instrument-btn {
      background: #333;
      border: none;
      color: white;
      padding: 8px 12px;
      margin: 5px;
      border-radius: 5px;
      cursor: pointer;
    }
    
    .peeling-instrument-btn.active {
      background: #0099ff;
      box-shadow: 0 0 10px rgba(0, 153, 255, 0.7);
    }
    
    .membrane-flap {
      position: absolute;
      background: rgba(150, 200, 255, 0.3);
      border: 1px solid rgba(180, 220, 255, 0.6);
      border-radius: 50%;
      transform-origin: center;
      pointer-events: none;
      z-index: 25;
    }

    /* ================== INTERFAZ DE USUARIO ================== */
    /* Panel de instrumentos */
    .instrument-panel {
      position: absolute;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.7);
      padding: 12px 15px;
      border-radius: 10px;
      z-index: 5000;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      max-width: 95%;
      box-shadow: 0 4px 15px rgba(0,0,0,0.5);
    }
    
    .toggle-btn {
      background: #1e3a8a;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: clamp(0.8rem, 2.5vw, 1rem);
      margin: 5px;
      min-width: 80px;
      transition: all 0.2s;
      font-weight: 500;
    }
    
    .toggle-btn.active {
      background: #3b82f6;
      box-shadow: 0 0 15px #3b82f6;
    }
    
    /* Panel de controles */
    .control-panel {
      position: absolute;
      right: 20px;
      top: 150px;
      background: rgba(10,10,10,0.95);
      padding: 15px;
      border-radius: 12px;
      z-index: 5000;
      font-size: clamp(0.7rem, 2vw, 0.9rem);
      width: 160px;
      max-height: 80vh;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      box-shadow: 0 4px 15px rgba(0,0,0,0.5);
      border: 1px solid #333;
    }
    
    .vital-sign {
      display: flex;
      justify-content: space-between;
      margin: 12px 0;
      flex-wrap: wrap;
    }
    
    .vital-label {
      color: #ccc;
      font-size: 0.85em;
      width: 60%;
    }
    
    .vital-value {
      font-family: 'Courier New', monospace;
      font-weight: bold;
      font-size: 0.95em;
      width: 40%;
      text-align: right;
    }
    
    .normal { color: #2ecc71; }
    .warning { color: #f39c12; }
    .danger { color: #e74c3c; }
    
    .gauge-container {
      width: 100%;
      height: 8px;
      background: #333;
      border-radius: 4px;
      margin: 5px 0 12px 0;
      overflow: hidden;
    }
    
    .gauge-level {
      height: 100%;
      border-radius: 4px;
      transition: width 0.5s ease;
    }
    
    #iop-gauge .gauge-level {
      background: linear-gradient(to right, #2ecc71, #f39c12, #e74c3c);
    }
    
    #perfusion-gauge .gauge-level {
      background: linear-gradient(to right, #3498db, #9b59b6);
    }
    
    #vitreous-gauge .gauge-level {
      background: linear-gradient(to right, #1abc9c, #f1c40f);
    }
    
    /* Joysticks */
    .joystick-container {
      position: absolute;
      bottom: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 15px;
      z-index: 3000;
    }
    
    #joystick-light-container {
      left: 20px;
    }
    
    #joystick-vitrectomo-container {
      right: 20px;
    }
    
    .joystick {
      width: 90px;
      height: 90px;
      background: rgba(255,255,255,0.1);
      border: 2px solid rgba(255,255,255,0.3);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      touch-action: none;
      position: relative;
      box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    }
    
    .joystick .joystick-handle {
      width: 35px;
      height: 35px;
      background: rgba(255,255,255,0.6);
      border-radius: 50%;
      position: absolute;
      transition: transform 0.1s ease;
    }
    
    /* Controles Z */
    .slider-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 100%;
      background: rgba(0,0,0,0.5);
      padding: 10px;
      border-radius: 10px;
    }
    
    .slider-container label {
      font-size: 0.75rem;
      margin-bottom: 8px;
      text-align: center;
      color: #eee;
      font-weight: 500;
    }
    
    input[type="range"] {
      width: 100%;
      -webkit-appearance: none;
      height: 8px;
      background: #333;
      border-radius: 4px;
      margin: 5px 0 10px 0;
    }
    
    input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 20px;
      height: 20px;
      background: #3b82f6;
      border-radius: 50%;
      cursor: pointer;
      border: 2px solid white;
    }
    
    #btn-precionar {
      background: #dc2626;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 0.9rem;
      margin-top: 5px;
      width: 100%;
      font-weight: bold;
      box-shadow: 0 2px 8px rgba(0,0,0,0.3);
      transition: all 0.2s;
    }
    
    #btn-precionar:active {
      transform: scale(0.95);
    }
    
    /* Mini mapa */
    #miniMapContainer {
      position: absolute;
      top: 20px;
      right: 20px;
      width: 150px;
      height: 110px;
      overflow: hidden;
      border-radius: 8px;
      z-index: 2000;
      background: rgba(0,0,0,0.7);
      border: 1px solid #333;
      box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    }
    
    #eyeCrossSection {
      width: 100%;
      height: 100%;
      display: block;
    }
    
    /* OCT */
    #oct-container {
      position: absolute;
      top: 150px;
      left: 20px;
      z-index: 1000;
      width: 180px;
      height: 140px;
      background: rgba(0,0,0,0.7);
      border-radius: 8px;
      border: 1px solid #333;
      overflow: hidden;
      box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    }
    
    #oct-container svg {
      width: 100%;
      height: 100%;
    }
    
    /* Efectos visuales */
    .laser-spot {
      width: 25px;
      height: 25px;
      background: radial-gradient(circle, rgba(255,50,50,0.8), transparent 70%);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: laser-fade 2.5s ease-out forwards;
      z-index: 15;
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
      z-index: 16;
    }
    
    .vitreous-particle {
      width: 5px;
      height: 5px;
      background: rgba(255,255,255,0.9);
      border-radius: 50%;
      position: absolute;
      pointer-events: none;
      animation: float-particle 1.5s ease-out forwards;
      z-index: 12;
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
    
    /* ================== RESPONSIVE ADJUSTMENTS ================== */
    @media (max-width: 768px) {
      .retina-container {
        width: 90vmin;
        height: 90vmin;
      }
      
      .instrument-panel {
        top: 10px;
        padding: 10px;
      }
      
      .toggle-btn {
        padding: 8px 12px;
        min-width: 70px;
        margin: 4px;
        font-size: 0.8rem;
      }
      
      .control-panel {
        width: 140px;
        padding: 12px;
        font-size: 0.75rem;
      }
      
      .joystick {
        width: 80px;
        height: 80px;
      }
      
      #miniMapContainer, #oct-container {
        width: 130px;
        height: 100px;
      }
    }
    
    @media (max-width: 480px) {
      .retina-container {
        width: 95vmin;
        height: 95vmin;
      }
      
      .instrument-panel {
        width: 98%;
        padding: 8px;
      }
      
      .toggle-btn {
        padding: 7px 10px;
        min-width: 65px;
        font-size: 0.75rem;
        margin: 3px;
      }
      
      .control-panel {
        width: 130px;
        padding: 10px;
        right: 10px;
      }
      
      .joystick {
        width: 75px;
        height: 75px;
      }
      
      .joystick-container {
        bottom: 15px;
      }
      
      #miniMapContainer, #oct-container {
        width: 110px;
        height: 85px;
      }
    }
  </style>
</head>
<body>
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

    <!-- PANEL DE INSTRUMENTOS -->
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
      <div id="iop-gauge" class="gauge-container">
        <div class="gauge-level" id="iop-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Perfusión Retiniana:</span>
        <span class="vital-value normal" id="perfusion-value">95%</span>
      </div>
      <div id="perfusion-gauge" class="gauge-container">
        <div class="gauge-level" id="perfusion-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Vitreo Removido:</span>
        <span class="vital-value" id="vitreous-value">0%</span>
      </div>
      <div id="vitreous-gauge" class="gauge-container">
        <div class="gauge-level" id="vitreous-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Estado Retina:</span>
        <span class="vital-value normal" id="retina-status">Estable</span>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Flujo de Irrigación:</span>
        <span class="vital-value normal" id="flow-value">25 ml/min</span>
      </div>
    </div>

    <!-- VISUALIZACIÓN OCT -->
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
    </div>

    <!-- JOYSTICKS -->
    <div id="joystick-light-container" class="joystick-container">
      <div id="joystick-light" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container">
        <label>Luz (Z)</label>
        <input type="range" id="endo-z-slider" min="0" max="200" value="50">
      </div>
    </div>

    <div id="joystick-vitrectomo-container" class="joystick-container">
      <div id="joystick-vitrectomo" class="joystick">
        <div class="joystick-handle"></div>
      </div>
      <div class="slider-container">
        <label>Instrumento (Z)</label>
        <input type="range" id="vitrectomo-z-slider" min="-250" max="-50" value="-150">
        <button id="btn-precionar">Accionar</button>
      </div>
    </div>

    <!-- RETINA CENTRAL -->
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

    <!-- ================== NUEVO SISTEMA DE PEELING ================== -->
    <div id="peeling-container">
      <canvas id="retina-canvas"></canvas>
      
      <div id="peeling-instrument-selector">
        <h3 style="margin-top: 0; color: #0099ff;">Instrumentos</h3>
        <button id="forceps-btn" class="peeling-instrument-btn active">Pinzas de Peeling</button>
        <button id="hook-btn" class="peeling-instrument-btn">Gancho Roto</button>
        <button id="scissors-btn" class="peeling-instrument-btn">Tijeras Vitreorretinianas</button>
      </div>
      
      <div id="peeling-hud">
        <h3 style="margin-top: 0; color: #0099ff;">Peeling de Membrana Epirretiniana</h3>
        <p id="procedure-status">Inicie el procedimiento seleccionando el borde de la membrana con las pinzas</p>
        <div id="traction-meter">
          <div id="traction-level"></div>
        </div>
        <small id="traction-warning" style="color: #ff6666; display: none;">¡ADVERTENCIA: Tracción excesiva! Riesgo de desgarro retiniano</small>
      </div>
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <script>
    /* ================== VARIABLES GLOBALES ================== */
    let lightJoystickX = 50, lightJoystickY = 50;
    let vitrectomoJoystickX = 50, vitrectomoJoystickY = 50;
    let currentDepth = parseInt(document.getElementById('vitrectomo-z-slider').value);
    let activeInstrument = null;
    let iop = 16; // mmHg
    let perfusion = 95; // %
    let vitreousRemoved = 0; // %
    let flowRate = 25; // ml/min
    let retinaStatus = 'Estable';

    /* ================== VARIABLES PARA EL SISTEMA DE PEELING ================== */
    let peelingState = {
      step: 'ready', // ready, grasping, peeling, complete
      instrument: 'forceps',
      traction: 0,
      complication: false,
      membraneDetached: false,
      flapCreated: false,
      flapPosition: { x: 0, y: 0 }
    };
    
    let peelingInstruments = {
      forceps: {
        position: { x: 0, y: 0 },
        size: 20,
        angle: 0,
        grasping: false,
        gripPosition: null
      },
      hook: {
        position: { x: 0, y: 0 },
        size: 15
      },
      scissors: {
        position: { x: 0, y: 0 },
        size: 25
      }
    };
    
    let retinaModel = {
      center: { x: 0, y: 0 },
      radius: 0,
      points: []
    };
    
    let membraneModel = {
      points: [],
      thickness: 1.5,
      elasticity: 0.85,
      adhesion: 0.7,
      detachedPoints: 0,
      flapPoints: []
    };

    /* ================== GESTIÓN DE INSTRUMENTOS ================== */
    function toggleInstrument(btnId, instrumentId) {
      const btn = document.getElementById(btnId);
      
      if(btn.classList.contains('active')) {
        btn.classList.remove('active');
        if(instrumentId === 'peeling') {
          endPeeling();
        } else {
          document.getElementById(instrumentId).style.display = 'none';
        }
        activeInstrument = null;
      } else {
        // Desactivar todos los instrumentos primero
        document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
        document.querySelectorAll('.instrument').forEach(i => i.style.display = 'none');
        
        // Activar el seleccionado
        btn.classList.add('active');
        activeInstrument = instrumentId;
        
        if(instrumentId === 'peeling') {
          initPeeling();
        } else {
          document.getElementById(instrumentId).style.display = 'block';
        }
      }
    }

    // Event listeners para instrumentos
    document.getElementById('btn-vitrectomo').addEventListener('click', () => toggleInstrument('btn-vitrectomo', 'vitrectome'));
    document.getElementById('btn-peeling').addEventListener('click', () => toggleInstrument('btn-peeling', 'peeling'));
    document.getElementById('btn-laser').addEventListener('click', () => toggleInstrument('btn-laser', 'laser-probe'));
    document.getElementById('btn-injection').addEventListener('click', performInjection);

    /* ================== BOTÓN DE ACCIÓN PRINCIPAL ================== */
    document.getElementById('btn-precionar').addEventListener('click', () => {
      if(!activeInstrument) return;
      
      const instrument = document.getElementById(activeInstrument);
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const instRect = instrument.getBoundingClientRect();
      
      const x = instRect.left + instRect.width/2 - retinaRect.left;
      const y = instRect.top + instRect.height/2 - retinaRect.top;
      
      const syntheticEvent = { 
        clientX: retinaRect.left + x, 
        clientY: retinaRect.top + y,
        preventDefault: () => {}
      };
      
      if(activeInstrument === 'laser-probe') {
        laserFunction(syntheticEvent);
      } else if(activeInstrument === 'vitrectome') {
        vitrectomyFunction(syntheticEvent);
      }
    });

    /* ================== FUNCIONES DE INSTRUMENTOS ================== */
    function laserFunction(e) {
      const retina = document.getElementById('retina');
      const rect = retina.getBoundingClientRect();
      
      // Crear efecto de láser
      const laserSpot = document.createElement('div');
      laserSpot.className = 'laser-spot';
      laserSpot.style.left = (e.clientX - rect.left - 12) + 'px';
      laserSpot.style.top = (e.clientY - rect.top - 12) + 'px';
      retina.appendChild(laserSpot);
      
      // Crear marca de quemadura
      const burnMark = document.createElement('div');
      burnMark.className = 'laser-burn';
      burnMark.style.left = (e.clientX - rect.left - 3) + 'px';
      burnMark.style.top = (e.clientY - rect.top - 3) + 'px';
      retina.appendChild(burnMark);
      
      // Afectar parámetros
      iop += 0.5;
      perfusion -= 0.2;
      
      // Eliminar después de la animación
      setTimeout(() => {
        laserSpot.remove();
        burnMark.remove();
      }, 2500);
    }

    function vitrectomyFunction(e) {
      const retina = document.getElementById('retina');
      const rect = retina.getBoundingClientRect();
      
      // Crear partículas de vítreo
      for (let i = 0; i < 8; i++) {
        const particle = document.createElement('div');
        particle.className = 'vitreous-particle';
        particle.style.left = (e.clientX - rect.left + (Math.random()*20 - 10)) + 'px';
        particle.style.top = (e.clientY - rect.top + (Math.random()*20 - 10)) + 'px';
        particle.style.setProperty('--tx', Math.random()*40 - 20);
        particle.style.setProperty('--ty', Math.random()*40 - 20);
        retina.appendChild(particle);
        
        setTimeout(() => particle.remove(), 1500);
      }
      
      // Actualizar progreso y parámetros
      vitreousRemoved = Math.min(100, vitreousRemoved + 0.8);
      iop -= 0.3;
      flowRate += 0.1;
    }

    function performInjection() {
      const retina = document.getElementById('retina');
      retina.style.background = `
        radial-gradient(circle at 35% 45%, rgba(100,150,255,0.2) 0%, rgba(50,100,255,0.3) 70%, rgba(20,50,255,0.2) 100%),
        repeating-linear-gradient(45deg, rgba(100,150,255,0.1) 0px, rgba(100,150,255,0.1) 1px, transparent 1px, transparent 10px)`;
      
      // Crear burbujas de inyección
      for (let i = 0; i < 20; i++) {
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
        }, i*200);
      }
      
      // Ajustar parámetros
      iop += 2;
      flowRate = 30;
      perfusion += 1;
    }

    /* ================== NUEVO SISTEMA DE PEELING ================== */
    function initPeeling() {
      const peelingContainer = document.getElementById('peeling-container');
      const retinaCanvas = document.getElementById('retina-canvas');
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      
      // Configurar canvas
      retinaCanvas.width = retinaRect.width;
      retinaCanvas.height = retinaRect.height;
      retinaModel.center = { x: retinaCanvas.width/2, y: retinaCanvas.height/2 };
      retinaModel.radius = retinaRect.width * 0.4;
      
      // Mostrar contenedor de peeling
      peelingContainer.style.display = 'block';
      
      // Inicializar modelos
      createRetinaModel();
      createMembraneModel();
      setupPeelingEventListeners();
      updatePeelingHUD();
      animatePeeling();
    }

    function endPeeling() {
      const peelingContainer = document.getElementById('peeling-container');
      peelingContainer.style.display = 'none';
      
      // Limpiar complicaciones
      document.querySelectorAll('.complication').forEach(el => el.remove());
      document.querySelectorAll('.membrane-flap').forEach(el => el.remove());
      
      // Restablecer estado
      peelingState = {
        step: 'ready',
        instrument: 'forceps',
        traction: 0,
        complication: false,
        membraneDetached: false,
        flapCreated: false,
        flapPosition: { x: 0, y: 0 }
      };
      
      // Actualizar OCT
      peelMLI_OCT();
      
      // Restaurar parámetros después del peeling
      setTimeout(() => {
        perfusion += 2;
        iop -= 1;
      }, 1000);
    }

    function createRetinaModel() {
      retinaModel.points = [];
      for(let angle = 0; angle < Math.PI * 2; angle += 0.02) {
        const radiusVariation = Math.sin(angle * 10) * 5 + Math.cos(angle * 3) * 8;
        retinaModel.points.push({
          x: retinaModel.center.x + (retinaModel.radius + radiusVariation) * Math.cos(angle),
          y: retinaModel.center.y + (retinaModel.radius + radiusVariation) * Math.sin(angle),
          originalX: retinaModel.center.x + (retinaModel.radius + radiusVariation) * Math.cos(angle),
          originalY: retinaModel.center.y + (retinaModel.radius + radiusVariation) * Math.sin(angle)
        });
      }
    }
    
    function createMembraneModel() {
      membraneModel.points = [];
      membraneModel.flapPoints = [];
      const baseRadius = retinaModel.radius * 0.7;
      
      for(let angle = 0; angle < Math.PI * 2; angle += 0.01) {
        const radiusVariation = Math.sin(angle * 15) * 3 + Math.cos(angle * 7) * 5;
        const radius = baseRadius + radiusVariation;
        
        membraneModel.points.push({
          x: retinaModel.center.x + radius * Math.cos(angle),
          y: retinaModel.center.y + radius * Math.sin(angle),
          originalX: retinaModel.center.x + radius * Math.cos(angle),
          originalY: retinaModel.center.y + radius * Math.sin(angle),
          detached: false,
          adhesion: 0.5 + Math.random() * 0.5,
          thickness: 0.8 + Math.random() * 0.4,
          flap: false
        });
      }
    }
    
    function drawPeelingScene() {
      const canvas = document.getElementById('retina-canvas');
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      
      // Dibujar retina
      drawRetinaModel(ctx);
      
      // Dibujar membrana
      drawMembraneModel(ctx);
      
      // Dibujar instrumentos
      drawPeelingInstruments(ctx);
      
      // Efectos de complicaciones
      if(peelingState.complication) {
        drawComplications(ctx);
      }
    }
    
    function drawRetinaModel(ctx) {
      // Dibujar fondo transparente para ver la retina original
      ctx.globalAlpha = 0.5;
      ctx.fillStyle = 'rgba(0,0,0,0)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.globalAlpha = 1.0;
    }
    
    function drawMembraneModel(ctx) {
      // Membrana intacta
      ctx.beginPath();
      let startedPath = false;
      
      membraneModel.points.forEach((point, index) => {
        if(!point.detached && !point.flap) {
          if(!startedPath) {
            ctx.moveTo(point.x, point.y);
            startedPath = true;
          } else {
            ctx.lineTo(point.x, point.y);
          }
        }
      });
      
      ctx.closePath();
      ctx.fillStyle = peelingState.step === 'ready' ? 'rgba(200, 140, 140, 0.4)' : 'rgba(100, 160, 220, 0.3)';
      ctx.strokeStyle = peelingState.step === 'ready' ? 'rgba(220, 160, 160, 0.8)' : 'rgba(150, 200, 255, 0.8)';
      ctx.lineWidth = 1.2;
      ctx.fill();
      ctx.stroke();
      
      // Estrías y pliegues
      ctx.beginPath();
      for(let i = 0; i < membraneModel.points.length; i += 15) {
        const point = membraneModel.points[i];
        if(!point.detached && !point.flap) {
          const nextIdx = (i + 20) % membraneModel.points.length;
          const nextPoint = membraneModel.points[nextIdx];
          
          if(!nextPoint.detached && !nextPoint.flap) {
            ctx.moveTo(point.x, point.y);
            ctx.lineTo(
              point.x + (nextPoint.x - point.x) * 0.4,
              point.y + (nextPoint.y - point.y) * 0.4
            );
            
            if(Math.random() > 0.7) {
              ctx.moveTo(point.x, point.y);
              ctx.lineTo(
                point.x + (Math.random() - 0.5) * 5,
                point.y + (Math.random() - 0.5) * 5
              );
            }
          }
        }
      }
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.3)';
      ctx.lineWidth = 0.7;
      ctx.stroke();
      
      // Bordes despegados
      membraneModel.points.forEach((point, index) => {
        if(point.detached || point.flap) {
          const prevIdx = (index - 1 + membraneModel.points.length) % membraneModel.points.length;
          const nextIdx = (index + 1) % membraneModel.points.length;
          const prevPoint = membraneModel.points[prevIdx];
          const nextPoint = membraneModel.points[nextIdx];
          
          if((!prevPoint.detached && !prevPoint.flap) || (!nextPoint.detached && !nextPoint.flap)) {
            ctx.beginPath();
            ctx.moveTo(point.x, point.y);
            
            if(!prevPoint.detached && !prevPoint.flap) {
              ctx.lineTo(prevPoint.x, prevPoint.y);
            } else {
              ctx.lineTo(point.x - 5, point.y - 5);
            }
            
            if(!nextPoint.detached && !nextPoint.flap) {
              ctx.lineTo(nextPoint.x, nextPoint.y);
            } else {
              ctx.lineTo(point.x + 5, point.y + 5);
            }
            
            ctx.closePath();
            ctx.fillStyle = point.flap ? 'rgba(180, 220, 255, 0.6)' : 'rgba(150, 200, 255, 0.5)';
            ctx.fill();
            
            if(point.flap) {
              ctx.beginPath();
              ctx.arc(point.x, point.y, 3, 0, Math.PI * 2);
              ctx.fillStyle = 'rgba(220, 240, 255, 0.8)';
              ctx.fill();
            }
          }
        }
      });
      
      // Dibujar la solapa de membrana si existe
      if(peelingState.flapCreated) {
        ctx.beginPath();
        let flapStarted = false;
        
        membraneModel.points.forEach(point => {
          if(point.flap) {
            if(!flapStarted) {
              ctx.moveTo(point.x, point.y);
              flapStarted = true;
            } else {
              ctx.lineTo(point.x, point.y);
            }
          }
        });
        
        ctx.closePath();
        ctx.fillStyle = 'rgba(180, 220, 255, 0.4)';
        ctx.strokeStyle = 'rgba(200, 230, 255, 0.8)';
        ctx.lineWidth = 1;
        ctx.fill();
        ctx.stroke();
        
        // Efecto de pliegues en la solapa
        ctx.beginPath();
        membraneModel.points.forEach(point => {
          if(point.flap && Math.random() > 0.7) {
            ctx.moveTo(point.x, point.y);
            ctx.lineTo(
              point.x + (Math.random() - 0.5) * 15,
              point.y + (Math.random() - 0.5) * 15
            );
          }
        });
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.4)';
        ctx.lineWidth = 0.5;
        ctx.stroke();
      }
    }
    
    function drawPeelingInstruments(ctx) {
      // Pinzas de peeling
      if(peelingState.instrument === 'forceps') {
        ctx.save();
        ctx.translate(peelingInstruments.forceps.position.x, peelingInstruments.forceps.position.y);
        ctx.rotate(peelingInstruments.forceps.angle);
        
        // Cuerpo
        ctx.beginPath();
        ctx.moveTo(0, -5);
        ctx.lineTo(-15, -15);
        ctx.lineTo(-20, -10);
        
        // Punta (varía según si está agarrando)
        if(peelingInstruments.forceps.grasping) {
          ctx.lineTo(-5, 25);
          ctx.lineTo(5, 25);
        } else {
          ctx.lineTo(-5, 20);
          ctx.lineTo(5, 20);
        }
        
        ctx.lineTo(20, -10);
        ctx.lineTo(15, -15);
        ctx.closePath();
        
        ctx.fillStyle = peelingInstruments.forceps.grasping ? 'rgba(255, 255, 150, 0.9)' : 'rgba(220, 220, 220, 0.9)';
        ctx.fill();
        ctx.strokeStyle = 'rgba(100, 100, 100, 0.8)';
        ctx.lineWidth = 1;
        ctx.stroke();
        
        // Efecto de agarre
        if(peelingInstruments.forceps.grasping) {
          ctx.beginPath();
          ctx.arc(0, 25, 3, 0, Math.PI * 2);
          ctx.fillStyle = 'rgba(255, 255, 180, 0.9)';
          ctx.fill();
        }
        
        ctx.restore();
      }
      
      // Gancho romo
      if(peelingState.instrument === 'hook') {
        ctx.beginPath();
        ctx.moveTo(peelingInstruments.hook.position.x - 3, peelingInstruments.hook.position.y - 20);
        ctx.lineTo(peelingInstruments.hook.position.x + 3, peelingInstruments.hook.position.y - 20);
        ctx.lineTo(peelingInstruments.hook.position.x + 2, peelingInstruments.hook.position.y + 25);
        ctx.lineTo(peelingInstruments.hook.position.x - 2, peelingInstruments.hook.position.y + 25);
        ctx.closePath();
        
        // Punta roma
        ctx.moveTo(peelingInstruments.hook.position.x, peelingInstruments.hook.position.y + 25);
        ctx.arc(peelingInstruments.hook.position.x, peelingInstruments.hook.position.y + 35, 10, Math.PI, 0, true);
        
        ctx.fillStyle = 'rgba(200, 200, 200, 0.9)';
        ctx.fill();
        ctx.strokeStyle = 'rgba(100, 100, 100, 0.8)';
        ctx.lineWidth = 1;
        ctx.stroke();
      }
    }
    
    function drawComplications(ctx) {
      // Sangrado
      for(let i = 0; i < 3; i++) {
        ctx.beginPath();
        const bleedX = retinaModel.center.x + (Math.random() - 0.5) * 100;
        const bleedY = retinaModel.center.y + (Math.random() - 0.5) * 100;
        const bleedSize = 10 + Math.random() * 40;
        
        ctx.arc(bleedX, bleedY, bleedSize, 0, Math.PI * 2);
        
        const bleedGradient = ctx.createRadialGradient(
          bleedX, bleedY, 0,
          bleedX, bleedY, bleedSize
        );
        bleedGradient.addColorStop(0, 'rgba(180, 0, 0, 0.8)');
        bleedGradient.addColorStop(1, 'rgba(120, 0, 0, 0.2)');
        
        ctx.fillStyle = bleedGradient;
        ctx.fill();
        
        // Pequeños coágulos
        if(Math.random() > 0.5) {
          ctx.beginPath();
          ctx.arc(
            bleedX + (Math.random() - 0.5) * bleedSize * 0.7,
            bleedY + (Math.random() - 0.5) * bleedSize * 0.7,
            2 + Math.random() * 4,
            0,
            Math.PI * 2
          );
          ctx.fillStyle = 'rgba(100, 0, 0, 0.9)';
          ctx.fill();
        }
      }
    }
    
    function updatePeeling() {
      if(peelingState.instrument === 'forceps' && peelingInstruments.forceps.grasping) {
        let totalForce = 0;
        let affectedPoints = 0;
        
        // Crear solapa si no existe y hay suficiente tracción
        if(!peelingState.flapCreated && peelingState.traction > 30 && Math.random() < 0.05) {
          createFlap();
        }
        
        membraneModel.points.forEach((point, index) => {
          // Calcular distancia al instrumento
          const dx = point.x - peelingInstruments.forceps.position.x;
          const dy = point.y - peelingInstruments.forceps.position.y;
          const distance = Math.sqrt(dx * dx + dy * dy);
          
          if(distance < 30 && !point.detached && !point.flap) {
            // Calcular dirección de tracción (tangencial a la retina)
            const angleToCenter = Math.atan2(
              retinaModel.center.y - point.y,
              retinaModel.center.x - point.x
            );
            const tangentAngle = angleToCenter + Math.PI/2;
            
            // Calcular fuerza basada en distancia, adherencia y si hay solapa
            const pullForce = (1 - distance/30) * (1 - point.adhesion) * (peelingState.flapCreated ? 1.5 : 1) * 2;
            totalForce += pullForce;
            affectedPoints++;
            
            // Aplicar movimiento tangencial con variación aleatoria
            const angleVariation = (Math.random() - 0.5) * 0.2;
            point.x += Math.cos(tangentAngle + angleVariation) * pullForce * 3;
            point.y += Math.sin(tangentAngle + angleVariation) * pullForce * 3;
            
            // Verificar si se despegó
            const originalDistance = Math.sqrt(
              Math.pow(point.x - point.originalX, 2) + 
              Math.pow(point.y - point.originalY, 2)
            );
            
            if(originalDistance > 15 + (point.adhesion * 20)) {
              if(peelingState.flapCreated) {
                point.flap = true;
                membraneModel.flapPoints.push(point);
              } else {
                point.detached = true;
                membraneModel.detachedPoints++;
              }
              
              // Efecto de desprendimiento
              gsap.to(point, {
                x: point.x + (Math.random() - 0.5) * 10,
                y: point.y + (Math.random() - 0.5) * 10,
                duration: 0.3
              });
            }
            
            // Propagación a puntos adyacentes con efecto de onda
            const prevIndex = (index - 1 + membraneModel.points.length) % membraneModel.points.length;
            const nextIndex = (index + 1) % membraneModel.points.length;
            
            [prevIndex, nextIndex].forEach((adjIndex, i) => {
              const adjPoint = membraneModel.points[adjIndex];
              if(!adjPoint.detached && !adjPoint.flap) {
                const distanceFactor = 1 - (i * 0.3);
                adjPoint.x += Math.cos(tangentAngle) * pullForce * 0.5 * distanceFactor;
                adjPoint.y += Math.sin(tangentAngle) * pullForce * 0.5 * distanceFactor;
              }
            });
          }
          
          // Elasticidad: intentar volver a posición original si no está agarrado
          if(!point.detached && !point.flap && distance > 35) {
            const returnSpeed = 0.05 * point.adhesion;
            point.x += (point.originalX - point.x) * returnSpeed;
            point.y += (point.originalY - point.y) * returnSpeed;
          }
          
          // Mover puntos de la solapa con el instrumento
          if(point.flap && distance < 50) {
            const followFactor = 0.1 + (1 - distance/50) * 0.4;
            point.x += (peelingInstruments.forceps.position.x - point.x) * followFactor;
            point.y += (peelingInstruments.forceps.position.y - point.y) * followFactor;
          }
        });
        
        // Actualizar fuerza de tracción
        if(affectedPoints > 0) {
          peelingState.traction = (totalForce / affectedPoints) * 100;
          
          // Complicación por tracción excesiva con probabilidad variable
          if(peelingState.traction > 70) {
            const complicationChance = 0.02 + (peelingState.traction - 70) * 0.01;
            if(Math.random() < complicationChance) {
              causePeelingComplication();
            }
          }
        } else {
          peelingState.traction = 0;
        }
        
        // Verificar si la membrana está completamente despegada
        if((membraneModel.detachedPoints + membraneModel.flapPoints.length) > membraneModel.points.length * 0.9) {
          peelingState.step = 'complete';
          updatePeelingHUD("¡Membrana removida exitosamente!");
        }
      }
    }
    
    function createFlap() {
      peelingState.flapCreated = true;
      peelingState.flapPosition = { 
        x: peelingInstruments.forceps.position.x, 
        y: peelingInstruments.forceps.position.y 
      };
      
      // Crear elemento visual para la solapa
      const flap = document.createElement('div');
      flap.className = 'membrane-flap';
      flap.style.width = '100px';
      flap.style.height = '100px';
      flap.style.left = `${peelingState.flapPosition.x - 50}px`;
      flap.style.top = `${peelingState.flapPosition.y - 50}px`;
      document.getElementById('peeling-container').appendChild(flap);
      
      // Marcar puntos cercanos como parte de la solapa
      membraneModel.points.forEach(point => {
        const dx = point.x - peelingState.flapPosition.x;
        const dy = point.y - peelingState.flapPosition.y;
        const distance = Math.sqrt(dx * dx + dy * dy);
        
        if(distance < 60 && !point.detached) {
          point.flap = true;
          membraneModel.flapPoints.push(point);
          
          // Efecto visual inicial
          gsap.to(point, {
            x: point.x + (Math.random() - 0.5) * 15,
            y: point.y + (Math.random() - 0.5) * 15,
            duration: 0.5
          });
        }
      });
      
      updatePeelingHUD("Solapa de membrana creada. Continúe el peeling con cuidado.");
    }
    
    function causePeelingComplication() {
      peelingState.complication = true;
      
      // Crear efecto de sangrado
      for(let i = 0; i < 8; i++) {
        const blood = document.createElement('div');
        blood.className = 'complication';
        const size = 10 + Math.random() * 40;
        blood.style.width = `${size}px`;
        blood.style.height = `${size}px`;
        blood.style.left = `${peelingInstruments.forceps.position.x + (Math.random() - 0.5) * 120}px`;
        blood.style.top = `${peelingInstruments.forceps.position.y + (Math.random() - 0.5) * 120}px`;
        blood.style.opacity = '0.8';
        document.getElementById('peeling-container').appendChild(blood);
        
        // Animación de sangrado con GSAP
        gsap.to(blood, {
          width: `${size * 1.5}px`,
          height: `${size * 1.5}px`,
          left: `${parseFloat(blood.style.left) + (Math.random() - 0.5) * 30}px`,
          top: `${parseFloat(blood.style.top) + (Math.random() - 0.5) * 30}px`,
          opacity: 0,
          duration: 3 + Math.random() * 3,
          onComplete: () => blood.remove()
        });
      }
      
      // Añadir puntos de desgarro en la retina
      for(let i = 0; i < 5; i++) {
        const idx = Math.floor(Math.random() * retinaModel.points.length);
        const point = retinaModel.points[idx];
        
        gsap.to(point, {
          x: point.x + (Math.random() - 0.5) * 30,
          y: point.y + (Math.random() - 0.5) * 30,
          duration: 0.5
        });
      }
      
      // Afectar parámetros vitales
      iop += 5;
      perfusion -= 15;
      
      updatePeelingHUD("¡COMPLICACIÓN! Desgarro retiniano detectado");
    }
    
    function updatePeelingHUD(message) {
      const statusEl = document.getElementById('procedure-status');
      const tractionEl = document.getElementById('traction-level');
      const warningEl = document.getElementById('traction-warning');
      
      tractionEl.style.width = `${peelingState.traction}%`;
      
      if(peelingState.traction > 70) {
        warningEl.style.display = 'block';
        tractionEl.style.background = 'linear-gradient(90deg, #ff0000, #ff0000)';
      } else {
        warningEl.style.display = 'none';
        tractionEl.style.background = 'linear-gradient(90deg, #00ff00, #ffcc00, #ff0000)';
      }
      
      switch(peelingState.step) {
        case 'ready':
          statusEl.textContent = message || "Identifique el borde de la membrana con las pinzas";
          break;
        case 'grasping':
          statusEl.textContent = message || "Agarre establecido. Aplique tracción TANGENCIAL suave";
          break;
        case 'peeling':
          const progress = Math.round((membraneModel.detachedPoints + membraneModel.flapPoints.length)/membraneModel.points.length*100);
          statusEl.textContent = message || `Peeling en progreso (${progress}% completado)`;
          break;
        case 'complete':
          statusEl.textContent = message || "¡Procedimiento completado con éxito!";
          break;
      }
      
      if(peelingState.complication) {
        statusEl.innerHTML = "<span style='color:#ff0000'>¡COMPLICACIÓN! Desgarro retiniano - Suspenda la tracción</span>";
      }
      
      if(peelingState.flapCreated) {
        statusEl.innerHTML += "<br><span style='color:#66ccff'>Solapa de membrana identificada</span>";
      }
    }
    
    function setupPeelingEventListeners() {
      // Selector de instrumentos
      document.getElementById('forceps-btn').addEventListener('click', () => {
        peelingState.instrument = 'forceps';
        document.getElementById('forceps-btn').classList.add('active');
        document.getElementById('hook-btn').classList.remove('active');
        document.getElementById('scissors-btn').classList.remove('active');
        updatePeelingHUD();
      });
      
      document.getElementById('hook-btn').addEventListener('click', () => {
        peelingState.instrument = 'hook';
        document.getElementById('forceps-btn').classList.remove('active');
        document.getElementById('hook-btn').classList.add('active');
        document.getElementById('scissors-btn').classList.remove('active');
        updatePeelingHUD("Usando gancho romo para liberar adherencias");
      });
      
      document.getElementById('scissors-btn').addEventListener('click', () => {
        peelingState.instrument = 'scissors';
        document.getElementById('forceps-btn').classList.remove('active');
        document.getElementById('hook-btn').classList.remove('active');
        document.getElementById('scissors-btn').classList.add('active');
        updatePeelingHUD("Tijeras vitreorretinianas seleccionadas");
      });
      
      // Control de instrumentos con mouse/touch
      const canvas = document.getElementById('retina-canvas');
      canvas.addEventListener('mousedown', handlePeelingStart);
      canvas.addEventListener('touchstart', handlePeelingStart);
      
      canvas.addEventListener('mousemove', handlePeelingMove);
      canvas.addEventListener('touchmove', handlePeelingMove);
      
      canvas.addEventListener('mouseup', handlePeelingEnd);
      canvas.addEventListener('touchend', handlePeelingEnd);
    }
    
    function handlePeelingStart(e) {
      e.preventDefault();
      const pos = getPeelingPosition(e);
      
      if(peelingState.instrument === 'forceps') {
        peelingInstruments.forceps.position = pos;
        peelingInstruments.forceps.grasping = true;
        peelingInstruments.forceps.gripPosition = pos;
        
        // Calcular ángulo hacia el centro
        const dx = retinaModel.center.x - pos.x;
        const dy = retinaModel.center.y - pos.y;
        peelingInstruments.forceps.angle = Math.atan2(dy, dx) - Math.PI/2;
        
        if(peelingState.step === 'ready') {
          peelingState.step = 'grasping';
          updatePeelingHUD();
        }
      } else if(peelingState.instrument === 'hook') {
        peelingInstruments.hook.position = pos;
      } else if(peelingState.instrument === 'scissors') {
        peelingInstruments.scissors.position = pos;
      }
    }
    
    function handlePeelingMove(e) {
      if((peelingState.instrument === 'forceps' && !peelingInstruments.forceps.grasping) || 
         peelingState.step === 'complete') return;
         
      e.preventDefault();
      const pos = getPeelingPosition(e);
      
      if(peelingState.instrument === 'forceps') {
        peelingInstruments.forceps.position = pos;
        
        // Calcular ángulo hacia el centro
        const dx = retinaModel.center.x - pos.x;
        const dy = retinaModel.center.y - pos.y;
        peelingInstruments.forceps.angle = Math.atan2(dy, dx) - Math.PI/2;
        
        if(peelingState.step === 'grasping') {
          peelingState.step = 'peeling';
        }
        
        updatePeeling();
      } else if(peelingState.instrument === 'hook') {
        peelingInstruments.hook.position = pos;
        
        // Lógica del gancho para liberar adherencias
        membraneModel.points.forEach(point => {
          if(!point.detached && !point.flap) {
            const dx = point.x - pos.x;
            const dy = point.y - pos.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if(distance < 25) {
              // Reducir adherencia donde se aplica el gancho
              point.adhesion = Math.max(0.1, point.adhesion - 0.05);
              
              // Empujar ligeramente el punto con variación
              const angle = Math.atan2(dy, dx);
              const variation = (Math.random() - 0.5) * 0.2;
              point.x += Math.cos(angle + variation) * 1.2;
              point.y += Math.sin(angle + variation) * 1.2;
            }
          }
        });
      } else if(peelingState.instrument === 'scissors') {
        peelingInstruments.scissors.position = pos;
        
        // Lógica de tijeras para cortar la membrana
        if(peelingState.flapCreated) {
          membraneModel.flapPoints.forEach(point => {
            const dx = point.x - pos.x;
            const dy = point.y - pos.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if(distance < 20) {
              point.detached = true;
              point.flap = false;
              membraneModel.detachedPoints++;
              
              // Efecto de corte
              gsap.to(point, {
                x: point.x + (Math.random() - 0.5) * 30,
                y: point.y + (Math.random() - 0.5) * 30,
                duration: 0.3
              });
            }
          });
        }
      }
    }
    
    function handlePeelingEnd() {
      if(peelingState.instrument === 'forceps') {
        peelingInstruments.forceps.grasping = false;
        peelingState.traction = 0;
      }
      
      updatePeelingHUD();
    }
    
    function getPeelingPosition(e) {
      const canvas = document.getElementById('retina-canvas');
      const rect = canvas.getBoundingClientRect();
      let clientX, clientY;
      
      if(e.touches) {
        clientX = e.touches[0].clientX;
        clientY = e.touches[0].clientY;
      } else {
        clientX = e.clientX;
        clientY = e.clientY;
      }
      
      return {
        x: clientX - rect.left,
        y: clientY - rect.top
      };
    }
    
    function animatePeeling() {
      drawPeelingScene();
      updatePeelingHUD();
      requestAnimationFrame(animatePeeling);
    }

    function peelMLI_OCT() {
      const mliGroup = document.getElementById('mli-group');
      if(mliGroup) {
        mliGroup.classList.add('peel-remove');
        setTimeout(() => {
          if(mliGroup.parentNode) mliGroup.parentNode.removeChild(mliGroup);
        }, 1000);
      }
    }

    /* ================== CONTROL DE JOYSTICKS ================== */
    function initJoystick(joystickElement, updateCallback) {
      const handle = joystickElement.querySelector('.joystick-handle');
      const rect = joystickElement.getBoundingClientRect();
      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const maxDistance = rect.width / 2;
      let isTouching = false;
      
      function handleStart(e) {
        e.preventDefault();
        isTouching = true;
        handleMove(e);
      }
      
      function handleMove(e) {
        if (!isTouching) return;
        
        const clientX = e.clientX || (e.touches && e.touches[0].clientX);
        const clientY = e.clientY || (e.touches && e.touches[0].clientY);
        
        if(!clientX || !clientY) return;
        
        const bounds = joystickElement.getBoundingClientRect();
        const x = clientX - bounds.left;
        const y = clientY - bounds.top;
        
        let deltaX = x - centerX;
        let deltaY = y - centerY;
        const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY);
        
        if (distance > maxDistance) {
          const angle = Math.atan2(deltaY, deltaX);
          deltaX = Math.cos(angle) * maxDistance;
          deltaY = Math.sin(angle) * maxDistance;
        }
        
        handle.style.transform = `translate(${deltaX}px, ${deltaY}px)`;
        
        const normalizedX = ((deltaX + maxDistance) / (2 * maxDistance)) * 100;
        const normalizedY = ((deltaY + maxDistance) / (2 * maxDistance)) * 100;
        
        updateCallback(normalizedX, normalizedY);
      }
      
      function handleEnd() {
        isTouching = false;
        handle.style.transform = `translate(0px, 0px)`;
        updateCallback(50, 50);
      }
      
      // Eventos táctiles
      joystickElement.addEventListener('touchstart', handleStart, { passive: false });
      joystickElement.addEventListener('touchmove', handleMove, { passive: false });
      joystickElement.addEventListener('touchend', handleEnd);
      
      // Eventos de ratón para compatibilidad
      joystickElement.addEventListener('mousedown', handleStart);
      document.addEventListener('mousemove', handleMove);
      document.addEventListener('mouseup', handleEnd);
    }

    // Inicializar joysticks
    const joystickVitrectomo = document.getElementById('joystick-vitrectomo');
    initJoystick(joystickVitrectomo, (x, y) => {
      vitrectomoJoystickX = x;
      vitrectomoJoystickY = y;
      updateVitrectomoPosition(x, y);
      updateMiniLeftLine(x, y);
    });
    
    const joystickLight = document.getElementById('joystick-light');
    initJoystick(joystickLight, (x, y) => {
      lightJoystickX = x;
      lightJoystickY = y;
      updateEndoLightEffect(x, y);
      updateMiniRightLine(x, y);
    });

    /* ================== ACTUALIZACIÓN DE POSICIONES ================== */
    function updateVitrectomoPosition(normX, normY) {
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const maxOffset = retinaRect.width / 2;
      const offsetX = (normX - 50) / 50 * maxOffset * 0.9;
      const offsetY = (normY - 50) / 50 * maxOffset * 0.9;
      
      if(activeInstrument) {
        const instr = document.getElementById(activeInstrument);
        if(instr) {
          instr.style.transform = `translate(calc(-50% + ${offsetX}px), calc(-50% + ${offsetY}px)) translateZ(${currentDepth}px)`;
        }
      }
      
      // Actualizar posición del peeling si está activo
      if(activeInstrument === 'peeling' && peelingInstruments[peelingState.instrument]) {
        peelingInstruments[peelingState.instrument].position = {
          x: retinaRect.left + retinaRect.width/2 + offsetX,
          y: retinaRect.top + retinaRect.height/2 + offsetY
        };
      }
    }

    function updateEndoLightEffect(normX, normY) {
      document.documentElement.style.setProperty('--light-x', normX + '%');
      document.documentElement.style.setProperty('--light-y', normY + '%');
      
      const zVal = parseInt(document.getElementById('endo-z-slider').value);
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const newLightSize = 20 + (retinaRect.width - 20) * (zVal / 200);
      
      document.documentElement.style.setProperty('--light-size', newLightSize + 'px');
      document.getElementById('light-reflection').style.opacity = 0.7;
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
      if(miniLeft && miniLeftInner) {
        miniLeft.setAttribute('x2', tipX);
        miniLeftInner.setAttribute('x2', tipX);
        miniLeft.setAttribute('y2', tipY);
        miniLeftInner.setAttribute('y2', tipY);
      }
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
      if(miniRight && miniRightInner) {
        miniRight.setAttribute('x2', tipX);
        miniRightInner.setAttribute('x2', tipX);
        miniRight.setAttribute('y2', tipY);
        miniRightInner.setAttribute('y2', tipY);
      }
    }

    /* ================== ACTUALIZACIÓN DE PARÁMETROS ================== */
    function updateVitals() {
      // Variación aleatoria de los parámetros
      iop += (Math.random() - 0.5) * 0.1;
      perfusion += (Math.random() - 0.5) * 0.2;
      flowRate += (Math.random() - 0.5) * 0.1;
      
      // Limitar valores
      iop = Math.max(10, Math.min(30, iop));
      perfusion = Math.max(60, Math.min(100, perfusion));
      vitreousRemoved = Math.max(0, Math.min(100, vitreousRemoved));
      flowRate = Math.max(20, Math.min(35, flowRate));
      
      // Actualizar visualización
      document.getElementById('iop-value').innerText = iop.toFixed(1) + " mmHg";
      document.getElementById('iop-level').style.width = ((iop - 10) / 20 * 100) + "%";
      
      document.getElementById('perfusion-value').innerText = perfusion.toFixed(0) + "%";
      document.getElementById('perfusion-level').style.width = perfusion + "%";
      
      document.getElementById('vitreous-value').innerText = vitreousRemoved.toFixed(0) + "%";
      document.getElementById('vitreous-level').style.width = vitreousRemoved + "%";
      
      document.getElementById('flow-value').innerText = flowRate.toFixed(1) + " ml/min";
      
      // Actualizar clase según presión
      const iopElement = document.getElementById('iop-value');
      iopElement.classList.remove('normal', 'warning', 'danger');
      
      if(iop < 15 || iop > 25) {
        iopElement.classList.add('danger');
        retinaStatus = 'Riesgo';
      } else if(iop < 18 || iop > 22) {
        iopElement.classList.add('warning');
        retinaStatus = 'Alerta';
      } else {
        iopElement.classList.add('normal');
        retinaStatus = 'Estable';
      }
      
      // Actualizar estado de la retina
      const retinaStatusElement = document.getElementById('retina-status');
      retinaStatusElement.innerText = retinaStatus;
      retinaStatusElement.className = `vital-value ${iopElement.className.split(' ')[1]}`;
      
      // Efectos de la perfusión en la retina
      if(perfusion < 70) {
        retinaStatusElement.classList.remove('normal', 'warning');
        retinaStatusElement.classList.add('danger');
        retinaStatus = 'Isquemia';
      } else if(perfusion < 85) {
        retinaStatusElement.classList.remove('normal', 'danger');
        retinaStatusElement.classList.add('warning');
        retinaStatus = 'Hipoperfusión';
      }
    }

    // Actualizar parámetros cada segundo
    setInterval(updateVitals, 1000);

    // Configurar sliders
    document.getElementById('vitrectomo-z-slider').addEventListener('input', function() {
      currentDepth = parseInt(this.value);
      updateVitrectomoPosition(vitrectomoJoystickX, vitrectomoJoystickY);
    });
    
    document.getElementById('endo-z-slider').addEventListener('input', function() {
      updateEndoLightEffect(lightJoystickX, lightJoystickY);
    });

    // Inicialización
    updateVitals();
    updateEndoLightEffect(50, 50);
  </script>
</body>
</html>
