<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SHAYNE.DEV — Systems Engineer</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --cyan: #00f5ff;
    --cyan-dim: rgba(0,245,255,0.15);
    --cyan-glow: rgba(0,245,255,0.4);
    --green: #39ff14;
    --green-dim: rgba(57,255,20,0.12);
    --amber: #ff9500;
    --red: #ff3366;
    --bg: #050a0f;
    --bg2: #080f16;
    --bg3: #0d1825;
    --panel: rgba(0,245,255,0.04);
    --border: rgba(0,245,255,0.2);
    --border-bright: rgba(0,245,255,0.6);
    --text: #a8d8e8;
    --text-dim: rgba(168,216,232,0.5);
    --font-mono: 'Share Tech Mono', monospace;
    --font-display: 'Orbitron', sans-serif;
    --font-body: 'Rajdhani', sans-serif;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    font-size: 16px;
    line-height: 1.6;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* CANVAS BACKGROUND */
  #bg-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    pointer-events: none;
  }

  /* SCAN LINE OVERLAY */
  body::after {
    content: '';
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }

  /* WRAPPER */
  .page-wrap {
    position: relative;
    z-index: 1;
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 2rem;
  }

  /* NAV */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    background: rgba(5,10,15,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
  }
  .nav-inner {
    max-width: 1100px;
    margin: 0 auto;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .nav-logo {
    font-family: var(--font-display);
    font-size: 14px;
    font-weight: 700;
    color: var(--cyan);
    letter-spacing: 0.15em;
    text-decoration: none;
  }
  .nav-logo span { color: var(--green); }
  .nav-links { display: flex; gap: 2rem; list-style: none; }
  .nav-links a {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text-dim);
    text-decoration: none;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    transition: color 0.2s;
    position: relative;
  }
  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 0; right: 0;
    height: 1px;
    background: var(--cyan);
    transform: scaleX(0);
    transition: transform 0.2s;
  }
  .nav-links a:hover { color: var(--cyan); }
  .nav-links a:hover::after { transform: scaleX(1); }
  .nav-status {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--green);
  }
  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--green);
    animation: pulse-dot 2s infinite;
  }
  @keyframes pulse-dot {
    0%,100% { opacity: 1; box-shadow: 0 0 4px var(--green); }
    50% { opacity: 0.4; box-shadow: none; }
  }

  /* HERO */
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    padding-top: 56px;
    position: relative;
  }
  .hero-content { width: 100%; }

  .system-label {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--cyan);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 1.5rem;
    opacity: 0;
    animation: fade-up 0.6s 0.3s ease forwards;
  }

  .hero-name {
    font-family: var(--font-display);
    font-size: clamp(3rem, 8vw, 6.5rem);
    font-weight: 900;
    line-height: 0.9;
    letter-spacing: -0.02em;
    color: #fff;
    text-shadow: 0 0 60px rgba(0,245,255,0.3);
    opacity: 0;
    animation: fade-up 0.6s 0.5s ease forwards;
    position: relative;
  }
  .hero-name .accent { color: var(--cyan); }
  .hero-name .glitch {
    position: relative;
    display: inline-block;
  }
  .hero-name .glitch::before,
  .hero-name .glitch::after {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    width: 100%;
  }
  .hero-name .glitch::before {
    color: var(--red);
    animation: glitch-1 4s infinite 2s;
    clip-path: polygon(0 0, 100% 0, 100% 35%, 0 35%);
  }
  .hero-name .glitch::after {
    color: var(--cyan);
    animation: glitch-2 4s infinite 2s;
    clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
  }
  @keyframes glitch-1 {
    0%,90%,100% { transform: translate(0); opacity: 0; }
    92% { transform: translate(-3px, 1px); opacity: 0.8; }
    94% { transform: translate(3px, -1px); opacity: 0.8; }
    96% { transform: translate(-2px, 2px); opacity: 0.8; }
    98% { transform: translate(0); opacity: 0; }
  }
  @keyframes glitch-2 {
    0%,90%,100% { transform: translate(0); opacity: 0; }
    91% { transform: translate(3px, -2px); opacity: 0.7; }
    93% { transform: translate(-3px, 1px); opacity: 0.7; }
    95% { transform: translate(2px, -1px); opacity: 0.7; }
    97% { transform: translate(0); opacity: 0; }
  }

  .hero-title {
    font-family: var(--font-mono);
    font-size: clamp(0.85rem, 2vw, 1.1rem);
    color: var(--cyan);
    letter-spacing: 0.25em;
    text-transform: uppercase;
    margin: 1.5rem 0;
    opacity: 0;
    animation: fade-up 0.6s 0.7s ease forwards;
    min-height: 1.6em;
  }
  #typed-cursor {
    display: inline-block;
    width: 2px;
    height: 1em;
    background: var(--cyan);
    margin-left: 2px;
    vertical-align: middle;
    animation: blink 0.8s infinite;
  }
  @keyframes blink { 0%,100% { opacity: 1; } 50% { opacity: 0; } }

  .hero-bio {
    max-width: 600px;
    color: var(--text-dim);
    font-size: 1.1rem;
    line-height: 1.8;
    margin-bottom: 2.5rem;
    opacity: 0;
    animation: fade-up 0.6s 0.9s ease forwards;
  }

  .hero-ctas {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    opacity: 0;
    animation: fade-up 0.6s 1.1s ease forwards;
  }
  .btn {
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    text-decoration: none;
    padding: 12px 28px;
    border: 1px solid;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
    display: inline-block;
  }
  .btn::before {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 100%;
    background: currentColor;
    opacity: 0.1;
    transition: left 0.3s;
  }
  .btn:hover::before { left: 0; }
  .btn-primary {
    color: var(--bg);
    background: var(--cyan);
    border-color: var(--cyan);
  }
  .btn-primary:hover {
    background: transparent;
    color: var(--cyan);
    box-shadow: 0 0 20px var(--cyan-glow);
  }
  .btn-secondary {
    color: var(--cyan);
    background: transparent;
    border-color: var(--border-bright);
  }
  .btn-secondary:hover {
    border-color: var(--cyan);
    box-shadow: 0 0 20px var(--cyan-glow);
  }

  /* HERO METRICS */
  .hero-metrics {
    position: absolute;
    right: 0; top: 50%;
    transform: translateY(-50%);
    display: flex;
    flex-direction: column;
    gap: 1rem;
    opacity: 0;
    animation: fade-left 0.8s 1.3s ease forwards;
  }
  .metric-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-left: 3px solid var(--cyan);
    padding: 1rem 1.5rem;
    min-width: 200px;
    position: relative;
    overflow: hidden;
  }
  .metric-card::before {
    content: '';
    position: absolute;
    top: 0; right: 0;
    width: 40px; height: 40px;
    background: var(--cyan-dim);
    clip-path: polygon(100% 0, 0 0, 100% 100%);
  }
  .metric-num {
    font-family: var(--font-display);
    font-size: 2rem;
    font-weight: 700;
    color: var(--cyan);
    line-height: 1;
  }
  .metric-label {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-dim);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-top: 4px;
  }
  .metric-bar {
    height: 2px;
    background: var(--border);
    margin-top: 8px;
    overflow: hidden;
  }
  .metric-bar-fill {
    height: 100%;
    background: var(--cyan);
    width: 0%;
    transition: width 2s 1.8s cubic-bezier(0.4,0,0.2,1);
  }

  @keyframes fade-up {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes fade-left {
    from { opacity: 0; transform: translate(20px, -50%); }
    to { opacity: 1; transform: translate(0, -50%); }
  }

  /* SECTION COMMON */
  section {
    padding: 6rem 0;
    position: relative;
  }
  .section-header {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    margin-bottom: 3rem;
  }
  .section-tag {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--cyan);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    white-space: nowrap;
  }
  .section-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(to right, var(--border-bright), transparent);
  }
  .section-title {
    font-family: var(--font-display);
    font-size: clamp(1.5rem, 4vw, 2.5rem);
    font-weight: 700;
    color: #fff;
    letter-spacing: 0.05em;
    margin-bottom: 0.5rem;
  }

  /* TECH STACK */
  #stack { background: var(--bg2); }
  .stack-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
    gap: 1px;
    background: var(--border);
    border: 1px solid var(--border);
  }
  .stack-item {
    background: var(--bg2);
    padding: 1.5rem 1rem;
    text-align: center;
    transition: all 0.2s;
    cursor: default;
    position: relative;
    overflow: hidden;
  }
  .stack-item::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--cyan-dim);
    opacity: 0;
    transition: opacity 0.2s;
  }
  .stack-item:hover::before { opacity: 1; }
  .stack-item:hover .stack-icon { color: var(--cyan); transform: scale(1.1); }
  .stack-icon {
    font-size: 2rem;
    display: block;
    margin-bottom: 0.5rem;
    transition: all 0.2s;
  }
  .stack-name {
    font-family: var(--font-mono);
    font-size: 10px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-dim);
  }
  .stack-level {
    display: flex;
    gap: 2px;
    justify-content: center;
    margin-top: 6px;
  }
  .level-pip {
    width: 6px; height: 6px;
    border: 1px solid var(--border);
    background: transparent;
    transition: background 0.3s;
  }
  .level-pip.active { background: var(--cyan); border-color: var(--cyan); }

  /* PROJECTS */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 1.5rem;
  }
  .project-card {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 1.5rem;
    position: relative;
    transition: all 0.3s;
    overflow: hidden;
    cursor: default;
  }
  .project-card::after {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(to right, var(--cyan), var(--green));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s;
  }
  .project-card:hover {
    border-color: var(--border-bright);
    transform: translateY(-2px);
    box-shadow: 0 8px 32px rgba(0,245,255,0.08);
  }
  .project-card:hover::after { transform: scaleX(1); }
  .project-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 0.75rem;
  }
  .project-name {
    font-family: var(--font-display);
    font-size: 1rem;
    font-weight: 700;
    color: var(--cyan);
    letter-spacing: 0.05em;
  }
  .project-badge {
    font-family: var(--font-mono);
    font-size: 9px;
    letter-spacing: 0.1em;
    padding: 3px 8px;
    border: 1px solid;
    text-transform: uppercase;
  }
  .badge-active { color: var(--green); border-color: rgba(57,255,20,0.4); background: rgba(57,255,20,0.06); }
  .badge-arch { color: var(--amber); border-color: rgba(255,149,0,0.4); background: rgba(255,149,0,0.06); }
  .badge-new { color: var(--cyan); border-color: rgba(0,245,255,0.4); background: rgba(0,245,255,0.06); }
  .project-desc {
    font-size: 0.9rem;
    color: var(--text-dim);
    line-height: 1.7;
    margin-bottom: 1rem;
  }
  .project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-bottom: 1rem;
  }
  .tag {
    font-family: var(--font-mono);
    font-size: 9px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-dim);
    background: rgba(168,216,232,0.06);
    border: 1px solid rgba(168,216,232,0.15);
    padding: 3px 8px;
  }
  .project-stats {
    display: flex;
    gap: 1.5rem;
    border-top: 1px solid var(--border);
    padding-top: 0.75rem;
  }
  .proj-stat {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-dim);
  }
  .proj-stat span { color: var(--cyan); }

  /* ACTIVITY GRAPH */
  #activity { background: var(--bg2); }
  .activity-graph {
    display: grid;
    grid-template-columns: repeat(52, 1fr);
    gap: 3px;
    margin-top: 1rem;
  }
  .activity-col { display: flex; flex-direction: column; gap: 3px; }
  .activity-cell {
    aspect-ratio: 1;
    background: rgba(0,245,255,0.04);
    border: 1px solid rgba(0,245,255,0.06);
    border-radius: 1px;
    transition: all 0.1s;
    cursor: default;
    position: relative;
  }
  .activity-cell:hover {
    transform: scale(1.4);
    z-index: 10;
    border-color: var(--cyan);
  }
  .activity-cell.l1 { background: rgba(0,245,255,0.12); border-color: rgba(0,245,255,0.2); }
  .activity-cell.l2 { background: rgba(0,245,255,0.25); border-color: rgba(0,245,255,0.35); }
  .activity-cell.l3 { background: rgba(0,245,255,0.45); border-color: rgba(0,245,255,0.55); }
  .activity-cell.l4 { background: rgba(0,245,255,0.75); border-color: var(--cyan); box-shadow: 0 0 4px var(--cyan-glow); }
  .activity-legend {
    display: flex;
    align-items: center;
    gap: 6px;
    margin-top: 0.75rem;
    justify-content: flex-end;
  }
  .activity-legend span {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-dim);
  }
  .legend-cell {
    width: 12px; height: 12px;
    border-radius: 1px;
  }

  /* TERMINAL SECTION */
  #terminal-section { background: var(--bg); }
  .terminal-wrap {
    border: 1px solid var(--border);
    background: #020608;
    overflow: hidden;
    position: relative;
  }
  .terminal-bar {
    background: var(--bg3);
    padding: 10px 16px;
    display: flex;
    align-items: center;
    gap: 10px;
    border-bottom: 1px solid var(--border);
  }
  .t-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .t-dot.red { background: #ff5f57; }
  .t-dot.yellow { background: #febc2e; }
  .t-dot.green { background: #28c840; }
  .terminal-title {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text-dim);
    margin-left: 6px;
    letter-spacing: 0.05em;
  }
  .terminal-body {
    padding: 1.5rem;
    font-family: var(--font-mono);
    font-size: 13px;
    line-height: 2;
    min-height: 280px;
  }
  .t-line { display: block; opacity: 0; }
  .t-prompt { color: var(--green); }
  .t-cmd { color: #fff; }
  .t-out { color: var(--text-dim); }
  .t-out.cyan { color: var(--cyan); }
  .t-out.amber { color: var(--amber); }
  .t-out.red { color: var(--red); }

  /* CONTACT */
  #contact {
    text-align: center;
    background: var(--bg2);
    padding: 6rem 0;
  }
  .contact-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1rem;
    margin-top: 3rem;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
  }
  .contact-link {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
    padding: 1.5rem;
    border: 1px solid var(--border);
    text-decoration: none;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
    background: var(--panel);
  }
  .contact-link::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--cyan-dim);
    opacity: 0;
    transition: opacity 0.2s;
  }
  .contact-link:hover::before { opacity: 1; }
  .contact-link:hover { border-color: var(--cyan); transform: translateY(-2px); }
  .contact-link:hover .contact-icon { color: var(--cyan); transform: scale(1.1); }
  .contact-icon {
    font-size: 1.5rem;
    transition: all 0.2s;
  }
  .contact-name {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text-dim);
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }
  .contact-handle {
    font-family: var(--font-mono);
    font-size: 12px;
    color: var(--cyan);
  }

  /* FOOTER */
  footer {
    border-top: 1px solid var(--border);
    padding: 2rem 0;
    text-align: center;
  }
  .footer-text {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-dim);
    letter-spacing: 0.1em;
  }
  .footer-text .cyan { color: var(--cyan); }

  /* CORNER DECORATIONS */
  .corner-deco {
    position: absolute;
    width: 24px; height: 24px;
    pointer-events: none;
  }
  .corner-deco.tl { top: -1px; left: -1px; border-top: 2px solid var(--cyan); border-left: 2px solid var(--cyan); }
  .corner-deco.tr { top: -1px; right: -1px; border-top: 2px solid var(--cyan); border-right: 2px solid var(--cyan); }
  .corner-deco.bl { bottom: -1px; left: -1px; border-bottom: 2px solid var(--cyan); border-left: 2px solid var(--cyan); }
  .corner-deco.br { bottom: -1px; right: -1px; border-bottom: 2px solid var(--cyan); border-right: 2px solid var(--cyan); }

  /* RADAR GRAPHIC */
  .hero-radar {
    position: absolute;
    right: -60px; top: 50%;
    transform: translateY(-50%);
    width: 340px; height: 340px;
    opacity: 0.12;
    pointer-events: none;
  }

  /* SCROLL INDICATOR */
  .scroll-hint {
    position: absolute;
    bottom: 2rem; left: 50%;
    transform: translateX(-50%);
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-dim);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    animation: fade-up 1s 2s ease forwards;
    opacity: 0;
  }
  .scroll-arrow {
    width: 1px; height: 40px;
    background: linear-gradient(to bottom, var(--cyan), transparent);
    animation: scroll-anim 1.5s ease-in-out infinite;
  }
  @keyframes scroll-anim {
    0% { transform: scaleY(0); transform-origin: top; }
    50% { transform: scaleY(1); transform-origin: top; }
    51% { transform: scaleY(1); transform-origin: bottom; }
    100% { transform: scaleY(0); transform-origin: bottom; }
  }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    .hero-metrics { display: none; }
    .hero-radar { display: none; }
    .activity-graph { grid-template-columns: repeat(26, 1fr); }
  }

  /* INTERSECTION OBSERVER ANIMATIONS */
  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }
