<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Calculator — Davron Murodullaev</title>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;700&family=JetBrains+Mono:wght@300;400;700&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  :root {
    --bg: #0d0d0f;
    --surface: #16161a;
    --surface2: #1e1e24;
    --border: rgba(255,255,255,0.07);
    --accent: #7c6af7;
    --accent2: #f76a8c;
    --text: #f0f0f5;
    --text-dim: #6b6b80;
    --btn-num: #1e1e2a;
    --btn-op: #252535;
    --btn-eq: linear-gradient(135deg, #7c6af7, #f76a8c);
    --shadow: 0 25px 60px rgba(0,0,0,0.6);
    --glow: 0 0 30px rgba(124,106,247,0.3);
  }

  body {
    background: var(--bg);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Outfit', sans-serif;
    overflow: hidden;
  }

  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: 
      radial-gradient(ellipse 60% 50% at 20% 20%, rgba(124,106,247,0.12) 0%, transparent 60%),
      radial-gradient(ellipse 50% 40% at 80% 80%, rgba(247,106,140,0.10) 0%, transparent 60%);
    pointer-events: none;
  }

  .calculator {
    width: 340px;
    background: var(--surface);
    border-radius: 28px;
    border: 1px solid var(--border);
    box-shadow: var(--shadow), var(--glow);
    padding: 24px;
    position: relative;
    animation: slideUp 0.6s cubic-bezier(0.34, 1.56, 0.64, 1) both;
  }

  @keyframes slideUp {
    from { opacity: 0; transform: translateY(40px) scale(0.95); }
    to   { opacity: 1; transform: translateY(0) scale(1); }
  }

  .brand {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 20px;
  }

  .brand-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 10px var(--accent);
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(0.8); }
  }

  .brand-name {
    font-size: 11px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--text-dim);
    font-weight: 600;
  }

  .display {
    background: var(--surface2);
    border-radius: 18px;
    border: 1px solid var(--border);
    padding: 20px 22px 16px;
    margin-bottom: 20px;
    min-height: 110px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
    overflow: hidden;
  }

  .display::after {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(124,106,247,0.5), transparent);
  }

  .expression {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: var(--text-dim);
    text-align: right;
    min-height: 18px;
    word-break: break-all;
    transition: all 0.2s;
  }

  .result {
    font-family: 'JetBrains Mono', monospace;
    font-size: 42px;
    font-weight: 300;
    color: var(--text);
    text-align: right;
    line-height: 1;
    word-break: break-all;
    transition: all 0.15s ease;
  }

  .result.small { font-size: 28px; }
  .result.tiny  { font-size: 20px; }

  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }

  button {
    height: 68px;
    border: none;
    border-radius: 16px;
    font-family: 'Outfit', sans-serif;
    font-size: 18px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.12s cubic-bezier(0.34, 1.56, 0.64, 1);
    position: relative;
    overflow: hidden;
    border: 1px solid var(--border);
    outline: none;
    -webkit-tap-highlight-color: transparent;
  }

  button::after {
    content: '';
    position: absolute;
    inset: 0;
    background: white;
    opacity: 0;
    transition: opacity 0.15s;
    border-radius: inherit;
  }

  button:active::after { opacity: 0.08; }
  button:active { transform: scale(0.93); }

  .btn-num {
    background: var(--btn-num);
    color: var(--text);
  }
  .btn-num:hover { background: #252535; border-color: rgba(255,255,255,0.12); }

  .btn-op {
    background: var(--btn-op);
    color: var(--accent);
    font-size: 20px;
    font-weight: 600;
  }
  .btn-op:hover { background: rgba(124,106,247,0.15); border-color: rgba(124,106,247,0.3); }

  .btn-fn {
    background: rgba(255,255,255,0.06);
    color: var(--text-dim);
    font-size: 15px;
  }
  .btn-fn:hover { background: rgba(255,255,255,0.1); color: var(--text); }

  .btn-eq {
    background: var(--btn-eq);
    color: white;
    font-size: 22px;
    font-weight: 700;
    box-shadow: 0 8px 24px rgba(124,106,247,0.4);
    border: none;
  }
  .btn-eq:hover { 
    box-shadow: 0 12px 30px rgba(124,106,247,0.5);
    transform: translateY(-1px);
  }
  .btn-eq:active { transform: scale(0.93) translateY(0); }

  .btn-zero {
    grid-column: span 2;
    text-align: left;
    padding-left: 26px;
  }

  .ripple {
    position: absolute;
    border-radius: 50%;
    background: rgba(255,255,255,0.15);
    transform: scale(0);
    animation: ripple 0.4s ease-out;
    pointer-events: none;
  }

  @keyframes ripple {
    to { transform: scale(4); opacity: 0; }
  }

  @keyframes numPop {
    0%   { transform: scale(1); }
    50%  { transform: scale(1.04); }
    100% { transform: scale(1); }
  }

  .pop { animation: numPop 0.15s ease; }
</style>
</head>
<body>

<div class="calculator" id="calc">
  <div class="brand">
    <div class="brand-dot"></div>
    <span class="brand-name">Calculator</span>
  </div>

  <div class="display">
    <div class="expression" id="expression"></div>
    <div class="result" id="result">0</div>
  </div>

  <div class="buttons">
    <button class="btn-fn" onclick="clearAll()">AC</button>
    <button class="btn-fn" onclick="toggleSign()">+/−</button>
    <button class="btn-fn" onclick="percent()">%</button>
    <button class="btn-op" onclick="setOp('/')">÷</button>

    <button class="btn-num" onclick="appendNum('7')">7</button>
    <button class="btn-num" onclick="appendNum('8')">8</button>
    <button class="btn-num" onclick="appendNum('9')">9</button>
    <button class="btn-op" onclick="setOp('*')">×</button>

    <button class="btn-num" onclick="appendNum('4')">4</button>
    <button class="btn-num" onclick="appendNum('5')">5</button>
    <button class="btn-num" onclick="appendNum('6')">6</button>
    <button class="btn-op" onclick="setOp('-')">−</button>

    <button class="btn-num" onclick="appendNum('1')">1</button>
    <button class="btn-num" onclick="appendNum('2')">2</button>
    <button class="btn-num" onclick="appendNum('3')">3</button>
    <button class="btn-op" onclick="setOp('+')">+</button>

    <button class="btn-num btn-zero" onclick="appendNum('0')">0</button>
    <button class="btn-num" onclick="appendDot()">.</button>
    <button class="btn-eq" onclick="calculate()">=</button>
  </div>
</div>

<script>
  let currentVal = '0';
  let prevVal = '';
  let operator = null;
  let shouldReset = false;

  const resultEl = document.getElementById('result');
  const exprEl = document.getElementById('expression');

  function updateDisplay(val) {
    let display = val.toString();
    if (!display.includes('.') && !display.includes('e')) {
      let num = parseFloat(display);
      if (!isNaN(num)) display = num.toLocaleString('en-US');
    }
    resultEl.textContent = display;
    resultEl.className = 'result';
    if (display.length > 12) resultEl.classList.add('tiny');
    else if (display.length > 9) resultEl.classList.add('small');
    resultEl.classList.remove('pop');
    void resultEl.offsetWidth;
    resultEl.classList.add('pop');
  }

  function appendNum(num) {
    addRipple(event);
    if (shouldReset) { currentVal = ''; shouldReset = false; }
    if (currentVal === '0' && num !== '.') currentVal = num;
    else currentVal += num;
    updateDisplay(currentVal);
  }

  function appendDot() {
    addRipple(event);
    if (shouldReset) { currentVal = '0'; shouldReset = false; }
    if (!currentVal.includes('.')) currentVal += '.';
    updateDisplay(currentVal);
  }

  function setOp(op) {
    addRipple(event);
    if (operator && !shouldReset) calculate(true);
    prevVal = currentVal;
    operator = op;
    shouldReset = true;
    const symbols = { '+': '+', '-': '−', '*': '×', '/': '÷' };
    exprEl.textContent = `${prevVal} ${symbols[op]}`;
  }

  function calculate(chain = false) {
    if (!operator || !prevVal) return;
    if (event) addRipple(event);
    let a = parseFloat(prevVal.replace(/,/g, ''));
    let b = parseFloat(currentVal.replace(/,/g, ''));
    let res;
    switch(operator) {
      case '+': res = a + b; break;
      case '-': res = a - b; break;
      case '*': res = a * b; break;
      case '/': res = b !== 0 ? a / b : 'Error'; break;
    }
    const symbols = { '+': '+', '-': '−', '*': '×', '/': '÷' };
    if (!chain) exprEl.textContent = `${prevVal} ${symbols[operator]} ${currentVal} =`;
    if (typeof res === 'number') res = Math.round(res * 1e10) / 1e10;
    currentVal = res.toString();
    updateDisplay(currentVal);
    operator = null;
    shouldReset = true;
    if (!chain) prevVal = '';
  }

  function clearAll() {
    addRipple(event);
    currentVal = '0'; prevVal = ''; operator = null; shouldReset = false;
    exprEl.textContent = '';
    updateDisplay('0');
  }

  function toggleSign() {
    addRipple(event);
    currentVal = (parseFloat(currentVal) * -1).toString();
    updateDisplay(currentVal);
  }

  function percent() {
    addRipple(event);
    currentVal = (parseFloat(currentVal) / 100).toString();
    updateDisplay(currentVal);
  }

  function addRipple(e) {
    if (!e || !e.currentTarget) return;
    const btn = e.currentTarget;
    const r = document.createElement('span');
    r.className = 'ripple';
    const size = Math.max(btn.offsetWidth, btn.offsetHeight);
    r.style.width = r.style.height = size + 'px';
    const rect = btn.getBoundingClientRect();
    r.style.left = (e.clientX - rect.left - size/2) + 'px';
    r.style.top  = (e.clientY - rect.top  - size/2) + 'px';
    btn.appendChild(r);
    r.addEventListener('animationend', () => r.remove());
  }

  document.addEventListener('keydown', e => {
    if ('0123456789'.includes(e.key)) appendNum(e.key);
    else if (e.key === '.') appendDot();
    else if (e.key === '+') setOp('+');
    else if (e.key === '-') setOp('-');
    else if (e.key === '*') setOp('*');
    else if (e.key === '/') { e.preventDefault(); setOp('/'); }
    else if (e.key === 'Enter' || e.key === '=') calculate();
    else if (e.key === 'Escape') clearAll();
    else if (e.key === 'Backspace') {
      if (currentVal.length > 1) currentVal = currentVal.slice(0, -1);
      else currentVal = '0';
      updateDisplay(currentVal);
    }
  });
</script>
</body>
</html>
