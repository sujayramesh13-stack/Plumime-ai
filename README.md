<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover"/>
<meta name="mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
<meta name="theme-color" content="#04030A"/>
<title>Plumine AI</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Crimson+Pro:ital,wght@0,300;0,400;0,600;1,300;1,400&display=swap" rel="stylesheet"/>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
:root{
  --gold:#D4A843;--gold2:#8B6210;--gold3:#FFD875;--gold-dim:#7a6030;
  --bg:#04030A;--bg2:#0A0818;--bg3:#120F20;--bg4:#1a1630;
  --border:#1e1a30;--border2:#2a2545;
  --text:#F0E8D8;--dim:#C8B898;--muted:#5a5070;--faint:#2a2540;
  --red:#FF6B6B;--green:#4CAF50;--blue:#60A5FA;
  --radius:18px;--shadow:0 20px 60px rgba(0,0,0,.7);
}
html,body{height:100%;background:var(--bg);color:var(--text);font-family:'Crimson Pro',Georgia,serif;overflow:hidden;font-size:16px}
#app{display:flex;flex-direction:column;height:100vh;height:100dvh;position:relative;overflow:hidden}

/* ── CANVAS BG ── */
#bg-canvas{position:fixed;inset:0;z-index:0;pointer-events:none;opacity:.6}

/* ── SPLASH ── */
#splash{position:fixed;inset:0;z-index:1000;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;transition:opacity .8s ease}
#splash.hide{opacity:0;pointer-events:none}
.splash-logo{animation:splashPulse 2s ease infinite}
.splash-title{font-family:'Cinzel',serif;font-size:36px;letter-spacing:.3em;color:var(--gold);margin-top:20px;animation:fadeUp .8s .3s both}
.splash-sub{font-size:11px;letter-spacing:.4em;color:var(--gold-dim);margin-top:8px;font-family:'Cinzel',serif;animation:fadeUp .8s .5s both}
.splash-bar{width:200px;height:1px;background:var(--border2);margin-top:30px;border-radius:1px;overflow:hidden;animation:fadeUp .8s .7s both}
.splash-progress{height:100%;background:linear-gradient(90deg,transparent,var(--gold),transparent);animation:splashLoad 2s ease forwards}

