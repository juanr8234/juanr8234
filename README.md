<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Juan Ramírez | Full Stack Developer</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --cyan: #00F5FF;
  --magenta: #FF00AA;
  --green: #00FF88;
  --violet: #7B2FBE;
  --violet-bright: #BF5FFF;
  --bg: #020408;
  --bg2: #050D15;
  --bg3: #0A1628;
  --glass: rgba(0,245,255,0.04);
  --glass2: rgba(255,0,170,0.04);
  --border: rgba(0,245,255,0.15);
  --border-m: rgba(255,0,170,0.15);
  --text: #C8E6FF;
  --text-dim: rgba(200,230,255,0.5);
  --font-mono: 'Share Tech Mono', monospace;
  --font-head: 'Orbitron', monospace;
  --font-body: 'Rajdhani', sans-serif;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  font-size: 16px;
  overflow-x: hidden;
  cursor: none;
}

/* ─── CUSTOM CURSOR ─── */
#cursor {
  position: fixed; width: 14px; height: 14px;
  background: var(--cyan);
  border-radius: 50%;
  pointer-events: none; z-index: 9999;
  transform: translate(-50%,-50%);
  box-shadow: 0 0 12px var(--cyan), 0 0 30px rgba(0,245,255,0.4);
  transition: transform 0.05s, width 0.2s, height 0.2s;
  mix-blend-mode: screen;
}
#cursor-ring {
  position: fixed; width: 38px; height: 38px;
  border: 1px solid rgba(0,245,255,0.5);
  border-radius: 50%;
  pointer-events: none; z-index: 9998;
  transform: translate(-50%,-50%);
  transition: transform 0.12s ease, width 0.3s, height 0.3s, border-color 0.3s;
}
body:has(a:hover) #cursor, body:has(button:hover) #cursor { width: 22px; height: 22px; }
body:has(a:hover) #cursor-ring, body:has(button:hover) #cursor-ring { width: 54px; height: 54px; border-color: rgba(255,0,170,0.6); }

/* ─── SCROLLBAR ─── */
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--cyan); border-radius: 2px; }

/* ─── CANVAS BACKGROUNDS ─── */
#canvas-bg { position: fixed; top:0; left:0; width:100%; height:100%; z-index:0; pointer-events:none; }
#snake-canvas { position: fixed; top:0; left:0; width:100%; height:100%; z-index:1; pointer-events:none; opacity:0.35; }

/* ─── SCAN LINE OVERLAY ─── */
.scanlines {
  position: fixed; top:0; left:0; width:100%; height:100%;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.08) 2px, rgba(0,0,0,0.08) 4px);
  pointer-events: none; z-index: 2;
}

/* ─── LAYOUT ─── */
.content { position: relative; z-index: 10; }

/* ─── NAV ─── */
nav {
  position: fixed; top:0; left:0; right:0; z-index: 1000;
  display: flex; align-items: center; justify-content: space-between;
  padding: 18px 48px;
  background: rgba(2,4,8,0.85);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border);
}
nav::after {
  content: '';
  position: absolute; bottom:-1px; left:0; right:0; height:1px;
  background: linear-gradient(90deg, transparent, var(--cyan), var(--magenta), transparent);
  animation: navLine 3s linear infinite;
}
@keyframes navLine {
  0%{background-position:-200% 0} 100%{background-position:200% 0}
}
.nav-logo {
  font-family: var(--font-head);
  font-size: 18px; font-weight: 900;
  color: var(--cyan);
  text-shadow: 0 0 20px var(--cyan);
  letter-spacing: 3px;
}
.nav-logo span { color: var(--magenta); text-shadow: 0 0 20px var(--magenta); }
.nav-links { display: flex; gap: 36px; }
.nav-links a {
  font-family: var(--font-mono); font-size: 12px; color: var(--text-dim);
  text-decoration: none; letter-spacing: 2px; text-transform: uppercase;
  transition: color 0.2s, text-shadow 0.2s;
  position: relative;
}
.nav-links a::before { content: '> '; color: var(--cyan); opacity: 0; transition: opacity 0.2s; }
.nav-links a:hover { color: var(--cyan); text-shadow: 0 0 10px var(--cyan); }
.nav-links a:hover::before { opacity: 1; }
.nav-status {
  font-family: var(--font-mono); font-size: 11px; color: var(--green);
  display: flex; align-items: center; gap: 8px;
}
.status-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--green);
  box-shadow: 0 0 10px var(--green);
  animation: pulse-green 2s infinite;
}
@keyframes pulse-green { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:0.6;transform:scale(0.8)} }

