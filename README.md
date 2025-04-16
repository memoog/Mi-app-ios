<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Simulador Quirúrgico Retiniano Avanzado</title>
  <link rel="stylesheet" href="si.css">
  <link rel="stylesheet" href="alerts.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
  <style>
    /* Estilos adicionales para los instrumentos 3D */
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

.alert-container#low-depth-alert {
    border-left-color: #3b82f6;
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

/* Efecto de emergencia para daño vascular */
.vessel-damage-emergency {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(200, 0, 0, 0.2);
    z-index: 9999;
    pointer-events: none;
    animation: pulseEmergency 1.5s infinite alternate;
}

@keyframes pulseEmergency {
    0% { opacity: 0.2; }
    100% { opacity: 0.4; }
}

/* Efecto de presión intraocular alta en nervio óptico */
.high-iop-effect {
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, rgba(255,255,255,0.8), transparent 70%);
    border-radius: 50%;
    z-index: 11;
    pointer-events: none;
    mix-blend-mode: overlay;
    animation: iopPulse 2s infinite;
}

@keyframes iopPulse {
    0% { opacity: 0.3; transform: scale(1); }
    50% { opacity: 0.7; transform: scale(1.05); }
    100% { opacity: 0.3; transform: scale(1); }
}
/* ================== ESTILOS BASE MEJORADOS ================== */
* {
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
}

body {
    margin: 0;
    padding: 0;
    background: #0a0a12;
    overflow: hidden;
    font-family: 'Roboto', 'Segoe UI', system-ui, sans-serif;
    color: #e0e0e0;
    touch-action: manipulation;
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

.retina-deformation {
    position: absolute;
    width: 10px;
    height: 10px;
    background: radial-gradient(circle, rgba(255,100,100,0.8), transparent 70%);
    border-radius: 50%;
    pointer-events: none;
    z-index: 11;
    transform: translate(-50%, -50%);
    will-change: transform, opacity;
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

/* Pinzas de Endograsper 3D mejoradas */
#end-grasper {
    width: 2mm;
    height: 13mm;
    transform-style: preserve-3d;
}

#end-grasper .instrument-body {
    background: linear-gradient(to bottom, 
      #aaa 0%, 
      #ddd 10%, 
      #fff 20%, 
      #fff 40%, 
      #ddd 60%, 
      #aaa 80%,
      #888 100%);
    border: 1px solid #777;
    border-radius: 1.2mm;
    box-shadow: 
      0 0 1.2mm rgba(0,0,0,0.7),
      0 0 2.5mm rgba(255,255,255,0.15),
      inset 0 0 5px rgba(255,255,255,0.3);
    transform: rotateX(10deg);
}

#end-grasper::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%) translateZ(1px);
    width: 2.5mm;
    height: 1mm;
    background: linear-gradient(to right, #aaa, #ddd, #aaa);
    border-radius: 0.5mm;
    box-shadow: 0 1px 2px rgba(0,0,0,0.5);
    z-index: 2;
}

#end-grasper::after {
    content: '';
    position: absolute;
    bottom: -1.2mm;
    left: 50%;
    transform: translateX(-50%) translateZ(-1px);
    width: 1.4mm;
    height: 1mm;
    background: #fff;
    border-radius: 0.5mm;
    box-shadow: 
      0 0 0.8mm rgba(0,0,0,0.7),
      0 0 1.2mm rgba(255,255,255,0.4),
      inset 0 0 3px rgba(255,255,255,0.6);
    z-index: 1;
}

#end-grasper .instrument-jaws {
    position: absolute;
    bottom: -1.2mm;
    left: 50%;
    transform: translateX(-50%) rotateX(0deg);
    width: 1.8mm;
    height: 2mm;
    transform-style: preserve-3d;
    transition: transform 0.2s ease;
}

#end-grasper.active .instrument-jaws {
    transform: translateX(-50%) rotateX(-30deg);
}

#end-grasper.grasping .instrument-jaws {
    transform: translateX(-50%) rotateX(-60deg);
}

#end-grasper .instrument-jaw {
    position: absolute;
    width: 0.8mm;
    height: 2mm;
    background: linear-gradient(to bottom, #ddd, #aaa);
    border-radius: 0.3mm;
    box-shadow: 
      inset 0 -1px 2px rgba(0,0,0,0.5),
      0 0 3px rgba(255,255,255,0.3);
}

#end-grasper .instrument-jaw.left {
    left: 0;
    transform-origin: right center;
}

