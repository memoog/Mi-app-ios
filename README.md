<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="Dra. Stephania Chávez Cobian - Oftalmóloga especialista en retina y vítreo en La Paz, BCS. Diagnóstico y tratamiento avanzado de enfermedades retinianas.">
<title>Dra. Stephania Chávez Cobian – Oftalmóloga Especialista en Retina</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Playfair+Display:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
<style>
  :root {
    --primary: #0056b3;
    --primary-dark: #003d82;
    --accent: #00a8e8;
    --accent-light: #6ecff6;
    --bg: #f8fafc;
    --text: #333;
    --text-light: #555;
    --light: #fff;
    --gray: #e2e8f0;
    --transition: all 0.4s cubic-bezier(0.25, 0.8, 0.25, 1);
    --shadow-sm: 0 2px 8px rgba(0,0,0,0.1);
    --shadow-md: 0 4px 12px rgba(0,0,0,0.15);
    --shadow-lg: 0 8px 24px rgba(0,0,0,0.2);
    --radius-sm: 8px;
    --radius-md: 12px;
    --radius-lg: 16px;
  }
  
  *, *::before, *::after { 
    margin:0; 
    padding:0; 
    box-sizing:border-box;
  }
  
  html { 
    scroll-behavior: smooth; 
    font-size: 16px;
  }
  
  body {
    font-family: 'Poppins', system-ui, sans-serif;
    color: var(--text);
    background: var(--bg);
    overflow-x: hidden;
    position: relative;
    line-height: 1.6;
  }

  h1, h2, h3, h4 {
    font-family: 'Playfair Display', serif;
    font-weight: 600;
    line-height: 1.2;
  }

  /* SCROLL PROGRESS */
  #progress {
    position: fixed; 
    top:0; 
    left:0; 
    height:4px; 
    background: linear-gradient(90deg, var(--accent), var(--primary));
    width:0; 
    z-index:1500; 
    transition: width .2s ease;
  }

  /* HEADER / NAV */
  header {
    position: fixed; 
    top:0; 
    left:0; 
    width:100%;
    background: rgba(255,255,255,0.98);
    backdrop-filter: blur(8px);
    box-shadow: var(--shadow-sm);
    z-index:1000;
    transition: var(--transition);
  }
  
  header.scrolled {
    box-shadow: var(--shadow-md);
  }
  
  .nav-container {
    max-width:1200px; 
    margin:auto;
    display:flex; 
    align-items:center; 
    justify-content:space-between;
    padding:1.2rem 2rem;
  }
  
  .logo { 
    font-size:1.5rem; 
    font-weight:600; 
    color:var(--primary); 
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .logo-icon {
    color: var(--accent);
    font-size: 1.8rem;
  }
  
  nav ul {
    list-style:none; 
    display:flex; 
    gap:2rem;
  }
  
  nav a {
    position: relative; 
    font-weight:500; 
    transition: color .3s;
    padding: 0.5rem 0;
    color: var(--text);
    font-size: 0.95rem;
  }
  
  nav a::after {
    content:''; 
    position:absolute; 
    left:0; 
    bottom:0;
    width:0; 
    height:2px; 
    background:var(--accent);
    transition: width .3s;
  }
  
  nav a:hover, 
  nav a.active { 
    color:var(--primary);
  }
  
  nav a:hover::after, 
  nav a.active::after { 
    width:100%;
  }
  
  .btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    padding: 0.75rem 1.75rem;
    border-radius: 50px;
    font-weight: 500;
    transition: var(--transition);
    cursor: pointer;
    text-decoration: none;
    border: none;
    white-space: nowrap;
  }
  
  .btn-primary {
    background: var(--primary);
    color: var(--light);
    box-shadow: 0 4px 12px rgba(0, 86, 179, 0.25);
  }
  
  .btn-primary:hover {
    background: var(--primary-dark);
    transform: translateY(-2px);
    box-shadow: 0 6px 16px rgba(0, 86, 179, 0.35);
  }
  
  .btn-outline {
    background: transparent;
    color: var(--primary);
    border: 2px solid var(--primary);
  }
  
  .btn-outline:hover {
    background: var(--primary);
    color: var(--light);
    transform: translateY(-2px);
  }
  
  .hamburger {
    display:none; 
    flex-direction:column; 
    gap:5px; 
    cursor:pointer;
    padding: 0.5rem;
    z-index: 1100;
  }
  
  .hamburger div {
    width:25px; 
    height:2px; 
    background: var(--primary);
    transition: var(--transition);
  }
  
  .hamburger.active div:nth-child(1) {
    transform: translateY(7px) rotate(45deg);
  }
  
  .hamburger.active div:nth-child(2) {
    opacity: 0;
  }
  
  .hamburger.active div:nth-child(3) {
    transform: translateY(-7px) rotate(-45deg);
  }
  
  @media(max-width:992px) {
    .nav-container {
      padding: 1rem;
    }
    
    nav ul {
      position:fixed; 
      top:0; 
      right:-100%; 
      height:100vh; 
      width:280px;
      background:var(--light); 
      flex-direction:column; 
      padding: 6rem 2rem 2rem;
      transition: right 0.5s ease;
      box-shadow: -4px 0 16px rgba(0,0,0,0.1);
    }
    
    nav ul.show { 
      right:0; 
    }
    
    .hamburger { 
      display:flex; 
    }
    
    .btn-primary { 
      display:none; 
    }
  }

  /* HERO */
  .hero {
    position:relative; 
    height:100vh; 
    min-height: 600px;
    overflow:hidden;
    display:flex; 
    align-items:center; 
    justify-content:center;
    text-align:center; 
    color:var(--light);
  }
  
  .hero-img {
    position:absolute; 
    top:0; 
    left:0; 
    width:100%; 
    height:100%;
    object-fit:cover; 
    filter: brightness(0.7);
    z-index:0; 
    transform: scale(1.1);
    animation: zoom 20s ease infinite;
  }
  
  @keyframes zoom {
    0% { transform: scale(1.1); }
    50% { transform: scale(1.2); }
    100% { transform: scale(1.1); }
  }
  
  .hero::before {
    content:''; 
    position:absolute; 
    inset:0;
    background: linear-gradient(135deg, rgba(0,64,128,0.7) 0%, rgba(0,168,232,0.4) 100%);
    z-index:1;
  }
  
  .hero-content {
    position:relative; 
    z-index:2;
    max-width:800px; 
    padding:0 2rem;
    opacity:0; 
    transform:translateY(20px);
    transition:opacity .8s ease, transform .8s ease;
  }
  
  .hero-content.visible {
    opacity:1; 
    transform:translateY(0);
  }
  
  .hero h1 {
    font-size:3.5rem; 
    margin-bottom:1.5rem;
    font-weight:700; 
    line-height:1.2; 
    letter-spacing:0.5px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }
  
  .hero p {
    font-size:1.3rem; 
    margin-bottom:2.5rem;
    color:rgba(255,255,255,0.9);
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }
  
  .typing {
    display:inline-block; 
    overflow:hidden;
    white-space:nowrap; 
    border-right:3px solid var(--light);
    animation: blink-caret 0.75s step-end infinite;
  }
  
  @keyframes blink-caret {
    from, to { border-color: transparent }
    50% { border-color: var(--light); }
  }
  
  .hero-btns {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
  }

  /* SECTIONS */
  section {
    padding:7rem 2rem; 
    opacity:0; 
    transform:translateY(30px);
    transition:opacity .8s ease, transform .8s ease;
  }
  
  section.visible {
    opacity:1; 
    transform:translateY(0);
  }
  
  .section-title {
    font-size:2.5rem; 
    color:var(--primary);
    margin-bottom:2rem; 
    display:inline-block;
    position:relative;
  }
  
  .section-title::after {
    content:''; 
    display:block;
    width:80px; 
    height:4px; 
    background:var(--accent);
    position:absolute; 
    bottom:-12px; 
    left:0;
  }
  
  .section-subtitle {
    color: var(--text-light);
    font-size: 1.1rem;
    max-width: 700px;
    margin-bottom: 3rem;
  }
  
  .container { 
    max-width:1200px; 
    margin:auto; 
  }
  
  .text-center {
    text-align: center;
  }
  
  .mx-auto {
    margin-left: auto;
    margin-right: auto;
  }

  /* ABOUT */
  .about {
    display:flex; 
    flex-wrap:wrap; 
    gap:3rem; 
    align-items:center;
  }
  
  .about-img {
    flex:1 1 350px; 
    border-radius:var(--radius-lg); 
    overflow:hidden;
    box-shadow: var(--shadow-lg);
    transition: var(--transition);
    position: relative;
  }
  
  .about-img::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(to bottom, rgba(0,88,179,0.1) 0%, rgba(0,168,232,0.2) 100%);
    z-index: 1;
  }
  
  .about-img img {
    width:100%; 
    height:100%; 
    object-fit:cover;
    min-height: 450px;
  }
  
  .about-img:hover { 
    transform:scale(1.02);
    box-shadow: var(--shadow-lg);
  }
  
  .about-text { 
    flex:2 1 400px; 
  }
  
  .about-text p {
    margin-bottom: 1.5rem;
    color: var(--text-light);
  }
  
  .about-features {
    margin-top: 2rem;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
  }
  
  .feature-item {
    display: flex;
    gap: 0.75rem;
  }
  
  .feature-icon {
    color: var(--accent);
    font-size: 1.5rem;
    margin-top: 0.2rem;
  }
  
  .feature-text h3 {
    font-size: 1.1rem;
    margin-bottom: 0.25rem;
    color: var(--primary);
  }
  
  .feature-text p {
    font-size: 0.9rem;
    margin-bottom: 0;
    color: var(--text-light);
  }

  /* SERVICES */
  .services-container {
    background: var(--light);
    padding: 4rem 2rem;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
  }
  
  .services {
    display:grid; 
    gap:2rem;
    grid-template-columns: repeat(auto-fit,minmax(280px,1fr));
  }
  
  .service-card {
    background:var(--light); 
    padding:2.5rem 2rem; 
    border-radius:var(--radius-md);
    box-shadow: var(--shadow-sm);
    transition: var(--transition);
    border: 1px solid var(--gray);
    position: relative;
    overflow: hidden;
    z-index: 1;
  }
  
  .service-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 4px;
    height: 0;
    background: var(--accent);
    transition: var(--transition);
    z-index: -1;
  }
  
  .service-card:hover {
    box-shadow: var(--shadow-md);
    transform: translateY(-5px);
    border-color: var(--accent-light);
  }
  
  .service-card:hover::before {
    height: 100%;
  }
  
  .service-icon {
    font-size: 2.5rem;
    color: var(--primary);
    margin-bottom: 1.5rem;
    transition: var(--transition);
  }
  
  .service-card:hover .service-icon {
    color: var(--accent);
    transform: scale(1.1);
  }
  
  .service-card h3 {
    margin-bottom:1rem; 
    color:var(--primary);
    font-weight:600; 
    font-size: 1.4rem;
  }
  
  .service-card p { 
    color:var(--text-light); 
    font-size: 0.95rem;
  }

  /* LOCATION */
  .location {
    background: var(--light);
  }
  
  .map-container {
    border-radius:var(--radius-md); 
    overflow:hidden;
    box-shadow: var(--shadow-md);
    margin-bottom:2rem;
    height: 400px;
  }
  
  .map-container iframe { 
    width:100%; 
    height:100%; 
    border:0; 
  }
  
  .location-info {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
    margin-top: 3rem;
  }
  
  .info-card {
    background: var(--bg);
    padding: 1.5rem;
    border-radius: var(--radius-md);
    display: flex;
    gap: 1rem;
    align-items: flex-start;
    transition: var(--transition);
  }
  
  .info-card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-sm);
  }
  
  .info-icon {
    font-size: 1.5rem;
    color: var(--accent);
    margin-top: 0.25rem;
  }
  
  .info-text h3 {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
    color: var(--primary);
  }
  
  .info-text p {
    color: var(--text-light);
    font-size: 0.95rem;
  }

  /* CONTACT */
  .contact { 
    max-width:800px; 
    margin:auto; 
  }
  
  .contact-container {
    background: var(--light);
    padding: 3rem;
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-sm);
  }
  
  .contact form {
    display:flex; 
    flex-direction:column; 
    gap:1.5rem;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .form-group label {
    font-weight: 500;
    color: var(--text);
    font-size: 0.95rem;
  }
  
  .contact input, 
  .contact textarea,
  .contact select {
    padding:1rem; 
    border:1px solid var(--gray);
    border-radius:var(--radius-sm); 
    font-family:inherit;
    transition: var(--transition);
    width: 100%;
  }
  
  .contact input:focus, 
  .contact textarea:focus,
  .contact select:focus {
    border-color:var(--accent);
    box-shadow:0 0 0 3px rgba(0,168,232,0.2);
    outline:none;
  }
  
  .contact button {
    margin-top: 1rem;
  }
  
  .contact-info {
    margin-top:3rem; 
    font-size:1rem; 
    color:var(--text-light);
  }
  
  .contact-info-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }
  
  .contact-method {
    display: flex;
    gap: 1rem;
    align-items: center;
  }
  
  .contact-icon {
    font-size: 1.5rem;
    color: var(--accent);
  }
  
  .contact-details h3 {
    font-size: 1.1rem;
    margin-bottom: 0.25rem;
    color: var(--primary);
  }
  
  .contact-details a {
    color: var(--text-light);
    text-decoration: none;
    transition: color 0.3s;
  }
  
  .contact-details a:hover {
    color: var(--accent);
    text-decoration: underline;
  }

  /* TESTIMONIALS */
  .testimonials {
    background: var(--primary);
    color: var(--light);
  }
  
  .testimonials .section-title {
    color: var(--light);
  }
  
  .testimonials .section-title::after {
    background: var(--accent-light);
  }
  
  .testimonials .section-subtitle {
    color: rgba(255,255,255,0.8);
  }
  
  .testimonial-container {
    max-width: 800px;
    margin: 0 auto;
  }
  
  .testimonial-slide {
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(5px);
    padding: 2.5rem;
    border-radius: var(--radius-md);
    margin: 0 1rem;
    text-align: center;
  }
  
  .testimonial-content {
    font-size: 1.1rem;
    font-style: italic;
    margin-bottom: 1.5rem;
    line-height: 1.8;
  }
  
  .testimonial-author {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
  
  .author-img {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid var(--accent-light);
  }
  
  .author-name {
    font-weight: 600;
    font-size: 1.1rem;
  }
  
  .author-title {
    font-size: 0.9rem;
    opacity: 0.8;
  }

  /* BACK TO TOP */
  #toTop {
    position:fixed; 
    bottom:30px; 
    right:30px;
    background: var(--primary); 
    color:var(--light);
    padding:1rem; 
    border:none; 
    border-radius:50%;
    font-size:1.2rem; 
    cursor:pointer; 
    opacity:0;
    transition: var(--transition); 
    z-index:999;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    box-shadow: var(--shadow-md);
  }
  
  #toTop:hover {
    background: var(--accent);
    transform: translateY(-3px);
  }
  
  #toTop.show { 
    opacity:1; 
  }

  /* FOOTER */
  footer {
    background:var(--primary-dark); 
    color:var(--light);
    padding:4rem 2rem 2rem;
  }
  
  .footer-container {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 3rem;
  }
  
  .footer-col h3 {
    font-size: 1.3rem;
    margin-bottom: 1.5rem;
    position: relative;
    display: inline-block;
  }
  
  .footer-col h3::after {
    content: '';
    position: absolute;
    left: 0;
    bottom: -8px;
    width: 40px;
    height: 2px;
    background: var(--accent);
  }
  
  .footer-links {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  
  .footer-links a {
    color: rgba(255,255,255,0.8);
    text-decoration: none;
    transition: color 0.3s;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .footer-links a:hover {
    color: var(--accent-light);
  }
  
  .footer-links i {
    font-size: 0.8rem;
  }
  
  .footer-about p {
    color: rgba(255,255,255,0.8);
    margin-bottom: 1.5rem;
    font-size: 0.95rem;
  }
  
  .social-links {
    display: flex;
    gap: 1rem;
  }
  
  .social-links a {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(255,255,255,0.1);
    color: var(--light);
    transition: var(--transition);
  }
  
  .social-links a:hover {
    background: var(--accent);
    transform: translateY(-3px);
  }
  
  .footer-bottom {
    text-align: center;
    margin-top: 4rem;
    padding-top: 2rem;
    border-top: 1px solid rgba(255,255,255,0.1);
  }
  
  .footer-bottom small {
    display:block; 
    color:rgba(255,255,255,0.6);
    font-size: 0.85rem;
  }
  
  /* RESPONSIVE ADJUSTMENTS */
  @media (max-width: 768px) {
    html {
      font-size: 15px;
    }
    
    .hero h1 {
      font-size: 2.5rem;
    }
    
    .hero p {
      font-size: 1.1rem;
    }
    
    .section-title {
      font-size: 2rem;
    }
    
    .contact-container {
      padding: 2rem 1.5rem;
    }
  }
  
  @media (max-width: 480px) {
    .hero h1 {
      font-size: 2rem;
    }
    
    .hero-btns {
      flex-direction: column;
      gap: 0.75rem;
    }
    
    .hero-btns .btn {
      width: 100%;
    }
    
    .about-img {
      min-height: 350px;
    }
  }
</style>
</head>
<body>

<div id="progress"></div>

<!-- HEADER -->
<header id="header">
  <div class="nav-container">
    <a href="#home" class="logo">
      <i class="fas fa-eye logo-icon"></i>
      <span>Dra. Stephania Chávez</span>
    </a>
    <nav>
      <div class="hamburger" id="hamburger">
        <div></div>
        <div></div>
        <div></div>
      </div>
      <ul id="nav-menu">
        <li><a href="#about">Sobre mí</a></li>
        <li><a href="#services">Servicios</a></li>
        <li><a href="#testimonials">Testimonios</a></li>
        <li><a href="#location">Ubicación</a></li>
        <li><a href="#contact">Contacto</a></li>
      </ul>
    </nav>
    <a href="#contact" class="btn btn-primary">
      <i class="fas fa-calendar-alt"></i> Agendar Cita
    </a>
  </div>
</header>

<!-- HERO -->
<section class="hero" id="home">
  <img class="hero-img" src="https://images.unsplash.com/photo-1579684385127-1ef15d508118?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80" alt="Consultorio de Oftalmología">
  <div class="hero-content" id="hero-content">
    <h1 class="typing" id="typing"></h1>
    <p>Diagnóstico y tratamiento avanzado de enfermedades de retina y vítreo en La Paz, Baja California Sur.</p>
    <div class="hero-btns">
      <a href="#about" class="btn btn-primary">
        <i class="fas fa-user-md"></i> Conoce más
      </a>
      <a href="#contact" class="btn btn-outline">
        <i class="fas fa-phone-alt"></i> Contactar
      </a>
    </div>
  </div>
</section>

<!-- SOBRE MÍ -->
<section id="about">
  <div class="container">
    <div class="text-center">
      <h2 class="section-title">Sobre la Doctora</h2>
      <p class="section-subtitle mx-auto">Especialista en retina con enfoque humano y tecnología de vanguardia para el cuidado de tu visión.</p>
    </div>
    <div class="about">
      <div class="about-img">
        <img src="" alt="Dra. Stephania Chávez Cobian">
      </div>
      <div class="about-text">
        <p>La <strong>Dra. Stephania Chávez Cobian</strong> es oftalmóloga especialista en retina y vítreo, egresada con honores de la Universidad Autónoma de Baja California (UABC). Con más de 10 años de experiencia en el campo de la oftalmología, se ha dedicado especialmente al diagnóstico y tratamiento de enfermedades retinianas.</p>
        <p>Su formación incluye una subespecialidad en Retina y Vítreo en el prestigioso Instituto de Oftalmología Conde de Valenciana en la Ciudad de México, donde se capacitó en las técnicas más avanzadas para el manejo de patologías retinianas complejas.</p>
        <p>Es miembro activo de la Sociedad Mexicana de Retina y Vítreo, y participa regularmente en congresos nacionales e internacionales para mantenerse a la vanguardia en los últimos avances en su campo.</p>
        
        <div class="about-features">
          <div class="feature-item">
            <div class="feature-icon">
              <i class="fas fa-award"></i>
            </div>
            <div class="feature-text">
              <h3>Experiencia</h3>
              <p>10+ años de práctica oftalmológica especializada</p>
            </div>
          </div>
          <div class="feature-item">
            <div class="feature-icon">
              <i class="fas fa-user-graduate"></i>
            </div>
            <div class="feature-text">
              <h3>Formación</h3>
              <p>Especialidad en Retina y Vítreo</p>
            </div>
          </div>
          <div class="feature-item">
            <div class="feature-icon">
              <i class="fas fa-flask"></i>
            </div>
            <div class="feature-text">
              <h3>Tecnología</h3>
              <p>Equipo de diagnóstico de última generación</p>
            </div>
          </div>
          <div class="feature-item">
            <div class="feature-icon">
              <i class="fas fa-hand-holding-heart"></i>
            </div>
            <div class="feature-text">
              <h3>Enfoque</h3>
              <p>Atención personalizada y humana</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- SERVICIOS -->
<section id="services">
  <div class="container">
    <div class="text-center">
      <h2 class="section-title">Servicios Especializados</h2>
      <p class="section-subtitle mx-auto">Técnicas avanzadas para el diagnóstico y tratamiento de enfermedades retinianas.</p>
    </div>
    
    <div class="services-container">
      <div class="services">
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-eye-dropper"></i>
          </div>
          <h3>Cirugía de Retina</h3>
          <p>Reparación mínimamente invasiva de desprendimientos y agujeros retinianos con técnicas de vitrectomía microincisional.</p>
        </div>
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-syringe"></i>
          </div>
          <h3>Retinopatía Diabética</h3>
          <p>Tratamiento integral con láser y terapias intravítreas antiangiogénicas para preservar la visión en pacientes diabéticos.</p>
        </div>
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-search-plus"></i>
          </div>
          <h3>Degeneración Macular</h3>
          <p>Diagnóstico y manejo personalizado de DMAE húmeda y seca con seguimiento especializado.</p>
        </div>
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-procedures"></i>
          </div>
          <h3>Hemorragias Vítreas</h3>
          <p>Vitrectomía para limpieza de hemorragias vítreas con equipos de alta precisión.</p>
        </div>
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-microscope"></i>
          </div>
          <h3>Evaluación Diagnóstica</h3>
          <p>Estudios especializados como OCT, angiografía fluoresceínica y ultrasonido ocular de alta resolución.</p>
        </div>
        <div class="service-card">
          <div class="service-icon">
            <i class="fas fa-laser"></i>
          </div>
          <h3>Desprendimiento de Retina</h3>
          <p>Cirugía especializada con técnicas de neumoretinopexia, retinopexia con láser y vitrectomía posterior.</p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- TESTIMONIALS -->
<section class="testimonials" id="testimonials">
  <div class="container text-center">
    <h2 class="section-title">Pacientes Satisfechos</h2>
    <p class="section-subtitle mx-auto">Experiencias de pacientes que han recuperado su visión con nuestro tratamiento especializado.</p>
    
    <div class="testimonial-container">
      <div class="testimonial-slide">
        <p class="testimonial-content">"La Dra. Chávez salvó mi visión cuando sufrí un desprendimiento de retina. Su profesionalismo y calidez humana hicieron que todo el proceso fuera mucho más llevadero. Estoy eternamente agradecido."</p>
        <div class="testimonial-author">
          <img src="https://randomuser.me/api/portraits/men/65.jpg" alt="Juan Pérez" class="author-img">
          <div>
            <div class="author-name">Juan Pérez</div>
            <div class="author-title">Paciente con desprendimiento de retina</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- UBICACIÓN -->
<section class="location" id="location">
  <div class="container">
    <div class="text-center">
      <h2 class="section-title">Nuestro Consultorio</h2>
      <p class="section-subtitle mx-auto">Ubicado en un área accesible con instalaciones modernas y equipamiento de última generación.</p>
    </div>
    
    <div class="map-container">
      <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3663.215245347626!2d-110.320888924374!3d24.13850987894633!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x86afd2e57f6a3f9f%3A0x7f0e6c8e3a4e4b4f!2sLa%20Paz%2C%20B.C.S.!5e0!3m2!1ses!2smx!4v1689876543210!5m2!1ses!2smx" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
    </div>
    
    <div class="location-info">
      <div class="info-card">
        <div class="info-icon">
          <i class="fas fa-map-marker-alt"></i>
        </div>
        <div class="info-text">
          <h3>Dirección</h3>
          <p>Blvd. Constituyentes 113, Consultorio 2, La Paz, Baja California Sur</p>
        </div>
      </div>
      <div class="info-card">
        <div class="info-icon">
          <i class="fas fa-clock"></i>
        </div>
        <div class="info-text">
          <h3>Horario</h3>
          <p>Lunes a Viernes: 9:00 - 18:00 hrs<br>Sábados: 9:00 - 13:00 hrs</p>
        </div>
      </div>
      <div class="info-card">
        <div class="info-icon">
          <i class="fas fa-parking"></i>
        </div>
        <div class="info-text">
          <h3>Estacionamiento</h3>
          <p>Área de estacionamiento privado disponible para pacientes</p>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- CONTACTO -->
<section id="contact">
  <div class="container">
    <div class="text-center">
      <h2 class="section-title">Agenda tu Cita</h2>
      <p class="section-subtitle mx-auto">Contáctanos para una evaluación especializada de tu salud visual.</p>
    </div>
    
    <div class="contact-container">
      <form>
        <div class="form-group">
          <label for="name">Nombre completo</label>
          <input type="text" id="name" placeholder="Ingresa tu nombre" required>
        </div>
        
        <div class="form-group">
          <label for="email">Correo electrónico</label>
          <input type="email" id="email" placeholder="Ingresa tu correo" required>
        </div>
        
        <div class="form-group">
          <label for="phone">Teléfono / WhatsApp</label>
          <input type="tel" id="phone" placeholder="Ingresa tu teléfono" required>
        </div>
        
        <div class="form-group">
          <label for="service">Servicio de interés</label>
          <select id="service" required>
            <option value="" disabled selected>Selecciona un servicio</option>
            <option value="consulta">Consulta de retina</option>
            <option value="cirugia">Cirugía de retina</option>
            <option value="diabetes">Retinopatía diabética</option>
            <option value="macula">Degeneración macular</option>
            <option value="otro">Otro</option>
          </select>
        </div>
        
        <div class="form-group">
          <label for="message">Mensaje</label>
          <textarea id="message" rows="5" placeholder="Describe tu motivo de consulta"></textarea>
        </div>
        
        <button type="submit" class="btn btn-primary">
          <i class="fas fa-paper-plane"></i> Enviar Mensaje
        </button>
      </form>
      
      <div class="contact-info text-center">
        <h3>Otras formas de contacto</h3>
        
        <div class="contact-info-grid">
          <div class="contact-method">
            <div class="contact-icon">
              <i class="fas fa-phone-alt"></i>
            </div>
            <div class="contact-details">
              <h3>Teléfono</h3>
              <a href="tel:+526122470993">+52 (612) 247 09 93</a>
            </div>
          </div>
          
          <div class="contact-method">
            <div class="contact-icon">
              <i class="fab fa-whatsapp"></i>
            </div>
            <div class="contact-details">
              <h3>WhatsApp</h3>
              <a href="https://wa.me/526121587749">+52 (612) 158 77 49</a>
            </div>
          </div>
          
          <div class="contact-method">
            <div class="contact-icon">
              <i class="fas fa-envelope"></i>
            </div>
            <div class="contact-details">
              <h3>Email</h3>
              <a href="mailto:contacto@bajavision.com.mx">contacto@bajavision.com.mx</a>
            </div>
          </div>
          
          <div class="contact-method">
            <div class="contact-icon">
              <i class="fas fa-calendar-check"></i>
            </div>
            <div class="contact-details">
              <h3>Citas en línea</h3>
              <a href="#contact">Agendar ahora</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- VOLVER ARRIBA -->
<button id="toTop" title="Volver arriba">
  <i class="fas fa-arrow-up"></i>
</button>

<!-- FOOTER -->
<footer>
  <div class="footer-container">
    <div class="footer-col footer-about">
      <h3>Dra. Stephania Chávez</h3>
      <p>Especialista en retina y vítreo comprometida con ofrecer la más alta calidad en el cuidado de la visión en La Paz, Baja California Sur.</p>
      <div class="social-links">
        <a href="#"><i class="fab fa-facebook-f"></i></a>
        <a href="#"><i class="fab fa-instagram"></i></a>
        <a href="#"><i class="fab fa-linkedin-in"></i></a>
        <a href="#"><i class="fab fa-youtube"></i></a>
      </div>
    </div>
    
    <div class="footer-col">
      <h3>Servicios</h3>
      <ul class="footer-links">
        <li><a href="#services"><i class="fas fa-chevron-right"></i> Cirugía de Retina</a></li>
        <li><a href="#services"><i class="fas fa-chevron-right"></i> Retinopatía Diabética</a></li>
        <li><a href="#services"><i class="fas fa-chevron-right"></i> Degeneración Macular</a></li>
        <li><a href="#services"><i class="fas fa-chevron-right"></i> Evaluación Diagnóstica</a></li>
      </ul>
    </div>
    
    <div class="footer-col">
      <h3>Enlaces Rápidos</h3>
      <ul class="footer-links">
        <li><a href="#about"><i class="fas fa-chevron-right"></i> Sobre la Doctora</a></li>
        <li><a href="#testimonials"><i class="fas fa-chevron-right"></i> Testimonios</a></li>
        <li><a href="#location"><i class="fas fa-chevron-right"></i> Ubicación</a></li>
        <li><a href="#contact"><i class="fas fa-chevron-right"></i> Contacto</a></li>
      </ul>
    </div>
    
    <div class="footer-col">
      <h3>Horario</h3>
      <ul class="footer-links">
        <li><i class="fas fa-clock"></i> Lunes - Viernes: 9am - 6pm</li>
        <li><i class="fas fa-clock"></i> Sábado: 9am - 1pm</li>
        <li><i class="fas fa-clock"></i> Domingo: Cerrado</li>
        <li><a href="#contact"><i class="fas fa-phone-alt"></i> Urgencias: (612) 158 77 49</a></li>
      </ul>
    </div>
  </div>
  
  <div class="footer-bottom">
    &copy; <span id="year"></span> Dra. Stephania Chávez Cobian. Todos los derechos reservados.
    <small>Desarrollado profesionalmente con <i class="fas fa-heart" style="color: #ff6b6b;"></i> para tu salud visual.</small>
  </div>
</footer>

<script>
// Scroll Progress
const progress = document.getElementById('progress');
window.addEventListener('scroll', () => {
  let h = document.documentElement.scrollHeight - document.documentElement.clientHeight;
  progress.style.width = (window.scrollY / h * 100) + '%';
});

// Header Scroll Effect
const header = document.getElementById('header');
window.addEventListener('scroll', () => {
  header.classList.toggle('scrolled', window.scrollY > 50);
});

// Hamburger Menu
const hamburger = document.getElementById('hamburger'),
      navMenu = document.getElementById('nav-menu');
      
hamburger.addEventListener('click', () => {
  hamburger.classList.toggle('active');
  navMenu.classList.toggle('show');
});

// Close mobile menu when clicking a link
document.querySelectorAll('#nav-menu a').forEach(link => {
  link.addEventListener('click', () => {
    hamburger.classList.remove('active');
    navMenu.classList.remove('show');
  });
});

// Typing Effect
const txt = 'Oftalmóloga Especialista en Retina';
const speed = 100;
let idx = 0;

function typeWriter() {
  if(idx < txt.length) {
    document.getElementById('typing').textContent += txt.charAt(idx);
    idx++;
    setTimeout(typeWriter, speed);
  } else {
    // Blinking cursor animation continues via CSS
  }
}

// Start typing effect when page loads
window.addEventListener('load', () => {
  typeWriter();
  document.getElementById('hero-content').classList.add('visible');
});

// Scroll Reveal & Scroll-Spy
const sections = document.querySelectorAll('section'),
      navLinks = document.querySelectorAll('nav a');
      
const obsOptions = {
  threshold: 0.2
};

const obs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if(e.isIntersecting) {
      e.target.classList.add('visible');
      
      // Activate nav link
      navLinks.forEach(a => {
        a.classList.toggle('active', a.getAttribute('href') === '#'+e.target.id);
      });
    }
  });
}, obsOptions);

