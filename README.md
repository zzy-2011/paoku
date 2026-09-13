(() => {
  'use strict';
  const cv = document.getElementById('game'); const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  const dpr = Math.max(1, Math.min(3, window.devicePixelRatio || 1));
  cv.width = W * dpr; cv.height = H * dpr; ctx.scale(dpr, dpr);
  const scoreEl = document.getElementById('score'), bestEl = document.getElementById('best');
  const overlay = document.getElementById('overlay'), ovTitle = document.getElementById('ov-title'), ovSub = document.getElementById('ov-sub');
  const GY = H - 60, PW = 30, PH = 40;
  let px, py, vy, onGround, obs, speed, score, best, over, spawnTimer;

  function reset() {
    px = 60; py = GY - PH; vy = 0; onGround = true; obs = []; speed = 4.5; score = 0; over = false; spawnTimer = 60;
    scoreEl.textContent = '0'; bestEl.textContent = best || '0'; overlay.classList.add('hidden');
  }
  function jump() { if (over) return; if (onGround) { vy = -13; onGround = false; } }
  function update() {
    if (over) return;
    if (!onGround) { vy += 0.7; py += vy; if (py >= GY - PH) { py = GY - PH; vy = 0; onGround = true; } }
    spawnTimer--; if (spawnTimer <= 0) { obs.push({ x: W + 20, w: 18 + Math.random() * 18, h: 24 + Math.random() * 36 }); spawnTimer = 60 + Math.random() * 50; }
    for (const o of obs) o.x -= speed;
    obs = obs.filter(o => o.x + o.w > -10);
    speed += 0.0008; score += 0.15; scoreEl.textContent = Math.floor(score);
    for (const o of obs) {
      if (px + PW > o.x && px < o.x + o.w && py + PH > GY - o.h && py + PH < GY + 4) {
        over = true; if (Math.floor(score) > (best || 0)) { best = Math.floor(score); bestEl.textContent = best; }
        ovTitle.textContent = '撞到了！'; ovSub.textContent = '得分 ' + Math.floor(score); overlay.classList.remove('hidden'); return;
      }
    }
  }
  function draw() {
    ctx.fillStyle = '#1a1c3a'; ctx.fillRect(0, 0, W, H);
    ctx.fillStyle = '#262a55'; ctx.fillRect(0, GY, W, H - GY);
    ctx.fillStyle = '#43d97a'; ctx.fillRect(px, py, PW, PH);
    ctx.fillStyle = '#10122a'; ctx.fillRect(px + 6, py + 8, 5, 5); ctx.fillRect(px + 18, py + 8, 5, 5);
    ctx.fillStyle = '#ff5c7a'; for (const o of obs) ctx.fillRect(o.x, GY - o.h, o.w, o.h);
  }
  window.addEventListener('keydown', e => { if (e.key === ' ' || e.key === 'ArrowUp' || e.key === 'w') { e.preventDefault(); jump(); } });
  cv.addEventListener('click', jump);
  cv.addEventListener('touchstart', e => { e.preventDefault(); jump(); }, { passive: false });
  document.getElementById('new').addEventListener('click', reset);
  document.getElementById('ov-btn').addEventListener('click', reset);
  function loop() { update(); draw(); requestAnimationFrame(loop); }
  reset(); requestAnimationFrame(loop);
})();
