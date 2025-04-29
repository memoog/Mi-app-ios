
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Quirúrgico Retiniano - Extracción Cuerpo Extraño</title>
    <link rel="stylesheet" href="css.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
    <style>
      
/* Estilos base */
body {
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #0f172a;
  color: white;
  overflow: hidden;
  touch-action: none;
  height: 100vh;
  width: 100vw;
}

#container {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

/* Caso clínico */
#clinical-case {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.5s;
}

#clinical-case-content {
  max-width: 800px;
  padding: 20px;
  background-color: #1e293b;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
}

#clinical-case h2 {
  color: #38bdf8;
  margin-top: 0;
}

#clinical-case button {
  background-color: #38bdf8;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  margin-top: 20px;
  transition: background-color 0.3s;
}

#clinical-case button:hover {
  background-color: #0ea5e9;
}

/* Sistema de alertas */
#alert-system {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 50;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.alert-container {
  background-color: #1e293b;
  border-left: 5px solid;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  display: none;
  max-width: 350px;
  animation: slideIn 0.3s ease-out;
  transform: translateY(0);
  opacity: 1;
}

.alert-icon {
  font-size: 1.5rem;
  margin-right: 10px;
}

.alert-content {
  flex: 1;
}

.alert-content h3 {
  margin: 0 0 5px 0;
  font-size: 1rem;
}

.alert-content p {
  margin: 0;
  font-size: 0.9rem;
  color: #e2e8f0;
}

.alert-timer {
  height: 3px;
  background-color: rgba(255, 255, 255, 0.3);
  margin-top: 10px;
  border-radius: 3px;
  overflow: hidden;
}

.alert-dismiss {
  background: none;
  border: none;
  color: white;
  font-size: 1.2rem;
  cursor: pointer;
  padding: 0;
  margin-left: 10px;
}

/* Mini mapa */
#miniMapContainer {
  position: absolute;
  top: 20px;
  left: 20px;
  width: 200px;
  height: 150px;
  background-color: #1e293b;
  border-radius: 10px;
  overflow: hidden;
  z-index: 20;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

/* Panel de instrumentos */
.instrument-panel {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  background-color: #1e293b;
  padding: 10px;
  border-radius: 10px;
  z-index: 20;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

.toggle-btn {
  background-color: #334155;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.toggle-btn:hover {
  background-color: #475569;
}

.toggle-btn.active {
  background-color: #38bdf8;
  color: white;
}

/* Panel de parámetros */
.control-panel {
  position: absolute;
  top: 20px;
  right: 20px;
  background-color: #1e293b;
  padding: 15px;
  border-radius: 10px;
  z-index: 10;
  width: 200px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

.vital-sign {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  font-size: 0.9rem;
}

.vital-label {
  color: #94a3b8;
}

.vital-value {
  font-weight: bold;
}

.vital-value.normal {
  color: #4ade80;
}

.vital-value.warning {
  color: #fbbf24;
}

.vital-value.danger {
  color: #f87171;
}

.gauge-container {
  height: 6px;
  background-color: #334155;
  border-radius: 3px;
  margin-bottom: 15px;
  overflow: hidden;
}

.gauge-level {
  height: 100%;
  background-color: #38bdf8;
  width: 50%;
  transition: width 0.5s ease;
}

/* Joysticks */
.joystick-container {
  position: absolute;
  bottom: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 20;
}

#joystick-light-container {
  left: 30px;
}

#joystick-vitrectomo-container {
  right: 30px;
}

.joystick {
  width: 80px;
  height: 80px;
  background-color: #334155;
  border-radius: 50%;
  position: relative;
  margin-bottom: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

.joystick-handle {
  width: 30px;
  height: 30px;
  background-color: #38bdf8;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: transform 0.1s;
}

.slider-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.slider-container label {
  margin-bottom: 5px;
  font-size: 0.8rem;
  color: #94a3b8;
}

.slider-container input[type="range"] {
  width: 100%;
  margin-bottom: 10px;
}

.slider-container button {
  background-color: #38bdf8;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.slider-container button:hover {
  background-color: #0ea5e9;
}

/* Cámara de retina */
#eye-chamber {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 5;
}

.retina-container {
  position: relative;
  width: 80vmin;
  height: 80vmin;
  max-width: 500px;
  max-height: 500px;
}

.retina-sphere {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at center, #570000 0%, #400000 20%, #300000 40%, #200000 70%, #100000 100%);
  box-shadow: inset 0 0 50px rgba(0, 0, 0, 0.8), 0 0 60px rgba(0, 0, 0, 0.6);
  transform-style: preserve-3d;
  overflow: hidden;
}

.retina-texture {
  position: absolute;
  width: 100%;
  height: 100%;
  background: 
    radial-gradient(circle at 30% 30%, transparent 60%, rgba(0, 0, 0, 0.3) 100%),
    repeating-linear-gradient(45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 1px, transparent 1px, transparent 20px);
  border-radius: 50%;
}

.retina-nerve {
  position: absolute;
  bottom: 15%;
  left: 50%;
  transform: translateX(-50%);
  width: 15%;
  height: 8%;
  background: radial-gradient(circle, rgba(209, 85, 85, 0.8) 0%, rgba(180, 40, 40, 0.6) 100%);
  border-radius: 50% 50% 0 0;
}

.optic-disc {
  position: absolute;
  bottom: 15%;
  left: 50%;
  transform: translateX(-50%);
  width: 10%;
  height: 5%;
  background: radial-gradient(circle, rgba(206, 88, 25, 0.8) 0%, rgba(180, 40, 40, 0.6) 100%);
  border-radius: 50%;
  box-shadow: inset 0 0 20px rgba(191, 78, 78, 0.6), 0 0 25px rgba(220, 80, 80, 0.5);
}

.optic-cup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
  height: 50%;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 50%;
}

.macula {
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translateX(-50%);
  width: 8%;
  height: 8%;
  background: radial-gradient(circle, rgba(255, 255, 200, 0.3) 0%, rgba(255, 255, 150, 0.2) 50%, transparent 100%);
  border-radius: 50%;
}

.blood-vessels {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

#light-mask {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
    transparent 0%, 
    rgba(0, 0, 0, 0.8) calc(var(--light-size, 100px) * 1.5), 
    rgba(0, 0, 0, 0.9) 100%);
  pointer-events: none;
  z-index: 6;
}

#light-reflection {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
    rgba(255, 255, 255, 0.3) 0%, 
    rgba(255, 255, 255, 0.1) var(--light-size, 100px), 
    transparent calc(var(--light-size, 100px) * 1.5));
  pointer-events: none;
  z-index: 7;
  opacity: 0;
  transition: opacity 0.3s;
}

.light-scatter {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at var(--light-x, 50%) var(--light-y, 50%), 
    rgba(255, 255, 255, 0.05) 0%, 
    transparent 70%);
  pointer-events: none;
  z-index: 4;
  opacity: 0;
  transition: opacity 0.3s;
}

.light-scatter.active {
  opacity: 1;
}

.specular-highlight {
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.8) 0%, transparent 70%);
  top: calc(var(--light-y, 50%) - 15px);
  left: calc(var(--light-x, 50%) - 15px);
  pointer-events: none;
  z-index: 8;
  opacity: 0;
  transition: opacity 0.3s;
}

.specular-highlight.active {
  opacity: 0.6;
}

/* Instrumentos */
.instrument {
  position: absolute;
  width: 40px;
  height: 120px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transform-style: preserve-3d;
  display: none;
  z-index: 10;
}

.instrument-body {
  position: relative;
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, #555 0%, #777 50%, #555 100%);
  border-radius: 5px;
  transform: rotateX(75deg);
  transform-origin: bottom center;
}

.instrument-tip {
  width: 10px;
  height: 30px;
  background: linear-gradient(to right, #333 0%, #666 50%, #333 100%);
  border-radius: 0 0 5px 5px;
}

.instrument-side {
  position: absolute;
  width: 100%;
  height: 20px;
  background: linear-gradient(to right, #444 0%, #666 50%, #444 100%);
  top: 50%;
  transform: translateY(-50%) rotateX(90deg);
  transform-origin: top center;
}

.instrument-light-reflection {
  position: absolute;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    135deg,
    rgba(255,255,255,0.3) 0%,
    rgba(255,255,255,0.1) 50%,
    transparent 100%
  );
  border-radius: inherit;
  pointer-events: none;
  z-index: 3;
}

.instrument-shadow {
  position: absolute;
  width: 100%;
  height: 20px;
  background: rgba(0, 0, 0, 0.5);
  bottom: -10px;
  left: 0;
  border-radius: 50%;
  filter: blur(5px);
  transform: rotateX(75deg) translateZ(-10px);
  z-index: -1;
}

/* Efectos */
.vitreous-particle {
  position: absolute;
  width: 5px;
  height: 5px;
  background-color: rgba(200, 200, 255, 0.7);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9;
  animation: float 1.5s ease-out forwards;
}

@keyframes float {
  to {
    transform: translate(var(--tx, 0), var(--ty, 0));
    opacity: 0;
  }
}

.pfc-bubble {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(100, 255, 255, 0.8) 0%, rgba(50, 200, 200, 0.6) 70%, transparent 100%);
  pointer-events: none;
  z-index: 11;
  animation: float-up 2s ease-out forwards;
}

@keyframes float-up {
  to {
    transform: translate(var(--tx, 0), var(--ty, -100px));
    opacity: 0;
  }
}

.hemorrhage-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(180, 0, 0, 0.7) 0%, rgba(120, 0, 0, 0.5) 100%);
  pointer-events: none;
  z-index: 12;
  opacity: 0;
  display: none;
}

