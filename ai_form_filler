// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      2026-06-08
// @description  try to take over the world!
// @author       You
// @match        https://*/*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=tampermonkey.net
// @grant GM_addStyle
// @grant GM_xmlhttpRequest
// ==/UserScript==

(function() {
     'use strict';
  // ── CONFIG ────────────────────────────────────────────
  const ANTHROPIC_API_KEY = '';
  const ANTHROPIC_MODEL = 'claude-sonnet-4-6';
    console.log(getCRMFields());

  // Prove the script is running at all — visible in DevTools console
  console.log('[VA] userscript loaded, href:', location.href);

  // ── STYLES — inject once, immediately ─────────────────
  GM_addStyle(`
      @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600&display=swap');

      :root {
        --va-bg:         #0e0f11;
        --va-surface:    #16181c;
        --va-surface2:   #1e2128;
        --va-surface3:   #252830;
        --va-border:     #2e3138;
        --va-border2:    #3a3f4a;
        --va-accent:     #4ade80;
        --va-accent2:    #22c55e;
        --va-accent-dim: rgba(74,222,128,0.12);
        --va-red:        #f87171;
        --va-red-dim:    rgba(248,113,113,0.12);
        --va-amber:      #fbbf24;
        --va-tp:         #e8eaf0;
        --va-ts:         #8b909e;
        --va-tm:         #555b6b;
        --va-mono:       'IBM Plex Mono', monospace;
        --va-sans:       'DM Sans', sans-serif;
      }

      /* ── FAB ── */
      #va-fab {
        position: fixed !important;
        bottom: 28px !important;
        right: 28px !important;
        width: 56px !important;
        height: 56px !important;
        border-radius: 50% !important;
        background: var(--va-bg) !important;
        border: 1.5px solid var(--va-border2) !important;
        cursor: pointer !important;
        display: flex !important;
        align-items: center !important;
        justify-content: center !important;
        box-shadow: 0 8px 32px rgba(0,0,0,0.35) !important;
        transition: transform 0.2s, box-shadow 0.2s !important;
        z-index: 2147483647 !important;
        padding: 0 !important;
        margin: 0 !important;
      }
      #va-fab:hover {
        transform: scale(1.08) !important;
        box-shadow: 0 12px 40px rgba(0,0,0,0.4), 0 0 0 8px var(--va-accent-dim) !important;
      }
      #va-fab.va-recording {
        background: var(--va-red-dim) !important;
        border-color: var(--va-red) !important;
        animation: va-fabPulse 1.4s ease-in-out infinite !important;
      }
      @keyframes va-fabPulse {
        0%,100% { box-shadow: 0 8px 32px rgba(0,0,0,0.28), 0 0 0 0 rgba(248,113,113,0.35); }
        50%     { box-shadow: 0 8px 32px rgba(0,0,0,0.28), 0 0 0 14px rgba(248,113,113,0); }
      }
      #va-fab svg { width: 22px; height: 22px; pointer-events: none; }

      /* ── PANEL ── */
      #va-panel {
        position: fixed !important;
        bottom: 96px !important;
        right: 28px !important;
        width: 360px !important;
        background: var(--va-surface) !important;
        border: 1px solid var(--va-border) !important;
        border-radius: 16px !important;
        box-shadow: 0 24px 60px rgba(0,0,0,0.5), 0 0 0 1px rgba(255,255,255,0.04) inset !important;
        z-index: 2147483646 !important;
        overflow: hidden !important;
        transform: translateY(16px) scale(0.97) !important;
        opacity: 0 !important;
        pointer-events: none !important;
        transition: transform 0.28s cubic-bezier(0.34,1.56,0.64,1), opacity 0.22s ease !important;
        font-family: var(--va-sans) !important;
        box-sizing: border-box !important;
      }
      #va-panel.va-open {
        transform: translateY(0) scale(1) !important;
        opacity: 1 !important;
        pointer-events: all !important;
      }
      #va-panel * { box-sizing: border-box; margin: 0; padding: 0; }

      .va-panel-header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 16px 18px 12px;
        border-bottom: 1px solid var(--va-border);
      }
      .va-panel-title {
        display: flex;
        align-items: center;
        gap: 8px;
      }
      .va-dot {
        width: 7px; height: 7px; border-radius: 50%;
        background: var(--va-accent);
        box-shadow: 0 0 6px var(--va-accent);
        flex-shrink: 0;
      }
      .va-dot.va-red {
        background: var(--va-red);
        box-shadow: 0 0 6px var(--va-red);
        animation: va-dotBlink 1s ease-in-out infinite;
      }
      @keyframes va-dotBlink {
        0%,100% { opacity: 1; } 50% { opacity: 0.3; }
      }
      .va-panel-title span {
        font-family: var(--va-mono);
        font-size: 11px; font-weight: 500;
        color: var(--va-ts);
        letter-spacing: 0.08em; text-transform: uppercase;
      }
      .va-btn-close {
        background: none; border: none; cursor: pointer;
        color: var(--va-tm); padding: 4px; border-radius: 6px;
        display: flex; align-items: center;
        transition: color 0.15s, background 0.15s;
      }
      .va-btn-close:hover { color: var(--va-tp); background: var(--va-surface2); }
      .va-btn-close svg { width: 16px; height: 16px; pointer-events: none; }

      /* ── VIZ ── */
      .va-viz-zone {
        padding: 20px 18px 16px;
        display: flex; flex-direction: column;
        align-items: center; gap: 14px;
      }
      #va-waveform {
        width: 100%; height: 52px;
        display: flex; align-items: center;
        justify-content: center; gap: 2.5px;
      }
      .va-bar {
        width: 3px; border-radius: 2px;
        background: var(--va-border2);
        transition: height 0.08s ease, background 0.3s;
        min-height: 4px;
      }
      .va-bar.va-active  { background: var(--va-accent); }
      .va-bar.va-red-bar { background: var(--va-red); }
      .va-timer {
        font-family: var(--va-mono); font-size: 22px; font-weight: 400;
        color: var(--va-tp); letter-spacing: 0.05em;
      }
      .va-timer.va-inactive { color: var(--va-tm); }

      /* ── RECORD BTN ── */
      .va-record-row { padding: 0 18px 18px; display: flex; gap: 10px; }
      .va-btn-record {
        flex: 1; height: 44px; border-radius: 10px; border: none;
        cursor: pointer; font-family: var(--va-sans);
        font-size: 13px; font-weight: 600; letter-spacing: 0.02em;
        transition: all 0.18s ease;
        display: flex; align-items: center; justify-content: center; gap: 8px;
      }
      .va-btn-record.va-start {
        background: var(--va-accent-dim); color: var(--va-accent);
        border: 1px solid rgba(74,222,128,0.3);
      }
      .va-btn-record.va-start:hover {
        background: rgba(74,222,128,0.2); border-color: var(--va-accent);
        box-shadow: 0 0 20px var(--va-accent-dim);
      }
      .va-btn-record.va-stop {
        background: var(--va-red-dim); color: var(--va-red);
        border: 1px solid rgba(248,113,113,0.3);
        animation: va-stopPulse 1.4s ease-in-out infinite;
      }
      @keyframes va-stopPulse {
        0%,100% { box-shadow: 0 0 0 0 rgba(248,113,113,0.3); }
        50%     { box-shadow: 0 0 16px rgba(248,113,113,0.2); }
      }
      .va-btn-record.va-stop:hover { background: rgba(248,113,113,0.2); }
      .va-btn-record svg { width: 16px; height: 16px; pointer-events: none; }

      /* ── TRANSCRIPT ── */
      .va-transcript-zone {
        margin: 0 18px;
        background: var(--va-surface2); border: 1px solid var(--va-border);
        border-radius: 8px; padding: 12px 14px;
        min-height: 70px; max-height: 120px; overflow-y: auto;
        font-family: var(--va-mono); font-size: 12px; line-height: 1.7;
        color: var(--va-ts); display: none;
      }
      .va-transcript-zone.va-visible { display: block; }
      .va-transcript-zone .va-placeholder { color: var(--va-tm); font-style: italic; }
      .va-transcript-zone .va-live-text  { color: var(--va-tp); }

      /* ── THINKING ── */
      .va-thinking-row {
        display: none; align-items: center; gap: 10px;
        padding: 10px 18px 14px;
        color: var(--va-ts); font-family: var(--va-mono);
        font-size: 11px; letter-spacing: 0.05em;
      }
      .va-thinking-row.va-visible { display: flex; }
      .va-thinking-dots span {
        animation: va-thinkDot 1.2s infinite;
        font-family: var(--va-mono); font-size: 18px; color: var(--va-accent);
      }
      .va-thinking-dots span:nth-child(2) { animation-delay: 0.2s; }
      .va-thinking-dots span:nth-child(3) { animation-delay: 0.4s; }
      @keyframes va-thinkDot {
        0%,80%,100% { opacity: 0.15; transform: scale(0.8); }
        40%         { opacity: 1;    transform: scale(1.1); }
      }

      /* ── SUGGESTIONS ── */
      .va-suggestions-zone {
        margin: 12px 18px 18px;
        display: none; flex-direction: column; gap: 8px;
      }
      .va-suggestions-zone.va-visible { display: flex; }
      .va-suggestions-header {
        display: flex; align-items: center; justify-content: space-between;
      }
      .va-suggestions-header > span {
        font-family: var(--va-mono); font-size: 10px; font-weight: 500;
        color: var(--va-tm); letter-spacing: 0.1em; text-transform: uppercase;
      }
      .va-count-badge {
        background: var(--va-accent-dim); color: var(--va-accent);
        font-size: 10px; font-family: var(--va-mono);
        padding: 2px 7px; border-radius: 20px;
        border: 1px solid rgba(74,222,128,0.25);
      }
      .va-suggestion-list {
        display: flex; flex-direction: column; gap: 6px;
        max-height: 220px; overflow-y: auto;
      }
      .va-suggestion-item {
        background: var(--va-surface2); border: 1px solid var(--va-border);
        border-radius: 8px; padding: 10px 12px; cursor: pointer;
        transition: border-color 0.18s, background 0.18s, transform 0.15s;
        display: flex; align-items: flex-start;
        justify-content: space-between; gap: 10px;
      }
      .va-suggestion-item:hover {
        border-color: var(--va-accent); background: var(--va-surface3);
        transform: translateX(-2px);
      }
      .va-suggestion-item.va-applied {
        border-color: var(--va-accent2); background: rgba(34,197,94,0.06);
        opacity: 0.6; cursor: default;
      }
      .va-suggestion-item.va-applied:hover { transform: none; }
      .va-suggestion-left { flex: 1; min-width: 0; }
      .va-suggestion-field {
        font-family: var(--va-mono); font-size: 10px; color: var(--va-tm);
        letter-spacing: 0.06em; text-transform: uppercase; margin-bottom: 3px;
      }
      .va-suggestion-value {
        font-size: 13px; color: var(--va-tp); font-weight: 500;
        white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
      }
      .va-suggestion-apply {
        flex-shrink: 0; background: var(--va-accent-dim);
        border: 1px solid rgba(74,222,128,0.25); color: var(--va-accent);
        font-size: 11px; font-family: var(--va-mono); font-weight: 500;
        padding: 4px 10px; border-radius: 6px; cursor: pointer;
        transition: background 0.15s, border-color 0.15s; white-space: nowrap;
      }
      .va-suggestion-apply:hover {
        background: rgba(74,222,128,0.22); border-color: var(--va-accent);
      }
      .va-suggestion-item.va-applied .va-suggestion-apply {
        background: rgba(34,197,94,0.08); color: var(--va-accent2); cursor: default;
      }
      .va-btn-apply-all {
        width: 100%; height: 40px; border-radius: 8px;
        background: var(--va-accent-dim); border: 1px solid rgba(74,222,128,0.3);
        color: var(--va-accent); font-family: var(--va-sans);
        font-size: 13px; font-weight: 600; cursor: pointer;
        transition: background 0.18s, box-shadow 0.18s;
        display: flex; align-items: center; justify-content: center; gap: 6px;
        margin-top: 2px;
      }
      .va-btn-apply-all:hover {
        background: rgba(74,222,128,0.2);
        box-shadow: 0 0 20px var(--va-accent-dim);
      }
      .va-btn-apply-all svg { width: 14px; height: 14px; pointer-events: none; }

      /* ── STATUS ── */
      .va-status-strip {
        padding: 10px 18px; border-top: 1px solid var(--va-border);
        font-family: var(--va-mono); font-size: 10px; color: var(--va-tm);
        letter-spacing: 0.06em; display: flex; align-items: center; gap: 6px;
      }
      .va-status-dot {
        width: 5px; height: 5px; border-radius: 50%;
        background: var(--va-tm); flex-shrink: 0;
      }
      .va-status-strip.va-ready .va-status-dot { background: var(--va-accent); box-shadow: 0 0 4px var(--va-accent); }
      .va-status-strip.va-busy  .va-status-dot { background: var(--va-amber); box-shadow: 0 0 4px var(--va-amber); animation: va-dotBlink 0.8s infinite; }
      .va-status-strip.va-error .va-status-dot { background: var(--va-red);   box-shadow: 0 0 4px var(--va-red); }

      /* ── SCROLLBAR ── */
      #va-panel ::-webkit-scrollbar { width: 4px; }
      #va-panel ::-webkit-scrollbar-track { background: transparent; }
      #va-panel ::-webkit-scrollbar-thumb { background: var(--va-border2); border-radius: 4px; }

      /* ── FIELD HIGHLIGHT ── */
      .va-field-suggested {
        border-color: #4ade80 !important;
        background: #f0fdf4 !important;
        box-shadow: 0 0 0 3px rgba(74,222,128,0.15) !important;
        animation: va-fieldPulse 0.5s ease !important;
      }
      @keyframes va-fieldPulse {
        0%   { transform: scale(1); }
        50%  { transform: scale(1.006); }
        100% { transform: scale(1); }
      }
  `);

  // ── STATE ─────────────────────────────────────────────
  let panelOpen = false;
  let isRecording = false;
  let mediaRecorder = null;
  let timerInterval = null;
  let elapsed = 0;
  let analyser = null;
  let animFrame = null;
  let audioCtx = null;
  let suggestions = [];
  let recognition = null;
  let liveTranscript = '';

  // ── BOOTSTRAP ─────────────────────────────────────────
  // Use document-start so we register early, but wait for body before injecting DOM.
  function bootstrap() {
    console.log('[VA] bootstrap(), body ready:', !!document.body);
    if (document.getElementById('va-fab')) {
      console.log('[VA] already injected, skipping');
      return;
    }
    injectWidget();
  }

  // document-start fires before body exists — wait for it
  if (document.body) {
    bootstrap();
  } else {
    document.addEventListener('DOMContentLoaded', bootstrap);
    // Belt-and-suspenders: also try on readystatechange
    document.addEventListener('readystatechange', function () {
      if (document.readyState !== 'loading') bootstrap();
    });
  }

  // SPA navigation: Zoho changes the URL without a page reload.
  // Use a polling interval (safer than MutationObserver for this).
  let lastUrl = location.href;
  setInterval(function () {
    if (location.href !== lastUrl) {
      lastUrl = location.href;
      console.log('[VA] URL changed to', lastUrl);
      // Re-inject after Zoho finishes rendering (~1.5s is enough)
      setTimeout(function () {
        if (!document.getElementById('va-fab')) bootstrap();
      }, 1500);
    }
  }, 500);

  // ── INJECT WIDGET ─────────────────────────────────────
  function injectWidget() {
    console.log('[VA] injecting widget');

    // FAB button
    const fab = document.createElement('button');
    fab.id = 'va-fab';
    fab.title = 'Lead Voice Assistant';
    fab.innerHTML = `<svg viewBox="0 0 24 24" fill="none" stroke="#4ade80" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12 2a3 3 0 0 1 3 3v7a3 3 0 0 1-6 0V5a3 3 0 0 1 3-3z"/>
      <path d="M19 10v2a7 7 0 0 1-14 0v-2"/>
      <line x1="12" y1="19" x2="12" y2="22"/>
      <line x1="8" y1="22" x2="16" y2="22"/>
    </svg>`;
    fab.addEventListener('click', togglePanel);
    document.body.appendChild(fab);

    // Panel
    const panel = document.createElement('div');
    panel.id = 'va-panel';
    panel.innerHTML = `
      <div class="va-panel-header">
        <div class="va-panel-title">
          <div class="va-dot" id="va-status-dot"></div>
          <span id="va-panel-title">Voice Assistant</span>
        </div>
        <button class="va-btn-close" id="va-close-btn" title="Close">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
            <path d="M18 6 6 18M6 6l12 12"/>
          </svg>
        </button>
      </div>
      <div class="va-viz-zone">
        <div id="va-waveform"></div>
        <div class="va-timer va-inactive" id="va-timer">00:00</div>
      </div>
      <div class="va-record-row">
        <button class="va-btn-record va-start" id="va-record-btn">
          <svg viewBox="0 0 24 24" fill="currentColor"><circle cx="12" cy="12" r="6"/></svg>
          Start Recording
        </button>
      </div>
      <div class="va-transcript-zone" id="va-transcript">
        <span class="va-placeholder">Transcript will appear here…</span>
      </div>
      <div class="va-thinking-row" id="va-thinking">
        <div class="va-thinking-dots"><span>·</span><span>·</span><span>·</span></div>
        <span>Analysing fields</span>
      </div>
      <div class="va-suggestions-zone" id="va-suggestions">
        <div class="va-suggestions-header">
          <span>Suggested values</span>
          <span class="va-count-badge" id="va-count">0</span>
        </div>
        <div class="va-suggestion-list" id="va-suggestion-list"></div>
        <button class="va-btn-apply-all" id="va-apply-all">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 6 9 17l-5-5"/>
          </svg>
          Apply All
        </button>
      </div>
      <div class="va-status-strip va-ready" id="va-status-strip">
        <div class="va-status-dot"></div>
        <span id="va-status-text">Ready — tap mic to begin</span>
      </div>

    `;
    document.body.appendChild(panel);

    // Build waveform bars
    const waveform = document.getElementById('va-waveform');
    for (let i = 0; i < 40; i++) {
      const b = document.createElement('div');
      b.className = 'va-bar';
      b.style.height = '4px';
      waveform.appendChild(b);
    }

    // Wire up buttons
    document.getElementById('va-close-btn').addEventListener('click', togglePanel);
    document.getElementById('va-record-btn').addEventListener('click', toggleRecording);
    document.getElementById('va-apply-all').addEventListener('click', applyAll);

    console.log('[VA] widget injected OK');
  }

  // ── PANEL TOGGLE ──────────────────────────────────────
  function togglePanel() {
    panelOpen = !panelOpen;
    document.getElementById('va-panel').classList.toggle('va-open', panelOpen);
  }

  // ── STATUS ────────────────────────────────────────────
  function setStatus(msg, state) {
    state = state || 'ready';
    document.getElementById('va-status-text').textContent = msg;
    document.getElementById('va-status-strip').className = 'va-status-strip va-' + state;
  }

  // ── TIMER ─────────────────────────────────────────────
  function startTimer() {
    elapsed = 0; updateTimer();
    timerInterval = setInterval(function () { elapsed++; updateTimer(); }, 1000);
  }
  function stopTimer() { clearInterval(timerInterval); }
  function updateTimer() {
    const el = document.getElementById('va-timer');
    el.textContent = String(Math.floor(elapsed / 60)).padStart(2, '0') + ':' + String(elapsed % 60).padStart(2, '0');
    el.classList.remove('va-inactive');
  }

  // ── WAVEFORM ──────────────────────────────────────────
  function animateBars(active, useRed) {
    const bars = document.querySelectorAll('.va-bar');
    if (!active) {
      bars.forEach(function (b) { b.style.height = '4px'; b.className = 'va-bar'; });
      return;
    }
    if (analyser) {
      const data = new Uint8Array(analyser.frequencyBinCount);
      analyser.getByteFrequencyData(data);
      const step = Math.floor(data.length / 40);
      bars.forEach(function (b, i) {
        const h = 4 + ((data[i * step] || 0) / 255) * 44;
        b.style.height = h + 'px';
        b.className = 'va-bar ' + (useRed ? 'va-red-bar' : 'va-active');
      });
    } else {
      bars.forEach(function (b, i) {
        const h = 4 + Math.abs(Math.sin(Date.now() / 200 + i * 0.5)) * 36;
        b.style.height = h + 'px';
        b.className = 'va-bar ' + (useRed ? 'va-red-bar' : 'va-active');
      });
    }
    animFrame = requestAnimationFrame(function () { animateBars(active, useRed); });
  }

  // ── READ FIELDS FROM ZOHO DOM ─────────────────────────
  // Zoho renders inputs with name attributes matching CRM field API names.
  // We scan visible inputs/selects/textareas to build the field list.
  function getCRMFields() {
    const fields = [];
    const seen = new Set();

    // Zoho CRM input selectors — covers standard and custom fields
    const selectors = [
      'input[name]:not([type="hidden"]):not([type="submit"]):not([type="button"])', 'input',
      'select',
      'textarea',
        '[data-zcqa]',
        'crm-create-fields'
    ];




    document.querySelectorAll(selectors.join(',')).forEach(el => {
console.log(el);
      const name = el.name || el.getAttribute('name') || el.getAttribute('id') || el.getAttribute('placeholder');
      if (!name || seen.has(name) || name.startsWith('_') || name === 'search') return;
      seen.add(name);

      // Skip Zoho internal / system fields
      if (['csrfParam', 'csrfToken', 'formName', 'module'].includes(name)) return;

      // Try to find a human label from the nearest <label> or aria-label
      let label = el.getAttribute('aria-label') || el.getAttribute('placeholder') || '';
      if (!label) {
        const id = el.id;
        if (id) {
          const lbl = document.querySelector(`label[for="${id}"]`);
          if (lbl) label = lbl.textContent.trim().replace(/\s*\*\s*$/, '');
        }
      }
      if (!label) label = name.replace(/_/g, ' ');

      let options = null;
      if (el.tagName === 'SELECT') {
        options = Array.from(el.options).map(o => o.value || o.text).filter(v => v && v !== '--None--' && v !== 'None');
      }

      fields.push({ apiName: name, label, type: el.tagName.toLowerCase(), element: el, options });
    });

    return fields;
  }

  // ── RECORDING ─────────────────────────────────────────
  async function toggleRecording() {
    isRecording ? stopRecording() : await startRecording();
  }

  async function startRecording() {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });

      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      analyser = audioCtx.createAnalyser();
      analyser.fftSize = 256;
      audioCtx.createMediaStreamSource(stream).connect(analyser);

      mediaRecorder = new MediaRecorder(stream);
      mediaRecorder.ondataavailable = () => {};
      mediaRecorder.onstop = handleRecordingStop;
      mediaRecorder.start();

      // Speech recognition
      liveTranscript = '';
      const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (SR) {
        recognition = new SR();
        recognition.continuous = true;
        recognition.interimResults = true;
        recognition.onresult = e => {
          let interim = '', final = '';
          for (let i = e.resultIndex; i < e.results.length; i++) {
            if (e.results[i].isFinal) final += e.results[i][0].transcript + ' ';
            else interim += e.results[i][0].transcript;
          }
          liveTranscript += final;
          showTranscript((liveTranscript + interim).trim(), true);
        };
        recognition.onerror = err => console.warn('[VA] Speech error:', err.error);
        recognition.start();
      }

      isRecording = true;
      document.getElementById('va-fab').classList.add('va-recording');
      document.getElementById('va-status-dot').classList.add('va-red');
      document.getElementById('va-panel-title').textContent = 'Recording…';

      const btn = document.getElementById('va-record-btn');
      btn.className = 'va-btn-record va-stop';
      btn.innerHTML = `<svg viewBox="0 0 24 24" fill="currentColor"><rect x="6" y="6" width="12" height="12" rx="2"/></svg> Stop Recording`;

      const tz = document.getElementById('va-transcript');
      tz.classList.add('va-visible');
      tz.innerHTML = '<span class="va-placeholder">Listening…</span>';
      document.getElementById('va-suggestions').classList.remove('va-visible');

      setStatus('Recording in progress', 'busy');
      startTimer();
      animateBars(true, true);

    } catch (err) {
      setStatus('Microphone access denied — check browser permissions', 'error');
      console.error('[VA] getUserMedia error:', err);
    }
  }

  function stopRecording() {
    isRecording = false;
    cancelAnimationFrame(animFrame);
    animateBars(false, false);
    stopTimer();

    document.getElementById('va-fab').classList.remove('va-recording');
    document.getElementById('va-status-dot').classList.remove('va-red');
    document.getElementById('va-panel-title').textContent = 'Processing…';
    document.getElementById('va-timer').classList.add('va-inactive');

    const btn = document.getElementById('va-record-btn');
    btn.className = 'va-btn-record va-start';
    btn.innerHTML = `<svg viewBox="0 0 24 24" fill="currentColor"><circle cx="12" cy="12" r="6"/></svg> Start Recording`;

    setStatus('Finishing transcript…', 'busy');

    // Wait for recognition.onend before stopping mediaRecorder
    if (recognition) {
      recognition.onend = () => {
        if (mediaRecorder && mediaRecorder.state !== 'inactive') mediaRecorder.stop();
        mediaRecorder.stream.getTracks().forEach(t => t.stop());
      };
      try { recognition.stop(); } catch(e) {
        if (mediaRecorder && mediaRecorder.state !== 'inactive') mediaRecorder.stop();
        mediaRecorder.stream.getTracks().forEach(t => t.stop());
      }
    } else {
      if (mediaRecorder && mediaRecorder.state !== 'inactive') mediaRecorder.stop();
      mediaRecorder.stream.getTracks().forEach(t => t.stop());
    }
  }

  async function handleRecordingStop() {
    const transcript = liveTranscript.trim();
    setStatus('Processing…', 'busy');

    if (!transcript) {
      setStatus('No speech detected — try again', 'error');
      document.getElementById('va-panel-title').textContent = 'Voice Assistant';
      document.getElementById('va-transcript').innerHTML =
        '<span class="va-placeholder">Nothing captured. Please try again.</span>';
      return;
    }

    showTranscript(transcript, false);
    await analyseTranscript(transcript);
  }

  function showTranscript(text, isLive) {
    const tz = document.getElementById('va-transcript');
    tz.classList.add('va-visible');
    tz.innerHTML = `<span class="${isLive ? 'va-live-text' : ''}">${text}</span>`;
  }

  // ── ANALYSE WITH CLAUDE via GM_xmlhttpRequest ─────────
  // GM_xmlhttpRequest runs in the extension context — no CORS restriction
  function analyseTranscript(transcript) {
    return new Promise((resolve) => {
      document.getElementById('va-thinking').classList.add('va-visible');
      setStatus('Asking Claude to map fields…', 'busy');

      const fields = getCRMFields();
      const fieldDescriptions = fields.map(f => {
        let desc = `- ${f.apiName} (label: "${f.label}", type: ${f.type})`;
        if (f.options && f.options.length) desc += ` — allowed values: [${f.options.slice(0,10).join(', ')}]`;
        return desc;
      }).join('\n');

      const prompt = `You are a CRM data extraction assistant. Given a spoken transcript from a sales rep about a new lead, extract relevant field values and map them to the CRM fields listed below.

CRM MODULE: Leads
AVAILABLE FIELDS:
${fieldDescriptions}

TRANSCRIPT:
"${transcript}"

INSTRUCTIONS:
- Only suggest fields where you found clear relevant information in the transcript.
- For select/picklist fields, only suggest values that exactly match one of the allowed values listed.
- For text fields, extract the most relevant clean value.
- Do not guess or infer values not mentioned.
- Return ONLY a valid JSON array. No markdown, no explanation, just the JSON.

FORMAT:
[
  { "apiName": "field_api_name", "label": "Human Label", "value": "extracted value", "confidence": "high|medium" }
]`;

      GM_xmlhttpRequest({
        method: 'POST',
        url: 'https://api.anthropic.com/v1/messages',
        headers: {
          'x-api-key': ANTHROPIC_API_KEY,
          'anthropic-version': '2023-06-01',
          'content-type': 'application/json'
        },
        data: JSON.stringify({
          model: ANTHROPIC_MODEL,
          max_tokens: 1000,
          messages: [{ role: 'user', content: prompt }]
        }),
        onload: (res) => {
          document.getElementById('va-thinking').classList.remove('va-visible');
          document.getElementById('va-panel-title').textContent = 'Voice Assistant';
          try {
            if (res.status !== 200) throw new Error(`API ${res.status}: ${res.responseText.slice(0,120)}`);
            const data = JSON.parse(res.responseText);
            const claudeText = data?.content?.[0]?.text || '[]';
            const clean = claudeText.replace(/```json|```/g, '').trim();
            const parsed = JSON.parse(clean);
            renderSuggestions(parsed, fields);
            setStatus(`${parsed.length} field(s) identified`, 'ready');
          } catch (err) {
            setStatus('Error: ' + err.message, 'error');
            console.error('[VA] Claude error:', err);
          }
          resolve();
        },
        onerror: (err) => {
          document.getElementById('va-thinking').classList.remove('va-visible');
          document.getElementById('va-panel-title').textContent = 'Voice Assistant';
          setStatus('Network error — check console', 'error');
          console.error('[VA] GM_xmlhttpRequest error:', err);
          resolve();
        }
      });
    });
  }

  // ── RENDER SUGGESTIONS ────────────────────────────────
  function renderSuggestions(items, fields) {
    suggestions = items.map(s => ({ ...s, applied: false, fieldEl: fields.find(f => f.apiName === s.apiName)?.element || null }));

    const list = document.getElementById('va-suggestion-list');
    document.getElementById('va-count').textContent = suggestions.length;
    list.innerHTML = '';

    suggestions.forEach((s, idx) => {
      const div = document.createElement('div');
      div.className = 'va-suggestion-item';
      div.id = `va-sug-${idx}`;
      div.innerHTML = `
        <div class="va-suggestion-left">
          <div class="va-suggestion-field">${s.label}</div>
          <div class="va-suggestion-value" title="${s.value}">${s.value}</div>
        </div>
        <button class="va-suggestion-apply" id="va-apply-btn-${idx}">Apply</button>
      `;
      div.querySelector(`#va-apply-btn-${idx}`).addEventListener('click', () => applySuggestion(idx));
      list.appendChild(div);
    });

    document.getElementById('va-suggestions').classList.add('va-visible');
  }

  // ── APPLY ─────────────────────────────────────────────
  function applySuggestion(idx) {
    const s = suggestions[idx];
    if (s.applied) return;

    // Find the live element at apply-time (Zoho may have re-rendered)
    const el = s.fieldEl || document.querySelector(`[name="${s.apiName}"]`);
    if (el) {
      if (el.tagName === 'SELECT') {
        const opt = Array.from(el.options).find(o =>
          o.value.toLowerCase() === s.value.toLowerCase() ||
          o.text.toLowerCase() === s.value.toLowerCase()
        );
        if (opt) {
          el.value = opt.value;
          el.dispatchEvent(new Event('change', { bubbles: true }));
        }
      } else {
        el.value = s.value;
        el.dispatchEvent(new Event('input', { bubbles: true }));
        el.dispatchEvent(new Event('change', { bubbles: true }));
      }
      el.classList.add('va-field-suggested');
      setTimeout(() => el.classList.remove('va-field-suggested'), 2500);
    }

    s.applied = true;
    const item = document.getElementById(`va-sug-${idx}`);
    item.classList.add('va-applied');
    item.querySelector(`#va-apply-btn-${idx}`).textContent = '✓ Done';
    document.getElementById('va-count').textContent = suggestions.filter(s => !s.applied).length;
  }

  function applyAll() {
    suggestions.forEach((_, idx) => applySuggestion(idx));
  }

})();
