(() => {
  'use strict';
  const cv = document.getElementById('game'); const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  const dpr = Math.max(1, Math.min(3, window.devicePixelRatio || 1));
  cv.width = W * dpr; cv.height = H * dpr; ctx.scale(dpr, dpr);
  const scoreEl = document.getElementById('score'), timeEl = document.getElementById('time');
  const overlay = document.getElementById('overlay'), ovTitle = document.getElementById('ov-title'), ovSub = document.getElementById('ov-sub');
  const halfW = 150, ballR = 12;
  let playerTilt, ballX, vx, t, over, secs, timer, keys;

  function reset() { playerTilt = 0; ballX = 0; vx = 0; t = 0; over = false; secs = 0; keys = {}; scoreEl.textContent = '0'; timeEl.textContent = '0'; overlay.classList.add('hidden'); if (timer) clearInterval(timer); timer = setInterval(() => { if (!over) { secs++; scoreEl.textContent = secs; timeEl.textContent = secs; } }, 1000); }
  function update() {
    if (over) return;
    t += 0.016;
    if (keys['ArrowLeft'] || keys['a']) playerTilt -= 0.02;
    if (keys['ArrowRight'] || keys['d']) playerTilt += 0.02;
    playerTilt = Math.max(-0.6, Math.min(0.6, playerTilt));
    const disturbance = 0.32 * Math.sin(t * 0.9) + 0.12 * Math.sin(t * 0.37);
    const tilt = playerTilt + disturbance;
    const acc = Math.sin(tilt) * 1.3;
    vx += acc; vx *= 0.985; ballX += vx;
    if (Math.abs(ballX) > halfW + ballR) { over = true; ovTitle.textContent = '掉下去了'; ovSub.textContent = '坚持了 ' + secs + ' 秒'; overlay.classList.remove('hidden'); }
  }
  function draw() {
    ctx.fillStyle = '#1a1c3a'; ctx.fillRect(0, 0, W, H);
    const disturbance = 0.32 * Math.sin(t * 0.9) + 0.12 * Math.sin(t * 0.37);
    const tilt = playerTilt + disturbance;
    ctx.save(); ctx.translate(W / 2, H / 2 + 40); ctx.rotate(tilt);
    ctx.fillStyle = '#34386e'; ctx.fillRect(-halfW, 0, halfW * 2, 16);
    ctx.fillStyle = '#6c7bff'; ctx.fillRect(-halfW, 0, halfW * 2, 4);
    ctx.fillStyle = '#ffd23f'; ctx.beginPath(); ctx.arc(ballX, -ballR - 8, ballR, 0, Math.PI * 2); ctx.fill();
    ctx.restore();
  }
  window.addEventListener('keydown', e => { keys[e.key] = true; if (['ArrowLeft', 'ArrowRight', 'a', 'd'].includes(e.key)) e.preventDefault(); });
  window.addEventListener('keyup', e => { keys[e.key] = false; });
  cv.addEventListener('touchstart', e => { e.preventDefault(); const t = e.changedTouches[0]; const rect = cv.getBoundingClientRect(); if ((t.clientX - rect.left) / rect.width < 0.5) playerTilt -= 0.18; else playerTilt += 0.18; }, { passive: false });
  document.getElementById('new').addEventListener('click', reset);
  document.getElementById('ov-btn').addEventListener('click', reset);
  function loop() { update(); draw(); requestAnimationFrame(loop); }
  reset(); requestAnimationFrame(loop);
})();