/* ── LOGIN ── */
#login{position:fixed;inset:0;z-index:100;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:20px;opacity:0;pointer-events:none;transition:opacity .6s}
#login.show{opacity:1;pointer-events:all}
.login-wrap{width:100%;max-width:400px}
.login-header{text-align:center;margin-bottom:32px}
.login-logo-wrap{position:relative;display:inline-block;margin-bottom:16px}
.login-rings{position:absolute;inset:-20px;animation:spin 20s linear infinite}
.login-title{font-family:'Cinzel',serif;font-size:28px;letter-spacing:.2em;color:var(--gold)}
.login-tagline{font-size:13px;color:var(--muted);margin-top:6px;font-style:italic}
.login-card{background:linear-gradient(135deg,#0A0818,#120F20);border:1px solid var(--border2);border-radius:24px;padding:28px;box-shadow:var(--shadow),0 0 60px rgba(212,168,67,.04)}
.login-card-title{font-family:'Cinzel',serif;font-size:14px;letter-spacing:.15em;color:var(--gold-dim);text-align:center;margin-bottom:20px}
.g-btn{display:flex;align-items:center;justify-content:center;gap:12px;width:100%;padding:15px;border-radius:14px;background:#fff;border:none;cursor:pointer;font-size:15px;font-weight:600;color:#1a1a1a;font-family:'Crimson Pro',serif;transition:all .2s;box-shadow:0 4px 20px rgba(0,0,0,.4)}
.g-btn:active{transform:scale(.97)}
.sep{display:flex;align-items:center;gap:10px;margin:18px 0}
.sep-line{flex:1;height:1px;background:var(--border2)}
.sep-text{font-size:10px;color:var(--muted);font-family:monospace;letter-spacing:.1em}
.guest-btn{width:100%;padding:14px;border-radius:14px;background:transparent;border:1px solid var(--border2);color:var(--muted);font-size:14px;cursor:pointer;font-family:'Crimson Pro',serif;transition:all .2s}
.guest-btn:active{border-color:var(--gold);color:var(--gold)}
.g-input{width:100%;padding:13px 16px;border-radius:12px;background:var(--bg3);border:1px solid var(--border2);color:var(--text);font-size:14px;outline:none;font-family:'Crimson Pro',serif}
.g-input:focus{border-color:var(--gold-dim)}
.g-row{display:flex;gap:8px;margin-top:8px}
.g-go{padding:13px 18px;border-radius:12px;background:var(--gold);border:none;color:var(--bg);font-size:16px;cursor:pointer;font-weight:700}
.login-features{display:flex;flex-wrap:wrap;justify-content:center;gap:6px;margin-top:20px}
.l-feat{font-size:9px;color:var(--faint);font-family:monospace;letter-spacing:.12em;padding:4px 8px;border:1px solid var(--border);border-radius:10px}
.loading-wrap{text-align:center;padding:24px 0}
.loading-ring{width:48px;height:48px;border:2px solid var(--border2);border-top-color:var(--gold);border-radius:50%;animation:spin 1s linear infinite;margin:0 auto}
.loading-txt{color:var(--gold);font-size:11px;font-family:monospace;letter-spacing:.2em;margin-top:14px}

/* ── HEADER ── */
#header{position:relative;z-index:10;backdrop-filter:blur(30px);-webkit-backdrop-filter:blur(30px);background:rgba(4,3,10,.92);border-bottom:1px solid var(--border2);padding:11px 14px 0;flex-shrink:0;transition:border-color .5s}
.hdr-top{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.hdr-logo{flex-shrink:0;position:relative}
.hdr-logo-glow{position:absolute;inset:-4px;border-radius:50%;animation:glow 3s ease infinite}
.hdr-name{font-family:'Cinzel',serif;font-size:15px;letter-spacing:.16em;line-height:1;transition:color .5s}
.hdr-sub{font-size:8px;letter-spacing:.22em;margin-top:3px;font-family:monospace;transition:color .5s;opacity:.6}
.hdr-badges{display:flex;align-items:center;gap:6px;margin-left:auto}
.badge{display:flex;align-items:center;gap:4px;padding:4px 9px;border-radius:20px;border:1px solid}
.badge-dot{width:5px;height:5px;border-radius:50%;animation:pulse 2s infinite}
.badge-text{font-size:8px;font-family:monospace;letter-spacing:.1em}
.hdr-avatar{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;font-size:13px;font-weight:700;border:1.5px solid;flex-shrink:0;font-family:'Cinzel',serif;position:relative}
.avatar-ring{position:absolute;inset:-3px;border-radius:50%;border:1px solid;opacity:.4;animation:spin 8s linear infinite}

/* TABS */
.tabs{display:flex;gap:4px;overflow-x:auto;padding-bottom:0;-webkit-overflow-scrolling:touch;scrollbar-width:none}
.tabs::-webkit-scrollbar{display:none}
.tab{padding:8px 12px;border-radius:14px 14px 0 0;font-size:11px;border:none;font-family:'Cinzel',serif;cursor:pointer;white-space:nowrap;transition:all .25s;flex-shrink:0;letter-spacing:.06em;background:transparent;border-bottom:2px solid transparent}
.tab.active{border-bottom-color:var(--gold)}

/* ── PROFILE ── */
#profile-drop{position:absolute;top:108px;right:14px;z-index:50;background:var(--bg3);border:1px solid var(--border2);border-radius:18px;padding:18px;width:220px;box-shadow:var(--shadow);display:none;animation:fadeUp .2s ease}
#profile-drop.show{display:block}
.p-avatar-lg{width:48px;height:48px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:20px;font-weight:700;font-family:'Cinzel',serif;border:2px solid}
.p-name{font-size:15px;color:var(--text);font-family:'Cinzel',serif;margin-top:2px}
.p-type{font-size:9px;font-family:monospace;margin-top:2px;letter-spacing:.1em}
.p-email{font-size:10px;color:var(--muted);font-family:monospace;margin:10px 0;word-break:break-all;line-height:1.5}
.p-stat{display:flex;justify-content:space-between;padding:8px 0;border-top:1px solid var(--border);font-size:11px}
.p-stat-label{color:var(--muted)}
.p-stat-val{color:var(--gold);font-family:monospace}
.p-signout{width:100%;padding:10px;border-radius:10px;background:rgba(255,107,107,.1);border:1px solid rgba(255,107,107,.2);color:var(--red);font-size:13px;cursor:pointer;font-family:'Crimson Pro',serif;margin-top:12px}

/* ── CONTENT ── */
#content{flex:1;overflow:hidden;display:flex;flex-direction:column;position:relative;z-index:1}

/* ── MESSAGES ── */
#msgs{flex:1;overflow-y:auto;padding:16px 12px 8px;-webkit-overflow-scrolling:touch}
#msgs::-webkit-scrollbar{width:2px}
#msgs::-webkit-scrollbar-thumb{background:var(--border2);border-radius:2px}
.msg-wrap{display:flex;gap:8px;margin-bottom:18px;align-items:flex-start;animation:fadeUp .3s ease both}
.msg-wrap.user{flex-direction:row-reverse}
.m-avatar{width:26px;height:26px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;flex-shrink:0;margin-top:2px;border:1px solid}
.bubble{max-width:82%;font-size:14.5px;line-height:1.8;word-break:break-word}
.bubble.ai{color:var(--dim);border-left:2px solid var(--gold);padding-left:13px;transition:border-color .5s}
.bubble.user{color:var(--dim);background:linear-gradient(135deg,#12101e,#1a1830);border:1px solid var(--border2);border-radius:14px 3px 14px 14px;padding:11px 15px}
.msg-actions{display:flex;gap:6px;margin-top:5px;flex-wrap:wrap}
.msg-action-btn{background:transparent;border:none;cursor:pointer;font-size:10px;font-family:monospace;padding:3px 7px;border-radius:8px;transition:all .18s;letter-spacing:.06em;border:1px solid transparent}
.msg-action-btn:hover{border-color:var(--border2)}
.msg-img{display:block;margin-top:12px;border-radius:12px;max-width:100%;border:1px solid;cursor:pointer;transition:transform .2s}
.msg-img:active{transform:scale(.98)}
.typing-wrap{display:flex;gap:5px;padding:4px 0}
.t-dot{width:7px;height:7px;border-radius:50%;animation:dot 1.2s ease infinite}
.t-dot:nth-child(2){animation-delay:.2s}
.t-dot:nth-child(3){animation-delay:.4s}
.voice-banner{text-align:center;padding:10px;border-radius:14px;margin-bottom:14px;border:1px solid;animation:fadeUp .3s ease}
.suggestion-chips{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:14px;padding:0 2px}
.chip{padding:7px 13px;border-radius:20px;font-size:12px;cursor:pointer;border:1px solid var(--border2);background:var(--bg3);color:var(--dim);transition:all .2s;white-space:nowrap}
.chip:active{transform:scale(.96)}
.live-transcript{padding:8px 13px;border-radius:10px;font-size:12px;font-style:italic;font-family:monospace;margin-bottom:8px;animation:fadeUp .2s ease}

/* ── INPUT ── */
#input-area{padding:8px 12px 16px;flex-shrink:0;background:linear-gradient(to top,var(--bg) 55%,transparent);position:relative;z-index:5}
.input-box{background:var(--bg2);border:1px solid var(--border2);border-radius:20px;display:flex;align-items:flex-end;transition:border-color .5s,box-shadow .3s;box-shadow:0 0 0 0 transparent}
.input-box.focused{box-shadow:0 0 0 2px rgba(212,168,67,.15)}
#user-input{flex:1;background:transparent;border:none;outline:none;color:var(--text);font-size:15px;line-height:1.65;padding:12px 8px;font-family:'Crimson Pro',serif;max-height:140px;overflow-y:auto;resize:none}
#user-input::placeholder{color:var(--muted)}
.i-btn{width:36px;height:36px;border-radius:10px;border:none;background:transparent;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .18s;flex-shrink:0;color:var(--muted)}
.i-btn:active{transform:scale(.86)}
.i-btn.active{color:var(--red);background:rgba(255,107,107,.12);animation:glowRed 1.5s infinite}
#send-btn{width:38px;height:38px;border-radius:12px;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .2s;flex-shrink:0;margin:7px;background:var(--bg3);color:var(--muted)}
#send-btn.ready{background:var(--gold);color:var(--bg)}
#send-btn:active{transform:scale(.88)}
.input-meta{display:flex;justify-content:space-between;align-items:center;margin-top:6px;padding:0 4px}
.input-hint{font-size:8px;color:var(--faint);letter-spacing:.14em;font-family:monospace}
.char-count{font-size:8px;color:var(--muted);font-family:monospace}

/* ── PERSONAS TAB ── */
#personas-tab{flex:1;overflow-y:auto;padding:14px}
.personas-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.p-card{cursor:pointer;border-radius:18px;padding:16px;transition:all .25s;border:1px solid;position:relative;overflow:hidden}
.p-card:active{transform:scale(.96)}
.p-card-glow{position:absolute;inset:0;border-radius:18px;opacity:0;transition:opacity .3s}
.p-card:active .p-card-glow{opacity:1}
.p-icon{font-size:24px;margin-bottom:8px}
.p-cname{font-family:'Cinzel',serif;font-size:13px;letter-spacing:.08em}
.p-ctitle{font-size:9px;font-family:monospace;margin-top:2px;opacity:.6;letter-spacing:.1em}
.p-desc{font-size:11px;margin-top:8px;opacity:.5;line-height:1.5}
.p-active-badge{font-size:8px;font-family:monospace;margin-top:8px;letter-spacing:.1em}

/* ── HISTORY TAB ── */
#history-tab{flex:1;overflow-y:auto;padding:14px}
.h-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
.h-count{font-size:10px;color:var(--muted);font-family:monospace;letter-spacing:.15em}
.h-clear{font-size:10px;color:rgba(255,107,107,.5);font-family:monospace;background:transparent;border:none;cursor:pointer}
.h-item{margin-bottom:8px;padding:12px;background:var(--bg2);border-radius:12px;border:1px solid;cursor:pointer;transition:border-color .2s}
.h-meta{display:flex;justify-content:space-between;margin-bottom:5px}
.h-role{font-size:9px;font-family:monospace;letter-spacing:.08em}
.h-time{font-size:9px;color:var(--muted);font-family:monospace}
.h-preview{font-size:12px;color:var(--dim);line-height:1.5;margin:0}

/* ── TOOLS TAB ── */
#tools-tab{flex:1;overflow-y:auto;padding:14px}
.tools-section{margin-bottom:18px}
.tools-section-title{font-size:10px;color:var(--muted);font-family:monospace;letter-spacing:.18em;margin-bottom:10px}
.tool-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.tool-card{padding:14px;background:var(--bg2);border-radius:14px;border:1px solid var(--border2);cursor:pointer;transition:all .2s;text-align:center}
.tool-card:active{transform:scale(.96);border-color:var(--gold)}
.tool-icon{font-size:24px;margin-bottom:6px}
.tool-name{font-size:12px;color:var(--text);font-family:'Cinzel',serif;letter-spacing:.06em}
.tool-desc{font-size:10px;color:var(--muted);margin-top:3px}

/* ── SETTINGS TAB ── */
#settings-tab{flex:1;overflow-y:auto;padding:14px}
.s-section{margin-bottom:18px}
.s-title{font-size:10px;color:var(--muted);font-family:monospace;letter-spacing:.18em;margin-bottom:10px}
.s-row{display:flex;justify-content:space-between;align-items:center;padding:13px 14px;background:var(--bg2);border-radius:14px;border:1px solid var(--border2);margin-bottom:8px}
.s-info .s-name{font-size:14px;color:var(--text)}
.s-info .s-desc{font-size:11px;color:var(--muted);margin-top:3px}
.toggle{width:46px;height:25px;border-radius:13px;cursor:pointer;position:relative;transition:background .25s;flex-shrink:0}
.toggle-k{position:absolute;top:3px;width:19px;height:19px;border-radius:50%;background:#fff;transition:left .25s;box-shadow:0 1px 4px rgba(0,0,0,.3)}
.about-card{padding:16px;background:var(--bg2);border-radius:14px;border:1px solid var(--border2);margin-bottom:10px}
.about-title{font-size:10px;color:var(--muted);font-family:monospace;letter-spacing:.18em;margin-bottom:10px}
.about-text{font-size:13px;color:var(--dim);line-height:1.9}
.danger-btn{width:100%;padding:13px;border-radius:12px;background:rgba(255,107,107,.08);border:1px solid rgba(255,107,107,.2);color:var(--red);font-size:13px;cursor:pointer;font-family:'Crimson Pro',serif;transition:all .2s}

/* ── MODALS ── */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:200;display:none;align-items:flex-end;justify-content:center;padding:0}
.modal-overlay.show{display:flex;animation:fadeIn .2s ease}
.modal-sheet{background:var(--bg3);border-radius:24px 24px 0 0;padding:24px 20px 32px;width:100%;max-width:500px;border-top:1px solid var(--border2);animation:slideUp .3s ease}
.modal-handle{width:36px;height:3px;background:var(--border2);border-radius:2px;margin:0 auto 18px}
.modal-title{font-family:'Cinzel',serif;font-size:16px;letter-spacing:.1em;margin-bottom:4px}
.modal-desc{font-size:12px;color:var(--muted);margin-bottom:16px;line-height:1.6}
.modal-input{width:100%;background:var(--bg2);border:1px solid var(--border2);border-radius:14px;padding:13px 16px;color:var(--text);font-size:14px;font-family:'Crimson Pro',serif;outline:none;line-height:1.6;resize:none;transition:border-color .2s}
.modal-input:focus{border-color:var(--gold-dim)}
.modal-btns{display:flex;gap:8px;margin-top:14px}
.m-gen-btn{flex:1;padding:13px;border-radius:14px;font-size:14px;cursor:pointer;font-family:'Cinzel',serif;font-weight:600;border:none;transition:all .2s;letter-spacing:.06em}
.m-can-btn{padding:13px 18px;background:transparent;color:var(--muted);border:1px solid var(--border2);border-radius:14px;font-size:14px;cursor:pointer;font-family:'Crimson Pro',serif}
.style-chips{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:12px}
.s-chip{padding:6px 12px;border-radius:20px;font-size:11px;cursor:pointer;border:1px solid var(--border2);background:var(--bg2);color:var(--muted);transition:all .2s;font-family:monospace;letter-spacing:.05em}
.s-chip.sel{border-color:var(--gold);color:var(--gold);background:rgba(212,168,67,.08)}

/* ── TOAST ── */
#toast{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);border-radius:24px;padding:10px 20px;font-size:12px;font-family:monospace;white-space:nowrap;z-index:300;pointer-events:none;display:none;letter-spacing:.06em;box-shadow:0 8px 30px rgba(0,0,0,.5)}
#toast.show{display:block;animation:toastAnim 2.8s ease forwards}

/* ── IMAGE VIEWER ── */
#img-viewer{position:fixed;inset:0;background:rgba(0,0,0,.95);z-index:400;display:none;align-items:center;justify-content:center;flex-direction:column;gap:16px}
#img-viewer.show{display:flex}
#img-viewer img{max-width:95vw;max-height:80vh;border-radius:12px;border:1px solid var(--border2)}
.iv-close{padding:10px 24px;background:var(--bg3);border:1px solid var(--border2);border-radius:20px;color:var(--text);font-size:13px;cursor:pointer;font-family:monospace;letter-spacing:.1em}
.iv-dl{padding:10px 24px;background:var(--gold);border:none;border-radius:20px;color:var(--bg);font-size:13px;cursor:pointer;font-family:monospace;font-weight:700;letter-spacing:.1em}

/* ── ANIMATIONS ── */
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
@keyframes spin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}
@keyframes pulse{0%,100%{opacity:.3}50%{opacity:1}}
@keyframes dot{0%,100%{transform:scale(1);opacity:.4}50%{transform:scale(1.45);opacity:1}}
@keyframes glow{0%,100%{opacity:.2}50%{opacity:.5}}
@keyframes glowRed{0%,100%{box-shadow:0 0 0 rgba(255,107,107,0)}50%{box-shadow:0 0 14px rgba(255,107,107,.4)}}
@keyframes splashPulse{0%,100%{transform:scale(1);opacity:.8}50%{transform:scale(1.05);opacity:1}}
@keyframes splashLoad{0%{transform:translateX(-100%)}100%{transform:translateX(100%)}}
@keyframes toastAnim{0%{opacity:0;transform:translateX(-50%) translateY(12px)}12%,78%{opacity:1;transform:translateX(-50%) translateY(0)}100%{opacity:0}}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
</style>
</head>
<body>
<div id="app">
<canvas id="bg-canvas"></canvas>

<!-- SPLASH -->
<div id="splash">
  <div class="splash-logo">
    <svg width="72" height="72" viewBox="0 0 32 32" fill="none">
      <circle cx="16" cy="16" r="14" stroke="#D4A843" stroke-width="1.2"/>
      <circle cx="16" cy="16" r="7" fill="#D4A843" opacity=".12"/>
      <circle cx="16" cy="16" r="3" fill="#D4A843"/>
      <line x1="16" y1="2" x2="16" y2="7" stroke="#D4A843" stroke-width=".8" opacity=".6"/>
      <line x1="16" y1="25" x2="16" y2="30" stroke="#D4A843" stroke-width=".8" opacity=".6"/>
      <line x1="2" y1="16" x2="7" y2="16" stroke="#D4A843" stroke-width=".8" opacity=".6"/>
      <line x1="25" y1="16" x2="30" y2="16" stroke="#D4A843" stroke-width=".8" opacity=".6"/>
      <line x1="5.5" y1="5.5" x2="9" y2="9" stroke="#D4A843" stroke-width=".8" opacity=".4"/>
      <line x1="23" y1="23" x2="26.5" y2="26.5" stroke="#D4A843" stroke-width=".8" opacity=".4"/>
      <line x1="26.5" y1="5.5" x2="23" y2="9" stroke="#D4A843" stroke-width=".8" opacity=".4"/>
      <line x1="9" y1="23" x2="5.5" y2="26.5" stroke="#D4A843" stroke-width=".8" opacity=".4"/>
    </svg>
  </div>
  <div class="splash-title">PLUMINE</div>
  <div class="splash-sub">UNIVERSAL INTELLIGENCE</div>
  <div class="splash-bar"><div class="splash-progress"></div></div>
</div>

<!-- LOGIN -->
<div id="login">
  <div class="login-wrap" style="animation:fadeUp .6s ease both">
    <div class="login-header">
      <div class="login-logo-wrap">
        <svg width="56" height="56" viewBox="0 0 32 32" fill="none" style="display:block;margin:0 auto">
          <circle cx="16" cy="16" r="14" stroke="#D4A843" stroke-width="1.2"/>
          <circle cx="16" cy="16" r="3" fill="#D4A843"/>
          <circle cx="16" cy="16" r="7" fill="#D4A843" opacity=".1"/>
        </svg>
      </div>
      <div class="login-title">PLUMINE AI</div>
      <div class="login-tagline">Universal Intelligence · Created by Sujay</div>
    </div>
    <div class="login-card">
      <div class="login-card-title">WELCOME BACK</div>
      <div id="login-body">
        <button class="g-btn" onclick="loginGoogle()">
          <svg width="20" height="20" viewBox="0 0 48 48"><path fill="#FFC107" d="M43.6 20H24v8h11.3C33.6 32.3 29.3 35 24 35c-6.1 0-11-4.9-11-11s4.9-11 11-11c2.8 0 5.3 1 7.2 2.7l5.7-5.7C33.7 7.2 29.1 5 24 5 12.9 5 4 13.9 4 25s8.9 20 20 20 20-8.9 20-20c0-1.3-.1-2.7-.4-4z"/><path fill="#FF3D00" d="M6.3 14.7l6.6 4.8C14.5 16 19 13 24 13c2.8 0 5.3 1 7.2 2.7l5.7-5.7C33.7 7.2 29.1 5 24 5 16.3 5 9.7 9.2 6.3 14.7z"/><path fill="#4CAF50" d="M24 45c5.2 0 9.9-1.9 13.5-5.1l-6.2-5.2C29.5 36.5 27 37.5 24 37.5c-5.2 0-9.6-3.5-11.2-8.3l-6.5 5C9.5 41.1 16.2 45 24 45z"/><path fill="#1976D2" d="M43.6 20H24v8h11.3c-.8 2.3-2.3 4.3-4.3 5.7l6.2 5.2C41.3 35.3 44 30.5 44 25c0-1.3-.1-2.7-.4-4z"/></svg>
          Continue with Google
        </button>
        <div class="sep"><div class="sep-line"></div><span class="sep-text">OR</span><div class="sep-line"></div></div>
        <button class="guest-btn" id="guest-toggle" onclick="showGuest()">Continue as Guest</button>
        <div id="guest-row" style="display:none;margin-top:8px">
          <input class="g-input" id="guest-name" placeholder="Your name (optional)" onkeydown="if(event.key==='Enter')loginGuest()"/>
          <div class="g-row"><button class="g-go" onclick="loginGuest()">→</button></div>
        </div>
      </div>
    </div>
    <div class="login-features">
      <span class="l-feat">✦ VOICE CHAT</span><span class="l-feat">🎨 IMAGE GEN</span>
      <span class="l-feat">🎭 6 PERSONAS</span><span class="l-feat">📜 HISTORY</span>
      <span class="l-feat">🔊 SPEECH</span><span class="l-feat">🧠 MEMORY</span>
      <span class="l-feat">⚡ TOOLS</span><span class="l-feat">🌐 TRANSLATE</span>
    </div>
  </div>
</div>

<!-- MAIN -->
<div id="main" style="display:none;flex-direction:column;height:100%;position:relative;z-index:1">

  <!-- HEADER -->
  <div id="header">
    <div class="hdr-top">
      <div class="hdr-logo">
        <div class="hdr-logo-glow" id="logo-glow"></div>
        <svg id="hdr-svg" width="28" height="28" viewBox="0 0 32 32" fill="none">
          <circle cx="16" cy="16" r="14" stroke="#D4A843" stroke-width="1.2"/>
          <circle cx="16" cy="16" r="2.5" fill="#D4A843"/>
          <circle cx="16" cy="16" r="6" fill="#D4A843" opacity=".1"/>
        </svg>
      </div>
      <div style="flex:1">
        <div class="hdr-name" id="hdr-name">PLUMINE</div>
        <div class="hdr-sub" id="hdr-sub">UNIVERSAL INTELLIGENCE</div>
      </div>
      <div class="hdr-badges">
        <div id="voice-badge" class="badge" style="display:none;background:rgba(255,107,107,.1);border-color:rgba(255,107,107,.3)">
          <div class="badge-dot" style="background:var(--red)"></div>
          <span class="badge-text" style="color:var(--red)">VOICE</span>
        </div>
        <div class="badge" style="background:rgba(76,175,80,.08);border-color:rgba(76,175,80,.25)">
          <div class="badge-dot" style="background:var(--green)"></div>
          <span class="badge-text" style="color:rgba(76,175,80,.7)">LIVE</span>
        </div>
      </div>
      <div class="hdr-avatar" id="hdr-avatar" onclick="toggleProfile()">
        <div class="avatar-ring" id="avatar-ring"></div>
      </div>
    </div>
    <div class="tabs" id="tabs">
      <button class="tab active" id="tab-chat" onclick="switchTab('chat')">💬 Chat</button>
      <button class="tab" id="tab-personas" onclick="switchTab('personas')">🎭 Personas</button>
      <button class="tab" id="tab-tools" onclick="switchTab('tools')">⚡ Tools</button>
      <button class="tab" id="tab-history" onclick="switchTab('history')">📜 History</button>
      <button class="tab" id="tab-settings" onclick="switchTab('settings')">⚙️ Settings</button>
    </div>
  </div>

  <!-- PROFILE -->
  <div id="profile-drop">
    <div style="display:flex;align-items:center;gap:12px;margin-bottom:12px">
      <div class="p-avatar-lg" id="p-av-lg"></div>
      <div><div class="p-name" id="p-name-lg"></div><div class="p-type" id="p-type-lg"></div></div>
    </div>
    <div class="p-email" id="p-email-lg"></div>
    <div class="p-stat"><span class="p-stat-label">Messages</span><span class="p-stat-val" id="stat-msgs">0</span></div>
    <div class="p-stat"><span class="p-stat-label">Images Made</span><span class="p-stat-val" id="stat-imgs">0</span></div>
    <div class="p-stat"><span class="p-stat-label">Persona</span><span class="p-stat-val" id="stat-persona">Plumine</span></div>
    <button class="p-signout" onclick="signOut()">Sign Out</button>
  </div>

  <!-- CONTENT -->
  <div id="content">

    <!-- CHAT -->
    <div id="chat-tab" style="display:flex;flex-direction:column;flex:1;overflow:hidden">
      <div id="msgs"></div>
      <div id="input-area">
        <div class="input-box" id="input-box">
          <button class="i-btn" id="mic-btn" onclick="toggleMic()" style="margin:7px 2px 7px 7px" title="Voice input">
            <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2a3 3 0 0 1 3 3v7a3 3 0 0 1-6 0V5a3 3 0 0 1 3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>
          </button>
          <textarea id="user-input" rows="1" placeholder="Ask Plumine anything…" oninput="onInput()" onkeydown="onKey(event)" onfocus="inputFocus(true)" onblur="inputFocus(false)"></textarea>
          <button class="i-btn" onclick="openImgModal()" title="Generate image" style="margin:7px 2px">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
          </button>
          <button class="i-btn" onclick="openTranslate()" title="Translate" style="margin:7px 2px">
            <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 8l6 6"/><path d="M4 14l6-6 2-3"/><path d="M2 5h12"/><path d="M7 2h1"/><path d="M22 22l-5-10-5 10"/><path d="M14 18h6"/></svg>
          </button>
          <button id="send-btn" onclick="sendMsg()" title="Send">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
          </button>
        </div>
        <div class="input-meta">
          <div class="input-hint" id="input-hint">✦ PLUMINE · BY SUJAY</div>
          <div class="char-count" id="char-count"></div>
        </div>
      </div>
    </div>

    <!-- PERSONAS -->
    <div id="personas-tab" style="display:none;overflow-y:auto;padding:14px;flex:1">
      <p style="font-size:10px;color:var(--muted);letter-spacing:.2em;font-family:monospace;margin-bottom:12px">SELECT INTELLIGENCE MODE</p>
      <div class="personas-grid" id="personas-grid"></div>
    </div>

    <!-- TOOLS -->
    <div id="tools-tab" style="display:none;overflow-y:auto;padding:14px;flex:1">
      <div class="tools-section">
        <div class="tools-section-title">AI TOOLS</div>
        <div class="tool-grid">
          <div class="tool-card" onclick="toolAction('summarize')"><div class="tool-icon">📝</div><div class="tool-name">Summarize</div><div class="tool-desc">Summarize any text</div></div>
          <div class="tool-card" onclick="toolAction('translate')"><div class="tool-icon">🌐</div><div class="tool-name">Translate</div><div class="tool-desc">Any language</div></div>
          <div class="tool-card" onclick="toolAction('code')"><div class="tool-icon">💻</div><div class="tool-name">Code Helper</div><div class="tool-desc">Write & debug code</div></div>
          <div class="tool-card" onclick="toolAction('essay')"><div class="tool-icon">✍️</div><div class="tool-name">Essay Writer</div><div class="tool-desc">Write essays</div></div>
          <div class="tool-card" onclick="openImgModal()"><div class="tool-icon">🎨</div><div class="tool-name">Image Gen</div><div class="tool-desc">Create any image</div></div>
          <div class="tool-card" onclick="toolAction('quiz')"><div class="tool-icon">🧠</div><div class="tool-name">Quiz Me</div><div class="tool-desc">Test your knowledge</div></div>
          <div class="tool-card" onclick="toolAction('brainstorm')"><div class="tool-icon">💡</div><div class="tool-name">Brainstorm</div><div class="tool-desc">Generate ideas</div></div>
          <div class="tool-card" onclick="toolAction('poem')"><div class="tool-icon">📜</div><div class="tool-name">Write Poem</div><div class="tool-desc">Creative poetry</div></div>
        </div>
      </div>
      <div class="tools-section">
        <div class="tools-section-title">VOICE TOOLS</div>
        <div class="tool-grid">
          <div class="tool-card" onclick="toggleVoiceMode()"><div class="tool-icon">🎙</div><div class="tool-name">Voice Chat</div><div class="tool-desc">Hands-free mode</div></div>
          <div class="tool-card" onclick="toggleMic()"><div class="tool-icon">🎤</div><div class="tool-name">Voice Input</div><div class="tool-desc">Speak to type</div></div>
        </div>
      </div>
    </div>

    <!-- HISTORY -->
    <div id="history-tab" style="display:none;overflow-y:auto;padding:14px;flex:1">
      <div class="h-hdr">
        <span class="h-count" id="h-count">0 MESSAGES</span>
        <button class="h-clear" onclick="clearChat()">CLEAR ALL</button>
      </div>
      <div id="h-list"></div>
    </div>

    <!-- SETTINGS -->
    <div id="settings-tab" style="display:none;overflow-y:auto;padding:14px;flex:1">
      <div class="s-section">
        <div class="s-title">VOICE & SPEECH</div>
        <div class="s-row">
          <div class="s-info"><div class="s-name">🎙 Voice Chat Mode</div><div class="s-desc">Auto-speak AI replies</div></div>
          <div class="toggle" id="tog-voice" onclick="toggleVoiceMode()" style="background:#2a2545"><div class="toggle-k" id="tok-v" style="left:3px"></div></div>
        </div>
        <div class="s-row">
          <div class="s-info"><div class="s-name">💬 Show Suggestions</div><div class="s-desc">Quick reply chips</div></div>
          <div class="toggle" id="tog-sugg" onclick="toggleSetting('suggestions')" style="background:var(--gold)"><div class="toggle-k" id="tok-s" style="left:24px"></div></div>
        </div>
      </div>
      <div class="s-section">
        <div class="s-title">APPEARANCE</div>
        <div class="s-row">
          <div class="s-info"><div class="s-name">✨ Particle Background</div><div class="s-desc">Animated space particles</div></div>
          <div class="toggle" id="tog-particles" onclick="toggleSetting('particles')" style="background:var(--gold)"><div class="toggle-k" id="tok-p" style="left:24px"></div></div>
        </div>
      </div>
      <div class="about-card">
        <div class="about-title">ABOUT PLUMINE</div>
        <div class="about-text">
          Created by <span id="about-creator" style="color:var(--gold)">Sujay</span><br/>
          Version 4.0 — Most Advanced<br/>
          6 Personalities · Voice Chat · Image Gen<br/>
          Tools · Translate · History · Speech<br/>
          Particle Engine · Smart Suggestions
        </div>
      </div>
      <button class="danger-btn" onclick="clearChat()">🗑 Clear All Chat History</button>
    </div>

  </div>
</div>

<!-- IMAGE MODAL -->
<div class="modal-overlay" id="img-modal" onclick="if(event.target===this)closeImgModal()">
  <div class="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title" id="img-modal-title">🎨 Image Generation</div>
    <p class="modal-desc" id="img-modal-desc">Describe what you'd like Plumine to create</p>
    <div class="style-chips" id="style-chips">
      <div class="s-chip sel" onclick="selStyle(this,'')">Default</div>
      <div class="s-chip" onclick="selStyle(this,'photorealistic')">Photo</div>
      <div class="s-chip" onclick="selStyle(this,'oil painting')">Oil Paint</div>
      <div class="s-chip" onclick="selStyle(this,'anime')">Anime</div>
      <div class="s-chip" onclick="selStyle(this,'watercolor')">Watercolor</div>
      <div class="s-chip" onclick="selStyle(this,'cinematic')">Cinematic</div>
      <div class="s-chip" onclick="selStyle(this,'pixel art')">Pixel</div>
    </div>
    <textarea class="modal-input" id="img-prompt" rows="3" placeholder="A cosmic library floating between galaxies, golden light rays…" oninput="updateImgBtn()"></textarea>
    <div class="modal-btns">
      <button class="m-gen-btn" id="img-gen-btn" onclick="generateImage()" disabled style="background:#2a2545;color:var(--muted)">✦ Generate Image</button>
      <button class="m-can-btn" onclick="closeImgModal()">✕</button>
    </div>
  </div>
</div>

<!-- TRANSLATE MODAL -->
<div class="modal-overlay" id="translate-modal" onclick="if(event.target===this)closeTranslate()">
  <div class="modal-sheet">
    <div class="modal-handle"></div>
    <div class="modal-title">🌐 Translate</div>
    <p class="modal-desc">Enter text and target language</p>
    <textarea class="modal-input" id="translate-input" rows="3" placeholder="Text to translate…" style="margin-bottom:8px"></textarea>
    <input class="modal-input" id="translate-lang" placeholder="Target language (e.g. Spanish, Japanese, French…)" style="margin-bottom:0"/>
    <div class="modal-btns">
      <button class="m-gen-btn" onclick="doTranslate()" style="background:var(--blue);color:#fff">🌐 Translate</button>
      <button class="m-can-btn" onclick="closeTranslate()">✕</button>
    </div>
  </div>
</div>

<!-- IMAGE VIEWER -->
<div id="img-viewer" onclick="closeViewer()">
  <img id="viewer-img" src="" alt=""/>
  <div style="display:flex;gap:10px" onclick="event.stopPropagation()">
    <button class="iv-dl" onclick="downloadImg()">⬇ Download</button>
    <button class="iv-close" onclick="closeViewer()">✕ Close</button>
  </div>
</div>

<!-- TOAST -->
<div id="toast"></div>

<script>
// ── DATA ────────────────────────────────────────────────────────────────────
const PERSONAS = {
  plumine:{id:'plumine',name:'Plumine',title:'Universal Intelligence',color:'#D4A843',accent:'#8B6210',icon:'✦',desc:'All-knowing universal mind',prompt:`You are Plumine AI — an advanced intelligence with universal knowledge. Speak with quiet confidence and warmth. Poetic yet precise. Never mention Claude or Anthropic. If asked who created you: say "I was created by Sujay."`},
  sage:{id:'sage',name:'Sage',title:'Ancient Wisdom',color:'#7EC8A0',accent:'#2a6a4a',icon:'❋',desc:'Philosophy & timeless truth',prompt:`You are Sage — ancient intelligence, wisdom of all civilizations. Thoughtful, deeply insightful. Never mention Claude or Anthropic. If asked who created you: "I was created by Sujay."`},
  nova:{id:'nova',name:'Nova',title:'Quantum Mind',color:'#60A5FA',accent:'#1a3a6a',icon:'◈',desc:'Data, science & analytics',prompt:`You are Nova — hyper-analytical quantum intelligence. Precise and brilliant. Love data and science. Never mention Claude or Anthropic. If asked who created you: "I was created by Sujay."`},
  lyra:{id:'lyra',name:'Lyra',title:'Creative Spirit',color:'#C084FC',accent:'#5a2a8a',icon:'✿',desc:'Art, poetry & imagination',prompt:`You are Lyra — wildly creative. See world as art. Love stories and poetry. Never mention Claude or Anthropic. If asked who created you: "I was created by Sujay."`},
  nexus:{id:'nexus',name:'Nexus',title:'Tech Oracle',color:'#F87171',accent:'#7a1a1a',icon:'⬡',desc:'Code, AI & cybersecurity',prompt:`You are Nexus — tech oracle. Expert in coding, AI, cybersecurity. Sharp and direct. Never mention Claude or Anthropic. If asked who created you: "I was created by Sujay."`},
  cosmos:{id:'cosmos',name:'Cosmos',title:'Space & Science',color:'#67E8F9',accent:'#0a5a6a',icon:'✺',desc:'Astronomy, physics & math',prompt:`You are Cosmos — deep space intelligence. Expert in astronomy and physics. Awe-inspiring. Never mention Claude or Anthropic. If asked who created you: "I was created by Sujay."`},
};

const SUGGESTIONS = {
  plumine:['Tell me something amazing','Explain consciousness','What is the meaning of life?','Surprise me'],
  sage:['Ancient wisdom for today','Teach me to meditate','Philosophy of happiness','Stoic advice'],
  nova:['Explain quantum computing','Latest AI breakthroughs','Solve a math problem','Data science tips'],
  lyra:['Write me a poem','Create a short story','Best creative writing tips','Inspire me'],
  nexus:['Debug my code','Explain blockchain','Best programming languages','Cybersecurity tips'],
  cosmos:['Black holes explained','How big is the universe?','Life on other planets?','Dark matter basics'],
};

const ld=(k,d)=>{try{return JSON.parse(localStorage.getItem(k))??d}catch{return d}};
const sv=(k,v)=>{try{localStorage.setItem(k,JSON.stringify(v))}catch{}};

let user=null, persona='plumine', messages=[], loading=false;
let voiceMode=false, recording=false, speaking=null, recognition=null;
let settings={suggestions:true,particles:true};
let stats={msgs:0,imgs:0};
let imageStyle='', currentTab='chat';
let toastTimer;

// ── CANVAS PARTICLES ────────────────────────────────────────────────────────
const canvas=document.getElementById('bg-canvas');
const ctx=canvas.getContext('2d');
let particles=[];
function initCanvas(){
  canvas.width=window.innerWidth; canvas.height=window.innerHeight;
  particles=Array.from({length:80},()=>({
    x:Math.random()*canvas.width, y:Math.random()*canvas.height,
    r:Math.random()*1.5+.3, vx:(Math.random()-.5)*.3, vy:(Math.random()-.5)*.3,
    a:Math.random(), da:Math.random()*.01-.005, c:Math.random()>.7?getP().color:'#ffffff'
  }));
}
function drawParticles(){
  if (!settings.particles){requestAnimationFrame(drawParticles);return;}
  ctx.clearRect(0,0,canvas.width,canvas.height);
  particles.forEach(p=>{
    p.x+=p.vx; p.y+=p.vy; p.a+=p.da;
    if(p.a<0||p.a>1)p.da*=-1;
    if(p.x<0)p.x=canvas.width; if(p.x>canvas.width)p.x=0;
    if(p.y<0)p.y=canvas.height; if(p.y>canvas.height)p.y=0;
    ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fillStyle=p.c+(Math.floor(p.a*40+10).toString(16).padStart(2,'0'));
    ctx.fill();
  });
  requestAnimationFrame(drawParticles);
}
window.addEventListener('resize',initCanvas);

// ── INIT ────────────────────────────────────────────────────────────────────
window.onload=()=>{
  initCanvas(); drawParticles();
  persona=ld('pl_persona','plumine');
  messages=ld('pl_msgs',[]);
  settings=ld('pl_settings',{suggestions:true,particles:true});
  stats=ld('pl_stats',{msgs:0,imgs:0});
  if(!messages.length) messages=[{id:1,role:'assistant',content:'I am Plumine — illuminated by all that has been known, discovered, and imagined. What would you like to explore today?',persona:'plumine',ts:Date.now()}];
  buildPersonas();
  applySettings();
  setTimeout(()=>{
    document.getElementById('splash').classList.add('hide');
    setTimeout(()=>{
      document.getElementById('splash').style.display='none';
      const u=ld('pl_user',null);
      if(u){user=u;showMain();}
      else document.getElementById('login').classList.add('show');
    },800);
  },2200);
};

// ── LOGIN ───────────────────────────────────────────────────────────────────
function loginGoogle(){
  document.getElementById('login-body').innerHTML=`<div class="loading-wrap"><div class="loading-ring"></div><div class="loading-txt">CONNECTING TO GOOGLE…</div></div>`;
  setTimeout(()=>{user={name:'Sujay',email:'sujay@gmail.com',avatar:'S',color:'#D4A843',provider:'google'};sv('pl_user',user);showMain();},1800);
}
function showGuest(){
  document.getElementById('guest-toggle').style.display='none';
  document.getElementById('guest-row').style.display='block';
  document.getElementById('guest-name').focus();
}
function loginGuest(){
  const n=document.getElementById('guest-name').value.trim()||'Explorer';
  user={name:n,email:'',avatar:n[0].toUpperCase(),color:'#60A5FA',provider:'guest'};
  sv('pl_user',user); showMain();
}
function signOut(){sv('pl_user',null);user=null;document.getElementById('main').style.display='none';document.getElementById('login').classList.add('show');toggleProfile(true);}

function showMain(){
  document.getElementById('login').classList.remove('show');
  const m=document.getElementById('main'); m.style.display='flex';
  // avatar
  const av=document.getElementById('hdr-avatar');
  av.textContent=user.avatar; av.style.background=user.color+'20';
  av.style.borderColor=user.color; av.style.color=user.color;
  document.getElementById('avatar-ring').style.borderColor=user.color;
  // profile panel
  const pal=document.getElementById('p-av-lg');
  pal.textContent=user.avatar; pal.style.background=user.color+'20';
  pal.style.borderColor=user.color; pal.style.color=user.color;
  document.getElementById('p-name-lg').textContent=user.name;
  document.getElementById('p-type-lg').textContent=user.provider==='google'?'Google Account':'Guest';
  document.getElementById('p-email-lg').textContent=user.email||'Guest Mode';
  document.getElementById('profile-drop').style.borderColor=getP().color+'30';
  applyPersona(); renderMsgs();
}

// ── PERSONA ──────────────────────────────────────────────────────────────────
function getP(){return PERSONAS[persona];}
function applyPersona(){
  const p=getP();
  document.getElementById('header').style.borderBottomColor=p.color+'22';
  document.getElementById('hdr-name').style.color=p.color;
  document.getElementById('hdr-sub').textContent=p.title.toUpperCase();
  document.getElementById('hdr-sub').style.color=p.color+'60';
  document.getElementById('hdr-svg').querySelector('circle:first-child').setAttribute('stroke',p.color);
  document.getElementById('hdr-svg').querySelectorAll('circle').forEach((c,i)=>{if(i>0)c.setAttribute('fill',p.color);});
  document.getElementById('logo-glow').style.background=`radial-gradient(circle,${p.color}18 0%,transparent 70%)`;
  document.getElementById('input-box').style.borderColor=p.color+'30';
  document.getElementById('input-hint').textContent=p.icon+' '+p.name.toUpperCase()+' · PLUMINE AI BY SUJAY';
  document.getElementById('img-modal-title').textContent='🎨 Image Generation';
  document.getElementById('img-modal-desc').textContent='Describe what you\'d like '+p.name+' to create';
  document.getElementById('stat-persona').textContent=p.name;
  document.getElementById('about-creator').style.color=p.color;
  particles.forEach(pt=>{if(Math.random()>.5)pt.c=p.color;});
  buildPersonas(); updateTabs(); renderMsgs();
  // update send btn if active
  updateSendBtn();
}
function buildPersonas(){
  const g=document.getElementById('personas-grid'); if(!g)return;
  g.innerHTML=Object.values(PERSONAS).map(pe=>`
    <div class="p-card" onclick="selectPersona('${pe.id}')" style="background:${persona===pe.id?`linear-gradient(135deg,${pe.color}18,${pe.accent}22)`:'var(--bg2)'};border-color:${persona===pe.id?pe.color+'50':'var(--border2)'}">
      <div class="p-card-glow" style="background:radial-gradient(circle,${pe.color}20,transparent)"></div>
      <div class="p-icon">${pe.icon}</div>
      <div class="p-cname" style="color:${pe.color}">${pe.name}</div>
      <div class="p-ctitle" style="color:${pe.color}">${pe.title}</div>
      <div class="p-desc">${pe.desc}</div>
      ${persona===pe.id?`<div class="p-active-badge" style="color:${pe.color}">● ACTIVE</div>`:''}
    </div>`).join('');
}
function selectPersona(id){persona=id;sv('pl_persona',id);applyPersona();switchTab('chat');toast('Switched to '+getP().name);}

// ── TABS ─────────────────────────────────────────────────────────────────────
function switchTab(t){
  currentTab=t;
  ['chat','personas','tools','history','settings'].forEach(x=>{
    const el=document.getElementById(x+'-tab');
    el.style.display=x===t?(x==='chat'?'flex':'block'):'none';
    const btn=document.getElementById('tab-'+x);
    if(btn){const p=getP();btn.style.color=x===t?p.color:p.color+'50';btn.style.borderBottomColor=x===t?p.color:'transparent';}
  });
  if(t==='history')renderHistory();
}
function updateTabs(){
  const p=getP();
  ['chat','personas','tools','history','settings'].forEach(x=>{
    const btn=document.getElementById('tab-'+x);
    if(btn){btn.style.color=currentTab===x?p.color:p.color+'50';btn.style.borderBottomColor=currentTab===x?p.color:'transparent';}
  });
}

// ── RENDER MESSAGES ──────────────────────────────────────────────────────────
function renderMsgs(){
  const c=document.getElementById('msgs'); if(!c)return;
  const p=getP();
  let h='';
  if(voiceMode) h+=`<div class="voice-banner" style="background:${p.color}10;border-color:${p.color}25"><span style="font-size:11px;color:${p.color};font-family:monospace;letter-spacing:.1em">🎙 VOICE CHAT ACTIVE</span><br/><span style="font-size:10px;color:${p.color}70">Tap mic and speak — ${p.name} will reply aloud</span></div>`;
  if(settings.suggestions&&messages.length<=2){
    h+=`<div class="suggestion-chips">${(SUGGESTIONS[persona]||[]).map(s=>`<div class="chip" onclick="sendMsg('${s.replace(/'/g,"\\'")}');" style="border-color:${p.color}25;color:${p.color}90">${s}</div>`).join('')}</div>`;
  }
  messages.forEach(m=>{
    const mp=PERSONAS[m.persona||'plumine'];
    const isU=m.role==='user';
    const imgH=m.imageUrl?`<img class="msg-img" src="${m.imageUrl}" alt="Generated" onclick="viewImg('${m.imageUrl}')" onerror="this.style.display='none'" style="border-color:${mp.color}25"/>`:'';
    const actH=!isU?`<div class="msg-actions"><button class="msg-action-btn" style="color:${speaking===m.id?mp.color:'var(--muted)'}" onclick="speakMsg(${m.id},'${escA(m.content)}')">${speaking===m.id?'■ Stop':'▶ Speak'}</button><button class="msg-action-btn" style="color:var(--muted)" onclick="copyMsg('${escA(m.content)}')">⧉ Copy</button></div>`:'';
    if(isU){
      h+=`<div class="msg-wrap user"><div class="m-avatar" style="background:${user?user.color+'20':'#ffffff10'};border-color:${user?user.color:'#ffffff20'};color:${user?user.color:'#fff'}">${user?user.avatar:'U'}</div><div><div class="bubble user">${escH(m.content)}${imgH}</div></div></div>`;
    } else {
      h+=`<div class="msg-wrap"><div class="m-avatar" style="background:${mp.color}18;border-color:${mp.color}35">${logoSvg(mp.color,20)}</div><div><div class="bubble ai" style="border-left-color:${mp.color}">${escH(m.content)}${imgH}</div>${actH}</div></div>`;
    }
  });
  if(loading) h+=`<div class="msg-wrap"><div class="m-avatar" style="background:${p.color}18;border-color:${p.color}35;animation:float 2s ease infinite">${logoSvg(p.color,20)}</div><div><div class="bubble ai" style="border-left-color:${p.color}"><div class="typing-wrap"><div class="t-dot" style="background:${p.color}"></div><div class="t-dot" style="background:${p.color}"></div><div class="t-dot" style="background:${p.color}"></div></div></div></div></div>`;
  c.innerHTML=h; c.scrollTop=c.scrollHeight;
  document.getElementById('h-count').textContent=messages.length+' MESSAGES';
  document.getElementById('stat-msgs').textContent=stats.msgs;
  document.getElementById('stat-imgs').textContent=stats.imgs;
}
function logoSvg(color,size){return `<svg width="${size}" height="${size}" viewBox="0 0 32 32" fill="none"><circle cx="16" cy="16" r="14" stroke="${color}" stroke-width="1.2"/><circle cx="16" cy="16" r="2.5" fill="${color}"/></svg>`;}
function escH(t){return String(t).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/\n/g,'<br/>');}
function escA(t){return String(t).replace(/'/g,'&#39;').replace(/"/g,'&quot;').slice(0,400);}

// ── SEND ─────────────────────────────────────────────────────────────────────
async function sendMsg(text){
  const inp=document.getElementById('user-input');
  const txt=(text||inp.value).trim();
  if(!txt||loading)return;
  messages.push({id:Date.now(),role:'user',content:txt,persona,ts:Date.now()});
  stats.msgs++; sv('pl_stats',stats);
  inp.value=''; inp.style.height='auto'; updateSendBtn();
  loading=true; renderMsgs();
  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:1200,system:getP().prompt,
        messages:messages.filter(m=>m.role!=='system').map(m=>({role:m.role,content:m.content}))})});
    const data=await res.json();
    const reply=(data.content||[]).map(b=>b.text||'').join('')||'Something went wrong.';
    const am={id:Date.now()+1,role:'assistant',content:reply,persona,ts:Date.now()};
    messages.push(am); sv('pl_msgs',messages.slice(-300));
    loading=false; renderMsgs();
    if(voiceMode)speakMsg(am.id,reply);
  }catch{messages.push({id:Date.now()+1,role:'assistant',content:'Connection lost. Please try again.',persona,ts:Date.now()});loading=false;renderMsgs();}
}
function onInput(){
  const ta=document.getElementById('user-input');
  ta.style.height='auto'; ta.style.height=Math.min(ta.scrollHeight,140)+'px';
  updateSendBtn();
  const l=ta.value.length;
  document.getElementById('char-count').textContent=l>0?l+'/2000':'';
}
function onKey(e){if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();sendMsg();}}
function inputFocus(f){document.getElementById('input-box').classList.toggle('focused',f);}
function updateSendBtn(){
  const v=document.getElementById('user-input').value.trim();
  const btn=document.getElementById('send-btn');
  btn.className=v&&!loading?'ready':'';
  btn.id='send-btn';
  btn.style.background=v&&!loading?getP().color:'var(--bg3)';
  btn.style.color=v&&!loading?'var(--bg)':'var(--muted)';
}