</style>
</head>
<body>

<canvas id="bg-canvas"></canvas>

<!-- NAV -->
<nav>
  <div class="nav-inner">
    <a href="#" class="nav-logo">SH<span>▸</span>YNE.DEV</a>
    <ul class="nav-links">
      <li><a href="#stack">Stack</a></li>
      <li><a href="#projects">Projects</a></li>
      <li><a href="#activity">Activity</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
    <div class="nav-status">
      <div class="status-dot"></div>
      SYSTEMS ONLINE
    </div>
  </div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="page-wrap" style="width:100%;">
    <div class="hero-content" style="max-width:620px;">
      <p class="system-label">// OPERATOR PROFILE — CLEARANCE: ALPHA</p>
      <h1 class="hero-name">
        <span class="glitch" data-text="SHAYNE">SHAYNE</span><br>
        <span style="color:var(--text-dim); font-weight:400;">SYSTEMS</span>
        <span class="accent">ENG</span>
      </h1>
      <p class="hero-title" id="typed-title"><span id="typed-text"></span><span id="typed-cursor"></span></p>
      <p class="hero-bio">
        Building observable, resilient systems that hold up under pressure. 
        Specializing in containerized multi-tier architectures, chaos engineering, 
        and full-stack telemetry pipelines. If it runs in a container and emits metrics, 
        I've probably instrumented it.
      </p>
      <div class="hero-ctas">
        <a href="#projects" class="btn btn-primary">View Projects</a>
        <a href="#contact" class="btn btn-secondary">Get in Touch</a>
      </div>
    </div>

    <!-- METRICS SIDEBAR -->
    <div class="hero-metrics">
      <div class="metric-card">
        <div class="metric-num" data-target="847">0</div>
        <div class="metric-label">Commits · 2024</div>
        <div class="metric-bar"><div class="metric-bar-fill" data-width="84"></div></div>
      </div>
      <div class="metric-card" style="border-left-color: var(--green);">
        <div class="metric-num" data-target="23">0</div>
        <div class="metric-label">Repos Active</div>
        <div class="metric-bar"><div class="metric-bar-fill" data-width="62" style="background:var(--green)"></div></div>
      </div>
      <div class="metric-card" style="border-left-color: var(--amber);">
        <div class="metric-num" data-target="4">0</div>
        <div class="metric-label">Years DevOps</div>
        <div class="metric-bar"><div class="metric-bar-fill" data-width="92" style="background:var(--amber)"></div></div>
      </div>
    </div>

    <!-- RADAR -->
    <svg class="hero-radar" viewBox="0 0 340 340" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="170" cy="170" r="160" stroke="#00f5ff" stroke-width="0.5"/>
      <circle cx="170" cy="170" r="120" stroke="#00f5ff" stroke-width="0.5"/>
      <circle cx="170" cy="170" r="80" stroke="#00f5ff" stroke-width="0.5"/>
      <circle cx="170" cy="170" r="40" stroke="#00f5ff" stroke-width="0.5"/>
      <line x1="10" y1="170" x2="330" y2="170" stroke="#00f5ff" stroke-width="0.5"/>
      <line x1="170" y1="10" x2="170" y2="330" stroke="#00f5ff" stroke-width="0.5"/>
      <line x1="57" y1="57" x2="283" y2="283" stroke="#00f5ff" stroke-width="0.5" opacity="0.5"/>
      <line x1="283" y1="57" x2="57" y2="283" stroke="#00f5ff" stroke-width="0.5" opacity="0.5"/>
      <!-- Sweep -->
      <path id="radar-sweep" d="M170 170 L170 10 A160 160 0 0 1 330 170 Z" fill="url(#radar-grad)" opacity="0.6">
        <animateTransform attributeName="transform" type="rotate" values="0 170 170;360 170 170" dur="4s" repeatCount="indefinite"/>
      </path>
      <defs>
        <linearGradient id="radar-grad" x1="170" y1="170" x2="330" y2="170">
          <stop offset="0%" stop-color="#00f5ff" stop-opacity="0.3"/>
          <stop offset="100%" stop-color="#00f5ff" stop-opacity="0"/>
        </linearGradient>
      </defs>
      <!-- Blips -->
      <circle cx="230" cy="110" r="3" fill="#00f5ff"><animate attributeName="opacity" values="0;1;0" dur="3s" repeatCount="indefinite" begin="1s"/></circle>
      <circle cx="140" cy="220" r="4" fill="#39ff14"><animate attributeName="opacity" values="0;1;0" dur="3s" repeatCount="indefinite" begin="0.3s"/></circle>
      <circle cx="90" cy="130" r="2" fill="#00f5ff"><animate attributeName="opacity" values="0;1;0" dur="3s" repeatCount="indefinite" begin="2.1s"/></circle>
      <circle cx="260" cy="200" r="3" fill="#ff9500"><animate attributeName="opacity" values="0;1;0" dur="3s" repeatCount="indefinite" begin="0.8s"/></circle>
    </svg>
  </div>

  <div class="scroll-hint">
    <span>SCROLL</span>
    <div class="scroll-arrow"></div>
  </div>