#end-grasper .instrument-jaw.right {
    right: 0;
    transform-origin: left center;
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

/* Tijeras Vitreorretinianas */
#scissors {
    width: 1mm;
    height: 14mm;
    transform-style: preserve-3d;
}

#scissors .instrument-body {
    background: linear-gradient(to bottom, 
      #aaa 0%, 
      #ddd 30%, 
      #fff 50%, 
      #ddd 70%, 
      #aaa 100%);
    border: 1px solid #777;
    border-radius: 0.5mm;
    box-shadow: 
      0 0 1.5mm rgba(0,0,0,0.7),
      0 0 3mm rgba(255,255,255,0.15),
      inset 0 0 5px rgba(255,255,255,0.3);
    transform: rotateX(10deg);
}

#scissors::before {
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

#scissors .instrument-jaws {
    position: absolute;
    bottom: -1mm;
    left: 50%;
    transform: translateX(-50%) rotateX(0deg);
    width: 1.5mm;
    height: 3mm;
    transform-style: preserve-3d;
    transition: transform 0.1s ease;
}

#scissors.active .instrument-jaws {
    transform: translateX(-50%) rotateX(-20deg);
}

#scissors.cutting .instrument-jaws {
    transform: translateX(-50%) rotateX(-60deg);
}

#scissors .instrument-blade {
    position: absolute;
    width: 0.6mm;
    height: 3mm;
    background: linear-gradient(to bottom, #eee, #aaa);
    border-radius: 0.3mm;
    box-shadow: 
      inset 0 -1px 2px rgba(0,0,0,0.5),
      0 0 3px rgba(255,255,255,0.3);
}

#scissors .instrument-blade.left {
    left: 0;
    transform-origin: right center;
}

#scissors .instrument-blade.right {
    right: 0;
    transform-origin: left center;
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

/* ================== SISTEMA DE PEELING MEJORADO ================== */
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
    box-shadow: 
      0 0 80px rgba(0, 100, 255, 0.4),
      inset 0 0 120px rgba(0, 50, 150, 0.3);
    cursor: crosshair;
    z-index: 21;
    background: rgba(0,0,0,0.4);
    will-change: transform;
}

#peeling-hud {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0, 0, 0, 0.85);
    color: white;
    padding: 20px;
    border-radius: 12px;
    border-top: 4px solid #0099ff;
    max-width: 600px;
    text-align: center;
    z-index: 22;
    backdrop-filter: blur(8px);
    box-shadow: 
      0 8px 25px rgba(0,0,0,0.6),
      inset 0 0 15px rgba(255,255,255,0.15);
    font-size: 0.95rem;
}

#traction-meter {
    margin-top: 15px;
    height: 12px;
    background: #222;
    border-radius: 6px;
    overflow: hidden;
    box-shadow: inset 0 0 8px rgba(0,0,0,0.7);
}

#traction-level {
    height: 100%;
    width: 0%;
    background: linear-gradient(90deg, #00ff00, #ffcc00, #ff0000);
    transition: width 0.1s;
}

#peeling-progress {
    margin-top: 15px;
    height: 8px;
    background: #222;
    border-radius: 4px;
    overflow: hidden;
    position: relative;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.7);
}

#peeling-progress-bar {
    height: 100%;
    width: 0%;
    background: linear-gradient(90deg, #0099ff, #00ccff);
    transition: width 0.3s;
}

#peeling-progress-text {
    position: absolute;
    top: 10px;
    left: 0;
    width: 100%;
    text-align: center;
    font-size: 0.8rem;
    color: #00ccff;
}

.complication {
    position: absolute;
    background: rgba(200, 0, 0, 0.8);
    border-radius: 50%;
    pointer-events: none;
    transform: translate(-50%, -50%);
    z-index: 23;
    filter: blur(1.5px);
    will-change: transform, opacity;
}

#peeling-instrument-selector {
    position: absolute;
    top: 20px;
    right: 20px;
    background: rgba(0, 0, 0, 0.85);
    padding: 15px;
    border-radius: 12px;
    color: white;
    z-index: 24;
    backdrop-filter: blur(8px);
    box-shadow: 
      0 8px 25px rgba(0,0,0,0.6),
      inset 0 0 15px rgba(255,255,255,0.15);
}

.peeling-instrument-btn {
    background: #333;
    border: none;
    color: white;
    padding: 10px 15px;
    margin: 8px 5px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    font-size: 0.85rem;
    min-width: 100px;
}