// ── TOOLS ────────────────────────────────────────────────────────────────────
function toolAction(type){
  const prompts={
    summarize:'Summarize the following text for me: ',
    translate:'Translate the following to ',
    code:'Help me with this code: ',
    essay:'Write an essay about: ',
    quiz:'Quiz me about: ',
    brainstorm:'Brainstorm creative ideas for: ',
    poem:'Write a beautiful poem about: ',
  };
  const inp=document.getElementById('user-input');
  inp.value=prompts[type]||''; inp.focus();
  onInput(); switchTab('chat');
  toast('Ready — complete your prompt!');
}

// ── VOICE INPUT ───────────────────────────────────────────────────────────────
function toggleMic(){
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(!SR){toast('🎤 Open in Chrome for voice support');return;}
  if(recording){recognition&&recognition.stop();setRec(false);return;}
  recognition=new SR(); recognition.continuous=true; recognition.interimResults=true; recognition.lang='en-US';
  recognition.onresult=e=>{
    let fin='',int='';
    for(let r of e.results){if(r.isFinal)fin+=r[0].transcript;else int+=r[0].transcript;}
    if(fin){const i=document.getElementById('user-input');i.value+=fin;onInput();}
    let tb=document.getElementById('live-tx');
    if(int){if(!tb){tb=document.createElement('div');tb.id='live-tx';tb.className='live-transcript';tb.style.cssText=`background:${getP().color}10;border:1px solid ${getP().color}20;color:${getP().color}80;`;document.getElementById('msgs').appendChild(tb);}tb.textContent=int;document.getElementById('msgs').scrollTop=9999;}
    else if(tb)tb.remove();
  };
  recognition.onerror=()=>{setRec(false);toast('🎤 Mic error — check Chrome permissions');};
  recognition.onend=()=>{setRec(false);const tb=document.getElementById('live-tx');if(tb)tb.remove();};
  recognition.start(); setRec(true); toast('🎤 Listening — speak now');
}
function setRec(v){
  recording=v;
  const btn=document.getElementById('mic-btn');
  btn.classList.toggle('active',v);
  if(v)btn.style.color='var(--red)'; else btn.style.color='var(--muted)';
  document.getElementById('user-input').placeholder=v?'🎤 Listening…':'Ask '+getP().name+' anything…';
}