/* ─── HERO ─── */
#hero {
  min-height: 100vh;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  text-align: center;
  padding: 120px 24px 80px;
  position: relative;
  overflow: hidden;
}
.hero-grid {
  position: absolute; inset: 0;
  background-image:
    linear-gradient(rgba(0,245,255,0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,245,255,0.04) 1px, transparent 1px);
  background-size: 60px 60px;
  animation: gridShift 20s linear infinite;
}
@keyframes gridShift { 0%{transform:translateY(0)} 100%{transform:translateY(60px)} }
.hero-vignette {
  position: absolute; inset: 0;
  background: radial-gradient(ellipse 80% 60% at 50% 50%, transparent 30%, var(--bg) 100%);
}
.hero-coords {
  position: absolute; top: 100px; left: 48px;
  font-family: var(--font-mono); font-size: 10px; color: rgba(0,245,255,0.3);
  line-height: 1.8;
}
.hero-sys {
  position: absolute; top: 100px; right: 48px;
  font-family: var(--font-mono); font-size: 10px; color: rgba(0,245,255,0.3);
  text-align: right; line-height: 1.8;
}
.hero-tag {
  font-family: var(--font-mono); font-size: 11px;
  color: var(--magenta); letter-spacing: 4px;
  margin-bottom: 20px;
  opacity: 0; animation: fadeUp 0.8s 0.2s forwards;
}
.hero-name {
  font-family: var(--font-head);
  font-size: clamp(48px, 10vw, 108px);
  font-weight: 900;
  line-height: 0.9;
  letter-spacing: -2px;
  margin-bottom: 16px;
  opacity: 0; animation: fadeUp 0.8s 0.4s forwards;
}
.hero-name .first { color: var(--text); }
.hero-name .last {
  color: var(--cyan);
  text-shadow: 0 0 30px var(--cyan), 0 0 60px rgba(0,245,255,0.3);
  display: block;
}
.hero-roles {
  font-family: var(--font-mono); font-size: clamp(14px, 2vw, 18px);
  color: var(--text-dim); margin-bottom: 12px;
  min-height: 28px;
  opacity: 0; animation: fadeUp 0.8s 0.6s forwards;
}
.typed-text { color: var(--cyan); }
.typed-cursor {
  display: inline-block; width: 2px; height: 1.1em;
  background: var(--cyan); vertical-align: middle;
  margin-left: 2px;
  box-shadow: 0 0 8px var(--cyan);
  animation: blink 0.8s infinite;
}
@keyframes blink { 0%,50%{opacity:1} 51%,100%{opacity:0} }
.hero-tagline {
  font-family: var(--font-body); font-size: 13px; font-weight: 300;
  color: var(--text-dim); letter-spacing: 3px; text-transform: uppercase;
  margin-bottom: 48px;
  opacity: 0; animation: fadeUp 0.8s 0.8s forwards;
}
.hero-btns {
  display: flex; flex-wrap: wrap; gap: 16px; justify-content: center;
  opacity: 0; animation: fadeUp 0.8s 1s forwards;
}
.btn {
  font-family: var(--font-mono); font-size: 12px;
  letter-spacing: 2px; text-transform: uppercase;
  padding: 14px 32px; border: none; cursor: none;
  text-decoration: none; display: inline-flex; align-items: center; gap: 8px;
  position: relative; overflow: hidden;
  transition: transform 0.2s;
}
.btn:hover { transform: translateY(-2px); }
.btn-primary {
  background: transparent;
  border: 1px solid var(--cyan);
  color: var(--cyan);
  text-shadow: 0 0 8px var(--cyan);
  box-shadow: 0 0 20px rgba(0,245,255,0.15), inset 0 0 20px rgba(0,245,255,0.05);
}
.btn-primary::before {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(135deg, transparent 40%, rgba(0,245,255,0.1));
  opacity: 0; transition: opacity 0.3s;
}
.btn-primary:hover { box-shadow: 0 0 40px rgba(0,245,255,0.4), inset 0 0 30px rgba(0,245,255,0.1); }
.btn-primary:hover::before { opacity: 1; }
.btn-secondary {
  background: transparent;
  border: 1px solid var(--magenta);
  color: var(--magenta);
  text-shadow: 0 0 8px var(--magenta);
  box-shadow: 0 0 20px rgba(255,0,170,0.15), inset 0 0 20px rgba(255,0,170,0.05);
}
.btn-secondary:hover { box-shadow: 0 0 40px rgba(255,0,170,0.4), inset 0 0 30px rgba(255,0,170,0.1); }
.btn-ghost {
  background: transparent;
  border: 1px solid rgba(200,230,255,0.2);
  color: var(--text-dim);
}
.btn-ghost:hover { border-color: var(--green); color: var(--green); box-shadow: 0 0 20px rgba(0,255,136,0.2); }
.hero-scroll {
  position: absolute; bottom: 40px; left: 50%;
  transform: translateX(-50%);
  font-family: var(--font-mono); font-size: 10px; color: var(--text-dim);
  letter-spacing: 3px; text-align: center;
  animation: scrollPulse 2s infinite;
}
.hero-scroll::after {
  content: ''; display: block; width: 1px; height: 40px;
  background: linear-gradient(var(--cyan), transparent);
  margin: 8px auto 0;
  animation: scrollLine 2s infinite;
}
@keyframes scrollPulse { 0%,100%{opacity:0.4} 50%{opacity:0.9} }
@keyframes scrollLine { 0%{transform:scaleY(0);transform-origin:top} 50%{transform:scaleY(1)} 100%{transform:scaleY(0);transform-origin:bottom} }