</section>

<!-- TECH STACK -->
<section id="stack">
  <div class="page-wrap">
    <div class="section-header">
      <span class="section-tag">// SYS_STACK</span>
      <div class="section-line"></div>
    </div>
    <h2 class="section-title reveal">Technology Arsenal</h2>
    <div class="stack-grid reveal" style="margin-top:2rem;" id="stack-grid"></div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="page-wrap">
    <div class="section-header">
      <span class="section-tag">// PROJECTS</span>
      <div class="section-line"></div>
    </div>
    <h2 class="section-title reveal">Active Deployments</h2>
    <div class="projects-grid reveal" style="margin-top:2rem;" id="projects-grid"></div>
  </div>
</section>

<!-- ACTIVITY -->
<section id="activity">
  <div class="page-wrap">
    <div class="section-header">
      <span class="section-tag">// COMMIT_LOG</span>
      <div class="section-line"></div>
    </div>
    <h2 class="section-title reveal">Contribution Matrix</h2>
    <div class="reveal" style="margin-top:1.5rem;">
      <div style="display:flex;justify-content:space-between;margin-bottom:0.5rem;">
        <span style="font-family:var(--font-mono);font-size:10px;color:var(--text-dim);letter-spacing:0.1em;">JAN</span>
        <span style="font-family:var(--font-mono);font-size:10px;color:var(--text-dim);letter-spacing:0.1em;">JUN</span>
        <span style="font-family:var(--font-mono);font-size:10px;color:var(--text-dim);letter-spacing:0.1em;">DEC</span>
      </div>
      <div class="activity-graph" id="activity-graph"></div>
      <div class="activity-legend">
        <span>Less</span>
        <div class="legend-cell" style="background:rgba(0,245,255,0.04);border:1px solid rgba(0,245,255,0.06);"></div>
        <div class="legend-cell" style="background:rgba(0,245,255,0.12);"></div>
        <div class="legend-cell" style="background:rgba(0,245,255,0.25);"></div>
        <div class="legend-cell" style="background:rgba(0,245,255,0.45);"></div>
        <div class="legend-cell" style="background:rgba(0,245,255,0.75);border:1px solid var(--cyan);box-shadow:0 0 4px var(--cyan-glow);"></div>
        <span>More</span>
      </div>
    </div>
  </div>