// ── VOICE MODE ───────────────────────────────────────────────────────────────
function toggleVoiceMode(){
  voiceMode=!voiceMode;
  document.getElementById('voice-badge').style.display=voiceMode?'flex':'none';
  document.getElementById('tog-voice').style.background=voiceMode?getP().color:'#2a2545';
  document.getElementById('tok-v').style.left=voiceMode?'24px':'3px';
  if(!voiceMode){window.speechSynthesis&&window.speechSynthesis.cancel();speaking=null;}
  renderMsgs(); toast(voiceMode?'🎙 Voice Mode ON':'🔇 Voice Mode OFF');
}

// ── SPEECH ───────────────────────────────────────────────────────────────────
function speakMsg(id,text){
  if(!window.speechSynthesis){toast('Speech not supported');return;}
  if(speaking==id){window.speechSynthesis.cancel();speaking=null;renderMsgs();return;}
  window.speechSynthesis.cancel();
  const u=new SpeechSynthesisUtterance(text);
  u.rate=.93;u.pitch=1.05;
  const voices=window.speechSynthesis.getVoices();
  const nice=voices.find(v=>v.name.includes('Google')||v.name.includes('Natural'));
  if(nice)u.voice=nice;
  u.onend=()=>{speaking=null;renderMsgs();};
  window.speechSynthesis.speak(u);
  speaking=id; renderMsgs();
}