/* ─── SECTION COMMONS ─── */
section { padding: 100px 48px; position: relative; }
@media(max-width:768px){section{padding:80px 24px}}
.section-label {
  font-family: var(--font-mono); font-size: 11px;
  color: var(--magenta); letter-spacing: 4px; text-transform: uppercase;
  margin-bottom: 12px;
}
.section-title {
  font-family: var(--font-head); font-size: clamp(28px,4vw,48px);
  font-weight: 900; color: var(--text);
  margin-bottom: 60px;
  position: relative;
}
.section-title span { color: var(--cyan); text-shadow: 0 0 20px var(--cyan); }
.section-title::after {
  content: ''; display: block; height: 2px; width: 80px; margin-top: 16px;
  background: linear-gradient(90deg, var(--cyan), var(--magenta), transparent);
}
.container { max-width: 1200px; margin: 0 auto; }

/* ─── TERMINAL LOG ─── */
.terminal-log {
  font-family: var(--font-mono); font-size: 11px;
  color: rgba(0,245,255,0.35); line-height: 2;
  position: absolute; right: 48px; top: 120px;
  text-align: right;
  pointer-events: none;
}
.log-line { display: flex; justify-content: flex-end; gap: 12px; }
.log-ok { color: var(--green); }
.log-warn { color: #FFAA00; }
.log-err { color: var(--magenta); }

@keyframes fadeUp { from{opacity:0;transform:translateY(24px)} to{opacity:1;transform:translateY(0)} }

/* ─── ABOUT ─── */
#about { background: var(--bg2); }
.about-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 64px; align-items: center; }
@media(max-width:768px){.about-grid{grid-template-columns:1fr}}
.about-avatar-wrap {
  display: flex; justify-content: center; align-items: center;
}
.avatar-hex {
  width: 260px; height: 260px;
  position: relative;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  background: linear-gradient(135deg, var(--bg3), #0D1F3C);
  display: flex; align-items: center; justify-content: center;
  border: 2px solid var(--cyan);
  box-shadow: 0 0 40px rgba(0,245,255,0.2), inset 0 0 40px rgba(0,245,255,0.05);
}
.avatar-inner {
  font-family: var(--font-head); font-size: 72px; font-weight: 900;
  background: linear-gradient(135deg, var(--cyan), var(--magenta));
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 0 20px var(--cyan));
}
.avatar-ring {
  position: absolute; inset: -12px;
  border: 1px solid rgba(0,245,255,0.2);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  animation: rotateRing 8s linear infinite;
}
@keyframes rotateRing { 0%{filter:hue-rotate(0)} 100%{filter:hue-rotate(360deg)} }
.about-text p {
  font-size: 16px; line-height: 1.9; color: var(--text-dim);
  margin-bottom: 20px; font-weight: 300;
}
.about-text p strong { color: var(--cyan); font-weight: 600; }
.about-stats {
  display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 32px;
}
.stat-card {
  background: var(--glass);
  border: 1px solid var(--border);
  padding: 16px; text-align: center;
}
.stat-num {
  font-family: var(--font-head); font-size: 28px; font-weight: 900;
  color: var(--cyan); text-shadow: 0 0 15px var(--cyan);
  display: block;
}
.stat-label { font-family: var(--font-mono); font-size: 10px; color: var(--text-dim); letter-spacing: 2px; }

/* ─── TECH STACK ─── */
#tech { background: var(--bg); }
.tech-categories { display: flex; flex-direction: column; gap: 48px; }
.tech-cat-label {
  font-family: var(--font-mono); font-size: 10px; letter-spacing: 3px;
  color: var(--text-dim); margin-bottom: 20px; display: flex; align-items: center; gap: 12px;
}
.tech-cat-label::before { content: '//'; color: var(--cyan); }
.tech-cat-label::after { content: ''; flex: 1; height: 1px; background: var(--border); }
.tech-grid { display: flex; flex-wrap: wrap; gap: 16px; }
.tech-card {
  background: var(--glass);
  border: 1px solid var(--border);
  padding: 16px 20px;
  display: flex; align-items: center; gap: 12px;
  cursor: none;
  transition: all 0.3s;
  position: relative; overflow: hidden;
  min-width: 140px;
}
.tech-card::before {
  content: ''; position: absolute;
  inset: 0; opacity: 0;
  background: linear-gradient(135deg, rgba(0,245,255,0.08), rgba(255,0,170,0.04));
  transition: opacity 0.3s;
}
.tech-card:hover { border-color: var(--cyan); transform: translateY(-3px); box-shadow: 0 8px 30px rgba(0,245,255,0.15); }
.tech-card:hover::before { opacity: 1; }
.tech-icon {
  width: 32px; height: 32px; display: flex; align-items: center; justify-content: center;
  font-size: 22px;
  filter: drop-shadow(0 0 6px currentColor);
}
.tech-name {
  font-family: var(--font-mono); font-size: 12px; color: var(--text);
  letter-spacing: 1px; white-space: nowrap;
}
.tech-card:hover .tech-name { color: var(--cyan); }

