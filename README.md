
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador Quirúrgico Retiniano Avanzado | VR Surgical Pro</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&family=Open+Sans:wght@300;400;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --primary-color: #0066cc;
      --primary-light: #00aaff;
      --secondary-color: #6a4b2e;
      --accent-color: #ff7043;
      --text-light: #f5f5f5;
      --text-muted: rgba(255, 255, 255, 0.7);
      --bg-dark: #0a0a1a;
      --bg-darker: #050510;
      --success-color: #4caf50;
      --warning-color: #ff9800;
      --danger-color: #f44336;
      --expert-color: #d32f2f;
      --advanced-color: #ffa000;
      --intermediate-color: #ffc107;
      --beginner-color: #4caf50;
    }

    /* Reset y estilos base */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    html, body {
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, var(--bg-darker) 0%, var(--bg-dark) 100%);
      font-family: 'Montserrat', sans-serif;
      color: var(--text-light);
      overflow: hidden;
      position: relative;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }

    /* Efecto de partículas de fondo mejorado */
    #particles-js {
      position: absolute;
      width: 100%;
      height: 100%;
      z-index: 1;
      background: radial-gradient(ellipse at bottom, var(--bg-darker) 0%, #000000 100%);
    }

    /* Contenedor principal mejorado */
    #main-container {
      position: relative;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 2;
      padding: 20px;
      overflow: hidden;
    }

    /* Encabezado profesional mejorado */
    .header {
      position: absolute;
      top: 0;
      width: 100%;
      padding: 20px 40px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 10;
      backdrop-filter: blur(5px);
      background: rgba(10, 10, 26, 0.6);
      border-bottom: 1px solid rgba(0, 170, 255, 0.1);
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
      transition: transform 0.3s ease;
    }

    .logo:hover {
      transform: scale(1.02);
    }

    .logo-icon {
      width: 44px;
      height: 44px;
      background: linear-gradient(135deg, var(--primary-light) 0%, var(--primary-color) 100%);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-weight: bold;
      font-size: 18px;
      box-shadow: 0 0 15px rgba(0, 170, 255, 0.4);
    }

    .logo-text {
      font-size: 20px;
      font-weight: 700;
      background: linear-gradient(90deg, var(--primary-light), #ffffff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      letter-spacing: 0.5px;
    }

    .user-controls {
      display: flex;
      align-items: center;
      gap: 15px;
    }

    .user-btn {
      background: rgba(0, 102, 204, 0.2);
      border: 1px solid rgba(0, 170, 255, 0.3);
      border-radius: 30px;
      padding: 8px 16px;
      color: var(--text-light);
      font-size: 0.85rem;
      font-weight: 500;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .user-btn:hover {
      background: rgba(0, 102, 204, 0.4);
      transform: translateY(-1px);
    }

    .user-btn i {
      font-size: 0.9rem;
    }

    /* Ojo central ultra realista */
    #eye-container {
      position: relative;
      width: 320px;
      height: 320px;
      margin: 40px 0;
      cursor: pointer;
      transition: all 0.5s cubic-bezier(0.22, 1, 0.36, 1);
      z-index: 5;
      filter: drop-shadow(0 0 30px rgba(0, 170, 255, 0.3));
    }

    #eye {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: radial-gradient(
        circle at 40% 40%, 
        #f5e6d0 0%, 
        #d2a679 15%, 
        #8c5e3c 45%, 
        #4b3621 70%,
        #2a1c0e 100%
      );
      box-shadow: 
        inset 0 0 50px rgba(255,255,255,0.15),
        inset 15px 0 40px rgba(255,255,255,0.08),
        inset -15px 0 40px rgba(0,0,0,0.4),
        inset 0 -15px 40px rgba(0,0,0,0.4),
        0 0 80px rgba(0, 0, 0, 0.9);
      overflow: hidden;
      transition: all 0.8s cubic-bezier(0.22, 1, 0.36, 1);
    }

    /* Iris con detalles profesionales mejorados */
    #eye::before {
      content: '';
      position: absolute;
      width: 66.66%;
      height: 66.66%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: radial-gradient(
        circle at 40% 40%,
        #6a4b2e 0%,
        #4b3621 30%,
        #2a1c0e 70%,
        #000 100%
      );
      border-radius: 50%;
      box-shadow: 
        inset 0 0 30px rgba(0,0,0,0.9),
        inset 15px 0 40px rgba(255,215,0,0.15),
        inset -15px 0 40px rgba(0,100,255,0.15),
        0 0 20px rgba(0, 0, 0, 0.6);
      transition: all 1.2s cubic-bezier(0.22, 1, 0.36, 1);
    }

    /* Reflejos profesionales en la córnea mejorados */
    #eye::after {
      content: '';
      position: absolute;
      width: 43.33%;
      height: 43.33%;
      top: 15%;
      left: 15%;
      background: radial-gradient(
        circle, 
        rgba(255,255,255,0.95) 0%, 
        rgba(255,255,255,0.75) 30%, 
        transparent 80%
      );
      border-radius: 50%;
      transform: rotate(-30deg);
      filter: blur(1.5px);
      transition: all 1s ease-out;
      pointer-events: none;
    }

    /* Pupila profesional con efecto dinámico mejorado */
    .pupil {
      position: absolute;
      width: 20%;
      height: 20%;
      top: 50%;
      left: 50%;
      background: radial-gradient(
        circle, 
        #000 0%, 
        #000 70%, 
        #1a0d00 100%
      );
      border-radius: 50%;
      transform: translate(-50%, -50%);
      box-shadow: 
        inset 0 0 30px rgba(0,0,0,0.9),
        inset 8px 0 20px rgba(100,100,255,0.25),
        inset -8px 0 20px rgba(255,100,50,0.25),
        0 0 25px rgba(0,0,0,0.7);
      z-index: 2;
      transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
    }

    /* Reflejos pupilares profesionales mejorados */
    .pupil::after {
      content: '';
      position: absolute;
      width: 25%;
      height: 25%;
      top: 25%;
      left: 25%;
      background: rgba(255,255,255,0.85);
      border-radius: 50%;
      filter: blur(1px);
      transition: all 0.8s ease-out;
    }

    /* Vasos sanguíneos ultra realistas mejorados */
    .blood-vessels {
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-image: 
        radial-gradient(ellipse at 30% 40%, transparent 60%, rgba(180, 40, 40, 0.25) 60.5%),
        radial-gradient(ellipse at 40% 50%, transparent 65%, rgba(180, 40, 40, 0.25) 65.5%),
        radial-gradient(ellipse at 50% 60%, transparent 70%, rgba(180, 40, 40, 0.25) 70.5%),
        radial-gradient(ellipse at 60% 50%, transparent 65%, rgba(180, 40, 40, 0.2) 65.5%),
        radial-gradient(ellipse at 50% 40%, transparent 60%, rgba(180, 40, 40, 0.2) 60.5%);
      z-index: 1;
      pointer-events: none;
      transition: all 1s ease-in-out;
      filter: drop-shadow(0 0 2px rgba(150, 30, 30, 0.3));
    }

    /* Contenido interior del ojo mejorado */
    .eye-interior {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: radial-gradient(circle, #1a0d00 0%, #000 100%);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      opacity: 0;
      pointer-events: none;
      z-index: 3;
      transition: opacity 0.8s cubic-bezier(0.64, 0, 0.78, 0);
      padding: 30px;
      text-align: center;
    }

    .eye-interior h2 {
      font-size: 2.2rem;
      margin-bottom: 20px;
      color: var(--primary-light);
      text-shadow: 0 0 15px rgba(0, 170, 255, 0.6);
      max-width: 90%;
      line-height: 1.3;
      opacity: 0;
      transform: translateY(20px);
      transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.3s;
      font-weight: 600;
    }

    .eye-interior p {
      font-size: 1.1rem;
      color: var(--text-muted);
      max-width: 80%;
      opacity: 0;
      transform: translateY(20px);
      transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.5s;
      font-family: 'Open Sans', sans-serif;
      line-height: 1.6;
    }

    .loading-bar {
      width: 80%;
      height: 4px;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 2px;
      margin-top: 30px;
      overflow: hidden;
      opacity: 0;
      transform: translateY(20px);
      transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.7s;
    }

    .loading-progress {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, var(--primary-color), var(--primary-light));
      border-radius: 2px;
      transition: width 1.5s ease 1s;
    }

    /* Contenedores de botones laterales mejorados */
    .buttons-container {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 20px;
      z-index: 4;
      transition: all 0.8s cubic-bezier(0.65, 0, 0.35, 1);
    }

    .buttons-left {
      left: 5%;
    }

    .buttons-right {
      right: 5%;
    }

    /* Botones de casos clínicos profesionales mejorados */
    .case-button {
      width: 240px;
      height: 70px;
      background: linear-gradient(145deg, rgba(0, 102, 204, 0.7), rgba(0, 68, 136, 0.7));
      border: none;
      border-radius: 35px;
      color: white;
      font-size: 0.95rem;
      font-weight: 500;
      text-align: left;
      cursor: pointer;
      padding: 0 25px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: flex-start;
      box-shadow: 
        0 6px 20px rgba(0, 0, 0, 0.4),
        0 0 0 1px rgba(0, 204, 255, 0.4);
      transition: all 0.4s cubic-bezier(0.22, 1, 0.36, 1);
      backdrop-filter: blur(8px);
      overflow: hidden;
      position: relative;
    }

    .case-button::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(145deg, rgba(0, 136, 255, 0.9), rgba(0, 102, 204, 0.9));
      opacity: 0;
      transition: opacity 0.4s ease;
      z-index: 0;
    }

    .case-button:hover {
      transform: translateY(-3px);
      box-shadow: 
        0 8px 25px rgba(0, 0, 0, 0.5),
        0 0 0 1px rgba(0, 204, 255, 0.6);
    }

    .case-button:hover::before {
      opacity: 1;
    }

    .case-button span {
      position: relative;
      z-index: 1;
    }

    .case-button .case-title {
      font-weight: 600;
      font-size: 1.05rem;
      margin-bottom: 5px;
      letter-spacing: 0.3px;
    }

    .case-button .case-difficulty {
      font-size: 0.75rem;
      font-weight: 400;
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .difficulty-badge {
      padding: 2px 8px;
      border-radius: 10px;
      font-size: 0.65rem;
      font-weight: 600;
      text-transform: uppercase;
    }

    .beginner {
      background-color: var(--beginner-color);
    }

    .intermediate {
      background-color: var(--intermediate-color);
      color: #000;
    }

    .advanced {
      background-color: var(--advanced-color);
      color: #000;
    }

    .expert {
      background-color: var(--expert-color);
    }

    /* Efecto de entrada profesional mejorado */
    @keyframes eyeEnter {
      0% {
        transform: scale(1);
        border-radius: 50%;
        box-shadow: 0 0 80px rgba(0, 0, 0, 0.9);
      }
      20% {
        transform: scale(1.8);
        border-radius: 45%;
        box-shadow: 0 0 120px rgba(0, 170, 255, 0.6);
      }
      50% {
        transform: scale(8);
        border-radius: 35%;
        box-shadow: 0 0 200px rgba(0, 170, 255, 0.8);
      }
      80% {
        transform: scale(20);
        border-radius: 15%;
        box-shadow: 0 0 300px rgba(0, 170, 255, 0.9);
      }
      100% {
        transform: scale(35);
        border-radius: 0;
        box-shadow: 0 0 400px rgba(0, 170, 255, 1);
      }
    }

    .enter-effect {
      animation: eyeEnter 2s cubic-bezier(0.22, 1, 0.36, 1) forwards;
    }

    /* Efectos durante la animación mejorados */
    .enter-effect::before {
      transform: translate(-50%, -50%) scale(1.8);
      opacity: 0.8;
    }

    .enter-effect::after {
      transform: scale(1.8) rotate(-30deg);
      opacity: 0.6;
    }

    .enter-effect .pupil {
      transform: translate(-50%, -50%) scale(2);
    }

    .enter-effect .pupil::after {
      transform: scale(1.8);
      opacity: 0.9;
    }

    .enter-effect .blood-vessels {
      opacity: 0.2;
      transform: scale(1.5);
    }

    .show-interior h2,
    .show-interior p,
    .show-interior .loading-bar {
      opacity: 1;
      transform: translateY(0);
    }

    .show-interior .loading-progress {
      width: 100%;
    }

    /* Footer profesional mejorado */
    .footer {
      position: absolute;
      bottom: 20px;
      width: 100%;
      text-align: center;
      font-size: 0.85rem;
      color: var(--text-muted);
      z-index: 10;
      font-family: 'Open Sans', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    .footer-links {
      display: flex;
      gap: 20px;
    }

    .footer-link {
      color: var(--text-muted);
      text-decoration: none;
      transition: color 0.3s ease;
    }

    .footer-link:hover {
      color: var(--primary-light);
    }

    /* Efecto de carga profesional mejorado */
    .loader {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: var(--bg-darker);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      transition: opacity 0.5s ease;
    }

    .loader-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 30px;
    }

    .loader-logo {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .loader-logo-icon {
      width: 44px;
      height: 44px;
      background: linear-gradient(135deg, var(--primary-light) 0%, var(--primary-color) 100%);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-weight: bold;
      font-size: 18px;
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(0, 170, 255, 0.4); }
      50% { transform: scale(1.05); box-shadow: 0 0 0 15px rgba(0, 170, 255, 0); }
      100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(0, 170, 255, 0); }
    }

    .loader-logo-text {
      font-size: 20px;
      font-weight: 700;
      background: linear-gradient(90deg, var(--primary-light), #ffffff);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      letter-spacing: 0.5px;
    }

    .loader-circle {
      width: 60px;
      height: 60px;
      border: 4px solid rgba(0, 170, 255, 0.2);
      border-top-color: var(--primary-light);
      border-radius: 50%;
      animation: spin 1.2s linear infinite;
    }

    .loader-text {
      color: var(--text-muted);
      font-size: 0.9rem;
      margin-top: 20px;
      font-family: 'Open Sans', sans-serif;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    /* Notificación de sistema */
    .notification {
      position: fixed;
      top: 30px;
      right: 30px;
      background: rgba(0, 68, 136, 0.9);
      backdrop-filter: blur(10px);
      border-left: 4px solid var(--primary-light);
      color: white;
      padding: 15px 20px;
      border-radius: 5px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
      z-index: 100;
      transform: translateX(200%);
      transition: transform 0.5s cubic-bezier(0.22, 1, 0.36, 1);
      max-width: 300px;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .notification.show {
      transform: translateX(0);
    }

    .notification i {
      font-size: 1.2rem;
      color: var(--primary-light);
    }

    .notification-content {
      flex: 1;
    }

    .notification-title {
      font-weight: 600;
      margin-bottom: 5px;
    }

    .notification-message {
      font-size: 0.9rem;
      opacity: 0.9;
      font-family: 'Open Sans', sans-serif;
    }

    /* Responsive design mejorado */
    @media (max-width: 1024px) {
      .buttons-container {
        gap: 15px;
      }
      
      .case-button {
        width: 200px;
        height: 65px;
        padding: 0 20px;
      }
    }

    @media (max-width: 768px) {
      .header {
        padding: 15px 20px;
      }
      
      .logo-icon, .loader-logo-icon {
        width: 36px;
        height: 36px;
        font-size: 16px;
      }
      
      .logo-text, .loader-logo-text {
        font-size: 16px;
      }
      
      .user-btn {
        padding: 6px 12px;
        font-size: 0.8rem;
      }
      
      #eye-container {
        width: 250px;
        height: 250px;
        margin: 30px 0;
      }
      
      .buttons-container {
        position: static;
        transform: none;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        margin-top: 30px;
        width: 100%;
        padding: 0 20px;
        gap: 12px;
      }
      
      .buttons-left, .buttons-right {
        position: static;
        width: 100%;
      }
      
      .case-button {
        width: 100%;
        max-width: 300px;
        height: 60px;
        margin-bottom: 0;
      }
      
      .eye-interior h2 {
        font-size: 1.8rem;
      }
      
      .eye-interior p {
        font-size: 1rem;
      }
      
      .footer {
        font-size: 0.75rem;
      }
    }

    @media (max-width: 480px) {
      .header {
        padding: 12px 15px;
      }
      
      .logo {
        gap: 8px;
      }
      
      .user-controls {
        gap: 8px;
      }
      
      #eye-container {
        width: 200px;
        height: 200px;
      }
      
      .case-button {
        height: 55px;
        padding: 0 15px;
      }
      
      .case-button .case-title {
        font-size: 0.95rem;
      }
      
      .eye-interior h2 {
        font-size: 1.5rem;
      }
      
      .eye-interior p {
        font-size: 0.9rem;
      }
      
      .footer-links {
        gap: 12px;
        flex-wrap: wrap;
        justify-content: center;
      }
    }
  </style>
</head>
<body>
  <!-- Efecto de partículas mejorado -->
  <div id="particles-js"></div>

  <!-- Pantalla de carga profesional -->
  <div class="loader">
    <div class="loader-content">
      <div class="loader-logo">
        <div class="loader-logo-icon">VR</div>
        <div class="loader-logo-text">VitreoRetinal Pro</div>
      </div>
      <div class="loader-circle"></div>
      <div class="loader-text">Inicializando simulador quirúrgico...</div>
    </div>
  </div>

  <!-- Notificación del sistema -->
  <div class="notification">
    <i class="fas fa-info-circle"></i>
    <div class="notification-content">
      <div class="notification-title">Simulador listo</div>
      <div class="notification-message">Seleccione un caso clínico para comenzar</div>
    </div>
  </div>

  <!-- Contenedor principal mejorado -->
  <div id="main-container">
    <!-- Encabezado profesional mejorado -->
    <div class="header">
      <div class="logo">
        <div class="logo-icon">VR</div>
        <div class="logo-text">VitreoRetinal Pro</div>
      </div>
      <div class="user-controls">
        <button class="user-btn">
          <i class="fas fa-user-md"></i>
          <span>Dr. Usuario</span>
        </button>
        <button class="user-btn">
          <i class="fas fa-cog"></i>
          <span>Ajustes</span>
        </button>
      </div>
    </div>

    <!-- Ojo central ultra realista mejorado -->
    <div id="eye-container">
      <div id="eye">
        <div class="blood-vessels"></div>
        <div class="pupil"></div>
        
        <!-- Contenido interior del ojo mejorado -->
        <div class="eye-interior">
          <h2 id="case-title">Caso Clínico</h2>
          <p id="case-description">Inicializando entorno quirúrgico...</p>
          <div class="loading-bar">
            <div class="loading-progress"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Contenedor de botones izquierdo mejorado -->
    <div class="buttons-container buttons-left">
      <button class="case-button" data-case="prueba.html" data-name="Membrana Epirretiniana" data-difficulty="Intermedio">
        <span class="case-title">Membrana Epirretiniana</span>
        <span class="case-difficulty">
          <span class="difficulty-badge intermediate">Intermedio</span>
        </span>
      </button>
      <button class="case-button" data-case="index.html" data-name="Agujero Macular" data-difficulty="Avanzado">
        <span class="case-title">Agujero Macular</span>
        <span class="case-difficulty">
          <span class="difficulty-badge advanced">Avanzado</span>
        </span>
      </button>
      <button class="case-button" data-case="simuladorDR.html" data-name="Desprendimiento de Retina" data-difficulty="Experto">
        <span class="case-title">Desprendimiento de Retina</span>
        <span class="case-difficulty">
          <span class="difficulty-badge expert">Experto</span>
        </span>
      </button>
      <button class="case-button" data-case="SimuladorHV.html" data-name="Hemorragia Vítrea" data-difficulty="Intermedio">
        <span class="case-title">Hemorragia Vítrea</span>
        <span class="case-difficulty">
          <span class="difficulty-badge intermediate">Intermedio</span>
        </span>
      </button>
    </div>

    <!-- Contenedor de botones derecho mejorado -->
    <div class="buttons-container buttons-right">
      <button class="case-button" data-case="app.html" data-name="Traumatismo Retiniano" data-difficulty="Avanzado">
        <span class="case-title">Traumatismo Retiniano</span>
        <span class="case-difficulty">
          <span class="difficulty-badge advanced">Avanzado</span>
        </span>
      </button>
      <button class="case-button" data-case="app5.html" data-name="Tracción Vítreo-Macular" data-difficulty="Intermedio">
        <span class="case-title">Tracción Vítreo-Macular</span>
        <span class="case-difficulty">
          <span class="difficulty-badge intermediate">Intermedio</span>
        </span>
      </button>
      <button class="case-button" data-case="academ.html" data-name="Extracción Cuerpo Extraño" data-difficulty="Experto">
        <span class="case-title">Extracción Cuerpo Extraño</span>
        <span class="case-difficulty">
          <span class="difficulty-badge expert">Experto</span>
        </span>
      </button>
      <button class="case-button" data-case="practica6.html" data-name="Síndrome de Interface" data-difficulty="Intermedio">
        <span class="case-title">Síndrome de Interface</span>
        <span class="case-difficulty">
          <span class="difficulty-badge intermediate">Intermedio</span>
        </span>
      </button>
    </div>

    <!-- Footer profesional mejorado -->
    <div class="footer">
      <div class="footer-links">
        <a href="#" class="footer-link">Términos de uso</a>
        <a href="#" class="footer-link">Privacidad</a>
        <a href="#" class="footer-link">Soporte técnico</a>
        <a href="#" class="footer-link">Contacto</a>
      </div>
      <div>Simulador Quirúrgico Retiniano Avanzado - © 2025 VitreoRetinal Pro</div>
    </div>
  </div>

  <!-- Scripts mejorados -->
  <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
  <script>
    // Ocultar loader con efecto profesional
    window.addEventListener('load', function() {
      setTimeout(function() {
        document.querySelector('.loader').style.opacity = '0';
        setTimeout(function() {
          document.querySelector('.loader').style.display = 'none';
          // Mostrar notificación
          const notification = document.querySelector('.notification');
          notification.classList.add('show');
          setTimeout(() => {
            notification.classList.remove('show');
          }, 5000);
        }, 500);
      }, 1500);
    });

    // Inicializar partículas mejoradas
    document.addEventListener('DOMContentLoaded', function() {
      if (typeof particlesJS !== 'undefined') {
        particlesJS('particles-js', {
          particles: {
            number: { 
              value: 80, 
              density: { 
                enable: true, 
                value_area: 1000 
              } 
            },
            color: { 
              value: "#00aaff",
              animation: {
                enable: true,
                speed: 10,
                sync: false
              }
            },
            shape: { 
              type: "circle",
              stroke: {
                width: 0,
                color: "#000000"
              }
            },
            opacity: { 
              value: 0.6, 
              random: true,
              animation: {
                enable: true,
                speed: 1,
                minimumValue: 0.1,
                sync: false
              }
            },
            size: { 
              value: 3, 
              random: true,
              animation: {
                enable: true,
                speed: 2,
                minimumValue: 1,
                sync: false
              }
            },
            line_linked: { 
              enable: true, 
              distance: 150, 
              color: "#00aaff", 
              opacity: 0.3, 
              width: 1,
              shadow: {
                enable: true,
                blur: 5,
                color: "#0066cc"
              }
            },
            move: { 
              enable: true, 
              speed: 3, 
              direction: "none", 
              random: true, 
              straight: false, 
              out_mode: "out",
              attract: {
                enable: true,
                rotateX: 600,
                rotateY: 1200
              }
            }
          },
          interactivity: {
            detect_on: "canvas",
            events: {
              onhover: { 
                enable: true, 
                mode: "grab",
                parallax: {
                  enable: true,
                  force: 30,
                  smooth: 10
                }
              },
              onclick: { 
                enable: true, 
                mode: "push" 
              }
            },
            modes: {
              grab: {
                distance: 140,
                line_linked: {
                  opacity: 0.8
                }
              },
              push: {
                particles_nb: 4
              }
            }
          },
          retina_detect: true
        });
      }

      const eye = document.getElementById('eye');
      const caseButtons = document.querySelectorAll('.case-button');
      const eyeInterior = document.querySelector('.eye-interior');
      const caseTitle = document.getElementById('case-title');
      const caseDescription = document.getElementById('case-description');
      const buttonsLeft = document.querySelector('.buttons-left');
      const buttonsRight = document.querySelector('.buttons-right');
      
      // Efecto de seguimiento de la pupila con el mouse mejorado
      document.addEventListener('mousemove', (e) => {
        const pupil = document.querySelector('.pupil');
        if (!pupil) return;
        
        const eyeRect = eye.getBoundingClientRect();
        const eyeCenterX = eyeRect.left + eyeRect.width / 2;
        const eyeCenterY = eyeRect.top + eyeRect.height / 2;
        
        const angle = Math.atan2(e.clientY - eyeCenterY, e.clientX - eyeCenterX);
        const distance = Math.min(35, 
          Math.sqrt(Math.pow(e.clientX - eyeCenterX, 2) + Math.pow(e.clientY - eyeCenterY, 2)) / 60 * 18);
        
        pupil.style.transform = `translate(-50%, -50%) translate(${Math.cos(angle) * distance}px, ${Math.sin(angle) * distance}px)`;
        
        // Efecto de reflejo dinámico
        const eyeContainer = document.getElementById('eye-container');
        const mouseX = (e.clientX / window.innerWidth) * 100;
        const mouseY = (e.clientY / window.innerHeight) * 100;
        eyeContainer.style.filter = `drop-shadow(${(mouseX - 50) / 5}px ${(mouseY - 50) / 5}px 30px rgba(0, 170, 255, 0.4))`;
      });

      // Función para iniciar la cirugía con efecto visual profesional mejorado
      function startSurgery(caseFile, caseName, difficulty) {
        // Deshabilitar botones durante la animación
        caseButtons.forEach(btn => {
          btn.style.pointerEvents = 'none';
          btn.style.opacity = '0.5';
          btn.style.transform = 'translateY(0)';
        });
        
        // Mostrar información del caso dentro del ojo
        caseTitle.textContent = caseName;
        caseDescription.textContent = `Preparando simulación ${difficulty.toLowerCase()}...`;
        
        // Ocultar botones laterales con animación mejorada
        buttonsLeft.style.opacity = '0';
        buttonsLeft.style.transform = 'translateY(-50%) translateX(-30px)';
        buttonsRight.style.opacity = '0';
        buttonsRight.style.transform = 'translateY(-50%) translateX(30px)';
        
        // Aplicar efecto de entrada al ojo mejorado
        eye.classList.add('enter-effect');
        
        // Mostrar el interior del ojo con efecto escalonado mejorado
        setTimeout(() => {
          eyeInterior.style.opacity = '1';
          setTimeout(() => {
            eyeInterior.classList.add('show-interior');
          }, 400);
        }, 1000);
        
        // Redirigir después de la animación mejorada
        setTimeout(() => {
          window.location.href = caseFile;
        }, 2500);
      }

      // Asignar eventos a los botones de casos mejorados
      caseButtons.forEach(btn => {
        btn.addEventListener('click', () => {
          const caseFile = btn.getAttribute('data-case');
          const caseName = btn.getAttribute('data-name');
          const difficulty = btn.getAttribute('data-difficulty');
          startSurgery(caseFile, caseName, difficulty);
        });

        // Efecto hover profesional mejorado
        btn.addEventListener('mouseenter', () => {
          const eyeContainer = document.getElementById('eye-container');
          eyeContainer.style.transform = 'scale(1.08)';
          eyeContainer.style.transition = 'transform 0.4s cubic-bezier(0.22, 1, 0.36, 1)';
          
          // Efecto de iluminación en el ojo
          eye.style.boxShadow = 'inset 0 0 60px rgba(255,255,255,0.2), inset 20px 0 50px rgba(255,255,255,0.1), inset -20px 0 50px rgba(0,0,0,0.5), inset 0 -20px 50px rgba(0,0,0,0.5), 0 0 80px rgba(0, 170, 255, 0.6)';
        });

        btn.addEventListener('mouseleave', () => {
          const eyeContainer = document.getElementById('eye-container');
          eyeContainer.style.transform = 'scale(1)';
          eye.style.boxShadow = 'inset 0 0 50px rgba(255,255,255,0.15), inset 15px 0 40px rgba(255,255,255,0.08), inset -15px 0 40px rgba(0,0,0,0.4), inset 0 -15px 40px rgba(0,0,0,0.4), 0 0 80px rgba(0, 0, 0, 0.9)';
        });
      });

      // Efecto hover en el ojo mejorado
      eye.addEventListener('mouseenter', () => {
        eye.style.transform = 'scale(1.05)';
        eye.style.boxShadow = 'inset 0 0 60px rgba(255,255,255,0.2), inset 20px 0 50px rgba(255,255,255,0.1), inset -20px 0 50px rgba(0,0,0,0.5), inset 0 -20px 50px rgba(0,0,0,0.5), 0 0 80px rgba(0, 170, 255, 0.6)';
      });

      eye.addEventListener('mouseleave', () => {
        eye.style.transform = 'scale(1)';
        eye.style.boxShadow = 'inset 0 0 50px rgba(255,255,255,0.15), inset 15px 0 40px rgba(255,255,255,0.08), inset -15px 0 40px rgba(0,0,0,0.4), inset 0 -15px 40px rgba(0,0,0,0.4), 0 0 80px rgba(0, 0, 0, 0.9)';
      });
      
      // Efecto de parpadeo aleatorio del ojo
      function randomBlink() {
        const blinkDuration = 100 + Math.random() * 50;
        const blinkDelay = 3000 + Math.random() * 8000;
        
        setTimeout(() => {
          eye.style.height = '5px';
          eye.style.transform = 'scaleY(0.05)';
          eye.style.transition = `all ${blinkDuration}ms cubic-bezier(0.47, 0, 0.745, 0.715)`;
          
          setTimeout(() => {
            eye.style.height = '100%';
            eye.style.transform = 'scaleY(1)';
            eye.style.transition = `all ${blinkDuration}ms cubic-bezier(0.755, 0.05, 0.855, 0.06)`;
            
            randomBlink();
          }, blinkDuration);
        }, blinkDelay);
      }
      
      // Iniciar el efecto de parpadeo después de la carga
      setTimeout(randomBlink, 3000);
    });
  </script>
</body>
</html>