</section>

<!-- TERMINAL -->
<section id="terminal-section">
  <div class="page-wrap">
    <div class="section-header">
      <span class="section-tag">// RUNTIME</span>
      <div class="section-line"></div>
    </div>
    <h2 class="section-title reveal">System Diagnostics</h2>
    <div class="terminal-wrap reveal" style="margin-top:2rem; position:relative;">
      <div class="corner-deco tl"></div>
      <div class="corner-deco tr"></div>
      <div class="corner-deco bl"></div>
      <div class="corner-deco br"></div>
      <div class="terminal-bar">
        <div class="t-dot red"></div>
        <div class="t-dot yellow"></div>
        <div class="t-dot green"></div>
        <span class="terminal-title">shayne@devstation — bash — 80×24</span>
      </div>
      <div class="terminal-body" id="terminal-body"></div>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section id="contact">
  <div class="page-wrap">
    <div class="section-header" style="justify-content:center; margin-bottom:1rem;">
      <span class="section-tag">// CONTACT</span>
    </div>
    <h2 class="section-title reveal" style="font-size:clamp(1.8rem,5vw,3rem);">Establish Connection</h2>
    <p class="reveal" style="color:var(--text-dim); font-family:var(--font-mono); font-size:13px; letter-spacing:0.05em; margin-top:0.5rem;">
      Available for contracts, collaborations, and technical deep-dives.
    </p>
    <div class="contact-grid reveal">
      <a href="https://github.com" class="contact-link" target="_blank">
        <div class="corner-deco tl"></div><div class="corner-deco tr"></div><div class="corner-deco bl"></div><div class="corner-deco br"></div>
        <span class="contact-icon">⌥</span>
        <span class="contact-name">GitHub</span>
        <span class="contact-handle">@shayne-dev</span>
      </a>
      <a href="https://linkedin.com" class="contact-link" target="_blank">
        <div class="corner-deco tl"></div><div class="corner-deco tr"></div><div class="corner-deco bl"></div><div class="corner-deco br"></div>
        <span class="contact-icon">◈</span>
        <span class="contact-name">LinkedIn</span>
        <span class="contact-handle">/in/shayne</span>
      </a>
      <a href="mailto:shayne@dev.io" class="contact-link">
        <div class="corner-deco tl"></div><div class="corner-deco tr"></div><div class="corner-deco bl"></div><div class="corner-deco br"></div>
        <span class="contact-icon">⌁</span>
        <span class="contact-name">Email</span>
        <span class="contact-handle">shayne@dev.io</span>
      </a>
      <a href="#" class="contact-link">
        <div class="corner-deco tl"></div><div class="corner-deco tr"></div><div class="corner-deco bl"></div><div class="corner-deco br"></div>
        <span class="contact-icon">⊞</span>
        <span class="contact-name">Resume</span>
        <span class="contact-handle">Download PDF</span>
      </a>
    </div>
  </div>