sections.forEach(s => obs.observe(s));

// Back to Top
const toTop = document.getElementById('toTop');
window.addEventListener('scroll', () => {
  toTop.classList.toggle('show', window.scrollY > 400);
});

toTop.addEventListener('click', () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
});

// Service Card Tilt Effect
document.querySelectorAll('.service-card').forEach(card => {
  card.addEventListener('mousemove', e => {
    const rect = card.getBoundingClientRect(),
          x = e.clientX - rect.left,
          y = e.clientY - rect.top;
    
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;
    
    const angleX = (y - centerY) / 20;
    const angleY = (centerX - x) / 20;
    
    card.style.transform = `perspective(1000px) rotateX(${angleX}deg) rotateY(${angleY}deg)`;
  });
  
  card.addEventListener('mouseleave', () => {
    card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0)';
  });
});

// Current Year
document.getElementById('year').textContent = new Date().getFullYear();

// Form Submission
const form = document.querySelector('form');
if(form) {
  form.addEventListener('submit', (e) => {
    e.preventDefault();
    
    // Here you would typically send the form data to a server
    // For demo purposes, we'll just show an alert
    alert('Gracias por tu mensaje. Nos pondremos en contacto contigo pronto.');
    form.reset();
  });
}
</script>
</body>
</html>