/* ─── PROJECTS ─── */
#projects { background: var(--bg2); }
.projects-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(340px, 1fr)); gap: 24px; }
.project-card {
  background: var(--glass);
  border: 1px solid var(--border);
  padding: 28px;
  position: relative; overflow: hidden;
  transition: all 0.3s; cursor: none;
}
.project-card::before {
  content: ''; position: absolute; top: 0; left: 0; right: 0; height: 2px;
  background: linear-gradient(90deg, var(--cyan), var(--magenta));
  opacity: 0; transition: opacity 0.3s;
}
.project-card:hover { border-color: rgba(0,245,255,0.3); transform: translateY(-4px); box-shadow: 0 12px 40px rgba(0,245,255,0.1); }
.project-card:hover::before { opacity: 1; }
.project-num {
  font-family: var(--font-mono); font-size: 10px;
  color: var(--magenta); letter-spacing: 2px; margin-bottom: 12px;
}
.project-name {
  font-family: var(--font-head); font-size: 18px; font-weight: 700;
  color: var(--text); margin-bottom: 12px;
  transition: color 0.3s;
}
.project-card:hover .project-name { color: var(--cyan); text-shadow: 0 0 10px var(--cyan); }
.project-desc { font-size: 14px; color: var(--text-dim); line-height: 1.7; margin-bottom: 20px; }
.project-tags { display: flex; flex-wrap: wrap; gap: 8px; }
.tag {
  font-family: var(--font-mono); font-size: 10px; letter-spacing: 1px;
  padding: 4px 10px;
  border: 1px solid rgba(0,245,255,0.2);
  color: var(--cyan);
  background: rgba(0,245,255,0.05);
}
.tag.m { border-color: rgba(255,0,170,0.2); color: var(--magenta); background: rgba(255,0,170,0.05); }
.tag.g { border-color: rgba(0,255,136,0.2); color: var(--green); background: rgba(0,255,136,0.05); }
.project-status {
  position: absolute; top: 20px; right: 20px;
  font-family: var(--font-mono); font-size: 9px; letter-spacing: 2px;
  padding: 4px 8px; border: 1px solid var(--green);
  color: var(--green); background: rgba(0,255,136,0.06);
}

/* ─── HUD BAR ─── */
.hud-bar {
  background: rgba(5,13,21,0.9);
  border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
  padding: 16px 48px;
  font-family: var(--font-mono); font-size: 11px; color: var(--text-dim);
  display: flex; align-items: center; justify-content: space-between;
  overflow: hidden; position: relative;
}
.hud-bar::before {
  content: '';
  position: absolute; top: 0; left: -100%;
  width: 60%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(0,245,255,0.03), transparent);
  animation: hudScan 4s linear infinite;
}
@keyframes hudScan { to { left: 200%; } }
.hud-item { display: flex; align-items: center; gap: 8px; }
.hud-item .dot { width: 6px; height: 6px; border-radius: 50%; background: var(--cyan); box-shadow: 0 0 6px var(--cyan); }
.loading-bar { width: 120px; height: 3px; background: rgba(0,245,255,0.1); position: relative; overflow: hidden; }
.loading-bar-fill { height: 100%; background: var(--cyan); box-shadow: 0 0 6px var(--cyan); animation: loadFill 3s ease-in-out infinite; }
@keyframes loadFill { 0%{width:0%} 70%{width:100%} 100%{width:100%} }

/* ─── FOOTER ─── */
footer {
  background: var(--bg);
  border-top: 1px solid var(--border);
  padding: 64px 48px;
  text-align: center;
  position: relative; overflow: hidden;
}
footer::before {
  content: '';
  position: absolute; top: 0; left: 50%; transform: translateX(-50%);
  width: 600px; height: 200px;
  background: radial-gradient(ellipse at top, rgba(0,245,255,0.06), transparent 70%);
  pointer-events: none;
}
.footer-name {
  font-family: var(--font-head); font-size: 36px; font-weight: 900;
  color: var(--cyan); text-shadow: 0 0 30px var(--cyan);
  margin-bottom: 8px;
}
.footer-tagline {
  font-family: var(--font-mono); font-size: 13px; color: var(--text-dim);
  letter-spacing: 4px; margin-bottom: 40px;
}
.social-links { display: flex; justify-content: center; gap: 20px; margin-bottom: 48px; }
.social-link {
  display: flex; align-items: center; gap: 8px;
  padding: 12px 20px;
  border: 1px solid var(--border);
  font-family: var(--font-mono); font-size: 11px; letter-spacing: 2px;
  color: var(--text-dim); text-decoration: none;
  transition: all 0.3s; cursor: none;
}
.social-link:hover { border-color: var(--cyan); color: var(--cyan); box-shadow: 0 0 20px rgba(0,245,255,0.15); transform: translateY(-2px); }
.footer-quote {
  font-family: var(--font-head); font-size: 14px; font-weight: 700;
  color: var(--text-dim); letter-spacing: 4px; text-transform: uppercase;
}
.footer-quote span { color: var(--magenta); text-shadow: 0 0 10px var(--magenta); }
.footer-copy {
  font-family: var(--font-mono); font-size: 10px; color: rgba(200,230,255,0.2);
  margin-top: 24px; letter-spacing: 2px;
}

/* ─── REVEAL ANIMATION ─── */
.reveal { opacity: 0; transform: translateY(30px); transition: opacity 0.7s, transform 0.7s; }
.reveal.in { opacity: 1; transform: translateY(0); }

