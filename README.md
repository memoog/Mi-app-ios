
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Simulador Avanzado de Desprendimiento de Retina</title>
  <link rel="stylesheet" href="d.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <style>
    /* ================== MEJORAS PARA LAS VENAS RETINIANAS ================== */
.blood-vessels {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 3;
  pointer-events: none;
  filter: drop-shadow(0 0 3px rgba(80, 0, 0, 0.7));
  will-change: transform;
  animation: vesselPulse 3s ease-in-out infinite;
}

@keyframes vesselPulse {
  0%, 100% { 
      transform: scale(1);
      opacity: 0.8;
  }
  50% { 
      transform: scale(1.02);
      opacity: 0.9;
  }
}

/* ================== MEJORAS PARA JOYSTICKS EN MÓVIL ================== */
.joystick {
  touch-action: none;
  position: relative;
  width: 100px;
  height: 100px;
  background: rgba(255,255,255,0.1);
  border: 2px solid rgba(255,255,255,0.4);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 
      0 6px 15px rgba(0,0,0,0.4),
      inset 0 0 25px rgba(255,255,255,0.15);
  backdrop-filter: blur(8px);
  will-change: transform;
}

.joystick-handle {
  touch-action: none;
  width: 40px;
  height: 40px;
  background: rgba(255,255,255,0.8);
  border-radius: 50%;
  position: absolute;
  transition: transform 0.05s ease;
  box-shadow: 
      0 0 15px rgba(255,255,255,0.4),
      inset 0 0 8px rgba(255,255,255,0.7);
  will-change: transform;
}

/* Mejoras de respuesta táctil */
#btn-precionar {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  transition: transform 0.1s ease;
}

#btn-precionar:active {
  transform: scale(0.95);
}

/* Ajustes para pantallas pequeñas */
@media (max-width: 768px) {
  .joystick {
      width: 80px;
      height: 80px;
  }
  
  .joystick-handle {
      width: 35px;
      height: 35px;
  }
}

@media (max-width: 480px) {
  .joystick {
      width: 70px;
      height: 70px;
  }
  
  .joystick-handle {
      width: 30px;
      height: 30px;
  }
}
/* ================== GUÍA VISUAL PARA PUNTOS DE LÁSER ================== */
#laser-guide-ring {
  animation: pulse 2s infinite ease-in-out;
}

@keyframes pulse {
  0% { opacity: 0.5; transform: scale(1); }
  50% { opacity: 0.8; transform: scale(1.05); }
  100% { opacity: 0.5; transform: scale(1); }
}

/* ================== MEJORAS PARA JOYSTICKS ================== */
.joystick {
  transition: transform 0.1s ease-out;
}

.joystick-handle {
  will-change: transform;
  transition: transform 0.2s ease-out;
}

.joystick:active {
  transform: scale(0.95);
}

/* Efecto de retroalimentación táctil */
.joystick-handle:active {
  transform: scale(0.9) !important;
  box-shadow: 
    0 0 20px rgba(255,255,255,0.6),
    inset 0 0 10px rgba(255,255,255,0.8) !important;
}

/* ================== INDICADOR DE PUNTOS RESTANTES ================== */
.laser-progress-indicator {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0,0,0,0.7);
  padding: 10px 20px;
  border-radius: 20px;
  color: white;
  font-size: 0.9rem;
  z-index: 1000;
  display: flex;
  align-items: center;
  box-shadow: 0 0 10px rgba(0,0,0,0.5);
}

.laser-progress-indicator::before {
  content: 'Puntos:';
  margin-right: 10px;
  color: #aaa;
}

.laser-progress-indicator span {
  font-weight: bold;
  color: #3b82f6;
}
/* ================== INDICADORES DE PUNTOS DE LÁSER ================== */
.laser-marker-number {
  position: absolute;
  width: 20px;
  height: 20px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 50%;
  color: #d00;
  font-weight: bold;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 16;
  pointer-events: none;
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.8);
  transform: translate(-50%, -50%);
}

/* ================== EFECTOS DE RESALTADO ================== */
#retinal-hole {
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

/* ================== ANIMACIONES ================== */
@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

.pulse-effect {
  animation: pulse 0.5s ease-in-out;
}
/* MEJORAS PARA JOYSTICKS EN MÓVIL */
.joystick {
  touch-action: none;
  position: relative;
  width: 100px;
  height: 100px;
  background: rgba(255,255,255,0.1);
  border: 2px solid rgba(255,255,255,0.4);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 
    0 6px 15px rgba(0,0,0,0.4),
    inset 0 0 25px rgba(255,255,255,0.15);
  backdrop-filter: blur(8px);
  will-change: transform;
}

.joystick-handle {
  touch-action: none;
  width: 40px;
  height: 40px;
  background: rgba(255,255,255,0.8);
  border-radius: 50%;
  position: absolute;
  transition: transform 0.05s ease;
  box-shadow: 
    0 0 15px rgba(255,255,255,0.4),
    inset 0 0 8px rgba(255,255,255,0.7);
  will-change: transform;
}

/* Mejoras de respuesta táctil */
#btn-precionar {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}

/* Ajustes para pantallas pequeñas */
@media (max-width: 768px) {
  .joystick {
    width: 80px;
    height: 80px;
  }
  
  .joystick-handle {
    width: 35px;
    height: 35px;
  }
}

@media (max-width: 480px) {
  .joystick {
    width: 70px;
    height: 70px;
  }
  
  .joystick-handle {
    width: 30px;
    height: 30px;
  }
}
/* Estilos para los palitos blancos del minimapa */
#line-probeLight, #line-probeLightInner,
#line-probeForceps, #line-probeForcepsInner {
  stroke-linecap: round;
  pointer-events: none;
}

#line-probeLight, #line-probeForceps {
  stroke: #FFFFFF;
  stroke-width: 9px;
}

#line-probeLightInner, #line-probeForcepsInner {
  stroke: #FFFFFF;
  stroke-width: 6px;
}

/* Efectos de láser mejorados */
.laser-spot {
  animation: laser-pulse 0.3s ease-out;
}

@keyframes laser-pulse {
  0% { transform: translate(-50%, -50%) scale(0.8); opacity: 0; }
  50% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; }
  100% { transform: translate(-50%, -50%) scale(1); opacity: 0.8; }
}

.laser-burn-permanent {
  transition: all 0.5s ease;
}

.laser-burn-permanent:hover {
  box-shadow: 0 0 10px rgba(255, 100, 100, 0.8);
  transform: translate(-50%, -50%) scale(1.2);
}

/* Efectos de cauterio mejorados */
.cautery-effect {
  animation: cautery-pulse 0.5s ease-out forwards;
}

.cautery-mark-permanent {
  transition: all 0.5s ease;
}

.cautery-mark-permanent:hover {
  box-shadow: 0 0 8px rgba(255, 200, 100, 0.7);
  transform: translate(-50%, -50%) scale(1.5);
}
/* ================== MARCAS PERMANENTES VISIBLES ================== */
.laser-burn-permanent {
  position: absolute;
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50%;
  pointer-events: none;
  z-index: 21; /* Mayor z-index para que aparezcan sobre todo */
  box-shadow: 
    0 0 10px rgba(255, 255, 255, 0.9),
    0 0 20px rgba(255, 100, 100, 0.7);
  transform: translate(-50%, -50%);
  animation: burnPulse 2s infinite alternate;
}