/* Indicador de profundidad */
.depth-indicator {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #1e293b;
  padding: 8px 15px;
  border-radius: 20px;
  font-size: 0.9rem;
  z-index: 20;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  color: var(--depth-color, #10b981);
}

/* Animaciones */
@keyframes slideIn {
  from {
    transform: translateY(-20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}
/* ================== SISTEMA DE ALERTAS PROFESIONAL ================== */
#alert-system {
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 10000;
    width: 90%;
    max-width: 600px;
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  
  .alert-container {
    display: none;
    background: rgba(20, 20, 30, 0.95);
    border-left: 5px solid;
    border-radius: 8px;
    padding: 15px 20px;
    align-items: center;
    box-shadow: 0 10px 25px rgba(0,0,0,0.5);
    backdrop-filter: blur(8px);
    animation: alertSlideIn 0.3s ease-out forwards;
    transform-origin: top center;
    border-left-color: #ff5555;
  }
  
  @keyframes alertSlideIn {
    0% { opacity: 0; transform: translateY(-20px); }
    100% { opacity: 1; transform: translateY(0); }
  }
  
  @keyframes alertSlideOut {
    0% { opacity: 1; transform: translateY(0); }
    100% { opacity: 0; transform: translateY(-20px); }
  }
  
  .alert-container#wall-collision-alert {
    border-left-color: #ff9933;
    background: rgba(40, 30, 10, 0.95);
    animation: alertShake 0.5s ease-out forwards;
  }
  
  @keyframes alertShake {
    0%, 100% { transform: translateX(0) translateY(0); }
    20% { transform: translateX(-10px) translateY(0); }
    40% { transform: translateX(10px) translateY(0); }
    60% { transform: translateX(-10px) translateY(0); }
    80% { transform: translateX(10px) translateY(0); }
  }
  
  .alert-container#vessel-damage-alert {
    border-left-color: #ff3333;
    background: rgba(40, 10, 10, 0.95);
  }
  
  .alert-container#retina-detachment-alert {
    border-left-color: #ff5555;
    background: rgba(30, 10, 20, 0.95);
  }
  
  .alert-container#lens-contact-alert {
    border-left-color: #55aaff;
    background: rgba(10, 20, 40, 0.95);
  }
  
  .alert-container#high-iop-alert {
    border-left-color: #ffcc00;
    background: rgba(40, 30, 10, 0.95);
  }
  
  .alert-container#retina-fixed-alert {
    border-left-color: #00cc66;
    background: rgba(10, 40, 20, 0.95);
  }
  
  .alert-container#vitreous-removed-alert {
    border-left-color: #55ff55;
    background: rgba(10, 40, 10, 0.95);
  }
  
  .alert-container#hole-located-alert {
    border-left-color: #ffff55;
    background: rgba(40, 40, 10, 0.95);
  }
  
  .alert-container#gas-injected-alert {
    border-left-color: #55aaff;
    background: rgba(10, 20, 40, 0.95);
  }
  
  .alert-icon {
    font-size: 1.8rem;
    margin-right: 15px;
    flex-shrink: 0;
  }
  
  .alert-content {
    flex-grow: 1;
  }
  
  .alert-content h3 {
    margin: 0 0 5px 0;
    color: white;
    font-size: 1.1rem;
    font-weight: 600;
  }
  
  .alert-content p {
    margin: 0;
    color: #ddd;
    font-size: 0.9rem;
    line-height: 1.4;
  }
  
  .alert-timer {
    height: 3px;
    background: rgba(255,255,255,0.3);
    margin-top: 8px;
    border-radius: 2px;
    overflow: hidden;
  }
  
  .alert-timer::after {
    content: '';
    display: block;
    height: 100%;
    width: 100%;
    background: white;
  }
  
  .alert-dismiss {
    background: none;
    border: none;
    color: white;
    font-size: 1.5rem;
    cursor: pointer;
    padding: 0 0 0 15px;
    opacity: 0.7;
    transition: opacity 0.2s;
    flex-shrink: 0;
  }
  
  .alert-dismiss:hover {
    opacity: 1;
  }
  
  /* Efecto de hemorragia */
  .hemorrhage-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(200, 0, 0, 0.5);
    z-index: 9998;
    pointer-events: none;
    display: none;
    opacity: 0;
    transition: opacity 1s ease;
  }
  
  /* Coágulos de sangre */
  .blood-clot {
    position: absolute;
    background: radial-gradient(circle, rgba(180,0,0,0.9), rgba(120,0,0,0.7));
    border-radius: 50%;
    transform: translate(-50%, -50%);
    z-index: 15;
    box-shadow: 
        0 0 10px rgba(180,0,0,0.8),
        inset 0 0 5px rgba(255,255,255,0.3);
    filter: blur(0.5px);
    animation: clotPulse 2s infinite alternate;
  }
  
  @keyframes clotPulse {
    0% { transform: translate(-50%, -50%) scale(1); }
    100% { transform: translate(-50%, -50%) scale(1.05); }
  }
  
  /* Burbujas subretinianas */
  .subretinal-bubble {
    position: absolute;
    background: rgba(200, 50, 50, 0.6);
    border-radius: 50%;
    pointer-events: none;
    z-index: 14;
    filter: blur(1px);
    box-shadow: 
      0 0 8px rgba(200, 50, 50, 0.7),
      inset 0 0 3px rgba(255, 255, 255, 0.4);
  }
  
  /* Marcas de cauterio */
  .cautery-mark {
    position: absolute;
    width: 8px;
    height: 8px;
    background: radial-gradient(circle, 
      white 0%, 
      rgba(255,255,255,0.9) 60%, 
      transparent 80%);
    border-radius: 50%;
    pointer-events: none;
    z-index: 16;
    box-shadow: 0 0 15px rgba(255,180,180,0.6);
    animation: burnPulse 3s infinite alternate;
  }
  
  @keyframes burnPulse {
    0%, 100% { opacity: 0.8; transform: scale(1); }
    50% { opacity: 1; transform: scale(1.1); }
    100% { opacity: 0.8; transform: scale(1); }
  }
  
  /* ================== ESTILOS BASE MEJORADOS ================== */
  * {
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
  }
  
  body {
    margin: 0;
    padding: 0;
    background: #0a0a12;
    overflow: hidden;
    font-family: 'Roboto', 'Segoe UI', system-ui, sans-serif;
    color: #e0e0e0;
  }
  
  #container {
    position: relative;
    width: 100vw;
    height: 100vh;
    perspective: 1200px;
    background: radial-gradient(circle at center, #1a1a2a 0%, #0a0a12 100%);
  }
  
  /* ================== RETINA CENTRAL - MEJORADA CON TEXTURAS REALISTAS ================== */
  #eye-chamber {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    touch-action: none;
    background: radial-gradient(circle at center, rgba(0,0,0,0.3) 0%, transparent 70%);
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
    filter: contrast(1.2) brightness(0.9) saturate(1.1);
    will-change: transform, filter;
    box-shadow: 
      inset 0 0 100px rgba(100, 20, 20, 0.3),
      0 0 80px rgba(0, 0, 0, 0.8);
  }
  
  .retina-sphere {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: radial-gradient(circle at center, 
      #500000 0%, 
      #400000 20%, 
      #300000 40%,
      #200000 70%,
      #100000 100%);
    transform: rotateX(15deg) translateZ(50px);
    box-shadow: 
      inset 0 0 200px rgba(220, 80, 80, 0.3),
      inset 0 0 80px rgba(255, 120, 120, 0.2),
      0 0 120px rgba(0, 0, 0, 0.9);
    overflow: hidden;
  }
  
  .retina-sphere::after {
    content: '';
    position: absolute;
    width: 200%;
    height: 200%;
    left: -50%;
    top: -50%;
    background-image: 
      radial-gradient(circle at 50% 50%, 
        rgba(255, 180, 180, 0.08) 0.5%, 
        transparent 1.2%),
      repeating-linear-gradient(
        45deg,
        transparent 0px,
        transparent 2px,
        rgba(255, 255, 255, 0.03) 2px,
        rgba(255, 255, 255, 0.03) 3px
      ),
      repeating-linear-gradient(
        -45deg,
        transparent 0px,
        transparent 3px,
        rgba(150, 50, 50, 0.05) 3px,
        rgba(150, 50, 50, 0.05) 5px
      );
    background-size: 
      60px 60px,
      100% 100%,
      100% 100%;
    transform: rotate(15deg);
    animation: textureMove 120s linear infinite;
    pointer-events: none;
  }
  
  @keyframes textureMove {
    0% { transform: rotate(15deg) translate(0,0); }
    100% { transform: rotate(15deg) translate(200px,200px); }
  }
  
  .retina-texture {
    position: absolute;
    width: 100%;
    height: 100%;
    background: 
      radial-gradient(circle at 50% 50%, rgba(255,200,200,0.2) 0%, rgba(255,150,150,0.15) 80%),
      url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><path d="M${Array.from({length:50},(_,i)=>`${Math.random()*100},${Math.random()*100}`).join(" ")}" stroke="rgba(180,80,80,0.08)" fill="none" stroke-width="0.5"/></svg>');
    background-size: 100%, 15px 15px;
    opacity: 0.8;
    border-radius: 50%;
    mix-blend-mode: overlay;
    filter: contrast(1.3);
    will-change: transform;
  }
  
  .macula {
    position: absolute;
    width: 100px;
    height: 100px;
    background: radial-gradient(circle at center, 
      rgba(255,230,200,0.7) 0%, 
      rgba(255,210,170,0.5) 60%, 
      transparent 100%);
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    box-shadow: 
      0 0 40px rgba(255, 220, 150, 0.4),
      inset 0 0 15px rgba(255, 200, 100, 0.5);
    animation: pulse 4s ease-in-out infinite;
  }
  
  @keyframes pulse {
    0%, 100% { opacity: 0.9; transform: translate(-50%, -50%) scale(1); }
    50% { opacity: 1; transform: translate(-50%, -50%) scale(1.05); }
  }
  
  .blood-vessels {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 3;
    pointer-events: none;
    filter: drop-shadow(0 0 3px rgba(80, 0, 0, 0.7));
    will-change: transform;
  }
  
  .blood-vessels-animated {
    animation: vesselPulse 3s ease-in-out infinite;
  }
  
  @keyframes vesselPulse {
    0%, 100% { opacity: 0.7; transform: scale(1); }
    50% { opacity: 0.9; transform: scale(1.01); }
  }
  
  .retina-nerve {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="300" height="300"><path d="M0,0 Q50,50 100,0 T200,0" stroke="rgba(255,255,255,0.06)" stroke-width="1.5" fill="none"/></svg>') center/cover;
    pointer-events: none;
    opacity: 0.6;
    z-index: 4;
  }
  
  .optic-disc {
    position: absolute;
    width: 80px;
    height: 80px;
    background: radial-gradient(circle at center, 
      rgba(220,120,120,0.5) 0%, 
      rgba(200,100,100,0.4) 100%);
    border-radius: 50%;
    top: 50%;
    left: 25%;
    transform: translate(-50%, -50%);
    box-shadow: 
      inset 0 0 20px rgba(220,80,80,0.6), 
      0 0 25px rgba(220,80,80,0.5);
    z-index: 5;
    animation: discPulse 5s ease-in-out infinite;
  }
  
  @keyframes discPulse {
    0%, 100% { transform: translate(-50%, -50%) scale(1); }
    50% { transform: translate(-50%, -50%) scale(1.03); }
  }
  
  .optic-cup {
    position: absolute;
    width: 35px;
    height: 35px;
    background: radial-gradient(circle at center, 
      rgba(220,80,80,0.6) 0%, 
      rgba(200,60,60,0.5) 100%);
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: inset 0 0 15px rgba(180,50,50,0.7);
    z-index: 6;
  }
  
  /* ================== LUZ ENDOILUMINADOR - EFECTOS DINÁMICOS MEJORADOS ================== */
  #light-mask {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.95);
    mask-image: radial-gradient(
      circle at var(--light-x, 50%) var(--light-y, 50%), 
      transparent var(--light-size, 80px), 
      black calc(var(--light-size, 80px) + 80px)
    );
    -webkit-mask-image: radial-gradient(
      circle at var(--light-x, 50%) var(--light-y, 50%), 
      transparent var(--light-size, 80px), 
      black calc(var(--light-size, 80px) + 80px)
    );
    transition: all 0.15s ease-out;
    pointer-events: none;
    z-index: 7;
    will-change: mask-image, -webkit-mask-image;
  }
  
  #light-reflection {
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(
      circle at var(--light-x, 50%) var(--light-y, 50%), 
      rgba(255,255,240,0.95) calc(var(--light-size, 80px)*0.1), 
      rgba(255,220,180,0.6) calc(var(--light-size, 80px)*0.3), 
      transparent var(--light-size, 80px)
    );
    opacity: 0;
    transition: opacity 0.25s ease, transform 0.25s ease;
    border-radius: 50%;
    pointer-events: none;
    z-index: 8;
    mix-blend-mode: soft-light;
    filter: blur(1.5px);
    will-change: opacity, transform;
  }
  
  .light-scatter {
    position: absolute;
    width: 300%;
    height: 300%;
    left: -100%;
    top: -100%;
    background: radial-gradient(
      circle at var(--light-x, 50%) var(--light-y, 50%),
      rgba(255, 230, 200, 0.15) 0%,
      transparent 80%
    );
    pointer-events: none;
    z-index: 9;
    opacity: 0;
    transition: opacity 0.3s ease;
    will-change: background, opacity;
  }
  
  .light-scatter.active {
    opacity: 0.7;
  }
  
  .specular-highlight {
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(
      circle at var(--light-x, 50%) var(--light-y, 50%),
      rgba(255, 255, 255, 0.5) 0%,
      transparent 70%
    );
    pointer-events: none;
    z-index: 10;
    mix-blend-mode: overlay;
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  
  .specular-highlight.active {
    opacity: 0.4;
  }
  
  /* ================== INSTRUMENTOS QUIRÚRGICOS 3D MEJORADOS ================== */
  .instrument {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%) translateZ(0);
    display: none;
    z-index: 10;
    transition: transform 0.1s ease-out;
    will-change: transform;
    transform-style: preserve-3d;
    perspective: 1000px;
  }
  
  .instrument-body {
    position: relative;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    transition: all 0.2s ease;
  }
  
  .instrument-shadow {
    position: absolute;
    width: 120%;
    height: 120%;
    left: -10%;
    top: -10%;
    background: radial-gradient(
      ellipse at center,
      rgba(0,0,0,0.4) 0%,
      transparent 70%
    );
    transform: rotateX(75deg) translateZ(-10px);
    pointer-events: none;
    z-index: -1;
    filter: blur(5px);
    opacity: 0.7;
    transition: all 0.3s ease;
  }
  
  /* Vitrectomo 3D mejorado */
  #vitrectome {
    width: 2.5mm;
    height: 15mm;
    transform-style: preserve-3d;
  }
  
  #vitrectome .instrument-body {
    background: linear-gradient(to bottom, 
      #999 0%, 
      #ccc 10%, 
      #eee 20%, 
      #fff 40%, 
      #ddd 60%, 
      #aaa 80%,
      #888 100%);
    border: 1px solid #777;
    border-radius: 1.2mm;
    box-shadow: 
      0 0 1.5mm rgba(0,0,0,0.7),
      0 0 3mm rgba(255,255,255,0.15),
      inset 0 0 5px rgba(255,255,255,0.3);
    transform: rotateX(10deg);
  }
  
  #vitrectome::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(1px);
    width: 3mm;
    height: 1mm;
    background: linear-gradient(to right, #aaa, #ddd, #aaa);
    border-radius: 0.5mm;
    box-shadow: 0 1px 2px rgba(0,0,0,0.5);
    z-index: 2;
  }
  
  #vitrectome::after {
    content: '';
    position: absolute;
    bottom: -1mm;
    left: 50%;
    transform: translateX(-50%) translateZ(-1px);
    width: 1.8mm;
    height: 1.8mm;
    background: radial-gradient(circle, 
      #fff 0%, 
      #bbb 50%, 
      #888 100%);
    border-radius: 50%;
    box-shadow: 
      0 0 0.8mm rgba(0,0,0,0.7),
      0 0 1.5mm rgba(255,255,255,0.3),
      inset 0 0 5px rgba(255,255,255,0.4);
    z-index: 1;
  }
  
  #vitrectome .instrument-tip {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(0);
    width: 1.2mm;
    height: 3mm;
    background: linear-gradient(to bottom, #ccc, #999);
    border-radius: 0.3mm;
    box-shadow: 
      inset 0 -1px 2px rgba(0,0,0,0.5),
      0 0 5px rgba(255,255,255,0.2);
  }
  
  #vitrectome .instrument-side {
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(to bottom, #888, #aaa, #888);
    transform: rotateY(90deg) translateZ(1.25mm);
    border-radius: 1.2mm;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.5);
  }
  
  /* Sonda de Láser 3D mejorada */
  #laser-probe {
    width: 0.5mm;
    height: 12mm;
    transform-style: preserve-3d;
  }
  
  #laser-probe .instrument-body {
    background: linear-gradient(to bottom, 
      #f00, 
      #f90 20%, 
      #ff0 30%, 
      #f90 70%, 
      #f00);
    border-radius: 0.3mm;
    box-shadow: 
      0 0 1.5mm rgba(255,0,0,0.9),
      0 0 3mm rgba(255,100,100,0.6),
      inset 0 0 5px rgba(255,255,255,0.4);
    transform: rotateX(10deg);
  }
  
  #laser-probe::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(1px);
    width: 1.5mm;
    height: 1mm;
    background: linear-gradient(to right, #aaa, #ddd, #aaa);
    border-radius: 0.5mm;
    box-shadow: 0 1px 2px rgba(0,0,0,0.5);
    z-index: 2;
  }
  
  #laser-probe .instrument-tip {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(0);
    width: 0.8mm;
    height: 1.5mm;
    background: radial-gradient(circle, #fff, #f99);
    border-radius: 50%;
    box-shadow: 
      0 0 5px rgba(255,100,100,0.8),
      inset 0 0 3px rgba(255,255,255,0.8);
  }
  
  /* Sonda de PFC */
  #pfc-probe {
    width: 0.8mm;
    height: 13mm;
    transform-style: preserve-3d;
  }
  
  #pfc-probe .instrument-body {
    background: linear-gradient(to bottom, 
      #0af 0%, 
      #5bf 30%, 
      #8df 70%, 
      #0af 100%);
    border: 1px solid #07a;
    border-radius: 0.5mm;
    box-shadow: 
      0 0 1.5mm rgba(0,100,255,0.7),
      0 0 3mm rgba(100,200,255,0.4),
      inset 0 0 5px rgba(255,255,255,0.6);
    transform: rotateX(10deg);
  }
  
  #pfc-probe::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(1px);
    width: 2mm;
    height: 1mm;
    background: linear-gradient(to right, #aaa, #ddd, #aaa);
    border-radius: 0.5mm;
    box-shadow: 0 1px 2px rgba(0,0,0,0.5);
    z-index: 2;
  }
  
  #pfc-probe .instrument-tip {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(0);
    width: 1mm;
    height: 1.5mm;
    background: radial-gradient(circle, #fff, #5bf);
    border-radius: 50%;
    box-shadow: 
      0 0 5px rgba(0,150,255,0.8),
      inset 0 0 3px rgba(255,255,255,0.8);
  }
  
  /* Cauterio */
  #cautery-probe {
    width: 1.5mm;
    height: 12mm;
    transform-style: preserve-3d;
  }
  
  #cautery-probe .instrument-body {
    background: linear-gradient(to bottom, 
      #ff3333 0%, 
      #ff6666 20%, 
      #ff9999 40%, 
      #ff6666 70%, 
      #ff3333 100%);
    border: 1px solid #cc0000;
    border-radius: 0.8mm;
    box-shadow: 
      0 0 1.5mm rgba(255,0,0,0.7),
      0 0 3mm rgba(255,100,100,0.4),
      inset 0 0 5px rgba(255,255,255,0.6);
    transform: rotateX(10deg);
  }
  
  #cautery-probe::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(1px);
    width: 2mm;
    height: 1mm;
    background: linear-gradient(to right, #aaa, #ddd, #aaa);
    border-radius: 0.5mm;
    box-shadow: 0 1px 2px rgba(0,0,0,0.5);
    z-index: 2;
  }
  
  #cautery-probe .instrument-tip {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(0);
    width: 1.2mm;
    height: 2mm;
    background: radial-gradient(circle, #ff9999, #ff3333);
    border-radius: 0.3mm;
    box-shadow: 
      0 0 5px rgba(255,100,100,0.8),
      inset 0 0 3px rgba(255,255,255,0.8);
    z-index: 1;
  }
  
  /* Efecto de cauterio */
  .cautery-effect {
    position: absolute;
    width: 30px;
    height: 30px;
    background: radial-gradient(circle, 
      rgba(255,200,200,0.9) 0%, 
      rgba(255,100,100,0.7) 70%, 
      transparent 90%);
    border-radius: 50%;
    pointer-events: none;
    z-index: 18;
    animation: cauteryFade 1s ease-out forwards;
    filter: blur(1.5px);
    box-shadow: 0 0 20px rgba(255,100,100,0.8);
  }
  
  @keyframes cauteryFade {
    0% { transform: scale(0.8); opacity: 1; }
    50% { transform: scale(1.5); opacity: 0.9; }
    100% { transform: scale(1.5); opacity: 0; }
  }
  
  /* Burbujas de PFC */
  .pfc-bubble {
    position: absolute;
    background: rgba(100, 255, 255, 0.7);
    border-radius: 50%;
    pointer-events: none;
    z-index: 5;
    filter: blur(1px);
    box-shadow: 
      0 0 10px rgba(100, 255, 255, 0.8),
      inset 0 0 5px rgba(255, 255, 255, 0.8);
    transform: translate(-50%, -50%);
    will-change: transform, opacity;
  }
  
  .instrument-active {
    animation: instrumentVibrate 0.1s linear infinite;
  }
  
  @keyframes instrumentVibrate {
    0% { transform: translate(-50%, -50%) rotate(0.5deg) translateZ(0); }
    50% { transform: translate(-50%, -50%) rotate(-0.5deg) translateZ(0); }
    100% { transform: translate(-50%, -50%) rotate(0.5deg) translateZ(0); }
  }
  
  /* ================== INTERFAZ DE USUARIO PROFESIONAL ================== */
  .instrument-panel {
    position: absolute;
    top: 50%;
    left: 20px;
    transform: translateY(-50%);
    background: rgba(0,0,0,0.85);
    padding: 15px 10px;
    border-radius: 12px;
    z-index: 5000;
    display: flex;
    flex-direction: column;
    box-shadow: 
      0 6px 20px rgba(0,0,0,0.6),
      inset 0 0 15px rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
    border: 1px solid rgba(255,255,255,0.15);
  }
  
  .toggle-btn {
    background: #1a3a8a;
    color: white;
    padding: 12px 18px;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    font-size: clamp(0.85rem, 2.5vw, 1rem);
    margin: 8px 5px;
    min-width: 100px;
    transition: all 0.2s;
    font-weight: 500;
    box-shadow: 
      0 3px 8px rgba(0,0,0,0.4),
      inset 0 0 8px rgba(255,255,255,0.15);
    position: relative;
    overflow: hidden;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  
  .toggle-btn::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 5px;
    height: 5px;
    background: rgba(255, 255, 255, 0.6);
    opacity: 0;
    border-radius: 100%;
    transform: scale(1, 1) translate(-50%, -50%);
    transform-origin: 50% 50%;
  }
  
  .toggle-btn:active::after {
    animation: ripple 0.6s ease-out;
  }
  
  @keyframes ripple {
    0% {
      transform: scale(0, 0);
      opacity: 1;
    }
    20% {
      transform: scale(25, 25);
      opacity: 1;
    }
    100% {
      opacity: 0;
      transform: scale(40, 40);
    }
  }
  
  .toggle-btn.active {
    background: #3b82f6;
    box-shadow: 
      0 0 20px #3b82f6,
      0 0 40px rgba(59, 130, 246, 0.4);
  }
  
  /* Panel de controles mejorado */
  .control-panel {
    position: absolute;
    right: 20px;
    top: 190px;
    background: rgba(10,10,20,0.95);
    padding: 20px;
    border-radius: 50px;
    z-index: 5000;
    font-size: clamp(0.75rem, 2vw, 0.9rem);
    width: 180px;
    max-height: 80vh;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    box-shadow: 
      0 6px 20px rgba(18, 20, 41, 0.6),
      inset 0 0 15px rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
  }
  
  .control-panel h3 {
    margin: 0 0 15px 0;
    color: #2990aa;
    font-size: 1.1rem;
    text-align: center;
    border-bottom: 1px solid #444;
    padding-bottom: 8px;
  }
  
  .vital-sign {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
    flex-wrap: wrap;
  }
  
  .vital-label {
    color: #aaa;
    font-size: 0.85em;
    width: 60%;
  }
  
  .vital-value {
    font-family: 'Courier New', monospace;
    font-weight: bold;
    font-size: 0.95em;
    width: 40%;
    text-align: right;
    transition: color 0.3s;
  }
  
  .normal { color: #2ecc71; text-shadow: 0 0 8px rgba(46, 204, 113, 0.4); }
  .warning { color: #f39c12; text-shadow: 0 0 8px rgba(243, 156, 18, 0.4); }
  .danger { color: #e74c3c; text-shadow: 0 0 8px rgba(231, 76, 60, 0.4); }
  
  .gauge-container {
    width: 100%;
    height: 10px;
    background: #222;
    border-radius: 5px;
    margin: 8px 0 15px 0;
    overflow: hidden;
    box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
  }
  
  .gauge-level {
    height: 100%;
    border-radius: 5px;
    transition: width 0.5s ease;
    box-shadow: inset 0 0 8px rgba(255,255,255,0.3);
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
  
  #pfc-gauge .gauge-level {
    background: linear-gradient(to right, #0af, #5bf);
  }
  
  /* Joysticks mejorados */
  .joystick-container {
    position: absolute;
    bottom: 1px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    z-index: 3000;
  }
  
  #joystick-light-container {
    left: 200px;
  }
  
  #joystick-vitrectomo-container {
    right: 180px;
  }
  
  .joystick {
    width: 100px;
    height: 100px;
    background: rgba(255,255,255,0.1);
    border: 2px solid rgba(255,255,255,0.4);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    touch-action: none;
    position: relative;
    box-shadow: 
      0 6px 15px rgba(0,0,0,0.4),
      inset 0 0 25px rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
    will-change: transform;
  }
  
  .joystick .joystick-handle {
    width: 40px;
    height: 40px;
    background: rgba(255,255,255,0.8);
    border-radius: 50%;
    position: absolute;
    transition: transform 0.1s ease;
    box-shadow: 
      0 0 15px rgba(255,255,255,0.4),
      inset 0 0 8px rgba(255,255,255,0.7);
    will-change: transform;
  }
  
  /* Controles Z mejorados */
  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    background: rgba(0,0,0,0.7);
    padding: 15px;
    border-radius: 12px;
    box-shadow: 
      0 4px 10px rgba(0,0,0,0.4),
      inset 0 0 8px rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
  }
  
  .slider-container label {
    font-size: 0.8rem;
    margin-bottom: 10px;
    text-align: center;
    color: #eee;
    font-weight: 500;
    text-shadow: 0 0 8px rgba(0,0,0,0.6);
  }
  
  input[type="range"] {
    width: 100%;
    -webkit-appearance: none;
    height: 10px;
    background: #333;
    border-radius: 5px;
    margin: 8px 0 12px 0;
    box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
  }
  
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 22px;
    height: 22px;
    background: #3b82f6;
    border-radius: 50%;
    cursor: pointer;
    border: 2px solid white;
    box-shadow: 
      0 0 8px rgba(0,0,0,0.7),
      0 0 15px rgba(59, 130, 246, 0.7);
    transition: all 0.2s;
  }
  
  input[type="range"]::-webkit-slider-thumb:active {
    transform: scale(1.2);
    box-shadow: 
      0 0 12px rgba(0,0,0,0.7),
      0 0 20px rgba(59, 130, 246, 1);
  }
  
  #btn-precionar {
    background: linear-gradient(to bottom, #dc2626, #b91c1c);
    color: white;
    padding: 12px 15px;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    font-size: 0.95rem;
    margin-top: 8px;
    width: 100%;
    font-weight: bold;
    box-shadow: 
      0 4px 12px rgba(0,0,0,0.4),
      0 0 15px rgba(220, 38, 38, 0.4);
    transition: all 0.2s;
    text-shadow: 0 0 8px rgba(0,0,0,0.4);
    text-transform: uppercase;
    letter-spacing: 1px;
  }
  
  #btn-precionar:active {
    transform: scale(0.95);
    box-shadow: 
      0 2px 6px rgba(0,0,0,0.4),
      0 0 8px rgba(220, 38, 38, 0.4);
  }
  
  /* Mini mapa mejorado */
  #miniMapContainer {
    position: absolute;
    top: 40px;
    right: 30px;
    width: 170px;
    height: 120px;
    overflow: hidden;
    border-radius: 5px;
    z-index: 2000;
    background: rgba(0,0,0,0.85);
    border: 1px solid rgba(255,255,255,0.15);
    box-shadow: 
      0 6px 15px rgba(0,0,0,0.5),
      inset 0 0 15px rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
  }
  
  #eyeCrossSection {
    width: 100%;
    height: 100%;
    display: block;
    filter: drop-shadow(0 0 8px rgba(0,0,0,0.7));
  }
  
  /* Indicador de profundidad */
  .depth-indicator {
    position: absolute;
    bottom: 130px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0,0,0,0.8);
    padding: 10px 20px;
    border-radius: 25px;
    color: white;
    font-size: 0.85rem;
    z-index: 2000;
    display: flex;
    align-items: center;
    backdrop-filter: blur(8px);
    box-shadow: 0 0 15px rgba(0,0,0,0.6);
    border: 1px solid var(--depth-color, #3b82f6);
    text-transform: uppercase;
    letter-spacing: 1px;
  }
  
  .depth-indicator::before {
    content: '';
    display: inline-block;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    margin-right: 10px;
    background: var(--depth-color, #3b82f6);
    box-shadow: 0 0 8px var(--depth-color, #3b82f6);
  }
  
  /* Efectos visuales mejorados */
  .laser-spot {
    width: 30px;
    height: 30px;
    background: radial-gradient(circle, 
      rgba(255,80,80,0.95), 
      rgba(255,0,0,0.8) 70%, 
      transparent 90%);
    border-radius: 50%;
    position: absolute;
    pointer-events: none;
    animation: laser-fade 2.5s ease-out forwards;
    z-index: 15;
    filter: blur(1px);
    will-change: transform, opacity;
  }
  
  @keyframes laser-fade {
    0% { opacity: 1; transform: scale(1); }
    100% { opacity: 0; transform: scale(0.3); }
  }
  
  .laser-burn-permanent {
    width: 8px;
    height: 8px;
    background: radial-gradient(circle, 
      white 0%, 
      rgba(255,255,255,0.9) 60%, 
      transparent 80%);
    border-radius: 50%;
    position: absolute;
    pointer-events: none;
    z-index: 16;
    box-shadow: 0 0 15px rgba(255,120,120,0.6);
    animation: burnPulse 3s infinite alternate;
  }
  
  .vitreous-particle {
    width: 6px;
    height: 6px;
    background: rgba(255,255,255,0.95);
    border-radius: 50%;
    position: absolute;
    pointer-events: none;
    animation: float-particle 1.5s ease-out forwards;
    z-index: 12;
    filter: blur(0.8px);
    box-shadow: 0 0 8px rgba(255,255,255,0.9);
    will-change: transform, opacity;
  }
  
  @keyframes float-particle {
    100% { transform: translate(var(--tx, 0px), var(--ty, 0px)); opacity: 0; }
  }
  
  .injection-bubble {
    position: absolute;
    background: rgba(200,230,255,0.9);
    border-radius: 50%;
    pointer-events: none;
    z-index: 3;
    filter: blur(1.5px);
    animation: bubble-float 4s ease-in-out forwards;
    box-shadow: 
      0 0 8px rgba(100,150,255,0.7),
      inset 0 0 5px rgba(255,255,255,0.7);
    will-change: transform, opacity;
  }
  
  @keyframes bubble-float {
    100% { 
      transform: translate(calc(var(--tx)*1px), calc(var(--ty)*1px));
      opacity: 0;
    }
  }
  
  /* ================== NUEVOS ESTILOS PARA EL DESPRENDIMIENTO MEJORADO ================== */
  #retinal-hole {
    display: none;
    position: absolute;
    width: 30px;
    height: 30px;
    background: radial-gradient(circle, 
      rgba(255,255,255,0.8) 0%, 
      rgba(200,0,0,0.6) 70%, 
      transparent 100%);
    border-radius: 50%;
    z-index: 15;
    box-shadow: 0 0 15px rgba(255,255,255,0.5);
    border: 2px dashed rgba(255,255,255,0.7);
    pointer-events: none;
    animation: holePulse 2s infinite alternate;
  }
  
  @keyframes holePulse {
    0% { transform: scale(1); opacity: 0.8; }
    50% { transform: scale(1.1); opacity: 1; }
    100% { transform: scale(1); opacity: 0.8; }
  }
  
  /* Burbujas de gas mejoradas */
  .gas-bubble {
    position: absolute;
    background: rgba(255, 255, 255, 0.95);
    border-radius: 50%;
    pointer-events: none;
    z-index: 6;
    filter: blur(1px);
    box-shadow: 
      0 0 15px rgba(255, 255, 255, 0.9),
      inset 0 0 8px rgba(255, 255, 255, 0.8);
    transform: translate(-50%, -50%);
    will-change: transform, opacity;
    animation: bubbleFloat 4s ease-in-out forwards;
  }
  
  .gas-bubble-large {
    position: absolute;
    width: 40%;
    height: 40%;
    background: radial-gradient(circle, 
      rgba(255,255,255,0.95) 0%, 
      rgba(220,220,255,0.8) 70%, 
      transparent 100%);
    border-radius: 50%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 5;
    filter: blur(2px);
    box-shadow: 
      0 0 30px rgba(255,255,255,0.9),
      inset 0 0 20px rgba(255,255,255,0.8);
    animation: largeBubblePulse 3s infinite alternate;
  }
  
  @keyframes bubbleFloat {
    0% { transform: translate(-50%, -50%) scale(0.8); opacity: 0; }
    50% { transform: translate(-50%, -50%) scale(1.2); opacity: 0.9; }
    100% { transform: translate(calc(var(--tx)*1px), calc(var(--ty)*1px)) scale(1); opacity: 0; }
  }
  
  @keyframes largeBubblePulse {
    0% { transform: translate(-50%, -50%) scale(1); }
    50% { transform: translate(-50%, -50%) scale(1.1); }
    100% { transform: translate(-50%, -50%) scale(1); }
  }
  
  /* Efecto de arrugas en la retina desprendida */
  #retina-edge-detachment {
    filter: url('#wrinkleFilter');
  }
  
  /* Efecto de PFC mejorado */
  #detached-retina-overlay {
    transition: background 1.5s ease;
  }
  
  /* ================== CASO CLÍNICO ================== */
  #clinical-case {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.9);
    z-index: 20000;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    padding: 20px;
    box-sizing: border-box;
    backdrop-filter: blur(5px);
  }
  
  #clinical-case-content {
    max-width: 800px;
    background: rgba(20, 20, 40, 0.95);
    padding: 30px;
    border-radius: 15px;
    border-left: 5px solid #3b82f6;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.7);
  }
  
  #clinical-case h2 {
    color: #3b82f6;
    margin-top: 0;
    text-align: center;
    font-size: 1.8rem;
    margin-bottom: 20px;
  }
  
  #clinical-case h3 {
    color: #10b981;
    margin-bottom: 10px;
    font-size: 1.3rem;
  }
  
  #clinical-case ul {
    padding-left: 20px;
  }
  
  #clinical-case li {
    margin-bottom: 8px;
    line-height: 1.5;
  }
  
  #start-simulation-btn {
    background: #3b82f6;
    color: white;
    border: none;
    padding: 15px 30px;
    font-size: 1.2rem;
    border-radius: 8px;
    cursor: pointer;
    margin-top: 30px;
    transition: all 0.3s;
    box-shadow: 0 5px 15px rgba(59, 130, 246, 0.4);
  }
  
  #start-simulation-btn:hover {
    background: #2563eb;
    transform: translateY(-2px);
    box-shadow: 0 7px 20px rgba(59, 130, 246, 0.6);
  }
  
  #start-simulation-btn:active {
    transform: translateY(0);
    box-shadow: 0 5px 15px rgba(59, 130, 246, 0.4);
  }
  
  /* ================== RESPONSIVE ADJUSTMENTS ================== */
  @media (max-width: 768px) {
    .instrument-panel {
      top: 10px;
      left: 10px;
      right: auto;
      flex-direction: row;
      flex-wrap: wrap;
      justify-content: center;
      padding: 8px;
      width: calc(100% - 20px);
    }
    
    .toggle-btn {
      padding: 10px 12px;
      min-width: auto;
      font-size: 0.8rem;
      margin: 4px;
      flex: 1 1 calc(33% - 8px);
      max-width: calc(33% - 8px);
    }
    
    .control-panel {
      top: 120px;
      right: 10px;
      width: 150px;
      padding: 12px;
    }
    
    #miniMapContainer {
      top: 120px;
      left: 10px;
      width: 150px;
      height: 110px;
    }
    
    .joystick-container {
      bottom: 20px;
      width: 45%;
    }
    
    #joystick-light-container {
      left: 10px;
    }
    
    #joystick-vitrectomo-container {
      right: 10px;
    }
    
    .joystick {
      width: 80px;
      height: 80px;
    }
    
    .joystick .joystick-handle {
      width: 35px;
      height: 35px;
    }
    
    .slider-container {
      padding: 10px;
    }
    
    #btn-precionar {
      padding: 10px;
      font-size: 0.9rem;
    }
    
    .depth-indicator {
      bottom: 110px;
      font-size: 0.8rem;
    }
  }
  
  @media (max-width: 480px) {
    .retina-container {
      width: 95vmin;
      height: 95vmin;
    }
    
    .toggle-btn {
      font-size: 0.7rem;
      padding: 8px 10px;
    }
    
    .joystick {
      width: 70px;
      height: 70px;
    }
    
    .joystick .joystick-handle {
      width: 30px;
      height: 30px;
    }
    
    .control-panel {
      width: 130px;
      padding: 10px;
    }
    
    #miniMapContainer {
      width: 130px;
      height: 95px;
    }
    
    .depth-indicator {
      bottom: 100px;
      font-size: 0.75rem;
      padding: 8px 15px;
    }
  }
  /* Sistema de alertas */
        #alert-system {
          position: fixed;
          top: 20px;
          left: 50%;
          transform: translateX(-50%);
          z-index: 10000;
          width: 90%;
          max-width: 600px;
          display: flex;
          flex-direction: column;
          gap: 15px;
        }
    
        .alert-container {
          display: none;
          background: rgba(20, 20, 30, 0.95);
          border-left: 5px solid;
          border-radius: 8px;
          padding: 15px 20px;
          align-items: center;
          box-shadow: 0 10px 25px rgba(0,0,0,0.5);
          backdrop-filter: blur(8px);
          transform-origin: top center;
          border-left-color: #ff5555;
        }
    
        .alert-icon {
          font-size: 1.8rem;
          margin-right: 15px;
          flex-shrink: 0;
        }
    
        .alert-content {
          flex-grow: 1;
        }
    
        .alert-content h3 {
          margin: 0 0 5px 0;
          color: white;
          font-size: 1.1rem;
          font-weight: 600;
        }
    
        .alert-content p {
          margin: 0;
          color: #ddd;
          font-size: 0.9rem;
          line-height: 1.4;
        }
    
        .alert-timer {
          height: 3px;
          background: rgba(255,255,255,0.3);
          margin-top: 8px;
          border-radius: 2px;
          overflow: hidden;
        }
    
        .alert-timer::after {
          content: '';
          display: block;
          height: 100%;
          width: 100%;
          background: white;
        }
    
        .alert-dismiss {
          background: none;
          border: none;
          color: white;
          font-size: 1.5rem;
          cursor: pointer;
          padding: 0 0 0 15px;
          opacity: 0.7;
          transition: opacity 0.2s;
          flex-shrink: 0;
        }
    
        .alert-dismiss:hover {
          opacity: 1;
        }
    
        .toggle-btn {
          background: #1a3a8a;
          color: white;
          padding: 12px 18px;
          border: none;
          border-radius: 10px;
          cursor: pointer;
          font-size: clamp(0.85rem, 2.5vw, 1rem);
          margin: 8px 5px;
          min-width: 100px;
          transition: all 0.2s;
          font-weight: 500;
          box-shadow: 
            0 3px 8px rgba(0,0,0,0.4),
            inset 0 0 8px rgba(255,255,255,0.15);
          position: relative;
          overflow: hidden;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }
    
        .toggle-btn.active {
          background: #3b82f6;
          box-shadow: 
            0 0 20px #3b82f6,
            0 0 40px rgba(59, 130, 246, 0.4);
        }
    
        /* Panel de parámetros */
        .control-panel {
          position: absolute;
          right: 20px;
          top: 190px;
          background: rgba(10,10,20,0.95);
          padding: 20px;
          border-radius: 50px;
          z-index: 5000;
          font-size: clamp(0.75rem, 2vw, 0.9rem);
          width: 180px;
          max-height: 80vh;
          overflow-y: auto;
          -webkit-overflow-scrolling: touch;
          box-shadow: 
            0 6px 20px rgba(18, 20, 41, 0.6),
            inset 0 0 15px rgba(255,255,255,0.15);
          border: 1px solid rgba(255,255,255,0.15);
          backdrop-filter: blur(8px);
        }
    
        .vital-sign {
          display: flex;
          justify-content: space-between;
          margin: 15px 0;
          flex-wrap: wrap;
        }
    
        .vital-label {
          color: #aaa;
          font-size: 0.85em;
          width: 60%;
        }
    
        .vital-value {
          font-family: 'Courier New', monospace;
          font-weight: bold;
          font-size: 0.95em;
          width: 40%;
          text-align: right;
          transition: color 0.3s;
        }
    
        .normal { color: #2ecc71; text-shadow: 0 0 8px rgba(46, 204, 113, 0.4); }
        .warning { color: #f39c12; text-shadow: 0 0 8px rgba(243, 156, 18, 0.4); }
        .danger { color: #e74c3c; text-shadow: 0 0 8px rgba(231, 76, 60, 0.4); }
    
        .gauge-container {
          width: 100%;
          height: 10px;
          background: #222;
          border-radius: 5px;
          margin: 8px 0 15px 0;
          overflow: hidden;
          box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
        }
    
        .gauge-level {
          height: 100%;
          border-radius: 5px;
          transition: width 0.5s ease;
          box-shadow: inset 0 0 8px rgba(255,255,255,0.3);
        }
    
        /* Joysticks */
        .joystick-container {
          position: absolute;
          bottom: 1px;
          display: flex;
          flex-direction: column;
          align-items: center;
          gap: 20px;
          z-index: 3000;
        }
    
        #joystick-light-container {
          left: 200px;
        }
    
        #joystick-vitrectomo-container {
          right: 180px;
        }
    
        .joystick {
          width: 100px;
          height: 100px;
          background: rgba(255,255,255,0.1);
          border: 2px solid rgba(255,255,255,0.4);
          border-radius: 50%;
          display: flex;
          align-items: center;
          justify-content: center;
          touch-action: none;
          position: relative;
          box-shadow: 
            0 6px 15px rgba(0,0,0,0.4),
            inset 0 0 25px rgba(255,255,255,0.15);
          backdrop-filter: blur(8px);
          will-change: transform;
        }
    
        .joystick .joystick-handle {
          width: 40px;
          height: 40px;
          background: rgba(255,255,255,0.8);
          border-radius: 50%;
          position: absolute;
          transition: transform 0.1s ease;
          box-shadow: 
            0 0 15px rgba(255,255,255,0.4),
            inset 0 0 8px rgba(255,255,255,0.7);
          will-change: transform;
        }
    
        /* Controles Z */
        .slider-container {
          display: flex;
          flex-direction: column;
          align-items: center;
          width: 100%;
          background: rgba(0,0,0,0.7);
          padding: 15px;
          border-radius: 12px;
          box-shadow: 
            0 4px 10px rgba(0,0,0,0.4),
            inset 0 0 8px rgba(255,255,255,0.15);
          backdrop-filter: blur(8px);
        }
    
        .slider-container label {
          font-size: 0.8rem;
          margin-bottom: 10px;
          text-align: center;
          color: #eee;
          font-weight: 500;
          text-shadow: 0 0 8px rgba(0,0,0,0.6);
        }
    
        input[type="range"] {
          width: 100%;
          -webkit-appearance: none;
          height: 10px;
          background: #333;
          border-radius: 5px;
          margin: 8px 0 12px 0;
          box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
        }
    
        input[type="range"]::-webkit-slider-thumb {
          -webkit-appearance: none;
          width: 22px;
          height: 22px;
          background: #3b82f6;
          border-radius: 50%;
          cursor: pointer;
          border: 2px solid white;
          box-shadow: 
            0 0 8px rgba(0,0,0,0.7),
            0 0 15px rgba(59, 130, 246, 0.7);
          transition: all 0.2s;
        }
    
        input[type="range"]::-webkit-slider-thumb:active {
          transform: scale(1.2);
          box-shadow: 
            0 0 12px rgba(0,0,0,0.7),
            0 0 20px rgba(59, 130, 246, 1);
        }
    
        #btn-precionar {
          background: linear-gradient(to bottom, #dc2626, #b91c1c);
          color: white;
          padding: 12px 15px;
          border: none;
          border-radius: 10px;
          cursor: pointer;
          font-size: 0.95rem;
          margin-top: 8px;
          width: 100%;
          font-weight: bold;
          box-shadow: 
            0 4px 12px rgba(0,0,0,0.4),
            0 0 15px rgba(220, 38, 38, 0.4);
          transition: all 0.2s;
          text-shadow: 0 0 8px rgba(0,0,0,0.4);
          text-transform: uppercase;
          letter-spacing: 1px;
        }
    
        #btn-precionar:active {
          transform: scale(0.95);
          box-shadow: 
            0 2px 6px rgba(0,0,0,0.4),
            0 0 8px rgba(220, 38, 38, 0.4);
        }
    
        .laser-spot {
          position: absolute;
          width: 24px;
          height: 24px;
          background: radial-gradient(circle, rgba(255,50,50,0.8) 0%, rgba(255,0,0,0.6) 70%, transparent 100%);
          border-radius: 50%;
          box-shadow: 0 0 15px rgba(255,100,100,0.7);
          pointer-events: none;
          z-index: 20;
        }
    
        .laser-burn-permanent {
          position: absolute;
          width: 6px;
          height: 6px;
          background: rgba(255, 255, 255, 0.9);
          border-radius: 50%;
          box-shadow: 0 0 5px rgba(255, 255, 255, 0.8);
          pointer-events: none;
          z-index: 15;
        }
    
        .cautery-effect {
          position: absolute;
          width: 30px;
          height: 30px;
          background: radial-gradient(circle, rgba(255,200,0,0.8) 0%, rgba(255,150,0,0.6) 70%, transparent 100%);
          border-radius: 50%;
          box-shadow: 0 0 20px rgba(255,200,0,0.7);
          pointer-events: none;
          z-index: 20;
        }
    
        .blood-clot {
          position: absolute;
          background: radial-gradient(circle, rgba(180,0,0,0.8) 0%, rgba(120,0,0,0.6) 70%, transparent 100%);
          border-radius: 50%;
          pointer-events: none;
          z-index: 10;
        }
    
        .instrument-action-indicator {
          position: absolute;
          top: -30px;
          left: 50%;
          transform: translateX(-50%);
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 5px 10px;
          border-radius: 15px;
          font-size: 0.8rem;
          white-space: nowrap;
          z-index: 100;
          opacity: 0;
          pointer-events: none;
        }
        
        .instrument-action-indicator.active {
          opacity: 1;
        }
    
        /* Efectos de vitrectomía mejorados */
        .vitrectomy-effect {
          position: absolute;
          width: 60px;
          height: 60px;
          background: radial-gradient(circle, 
            rgba(255,255,255,0.2) 0%, 
            rgba(255,255,255,0.1) 40%, 
            transparent 70%);
          border-radius: 50%;
          pointer-events: none;
          z-index: 14;
          transform: scale(0);
          opacity: 0;
        }
    
        .suction-particle {
          position: absolute;
          width: 4px;
          height: 4px;
          background: rgba(200, 200, 255, 0.8);
          border-radius: 50%;
          pointer-events: none;
          z-index: 12;
          box-shadow: 0 0 5px rgba(255,255,255,0.6);
        }
    
        /* Efecto de sangre mejorado */
        .blood-extraction {
          position: absolute;
          width: 40px;
          height: 40px;
          background: radial-gradient(circle, 
            rgba(200,0,0,0.8) 0%, 
            rgba(150,0,0,0.6) 50%, 
            transparent 80%);
          border-radius: 50%;
          pointer-events: none;
          z-index: 13;
          transform: scale(0);
          filter: blur(2px);
        }
        
        /* Estilos para el instrumento único - PALO GRIS */
        .instrument-container {
          position: absolute;
          width: 100%;
          height: 100%;
          pointer-events: none;
          z-index: 30;
        }
        
        .instrument-rod {
          position: absolute;
          height: 8px;
          background: linear-gradient(to right, #888, #ccc);
          border-radius: 4px;
          transform-origin: left center;
          z-index: 30;
          box-shadow: 0 0 10px rgba(0,0,0,0.5);
        }
        
        .instrument-tip {
          position: absolute;
          width: 20px;
          height: 20px;
          border-radius: 50%;
          background: #ccc;
          z-index: 31;
          box-shadow: 0 0 10px rgba(0,0,0,0.5);
          transform: translate(-50%, -50%);
        }
        
        .instrument-base {
          position: absolute;
          width: 30px;
          height: 30px;
          border-radius: 50%;
          background: #555;
          z-index: 29;
          box-shadow: 0 0 15px rgba(0,0,0,0.7);
        }
        
        .instrument-tip.active-vitrectomo {
          background: #3b82f6;
          box-shadow: 0 0 15px rgba(59, 130, 246, 0.7);
        }
        
        .instrument-tip.active-laser {
          background: #ff5555;
          box-shadow: 0 0 15px rgba(255, 85, 85, 0.7);
        }
        
        .instrument-tip.active-cautery {
          background: #ff9900;
          box-shadow: 0 0 15px rgba(255, 153, 0, 0.7);
        }
        
        .instrument-tip.active-forceps {
          background: #00cc66;
          box-shadow: 0 0 15px rgba(0, 204, 102, 0.7);
        }
        
        /* Efecto de pinzas agarrando */
        .instrument-tip.grasping {
          animation: grasping 0.5s infinite alternate;
        }
        
        @keyframes grasping {
          0% { transform: translate(-50%, -50%) scale(1); }
          100% { transform: translate(-50%, -50%) scale(1.2); }
        }
        
        /* Indicador de profundidad */
        .depth-indicator {
          position: absolute;
          bottom: 20px;
          left: 50%;
          transform: translateX(-50%);
          background: rgba(0,0,0,0.7);
          color: var(--depth-color, white);
          padding: 8px 15px;
          border-radius: 20px;
          font-size: 0.9rem;
          z-index: 1000;
          box-shadow: 0 0 10px rgba(0,0,0,0.5);
        }
        
        /* Efecto de hemorragia */
        .hemorrhage-overlay {
          position: absolute;
          width: 100%;
          height: 100%;
          background: radial-gradient(circle, rgba(200,0,0,0.7) 0%, transparent 70%);
          z-index: 25;
          opacity: 0;
          display: none;
        }
        
        /* Cuerpo extraño */
        .foreign-body {
          position: absolute;
          background: radial-gradient(circle, rgba(150,150,150,0.9) 0%, rgba(100,100,100,0.8) 70%, rgba(50,50,50,0.7) 100%);
          border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
          box-shadow: 0 0 15px rgba(0,0,0,0.5);
          z-index: 15;
          transition: transform 0.3s ease;
        }
        
        .foreign-body.grabbed {
          transform: translate(-50%, -50%) scale(1.2);
          box-shadow: 0 0 20px rgba(0, 204, 102, 0.5);
          z-index: 32;
        }
        
        .foreign-body-particle {
          position: absolute;
          width: 6px;
          height: 6px;
          background: rgba(150,150,150,0.9);
          border-radius: 50%;
          z-index: 16;
          animation: float-away 2s forwards;
          transform: translate(var(--tx, 0), var(--ty, 0));
        }
        
        @keyframes float-away {
          0% { opacity: 1; transform: translate(0, 0); }
          100% { opacity: 0; transform: translate(var(--tx), var(--ty)); }
        }
        
        /* Cráter del cuerpo extraño */
        .foreign-body-crater {
          position: absolute;
          background: radial-gradient(circle, rgba(100,0,0,0.8) 0%, rgba(50,0,0,0.6) 70%, transparent 100%);
          border-radius: 50%;
          z-index: 14;
        }
        
        /* VISUALIZACIÓN OCT - Mejorado */
        #oct-container {
          position: absolute;
          right: 20px;
          width: 280px;
          height: 180px;
          background: rgba(0,0,0,0.7);
          border-radius: 1px;
          box-shadow: 0 0 15px rgba(0,0,0,0.5);
          border: 10px solid rgba(255,255,255,0.1);
        }
        
        #oct-scan {
          width: 100%;
          height: 100%;
        }
        
        .oct-layer {
          stroke-width: 1.5;
          fill: none;
        }
        
        #oct-retina {
          stroke: #00aaff;
          stroke-width: 2;
        }
        
        #oct-choroid {
          stroke: #0066aa;
        }
        
        #oct-sclera {
          stroke: #ffffff;
          stroke-width: 1;
        }
        
        #oct-foreign-body {
          stroke: #ff5555;
          stroke-width: 2;
          fill: rgba(255,100,100,0.3);
        }
        
        .oct-scanline {
          stroke: rgba(255,255,255,0.8);
          stroke-width: 1;
          stroke-dasharray: 5,5;
          animation: scanline 2s linear infinite;
        }
        
        @keyframes scanline {
          from { transform: translateY(-100%); }
          to { transform: translateY(100%); }
        }
        
        /* Retina y efectos visuales */
        .macula {
          position: absolute;
          width: 30px;
          height: 30px;
          border-radius: 50%;
          background: rgba(200, 200, 100, 0.3);
          top: 50%;
          left: 50%;
          transform: translate(calc(-50% + 100px), -50%);
          box-shadow: 0 0 20px rgba(200, 200, 100, 0.5);
        }
        
        /* Vitreo removido */
        .vitreous-particle {
          position: absolute;
          width: 4px;
          height: 4px;
          background: rgba(200, 200, 255, 0.8);
          border-radius: 50%;
          z-index: 12;
          animation: float-away 1.5s forwards;
          transform: translate(var(--tx, 0), var(--ty, 0));
        }
    </style>