/* ─── GLITCH TEXT ─── */
.glitch {
  position: relative;
  animation: glitchMain 6s infinite;
}
.glitch::before, .glitch::after {
  content: attr(data-text);
  position: absolute; top: 0; left: 0;
  width: 100%; overflow: hidden;
}
.glitch::before {
  color: var(--cyan);
  animation: glitchTop 6s infinite;
  clip-path: polygon(0 0, 100% 0, 100% 35%, 0 35%);
}
.glitch::after {
  color: var(--magenta);
  animation: glitchBot 6s infinite;
  clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
}
@keyframes glitchMain {
  0%,90%,100%{transform:none}
  92%{transform:skewX(-0.5deg)}
  94%{transform:skewX(0.5deg) translateX(2px)}
  96%{transform:none}
}
@keyframes glitchTop {
  0%,88%,100%{transform:none;opacity:0}
  90%{transform:translateX(-3px);opacity:0.7}
  92%{transform:translateX(2px);opacity:0.7}
  94%{transform:none;opacity:0}
}
@keyframes glitchBot {
  0%,88%,100%{transform:none;opacity:0}
  91%{transform:translateX(3px);opacity:0.7}
  93%{transform:translateX(-2px);opacity:0.7}
  95%{transform:none;opacity:0}
}

/* ─── MOBILE ─── */
@media(max-width:768px){
  nav { padding: 16px 20px; }
  .nav-links { display: none; }
  .hero-coords, .hero-sys, .terminal-log { display: none; }
  .tech-card { min-width: 120px; }
  .hud-bar { flex-wrap: wrap; gap: 8px; }
  .social-links { flex-wrap: wrap; }
  .footer-quote { font-size: 11px; }
}
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-ring"></div>
<canvas id="canvas-bg"></canvas>
<canvas id="snake-canvas"></canvas>
<div class="scanlines"></div>

<div class="content">

<!-- NAV -->
<nav>
  <div class="nav-logo">JR<span>.</span>DEV</div>
  <div class="nav-links">
    <a href="#about">About</a>
    <a href="#tech">Stack</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
  </div>
  <div class="nav-status">
    <div class="status-dot"></div>
    OPEN TO WORK
  </div>
</nav>

<!-- HERO -->
<section id="hero">
  <div class="hero-grid"></div>
  <div class="hero-vignette"></div>

  <div class="hero-coords">
    LAT // -27.4692° S<br>
    LON // -58.8306° W<br>
    SYS // ONLINE<br>
    NET // SECURED<br>
    VER // 2.4.1
  </div>
  <div class="hero-sys">
    CPU_LOAD // 12%<br>
    MEM_USE // 4.2GB<br>
    UPTIME // 847h<br>
    STACK // FULL<br>
    STATUS // READY
  </div>

  <div class="hero-tag">// INITIALIZING PROFILE SEQUENCE_</div>
  <h1 class="hero-name">
    <span class="first">Juan</span>
    <span class="last glitch" data-text="Ramírez">Ramírez</span>
  </h1>
  <div class="hero-roles">
    <span class="typed-text" id="typed"></span><span class="typed-cursor"></span>
  </div>
  <p class="hero-tagline">Building scalable and futuristic experiences</p>
  <div class="hero-btns">
    <a href="#projects" class="btn btn-primary">▸ View Projects</a>
    <a href="#contact" class="btn btn-secondary">⬡ Contact Me</a>
    <a href="#" class="btn btn-ghost">⬇ Download CV</a>
  </div>

  <div class="hero-scroll">SCROLL<br>TO EXPLORE</div>
</section>

<!-- HUD BAR 1 -->
<div class="hud-bar reveal">
  <div class="hud-item"><div class="dot"></div> SYS_LOG: ALL MODULES OPERATIONAL</div>
  <div class="hud-item">
    BUILD_STATUS:
    <div class="loading-bar"><div class="loading-bar-fill"></div></div>
    COMPILE OK
  </div>
  <div class="hud-item"><div class="dot"></div> NODE_ENV: PRODUCTION</div>
</div>

<!-- ABOUT -->
<section id="about">
  <div class="container">
    <div class="about-grid">
      <div class="about-avatar-wrap reveal">
        <div style="position:relative;">
          <div class="avatar-ring"></div>
          <div class="avatar-hex">
            <div class="avatar-inner">JR</div>
          </div>
        </div>
      </div>
      <div class="about-text reveal">
        <div class="section-label">// ABOUT_ME</div>
        <h2 class="section-title">The <span>Human</span> Behind<br>the Machine</h2>
        <p>I'm a <strong>self-taught Full Stack Developer</strong> with a deep passion for building systems that scale. From backend architecture to cloud infrastructure, I engineer solutions that are built to last.</p>
        <p>My focus areas span <strong>backend development, cloud computing, automation</strong>, and interactive systems. I'm always exploring the intersection of engineering and creativity — including <strong>game development with Godot</strong>.</p>
        <p>Currently seeking new opportunities to contribute to ambitious projects. I thrive in environments where <strong>continuous learning and ambitious thinking</strong> are the norm.</p>
        <div class="about-stats">
          <div class="stat-card">
            <span class="stat-num">3+</span>
            <span class="stat-label">YEARS DEV</span>
          </div>
          <div class="stat-card">
            <span class="stat-num">10+</span>
            <span class="stat-label">PROJECTS</span>
          </div>
          <div class="stat-card">
            <span class="stat-num">∞</span>
            <span class="stat-label">LEARNING</span>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="terminal-log">
    <div class="log-line"><span class="log-ok">✓</span> identity.json loaded</div>
    <div class="log-line"><span class="log-ok">✓</span> skills module compiled</div>
    <div class="log-line"><span class="log-ok">✓</span> passion: HIGH</div>
    <div class="log-line"><span class="log-warn">!</span> coffee_level: LOW</div>
    <div class="log-line"><span class="log-ok">✓</span> ready_for_hire: TRUE</div>
  </div>