.peeling-instrument-btn.active {
    background: #0099ff;
    box-shadow: 
      0 0 15px rgba(0, 153, 255, 0.8),
      0 0 30px rgba(0, 100, 255, 0.4);
}

.membrane-flap {
    position: absolute;
    background: rgba(150, 200, 255, 0.35);
    border: 1px solid rgba(180, 220, 255, 0.7);
    border-radius: 50%;
    transform-origin: center;
    pointer-events: none;
    z-index: 25;
    backdrop-filter: blur(3px);
    box-shadow: 
      0 0 30px rgba(100, 150, 255, 0.4),
      inset 0 0 15px rgba(255, 255, 255, 0.3);
    will-change: transform, opacity;
}

.cut-effect {
    position: absolute;
    width: 30px;
    height: 30px;
    background: rgba(255, 255, 255, 0.7);
    border-radius: 50%;
    pointer-events: none;
    z-index: 26;
    animation: cutFade 1s ease-out forwards;
    filter: blur(2px);
}

@keyframes cutFade {
    0% { transform: scale(0.5); opacity: 1; }
    100% { transform: scale(1.5); opacity: 0; }
}

.cut-mark {
    position: absolute;
    width: 10px;
    height: 10px;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 50%;
    pointer-events: none;
    z-index: 26;
    animation: markFade 1s ease-out forwards;
}

@keyframes markFade {
    0% { transform: scale(1); opacity: 1; }
    100% { transform: scale(1.5); opacity: 0; }
}

/* ================== VISUALIZACIÓN OCT ================== */
#oct-container {
    position: absolute;
    top: 40px;
    left: 30px;
    width: 150px;
    height: 200px;
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

#oct-container svg {
    width: 100%;
    height: 100%;
    filter: drop-shadow(0 0 8px rgba(0,0,0,0.7));
}

#oct-container path {
    transition: all 0.3s ease;
}

.oct-scan {
    animation: octScan 6s linear infinite;
}

@keyframes octScan {
    0% { transform: translateY(0); }
    100% { transform: translateY(-100%); }
}

.peel-remove {
    animation: peelRemove 1s ease-out forwards;
}

@keyframes peelRemove {
    0% { opacity: 1; transform: translateY(0); }
    100% { opacity: 0; transform: translateY(20px); }
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

.toggle-btn .tooltip {
    visibility: hidden;
    width: 140px;
    background-color: #222;
    color: #fff;
    text-align: center;
    border-radius: 8px;
    padding: 8px;
    position: absolute;
    z-index: 1;
    right: 125%;
    top: 50%;
    transform: translateY(-50%);
    opacity: 0;
    transition: opacity 0.3s;
    font-size: 0.85rem;
    font-weight: normal;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    border: 1px solid #444;
}

.toggle-btn:hover .tooltip {
    visibility: visible;
    opacity: 1;
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

.laser-burn {
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

/* ================== RESPONSIVE ADJUSTMENTS ================== */
@media (max-width: 768px) {
    .retina-container {
      width: 90vmin;
      height: 90vmin;
    }
    
    .instrument-panel {
      top: 15px;
      left: 15px;
      transform: none;
      flex-direction: row;
      padding: 10px;
    }
    
    .toggle-btn {
      padding: 10px 15px;
      min-width: 90px;
      margin: 5px;
      font-size: 0.8rem;
    }
    
    .control-panel {
      width: 160px;
      padding: 15px;
      font-size: 0.75rem;
    }
    
    .joystick {
      width: 90px;
      height: 90px;
    }
    
    #miniMapContainer, #oct-container {
      width: 140px;
      height: 110px;
    }
    
    #joystick-light-container {
      left: 150px;
      bottom: 20px;
    }
    
    #joystick-vitrectomo-container {
      right: 150px;
      bottom: 20px;
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
      flex-wrap: wrap;
      justify-content: center;
    }
    
    .toggle-btn {
      padding: 8px 12px;
      min-width: 80px;
      font-size: 0.75rem;
      margin: 4px;
    }
    
    .control-panel {
      width: 150px;
      padding: 12px;
      right: 10px;
    }
    
    .joystick {
      width: 80px;
      height: 80px;
    }
    
    .joystick-container {
      bottom: 15px;
    }
    
    #miniMapContainer, #oct-container {
      width: 120px;
      height: 95px;
    }

    .depth-indicator {
      bottom: 110px;
      font-size: 0.75rem;
    }
    
    #joystick-light-container {
      left: 120px;
    }
    
    #joystick-vitrectomo-container {
      right: 120px;
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