.cautery-mark-permanent {
  position: absolute;
  width: 6px;
  height: 6px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50%;
  pointer-events: none;
  z-index: 21; /* Mayor z-index para que aparezcan sobre todo */
  box-shadow: 
    0 0 8px rgba(255, 255, 255, 0.8),
    0 0 15px rgba(255, 200, 100, 0.6);
  transform: translate(-50%, -50%);
  animation: burnPulse 2s infinite alternate;
}

@keyframes burnPulse {
  0% { transform: translate(-50%, -50%) scale(1); opacity: 0.9; }
  50% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; }
  100% { transform: translate(-50%, -50%) scale(1); opacity: 0.9; }
}

/* Asegurar que el contenedor de marcas esté posicionado correctamente */
#permanent-marks {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 20;
}
/* ================== NUEVAS ALERTAS ================== */
.alert-container#vision-loss-alert {
  border-left-color: #ff5555;
  background: rgba(40, 10, 10, 0.95);
  animation: alertShake 0.5s ease-out forwards;
}

.alert-container#surgery-complete-alert {
  border-left-color: #00cc66;
  background: rgba(10, 40, 20, 0.95);
}

/* ================== BURBUJAS DE GAS TRANSPARENTES ================== */
.gas-bubble {
  background: rgba(255, 255, 255, 0.2) !important;
  border: 1px solid rgba(255, 255, 255, 0.4) !important;
  box-shadow: 
    0 0 15px rgba(255, 255, 255, 0.5),
    inset 0 0 8px rgba(255, 255, 255, 0.3) !important;
}

.gas-bubble-large {
  background: radial-gradient(circle, 
    rgba(255,255,255,0.2) 0%, 
    rgba(220,220,255,0.15) 70%, 
    transparent 100%) !important;
  border: 1px solid rgba(255, 255, 255, 0.4) !important;
  box-shadow: 
    0 0 30px rgba(255,255,255,0.5),
    inset 0 0 20px rgba(255,255,255,0.3) !important;
}

/* ================== BOTÓN VOLVER AL MENÚ ================== */
#back-to-menu-btn {
  background: #3b82f6;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
  font-size: 0.9rem;
  transition: all 0.3s;
}

#back-to-menu-btn:hover {
  background: #2563eb;
}

/* ================== MEJORAS PARA JOYSTICKS EN MÓVIL ================== */
.joystick {
  touch-action: none;
}

.joystick-handle {
  touch-action: none;
}

#btn-precionar {
  touch-action: manipulation;
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
/* MEJORAS PARA JOYSTICKS */
.joystick {
  touch-action: none;
  position: relative;
  width: 100px;
  height: 100px;
  background: rgba(255,255,255,0.1);
  border: 2px solid rgba(255,255,255,0.4);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 
    0 6px 15px rgba(0,0,0,0.4),
    inset 0 0 25px rgba(255,255,255,0.15);
  backdrop-filter: blur(8px);
  will-change: transform;
}

.joystick-handle {
  touch-action: none;
  width: 40px;
  height: 40px;
  background: rgba(255,255,255,0.8);
  border-radius: 50%;
  position: absolute;
  transition: transform 0.05s ease;
  box-shadow: 
    0 0 15px rgba(255,255,255,0.4),
    inset 0 0 8px rgba(255,255,255,0.7);
  will-change: transform;
}

/* Mejoras de respuesta táctil */
#btn-precionar {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  transition: transform 0.1s ease;
}

#btn-precionar:active {
  transform: scale(0.95);
}

/* Ajustes para pantallas pequeñas */
@media (max-width: 768px) {
  .joystick {
    width: 80px;
    height: 80px;
  }
  
  .joystick-handle {
    width: 35px;
    height: 35px;
  }
}

@media (max-width: 480px) {
  .joystick {
    width: 70px;
    height: 70px;
  }
  
  .joystick-handle {
    width: 30px;
    height: 30px;
  }
}

/* Mejoras para los instrumentos */
#laser-probe .instrument-tip {
  width: 0.8mm;
  height: 1.5mm;
  background: radial-gradient(circle, #fff, #f99);
  border-radius: 50%;
  box-shadow: 
    0 0 5px rgba(255,100,100,0.8),
    inset 0 0 3px rgba(255,255,255,0.8);
}

#cautery-probe .instrument-tip {
  width: 1.2mm;
  height: 2mm;
  background: radial-gradient(circle, #ff9999, #ff3333);
  border-radius: 0.3mm;
  box-shadow: 
    0 0 5px rgba(255,100,100,0.8),
    inset 0 0 3px rgba(255,255,255,0.8);
  z-index: 1;
}
/* ================== INDICADOR DE ACCIÓN DEL INSTRUMENTO ================== */
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
  transition: opacity 0.3s;
  pointer-events: none;
  box-shadow: 0 2px 10px rgba(0,0,0,0.5);
}

.instrument-action-indicator.active {
  opacity: 1;
  animation: fadeInOut 1s ease-in-out;
}

@keyframes fadeInOut {
  0% { opacity: 0; transform: translateX(-50%) translateY(0); }
  20% { opacity: 1; transform: translateX(-50%) translateY(-10px); }
  80% { opacity: 1; transform: translateX(-50%) translateY(-10px); }
  100% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
}

/* ================== EFECTO DE PFC MEJORADO ================== */
#detached-retina-overlay {
  transition: background 1.5s ease;
  background: radial-gradient(circle at 30% 20%, rgba(100,255,255,0.3) 0%, rgba(50,200,200,0.2) 60%, transparent 100%);
}

/* ================== EFECTO DE GAS MEJORADO ================== */
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
}    /* Estilos adicionales para los instrumentos 3D */
    .instrument-tip {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%) translateZ(0);
      z-index: 1;
    }
    
    .instrument-jaws {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      z-index: 2;
    }
    
    .instrument-jaw {
      position: absolute;
      bottom: 0;
      z-index: 1;
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

    /* Estilos para efectos de láser y cauterio */
    .laser-spot {
      position: absolute;
      width: 24px;
      height: 24px;
      background: radial-gradient(circle, rgba(255,50,50,0.8) 0%, rgba(255,0,0,0.6) 70%, transparent 100%);
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(255,100,100,0.7);
      pointer-events: none;
      z-index: 20;
      animation: laser-pulse 0.5s ease-out;
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
      animation: cautery-fade 1s ease-out forwards;
    }

    .cautery-mark-permanent {
      position: absolute;
      width: 4px;
      height: 4px;
      background: rgba(255, 255, 255, 0.8);
      border-radius: 50%;
      pointer-events: none;
      z-index: 15;
    }

    @keyframes laser-pulse {
      0% { transform: scale(0.5); opacity: 0; }
      50% { transform: scale(1.2); opacity: 1; }
      100% { transform: scale(1); opacity: 0.8; }
    }

    @keyframes cautery-fade {
      0% { transform: scale(0.5); opacity: 0; }
      50% { transform: scale(1.3); opacity: 1; }
      100% { transform: scale(1); opacity: 0; }
    }

    /* Nuevo estilo para mostrar acción del instrumento */
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
      transition: opacity 0.3s;
      pointer-events: none;
    }
    
    .instrument-action-indicator.active {
      opacity: 1;
    }
  </style>