</section>

<!-- TECH STACK -->
<section id="tech">
  <div class="container">
    <div class="section-label">// TECH_STACK</div>
    <h2 class="section-title">My <span>Arsenal</span></h2>
    <div class="tech-categories">

      <div class="reveal">
        <div class="tech-cat-label">FRONTEND</div>
        <div class="tech-grid">
          <div class="tech-card"><div class="tech-icon" style="color:#61DAFB">⚛</div><div class="tech-name">React</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#fff">▲</div><div class="tech-name">Next.js</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#F7DF1E">JS</div><div class="tech-name">JavaScript</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#3178C6">TS</div><div class="tech-name">TypeScript</div></div>
        </div>
      </div>

      <div class="reveal">
        <div class="tech-cat-label">BACKEND</div>
        <div class="tech-grid">
          <div class="tech-card"><div class="tech-icon" style="color:#68A063">⬡</div><div class="tech-name">Node.js</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#aaa">⬡</div><div class="tech-name">Express</div></div>
        </div>
      </div>

      <div class="reveal">
        <div class="tech-cat-label">DATA & SERVICES</div>
        <div class="tech-grid">
          <div class="tech-card"><div class="tech-icon" style="color:#4DB33D">◈</div><div class="tech-name">MongoDB</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#FFCA28">🔥</div><div class="tech-name">Firebase</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#4285F4">◻</div><div class="tech-name">BigQuery</div></div>
        </div>
      </div>

      <div class="reveal">
        <div class="tech-cat-label">CLOUD & DEVOPS</div>
        <div class="tech-grid">
          <div class="tech-card"><div class="tech-icon" style="color:#FF9900">λ</div><div class="tech-name">AWS Lambda</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#FF9900">S3</div><div class="tech-name">Amazon S3</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#2496ED">🐳</div><div class="tech-name">Docker</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#00FF88">⚡</div><div class="tech-name">CI/CD</div></div>
        </div>
      </div>

      <div class="reveal">
        <div class="tech-cat-label">GAME DEV & QA</div>
        <div class="tech-grid">
          <div class="tech-card"><div class="tech-icon" style="color:#478CBF">◆</div><div class="tech-name">Godot Engine</div></div>
          <div class="tech-card"><div class="tech-icon" style="color:#BF5FFF">✓</div><div class="tech-name">QA Automation</div></div>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="container">
    <div class="section-label">// PROJECTS</div>
    <h2 class="section-title">Selected <span>Work</span></h2>
    <div class="projects-grid">

      <div class="project-card reveal">
        <div class="project-status">ACTIVE</div>
        <div class="project-num">// 001</div>
        <h3 class="project-name">CloudSync API</h3>
        <p class="project-desc">Scalable REST API built with Node.js and AWS Lambda, featuring automated CI/CD pipeline and real-time data sync across distributed services.</p>
        <div class="project-tags">
          <span class="tag">Node.js</span>
          <span class="tag">AWS Lambda</span>
          <span class="tag m">MongoDB</span>
          <span class="tag g">Docker</span>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-status">SHIPPED</div>
        <div class="project-num">// 002</div>
        <h3 class="project-name">Analytics Dashboard</h3>
        <p class="project-desc">Full-stack analytics platform using BigQuery for data warehousing and Next.js for a real-time, responsive dashboard with Firebase auth.</p>
        <div class="project-tags">
          <span class="tag">Next.js</span>
          <span class="tag">BigQuery</span>
          <span class="tag m">Firebase</span>
          <span class="tag">TypeScript</span>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-status">WIP</div>
        <div class="project-num">// 003</div>
        <h3 class="project-name">Neon Runner</h3>
        <p class="project-desc">Cyberpunk indie game built in Godot Engine. Features procedural level generation, neon shader effects, and online leaderboard integration.</p>
        <div class="project-tags">
          <span class="tag g">Godot Engine</span>
          <span class="tag">GDScript</span>
          <span class="tag m">Firebase</span>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-status">ACTIVE</div>
        <div class="project-num">// 004</div>
        <h3 class="project-name">Automata QA Suite</h3>
        <p class="project-desc">End-to-end QA automation framework built with modern testing tools, integrated into CI/CD pipelines with detailed reporting and alert systems.</p>
        <div class="project-tags">
          <span class="tag m">QA Automation</span>
          <span class="tag">Node.js</span>
          <span class="tag g">CI/CD</span>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-status">SHIPPED</div>
        <div class="project-num">// 005</div>
        <h3 class="project-name">S3 Media Pipeline</h3>
        <p class="project-desc">Serverless media processing pipeline on AWS. Automated image optimization, CDN distribution, and metadata indexing using S3 + Lambda.</p>
        <div class="project-tags">
          <span class="tag">Amazon S3</span>
          <span class="tag">AWS Lambda</span>
          <span class="tag m">Node.js</span>
        </div>
      </div>

      <div class="project-card reveal">
        <div class="project-status">ACTIVE</div>
        <div class="project-num">// 006</div>
        <h3 class="project-name">React Component Forge</h3>
        <p class="project-desc">Open-source component library built with React and TypeScript. Themeable, accessible, and optimized for performance with full test coverage.</p>
        <div class="project-tags">
          <span class="tag">React</span>
          <span class="tag">TypeScript</span>
          <span class="tag g">QA</span>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- HUD BAR 2 -->