</head>
<body>
  <!-- CASO CLÍNICO -->
  <div id="clinical-case">
    <div id="clinical-case-content">
      <h2>CASO CLÍNICO: CUERPO EXTRAÑO INTRAOCULAR</h2>
      
      <h3>Paciente:</h3>
      <p>Varón de 35 años, trauma ocular por martillado de metal</p>
      
      <h3>Síntomas:</h3>
      <p>Dolor ocular intenso, visión borrosa, fotofobia</p>
      
      <h3>Hallazgos:</h3>
      <ul>
        <li>Cuerpo extraño metálico en vítreo posterior</li>
        <li>Hemorragia vítrea moderada</li>
        <li>Edema macular</li>
        <li>Desgarro retiniano asociado</li>
      </ul>
      
      <h3>OBJETIVOS QUIRÚRGICOS:</h3>
      <ol>
        <li>Realizar vitrectomía posterior para limpieza de hemorragia</li>
        <li>Extraer cuerpo extraño con pinzas intraoculares</li>
        <li>Aplicar endoláser en desgarro retiniano</li>
      </ol>
      
      <button id="start-simulation-btn">Iniciar Simulación</button>
    </div>
  </div>

  <div id="container">
    <!-- SISTEMA DE ALERTAS -->
    <div id="alert-system">
      <div class="alert-container" id="retina-detachment-alert">
        <div class="alert-icon">❗</div>
        <div class="alert-content">
          <h3>DESPRENDIMIENTO DE RETINA</h3>
          <p>¡Emergencia! Se ha detectado desprendimiento retiniano. Suspender procedimiento e inyectar PFC inmediatamente.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="high-iop-alert">
        <div class="alert-icon">📈</div>
        <div class="alert-content">
          <h3>PRESIÓN INTRAOCULAR ELEVADA</h3>
          <p>PIO > 25 mmHg. Riesgo de daño al nervio óptico. Reducir flujo de irrigación y considerar paracentesis.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="vessel-damage-alert">
        <div class="alert-icon">🩸</div>
        <div class="alert-content">
          <h3>HEMORRAGIA RETINIANA</h3>
          <p>¡Daño vascular detectado! Aplicar presión suave con PFC y considerar coagulación con cauterio.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="wall-collision-alert">
        <div class="alert-icon">⚠️</div>
        <div class="alert-content">
          <h3>COLISIÓN CON ESTRUCTURA OCULAR</h3>
          <p>¡Advertencia! El instrumento ha contactado con la pared ocular. Retire inmediatamente y reevalúe la posición.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="vision-loss-alert">
        <div class="alert-icon">👁️</div>
        <div class="alert-content">
          <h3>PÉRDIDA DE VISIÓN</h3>
          <p>¡Precaución! Se ha aplicado cauterio en la mácula. Riesgo de pérdida visual permanente.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="surgery-complete-alert">
        <div class="alert-icon">🏥</div>
        <div class="alert-content">
          <h3>CIRUGÍA COMPLETADA</h3>
          <p>Procedimiento finalizado con éxito. El cuerpo extraño ha sido removido y las áreas dañadas han sido tratadas.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss" id="back-to-menu-btn">Volver al Menú</button>
      </div>
    </div>

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
          <filter id="bloodFilter">
            <feTurbulence type="fractalNoise" baseFrequency="0.05" numOctaves="3" result="turbulence" />
            <feDisplacementMap in2="turbulence" in="SourceGraphic" scale="5" xChannelSelector="R" yChannelSelector="G" />
          </filter>
        </defs>
        <rect width="800" height="600" fill="url(#bgGradient)" />
        <ellipse id="lens-minimap" cx="400" cy="150" rx="100" ry="65" fill="url(#lensGradient)" filter="url(#dropShadow)" />
        <ellipse cx="400" cy="150" rx="95" ry="60" fill="none" stroke="#B3E5FC" stroke-width="2" />
        <circle id="iris-minimap" cx="400" cy="150" r="55" fill="none" stroke="url(#irisGradient)" stroke-width="15" />
        <circle cx="400" cy="150" r="30" fill="#000000" />
        <circle id="retina-wall" cx="400" cy="300" r="250" fill="none" stroke="#E0E0E0" stroke-width="4" />
        <circle id="retina-minimap" cx="400" cy="300" r="240" fill="#400000" opacity="0.8" filter="url(#bloodFilter)" />
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
      <button class="toggle-btn" id="btn-forceps">Pinzas</button>
      <button class="toggle-btn" id="btn-laser">Láser</button>
      <button class="toggle-btn" id="btn-cautery">Cauterio</button>
    </div>

    <!-- PANEL DE PARÁMETROS -->
    <div class="control-panel">
      <div class="vital-sign">
        <span class="vital-label">Presión Intraocular:</span>
        <span class="vital-value normal" id="iop-value">20 mmHg</span>
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
        <span class="vital-label">Cuerpo Extraño:</span>
        <span class="vital-value" id="foreign-body-value">Presente</span>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Puntos de Láser:</span>
        <span class="vital-value" id="laser-value">0/15</span>
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
            <path d="M150,350 Q200,250 250,350 T350,350" fill="none" stroke="#8b0000" stroke-width="1.2" stroke-opacity="0.5"/>
            <path d="M450,250 Q400,300 450,350" fill="none" stroke="#8b0000" stroke-width="1.3" stroke-opacity="0.6"/>
            <path d="M200,200 Q250,150 300,200 T400,200" fill="none" stroke="#8b0000" stroke-width="1.1" stroke-opacity="0.5"/>
            <path d="M400,400 Q350,450 400,500" fill="none" stroke="#8b0000" stroke-width="1.4" stroke-opacity="0.6"/>
          </svg>
        </div>
        <div id="light-mask"></div>
        <div id="light-reflection"></div>
        <div id="light-scatter" class="light-scatter"></div>
        <div id="specular-highlight" class="specular-highlight"></div>
        
        <!-- Instrumento único completo -->
        <div class="instrument-container">
          <div id="instrument-base" class="instrument-base"></div>
          <div id="instrument-rod" class="instrument-rod"></div>
          <div id="instrument-tip" class="instrument-tip"></div>
          <div id="instrument-action-indicator" class="instrument-action-indicator">Vitrectomía</div>
        </div>
        
        <!-- Cuerpo extraño -->
        <div id="foreign-body" class="foreign-body"></div>
        
        <!-- Coágulos de sangre -->
        <div id="blood-clots"></div>
        
        <!-- Marcas permanentes de láser y cauterio -->
        <div id="permanent-marks"></div>
        
        <!-- Cráter del cuerpo extraño -->
        <div id="foreign-body-crater" class="foreign-body-crater"></div>
      </div>
    </div>

    <!-- VISUALIZACIÓN OCT - Mejorado -->
    <div id="oct-container">
      <svg id="oct-scan" viewBox="0 0 300 120" preserveAspectRatio="xMidYMid meet">
        <defs>
          <linearGradient id="octGradient" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" stop-color="#000000" stop-opacity="0" />
            <stop offset="100%" stop-color="#000000" stop-opacity="0.8" />
          </linearGradient>
          <pattern id="octPattern" patternUnits="userSpaceOnUse" width="300" height="120">
            <rect width="300" height="120" fill="url(#octGradient)" />
            <path id="oct-retina" class="oct-layer" d="M0,40 Q75,30 150,40 T300,40" />
            <path id="oct-choroid" class="oct-layer" d="M0,60 Q75,50 150,60 T300,60" />
            <path id="oct-sclera" class="oct-layer" d="M0,80 Q75,70 150,80 T300,80" />
            <path id="oct-foreign-body" d="" />
          </pattern>
        </defs>
        <rect width="300" height="120" fill="url(#octPattern)" />
        <line class="oct-scanline" x1="0" y1="0" x2="300" y2="0" />
      </svg>
    </div>

    <!-- INDICADOR DE PROFUNDIDAD -->
    <div class="depth-indicator" id="depth-indicator">
      Profundidad: Media
    </div>
    
    <!-- EFECTO DE HEMORRAGIA -->
    <div id="hemorrhage-effect" class="hemorrhage-overlay"></div>
  </div>
  <script>
