<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Zakariya Al Shuaibi — Dev Profile</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&display=swap');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  :root {
    --bg: #050a14;
    --surface: #0a1628;
    --accent1: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #f97316;
    --text: #e2e8f0;
    --muted: #64748b;
    --glow1: rgba(0,212,255,0.15);
    --glow2: rgba(124,58,237,0.15);
  }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Starfield */
  #stars-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    pointer-events: none;
  }

  /* 3D Scene */
  #three-canvas {
    position: fixed;
    top: 0; right: 0;
    width: 45%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
  }

  .content {
    position: relative;
    z-index: 2;
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 40px;
  }

  /* NAV */
  nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 28px 0 0;
  }
  .nav-logo {
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    color: var(--accent1);
    letter-spacing: 3px;
    text-transform: uppercase;
  }
  .nav-dot {
    width: 8px; height: 8px;
    background: var(--accent1);
    border-radius: 50%;
    box-shadow: 0 0 12px var(--accent1);
    animation: pulse 2s ease-in-out infinite;
  }
  @keyframes pulse {
    0%,100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(0.7); }
  }

  /* HERO */
  .hero {
    padding-top: 80px;
    width: 55%;
  }

  .hero-eyebrow {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--accent1);
    text-transform: uppercase;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 10px;
    opacity: 0;
    animation: fadeUp 0.6s 0.2s ease forwards;
  }
  .hero-eyebrow::before {
    content: '';
    display: block;
    width: 32px; height: 1px;
    background: var(--accent1);
  }

  .hero-name {
    font-size: clamp(38px, 5vw, 64px);
    font-weight: 800;
    line-height: 1.05;
    letter-spacing: -1px;
    margin-bottom: 12px;
    opacity: 0;
    animation: fadeUp 0.6s 0.35s ease forwards;
  }
  .hero-name .highlight {
    background: linear-gradient(135deg, var(--accent1), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-title {
    font-size: 15px;
    font-weight: 600;
    color: var(--muted);
    letter-spacing: 1px;
    margin-bottom: 28px;
    opacity: 0;
    animation: fadeUp 0.6s 0.5s ease forwards;
  }

  .typing-line {
    font-family: 'Space Mono', monospace;
    font-size: 13px;
    color: var(--accent3);
    margin-bottom: 36px;
    opacity: 0;
    animation: fadeUp 0.6s 0.65s ease forwards;
    min-height: 20px;
  }
  .cursor { display: inline-block; animation: blink 1s step-end infinite; }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }

  /* ABOUT CARD */
  .about-card {
    background: rgba(10,22,40,0.7);
    border: 1px solid rgba(0,212,255,0.12);
    border-radius: 16px;
    padding: 24px 28px;
    margin-bottom: 32px;
    backdrop-filter: blur(12px);
    opacity: 0;
    animation: fadeUp 0.6s 0.8s ease forwards;
    position: relative;
    overflow: hidden;
  }
  .about-card::before {
    content: '';
    position: absolute;
    top: -1px; left: 20px; right: 20px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent1), transparent);
  }
  .about-card p {
    font-size: 14px;
    line-height: 1.8;
    color: #94a3b8;
  }
  .about-card p span { color: var(--text); font-weight: 600; }

  /* STATUS BADGES */
  .status-row {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.6s 0.95s ease forwards;
  }
  .badge {
    display: flex;
    align-items: center;
    gap: 7px;
    padding: 7px 14px;
    border-radius: 30px;
    font-size: 12px;
    font-weight: 600;
    border: 1px solid;
    letter-spacing: 0.5px;
    white-space: nowrap;
  }
  .badge.working { background: rgba(0,212,255,0.07); border-color: rgba(0,212,255,0.3); color: var(--accent1); }
  .badge.learning { background: rgba(124,58,237,0.07); border-color: rgba(124,58,237,0.3); color: #a78bfa; }
  .badge.past { background: rgba(249,115,22,0.07); border-color: rgba(249,115,22,0.3); color: var(--accent3); }
  .badge-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }
  .badge.working .badge-dot { animation: pulse 1.5s ease-in-out infinite; }

  /* SKILLS GRID */
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--muted);
    text-transform: uppercase;
    margin-bottom: 14px;
  }

  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
    gap: 10px;
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.6s 1.1s ease forwards;
  }
  .skill-chip {
    background: rgba(10,22,40,0.8);
    border: 1px solid rgba(255,255,255,0.06);
    border-radius: 10px;
    padding: 10px 8px;
    text-align: center;
    font-size: 11px;
    font-weight: 600;
    color: var(--muted);
    cursor: default;
    transition: all 0.25s ease;
  }
  .skill-chip:hover {
    border-color: var(--accent1);
    color: var(--accent1);
    background: rgba(0,212,255,0.06);
    transform: translateY(-3px);
    box-shadow: 0 8px 20px rgba(0,212,255,0.1);
  }
  .skill-chip .icon { font-size: 18px; display: block; margin-bottom: 4px; }

  /* SOCIAL LINKS */
  .social-row {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    opacity: 0;
    animation: fadeUp 0.6s 1.25s ease forwards;
    margin-bottom: 60px;
  }
  .social-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 11px 20px;
    border-radius: 10px;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.5px;
    text-decoration: none;
    cursor: pointer;
    transition: all 0.25s ease;
    border: 1px solid;
  }
  .social-btn.linkedin { background: rgba(0,119,181,0.1); border-color: rgba(0,119,181,0.4); color: #60a5fa; }
  .social-btn.instagram { background: rgba(228,64,95,0.1); border-color: rgba(228,64,95,0.4); color: #f472b6; }
  .social-btn.whatsapp { background: rgba(37,211,102,0.1); border-color: rgba(37,211,102,0.4); color: #4ade80; }
  .social-btn:hover { transform: translateY(-3px); filter: brightness(1.3); box-shadow: 0 8px 24px rgba(0,0,0,0.3); }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* Decorative grid line */
  .grid-line {
    position: fixed;
    top: 0; bottom: 0;
    left: 55%;
    width: 1px;
    background: linear-gradient(to bottom, transparent, rgba(0,212,255,0.15) 40%, rgba(124,58,237,0.15) 60%, transparent);
    z-index: 1;
    pointer-events: none;
  }

  /* Profile views badge */
  .views-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    padding: 4px 10px;
    border: 1px solid rgba(255,255,255,0.06);
    border-radius: 20px;
    margin-bottom: 14px;
    letter-spacing: 1px;
  }
  .views-badge span { color: var(--accent1); font-weight: 700; }
</style>
</head>
<body>

<canvas id="stars-canvas"></canvas>
<canvas id="three-canvas"></canvas>
<div class="grid-line"></div>

<div class="content">
  <nav>
    <div class="nav-logo">ZAS.DEV</div>
    <div class="nav-dot"></div>
  </nav>

  <section class="hero">
    <div class="hero-eyebrow">Full-Stack &amp; Mobile Developer</div>

    <h1 class="hero-name">
      Zakariya<br><span class="highlight">Al Shuaibi</span>
    </h1>

    <div class="hero-title">Building Smart Platforms &amp; Smooth Interfaces</div>

    <div class="typing-line" id="typing-el"><span class="cursor">▋</span></div>

    <div class="about-card">
      <p>
        🚀 Passionate about crafting <span>innovative web applications</span> that blend design and functionality.<br>
        🌍 Focused on delivering <span>real-world impact</span> through elegant and scalable tech solutions.
      </p>
    </div>

    <div class="status-row">
      <div class="badge working"><div class="badge-dot"></div>Working on AFAQ</div>
      <div class="badge learning"><div class="badge-dot"></div>React.js &amp; TypeScript</div>
      <div class="badge learning"><div class="badge-dot"></div>AI / ML Integration</div>
      <div class="badge past"><div class="badge-dot"></div>Flutter Alumni</div>
      <div class="badge past"><div class="badge-dot"></div>Firebase Expert</div>
    </div>

    <div class="section-label">Tech Stack</div>
    <div class="skills-grid">
      <div class="skill-chip"><span class="icon">⚛️</span>React</div>
      <div class="skill-chip"><span class="icon">🟦</span>TypeScript</div>
      <div class="skill-chip"><span class="icon">🌿</span>Node.js</div>
      <div class="skill-chip"><span class="icon">🐦</span>Flutter</div>
      <div class="skill-chip"><span class="icon">🔥</span>Firebase</div>
      <div class="skill-chip"><span class="icon">🤖</span>AI/ML</div>
      <div class="skill-chip"><span class="icon">🎨</span>UI/UX</div>
      <div class="skill-chip"><span class="icon">🗄️</span>SQL</div>
    </div>

    <div class="views-badge">👁 Profile Views <span>1.2K+</span></div>

    <div class="section-label">Connect</div>
    <div class="social-row">
      <a class="social-btn linkedin" href="https://www.linkedin.com/in/zakariya-al-shuaibi-6a2a1a28b" target="_blank">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
        LinkedIn
      </a>
      <a class="social-btn instagram" href="https://www.instagram.com/z370.z" target="_blank">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
        Instagram
      </a>
      <a class="social-btn whatsapp" href="https://wa.me/96895616133" target="_blank">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 00-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
        WhatsApp
      </a>
    </div>
  </section>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ── STARS ──────────────────────────────────────────────────────────────
const sc = document.getElementById('stars-canvas');
const sx = sc.getContext('2d');
function resizeStars() { sc.width = window.innerWidth; sc.height = window.innerHeight; }
resizeStars();
window.addEventListener('resize', resizeStars);

const stars = Array.from({length: 200}, () => ({
  x: Math.random(), y: Math.random(),
  r: Math.random() * 1.4 + 0.2,
  a: Math.random(),
  speed: Math.random() * 0.003 + 0.001
}));

function drawStars() {
  sx.clearRect(0, 0, sc.width, sc.height);
  stars.forEach(s => {
    s.a += s.speed;
    const alpha = (Math.sin(s.a) + 1) / 2 * 0.7 + 0.1;
    sx.beginPath();
    sx.arc(s.x * sc.width, s.y * sc.height, s.r, 0, Math.PI * 2);
    sx.fillStyle = `rgba(150,200,255,${alpha})`;
    sx.fill();
  });
  requestAnimationFrame(drawStars);
}
drawStars();

// ── THREE.JS 3D SCENE ──────────────────────────────────────────────────
const canvas3d = document.getElementById('three-canvas');
const renderer = new THREE.WebGLRenderer({ canvas: canvas3d, antialias: true, alpha: true });
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(canvas3d.clientWidth || window.innerWidth * 0.45, window.innerHeight);
renderer.setClearColor(0x000000, 0);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(50, (window.innerWidth * 0.45) / window.innerHeight, 0.1, 100);
camera.position.set(0, 0, 6);

function resize3d() {
  const w = window.innerWidth * 0.45;
  const h = window.innerHeight;
  renderer.setSize(w, h);
  camera.aspect = w / h;
  camera.updateProjectionMatrix();
}
window.addEventListener('resize', resize3d);

// Lighting
scene.add(new THREE.AmbientLight(0x112244, 2));
const dLight = new THREE.DirectionalLight(0x00d4ff, 3);
dLight.position.set(5, 5, 5);
scene.add(dLight);
const pLight1 = new THREE.PointLight(0x7c3aed, 4, 20);
pLight1.position.set(-3, 2, 3);
scene.add(pLight1);
const pLight2 = new THREE.PointLight(0xf97316, 3, 15);
pLight2.position.set(3, -2, 2);
scene.add(pLight2);

// Central icosahedron
const geoCore = new THREE.IcosahedronGeometry(1.2, 1);
const matCore = new THREE.MeshStandardMaterial({
  color: 0x0a1628,
  emissive: 0x001133,
  metalness: 0.95,
  roughness: 0.1,
});
const meshCore = new THREE.Mesh(geoCore, matCore);
scene.add(meshCore);

// Wireframe overlay
const geoWire = new THREE.IcosahedronGeometry(1.22, 1);
const matWire = new THREE.MeshBasicMaterial({ color: 0x00d4ff, wireframe: true, transparent: true, opacity: 0.25 });
const meshWire = new THREE.Mesh(geoWire, matWire);
scene.add(meshWire);

// Rings
const ring1 = new THREE.Mesh(
  new THREE.TorusGeometry(2.2, 0.015, 8, 80),
  new THREE.MeshBasicMaterial({ color: 0x00d4ff, transparent: true, opacity: 0.5 })
);
ring1.rotation.x = Math.PI / 3;
scene.add(ring1);

const ring2 = new THREE.Mesh(
  new THREE.TorusGeometry(2.8, 0.01, 8, 100),
  new THREE.MeshBasicMaterial({ color: 0x7c3aed, transparent: true, opacity: 0.4 })
);
ring2.rotation.x = Math.PI / 5;
ring2.rotation.z = Math.PI / 4;
scene.add(ring2);

// Orbiting tech nodes
const nodeData = [
  { color: 0x61dafb, r: 2.2, speed: 0.6, phase: 0 },
  { color: 0x3178c6, r: 2.5, speed: 0.4, phase: 2.1 },
  { color: 0x4fc3f7, r: 2.0, speed: 0.8, phase: 1.0 },
  { color: 0x54c5f8, r: 2.7, speed: 0.35, phase: 3.5 },
  { color: 0xff9800, r: 1.9, speed: 0.9, phase: 0.6 },
  { color: 0xa78bfa, r: 2.4, speed: 0.5, phase: 4.2 },
];

const nodes = nodeData.map((d, i) => {
  const mesh = new THREE.Mesh(
    new THREE.SphereGeometry(0.12, 16, 16),
    new THREE.MeshBasicMaterial({ color: d.color })
  );
  scene.add(mesh);
  const gMesh = new THREE.Mesh(
    new THREE.SphereGeometry(0.22, 16, 16),
    new THREE.MeshBasicMaterial({ color: d.color, transparent: true, opacity: 0.12 })
  );
  mesh.add(gMesh);
  const axis = new THREE.Vector3(Math.sin(i * 1.3), Math.cos(i * 0.9), Math.sin(i * 0.5)).normalize();
  return { mesh, ...d, axis, t: d.phase };
});

// Floating particles
const partCount = 80;
const partPositions = new Float32Array(partCount * 3);
for (let i = 0; i < partCount; i++) {
  const r = 3.5 + Math.random() * 1.5;
  const theta = Math.random() * Math.PI * 2;
  const phi = Math.acos(2 * Math.random() - 1);
  partPositions[i*3]   = r * Math.sin(phi) * Math.cos(theta);
  partPositions[i*3+1] = r * Math.sin(phi) * Math.sin(theta);
  partPositions[i*3+2] = r * Math.cos(phi);
}
const partGeo = new THREE.BufferGeometry();
partGeo.setAttribute('position', new THREE.BufferAttribute(partPositions, 3));
const particles = new THREE.Points(partGeo, new THREE.PointsMaterial({ color: 0x00d4ff, size: 0.04, transparent: true, opacity: 0.6 }));
scene.add(particles);

// Mouse parallax
let mx = 0, my = 0;
window.addEventListener('mousemove', e => {
  mx = (e.clientX / window.innerWidth - 0.5) * 2;
  my = (e.clientY / window.innerHeight - 0.5) * 2;
});

let t = 0;
function animate() {
  requestAnimationFrame(animate);
  t += 0.008;

  meshCore.rotation.x = t * 0.3;
  meshCore.rotation.y = t * 0.5;
  meshWire.rotation.x = -t * 0.2;
  meshWire.rotation.y = -t * 0.4;
  ring1.rotation.z = t * 0.4;
  ring2.rotation.y = t * 0.3;
  ring2.rotation.x += 0.003;

  scene.position.y = Math.sin(t * 0.7) * 0.15;
  scene.position.x = Math.cos(t * 0.5) * 0.08;

  camera.position.x += (mx * 0.8 - camera.position.x) * 0.04;
  camera.position.y += (-my * 0.5 - camera.position.y) * 0.04;
  camera.lookAt(scene.position);

  nodes.forEach(n => {
    n.t += 0.01 * n.speed;
    const q = new THREE.Quaternion().setFromAxisAngle(n.axis, n.t);
    n.mesh.position.copy(new THREE.Vector3(n.r, 0, 0).applyQuaternion(q));
    n.mesh.children[0].scale.setScalar(1 + Math.sin(t * 3 + n.phase) * 0.3);
  });

  particles.rotation.y = t * 0.05;
  pLight1.position.x = Math.sin(t * 0.7) * 4;
  pLight1.position.y = Math.cos(t * 0.5) * 3;
  pLight2.position.x = Math.cos(t * 0.6) * 4;
  pLight2.position.y = Math.sin(t * 0.8) * 3;

  renderer.render(scene, camera);
}
animate();

// ── TYPING ANIMATION ──────────────────────────────────────────────────
const lines = [
  '> Building AFAQ platforms...',
  '> Mastering React + TypeScript',
  '> Integrating AI into UX',
  '> Flutter × Firebase veteran',
  '> Crafting seamless interfaces'
];
let li = 0, ci = 0, deleting = false;
const el = document.getElementById('typing-el');

function typeLoop() {
  const line = lines[li];
  if (!deleting) {
    ci++;
    el.innerHTML = line.slice(0, ci) + '<span class="cursor">▋</span>';
    if (ci === line.length) { deleting = true; setTimeout(typeLoop, 1800); return; }
    setTimeout(typeLoop, 55);
  } else {
    ci--;
    el.innerHTML = line.slice(0, ci) + '<span class="cursor">▋</span>';
    if (ci === 0) { deleting = false; li = (li + 1) % lines.length; setTimeout(typeLoop, 300); return; }
    setTimeout(typeLoop, 30);
  }
}
typeLoop();
</script>
</body>
</html>

---

### ⚙️ Tech Stack

#### 💻 Languages
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/dart/dart-original.svg" width="40" />
</p>

#### 🌐 Frontend
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/redux/redux-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/bootstrap/bootstrap-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tailwindcss/tailwindcss-plain.svg" width="40" />
</p>

#### 🖥 Backend
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/express/express-original.svg" width="40" />
</p>

#### 📱 Mobile
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/flutter/flutter-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" width="40" />
</p>

#### 🤖 AI / ML
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tensorflow/tensorflow-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/pytorch/pytorch-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/pandas/pandas-original.svg" width="40" />
  <img src="https://seaborn.pydata.org/_images/logo-mark-lightbg.svg" width="40" />
</p>

#### 🗄 Databases
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/firebase/firebase-plain.svg" width="40" />
</p>

#### 🛠 Tools
<p>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postman/postman-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/figma/figma-original.svg" width="40" />
</p>

---

### 🧩 Specialties

- RESTful API Development  
- Firebase Auth & Firestore  
- UI/UX Prototyping  
- Agile Workflows & Clean Code  

---

### ⚡ Fun Fact

> I build things that make people say “Wow!” before they even click a button.

---