<div class="hud-bar reveal">
  <div class="hud-item"><div class="dot" style="background:var(--magenta);box-shadow:0 0 6px var(--magenta)"></div> SIGNAL: ENCRYPTED</div>
  <div class="hud-item">
    SKILL_MATRIX:
    <div class="loading-bar"><div class="loading-bar-fill" style="animation-delay:1s"></div></div>
    92% LOADED
  </div>
  <div class="hud-item"><div class="dot" style="background:var(--green);box-shadow:0 0 6px var(--green)"></div> CLOUD_SYNC: ACTIVE</div>
</div>

<!-- FOOTER / CONTACT -->
<footer id="contact">
  <div class="footer-name reveal">JUAN RAMÍREZ</div>
  <div class="footer-tagline reveal">// FULL STACK DEVELOPER · CLOUD ENTHUSIAST · GODOT BUILDER</div>
  <div class="social-links reveal">
    <a href="https://github.com" class="social-link" target="_blank">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.387.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 21.795 24 17.295 24 12c0-6.63-5.37-12-12-12"/></svg>
      GITHUB
    </a>
    <a href="https://linkedin.com" class="social-link" target="_blank">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
      LINKEDIN
    </a>
    <a href="mailto:juan@example.com" class="social-link">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
      EMAIL
    </a>
  </div>
  <div class="footer-quote reveal">
    <span>"</span>Building systems <span>for the future.</span><span>"</span>
  </div>
  <div class="footer-copy">© 2025 Juan Ramírez · All Rights Reserved · SYS_VER 2.4.1</div>
</footer>

</div><!-- end .content -->

<script>
// ─── CURSOR ───
const cursor = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx = 0, my = 0, rx = 0, ry = 0;
document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
(function animCursor() {
  cursor.style.left = mx + 'px'; cursor.style.top = my + 'px';
  rx += (mx - rx) * 0.15; ry += (my - ry) * 0.15;
  ring.style.left = rx + 'px'; ring.style.top = ry + 'px';
  requestAnimationFrame(animCursor);
})();

// ─── PARTICLE BACKGROUND ───
const bgCanvas = document.getElementById('canvas-bg');
const bgCtx = bgCanvas.getContext('2d');
let W, H, particles = [];
function resize() {
  W = bgCanvas.width = window.innerWidth;
  H = bgCanvas.height = window.innerHeight;
}
resize(); window.addEventListener('resize', resize);

function mkParticle() {
  return {
    x: Math.random() * W,
    y: Math.random() * H,
    vx: (Math.random() - 0.5) * 0.3,
    vy: (Math.random() - 0.5) * 0.3,
    r: Math.random() * 1.5 + 0.2,
    color: Math.random() > 0.5 ? '0,245,255' : Math.random() > 0.5 ? '255,0,170' : '0,255,136',
    alpha: Math.random() * 0.5 + 0.1,
    life: Math.random() * 200 + 100,
    age: 0
  };
}
for (let i = 0; i < 120; i++) particles.push(mkParticle());

// Connection lines
function drawConnections() {
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x;
      const dy = particles[i].y - particles[j].y;
      const dist = Math.sqrt(dx*dx + dy*dy);
      if (dist < 120) {
        const a = (1 - dist / 120) * 0.12;
        bgCtx.beginPath();
        bgCtx.strokeStyle = `rgba(0,245,255,${a})`;
        bgCtx.lineWidth = 0.5;
        bgCtx.moveTo(particles[i].x, particles[i].y);
        bgCtx.lineTo(particles[j].x, particles[j].y);
        bgCtx.stroke();
      }
    }
  }
}

function animBg() {
  bgCtx.clearRect(0, 0, W, H);
  particles.forEach((p, i) => {
    p.x += p.vx; p.y += p.vy; p.age++;
    if (p.x < 0) p.x = W; if (p.x > W) p.x = 0;
    if (p.y < 0) p.y = H; if (p.y > H) p.y = 0;
    const fade = p.age < 30 ? p.age/30 : p.age > p.life - 30 ? (p.life - p.age)/30 : 1;
    bgCtx.beginPath();
    bgCtx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    bgCtx.fillStyle = `rgba(${p.color},${p.alpha * fade})`;
    bgCtx.shadowBlur = 8;
    bgCtx.shadowColor = `rgba(${p.color},0.8)`;
    bgCtx.fill();
    bgCtx.shadowBlur = 0;
    if (p.age >= p.life) particles[i] = mkParticle();
  });
  drawConnections();
  requestAnimationFrame(animBg);
}
animBg();