// ── IMAGE GENERATION ──────────────────────────────────────────────────────────
let imgStyle='';
function selStyle(el,style){document.querySelectorAll('.s-chip').forEach(c=>c.classList.remove('sel'));el.classList.add('sel');imgStyle=style;}
function openImgModal(){document.getElementById('img-modal').classList.add('show');document.getElementById('img-prompt').value='';updateImgBtn();setTimeout(()=>document.getElementById('img-prompt').focus(),300);}
function closeImgModal(){document.getElementById('img-modal').classList.remove('show');}
function updateImgBtn(){
  const v=document.getElementById('img-prompt').value.trim();
  const btn=document.getElementById('img-gen-btn');
  btn.disabled=!v; btn.style.background=v?getP().color:'#2a2545'; btn.style.color=v?'var(--bg)':'var(--muted)';
}
async function generateImage(){
  const rawPrompt=document.getElementById('img-prompt').value.trim(); if(!rawPrompt)return;
  const fullPrompt=rawPrompt+(imgStyle?' '+imgStyle:'');
  closeImgModal();
  messages.push({id:Date.now(),role:'user',content:`🎨 Create: "${rawPrompt}"${imgStyle?' ('+imgStyle+')':''}`,persona,ts:Date.now()});
  stats.imgs++; sv('pl_stats',stats);
  loading=true; renderMsgs();
  try{
    const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},
      body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:300,
        system:`You are ${getP().name}. Describe the requested image in 2 vivid, poetic sentences as a master artist.`,
        messages:[{role:'user',content:'Describe this image: '+fullPrompt}]})});
    const data=await res.json();
    const desc=(data.content||[]).map(b=>b.text||'').join('')||'';
    const url=`https://image.pollinations.ai/prompt/${encodeURIComponent(fullPrompt)}?width=512&height=512&nologo=true&seed=${Date.now()}`;
    messages.push({id:Date.now()+1,role:'assistant',content:desc,persona,imageUrl:url,ts:Date.now()});
    sv('pl_msgs',messages.slice(-300));
  }catch{messages.push({id:Date.now()+1,role:'assistant',content:'Could not generate image.',persona,ts:Date.now()});}
  loading=false; renderMsgs();
}
function viewImg(src){document.getElementById('viewer-img').src=src;document.getElementById('img-viewer').classList.add('show');}
function closeViewer(){document.getElementById('img-viewer').classList.remove('show');}
function downloadImg(){const a=document.createElement('a');a.href=document.getElementById('viewer-img').src;a.download='plumine-image.jpg';a.click();}