/* ================== VARIABLES GLOBALES ================== */
let lightJoystickX = 50, lightJoystickY = 50;
let vitrectomoJoystickX = 50, vitrectomoJoystickY = 50;
let currentDepth = parseInt(document.getElementById('vitrectomo-z-slider').value);
let activeInstrument = null;
let iop = 20; // mmHg (valor inicial ajustado a 20)
let perfusion = 95; // %
let vitreousRemoved = 0; // % de vítreo removido
let retinaStatus = 'Estable';
let foreignBodyPresent = true;
let bloodClots = [];
let laserBurns = [];
let cauteryMarks = [];
let hemorrhageActive = false;
let wallCollisionActive = false;
let retinaDetached = false;
let procedureStep = 0; // 0: vitrectomía, 1: extracción cuerpo extraño, 2: láser
let isTouchingLight = false;
let isTouchingVitrectomo = false;
let lastLightX = 50, lastLightY = 50;
let lastVitrectomoX = 50, lastVitrectomoY = 50;
let joystickSensitivity = 0.7;
let activeTouchId = null;
let isActionButtonPressed = false;
let vitrectomyInterval = null;
let foreignBodyX = 0, foreignBodyY = 0;
let foreignBodySize = 0;
let laserPoints = 0;
let isGrasping = false;
let foreignBodyGrabbed = false;

