---
title: 🏔️Faraway's Way
toc: false
sidebar:
  hide: true
---

<style>
@media (max-width: 640px) {
  .hero-intro { flex-direction: column !important; align-items: center !important; }
  .hero-intro .hero-text { text-align: center; }
  .hero-intro .hero-links { justify-content: center !important; }
}
.hobby-cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
  margin: 1rem 0;
}
.hobby-card {
  position: relative;
  display: flex;
  flex-direction: column;
  border-radius: 0.5rem;
  border: 1px solid #e5e7eb;
  background: #f3f4f6;
  overflow: hidden;
  box-shadow: 0 1px 2px rgba(0,0,0,.05);
  transition: box-shadow .2s ease, border-color .2s ease;
}
.hobby-card:hover {
  box-shadow: 0 8px 20px rgba(0,0,0,.12);
  border-color: #d1d5db;
}
.dark .hobby-card {
  background: #262626;
  border-color: #404040;
  box-shadow: none;
}
.dark .hobby-card:hover {
  border-color: #525252;
  box-shadow: 0 8px 20px rgba(0,0,0,.35);
}
.hobby-card__image-link { display: block; line-height: 0; }
.hobby-card__image-link img {
  width: 100%;
  height: 120px;
  object-fit: contain;
  display: block;
}
.hobby-card__body-link {
  display: block;
  padding: 1rem;
  text-decoration: none !important;
  color: inherit;
}
.hobby-card__title {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-weight: 600;
  font-size: 1rem;
  color: #374151;
}
.dark .hobby-card__title { color: #e5e5e5; }
.hobby-card__body-link:hover .hobby-card__title { color: #111827; }
.dark .hobby-card__body-link:hover .hobby-card__title { color: #fafafa; }
.hobby-card__tag {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #9ca3af;
  margin-bottom: 0.35rem;
}
.dark .hobby-card__tag { color: #737373; }
.hobby-card__subtitle {
  margin-top: 0.5rem;
  font-size: 0.875rem;
  line-height: 1.4;
  color: #6b7280;
}
.dark .hobby-card__subtitle { color: #a3a3a3; }
.hobby-card__arrow {
  position: absolute;
  top: 5px;
  right: 5px;
  z-index: 10;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.15rem 0.55rem;
  border-radius: 9999px;
  border: 1px solid #d1d5db;
  background: #fff;
  color: #000;
  font-weight: 900;
  font-size: 0.85rem;
  text-decoration: none !important;
  text-shadow: 0.4px 0 currentColor, -0.4px 0 currentColor, 0 0.4px currentColor, 0 -0.4px currentColor;
}
</style>

<div style="display: flex; gap: 0.6rem; flex-wrap: wrap; margin: 0.5rem 0 1.5rem;">
  <span style="padding: 0.3rem 0.9rem; border-radius: 9999px; border: 2.5px solid #E53935; color: #E53935; font-size: 0.92rem; font-weight: 500;">😃I'm Yuan Jack Yao</span>
  <span style="padding: 0.3rem 0.9rem; border-radius: 9999px; border: 2.5px solid #0071CE; color: #0071CE; font-size: 0.92rem; font-weight: 500;">Prev. AI DS III Intern @ Walmart🇺🇸</span>
  <span style="padding: 0.3rem 0.9rem; border-radius: 9999px; border: 2.5px solid #007A33; color: #007A33; font-size: 0.92rem; font-weight: 500;">Former Junior DS @ SUEZ · Shanghai🇨🇳</span>
  <span style="padding: 0.3rem 0.9rem; border-radius: 9999px; border: 2.5px solid #B8860B; color: #B8860B; font-size: 0.92rem; font-weight: 500;">School of CSE · Georgia Tech🐝</span>
</div>

<div class="hero-intro" style="display: flex; flex-direction: row; align-items: flex-start; gap: 2rem; width: 100%; margin: 1rem 0 0;">
  <img src="images/profile.jpg" alt="Yuan Jack Yao" style="width: 200px; height: 250px; border-radius: 8px; object-fit: cover; flex-shrink: 0;" />
  <div class="hero-text" style="display: flex; flex-direction: column; gap: 0.75rem; flex: 1; padding-top: 0.25rem;">
    <p style="font-size: 18px; line-height: 1.6;">
      In Chinese, my name is placed as『Yao Yuan』.<br/>
      The adjective faraway in Chinese is also『yao yuan』.<br/>
      Back to good old school days, I had 🎙️homophone nickname "Faraway" 😉
    </p>
    <p style="font-size: 18px; line-height: 1.6;">
      A big passion of mine is building & delivering AI/ML systems, and clear communication with clients.<br/>
      It was all my full-time Data Scientist role in Shanghai, China 🇨🇳<br/><br/>
      Bringing it into U.S. 🇺🇸, I keep deploying Agentic AI to
      closely connect AI products & real-time demands. Welcome to chat 🤓
    </p>
  <div class="hero-links" style="display: flex; gap: 1.5rem; flex-wrap: wrap; align-items: center; font-size: 1rem;">
      <a href="https://github.com/StarsExpress" target="_blank" rel="noopener noreferrer" style="display: flex; align-items: center; gap: 0.5rem; text-decoration: none; font-size: 1.2rem;">
        <span style="display: inline-flex; transform: scale(1.5);">{{< icon "github" >}}</span> GitHub
      </a>
      <a href="mailto:yyao411@gatech.edu" style="display: flex; align-items: center; gap: 0.5rem; text-decoration: none; font-size: 1.2rem;">
        <span style="display: inline-flex; transform: scale(1.5);">{{< icon "mail" >}}</span> Email
      </a>
      <a href="https://www.linkedin.com/in/Yuan-Jack-Yao" target="_blank" rel="noopener noreferrer" style="display: flex; align-items: center; gap: 0.5rem; text-decoration: none; font-size: 1.2rem;">
        <span style="display: inline-flex; transform: scale(1.5);">{{< icon "linkedin" >}}</span> LinkedIn
      </a>
    </div>
  </div>
</div>


## Some OSS Contributed & Hobbies Deployed

<div class="hobby-cards">
  <div class="hobby-card">
    <a class="hobby-card__image-link" href="https://code-puppy.dev" target="_blank" rel="noopener noreferrer">
      <img src="images/code_puppy_logo.png" alt="🐶Code Puppy" style="background: #111827;">
    </a>
    <a class="hobby-card__arrow" href="https://code-puppy.dev" target="_blank" rel="noopener noreferrer" aria-label="Open LinesShines live site">↗</a>
    <a class="hobby-card__body-link" href="notes/code_puppy/">
      <div class="hobby-card__tag">Open Source · Coding Agent</div>
      <div class="hobby-card__title">🐶Code Puppy</div>
      <div class="hobby-card__subtitle">💻I contribute by bug fixes and performance enhancement.</div>
    </a>
  </div>
  
  <div class="hobby-card">
    <a class="hobby-card__image-link" href="https://www.lines-shines.com" target="_blank" rel="noopener noreferrer">
      <img src="images/lines_shines_logo.png" alt="🏈LinesShines · 鋒光" style="background: #111827;">
    </a>
    <a class="hobby-card__arrow" href="https://www.lines-shines.com" target="_blank" rel="noopener noreferrer" aria-label="Open LinesShines live site">↗</a>
    <a class="hobby-card__body-link" href="linesshines/">
      <div class="hobby-card__tag">Hobby • NFL Metrics</div>
      <div class="hobby-card__title">🏈LinesShines · 鋒光</div>
      <div class="hobby-card__subtitle">★Customized interactive NFL O & D Lines stats system.</div>
    </a>
  </div>

  <div class="hobby-card">
    <a class="hobby-card__image-link" href="https://www.jack-s-onlineblackjack.com/" target="_blank" rel="noopener noreferrer">
      <img src="images/job_logo.png" alt="🎰MonkeyJOB" style="background: #111827;">
    </a>
    <a class="hobby-card__arrow" href="https://www.jack-s-onlineblackjack.com/" target="_blank" rel="noopener noreferrer" aria-label="Open MonkeyJOB live site">↗</a>
    <a class="hobby-card__body-link" href="monkeyjob/">
      <div class="hobby-card__tag">Hobby • Game</div>
      <div class="hobby-card__title">🎰MonkeyJOB</div>
      <div class="hobby-card__subtitle">🎰MonkeyJOB: Jack's Online Blackjack — 🎮Casino-style UI web Blackjack game.</div>
    </a>
  </div>

  <div class="hobby-card">
    <a class="hobby-card__image-link" href="https://starsexpress.github.io/SkyHorse/" target="_blank" rel="noopener noreferrer">
      <img src="images/skyhorse_logo.jpg" alt="🏇天码行空 · SkyHorse" style="background: #1e293b;">
    </a>
    <a class="hobby-card__arrow" href="https://starsexpress.github.io/SkyHorse/" target="_blank" rel="noopener noreferrer" aria-label="Open SkyHorse live site">↗</a>
    <a class="hobby-card__body-link" href="skyhorse/">
      <div class="hobby-card__tag">Hobby • DSA</div>
      <div class="hobby-card__title">🏇天码行空 · SkyHorse</div>
      <div class="hobby-card__subtitle">📙Bilingual &amp; double-indexed tech blog for data structures &amp; algorithms.</div>
    </a>
  </div>
</div>