.alert-container#wall-collision-alert {
    border-left-color: #ff9933;
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
}

.alert-container#high-iop-alert {
    border-left-color: #ffcc00;
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
}

.alert-content p {
    margin: 0;
    color: #ddd;
    font-size: 0.9rem;
    line-height: 1.4;
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
}

.alert-dismiss:hover {
    opacity: 1;
}

/* Efecto de emergencia para daño vascular */
.vessel-damage-emergency {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(200, 0, 0, 0.2);
    z-index: 9999;
    pointer-events: none;
    animation: pulseEmergency 1.5s infinite alternate;
}

@keyframes pulseEmergency {
    0% { opacity: 0.2; }
    100% { opacity: 0.4; }
}

/* Efecto de presión intraocular alta en nervio óptico */
.high-iop-effect {
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, rgba(255,255,255,0.8), transparent 70%);
    border-radius: 50%;
    z-index: 11;
    pointer-events: none;
    mix-blend-mode: overlay;
    animation: iopPulse 2s infinite;
}

@keyframes iopPulse {
    0% { opacity: 0.3; transform: scale(1); }
    50% { opacity: 0.7; transform: scale(1.05); }
    100% { opacity: 0.3; transform: scale(1); }
}
/* Estilo para las marcas permanentes de láser */
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

@keyframes burnPulse {
    0% { opacity: 0.8; transform: scale(1); }
    50% { opacity: 1; transform: scale(1.1); }
    100% { opacity: 0.8; transform: scale(1); }
}

/* Efecto de succión del vitrectomo */
.vitreous-suction {
    position: absolute;
    width: 40px;
    height: 40px;
    background: radial-gradient(circle, 
      rgba(255,255,255,0.8) 0%, 
      rgba(200,200,255,0.6) 50%, 
      transparent 70%);
    border-radius: 50%;
    pointer-events: none;
    z-index: 17;
    animation: suctionEffect 0.5s ease-out forwards;
    filter: blur(2px);
}