// ─── SNAKE ANIMATION ───
const sCanvas = document.getElementById('snake-canvas');
const sCtx = sCanvas.getContext('2d');
function sResize() { sCanvas.width = window.innerWidth; sCanvas.height = window.innerHeight; }
sResize(); window.addEventListener('resize', sResize);

const SNAKE = {
  segs: [],
  segLen: 18,
  maxLen: 60,
  speed: 1.8,
  angle: Math.random() * Math.PI * 2,
  turnSpeed: 0.022,
  mouseX: window.innerWidth / 2,
  mouseY: window.innerHeight / 2,
  attracted: false
};

// Init snake
let sx = Math.random() * window.innerWidth;
let sy = Math.random() * window.innerHeight;
for (let i = 0; i < SNAKE.maxLen; i++) SNAKE.segs.push({ x: sx, y: sy });

document.addEventListener('mousemove', e => {
  SNAKE.mouseX = e.clientX;
  SNAKE.mouseY = e.clientY;
  SNAKE.attracted = true;
  clearTimeout(SNAKE._timer);
  SNAKE._timer = setTimeout(() => SNAKE.attracted = false, 2000);
});

let snakeHue = 180;
function animSnake() {
  sCtx.clearRect(0, 0, sCanvas.width, sCanvas.height);
  snakeHue = (snakeHue + 0.3) % 360;

  const head = SNAKE.segs[0];

  if (SNAKE.attracted) {
    const tx = SNAKE.mouseX - head.x, ty = SNAKE.mouseY - head.y;
    const targetAngle = Math.atan2(ty, tx);
    let diff = targetAngle - SNAKE.angle;
    while (diff > Math.PI) diff -= Math.PI * 2;
    while (diff < -Math.PI) diff += Math.PI * 2;
    SNAKE.angle += diff * 0.04;
  } else {
    SNAKE.angle += (Math.random() - 0.5) * SNAKE.turnSpeed * 2;
  }

  let nx = head.x + Math.cos(SNAKE.angle) * SNAKE.speed;
  let ny = head.y + Math.sin(SNAKE.angle) * SNAKE.speed;

  // Bounce at edges
  if (nx < 0 || nx > sCanvas.width) SNAKE.angle = Math.PI - SNAKE.angle;
  if (ny < 0 || ny > sCanvas.height) SNAKE.angle = -SNAKE.angle;
  nx = Math.max(0, Math.min(sCanvas.width, nx));
  ny = Math.max(0, Math.min(sCanvas.height, ny));

  SNAKE.segs.unshift({ x: nx, y: ny });
  if (SNAKE.segs.length > SNAKE.maxLen) SNAKE.segs.pop();

  // Draw snake
  for (let i = 1; i < SNAKE.segs.length; i++) {
    const t = 1 - i / SNAKE.segs.length;
    const thickness = t * 3.5;
    const a = t * 0.7;

    // Alternate color
    const c = i < SNAKE.segs.length * 0.5
      ? `rgba(0,245,255,${a})`
      : `rgba(255,0,170,${a * 0.6})`;

    sCtx.beginPath();
    sCtx.lineWidth = thickness;
    sCtx.strokeStyle = c;
    sCtx.shadowBlur = 12 * t;
    sCtx.shadowColor = i < SNAKE.segs.length * 0.5 ? '#00F5FF' : '#FF00AA';
    sCtx.moveTo(SNAKE.segs[i - 1].x, SNAKE.segs[i - 1].y);
    sCtx.lineTo(SNAKE.segs[i].x, SNAKE.segs[i].y);
    sCtx.stroke();
  }
  sCtx.shadowBlur = 0;

  // Draw head dot
  sCtx.beginPath();
  sCtx.arc(head.x, head.y, 4, 0, Math.PI * 2);
  sCtx.fillStyle = '#00F5FF';
  sCtx.shadowBlur = 20; sCtx.shadowColor = '#00F5FF';
  sCtx.fill();
  sCtx.shadowBlur = 0;

  requestAnimationFrame(animSnake);
}
animSnake();

// ─── TYPED TEXT ───
const roles = [
  'Full Stack Developer',
  'Cloud Enthusiast',
  'Godot Builder',
  'Backend Engineer',
  'Problem Solver',
];
let ri = 0, ci = 0, deleting = false;
const typedEl = document.getElementById('typed');
function typeLoop() {
  const word = roles[ri];
  if (!deleting) {
    typedEl.textContent = word.slice(0, ++ci);
    if (ci === word.length) { deleting = true; setTimeout(typeLoop, 2200); return; }
  } else {
    typedEl.textContent = word.slice(0, --ci);
    if (ci === 0) { deleting = false; ri = (ri + 1) % roles.length; }
  }
  setTimeout(typeLoop, deleting ? 45 : 85);
}
typeLoop();

// ─── REVEAL ON SCROLL ───
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver(entries => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      setTimeout(() => e.target.classList.add('in'), i * 80);
      observer.unobserve(e.target);
    }
  });
}, { threshold: 0.1 });
reveals.forEach(el => observer.observe(el));
</script>
</body>
</html>