</head>
<body>
  <!-- CASO CLÍNICO -->
  <div id="clinical-case">
    <div id="clinical-case-content">
      <h2>CASO CLÍNICO: DESPRENDIMIENTO DE RETINA REGMATÓGENO</h2>
      
      <h3>Paciente:</h3>
      <p>Varón de 58 años, miope alto</p>
      
      <h3>Síntomas:</h3>
      <p>Moscas volantes y pérdida de visión súbita en cuadrante superior temporal del ojo derecho</p>
      
      <h3>Hallazgos:</h3>
      <ul>
        <li>Desgarro retiniano en cuadrante superior temporal</li>
        <li>Desprendimiento de retina parcial con afectación macular</li>
        <li>Vítreo desprendido con tracción residual</li>
      </ul>
      
      <h3>OBJETIVOS QUIRÚRGICOS:</h3>
      <ol>
        <li>Realizar vitrectomía posterior completa</li>
        <li>Identificar y marcar agujero con endocauterio</li>
        <li>Inyectar perfluorocarbono (PFC) para reaplicación retiniana</li>
        <li>Aplicar endoláser alrededor de los desgarros</li>
        <li>Considerar inyección de gas o aceite de silicona según evolución</li>
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
      
      <div class="alert-container" id="retina-fixed-alert">
        <div class="alert-icon">✅</div>
        <div class="alert-content">
          <h3>RETINA FIJADA</h3>
          <p>Procedimiento completado con éxito. La retina ha sido reposicionada y fijada con láser.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="gas-injected-alert">
        <div class="alert-icon">💨</div>
        <div class="alert-content">
          <h3>GAS INYECTADO</h3>
          <p>Gas SF6 inyectado con éxito. El paciente debe mantener posición prono durante 7 días.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss" id="inicio.html">Volver al Menú</button>
      </div>
      
      <div class="alert-container" id="vitreous-removed-alert">
        <div class="alert-icon">🧬</div>
        <div class="alert-content">
          <h3>VITREO REMOVIDO</h3>
          <p>Vitrectomía completada. Proceda a localizar el agujero retiniano con cauterio.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="hole-located-alert">
        <div class="alert-icon">📍</div>
        <div class="alert-content">
          <h3>AGUJERO LOCALIZADO</h3>
          <p>Agujero retiniano marcado. Proceda a inyectar PFC para reposicionar la retina.</p>
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
          <p>Procedimiento finalizado con éxito. La retina ha sido completamente reparada.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss" id="inicio.html">Volver al Menú</button>
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
          <filter id="wrinkleFilter" x="-20%" y="-20%" width="140%" height="140%">
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
        <circle id="retina-minimap" cx="400" cy="300" r="240" fill="#400000" opacity="0.8" />
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
      <button class="toggle-btn" id="btn-laser">Láser</button>
      <button class="toggle-btn" id="btn-pfc">PFC</button>
      <button class="toggle-btn" id="btn-cautery">Cauterio</button>
      <button class="toggle-btn" id="btn-gas">Gas SF6</button>
      <button class="toggle-btn" id="btn-simulate-detachment">Simular DR</button>
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
        <span class="vital-label">Nivel de PFC:</span>
        <span class="vital-value" id="pfc-value">0%</span>
      </div>
      <div id="pfc-gauge" class="gauge-container">
        <div class="gauge-level" id="pfc-level"></div>
      </div>
      
      <div class="vital-sign">
        <span class="vital-label">Nivel de Gas:</span>
        <span class="vital-value" id="gas-value">0%</span>
      </div>
      <div id="gas-gauge" class="gauge-container">
        <div class="gauge-level" id="gas-level"></div>
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
          <div id="detached-retina-overlay"></div>
          <div id="retina-edge-detachment"></div>
          <div id="retinal-hole"></div>
        </div>
        <div id="light-mask"></div>
        <div id="light-reflection"></div>
        <div id="light-scatter" class="light-scatter"></div>
        <div id="specular-highlight" class="specular-highlight"></div>
        
        <!-- Vitrectomo -->
        <div id="vitrectome" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
            <div class="instrument-side"></div>
          </div>
          <div class="instrument-shadow"></div>
          <div class="instrument-action-indicator">Vitrectomía</div>
        </div>
        
        <!-- Sonda de Láser -->
        <div id="laser-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
          <div class="instrument-action-indicator">Aplicando láser</div>
        </div>

        <!-- Sonda de PFC -->
        <div id="pfc-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
          <div class="instrument-action-indicator">Inyectando PFC</div>
        </div>

        <!-- Cauterio -->
        <div id="cautery-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
          <div class="instrument-action-indicator">Marcando con cauterio</div>
        </div>
        
        <!-- Jeringa de Gas -->
        <div id="gas-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
          <div class="instrument-action-indicator">Inyectando gas</div>
        </div>

        <!-- Burbujas de PFC -->
        <div id="pfc-bubbles"></div>
        
        <!-- Burbujas de Gas -->
        <div id="gas-bubbles"></div>
        
        <!-- Coágulos de sangre -->
        <div id="blood-clots"></div>
        
        <!-- Marcas permanentes de láser y cauterio -->
        <div id="permanent-marks"></div>
      </div>
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
let iop = 16; // mmHg
let perfusion = 95; // %
let vitreousRemoved = 0; // %
let pfcLevel = 0; // %
let gasLevel = 0; // %
let bloodClots = [];
let laserBurns = [];
let cauteryMarks = [];
let hemorrhageActive = false;
let wallCollisionActive = false;
let retinaDetached = false;
let retinaFixed = false;
let pfcInjected = false;
let gasInjected = false;
let procedureStep = 0; // 0: no iniciado, 1: vitrectomía, 2: localización agujero, 3: inyección PFC, 4: burbujeo, 5: láser, 6: remoción PFC, 7: inyección gas
let holeLocated = false;
let isTouchingLight = false;
let isTouchingVitrectomo = false;
let requiredMarks = 7; // Cambiado a 7 puntos como solicitaste
let marksAroundHole = 0;
let lastLightX = 50, lastLightY = 50;
let lastVitrectomoX = 50, lastVitrectomoY = 50;
let joystickSensitivity = 0.7; // Factor de sensibilidad para los joysticks (0.5-1.0)
let activeTouchId = null; // Para manejar multitouch