@keyframes suctionEffect {
    0% { transform: scale(1); opacity: 1; }
    100% { transform: scale(0.5); opacity: 0; }
}  </style>
</head>
<body>
  <div id="container">
    <!-- SISTEMA DE ALERTAS PROFESIONAL -->
    <div id="alert-system">
      <div class="alert-container" id="wall-collision-alert">
        <div class="alert-icon">⚠️</div>
        <div class="alert-content">
          <h3>COLISIÓN CON ESTRUCTURA OCULAR</h3>
          <p>¡Advertencia! El instrumento ha contactado con la pared ocular. Retire inmediatamente y reevalúe la posición.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="vessel-damage-alert">
        <div class="alert-icon">🩸</div>
        <div class="alert-content">
          <h3>HEMORRAGIA RETINIANA</h3>
          <p>¡Daño vascular detectado! Aplicar presión suave con PFC y considerar coagulación con láser.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="retina-detachment-alert">
        <div class="alert-icon">❗</div>
        <div class="alert-content">
          <h3>DESPRENDIMIENTO DE RETINA</h3>
          <p>¡Emergencia! Se ha detectado desprendimiento retiniano. Suspender procedimiento e inyectar PFC inmediatamente.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
      </div>
      
      <div class="alert-container" id="lens-contact-alert">
        <div class="alert-icon">🔍</div>
        <div class="alert-content">
          <h3>CONTACTO CON CRISTALINO</h3>
          <p>¡Precaución! El instrumento ha contactado con el cristalino. Riesgo de catarata traumática. Retire con cuidado.</p>
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
      
      <div class="alert-container" id="low-depth-alert">
        <div class="alert-icon">⬇️</div>
        <div class="alert-content">
          <h3>PROFUNDIDAD INSUFICIENTE</h3>
          <p>El instrumento está demasiado superficial. Aumente la profundidad para evitar daños en estructuras anteriores.</p>
          <div class="alert-timer"></div>
        </div>
        <button class="alert-dismiss">×</button>
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
        </defs>
        <rect width="800" height="600" fill="url(#bgGradient)" />
        <ellipse id="lens-minimap" cx="400" cy="150" rx="100" ry="65" fill="url(#lensGradient)" filter="url(#dropShadow)" />
        <ellipse cx="400" cy="150" rx="95" ry="60" fill="none" stroke="#B3E5FC" stroke-width="2" />
        <circle id="iris-minimap" cx="400" cy="150" r="55" fill="none" stroke="url(#irisGradient)" stroke-width="15" />
        <circle cx="400" cy="150" r="30" fill="#000000" />
        <circle id="retina-wall" cx="400" cy="300" r="250" fill="none" stroke="#E0E0E0" stroke-width="4" />
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
      <button class="toggle-btn" id="btn-scissors">Tijeras</button>
      <button class="toggle-btn" id="btn-pfc">PFC</button>
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

      <div class="vital-sign">
        <span class="vital-label">Nivel de PFC:</span>
        <span class="vital-value" id="pfc-value">0%</span>
      </div>
    </div>

    <!-- JOYSTICKS MEJORADOS -->
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
        <div id="light-scatter" class="light-scatter"></div>
        <div id="specular-highlight" class="specular-highlight"></div>
        
        <!-- Vitrectomo 3D mejorado -->
        <div id="vitrectome" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
            <div class="instrument-side"></div>
          </div>
          <div class="instrument-shadow"></div>
        </div>
        
        <!-- Pinzas de Endograsper 3D mejoradas -->
        <div id="end-grasper" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-jaws">
              <div class="instrument-jaw left"></div>
              <div class="instrument-jaw right"></div>
            </div>
          </div>
          <div class="instrument-shadow"></div>
        </div>
        
        <!-- Sonda de Láser 3D mejorada -->
        <div id="laser-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
        </div>

        <!-- Tijeras Vitreorretinianas -->
        <div id="scissors" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-jaws">
              <div class="instrument-blade left"></div>
              <div class="instrument-blade right"></div>
            </div>
          </div>
          <div class="instrument-shadow"></div>
        </div>

        <!-- Sonda de PFC -->
        <div id="pfc-probe" class="instrument">
          <div class="instrument-body">
            <div class="instrument-light-reflection"></div>
            <div class="instrument-tip"></div>
          </div>
          <div class="instrument-shadow"></div>
        </div>

        <!-- Burbujas de PFC -->
        <div id="pfc-bubbles"></div>
        
        <!-- Coágulos de sangre -->
        <div id="blood-clots"></div>
      </div>
    </div>

    <!-- SISTEMA DE PEELING -->
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
        <div id="peeling-progress">
          <div id="peeling-progress-bar"></div>
          <span id="peeling-progress-text">0% completado</span>
        </div>
      </div>
    </div>

    <!-- VISUALIZACIÓN OCT -->
    <div id="oct-container">
      <svg viewBox="0 0 300 400" preserveAspectRatio="xMidYMid meet">
        <defs>
          <linearGradient id="octGradient" x1="0%" y1="0%" x2="0%" y2="100%">
            <stop offset="0%" stop-color="#000000" stop-opacity="0" />
            <stop offset="100%" stop-color="#000000" stop-opacity="0.8" />
          </linearGradient>
        </defs>
        <rect width="300" height="400" fill="url(#octGradient)" />
        <g id="oct-scan-group">
          <path id="oct-retina" d="M0,200 Q75,180 150,200 T300,200" stroke="#00aaff" stroke-width="2" fill="none" />
          <path id="oct-membrane" d="M0,190 Q75,170 150,190 T300,190" stroke="#ff9900" stroke-width="1.5" fill="none" />
        </g>
        <g id="mli-group">
          <path d="M0,250 L300,250" stroke="#ff0000" stroke-width="1" stroke-dasharray="5,3" />
          <text x="10" y="265" fill="#ff0000" font-size="12">MLI</text>
        </g>
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
let iop = 16; // mmHg
let perfusion = 95; // %
let vitreousRemoved = 0; // %
let flowRate = 25; // ml/min
let retinaStatus = 'Estable';
let pfcLevel = 0; // %
let instrumentVelocity = { x: 0, y: 0 };
let lastInstrumentPosition = { x: 0, y: 0 };
let currentTime = Date.now();
let lastUpdateTime = currentTime;
let alertTimeout = null;
let vesselDamageActive = false;
let hemorrhageActive = false;
let bloodClots = [];
let laserBurns = []; // Array para almacenar las marcas permanentes del láser

/* ================== INICIALIZACIÓN ================== */
document.addEventListener('DOMContentLoaded', function() {
  initJoysticks();
  setupEventListeners();
  setupAlertDismissButtons();
  updateVitals();
  updateEndoLightEffect(50, 50);
  
  // Actualizar posición de instrumentos
  requestAnimationFrame(updateInstrumentPositions);
});

/* ================== SISTEMA DE ALERTAS MEJORADO ================== */
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
        }
      });
    });
  });
}