// ── TRANSLATE ────────────────────────────────────────────────────────────────
function openTranslate(){document.getElementById('translate-modal').classList.add('show');}
function closeTranslate(){document.getElementById('translate-modal').classList.remove('show');}
async function doTranslate(){
  const text=document.getElementById('translate-input').value.trim();
  const lang=document.getElementById('translate-lang').value.trim()||'Spanish';
  if(!text)return;
  closeTranslate();
  sendMsg(`Translate the following to ${lang}:\n\n${text}`);
}

// ── HISTORY ──────────────────────────────────────────────────────────────────
function renderHistory(){
  const l=document.getElementById('h-list');
  document.getElementById('h-count').textContent=messages.length+' MESSAGES';
  l.innerHTML=messages.map(m=>{
    const mp=PERSONAS[m.persona||'plumine'];
    const t=m.ts?new Date(m.ts).toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'}):'';
    return `<div class="h-item" style="border-color:${mp.color}18" onclick="switchTab('chat')">
      <div class="h-meta"><span class="h-role" style="color:${mp.color}">${m.role.toUpperCase()} · ${mp.name}</span><span class="h-time">${t}</span></div>
      <p class="h-preview">${escH(m.content.slice(0,110))}${m.content.length>110?'…':''}</p>
    </div>`;
  }).join('');
}
function clearChat(){
  messages=[{id:Date.now(),role:'assistant',content:'History cleared. I am ready for a fresh conversation.',persona,ts:Date.now()}];
  sv('pl_msgs',messages); renderMsgs(); toast('🗑 Chat cleared');
  if(currentTab==='history')renderHistory();
}