</section>

<footer>
  <div class="page-wrap">
    <p class="footer-text">
      SHAYNE.DEV · BUILT WITH <span class="cyan">HTML + CSS + VANILLA JS</span> · 
      ALL SYSTEMS <span class="cyan">NOMINAL</span> · 
      <span id="uptime-counter">00:00:00</span>
    </p>
  </div>
</footer>

<script>
/* ====== PARTICLE CANVAS ====== */
(function() {
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, particles = [], mouse = { x: -1000, y: -1000 };

  function resize() {
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }
  resize();
  window.addEventListener('resize', resize);

  document.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });

  class Particle {
    constructor() { this.reset(); }
    reset() {
      this.x = Math.random() * W;
      this.y = Math.random() * H;
      this.vx = (Math.random() - 0.5) * 0.4;
      this.vy = (Math.random() - 0.5) * 0.4;
      this.r = Math.random() * 1.5 + 0.3;
      this.alpha = Math.random() * 0.4 + 0.1;
      this.color = Math.random() < 0.8 ? '0,245,255' : '57,255,20';
    }
    update() {
      // Mouse repulsion
      const dx = this.x - mouse.x, dy = this.y - mouse.y;
      const dist = Math.sqrt(dx*dx + dy*dy);
      if (dist < 120) {
        const force = (120 - dist) / 120 * 0.8;
        this.vx += dx / dist * force;
        this.vy += dy / dist * force;
      }
      this.vx *= 0.99;
      this.vy *= 0.99;
      this.x += this.vx;
      this.y += this.vy;
      if (this.x < 0 || this.x > W || this.y < 0 || this.y > H) this.reset();
    }
    draw() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(${this.color},${this.alpha})`;
      ctx.fill();
    }
  }

  for (let i = 0; i < 140; i++) particles.push(new Particle());

  function drawGrid() {
    const spacing = 60;
    ctx.strokeStyle = 'rgba(0,245,255,0.025)';
    ctx.lineWidth = 0.5;
    for (let x = 0; x < W; x += spacing) {
      ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, H); ctx.stroke();
    }
    for (let y = 0; y < H; y += spacing) {
      ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(W, y); ctx.stroke();
    }
  }

  function connectParticles() {
    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const dx = particles[i].x - particles[j].x;
        const dy = particles[i].y - particles[j].y;
        const dist = Math.sqrt(dx*dx + dy*dy);
        if (dist < 100) {
          ctx.beginPath();
          ctx.moveTo(particles[i].x, particles[i].y);
          ctx.lineTo(particles[j].x, particles[j].y);
          ctx.strokeStyle = `rgba(0,245,255,${0.06 * (1 - dist/100)})`;
          ctx.lineWidth = 0.5;
          ctx.stroke();
        }
      }
    }
  }

  function animate() {
    ctx.clearRect(0, 0, W, H);
    drawGrid();
    particles.forEach(p => { p.update(); p.draw(); });
    connectParticles();
    requestAnimationFrame(animate);
  }
  animate();
})();

/* ====== TYPING EFFECT ====== */
const titles = [
  'DevOps Engineer',
  'Chaos Architect',
  'Observability Specialist',
  'Container Whisperer',
  'SNMP Wrangler',
];
let ti = 0, ci = 0, deleting = false;
const typedEl = document.getElementById('typed-text');

function typeNext() {
  const current = titles[ti];
  if (!deleting) {
    typedEl.textContent = current.slice(0, ci + 1);
    ci++;
    if (ci === current.length) {
      deleting = true;
      setTimeout(typeNext, 1800);
      return;
    }
  } else {
    typedEl.textContent = current.slice(0, ci - 1);
    ci--;
    if (ci === 0) {
      deleting = false;
      ti = (ti + 1) % titles.length;
    }
  }
  setTimeout(typeNext, deleting ? 45 : 85);
}
setTimeout(typeNext, 1400);

/* ====== COUNTER ANIMATION ====== */
function animateCounters() {
  document.querySelectorAll('[data-target]').forEach(el => {
    const target = parseInt(el.getAttribute('data-target'));
    let cur = 0;
    const step = Math.ceil(target / 60);
    const id = setInterval(() => {
      cur = Math.min(cur + step, target);
      el.textContent = cur;
      if (cur >= target) clearInterval(id);
    }, 25);
  });
  document.querySelectorAll('.metric-bar-fill').forEach(el => {
    setTimeout(() => { el.style.width = el.getAttribute('data-width') + '%'; }, 100);
  });
}
setTimeout(animateCounters, 1600);

/* ====== STACK GRID ====== */
const techs = [
  { icon: '🐳', name: 'Docker', level: 5 },
  { icon: '⚙', name: 'Nginx', level: 4 },
  { icon: '🐍', name: 'FastAPI', level: 4 },
  { icon: '💾', name: 'MySQL', level: 4 },
  { icon: '🔷', name: '.NET', level: 3 },
  { icon: '🐘', name: 'PHP', level: 3 },
  { icon: '📊', name: 'Datadog', level: 5 },
  { icon: '🌐', name: 'SNMP', level: 5 },
  { icon: '🛡', name: 'Linux', level: 5 },
  { icon: '🔧', name: 'Apache', level: 4 },
  { icon: '💿', name: 'MariaDB', level: 4 },
  { icon: '☁', name: 'AWS', level: 3 },
  { icon: '🔐', name: 'SELinux', level: 3 },
  { icon: '📦', name: 'Compose', level: 5 },
  { icon: '⚡', name: 'IIS', level: 3 },
  { icon: '🔬', name: 'APM', level: 4 },
];
const stackGrid = document.getElementById('stack-grid');
techs.forEach(t => {
  const div = document.createElement('div');
  div.className = 'stack-item';
  const pips = Array(5).fill(0).map((_,i) => 
    `<div class="level-pip${i < t.level ? ' active' : ''}"></div>`
  ).join('');
  div.innerHTML = `
    <span class="stack-icon">${t.icon}</span>
    <div class="stack-name">${t.name}</div>
    <div class="stack-level">${pips}</div>
  `;
  stackGrid.appendChild(div);
});

/* ====== PROJECTS ====== */
const projects = [
  {
    name: 'AVION-DASH',
    badge: 'ACTIVE',
    badgeClass: 'badge-active',
    desc: 'Three-tier aviation ops monitoring platform. Nginx → FastAPI → MySQL with 22 chaos fault injections, full SNMPv3 telemetry, and glass-cockpit SPA frontend.',
    tags: ['Docker','FastAPI','MySQL','SNMPv3','Datadog','Nginx'],
    stars: 47, forks: 12, issues: 3
  },
  {
    name: 'ARTEMIS',
    badge: 'STABLE',
    badgeClass: 'badge-arch',
    desc: '.NET 8 / IIS / SQL Server native Windows monitoring stack with 30 fault scenarios. Compatible with Windows Server 2016+.',
    tags: ['.NET 8','IIS','MSSQL','Datadog','Dapper','Windows'],
    stars: 31, forks: 8, issues: 1
  },
  {
    name: 'DOCKER-DASH',
    badge: 'ACTIVE',
    badgeClass: 'badge-active',
    desc: 'Container operations dashboard with dark terminal aesthetic. 20 Docker/orchestration-specific fault scenarios for chaos engineering demos.',
    tags: ['Docker','Python','Redis','Prometheus','Grafana'],
    stars: 55, forks: 19, issues: 7
  },
  {
    name: 'AVION-RHEL',
    badge: 'STABLE',
    badgeClass: 'badge-arch',
    desc: 'PHP/Apache/MariaDB deployment on RHEL 9 with SELinux hardening, Apache Alias routing, and full Datadog integration.',
    tags: ['RHEL 9','PHP','Apache','MariaDB','SELinux','Datadog'],
    stars: 22, forks: 5, issues: 0
  },
  {
    name: 'CHAOS-LAB',
    badge: 'NEW',
    badgeClass: 'badge-new',
    desc: 'Unified fault injection framework for multi-tier demo environments. Supports application, container, and observability tier chaos scenarios.',
    tags: ['Python','REST','Docker','Bash','YAML'],
    stars: 14, forks: 3, issues: 2
  },
  {
    name: 'OBS-TOOLKIT',
    badge: 'NEW',
    badgeClass: 'badge-new',
    desc: 'Curated Datadog agent configs, SNMP MIB templates, auto-discovery labels, and monitor templates for rapid observability bootstrapping.',
    tags: ['Datadog','SNMP','YAML','Python','MIB'],
    stars: 38, forks: 11, issues: 4
  },
];
const projGrid = document.getElementById('projects-grid');
projects.forEach(p => {
  const div = document.createElement('div');
  div.className = 'project-card';
  div.innerHTML = `
    <div class="project-header">
      <div class="project-name">${p.name}</div>
      <div class="project-badge ${p.badgeClass}">${p.badge}</div>
    </div>
    <p class="project-desc">${p.desc}</p>
    <div class="project-tags">${p.tags.map(t => `<span class="tag">${t}</span>`).join('')}</div>
    <div class="project-stats">
      <div class="proj-stat">★ <span>${p.stars}</span></div>
      <div class="proj-stat">⑂ <span>${p.forks}</span></div>
      <div class="proj-stat">◎ <span>${p.issues}</span> open</div>
    </div>
  `;
  projGrid.appendChild(div);
});

/* ====== ACTIVITY GRAPH ====== */
const graphEl = document.getElementById('activity-graph');
const levels = [0,0,0,0,1,1,2,2,2,3,3,4];
function randLevel() {
  const r = Math.random();
  if (r < 0.3) return 0;
  if (r < 0.55) return 1;
  if (r < 0.75) return 2;
  if (r < 0.9) return 3;
  return 4;
}
for (let w = 0; w < 52; w++) {
  const col = document.createElement('div');
  col.className = 'activity-col';
  for (let d = 0; d < 7; d++) {
    const cell = document.createElement('div');
    const l = randLevel();
    cell.className = 'activity-cell' + (l > 0 ? ` l${l}` : '');
    col.appendChild(cell);
  }
  graphEl.appendChild(col);
}

/* ====== TERMINAL ANIMATION ====== */
const termLines = [
  { type: 'cmd', content: '$ whoami', delay: 400 },
  { type: 'out', content: 'shayne → DevOps Engineer / Systems Architect', delay: 800 },
  { type: 'cmd', content: '$ docker ps --format "table {{.Names}}\\t{{.Status}}"', delay: 1400 },
  { type: 'out cyan', content: 'NAMES                   STATUS', delay: 1800 },
  { type: 'out', content: 'avion-nginx             Up 14 days (healthy)', delay: 2000 },
  { type: 'out', content: 'avion-fastapi           Up 14 days (healthy)', delay: 2100 },
  { type: 'out', content: 'avion-mysql             Up 14 days (healthy)', delay: 2200 },
  { type: 'out', content: 'avion-snmpd             Up 14 days (healthy)', delay: 2300 },
  { type: 'cmd', content: '$ datadog-agent status | grep -E "(Running|Checks)"', delay: 2800 },
  { type: 'out cyan', content: 'Agent (v7.58.0) · Running', delay: 3300 },
  { type: 'out', content: '  snmp (v2.21.0) · 17 OIDs polled · OK', delay: 3500 },
  { type: 'out', content: '  docker (v7.1.0) · 4 containers · OK', delay: 3700 },
  { type: 'cmd', content: '$ uptime && uname -r', delay: 4200 },
  { type: 'out', content: 'up 14 days, 6:22,  load: 0.08, 0.12, 0.11', delay: 4600 },
  { type: 'out', content: '6.6.15-linuxkit', delay: 4800 },
  { type: 'cmd', content: '$ echo "All systems nominal ✓"', delay: 5300 },
  { type: 'out', content: 'All systems nominal ✓', delay: 5700, special: 'green' },
];
const termBody = document.getElementById('terminal-body');
termLines.forEach(l => {
  setTimeout(() => {
    const span = document.createElement('span');
    span.className = 't-line';
    const isCmd = l.type === 'cmd';
    const cls = l.special === 'green' ? 'color:var(--green)' : '';
    span.innerHTML = isCmd
      ? `<span class="t-prompt">shayne@dev</span><span style="color:var(--text-dim)">:</span><span style="color:var(--cyan)">~</span><span class="t-cmd"> ${l.content.replace(/^\$ /,'$ ')}</span>`
      : `<span class="t-out ${l.type.split(' ')[1] || ''}" style="${cls}">${l.content}</span>`;
    termBody.appendChild(span);
    requestAnimationFrame(() => { span.style.opacity = '1'; });
  }, l.delay);
});

/* ====== UPTIME COUNTER ====== */
const start = Date.now();
setInterval(() => {
  const s = Math.floor((Date.now() - start) / 1000);
  const h = String(Math.floor(s/3600)).padStart(2,'0');
  const m = String(Math.floor((s%3600)/60)).padStart(2,'0');
  const sec = String(s%60).padStart(2,'0');
  document.getElementById('uptime-counter').textContent = `${h}:${m}:${sec}`;
}, 1000);

/* ====== INTERSECTION OBSERVER ====== */
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); } });
}, { threshold: 0.1 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

/* ====== CUSTOM CURSOR CROSSHAIR ====== */
let cursorRing = null;
(function() {
  cursorRing = document.createElement('div');
  Object.assign(cursorRing.style, {
    position: 'fixed', width: '32px', height: '32px',
    border: '1px solid rgba(0,245,255,0.6)',
    borderRadius: '50%', pointerEvents: 'none', zIndex: '9998',
    transform: 'translate(-50%,-50%)', transition: 'transform 0.15s ease',
    display: 'flex', alignItems: 'center', justifyContent: 'center',
    left: '-100px', top: '-100px'
  });
  document.body.appendChild(cursorRing);
  document.addEventListener('mousemove', e => {
    cursorRing.style.left = e.clientX + 'px';
    cursorRing.style.top = e.clientY + 'px';
  });
  document.addEventListener('mousedown', () => { cursorRing.style.transform = 'translate(-50%,-50%) scale(0.7)'; });
  document.addEventListener('mouseup', () => { cursorRing.style.transform = 'translate(-50%,-50%) scale(1)'; });
})();
</script>
</body>
</html>