function showAlert(alertId, duration = 5000) {
  const alert = document.getElementById(alertId);
  if (!alert) return;
  
  // Cancelar timeout anterior si existe
  if (alertTimeout) {
    clearTimeout(alertTimeout);
  }
  
  // Mostrar alerta con animación
  alert.style.display = 'flex';
  gsap.fromTo(alert, 
    { opacity: 0, y: -20 }, 
    { opacity: 1, y: 0, duration: 0.3, ease: "power1.out" }
  );
  
  // Configurar temporizador visual
  const timer = alert.querySelector('.alert-timer');
  if (timer) {
    timer.style.width = '100%';
    timer.style.transition = `width ${duration/1000}s linear`;
    setTimeout(() => {
      timer.style.width = '0%';
    }, 10);
  }
  
  // Configurar para ocultar después de la duración
  if (duration > 0) {
    alertTimeout = setTimeout(() => {
      gsap.to(alert, {
        opacity: 0,
        y: -20,
        duration: 0.3,
        ease: "power1.out",
        onComplete: () => {
          alert.style.display = 'none';
          alert.style.opacity = '1';
          alert.style.transform = 'translateY(0)';
        }
      });
    }, duration);
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
    
    if (distance > retinaRadius * 0.9) {
      showAlert('wall-collision-alert');
      return true;
    }
    
    // Verificar contacto con iris (cristalino) en mini mapa
    const iris = document.getElementById('iris-minimap');
    const irisX = 400 * (miniMapRect.width / 800);
    const irisY = 150 * (miniMapRect.height / 600);
    const irisDistance = Math.sqrt(Math.pow(relX - irisX, 2) + Math.pow(relY - irisY, 2));
    
    if (irisDistance < 30 && currentDepth > -100) {
      showAlert('lens-contact-alert');
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
  
  if (lightDistance > retinaRadius * 0.9) {
    showAlert('wall-collision-alert');
    return true;
  }
  
  // Verificar contacto con iris (cristalino) para el endoiluminador
  const iris = document.getElementById('iris-minimap');
  const irisX = 400 * (miniMapRect.width / 800);
  const irisY = 150 * (miniMapRect.height / 600);
  const lightIrisDistance = Math.sqrt(Math.pow(lightRelX - irisX, 2) + Math.pow(lightRelY - irisY, 2));
  
  if (lightIrisDistance < 30) {
    showAlert('lens-contact-alert');
    return true;
  }
  
  return false;
}

function checkVesselDamage() {
  // Solo activar hemorragia si está en profundidad máxima (valor más negativo)
  if (currentDepth > -200 || hemorrhageActive) return false;
  
  const retinaRect = document.getElementById('retina').getBoundingClientRect();
  const instrumentRect = document.getElementById(activeInstrument).getBoundingClientRect();
  
  const instrumentCenterX = instrumentRect.left + instrumentRect.width/2;
  const instrumentCenterY = instrumentRect.top + instrumentRect.height/2;
  
  // Verificar colisión con vasos sanguíneos (simplificado)
  const bloodVessels = document.querySelector('.blood-vessels');
  const bloodVesselsRect = bloodVessels.getBoundingClientRect();
  
  // Coordenadas relativas al contenedor de retina
  const relX = instrumentCenterX - bloodVesselsRect.left;
  const relY = instrumentCenterY - bloodVesselsRect.top;
  
  // Verificar proximidad a vasos (simulación)
  const proximityThreshold = 30;
  const vesselProximity = Math.abs(relX - bloodVesselsRect.width/2) < proximityThreshold && 
                         Math.abs(relY - bloodVesselsRect.height/2) < proximityThreshold;
  
  if (vesselProximity && !vesselDamageActive) {
    vesselDamageActive = true;
    hemorrhageActive = true;
    showAlert('vessel-damage-alert', 0); // Mostrar hasta que el usuario la cierre
    
    // Efecto visual de hemorragia
    document.getElementById('hemorrhage-effect').style.display = 'block';
    gsap.to('#hemorrhage-effect', { opacity: 0.7, duration: 1 });
    
    // Crear coágulos de sangre
    createBloodClots(5, {x: instrumentCenterX, y: instrumentCenterY});
    
    // Afectar parámetros
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
    
    // Posición aleatoria cerca de la posición del instrumento o del corte
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
    clot.style.height = `${10 + Math.random() * 20}px`;
    
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
    
    // Si no quedan coágulos, limpiar hemorragia
    if (bloodClots.length === 0) {
      hemorrhageActive = false;
      vesselDamageActive = false;
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
  
  // Verificar colisión con cada coágulo de sangre
  bloodClots.forEach((clot, index) => {
    const dx = instrumentCenterX - (retinaRect.left + clot.x);
    const dy = instrumentCenterY - (retinaRect.top + clot.y);
    const distance = Math.sqrt(dx * dx + dy * dy);
    
    if (distance < clot.size + instrumentRect.width/2) {
      // Eliminar coágulo
      removeBloodClot(index);
      
      // Efecto visual de eliminación
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
  // Simular desprendimiento basado en presión
  if (iop > 28) {
    showAlert('retina-detachment-alert');
    return true;
  }
  return false;
}

function checkHighIOP() {
  if (iop > 25) {
    showAlert('high-iop-alert', 0); // Mostrar hasta que la presión baje
    
    // Efecto visual en nervio óptico
    document.querySelector('.retina-nerve').style.backgroundColor = 'rgba(255,255,255,0.8)';
    document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(255,255,255,0.6), 0 0 25px rgba(255,255,255,0.5)';
    
    return true;
  } else {
    // Restaurar apariencia normal si la presión baja
    document.querySelector('.retina-nerve').style.backgroundColor = '';
    document.querySelector('.optic-disc').style.boxShadow = 'inset 0 0 20px rgba(220,80,80,0.6), 0 0 25px rgba(220,80,80,0.5)';
    document.getElementById('high-iop-alert').style.display = 'none';
    return false;
  }
}

function checkLowDepth() {
  if (currentDepth > -80) {
    showAlert('low-depth-alert');
    return true;
  }
  return false;
}

/* ================== CONTROL DE JOYSTICKS MEJORADO ================== */
function initJoysticks() {
  const joystickVitrectomo = document.getElementById('joystick-vitrectomo');
  initJoystick(joystickVitrectomo, (x, y) => {
    vitrectomoJoystickX = x;
    vitrectomoJoystickY = y;
    updateInstrumentPosition(x, y);
    updateMiniLeftLine(x, y);
  });
  
  const joystickLight = document.getElementById('joystick-light');
  initJoystick(joystickLight, (x, y) => {
    lightJoystickX = x;
    lightJoystickY = y;
    updateEndoLightEffect(x, y);
    updateMiniRightLine(x, y);
    checkWallCollision(); // Verificar colisión para el endoiluminador
  });
}

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
      
      // Actualizar sombra según posición de la luz
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
      
      // Verificar colisiones
      checkWallCollision();
      
      // Verificar hemorragia si está en profundidad máxima
      if (currentDepth <= -200) {
        checkVesselDamage();
      }
      
      // Actualizar indicador de profundidad
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
  
  // Activar efectos de dispersión de luz
  if (zVal > 100) {
    document.getElementById('light-scatter').classList.add('active');
    document.getElementById('specular-highlight').classList.add('active');
  } else {
    document.getElementById('light-scatter').classList.remove('active');
    document.getElementById('specular-highlight').classList.remove('active');
  }
  
  // Actualizar reflejos en los instrumentos
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

/* ================== GESTIÓN DE INSTRUMENTOS ================== */
function setupEventListeners() {
  // Event listeners para instrumentos
  document.getElementById('btn-vitrectomo').addEventListener('click', () => toggleInstrument('btn-vitrectomo', 'vitrectome'));
  document.getElementById('btn-peeling').addEventListener('click', () => toggleInstrument('btn-peeling', 'end-grasper'));
  document.getElementById('btn-laser').addEventListener('click', () => toggleInstrument('btn-laser', 'laser-probe'));
  document.getElementById('btn-injection').addEventListener('click', performInjection);
  document.getElementById('btn-scissors').addEventListener('click', () => toggleInstrument('btn-scissors', 'scissors'));
  document.getElementById('btn-pfc').addEventListener('click', () => toggleInstrument('btn-pfc', 'pfc-probe'));
  
  // Botón de acción principal
  document.getElementById('btn-precionar').addEventListener('click', handleMainAction);
  
  // Sliders
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
    // Desactivar todos los instrumentos primero
    document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.instrument').forEach(i => i.style.display = 'none');
    
    // Activar el seleccionado
    btn.classList.add('active');
    activeInstrument = instrumentId;
    document.getElementById(instrumentId).style.display = 'block';
    
    // Añadir clase active si corresponde
    if(instrumentId === 'end-grasper') {
      document.getElementById('end-grasper').classList.add('active');
    } else if (instrumentId === 'scissors') {
      document.getElementById('scissors').classList.add('active');
    }
  }
}

function handleMainAction() {
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
  
  switch(activeInstrument) {
    case 'laser-probe':
      laserFunction(syntheticEvent);
      break;
    case 'vitrectome':
      vitrectomyFunction(syntheticEvent);
      // Eliminar coágulos cercanos
      removeNearbyBloodClots(instrument, x, y);
      break;
    case 'end-grasper':
      toggleGrasping();
      break;
    case 'scissors':
      cuttingFunction(syntheticEvent);
      // Crear coágulo solo si está en profundidad profunda
      if (currentDepth <= -150) {
        createBloodClots(1, {x: retinaRect.left + x, y: retinaRect.top + y});
      }
      break;
    case 'pfc-probe':
      injectPFC();
      break;
  }
  
  // Verificar daño vascular solo si está en profundidad máxima y es un instrumento que puede causarlo
  if (currentDepth <= -200 && (activeInstrument === 'vitrectome' || activeInstrument === 'scissors')) {
    checkVesselDamage();
  }
}

function toggleGrasping() {
  const endGrasper = document.getElementById('end-grasper');
  if(endGrasper.classList.contains('grasping')) {
    endGrasper.classList.remove('grasping');
  } else {
    endGrasper.classList.add('grasping');
  }
}

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
  
  // Crear marca de quemadura permanente
  const burnMark = document.createElement('div');
  burnMark.className = 'laser-burn-permanent';
  burnMark.style.left = (e.clientX - rect.left - 3) + 'px';
  burnMark.style.top = (e.clientY - rect.top - 3) + 'px';
  retina.appendChild(burnMark);
  
  // Guardar la marca permanente en el array
  laserBurns.push({
    element: burnMark,
    x: e.clientX - rect.left - 3,
    y: e.clientY - rect.top - 3
  });
  
  // Afectar parámetros
  iop += 0.5;
  perfusion -= 0.2;
  
  // Eliminar solo el efecto temporal
  setTimeout(() => {
    laserSpot.remove();
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

function cuttingFunction(e) {
  const retina = document.getElementById('retina');
  const rect = retina.getBoundingClientRect();
  
  // Animación de corte
  const scissors = document.getElementById('scissors');
  scissors.classList.add('cutting');
  setTimeout(() => scissors.classList.remove('cutting'), 300);
  
  // Efecto visual de corte
  const cutEffect = document.createElement('div');
  cutEffect.className = 'cut-effect';
  cutEffect.style.left = (e.clientX - rect.left - 15) + 'px';
  cutEffect.style.top = (e.clientY - rect.top - 15) + 'px';
  retina.appendChild(cutEffect);
  
  setTimeout(() => cutEffect.remove(), 1000);
  
  // Actualizar parámetros
  iop += 0.2;
}

function injectPFC() {
  if (pfcLevel >= 100) return;
  
  const retina = document.getElementById('retina');
  const pfcBubbles = document.getElementById('pfc-bubbles');
  
  // Crear burbujas de PFC
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
  
  // Actualizar nivel de PFC
  pfcLevel = Math.min(100, pfcLevel + 10);
  document.getElementById('pfc-value').textContent = `${pfcLevel}%`;
  
  // Afectar parámetros
  iop += 3;
  perfusion += 2;
  
  // Cambiar aspecto de la retina
  retina.style.background = `
    radial-gradient(circle at 35% 45%, rgba(100,255,100,0.1) 0%, rgba(50,200,50,0.2) 70%, transparent 100%),
    ${retina.style.background}`;
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

/* ================== ACTUALIZACIÓN DE PARÁMETROS ================== */
function updateVitals() {
  // Variación aleatoria de los parámetros
  iop += (Math.random() - 0.5) * 0.1;
  perfusion += (Math.random() - 0.5) * 0.2;
  flowRate += (Math.random() - 0.5) * 0.1;
  
  // Efecto de instrumentos activos
  if (activeInstrument === 'vitrectome') {
    iop -= 0.05;
    vitreousRemoved = Math.min(100, vitreousRemoved + 0.1);
  }
  
  // Efecto de PFC
  if (pfcLevel > 0) {
    iop = Math.max(iop, 12 + pfcLevel / 10);
    perfusion = Math.min(perfusion + pfcLevel / 50, 100);
  }
  
  // Verificar alertas basadas en parámetros
  checkRetinaDetachment();
  checkHighIOP();
  
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
  
  // Actualizar cada segundo
  setTimeout(updateVitals, 1000);
}
    </script>

</body>
</html>