/* ================== INICIALIZACIÓN ================== */
document.addEventListener('DOMContentLoaded', function() {
  // Mostrar caso clínico al inicio
  document.getElementById('start-simulation-btn').addEventListener('click', function() {
    gsap.to('#clinical-case', {
      opacity: 0,
      duration: 0.5,
      onComplete: () => {
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
  initJoysticks();
  setupEventListeners();
  setupAlertDismissButtons();
  updateVitals();
  updateEndoLightEffect(50, 50);
  
  requestAnimationFrame(updateInstrumentPositions);
}

/* ================== SISTEMA DE ALERTAS ================== */
function setupAlertDismissButtons() {
  document.querySelectorAll('.alert-dismiss').forEach(btn => {
    btn.addEventListener('click', function() {
      const alert = this.parentElement;
      gsap.to(alert, {
        opacity: 0,
        y: -20,
        duration: 0.3,
        ease: "power1.out",
        onComplete: () => {
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
  gsap.fromTo(alert, 
    { opacity: 0, y: -20 }, 
    { opacity: 1, y: 0, duration: 0.3, ease: "power1.out" }
  );
  
  const timer = alert.querySelector('.alert-timer');
  if (timer) {
    timer.style.width = '100%';
    timer.style.transition = `width ${duration/1000}s linear`;
    setTimeout(() => {
      timer.style.width = '0%';
    }, 10);
  }
  
  if (duration > 0) {
    setTimeout(() => {
      gsap.to(alert, {
        opacity: 0,
        y: -20,
        duration: 0.3,
        ease: "power1.out",
        onComplete: () => {
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
  
  // Verificar colisión para el instrumento activo
  if (activeInstrument) {
    const instrumentRect = document.getElementById(activeInstrument).getBoundingClientRect();
    const instrumentCenterX = instrumentRect.left + instrumentRect.width/2;
    const instrumentCenterY = instrumentRect.top + instrumentRect.height/2;
    
    // Coordenadas relativas al mini mapa
    const relX = (instrumentCenterX - retinaRect.left) / retinaRect.width * miniMapRect.width;
    const relY = (instrumentCenterY - retinaRect.top) / retinaRect.height * miniMapRect.height;
    
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
  }
  // Verificar colisión para el endoiluminador
  const lightPosX = retinaRect.left + retinaRect.width * lightJoystickX / 100;
  const lightPosY = retinaRect.top + retinaRect.height * lightJoystickY / 100;
  
  // Coordenadas relativas al mini mapa
  const lightRelX = (lightPosX - retinaRect.left) / retinaRect.width * miniMapRect.width;
  const lightRelY = (lightPosY - retinaRect.top) / retinaRect.height * miniMapRect.height;
  
  // Verificar contacto con retina para el endoiluminador
  const retinaX = 400 * (miniMapRect.width / 800);
  const retinaY = 300 * (miniMapRect.height / 600);
  const retinaRadius = 250 * (miniMapRect.width / 800);
  
  const lightDistance = Math.sqrt(Math.pow(lightRelX - retinaX, 2) + Math.pow(lightRelY - retinaY, 2));
  
  if (lightDistance > retinaRadius * 1.1 && !wallCollisionActive) {
    showAlert('wall-collision-alert');
    return true;
  }
  
  return false;
}

function checkVesselDamage() {
  if (currentDepth > -200 || hemorrhageActive) return false;
  
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const instrumentRect = document.getElementById(activeInstrument).getBoundingClientRect();
  
  const instrumentCenterX = instrumentRect.left + instrumentRect.width/2;
  const instrumentCenterY = instrumentRect.top + instrumentRect.height/2;
  
  const bloodVessels = document.querySelector('.blood-vessels');
  const bloodVesselsRect = bloodVessels.getBoundingClientRect();
  
  const relX = instrumentCenterX - bloodVesselsRect.left;
  const relY = instrumentCenterY - bloodVesselsRect.top;
  
  const proximityThreshold = 30;
  const vesselProximity = Math.abs(relX - bloodVesselsRect.width/2) < proximityThreshold && 
                        Math.abs(relY - bloodVesselsRect.height/2) < proximityThreshold;
  
  if (vesselProximity && !hemorrhageActive) {
    hemorrhageActive = true;
    showAlert('vessel-damage-alert', 0);
    
    document.getElementById('hemorrhage-effect').style.display = 'block';
    gsap.to('#hemorrhage-effect', { opacity: 0.7, duration: 1 });
    
    createBloodClots(5, {x: instrumentCenterX, y: instrumentCenterY});
    
    perfusion -= 15;
    iop += 5;
    
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
    clot.style.width = `${10 + Math.random() * 20}px`;
    clot.style.height = clot.style.width;
    
    bloodClotsContainer.appendChild(clot);
    bloodClots.push({
      element: clot,
      x: x,
      y: y,
      size: parseFloat(clot.style.width)
    });
  }
}

function removeBloodClot(index) {
  if (index >= 0 && index < bloodClots.length) {
    const clot = bloodClots[index];
    if (clot.element.parentNode) {
      gsap.to(clot.element, {
        opacity: 0,
        scale: 0.5,
        duration: 0.5,
        onComplete: () => {
          if (clot.element.parentNode) {
            clot.element.parentNode.removeChild(clot.element);
          }
        }
      });
    }
    bloodClots.splice(index, 1);
    
    if (bloodClots.length === 0) {
      hemorrhageActive = false;
      gsap.to('#hemorrhage-effect', { 
        opacity: 0, 
        duration: 1, 
        onComplete: () => {
          document.getElementById('hemorrhage-effect').style.display = 'none';
        }
      });
    }
  }
}

function removeNearbyBloodClots(instrument, offsetX, offsetY) {
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const instrumentRect = instrument.getBoundingClientRect();
  
  const instrumentCenterX = retinaRect.left + retinaRect.width/2 + offsetX;
  const instrumentCenterY = retinaRect.top + retinaRect.height/2 + offsetY;
  
  bloodClots.forEach((clot, index) => {
    const dx = instrumentCenterX - (retinaRect.left + clot.x);
    const dy = instrumentCenterY - (retinaRect.top + clot.y);
    const distance = Math.sqrt(dx * dx + dy * dy);
    
    if (distance < clot.size + instrumentRect.width/2) {
      removeBloodClot(index);
      
      const particleCount = 5 + Math.floor(Math.random() * 5);
      for (let i = 0; i < particleCount; i++) {
        const particle = document.createElement('div');
        particle.className = 'vitreous-particle';
        particle.style.left = `${clot.x + (Math.random()*20 - 10)}px`;
        particle.style.top = `${clot.y + (Math.random()*20 - 10)}px`;
        particle.style.setProperty('--tx', Math.random()*40 - 20);
        particle.style.setProperty('--ty', Math.random()*40 - 20);
        document.getElementById('retina').appendChild(particle);
        
        setTimeout(() => particle.remove(), 1500);
      }
    }
  });
}

function checkRetinaDetachment() {
  if (iop > 28 && !retinaDetached) {
    showAlert('retina-detachment-alert');
    retinaDetached = true;
    simulateRetinaDetachment();
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

/* ================== SIMULACIÓN DE DESPRENDIMIENTO DE RETINA ================== */
function simulateRetinaDetachment() {
  const overlay = document.getElementById('detached-retina-overlay');
  overlay.style.position = 'absolute';
  overlay.style.width = '100%';
  overlay.style.height = '100%';
  overlay.style.borderRadius = '50%';
  overlay.style.background = 'radial-gradient(circle at 30% 20%, rgba(200,0,0,0.7) 0%, rgba(100,0,0,0.5) 60%, transparent 100%)';
  overlay.style.zIndex = '13';
  overlay.style.pointerEvents = 'none';
  
  // Crear bolsa de desprendimiento en la parte superior izquierda
  const edgeDetachment = document.getElementById('retina-edge-detachment');
  edgeDetachment.style.position = 'absolute';
  edgeDetachment.style.width = '60%';
  edgeDetachment.style.height = '60%';
  edgeDetachment.style.left = '10%';
  edgeDetachment.style.top = '10%';
  edgeDetachment.style.borderRadius = '50%';
  edgeDetachment.style.background = 'radial-gradient(circle, transparent 65%, rgba(180,0,0,0.5) 85%, rgba(150,0,0,0.7) 100%)';
  edgeDetachment.style.zIndex = '14';
  edgeDetachment.style.pointerEvents = 'none';
  edgeDetachment.style.boxShadow = 'inset 0 0 50px rgba(0,0,0,0.5)';
  edgeDetachment.style.transform = 'rotate(-15deg)';
  edgeDetachment.style.filter = 'url(#wrinkleFilter)';
  
  // Crear agujero retiniano pequeño en la parte superior
  const hole = document.getElementById('retinal-hole');
  hole.style.position = 'absolute';
  hole.style.width = '30px';
  hole.style.height = '30px';
  hole.style.left = '25%';
  hole.style.top = '25%';
  hole.style.borderRadius = '50%';
  hole.style.background = 'radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(200,0,0,0.6) 70%, transparent 100%)';
  hole.style.zIndex = '15';
  hole.style.pointerEvents = 'none';
  hole.style.boxShadow = '0 0 15px rgba(255,255,255,0.5)';
  hole.style.border = '2px dashed rgba(255,255,255,0.7)';
  hole.style.display = 'block';
  
  // Crear efecto de desprendimiento central
  const retina = document.querySelector('.retina-sphere');
  retina.style.transform = 'rotateX(15deg) translateZ(50px) scale(1.05)';
  
  // Crear burbujas subretinianas
  for (let i = 0; i < 15; i++) {
    setTimeout(() => {
      const bubble = document.createElement('div');
      bubble.className = 'subretinal-bubble';
      bubble.style.left = `${10 + Math.random() * 30}%`;
      bubble.style.top = `${10 + Math.random() * 30}%`;
      bubble.style.width = `${10 + Math.random() * 30}px`;
      bubble.style.height = bubble.style.width;
      overlay.appendChild(bubble);
      
      gsap.to(bubble, {
        y: `-=${10 + Math.random() * 20}`,
        x: `+=${Math.random() * 20 - 10}`,
        opacity: 0.7,
        duration: 3,
        onComplete: () => bubble.remove()
      });
    }, i * 300);
  }
}

function simulateRetinaDetachmentProcedure() {
  if (retinaDetached) return;
  
  procedureStep = 1;
  showAlert('retina-detachment-alert');
  
  // Simular aumento rápido de PIO
  iop = 30;
  showAlert('high-iop-alert');
  
  // Simular desprendimiento
  setTimeout(() => {
    simulateRetinaDetachment();
    retinaDetached = true;
    
    // Guiar al usuario a usar el vitrectomo
    setTimeout(() => {
      document.getElementById('btn-vitrectomo').classList.add('active');
      document.getElementById('vitrectome').style.display = 'block';
      activeInstrument = 'vitrectome';
    }, 1000);
  }, 1000);
}

function injectPFCForDetachment() {
  if (pfcInjected) return;
  
  pfcInjected = true;
  procedureStep = 3;
  const overlay = document.getElementById('detached-retina-overlay');
  const edgeDetachment = document.getElementById('retina-edge-detachment');
  
  // Eliminar la bolsa de desprendimiento
  gsap.to(edgeDetachment, {
    opacity: 0,
    duration: 1.5,
    onComplete: () => {
      edgeDetachment.style.display = 'none';
    }
  });
  
  // Cambiar color del overlay para simular PFC
  overlay.style.background = 'radial-gradient(circle at 30% 20%, rgba(100,255,255,0.3) 0%, rgba(50,200,200,0.2) 60%, transparent 100%)';
  
  // Crear burbujas de PFC
  for (let i = 0; i < 30; i++) {
    setTimeout(() => {
      const bubble = document.createElement('div');
      bubble.className = 'pfc-bubble';
      bubble.style.left = `${30 + Math.random() * 40}%`;
      bubble.style.top = `${70 + Math.random() * 20}%`;
      bubble.style.width = `${10 + Math.random() * 30}px`;
      bubble.style.height = bubble.style.width;
      document.getElementById('pfc-bubbles').appendChild(bubble);
      
      gsap.to(bubble, {
        y: `-=${50 + Math.random() * 50}`,
        x: `+=${Math.random() * 40 - 20}`,
        opacity: 0.8,
        duration: 2,
        onComplete: () => bubble.remove()
      });
    }, i * 100);
  }
  
  // Reposicionar retina
  setTimeout(() => {
    const retina = document.querySelector('.retina-sphere');
    retina.style.transform = 'rotateX(15deg) translateZ(50px) scale(1)';
    
    // Eliminar burbujas subretinianas
    const bubbles = overlay.querySelectorAll('.subretinal-bubble');
    bubbles.forEach(bubble => bubble.remove());
    
    // Guiar al usuario a usar el láser
    setTimeout(() => {
      document.getElementById('btn-cautery').classList.add('active');
      document.getElementById('cautery-probe').style.display = 'block';
      activeInstrument = 'cautery-probe';
      procedureStep = 4;
    }, 2000);
  }, 2000);
  
  pfcLevel = 100;
  document.getElementById('pfc-value').textContent = `${pfcLevel}%`;
  document.getElementById('pfc-level').style.width = `${pfcLevel}%`;
  
  iop += 10;
  perfusion += 5;
}

function injectGas() {
  if (gasLevel >= 100) return;
  
  const retina = document.getElementById('retina');
  const gasBubbles = document.getElementById('gas-bubbles');
  
  // Crear burbuja grande central
  if (gasLevel === 0) {
    const bigBubble = document.createElement('div');
    bigBubble.className = 'gas-bubble-large';
    bigBubble.style.left = '50%';
    bigBubble.style.top = '50%';
    bigBubble.style.width = '40%';
    bigBubble.style.height = '40%';
    bigBubble.style.zIndex = '5';
    gasBubbles.appendChild(bigBubble);
    
    gsap.to(bigBubble, {
      scale: 1.2,
      opacity: 0.9,
      duration: 3,
      onComplete: () => {
        if (bigBubble.parentNode) {
          bigBubble.parentNode.removeChild(bigBubble);
        }
      }
    });
  }
  
  // Crear múltiples burbujas de gas transparentes con bordes grises claros
  for (let i = 0; i < 25; i++) { // Más burbujas para cubrir el 85% de la retina
    setTimeout(() => {
      const bubble = document.createElement('div');
      bubble.className = 'gas-bubble';
      
      // Tamaños aleatorios (grandes y pequeñas)
      const size = 15 + Math.random() * 40; // Tamaños entre 15px y 55px
      
      bubble.style.left = `${30 + Math.random() * 40}%`; // Cubrir área central del 85%
      bubble.style.top = `${30 + Math.random() * 40}%`;
      bubble.style.width = `${size}px`;
      bubble.style.height = `${size}px`;
      
      // Estilo de burbuja transparente con borde gris claro
      bubble.style.background = 'rgba(255, 255, 255, 0.1)';
      bubble.style.border = '1px solid rgba(200, 200, 200, 0.6)';
      bubble.style.boxShadow = 
        '0 0 10px rgba(255, 255, 255, 0.5), ' +
        'inset 0 0 5px rgba(255, 255, 255, 0.3)';
      
      bubble.style.setProperty('--tx', Math.random() * 40 - 20);
      bubble.style.setProperty('--ty', -Math.random() * 60 - 30);
      gasBubbles.appendChild(bubble);
      
      // Animación de flotación
      gsap.to(bubble, {
        y: `-=${20 + Math.random() * 30}`,
        x: `+=${Math.random() * 20 - 10}`,
        scale: 1.2,
        opacity: 0.8,
        duration: 3,
        onComplete: () => bubble.remove()
      });
    }, i * 120); // Más rápido para crear más burbujas
  }
  
  gasLevel = Math.min(100, gasLevel + 20);
  document.getElementById('gas-value').textContent = `${gasLevel}%`;
  document.getElementById('gas-level').style.width = `${gasLevel}%`;
  
  iop += 5;
  perfusion -= 2;
  
  if (gasLevel >= 100) {
    showAlert('gas-injected-alert');
    retinaFixed = true;
    completeSurgery();
  }
}

function completeSurgery() {
  // Restaurar retina a su color original
  const retina = document.querySelector('.retina-sphere');
  retina.style.background = 'radial-gradient(circle at center, #500000 0%, #400000 20%, #300000 40%, #200000 70%, #100000 100%)';
  
  // Mostrar alerta de cirugía completada
  showAlert('surgery-complete-alert', 0);
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
    
    // Suavizar el movimiento con interpolación
    gsap.to(handle, {
      x: deltaX,
      y: deltaY,
      duration: 0.1,
      ease: "power1.out"
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
    
    // Suavizar el retorno a la posición central
    gsap.to(handle, {
      x: 0,
      y: 0,
      duration: 0.3,
      ease: "elastic.out(1, 0.5)"
    });
    
    // Resetear posición cuando se suelta
    if (type === 'light') {
      updateCallback(50, 50);
    } else {
      updateCallback(50, 50);
    }
  }
  
  // Eventos táctiles
  joystickElement.addEventListener('touchstart', handleStart, { passive: false });
  document.addEventListener('touchmove', (e) => {
    if (activeTouchId !== null) {
      const touches = Array.from(e.touches);
      if (touches.some(t => t.identifier === activeTouchId)) {
        handleMove(e);
      }
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
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const maxOffset = retinaRect.width / 2;
  const offsetX = (normX - 50) / 50 * maxOffset * 0.9;
  const offsetY = (normY - 50) / 50 * maxOffset * 0.9;
  
  if(activeInstrument) {
    const instr = document.getElementById(activeInstrument);
    if(instr) {
      instr.style.transform = `translate(calc(-50% + ${offsetX}px), calc(-50% + ${offsetY}px)) translateZ(${currentDepth}px)`;
      
      const shadow = instr.querySelector('.instrument-shadow');
      if(shadow) {
        const lightX = parseInt(document.documentElement.style.getPropertyValue('--light-x'));
        const lightY = parseInt(document.documentElement.style.getPropertyValue('--light-y'));
        
        const shadowOffsetX = (lightX - 50) / 10;
        const shadowOffsetY = (lightY - 50) / 10;
        const shadowBlur = 10 - Math.abs(lightX - normX) / 10;
        
        shadow.style.transform = `translate(${shadowOffsetX}px, ${shadowOffsetY}px) rotateX(75deg) translateZ(-10px)`;
        shadow.style.filter = `blur(${Math.max(3, shadowBlur)}px)`;
      }
      
      checkWallCollision();
      
      if (currentDepth <= -200) {
        checkVesselDamage();
      }
      
      updateDepthIndicator();
    }
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
  
  document.querySelectorAll('.instrument-light-reflection').forEach(reflection => {
    const instrument = reflection.closest('.instrument');
    if(instrument) {
      const instrRect = instrument.getBoundingClientRect();
      const instrCenterX = instrRect.left + instrRect.width/2;
      const instrCenterY = instrRect.top + instrRect.height/2;
      
      const retinaRect = document.getElementById('retina').getBoundingClientRect();
      const retinaCenterX = retinaRect.left + retinaRect.width/2;
      const retinaCenterY = retinaRect.top + retinaRect.height/2;
      
      const lightPosX = retinaRect.left + retinaRect.width * normX / 100;
      const lightPosY = retinaRect.top + retinaRect.height * normY / 100;
      
      const angle = Math.atan2(lightPosY - instrCenterY, lightPosX - instrCenterX);
      const distance = Math.sqrt(
        Math.pow(lightPosX - instrCenterX, 2) + 
        Math.pow(lightPosY - instrCenterY, 2)
      );
      
      const intensity = Math.min(1, 100 / distance);
      
      reflection.style.background = `linear-gradient(
        ${angle}rad,
        rgba(255,255,255,${0.2 * intensity}) 0%,
        rgba(255,255,255,${0.1 * intensity}) 50%,
        transparent 100%
      )`;
    }
  });
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

/* ================== GESTIÓN DE INSTRUMENTOS ================== */
function setupEventListeners() {
  document.getElementById('btn-vitrectomo').addEventListener('click', () => toggleInstrument('btn-vitrectomo', 'vitrectome'));
  document.getElementById('btn-laser').addEventListener('click', () => toggleInstrument('btn-laser', 'laser-probe'));
  document.getElementById('btn-pfc').addEventListener('click', () => toggleInstrument('btn-pfc', 'pfc-probe'));
  document.getElementById('btn-cautery').addEventListener('click', () => toggleInstrument('btn-cautery', 'cautery-probe'));
  document.getElementById('btn-gas').addEventListener('click', () => toggleInstrument('btn-gas', 'gas-probe'));
  document.getElementById('btn-simulate-detachment').addEventListener('click', simulateRetinaDetachmentProcedure);
  
  document.getElementById('btn-precionar').addEventListener('click', handleMainAction);
  document.getElementById('btn-precionar').addEventListener('touchstart', handleMainAction, { passive: false });
  
  document.getElementById('vitrectomo-z-slider').addEventListener('input', function() {
    currentDepth = parseInt(this.value);
    updateInstrumentPosition(vitrectomoJoystickX, vitrectomoJoystickY);
  });
  
  document.getElementById('endo-z-slider').addEventListener('input', function() {
    updateEndoLightEffect(lightJoystickX, lightJoystickY);
  });
}

function toggleInstrument(btnId, instrumentId) {
  const btn = document.getElementById(btnId);
  
  if(btn.classList.contains('active')) {
    btn.classList.remove('active');
    document.getElementById(instrumentId).style.display = 'none';
    activeInstrument = null;
  } else {
    document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.instrument').forEach(i => i.style.display = 'none');
    
    btn.classList.add('active');
    activeInstrument = instrumentId;
    document.getElementById(instrumentId).style.display = 'block';
  }
}

function handleMainAction() {
  if(!activeInstrument) return;
  
  const instrument = document.getElementById(activeInstrument);
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const instRect = instrument.getBoundingClientRect();
  
  // Mostrar indicador de acción
  const actionIndicator = instrument.querySelector('.instrument-action-indicator');
  if (actionIndicator) {
    actionIndicator.classList.add('active');
    setTimeout(() => actionIndicator.classList.remove('active'), 1000);
  }
  
  // Calcular posición de la punta del instrumento
  const tip = instrument.querySelector('.instrument-tip');
  const tipRect = tip.getBoundingClientRect();
  const x = tipRect.left + tipRect.width/2 - retinaRect.left;
  const y = tipRect.top + tipRect.height/2 - retinaRect.top;
  
  const syntheticEvent = { 
    clientX: retinaRect.left + x, 
    clientY: retinaRect.top + y,
    preventDefault: () => {}
  };
  
  switch(activeInstrument) {
    case 'laser-probe':
      if (procedureStep === 4) {
        laserFunction(syntheticEvent);
      }
      break;
    case 'vitrectome':
      if (procedureStep === 1 || procedureStep === 6) {
        vitrectomyFunction(syntheticEvent);
        if (vitreousRemoved >= 100 && procedureStep === 1) {
          procedureStep = 2;
          document.getElementById('btn-cautery').classList.add('active');
          document.getElementById('cautery-probe').style.display = 'block';
          activeInstrument = 'cautery-probe';
          showAlert('vitreous-removed-alert');
        }
        // Activar el gas cuando el PFC llegue al 50%
        if (pfcLevel <= 50 && procedureStep === 6) {
          procedureStep = 7;
          document.getElementById('btn-gas').classList.add('active');
          document.getElementById('gas-probe').style.display = 'block';
          activeInstrument = 'gas-probe';
          
          // Inyectar gas automáticamente
          setTimeout(() => {
            injectGas();
          }, 500);
        }
      }
      removeNearbyBloodClots(instrument, x, y);
      break;
    case 'pfc-probe':
      if (procedureStep === 3) {
        injectPFCForDetachment();
      } else {
        injectPFC();
      }
      break;
    case 'cautery-probe':
      if (procedureStep === 2 || procedureStep === 4) {
        cauteryFunction(syntheticEvent);
        // Crear marca permanente en la punta del instrumento
        const retina = document.getElementById('retina');
        const rect = retina.getBoundingClientRect();
        
        const burnMark = document.createElement('div');
        burnMark.className = 'cautery-mark-permanent';
        burnMark.style.left = (syntheticEvent.clientX - rect.left - 2) + 'px';
        burnMark.style.top = (syntheticEvent.clientY - rect.top - 2) + 'px';
        document.getElementById('permanent-marks').appendChild(burnMark);
        
        cauteryMarks.push({
          element: burnMark,
          x: syntheticEvent.clientX - rect.left - 2,
          y: syntheticEvent.clientY - rect.top - 2
        });
        
        // Verificar si se han colocado los 7 puntos alrededor del agujero
        if (procedureStep === 2) {
          const hole = document.getElementById('retinal-hole');
          const holeRect = hole.getBoundingClientRect();
          const holeCenterX = holeRect.left + holeRect.width/2;
          const holeCenterY = holeRect.top + holeRect.height/2;
          
          const distance = Math.sqrt(
            Math.pow(syntheticEvent.clientX - holeCenterX, 2) + 
            Math.pow(syntheticEvent.clientY - holeCenterY, 2)
          );
          
          if (distance < holeRect.width/2 + 30) {
            marksAroundHole++;
            
            // Crear indicador numérico del punto aplicado
            const markerNumber = document.createElement('div');
            markerNumber.className = 'laser-marker-number';
            markerNumber.textContent = marksAroundHole;
            markerNumber.style.left = (syntheticEvent.clientX - rect.left - 10) + 'px';
            markerNumber.style.top = (syntheticEvent.clientY - rect.top - 10) + 'px';
            document.getElementById('permanent-marks').appendChild(markerNumber);
            
            // Animación de confirmación
            gsap.to(markerNumber, {
              scale: 1.5,
              duration: 0.3,
              yoyo: true,
              repeat: 1
            });
            
            if (marksAroundHole >= requiredMarks) {
              holeLocated = true;
              showAlert('hole-located-alert');
              procedureStep = 3;
              
              // Cambiar a PFC después de 2 segundos
              setTimeout(() => {
                document.getElementById('btn-pfc').classList.add('active');
                document.getElementById('pfc-probe').style.display = 'block';
                activeInstrument = 'pfc-probe';
              }, 2000);
            }
          }
        }
      }
      break;
    case 'gas-probe':
      if (procedureStep === 7) {
        injectGas();
      }
      break;
  }
  
  if (currentDepth <= -200 && (activeInstrument === 'vitrectome' || activeInstrument === 'cautery-probe')) {
    checkVesselDamage();
  }
}

/* ================== FUNCIONES DE INSTRUMENTOS MEJORADAS ================== */
function laserFunction(e) {
  const retina = document.getElementById('retina');
  const retinaRect = retina.getBoundingClientRect();
  
  // Crear efecto de láser temporal
  const laserSpot = document.createElement('div');
  laserSpot.className = 'laser-spot';
  laserSpot.style.left = (e.clientX - retinaRect.left - 12) + 'px';
  laserSpot.style.top = (e.clientY - retinaRect.top - 12) + 'px';
  retina.appendChild(laserSpot);
  
  // Crear marca permanente de láser (punto blanco)
  const burnMark = document.createElement('div');
  burnMark.className = 'laser-burn-permanent';
  burnMark.style.left = (e.clientX - retinaRect.left - 3) + 'px';
  burnMark.style.top = (e.clientY - retinaRect.top - 3) + 'px';
  document.getElementById('permanent-marks').appendChild(burnMark);
  
  // Actualizar parámetros fisiológicos
  iop += 0.5;
  perfusion -= 0.2;
  
  // Solo verificar proximidad al agujero cuando se está en la fase de fijación retiniana (paso 4)
  if (procedureStep === 4) {
    const hole = document.getElementById('retinal-hole');
    const holeRect = hole.getBoundingClientRect();
    const holeCenterX = holeRect.left + holeRect.width/2;
    const holeCenterY = holeRect.top + holeRect.height/2;
    
    // Calcular distancia al centro del agujero
    const distance = Math.sqrt(
      Math.pow(e.clientX - holeCenterX, 2) + 
      Math.pow(e.clientY - holeCenterY, 2)
    );
    
    // Radio efectivo (radio del agujero + margen de 15px)
    const effectiveRadius = holeRect.width/2 + 15;
    
    // Mostrar guía visual para los puntos de láser
    if (distance < effectiveRadius + 50 && marksAroundHole < requiredMarks) {
      // Crear anillo guía alrededor del agujero
      if (!document.getElementById('laser-guide-ring')) {
        const guideRing = document.createElement('div');
        guideRing.id = 'laser-guide-ring';
        guideRing.style.position = 'absolute';
        guideRing.style.width = `${effectiveRadius * 2}px`;
        guideRing.style.height = `${effectiveRadius * 2}px`;
        guideRing.style.left = `${holeCenterX - retinaRect.left - effectiveRadius}px`;
        guideRing.style.top = `${holeCenterY - retinaRect.top - effectiveRadius}px`;
        guideRing.style.border = '2px dashed rgba(255, 255, 255, 0.5)';
        guideRing.style.borderRadius = '50%';
        guideRing.style.pointerEvents = 'none';
        guideRing.style.zIndex = '16';
        guideRing.style.boxShadow = '0 0 10px rgba(255, 255, 255, 0.3)';
        retina.appendChild(guideRing);
      }
      
      // Resaltar el agujero cuando se acerca el instrumento
      gsap.to(hole, {
        boxShadow: '0 0 20px rgba(255,255,255,0.8)',
        duration: 0.3
      });
    }
    
    if (distance < effectiveRadius) {
      marksAroundHole++;
      
      // Crear indicador numérico del punto aplicado
      const markerNumber = document.createElement('div');
      markerNumber.className = 'laser-marker-number';
      markerNumber.textContent = marksAroundHole;
      markerNumber.style.left = (e.clientX - retinaRect.left - 10) + 'px';
      markerNumber.style.top = (e.clientY - retinaRect.top - 10) + 'px';
      document.getElementById('permanent-marks').appendChild(markerNumber);
      
      // Animación de confirmación
      gsap.to(markerNumber, {
        scale: 1.5,
        duration: 0.3,
        yoyo: true,
        repeat: 1
      });
      
      // Si se alcanza el número requerido de puntos (7)
      if (marksAroundHole >= requiredMarks) {
        // Eliminar el anillo guía
        const guideRing = document.getElementById('laser-guide-ring');
        if (guideRing) guideRing.remove();
        
        // Mostrar mensaje de éxito
        showAlert('retina-fixed-alert');
        procedureStep = 5;
        
        // Cambiar a vitrectomo después de 2 segundos
        setTimeout(() => {
          procedureStep = 6;
          document.getElementById('btn-vitrectomo').classList.add('active');
          document.getElementById('vitrectome').style.display = 'block';
          activeInstrument = 'vitrectome';
          
          // Restaurar apariencia normal del agujero
          gsap.to(hole, {
            boxShadow: '0 0 15px rgba(255,255,255,0.5)',
            duration: 0.5
          });
        }, 2000);
      }
    }
  }
  
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
  
  // Efecto visual de cauterio temporal
  const cauteryEffect = document.createElement('div');
  cauteryEffect.className = 'cautery-effect';
  cauteryEffect.style.left = (e.clientX - retinaRect.left - 15) + 'px';
  cauteryEffect.style.top = (e.clientY - retinaRect.top - 15) + 'px';
  retina.appendChild(cauteryEffect);
  
  // Verificar si está en la mácula
  const macula = document.querySelector('.macula');
  const maculaRect = macula.getBoundingClientRect();
  const maculaDistance = Math.sqrt(
    Math.pow(e.clientX - (maculaRect.left + maculaRect.width/2), 2) + 
    Math.pow(e.clientY - (maculaRect.top + maculaRect.height/2), 2)
  );
  
  if (maculaDistance < maculaRect.width/2) {
    showAlert('vision-loss-alert');
  }
  
  // Actualizar parámetros fisiológicos
  perfusion += 1;
  iop = Math.min(30, iop + 0.3);
  
  // Eliminar efecto de cauterio después de 1 segundo
  setTimeout(() => {
    if (cauteryEffect.parentNode) {
      cauteryEffect.parentNode.removeChild(cauteryEffect);
    }
  }, 1000);
}

function vitrectomyFunction(e) {
  const retina = document.getElementById('retina');
  const rect = retina.getBoundingClientRect();
  
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
  
  vitreousRemoved = Math.min(100, vitreousRemoved + 0.8);
  iop = Math.max(20, iop - 0.3); // No permitir que la PIO baje de 20
  
  // Remoción de PFC
  if (procedureStep === 6) {
    pfcLevel = Math.max(0, pfcLevel - 2);
    document.getElementById('pfc-value').textContent = `${pfcLevel}%`;
    document.getElementById('pfc-level').style.width = `${pfcLevel}%`;
    
    // Eliminar el efecto de PFC cuando se remueve completamente
    if (pfcLevel <= 0) {
      document.getElementById('detached-retina-overlay').style.background = 'transparent';
      
      // Completar la cirugía cuando se elimina todo el PFC
      completeSurgery();
    }
  }
}

function injectPFC() {
  if (pfcLevel >= 100) return;
  
  const retina = document.getElementById('retina');
  const pfcBubbles = document.getElementById('pfc-bubbles');
  
  for (let i = 0; i < 5; i++) {
    setTimeout(() => {
      const bubble = document.createElement('div');
      bubble.className = 'pfc-bubble';
      bubble.style.left = `${50 + Math.random() * 20}%`;
      bubble.style.top = `${50 + Math.random() * 20}%`;
      bubble.style.width = `${10 + Math.random() * 20}px`;
      bubble.style.height = bubble.style.width;
      pfcBubbles.appendChild(bubble);
      
      gsap.to(bubble, {
        y: `-=${30 + Math.random() * 50}`,
        x: `+=${Math.random() * 40 - 20}`,
        opacity: 0.8,
        duration: 2,
        onComplete: () => bubble.remove()
      });
    }, i * 200);
  }
  
  pfcLevel = Math.min(100, pfcLevel + 10);
  document.getElementById('pfc-value').textContent = `${pfcLevel}%`;
  document.getElementById('pfc-level').style.width = `${pfcLevel}%`;
  
  iop = Math.min(30, iop + 3);
  perfusion = Math.min(100, perfusion + 2);
  
  retina.style.background = `
    radial-gradient(circle at 35% 45%, rgba(100,255,100,0.1) 0%, rgba(50,200,50,0.2) 70%, transparent 100%),
    ${retina.style.background}`;
}

/* ================== ACTUALIZACIÓN DE PARÁMETROS ================== */
function updateVitals() {
  // Variación natural de los parámetros
  iop += (Math.random() - 0.5) * 0.1;
  perfusion += (Math.random() - 0.5) * 0.2;
  
  // Efecto de la vitrectomía en la PIO
  if (activeInstrument === 'vitrectome') {
    iop = Math.max(20, iop - 0.05); // No permitir que la PIO baje de 20
    vitreousRemoved = Math.min(100, vitreousRemoved + 0.1);
    
    // Estabilizar la PIO cuando se completa la vitrectomía
    if (vitreousRemoved >= 100) {
      iop = Math.max(20, iop - 0.1);
    }
  }
  
  if (pfcLevel > 0) {
    iop = Math.max(iop, 12 + pfcLevel / 10);
    perfusion = Math.min(perfusion + pfcLevel / 50, 100);
  }
  
  if (gasLevel > 0) {
    iop = Math.max(iop, 15 + gasLevel / 20);
    perfusion = Math.max(perfusion - gasLevel / 100, 60);
  }
  
  checkRetinaDetachment();
  checkHighIOP();
  
  // Limitar valores dentro de rangos razonables
  iop = Math.max(20, Math.min(30, iop)); // Mínimo de 20 mmHg
  perfusion = Math.max(60, Math.min(100, perfusion));
  vitreousRemoved = Math.max(0, Math.min(100, vitreousRemoved));
  
  // Actualizar valores en la interfaz
  document.getElementById('iop-value').innerText = iop.toFixed(1) + " mmHg";
  document.getElementById('iop-level').style.width = ((iop - 10) / 20 * 100) + "%";
  
  document.getElementById('perfusion-value').innerText = perfusion.toFixed(0) + "%";
  document.getElementById('perfusion-level').style.width = perfusion + "%";
  
  document.getElementById('vitreous-value').innerText = vitreousRemoved.toFixed(0) + "%";
  document.getElementById('vitreous-level').style.width = vitreousRemoved + "%";
  
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