/* ================== INICIALIZACIÓN ================== */
document.addEventListener('DOMContentLoaded', function() {
  // Mostrar caso clínico al inicio
  document.getElementById('start-simulation-btn').addEventListener('click', function() {
    anime({
      targets: '#clinical-case',
      opacity: 0,
      duration: 500,
      easing: 'easeInOutQuad',
      complete: () => {
        document.getElementById('clinical-case').style.display = 'none';
        initSimulation();
      }
    });
  });
  
  // Botón para volver al menú
  document.getElementById('back-to-menu-btn').addEventListener('click', function() {
    location.reload();
  });
});

function initSimulation() {
  // Configurar el instrumento único
  setupSingleInstrument();
  
  initJoysticks();
  setupEventListeners();
  setupAlertDismissButtons();
  updateVitals();
  updateEndoLightEffect(50, 50);
  
  // Crear cuerpo extraño inicial y coágulos de sangre
  createForeignBody();
  createBloodClots(5);
  
  requestAnimationFrame(updateInstrumentPositions);
}

function setupSingleInstrument() {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  
  // Configurar la base del instrumento (fija en la parte inferior derecha)
  const base = document.getElementById('instrument-base');
  base.style.left = (retinaRect.width * 0.85 - 15) + 'px';
  base.style.top = (retinaRect.height * 0.85 - 15) + 'px';
  
  // Configurar la punta del instrumento (inicialmente en el centro)
  const tip = document.getElementById('instrument-tip');
  tip.style.left = (retinaRect.width / 2) + 'px';
  tip.style.top = (retinaRect.height / 2) + 'px';
  
  // Actualizar el palo para que conecte base y punta
  updateInstrumentRod();
}