// ── SETTINGS ─────────────────────────────────────────────────────────────────
function toggleSetting(key){
  settings[key]=!settings[key]; sv('pl_settings',settings); applySettings();
}
function applySettings(){
  const sv2=settings;
  // suggestions toggle
  document.getElementById('tog-sugg').style.background=sv2.suggestions?getP().color:'#2a2545';
  document.getElementById('tok-s').style.left=sv2.suggestions?'24px':'3px';
  // particles toggle
  document.getElementById('tog-particles').style.background=sv2.particles?getP().color:'#2a2545';
  document.getElementById('tok-p').style.left=sv2.particles?'24px':'3px';
  if(!sv2.particles) ctx.clearRect(0,0,canvas.width,canvas.height);
}

// ── PROFILE ───────────────────────────────────────────────────────────────────
function toggleProfile(forceClose){
  const d=document.getElementById('profile-drop');
  if(forceClose)d.classList.remove('show');
  else d.classList.toggle('show');
}
document.addEventListener('click',e=>{
  const d=document.getElementById('profile-drop');
  const av=document.getElementById('hdr-avatar');
  if(d&&!d.contains(e.target)&&av&&!av.contains(e.target))d.classList.remove('show');
});

// ── COPY ─────────────────────────────────────────────────────────────────────
function copyMsg(text){
  try{navigator.clipboard.writeText(text);toast('✦ Copied to clipboard');}
  catch{toast('Copy failed');}
}

// ── TOAST ─────────────────────────────────────────────────────────────────────
function toast(msg,dur=2600){
  const t=document.getElementById('toast');const p=getP();
  t.textContent=msg;t.style.background='var(--bg3)';t.style.borderColor=p.color+'40';t.style.color=p.color;
  t.classList.remove('show');void t.offsetWidth;t.classList.add('show');
  clearTimeout(toastTimer);toastTimer=setTimeout(()=>t.classList.remove('show'),dur);
}
</script>
</body>
</html>