function updateInstrumentRod() {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  const base = document.getElementById('instrument-base');
  const baseRect = base.getBoundingClientRect();
  const tip = document.getElementById('instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  const rod = document.getElementById('instrument-rod');
  
  // Posiciones absolutas
  const baseX = baseRect.left - retinaRect.left + baseRect.width/2;
  const baseY = baseRect.top - retinaRect.top + baseRect.height/2;
  const tipX = tipRect.left - retinaRect.left + tipRect.width/2;
  const tipY = tipRect.top - retinaRect.top + tipRect.height/2;
  
  // Calcular longitud y ángulo
  const dx = tipX - baseX;
  const dy = tipY - baseY;
  const length = Math.sqrt(dx * dx + dy * dy);
  const angle = Math.atan2(dy, dx) * (180 / Math.PI);
  
  // Posicionar y rotar el palo
  rod.style.width = length + 'px';
  rod.style.left = baseX + 'px';
  rod.style.top = baseY + 'px';
  rod.style.transform = `rotate(${angle}deg)`;
}

function createForeignBody() {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  const foreignBody = document.getElementById('foreign-body');
  
  // Posicionar el cuerpo extraño en el cuadrante inferior izquierdo (50% del radio desde el centro)
  const angle = Math.PI * 1.25; // 225 grados (inferior izquierdo)
  const distance = retinaRect.width * 0.25; // 50% del radio
  
  foreignBodyX = retinaRect.width/2 + Math.cos(angle) * distance;
  foreignBodyY = retinaRect.height/2 + Math.sin(angle) * distance;
  foreignBodySize = 25 + Math.random() * 15; // Tamaño entre 25-40px
  
  foreignBody.style.left = `${foreignBodyX}px`;
  foreignBody.style.top = `${foreignBodyY}px`;
  foreignBody.style.width = `${foreignBodySize}px`;
  foreignBody.style.height = `${foreignBodySize}px`;
  
  // Actualizar OCT para mostrar el cuerpo extraño
  updateOCTScan();
}

function updateOCTScan() {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  const octForeignBody = document.getElementById('oct-foreign-body');
  
  if (!foreignBodyPresent) {
    octForeignBody.setAttribute('d', '');
    return;
  }
  
  // Calcular posición relativa del cuerpo extraño
  const relX = (foreignBodyX / retinaRect.width) * 300;
  const relY = (foreignBodyY / retinaRect.height) * 120;
  const relSize = (foreignBodySize / retinaRect.width) * 100;
  
  // Crear forma de cuerpo extraño en OCT
  const octPath = `
    M${relX-relSize/2},${relY-relSize/3} 
    Q${relX-relSize/4},${relY-relSize/2} ${relX},${relY-relSize/3}
    T${relX+relSize/2},${relY-relSize/3}
    Q${relX+relSize/4},${relY+relSize/3} ${relX},${relY+relSize/2}
    T${relX-relSize/2},${relY-relSize/3}
    Z
  `;
  
  octForeignBody.setAttribute('d', octPath);
  
  // Distorsionar capas de retina donde está el cuerpo extraño
  const retinaDistortion = `
    M0,40 
    Q${relX-relSize},30 ${relX-relSize/2},${relY-relSize/3} 
    Q${relX},${relY-relSize/2} ${relX+relSize/2},${relY-relSize/3}
    Q${relX+relSize},30 300,40
  `;
  
  document.getElementById('oct-retina').setAttribute('d', retinaDistortion);
  
  // Distorsión similar para coroides
  const choroidDistortion = `
    M0,60 
    Q${relX-relSize},50 ${relX-relSize/2},${relY-relSize/6} 
    Q${relX},${relY} ${relX+relSize/2},${relY-relSize/6}
    Q${relX+relSize},50 300,60
  `;
  
  document.getElementById('oct-choroid').setAttribute('d', choroidDistortion);
}

/* ================== SISTEMA DE ALERTAS ================== */
function setupAlertDismissButtons() {
  document.querySelectorAll('.alert-dismiss').forEach(btn => {
    btn.addEventListener('click', function() {
      const alert = this.parentElement;
      anime({
        targets: alert,
        opacity: 0,
        translateY: -20,
        duration: 300,
        easing: 'easeOutQuad',
        complete: () => {
          alert.style.display = 'none';
          alert.style.opacity = '1';
          alert.style.transform = 'translateY(0)';
          if(alert.id === 'wall-collision-alert') {
            wallCollisionActive = false;
          }
        }
      });
    });
  });
}

function showAlert(alertId, duration = 5000) {
  const alert = document.getElementById(alertId);
  if (!alert) return;
  
  alert.style.display = 'flex';
  anime({
    targets: alert,
    opacity: [0, 1],
    translateY: [-20, 0],
    duration: 300,
    easing: 'easeOutQuad'
  });
  
  const timer = alert.querySelector('.alert-timer');
  if (timer) {
    anime({
      targets: timer,
      width: ['100%', '0%'],
      duration: duration,
      easing: 'linear'
    });
  }
  
  if (duration > 0) {
    setTimeout(() => {
      anime({
        targets: alert,
        opacity: 0,
        translateY: -20,
        duration: 300,
        easing: 'easeOutQuad',
        complete: () => {
          alert.style.display = 'none';
          alert.style.opacity = '1';
          alert.style.transform = 'translateY(0)';
          if(alertId === 'wall-collision-alert') {
            wallCollisionActive = false;
          }
        }
      });
    }, duration);
  }
  
  if(alertId === 'wall-collision-alert') {
    wallCollisionActive = true;
  }
}

function checkWallCollision() {
  const miniMapRect = document.getElementById('miniMapContainer').getBoundingClientRect();
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  
  // Verificar colisión para la punta del instrumento
  const tip = document.getElementById('instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  const tipCenterX = tipRect.left + tipRect.width/2;
  const tipCenterY = tipRect.top + tipRect.height/2;
  
  // Coordenadas relativas al mini mapa
  const relX = (tipCenterX - retinaRect.left) / retinaRect.width * miniMapRect.width;
  const relY = (tipCenterY - retinaRect.top) / retinaRect.height * miniMapRect.height;
  
  // Verificar contacto con retina en mini mapa
  const retinaWall = document.getElementById('retina-wall');
  const retinaX = 400 * (miniMapRect.width / 800);
  const retinaY = 300 * (miniMapRect.height / 600);
  const retinaRadius = 250 * (miniMapRect.width / 800);
  
  const distance = Math.sqrt(Math.pow(relX - retinaX, 2) + Math.pow(relY - retinaY, 2));
  
  // Solo mostrar alerta si estamos en el borde (90% del radio)
  if (distance > retinaRadius * 1.1 && !wallCollisionActive) {
    showAlert('wall-collision-alert');
    return true;
  }
  
  // Verificar colisión para el endoiluminador
  const lightPosX = retinaRect.left + retinaRect.width * lightJoystickX / 100;
  const lightPosY = retinaRect.top + retinaRect.height * lightJoystickY / 100;
  
  // Coordenadas relativas al mini mapa
  const lightRelX = (lightPosX - retinaRect.left) / retinaRect.width * miniMapRect.width;
  const lightRelY = (lightPosY - retinaRect.top) / retinaRect.height * miniMapRect.height;
  
  // Verificar contacto con retina para el endoiluminador
  const lightDistance = Math.sqrt(Math.pow(lightRelX - retinaX, 2) + Math.pow(lightRelY - retinaY, 2));
  
  if (lightDistance > retinaRadius * 1.1 && !wallCollisionActive) {
    showAlert('wall-collision-alert');
    return true;
  }
  
  return false;
}

function checkVesselDamage() {
  if (currentDepth > -200 || !hemorrhageActive) return false;
  
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const tip = document.getElementById('instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  
  const tipCenterX = tipRect.left + tipRect.width/2;
  const tipCenterY = tipRect.top + tipRect.height/2;
  
  const bloodVessels = document.querySelector('.blood-vessels');
  const bloodVesselsRect = bloodVessels.getBoundingClientRect();
  
  const relX = tipCenterX - bloodVesselsRect.left;
  const relY = tipCenterY - bloodVesselsRect.top;
  
  const proximityThreshold = 30;
  const vesselProximity = Math.abs(relX - bloodVesselsRect.width/2) < proximityThreshold && 
                        Math.abs(relY - bloodVesselsRect.height/2) < proximityThreshold;
  
  if (vesselProximity && hemorrhageActive) {
    showAlert('vessel-damage-alert', 0);
    
    document.getElementById('hemorrhage-effect').style.display = 'block';
    anime({
      targets: '#hemorrhage-effect',
      opacity: 0.7,
      duration: 1000,
      easing: 'easeOutQuad'
    });
    
    createBloodClots(5, {x: tipCenterX, y: tipCenterY});
    
    perfusion -= 15;
    iop += 5;
    updateVitals();
    
    return true;
  }
  return false;
}

function createBloodClots(count, position = null) {
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const bloodClotsContainer = document.getElementById('blood-clots');
  
  for (let i = 0; i < count; i++) {
    const clot = document.createElement('div');
    clot.className = 'blood-clot';
    
    let x, y;
    if (position) {
      x = position.x - retinaRect.left + (Math.random() * 40 - 20);
      y = position.y - retinaRect.top + (Math.random() * 40 - 20);
    } else {
      const angle = Math.random() * Math.PI * 2;
      const distance = Math.random() * retinaRect.width * 0.4;
      x = retinaRect.width/2 + Math.cos(angle) * distance;
      y = retinaRect.height/2 + Math.sin(angle) * distance;
    }
    
    clot.style.left = `${x}px`;
    clot.style.top = `${y}px`;
    const size = 10 + Math.random() * 20;
    clot.style.width = `${size}px`;
    clot.style.height = `${size}px`;
    
    // Animación de aparición con anime.js
    anime({
      targets: clot,
      scale: [0, 1],
      opacity: [0, 1],
      duration: 500,
      easing: 'easeOutElastic'
    });
    
    bloodClotsContainer.appendChild(clot);
    bloodClots.push({
      element: clot,
      x: x,
      y: y,
      size: size
    });
  }
}

function removeBloodClot(index) {
  if (index >= 0 && index < bloodClots.length) {
    const clot = bloodClots[index];
    if (clot.element.parentNode) {
      // Efecto de extracción de sangre
      createBloodExtractionEffect(clot.x + clot.size/2, clot.y + clot.size/2, clot.size);
      
      anime({
        targets: clot.element,
        opacity: 0,
        scale: 0.5,
        duration: 500,
        easing: 'easeInOutQuad',
        complete: () => {
          if (clot.element.parentNode) {
            clot.element.parentNode.removeChild(clot.element);
          }
        }
      });
    }
    bloodClots.splice(index, 1);
  }
}

function createBloodExtractionEffect(x, y, size) {
  const retina = document.getElementById('retina');
  const bloodEffect = document.createElement('div');
  bloodEffect.className = 'blood-extraction';
  bloodEffect.style.left = `${x - 20}px`;
  bloodEffect.style.top = `${y - 20}px`;
  
  // Ajustar tamaño según el coágulo
  const effectSize = Math.max(20, Math.min(60, size * 2));
  bloodEffect.style.width = `${effectSize}px`;
  bloodEffect.style.height = `${effectSize}px`;
  
  anime({
    targets: bloodEffect,
    scale: [0, 1.5],
    opacity: [1, 0],
    duration: 800,
    easing: 'easeOutQuad',
    complete: () => {
      if (bloodEffect.parentNode) {
        bloodEffect.parentNode.removeChild(bloodEffect);
      }
    }
  });
  
  retina.appendChild(bloodEffect);
}

function removeNearbyBloodClots() {
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const tip = document.getElementById('instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  
  // Calcular posición central de la punta relativa a la retina
  const tipCenterX = tipRect.left - retinaRect.left + tipRect.width/2;
  const tipCenterY = tipRect.top - retinaRect.top + tipRect.height/2;
  
  // Hacer una copia del array para evitar problemas al modificar durante la iteración
  const clotsToCheck = [...bloodClots];
  
  clotsToCheck.forEach((clot, index) => {
    const dx = tipCenterX - clot.x;
    const dy = tipCenterY - clot.y;
    const distance = Math.sqrt(dx * dx + dy * dy);
    
    if (distance < clot.size + tipRect.width/2) {
      // Encontrar el índice real en el array bloodClots
      const realIndex = bloodClots.findIndex(c => 
        c.x === clot.x && c.y === clot.y && c.size === clot.size
      );
      
      if (realIndex !== -1) {
        removeBloodClot(realIndex);
        
        // Crear partículas de succión
        for (let i = 0; i < 5; i++) {
          const particle = document.createElement('div');
          particle.className = 'suction-particle';
          particle.style.left = `${clot.x + (Math.random()*20 - 10)}px`;
          particle.style.top = `${clot.y + (Math.random()*20 - 10)}px`;
          
          anime({
            targets: particle,
            translateX: Math.random() * 40 - 20,
            translateY: Math.random() * 40 - 20,
            opacity: [1, 0],
            duration: 1500,
            easing: 'easeOutQuad',
            complete: () => {
              if (particle.parentNode) {
                particle.parentNode.removeChild(particle);
              }
            }
          });
          
          document.getElementById('retina').appendChild(particle);
        }
      }
    }
  });
}

function checkRetinaDetachment() {
  if (iop > 28 && !retinaDetached) {
    showAlert('retina-detachment-alert');
    retinaDetached = true;
    return true;
  }
  return false;
}

function checkHighIOP() {
  if (iop > 25) {
    showAlert('high-iop-alert', 0);
    document.querySelector('.retina-nerve').style.backgroundColor = 'rgba(255,255,255,0.8)';
    document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(255,255,255,0.6), 0 0 25px rgba(255,255,255,0.5)';
    return true;
  } else {
    document.querySelector('.retina-nerve').style.backgroundColor = '';
    document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(220,80,80,0.6), 0 0 25px rgba(220,80,80,0.5)';
    document.getElementById('high-iop-alert').style.display = 'none';
    return false;
  }
}

/* ================== CONTROL DE JOYSTICKS MEJORADO ================== */
function initJoysticks() {
  const joystickVitrectomo = document.getElementById('joystick-vitrectomo');
  initJoystick(joystickVitrectomo, (x, y) => {
    vitrectomoJoystickX = x;
    vitrectomoJoystickY = y;
    lastVitrectomoX = x;
    lastVitrectomoY = y;
    updateInstrumentPosition(x, y);
    updateMiniLeftLine(x, y);
    checkWallCollision();
  }, 'vitrectomo');
  
  const joystickLight = document.getElementById('joystick-light');
  initJoystick(joystickLight, (x, y) => {
    lightJoystickX = x;
    lightJoystickY = y;
    lastLightX = x;
    lastLightY = y;
    updateEndoLightEffect(x, y);
    updateMiniRightLine(x, y);
    checkWallCollision();
  }, 'light');
}

function initJoystick(joystickElement, updateCallback, type) {
  const handle = joystickElement.querySelector('.joystick-handle');
  const rect = joystickElement.getBoundingClientRect();
  const centerX = rect.width / 2;
  const centerY = rect.height / 2;
  const maxDistance = rect.width / 2 * joystickSensitivity;
  
  let isDragging = false;
  let touchId = null;
  
  function handleStart(e) {
    e.preventDefault();
    if (isDragging) return;
    
    isDragging = true;
    if (e.touches) {
      // Para multitouch, encontrar el touch correcto
      const touches = Array.from(e.touches);
      const touch = touches.find(t => {
        const element = document.elementFromPoint(t.clientX, t.clientY);
        return element === joystickElement || element === handle;
      });
      
      if (!touch) return;
      touchId = touch.identifier;
      activeTouchId = touchId;
      handleMove(e);
    } else {
      handleMove(e);
    }
  }
  
  function handleMove(e) {
    if (!isDragging) return;
    
    let clientX, clientY;
    
    if (e.touches) {
      // Encontrar el touch específico
      const touches = Array.from(e.touches);
      const touch = touches.find(t => t.identifier === touchId);
      if (!touch) return;
      
      clientX = touch.clientX;
      clientY = touch.clientY;
    } else {
      clientX = e.clientX;
      clientY = e.clientY;
    }
    
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
    
    // Suavizar el movimiento con anime.js
    anime({
      targets: handle,
      translateX: deltaX,
      translateY: deltaY,
      duration: 100,
      easing: 'easeOutQuad'
    });
    
    // Normalizar las coordenadas (0-100)
    const normalizedX = ((deltaX + maxDistance) / (2 * maxDistance)) * 100;
    const normalizedY = ((deltaY + maxDistance) / (2 * maxDistance)) * 100;
    
    updateCallback(normalizedX, normalizedY);
  }
  
  function handleEnd(e) {
    if (!isDragging) return;
    
    // Verificar si el touch que terminó es el que controla este joystick
    if (e.touches) {
      const touches = Array.from(e.changedTouches);
      const touch = touches.find(t => t.identifier === touchId);
      if (!touch) return;
    }
    
    isDragging = false;
    touchId = null;
    activeTouchId = null;
    
    // Suavizar el retorno a la posición central con anime.js
    anime({
      targets: handle,
      translateX: 0,
      translateY: 0,
      duration: 300,
      easing: 'easeOutElastic(1, 0.5)'
    });
    
    // Solo resetear la posición del joystick, no del instrumento
    if (type === 'light') {
      updateCallback(50, 50);
    }
  }
  
  // Eventos táctiles mejorados para multitouch
  joystickElement.addEventListener('touchstart', handleStart, { passive: false });
  document.addEventListener('touchmove', (e) => {
    if (isDragging) {
      e.preventDefault();
      handleMove(e);
    }
  }, { passive: false });
  
  document.addEventListener('touchend', handleEnd);
  document.addEventListener('touchcancel', handleEnd);
  
  // Eventos de ratón
  joystickElement.addEventListener('mousedown', handleStart);
  document.addEventListener('mousemove', handleMove);
  document.addEventListener('mouseup', handleEnd);
}

/* ================== ACTUALIZACIÓN DE POSICIONES ================== */
function updateInstrumentPositions() {
  updateInstrumentPosition(vitrectomoJoystickX, vitrectomoJoystickY);
  requestAnimationFrame(updateInstrumentPositions);
}

function updateInstrumentPosition(normX, normY) {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  const base = document.getElementById('instrument-base');
  const tip = document.getElementById('instrument-tip');
  const rod = document.getElementById('instrument-rod');
  
  // Calcular posición de la punta basada en el joystick
  const maxOffset = retinaRect.width * 0.4; // Reducir el rango para que no salga de la retina
  const offsetX = (normX - 50) / 50 * maxOffset;
  const offsetY = (normY - 50) / 50 * maxOffset;
  
  // Posición absoluta de la punta
  const tipX = retinaRect.width / 2 + offsetX;
  const tipY = retinaRect.height / 2 + offsetY;
  
  // Posición de la base (fija en la parte inferior derecha)
  const baseX = retinaRect.width * 0.85;
  const baseY = retinaRect.height * 0.85;
  
  // Actualizar posición de la punta
  tip.style.left = tipX + 'px';
  tip.style.top = tipY + 'px';
  
  // Actualizar el palo para que conecte base y punta
  updateInstrumentRod();
  
  // Actualizar profundidad
  tip.style.transform = `translateZ(${currentDepth}px)`;
  
  checkWallCollision();
  
  if (currentDepth <= -200) {
    checkVesselDamage();
  }
  
  updateDepthIndicator();
  
  // Verificar si estamos cerca del cuerpo extraño
  if (foreignBodyPresent && activeInstrument === 'forceps') {
    checkForeignBodyGrasp();
  }
  
  // Si el cuerpo extraño está agarrado, moverlo con la punta
  if (foreignBodyGrabbed) {
    const foreignBody = document.getElementById('foreign-body');
    foreignBody.style.left = `${tipX}px`;
    foreignBody.style.top = `${tipY}px`;
  }
}

function checkForeignBodyGrasp() {
  if (!foreignBodyPresent || activeInstrument !== 'forceps') return;
  
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const tip = document.getElementById('instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  const foreignBody = document.getElementById('foreign-body');
  
  const tipCenterX = tipRect.left - retinaRect.left + tipRect.width/2;
  const tipCenterY = tipRect.top - retinaRect.top + tipRect.height/2;
  
  // Calcular distancia al cuerpo extraño
  const dx = tipCenterX - foreignBodyX;
  const dy = tipCenterY - foreignBodyY;
  const distance = Math.sqrt(dx * dx + dy * dy);
  
  // Si está lo suficientemente cerca y se está accionando el instrumento
  if (distance < foreignBodySize && isActionButtonPressed && !isGrasping) {
    // Agarrar el cuerpo extraño
    isGrasping = true;
    foreignBodyGrabbed = true;
    foreignBody.classList.add('grabbed');
    tip.classList.add('grasping');
    
    // Actualizar indicador de acción
    const actionIndicator = document.getElementById('instrument-action-indicator');
    actionIndicator.textContent = "Agarrando cuerpo extraño";
    actionIndicator.classList.add('active');
    
    // Después de 2 segundos, permitir mover el cuerpo extraño
    setTimeout(() => {
      if (isActionButtonPressed) {
        // El usuario sigue presionando, puede mover el cuerpo extraño
        foreignBodyGrabbed = true;
      } else {
        // El usuario soltó el botón, soltar el cuerpo extraño
        releaseForeignBody();
      }
    }, 2000);
  } else if (distance < foreignBodySize && foreignBodyGrabbed && !isActionButtonPressed) {
    // Soltar el cuerpo extraño si se suelta el botón de acción
    releaseForeignBody();
  }
}

function releaseForeignBody() {
  isGrasping = false;
  foreignBodyGrabbed = false;
  const foreignBody = document.getElementById('foreign-body');
  foreignBody.classList.remove('grabbed');
  const tip = document.getElementById('instrument-tip');
  tip.classList.remove('grasping');
  
  // Ocultar indicador de acción
  const actionIndicator = document.getElementById('instrument-action-indicator');
  actionIndicator.classList.remove('active');
}

function removeForeignBody() {
  foreignBodyPresent = false;
  foreignBodyGrabbed = false;
  const foreignBody = document.getElementById('foreign-body');
  foreignBody.style.display = 'none';
  document.getElementById('foreign-body-value').textContent = 'Removido';
  
  // Crear efecto de extracción
  for (let i = 0; i < 15; i++) {
    setTimeout(() => {
      const particle = document.createElement('div');
      particle.className = 'foreign-body-particle';
      particle.style.left = `${foreignBodyX}px`;
      particle.style.top = `${foreignBodyY}px`;
      particle.style.setProperty('--tx', Math.random() * 100 - 50);
      particle.style.setProperty('--ty', Math.random() * 100 - 50);
      document.getElementById('retina').appendChild(particle);
      
      setTimeout(() => {
        if (particle.parentNode) {
          particle.parentNode.removeChild(particle);
        }
      }, 2000);
    }, i * 100);
  }
  
  // Crear cráter donde estaba el cuerpo extraño
  const crater = document.getElementById('foreign-body-crater');
  crater.style.left = `${foreignBodyX - foreignBodySize/2}px`;
  crater.style.top = `${foreignBodyY - foreignBodySize/2}px`;
  crater.style.width = `${foreignBodySize}px`;
  crater.style.height = `${foreignBodySize}px`;
  
  anime({
    targets: crater,
    opacity: [0, 0.8],
    duration: 1000,
    easing: 'easeOutQuad'
  });
  
  // Avanzar al siguiente paso del procedimiento
  procedureStep = 2;
  document.getElementById('btn-forceps').classList.remove('active');
  document.getElementById('btn-laser').classList.add('active');
  activeInstrument = 'laser';
  updateInstrumentTipAppearance();
  
  // Actualizar OCT para mostrar solo el cráter
  updateOCTScan();
  
  // Mostrar mensaje de que se debe aplicar láser
  showAlert('vision-loss-alert', 3000);
  document.getElementById('vision-loss-alert').querySelector('h3').textContent = "FASE DE LÁSER";
  document.getElementById('vision-loss-alert').querySelector('p').textContent = "Aplique puntos de láser alrededor del cráter para completar la cirugía (15 puntos necesarios).";
}

function applyLaserAroundCrater() {
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const crater = document.getElementById('foreign-body-crater');
  const craterRect = crater.getBoundingClientRect();
  
  const centerX = craterRect.left - retinaRect.left + craterRect.width/2;
  const centerY = craterRect.top - retinaRect.top + craterRect.height/2;
  const radius = foreignBodySize * 1.5;
  
  // Aplicar 15 puntos de láser en un círculo alrededor del cráter
  const points = 15;
  for (let i = 0; i < points; i++) {
    setTimeout(() => {
      const angle = (i / points) * Math.PI * 2;
      const x = centerX + Math.cos(angle) * radius;
      const y = centerY + Math.sin(angle) * radius;
      
      laserFunction({
        clientX: x + retinaRect.left,
        clientY: y + retinaRect.top,
        preventDefault: () => {}
      });
      
      // Actualizar contador de puntos de láser
      laserPoints++;
      document.getElementById('laser-value').textContent = `${laserPoints}/15`;
      
      // Completar cirugía si se han aplicado todos los puntos
      if (laserPoints >= 15) {
        setTimeout(() => {
          showAlert('surgery-complete-alert');
        }, 1000);
      }
    }, i * 300);
  }
}

function updateDepthIndicator() {
  const depthIndicator = document.getElementById('depth-indicator');
  
  if (currentDepth > -80) {
    depthIndicator.textContent = "Profundidad: Superficial";
    depthIndicator.style.setProperty('--depth-color', '#3b82f6');
  } else if (currentDepth > -150) {
    depthIndicator.textContent = "Profundidad: Media";
    depthIndicator.style.setProperty('--depth-color', '#10b981');
  } else {
    depthIndicator.textContent = "Profundidad: Profunda";
    depthIndicator.style.setProperty('--depth-color', '#ef4444');
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
  
  if (zVal > 100) {
    document.getElementById('light-scatter').classList.add('active');
    document.getElementById('specular-highlight').classList.add('active');
  } else {
    document.getElementById('light-scatter').classList.remove('active');
    document.getElementById('specular-highlight').classList.remove('active');
  }
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
  
  tipX = Math.max(50, Math.min(750, tipX));
  tipY = Math.max(50, Math.min(550, tipY));
  
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
  
  tipX = Math.max(50, Math.min(750, tipX));
  tipY = Math.max(50, Math.min(550, tipY));
  
  const miniRight = document.getElementById('probeForceps');
  const miniRightInner = document.getElementById('probeForcepsInner');
  if(miniRight && miniRightInner) {
    miniRight.setAttribute('x2', tipX);
    miniRightInner.setAttribute('x2', tipX);
    miniRight.setAttribute('y2', tipY);
    miniRightInner.setAttribute('y2', tipY);
  }
}

/* ================== GESTIÓN DE INSTRUMENTOS MEJORADAS ================== */
function setupEventListeners() {
  document.getElementById('btn-vitrectomo').addEventListener('click', () => toggleInstrument('btn-vitrectomo', 'vitrectomo'));
  document.getElementById('btn-forceps').addEventListener('click', () => toggleInstrument('btn-forceps', 'forceps'));
  document.getElementById('btn-laser').addEventListener('click', () => toggleInstrument('btn-laser', 'laser'));
  document.getElementById('btn-cautery').addEventListener('click', () => toggleInstrument('btn-cautery', 'cautery'));
  
  // Mejorar los eventos para el botón de acción
  const actionButton = document.getElementById('btn-precionar');
  
  // Eventos de ratón
  actionButton.addEventListener('mousedown', startAction);
  actionButton.addEventListener('mouseup', stopAction);
  actionButton.addEventListener('mouseleave', stopAction);
  
  // Eventos táctiles mejorados
  actionButton.addEventListener('touchstart', function(e) {
    e.preventDefault();
    startAction();
  }, { passive: false });
  
  actionButton.addEventListener('touchend', function(e) {
    e.preventDefault();
    stopAction();
  });
  
  document.getElementById('vitrectomo-z-slider').addEventListener('input', function() {
    currentDepth = parseInt(this.value);
    updateInstrumentPosition(vitrectomoJoystickX, vitrectomoJoystickY);
  });
  
  document.getElementById('endo-z-slider').addEventListener('input', function() {
    updateEndoLightEffect(lightJoystickX, lightJoystickY);
  });
}

function startAction() {
  if (!activeInstrument) return;
  
  isActionButtonPressed = true;
  
  // Mostrar indicador de acción
  const actionIndicator = document.getElementById('instrument-action-indicator');
  if (actionIndicator) {
    actionIndicator.classList.add('active');
    
    // Actualizar texto según el instrumento activo
    if (activeInstrument === 'vitrectomo') {
      actionIndicator.textContent = "Vitrectomía";
    } else if (activeInstrument === 'laser') {
      actionIndicator.textContent = "Aplicando láser";
    } else if (activeInstrument === 'cautery') {
      actionIndicator.textContent = "Coagulando";
    } else if (activeInstrument === 'forceps') {
      actionIndicator.textContent = "Agarrando";
    }
  }
  
  // Iniciar el efecto continuo
  vitrectomyInterval = setInterval(() => {
    if (!isActionButtonPressed || !activeInstrument) {
      clearInterval(vitrectomyInterval);
      return;
    }
    
    const retinaRect = document.getElementById('retina').getBoundingClientRect();
    const tip = document.getElementById('instrument-tip');
    const tipRect = tip.getBoundingClientRect();
    
    // Calcular posición relativa al centro de la retina
    const offsetX = tipRect.left - retinaRect.left + tipRect.width/2 - retinaRect.width/2;
    const offsetY = tipRect.top - retinaRect.top + tipRect.height/2 - retinaRect.height/2;
    
    // Crear efecto visual
    if (activeInstrument === 'vitrectomo') {
      createVitrectomyEffect(tipRect.left + tipRect.width/2, tipRect.top + tipRect.height/2);
      removeNearbyBloodClots();
      
      // Actualizar parámetros fisiológicos
      iop = Math.max(18, iop - 0.1);
      perfusion = Math.min(100, perfusion + 0.05);
      
      // Aumentar porcentaje de vítreo removido
      vitreousRemoved = Math.min(100, vitreousRemoved + 0.5);
      document.getElementById('vitreous-value').textContent = `${Math.round(vitreousRemoved)}%`;
      document.getElementById('vitreous-level').style.width = `${vitreousRemoved}%`;
      
      // Si se ha removido suficiente vítreo, pasar a la siguiente fase
      if (vitreousRemoved >= 80 && procedureStep === 0) {
        procedureStep = 1;
        document.getElementById('btn-vitrectomo').classList.remove('active');
        document.getElementById('btn-forceps').classList.add('active');
        activeInstrument = 'forceps';
        updateInstrumentTipAppearance();
        
        // Mostrar mensaje de que se debe extraer el cuerpo extraño
        showAlert('vision-loss-alert', 3000);
        document.getElementById('vision-loss-alert').querySelector('h3').textContent = "FASE DE EXTRACCIÓN";
        document.getElementById('vision-loss-alert').querySelector('p').textContent = "Use las pinzas para extraer el cuerpo extraño. Mantenga presionado el botón de acción para agarrarlo.";
      }
    } else if (activeInstrument === 'laser' && procedureStep === 2) {
      laserFunction({ 
        clientX: tipRect.left + tipRect.width/2, 
        clientY: tipRect.top + tipRect.height/2,
        preventDefault: () => {}
      });
    } else if (activeInstrument === 'cautery') {
      cauteryFunction({ 
        clientX: tipRect.left + tipRect.width/2, 
        clientY: tipRect.top + tipRect.height/2,
        preventDefault: () => {}
      });
    } else if (activeInstrument === 'forceps' && foreignBodyGrabbed) {
      // Mover el cuerpo extraño con la pinza
      const foreignBody = document.getElementById('foreign-body');
      foreignBody.style.left = `${tipRect.left - retinaRect.left + tipRect.width/2}px`;
      foreignBody.style.top = `${tipRect.top - retinaRect.top + tipRect.height/2}px`;
      
      // Si el cuerpo extraño está cerca del borde, extraerlo
      if (tipRect.left < retinaRect.left + 50 || 
          tipRect.top < retinaRect.top + 50 || 
          tipRect.left > retinaRect.left + retinaRect.width - 50 || 
          tipRect.top > retinaRect.top + retinaRect.height - 50) {
        removeForeignBody();
      }
    }
    
    // Actualizar UI inmediatamente
    updateVitals();
    
  }, 100); // Ejecutar cada 100ms mientras se mantenga presionado
}

function stopAction() {
  isActionButtonPressed = false;
  
  // Ocultar indicador de acción
  const actionIndicator = document.getElementById('instrument-action-indicator');
  if (actionIndicator) {
    actionIndicator.classList.remove('active');
  }
  
  // Detener el efecto continuo
  if (vitrectomyInterval) {
    clearInterval(vitrectomyInterval);
    vitrectomyInterval = null;
  }
  
  // Si estábamos agarrando el cuerpo extraño, soltarlo
  if (isGrasping) {
    releaseForeignBody();
  }
}

function createVitrectomyEffect(x, y) {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  
  // Efecto de vitrectomía
  const vitrectomyEffect = document.createElement('div');
  vitrectomyEffect.className = 'vitrectomy-effect';
  vitrectomyEffect.style.left = (x - retinaRect.left - 30) + 'px';
  vitrectomyEffect.style.top = (y - retinaRect.top - 30) + 'px';
  
  anime({
    targets: vitrectomyEffect,
    scale: [0, 1],
    opacity: [0.8, 0],
    duration: 800,
    easing: 'easeOutQuad',
    complete: () => {
      if (vitrectomyEffect.parentNode) {
        vitrectomyEffect.parentNode.removeChild(vitrectomyEffect);
      }
    }
  });
  
  retina.appendChild(vitrectomyEffect);
  
  // Partículas de succión
  for (let i = 0; i < 8; i++) {
    const particle = document.createElement('div');
    particle.className = 'vitreous-particle';
    particle.style.left = (x - retinaRect.left + (Math.random()*20 - 10)) + 'px';
    particle.style.top = (y - retinaRect.top + (Math.random()*20 - 10)) + 'px';
    particle.style.setProperty('--tx', Math.random() * 40 - 20);
    particle.style.setProperty('--ty', Math.random() * 40 - 20);
    
    anime({
      targets: particle,
      translateX: Math.random() * 40 - 20,
      translateY: Math.random() * 40 - 20,
      opacity: [1, 0],
      duration: 1500,
      easing: 'easeOutQuad',
      complete: () => {
        if (particle.parentNode) {
          particle.parentNode.removeChild(particle);
        }
      }
    });
    
    retina.appendChild(particle);
  }
}

function toggleInstrument(btnId, instrumentType) {
  const btn = document.getElementById(btnId);
  
  if(btn.classList.contains('active')) {
    btn.classList.remove('active');
    activeInstrument = null;
    isActionButtonPressed = false;
    
    // Resetear apariencia de la punta
    const tip = document.getElementById('instrument-tip');
    tip.className = 'instrument-tip';
    tip.classList.remove('grasping');
    
    // Detener cualquier intervalo activo
    if (vitrectomyInterval) {
      clearInterval(vitrectomyInterval);
      vitrectomyInterval = null;
    }
  } else {
    document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
    
    btn.classList.add('active');
    activeInstrument = instrumentType;
    
    // Actualizar apariencia de la punta según el instrumento
    updateInstrumentTipAppearance();
  }
}

function updateInstrumentTipAppearance() {
  const tip = document.getElementById('instrument-tip');
  tip.className = 'instrument-tip';
  
  if (activeInstrument === 'vitrectomo') {
    tip.classList.add('active-vitrectomo');
  } else if (activeInstrument === 'laser') {
    tip.classList.add('active-laser');
  } else if (activeInstrument === 'cautery') {
    tip.classList.add('active-cautery');
  } else if (activeInstrument === 'forceps') {
    tip.classList.add('active-forceps');
  }
}

/* ================== FUNCIONES DE INSTRUMENTOS MEJORADAS ================== */
function laserFunction(e) {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  
  // Verificar que no esté en la mácula, nervio óptico o vasos sanguíneos
  if (isInSensitiveArea(e.clientX, e.clientY)) {
    return;
  }
  
  // Verificar que no esté demasiado cerca de otro punto de láser
  const tooClose = laserBurns.some(burn => {
    const dx = e.clientX - retinaRect.left - burn.x;
    const dy = e.clientY - retinaRect.top - burn.y;
    return Math.sqrt(dx * dx + dy * dy) < 30;
  });
  
  if (tooClose) return;
  
  // Crear efecto de láser temporal
  const laserSpot = document.createElement('div');
  laserSpot.className = 'laser-spot';
  laserSpot.style.left = (e.clientX - retinaRect.left - 12) + 'px';
  laserSpot.style.top = (e.clientY - retinaRect.top - 12) + 'px';
  
  anime({
    targets: laserSpot,
    scale: [0.5, 1.2, 1],
    opacity: [0, 1, 0.8],
    duration: 500,
    easing: 'easeOutQuad'
  });
  
  retina.appendChild(laserSpot);
  
  // Crear marca permanente de láser (punto blanco)
  const burnMark = document.createElement('div');
  burnMark.className = 'laser-burn-permanent';
  burnMark.style.left = (e.clientX - retinaRect.left - 3) + 'px';
  burnMark.style.top = (e.clientY - retinaRect.top - 3) + 'px';
  
  anime({
    targets: burnMark,
    scale: [0, 1],
    opacity: [0, 1],
    duration: 500,
    easing: 'easeOutElastic'
  });
  
  document.getElementById('permanent-marks').appendChild(burnMark);
  
  laserBurns.push({
    element: burnMark,
    x: e.clientX - retinaRect.left - 3,
    y: e.clientY - retinaRect.top - 3
  });
  
  // Actualizar contador de puntos de láser
  laserPoints++;
  document.getElementById('laser-value').textContent = `${laserPoints}/15`;
  
  // Completar cirugía si se han aplicado todos los puntos
  if (laserPoints >= 15) {
    setTimeout(() => {
      showAlert('surgery-complete-alert');
    }, 1000);
  }
  
  // Actualizar parámetros fisiológicos
  iop += 0.5;
  perfusion -= 0.2;
  updateVitals();
  
  // Eliminar efecto de láser después de 2.5 segundos
  setTimeout(() => {
    if (laserSpot.parentNode) {
      laserSpot.parentNode.removeChild(laserSpot);
    }
  }, 2500);
}

function cauteryFunction(e) {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  
  // Verificar que no esté en la mácula, nervio óptico o vasos sanguíneos
  if (isInSensitiveArea(e.clientX, e.clientY)) {
    return;
  }
  
  // Verificar que no esté demasiado cerca de otro punto de cauterio
  const tooClose = cauteryMarks.some(mark => {
    const dx = e.clientX - retinaRect.left - mark.x;
    const dy = e.clientY - retinaRect.top - mark.y;
    return Math.sqrt(dx * dx + dy * dy) < 30;
  });
  
  if (tooClose) return;
  
  // Efecto visual de cauterio temporal
  const cauteryEffect = document.createElement('div');
  cauteryEffect.className = 'cautery-effect';
  cauteryEffect.style.left = (e.clientX - retinaRect.left - 15) + 'px';
  cauteryEffect.style.top = (e.clientY - retinaRect.top - 15) + 'px';
  
  anime({
    targets: cauteryEffect,
    scale: [0.5, 1.3, 1],
    opacity: [0, 1, 0],
    duration: 1000,
    easing: 'easeOutQuad',
    complete: () => {
      if (cauteryEffect.parentNode) {
        cauteryEffect.parentNode.removeChild(cauteryEffect);
      }
    }
  });
  
  retina.appendChild(cauteryEffect);
  
  // Crear marca permanente de cauterio (punto blanco)
  const burnMark = document.createElement('div');
  burnMark.className = 'laser-burn-permanent';
  burnMark.style.left = (e.clientX - retinaRect.left - 3) + 'px';
  burnMark.style.top = (e.clientY - retinaRect.top - 3) + 'px';
  
  anime({
    targets: burnMark,
    scale: [0, 1],
    opacity: [0, 1],
    duration: 500,
    easing: 'easeOutElastic'
  });
  
  document.getElementById('permanent-marks').appendChild(burnMark);
  
  cauteryMarks.push({
    element: burnMark,
    x: e.clientX - retinaRect.left - 3,
    y: e.clientY - retinaRect.top - 3
  });
  
  // Actualizar parámetros fisiológicos
  perfusion += 1;
  iop = Math.min(30, iop + 0.3);
  updateVitals();
}

function isInSensitiveArea(x, y) {
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  
  // Verificar mácula
  const macula = document.querySelector('.macula');
  const maculaRect = macula.getBoundingClientRect();
  const maculaDistance = Math.sqrt(
    Math.pow(x - (maculaRect.left + maculaRect.width/2), 2) + 
    Math.pow(y - (maculaRect.top + maculaRect.height/2), 2)
  );
  
  if (maculaDistance < maculaRect.width/2) {
    showAlert('vision-loss-alert');
    return true;
  }
  
  // Verificar nervio óptico
  const opticDisc = document.querySelector('.optic-disc');
  const opticDiscRect = opticDisc.getBoundingClientRect();
  const opticDiscDistance = Math.sqrt(
    Math.pow(x - (opticDiscRect.left + opticDiscRect.width/2), 2) + 
    Math.pow(y - (opticDiscRect.top + opticDiscRect.height/2), 2)
  );
  
  if (opticDiscDistance < opticDiscRect.width/2) {
    return true;
  }
  
  // Verificar vasos sanguíneos (simplificado)
  const bloodVessels = document.querySelector('.blood-vessels');
  const bloodVesselsRect = bloodVessels.getBoundingClientRect();
  const relX = x - bloodVesselsRect.left;
  const relY = y - bloodVesselsRect.top;
  
  const proximityThreshold = 15;
  const vesselProximity = Math.abs(relX - bloodVesselsRect.width/2) < proximityThreshold && 
                        Math.abs(relY - bloodVesselsRect.height/2) < proximityThreshold;
  
  if (vesselProximity) {
    showAlert('vessel-damage-alert');
    return true;
  }
  
  return false;
}

/* ================== ACTUALIZACIÓN DE PARÁMETROS ================== */
function updateVitals() {
  // Variación natural de los parámetros
  iop += (Math.random() - 0.5) * 0.1;
  perfusion += (Math.random() - 0.5) * 0.2;
  
  // Mantener la PIO alrededor de 20 mmHg
  if (iop < 19.5) iop += 0.1;
  if (iop > 20.5) iop -= 0.1;
  
  checkRetinaDetachment();
  checkHighIOP();
  
  // Limitar valores dentro de rangos razonables
  iop = Math.max(18, Math.min(30, iop));
  perfusion = Math.max(60, Math.min(100, perfusion));
  
  // Actualizar valores en la interfaz
  document.getElementById('iop-value').innerText = iop.toFixed(1) + " mmHg";
  document.getElementById('iop-level').style.width = ((iop - 10) / 20 * 100) + "%";
  
  document.getElementById('perfusion-value').innerText = perfusion.toFixed(0) + "%";
  document.getElementById('perfusion-level').style.width = perfusion + "%";
  
  // Actualizar estado de la retina
  const retinaStatusElement = document.getElementById('retina-status');
  if (iop < 15 || iop > 25) {
    retinaStatusElement.classList.remove('normal', 'warning');
    retinaStatusElement.classList.add('danger');
    retinaStatus = 'Riesgo';
  } else if (iop < 18 || iop > 22) {
    retinaStatusElement.classList.remove('normal', 'danger');
    retinaStatusElement.classList.add('warning');
    retinaStatus = 'Alerta';
  } else {
    retinaStatusElement.classList.remove('warning', 'danger');
    retinaStatusElement.classList.add('normal');
    retinaStatus = 'Estable';
  }
  
  retinaStatusElement.innerText = retinaStatus;
  
  // Actualizar clases de estado
  const iopElement = document.getElementById('iop-value');
  iopElement.classList.remove('normal', 'warning', 'danger');
  
  if(iop < 15 || iop > 25) {
    iopElement.classList.add('danger');
  } else if(iop < 18 || iop > 22) {
    iopElement.classList.add('warning');
  } else {
    iopElement.classList.add('normal');
  }
  
  setTimeout(updateVitals, 1000);
}
 </script>
</body>
</html>
