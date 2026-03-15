<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ZombieSlayer</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;600;700&family=Bebas+Neue&family=Space+Mono&display=swap');
*{margin:0;padding:0;box-sizing:border-box;}
body{overflow:hidden;background:#000;font-family:'Rajdhani',sans-serif;user-select:none;}
#c{position:fixed;top:0;left:0;width:100%;height:100%;z-index:1;}
.overlay{position:fixed;top:0;left:0;width:100%;height:100%;z-index:20;display:none;}
.overlay.show{display:flex;}
#menuScreen{background:#06060f;flex-direction:column;z-index:30;}
.menu-bg{position:absolute;inset:0;background:radial-gradient(ellipse at 20% 50%,rgba(255,40,0,.15) 0%,transparent 60%),radial-gradient(ellipse at 80% 20%,rgba(60,0,200,.12) 0%,transparent 50%);pointer-events:none;}
.scanlines{position:absolute;inset:0;background:repeating-linear-gradient(transparent,transparent 2px,rgba(0,0,0,.08) 2px,rgba(0,0,0,.08) 4px);pointer-events:none;}
.menu-inner{position:relative;z-index:2;display:flex;flex-direction:column;width:100%;height:100%;}
.menu-header{text-align:center;padding:30px 20px 0;}
.menu-header h1{font-family:'Bebas Neue',sans-serif;font-size:80px;letter-spacing:10px;background:linear-gradient(180deg,#ff6a00,#ff2020 60%,#990000);-webkit-background-clip:text;-webkit-text-fill-color:transparent;filter:drop-shadow(0 0 28px rgba(255,60,0,.5));line-height:1;}
.subtitle{font-size:13px;letter-spacing:5px;color:#ff4020;font-family:'Space Mono',monospace;margin-top:4px;}
/* Settings */
.settings-wrap{max-width:500px;margin:0 auto;padding-top:10px;}
.setting-row{display:flex;align-items:center;gap:16px;padding:14px 0;border-bottom:1px solid rgba(255,255,255,.05);}
.setting-label{font-size:13px;font-weight:700;color:#bbb;letter-spacing:1px;width:130px;flex-shrink:0;font-family:'Rajdhani',sans-serif;text-transform:uppercase;}
.setting-value{font-size:12px;color:#ff8040;font-family:'Space Mono',monospace;min-width:40px;text-align:right;}
.setting-slider{flex:1;-webkit-appearance:none;height:4px;background:rgba(255,255,255,.1);border-radius:2px;outline:none;}
.setting-slider::-webkit-slider-thumb{-webkit-appearance:none;width:14px;height:14px;border-radius:50%;background:linear-gradient(135deg,#ff8800,#ff4400);cursor:pointer;}
/* HUD skin progress */
.hud-skin-prog{position:absolute;bottom:62px;left:24px;display:flex;align-items:center;gap:7px;}
.hud-spbar-bg{width:120px;height:3px;background:rgba(255,255,255,.08);border-radius:2px;overflow:hidden;}
.hud-spbar-fill{height:100%;background:linear-gradient(90deg,#ff8800,#ffd700);border-radius:2px;transition:width .4s;}
.hud-sp-label{font-size:9px;color:#555;font-family:'Space Mono',monospace;}
.cred-disp{display:inline-flex;align-items:center;gap:7px;background:rgba(255,200,0,.07);border:1px solid rgba(255,200,0,.22);border-radius:4px;padding:4px 14px;font-size:14px;color:#ffd700;font-weight:700;margin-top:8px;}
.nav-tabs{display:flex;justify-content:center;gap:2px;padding:18px 40px 0;position:relative;z-index:2;}
.nav-tab{padding:11px 32px;background:transparent;border:none;border-bottom:3px solid transparent;color:#444;font-size:13px;font-weight:700;letter-spacing:2px;cursor:pointer;font-family:'Rajdhani',sans-serif;transition:all .2s;text-transform:uppercase;}
.nav-tab:hover{color:#aaa;}
.nav-tab.active{color:#ff6a00;border-bottom-color:#ff6a00;}
.tab-content{flex:1;overflow-y:auto;padding:20px 40px;display:none;position:relative;z-index:2;}
.tab-content.active{display:block;}
.tab-content::-webkit-scrollbar{width:4px;}
.tab-content::-webkit-scrollbar-thumb{background:rgba(255,100,0,.3);border-radius:2px;}
.main-play-area{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:260px;gap:18px;}
.btn-play{padding:18px 80px;background:linear-gradient(90deg,#ff4400,#ff8800);border:none;border-radius:4px;color:#fff;font-family:'Bebas Neue',sans-serif;font-size:32px;letter-spacing:5px;cursor:pointer;transition:all .2s;}
.btn-play:hover{transform:scale(1.04);box-shadow:0 0 40px rgba(255,100,0,.5);}
.controls-hint{color:#333;font-size:12px;font-family:'Space Mono',monospace;text-align:center;line-height:2.2;}
.controls-hint b{color:#555;}
.sec-title{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:3px;color:#ff5030;margin:10px 0 10px;}
.shop-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:10px;margin-bottom:20px;}
.shop-card{background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.06);border-radius:6px;padding:16px;text-align:center;transition:border-color .2s;}
.shop-card:hover{border-color:rgba(255,100,0,.3);}
.shop-card .icon{font-size:32px;margin-bottom:6px;}
.shop-card h3{font-size:15px;font-weight:700;color:#ddd;margin-bottom:3px;}
.shop-card .desc{font-size:10px;color:#555;font-family:'Space Mono',monospace;margin-bottom:8px;line-height:1.5;}
.shop-card .price{color:#ffd700;font-size:13px;font-weight:700;margin-bottom:8px;}
.shop-card .owned-badge{color:#44dd66;font-size:11px;font-family:'Space Mono',monospace;}
.btn-buy{padding:6px 16px;background:linear-gradient(90deg,#ffd700,#ff8800);border:none;border-radius:3px;color:#000;font-weight:700;font-size:11px;letter-spacing:1px;cursor:pointer;transition:all .2s;font-family:'Rajdhani',sans-serif;}
.btn-buy:hover:not(:disabled){transform:scale(1.05);}
.btn-buy:disabled{background:#2a2a2a;color:#555;cursor:not-allowed;}
.codes-wrap{max-width:420px;margin:20px auto 0;}
.codes-title{font-family:'Bebas Neue',sans-serif;font-size:28px;letter-spacing:4px;color:#eee;margin-bottom:4px;}
.codes-sub{font-size:11px;color:#444;font-family:'Space Mono',monospace;margin-bottom:20px;}
.code-row{display:flex;gap:8px;margin-bottom:10px;}
.code-input{flex:1;padding:10px 14px;background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);border-radius:4px;color:#fff;font-size:14px;font-family:'Space Mono',monospace;transition:border-color .2s;}
.code-input:focus{outline:none;border-color:#ff6040;}
.code-msg-ok{background:rgba(0,200,80,.1);color:#44ee88;border:1px solid rgba(0,200,80,.3);padding:8px 12px;border-radius:4px;font-size:12px;font-weight:700;font-family:'Space Mono',monospace;margin-top:4px;}
.code-msg-err{background:rgba(200,0,0,.1);color:#ff4444;border:1px solid rgba(200,0,0,.3);padding:8px 12px;border-radius:4px;font-size:12px;font-weight:700;font-family:'Space Mono',monospace;margin-top:4px;}
.skins-wrap{display:flex;gap:20px;}
/* Inventory sub-tabs */
.inv-subtabs{display:flex;gap:4px;margin-bottom:16px;}
.inv-subtab{padding:7px 20px;background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.07);border-radius:4px;color:#444;font-size:11px;font-weight:700;letter-spacing:2px;cursor:pointer;font-family:'Rajdhani',sans-serif;text-transform:uppercase;transition:all .2s;}
.inv-subtab:hover{color:#aaa;}
.inv-subtab.active{background:rgba(255,100,0,.12);border-color:rgba(255,100,0,.4);color:#ff8040;}
.weapon-list-panel{width:150px;flex-shrink:0;}
.weapon-list-panel h4{font-size:10px;letter-spacing:3px;color:#444;text-transform:uppercase;margin-bottom:8px;font-family:'Space Mono',monospace;}
.wpick-btn{display:block;width:100%;padding:9px 12px;background:transparent;border:1px solid rgba(255,255,255,.06);border-radius:4px;color:#555;font-size:12px;font-weight:700;letter-spacing:1px;text-align:left;cursor:pointer;margin-bottom:5px;transition:all .2s;font-family:'Rajdhani',sans-serif;text-transform:uppercase;}
.wpick-btn:hover{background:rgba(255,255,255,.04);color:#bbb;}
.wpick-btn.active{background:rgba(255,100,0,.1);border-color:rgba(255,100,0,.4);color:#ff8040;}
.skins-panel{flex:1;}
.prog-box{padding:12px 14px;background:rgba(255,255,255,.02);border:1px solid rgba(255,255,255,.05);border-radius:6px;margin-bottom:12px;}
.prog-label{font-size:11px;color:#555;font-family:'Space Mono',monospace;margin-bottom:7px;}
.prog-label b{color:#ff8040;}
.prog-bar-bg{height:4px;background:rgba(255,255,255,.06);border-radius:2px;overflow:hidden;}
.prog-bar-fill{height:100%;background:linear-gradient(90deg,#ff4400,#ff8800);border-radius:2px;transition:width .4s;}
.prog-kills{font-size:10px;color:#444;font-family:'Space Mono',monospace;margin-top:4px;}
.skin-row{display:flex;align-items:center;gap:12px;padding:10px 12px;background:rgba(255,255,255,.02);border:1px solid rgba(255,255,255,.05);border-radius:6px;margin-bottom:7px;transition:all .2s;}
.skin-row:hover:not(.sk-locked){background:rgba(255,255,255,.05);}
.skin-row.sk-equipped{border-color:rgba(255,130,0,.45);background:rgba(255,100,0,.05);}
.skin-row.sk-locked{opacity:.4;cursor:not-allowed;}
.skin-thumb{width:48px;height:48px;border-radius:5px;display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0;}
.skin-info{flex:1;}
.skin-info .sname{font-size:13px;font-weight:700;}
.skin-info .srarity{font-size:9px;letter-spacing:2px;text-transform:uppercase;font-family:'Space Mono',monospace;margin-bottom:1px;}
.skin-info .sstatus{font-size:10px;font-family:'Space Mono',monospace;color:#555;}
.skin-actions{display:flex;flex-direction:column;gap:4px;align-items:flex-end;}
.btn-view-sk{padding:5px 10px;background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);border-radius:3px;color:#bbb;font-size:10px;font-weight:700;letter-spacing:1px;cursor:pointer;font-family:'Rajdhani',sans-serif;transition:all .2s;white-space:nowrap;}
.btn-view-sk:hover{background:rgba(255,255,255,.12);color:#fff;}
.btn-equip-sm{padding:5px 10px;background:linear-gradient(90deg,#ff4400,#ff8800);border:none;border-radius:3px;color:#fff;font-size:10px;font-weight:700;cursor:pointer;font-family:'Rajdhani',sans-serif;white-space:nowrap;}
#hud{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:10;display:none;}
.crosshair{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);}
.crosshair::before,.crosshair::after{content:'';position:absolute;background:rgba(255,255,255,.8);}
.crosshair::before{width:16px;height:1.5px;top:0;left:-8px;}
.crosshair::after{height:16px;width:1.5px;top:-8px;left:0;}
.ch-dot{position:absolute;width:3px;height:3px;background:#000;border-radius:50%;top:50%;left:50%;transform:translate(-50%,-50%);}
#hitMarker{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);opacity:0;transition:opacity .12s;pointer-events:none;}
.hud-bl{position:absolute;bottom:24px;left:24px;display:flex;flex-direction:column;gap:5px;}
.health-row{display:flex;align-items:center;gap:8px;}
.hp-icon{color:#ff4444;font-size:13px;}
.hp-bar-bg{width:160px;height:7px;background:rgba(0,0,0,.6);border:1px solid rgba(255,255,255,.12);border-radius:2px;overflow:hidden;}
.hp-bar-fill{height:100%;background:linear-gradient(90deg,#ff2020,#ff6060);border-radius:2px;transition:width .2s;}
#hpText{color:#fff;font-size:12px;font-weight:700;font-family:'Space Mono',monospace;}
.stam-row{display:flex;align-items:center;gap:8px;}
.stam-icon{color:#44aaff;font-size:11px;font-family:'Space Mono',monospace;}
.stam-bar-bg{width:160px;height:5px;background:rgba(0,0,0,.6);border:1px solid rgba(255,255,255,.1);border-radius:2px;overflow:hidden;}
.stam-bar-fill{height:100%;background:linear-gradient(90deg,#1188ff,#44ddff);border-radius:2px;transition:width .08s;}
.stam-bar-fill.depleted{background:#334455;}
.stam-bar-fill.recharging{background:linear-gradient(90deg,#2255aa,#3399cc);}
.hud-info{color:#888;font-size:11px;font-family:'Space Mono',monospace;}
.hud-tr{position:absolute;top:18px;right:18px;text-align:right;}
.hud-cred{color:#ffd700;font-size:13px;font-weight:700;font-family:'Space Mono',monospace;}
.hud-wave-txt{color:#ff8040;font-size:11px;font-family:'Space Mono',monospace;margin-top:2px;}
.hud-wep{position:absolute;bottom:70px;right:24px;text-align:right;}
.hud-wep-name{font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:3px;color:#ff8040;}
.hud-skin-name{font-size:10px;color:#555;font-family:'Space Mono',monospace;}
.hud-ammo{position:absolute;bottom:24px;right:24px;text-align:right;}
.hud-ammo-cur{font-family:'Bebas Neue',sans-serif;font-size:40px;line-height:1;color:#fff;}
.hud-ammo-res{font-size:13px;font-family:'Space Mono',monospace;color:#555;}
.hud-vign{position:absolute;inset:0;pointer-events:none;opacity:0;transition:opacity .3s;background:radial-gradient(ellipse at center,transparent 55%,rgba(255,0,0,.45) 100%);}
.dmg-flash{position:absolute;inset:0;background:rgba(255,0,0,.22);pointer-events:none;opacity:0;transition:opacity .12s;}
.wave-announce{position:absolute;top:38%;left:50%;transform:translate(-50%,-50%);text-align:center;pointer-events:none;opacity:0;transition:opacity .4s;}
.wn-label{font-size:11px;letter-spacing:4px;color:#ff8800;font-family:'Space Mono',monospace;}
.wn-num{font-family:'Bebas Neue',sans-serif;font-size:72px;letter-spacing:8px;color:#fff;line-height:1;filter:drop-shadow(0 0 30px rgba(255,80,0,.7));}
.wn-count{font-size:13px;color:#aaa;font-family:'Space Mono',monospace;margin-top:2px;}
#skinOverlay{background:rgba(4,4,14,.93);flex-direction:column;align-items:center;justify-content:center;z-index:60;}
.pv-title{font-family:'Bebas Neue',sans-serif;font-size:38px;letter-spacing:6px;color:#fff;margin-bottom:3px;}
.pv-rarity{font-size:11px;letter-spacing:4px;margin-bottom:16px;font-family:'Space Mono',monospace;text-transform:uppercase;}
#pvCanvas{width:480px;height:340px;border:1px solid rgba(255,255,255,.1);border-radius:8px;}
.pv-hint{color:#333;font-size:10px;font-family:'Space Mono',monospace;margin-top:8px;}
.pv-btns{display:flex;gap:10px;margin-top:16px;}
.btn-equip-big{padding:10px 28px;background:linear-gradient(90deg,#ff8800,#ffd700);border:none;border-radius:4px;color:#000;font-family:'Bebas Neue',sans-serif;font-size:18px;letter-spacing:2px;cursor:pointer;}
.btn-close{padding:10px 22px;background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.12);border-radius:4px;color:#bbb;font-family:'Bebas Neue',sans-serif;font-size:18px;letter-spacing:2px;cursor:pointer;}
#unlockBanner{flex-direction:column;align-items:center;justify-content:center;z-index:80;}
.ub-inner{background:rgba(5,5,18,.97);border:2px solid rgba(255,180,0,.45);border-radius:12px;padding:26px 36px;text-align:center;box-shadow:0 0 60px rgba(255,150,0,.15);display:flex;flex-direction:column;align-items:center;}
.ub-label{font-size:10px;letter-spacing:4px;color:#ff8800;font-family:'Space Mono',monospace;margin-bottom:4px;}
.ub-title{font-family:'Bebas Neue',sans-serif;font-size:28px;letter-spacing:4px;color:#ffd700;margin-bottom:3px;}
.ub-sub{font-size:12px;color:#888;font-family:'Space Mono',monospace;margin-bottom:12px;}
#ubCanvas{width:300px;height:200px;border:1px solid rgba(255,255,255,.08);border-radius:6px;}
.ub-hint{font-size:9px;color:#333;font-family:'Space Mono',monospace;margin:6px 0 12px;}
.ub-btns{display:flex;gap:8px;justify-content:center;}
.btn-ub-equip{padding:8px 22px;background:linear-gradient(90deg,#ff8800,#ffd700);border:none;border-radius:3px;color:#000;font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:2px;cursor:pointer;}
.btn-ub-close{padding:8px 18px;background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.1);border-radius:3px;color:#bbb;font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:2px;cursor:pointer;}
#pauseMenu{background:rgba(0,0,0,.8);flex-direction:column;align-items:center;justify-content:center;gap:14px;z-index:70;}
#pauseMenu h2{font-family:'Bebas Neue',sans-serif;font-size:52px;letter-spacing:8px;color:#fff;margin-bottom:8px;}
.btn-sm-sec{padding:8px 22px;background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);border-radius:3px;color:#bbb;font-family:'Rajdhani',sans-serif;font-size:13px;font-weight:700;letter-spacing:1px;cursor:pointer;transition:all .2s;}
.btn-sm-sec:hover{background:rgba(255,255,255,.1);color:#fff;}
#gameOverScreen{background:rgba(0,0,0,.88);flex-direction:column;align-items:center;justify-content:center;gap:14px;z-index:90;}
#gameOverScreen h2{font-family:'Bebas Neue',sans-serif;font-size:76px;letter-spacing:10px;color:#ff2020;filter:drop-shadow(0 0 40px rgba(255,0,0,.5));}
.go-stats{font-family:'Space Mono',monospace;font-size:13px;color:#555;text-align:center;line-height:2.2;}
#notif{position:fixed;top:16px;left:50%;transform:translateX(-50%) translateY(-60px);background:linear-gradient(90deg,#ff4400,#ff8800);color:#fff;padding:9px 22px;border-radius:4px;font-size:13px;font-weight:700;letter-spacing:1px;z-index:200;transition:transform .25s;pointer-events:none;font-family:'Rajdhani',sans-serif;white-space:nowrap;}
#notif.show{transform:translateX(-50%) translateY(0);}
</style>
</head>
<body>
<canvas id="c"></canvas>
<div id="notif"></div>

<!-- MENU -->
<div id="menuScreen" class="overlay show">
  <div class="menu-bg"></div><div class="scanlines"></div>
  <div class="menu-inner">
    <div class="menu-header">
      <h1>ZOMBIE SLAYER</h1>
      <div class="subtitle">SURVIVE · KILL · UNLOCK</div>
      <div style="display:flex;align-items:center;justify-content:center;margin-top:8px">
        <div class="cred-disp">CREDITS: <span id="menuCredits">0</span></div>
      </div>
    </div>
    <div class="nav-tabs">
      <button class="nav-tab active" data-tab="main">PLAY</button>
      <button class="nav-tab" data-tab="shop">SHOP</button>
      <button class="nav-tab" data-tab="codes">CODES</button>
      <button class="nav-tab" data-tab="inventory">INVENTORY</button>
      <button class="nav-tab" data-tab="settings">SETTINGS</button>
    </div>
    <div class="tab-content active" id="tabMain">
      <div class="main-play-area">
        <button class="btn-play" id="btnPlay">PLAY</button>
        <div class="controls-hint">
          <b>WASD</b> move &nbsp;·&nbsp; <b>MOUSE</b> look &nbsp;·&nbsp; <b>LMB</b> shoot<br>
          <b>1</b> primary &nbsp;·&nbsp; <b>2</b> secondary &nbsp;·&nbsp; <b>3 / Q</b> knife &nbsp;·&nbsp; <b>R</b> reload<br>
          <b>F</b> inspect &nbsp;·&nbsp; <b>H</b> heal &nbsp;·&nbsp; <b>ESC</b> pause
        </div>
      </div>
    </div>
    <div class="tab-content" id="tabShop">
      <div class="sec-title">WEAPONS</div>
      <div class="shop-grid" id="shopWeapons"></div>
      <div class="sec-title">AMMO and SUPPLIES</div>
      <div class="shop-grid" id="shopSupplies"></div>
    </div>
    <div class="tab-content" id="tabCodes">
      <div class="codes-wrap">
        <div class="codes-title">PROMO CODES</div>
        <div class="codes-sub">Enter a code to claim rewards. Case-sensitive.</div>
        <div class="code-row">
          <input type="text" class="code-input" id="codeInput" placeholder="Enter code...">
          <button class="btn-buy" id="btnRedeem" style="white-space:nowrap;padding:0 18px">REDEEM</button>
        </div>
        <div id="codeMsg"></div>
      </div>
    </div>
    <div class="tab-content" id="tabInventory">
      <div class="inv-subtabs">
        <button class="inv-subtab active" data-islot="primary">PRIMARY</button>
        <button class="inv-subtab" data-islot="secondary">SECONDARY</button>
        <button class="inv-subtab" data-islot="knife">KNIFE</button>
      </div>
      <div id="invWeaponList"></div>
    </div>
    <div class="tab-content" id="tabSettings">
      <div class="settings-wrap">
        <div class="sec-title" style="margin-bottom:4px">DISPLAY</div>
        <div class="setting-row">
          <span class="setting-label">Field of View</span>
          <input type="range" class="setting-slider" id="settingFov" min="70" max="120" value="72" oninput="applySetting('fov',this.value)">
          <span class="setting-value" id="valFov">72°</span>
        </div>
        <div class="setting-row">
          <span class="setting-label">Brightness</span>
          <input type="range" class="setting-slider" id="settingBright" min="20" max="200" value="100" oninput="applySetting('brightness',this.value)">
          <span class="setting-value" id="valBright">100%</span>
        </div>
        <div class="sec-title" style="margin-top:20px;margin-bottom:4px">CONTROLS</div>
        <div class="setting-row">
          <span class="setting-label">Sensitivity</span>
          <input type="range" class="setting-slider" id="settingSens" min="1" max="20" value="9" oninput="applySetting('sensitivity',this.value)">
          <span class="setting-value" id="valSens">1.0×</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- HUD -->
<div id="hud">
  <div class="hud-vign" id="hudVign"></div>
  <div class="dmg-flash" id="dmgFlash"></div>
  <div class="crosshair"><div class="ch-dot"></div></div>
  <!-- SCOPE OVERLAY -->
  <div id="scopeOverlay" style="display:none;position:absolute;inset:0;pointer-events:none;">
    <!-- Black corners masking outside the scope circle -->
    <svg width="100%" height="100%" style="position:absolute;inset:0;" id="scopeSVG">
      <defs>
        <mask id="scopeMask">
          <rect width="100%" height="100%" fill="white"/>
          <circle id="scopeCircle" cx="50%" cy="50%" r="300" fill="black"/>
        </mask>
      </defs>
      <rect width="100%" height="100%" fill="black" mask="url(#scopeMask)" opacity="0.96"/>
      <!-- Scope reticle lines (crosshair inside circle) -->
      <line id="sL1" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.7)" stroke-width="1"/>
      <line id="sL2" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.7)" stroke-width="1"/>
      <line id="sL3" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.7)" stroke-width="1"/>
      <line id="sL4" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.7)" stroke-width="1"/>
      <!-- Mil-dot centre -->
      <circle id="sDot" cx="50%" cy="50%" r="3" fill="none" stroke="rgba(0,255,80,0.8)" stroke-width="1"/>
      <!-- Ranging marks on horizontal line -->
      <line id="sM1" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.5)" stroke-width="1"/>
      <line id="sM2" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.5)" stroke-width="1"/>
      <line id="sM3" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.5)" stroke-width="1"/>
      <line id="sM4" x1="0" y1="0" x2="0" y2="0" stroke="rgba(0,255,80,0.5)" stroke-width="1"/>
      <!-- Scope ring (inner edge highlight) -->
      <circle id="scopeRing" cx="50%" cy="50%" r="300" fill="none" stroke="rgba(0,255,80,0.18)" stroke-width="2"/>
    </svg>
    <!-- Scope lens tint -->
    <div style="position:absolute;inset:0;background:radial-gradient(circle 28% at 50% 50%, rgba(0,20,0,0.25) 0%, transparent 100%);pointer-events:none;"></div>
  </div>
  <div id="knifeCoolBar" style="position:absolute;top:calc(50% + 16px);left:50%;transform:translateX(-50%);width:60px;height:4px;background:rgba(0,0,0,.5);border-radius:2px;overflow:hidden;opacity:0;transition:opacity .15s;pointer-events:none;">
    <div id="knifeCoolFill" style="height:100%;width:100%;background:linear-gradient(90deg,#ff4400,#ff8800);border-radius:2px;transform-origin:left;transition:none;"></div>
  </div>
  <div id="hitMarker">
    <svg width="16" height="16"><line x1="0" y1="0" x2="5" y2="5" stroke="#ff3333" stroke-width="1.5"/><line x1="16" y1="0" x2="11" y2="5" stroke="#ff3333" stroke-width="1.5"/><line x1="0" y1="16" x2="5" y2="11" stroke="#ff3333" stroke-width="1.5"/><line x1="16" y1="16" x2="11" y2="11" stroke="#ff3333" stroke-width="1.5"/></svg>
  </div>
  <div class="hud-bl">
    <div class="health-row">
      <span class="hp-icon">HP</span>
      <div class="hp-bar-bg"><div class="hp-bar-fill" id="hpFill" style="width:100%"></div></div>
      <span id="hpText">100</span>
    </div>
    <div class="hud-info" id="hudKills">KILLS: 0</div>
    <div class="hud-info" style="color:#44dd66" id="hudKits">KITS: 0</div>
    <div class="stam-row" style="margin-top:2px">
      <span class="stam-icon">RUN</span>
      <div class="stam-bar-bg"><div class="stam-bar-fill" id="stamFill" style="width:100%"></div></div>
    </div>
  </div>
  <div class="hud-tr">
    <div class="hud-cred" id="hudCred">CREDITS: 0</div>
    <div class="hud-wave-txt" id="hudWave">WAVE 1</div>
    <div class="hud-wave-txt" id="hudZombies" style="color:#ff4444">ZOMBIES: 0</div>
  </div>
  <div class="hud-wep">
    <div class="hud-wep-name" id="hudWepName">PISTOL</div>
    <div class="hud-skin-name" id="hudSkinName">Default</div>
  </div>
  <div class="hud-skin-prog" id="hudSkinProg">
    <span class="hud-sp-label" id="hudSpLabel">NEXT SKIN</span>
    <div class="hud-spbar-bg"><div class="hud-spbar-fill" id="hudSpFill" style="width:0%"></div></div>
    <span class="hud-sp-label" id="hudSpPct">0%</span>
  </div>
  <div class="hud-ammo" id="hudAmmoBlock">
    <div class="hud-ammo-cur" id="ammoCur">30</div>
    <div class="hud-ammo-res" id="ammoRes">/ 90</div>
  </div>
  <div id="hsPopup" style="position:absolute;top:42%;left:50%;transform:translate(-50%,-50%) scale(0.9);opacity:0;transition:opacity .15s,transform .15s;pointer-events:none;text-align:center;">
    <div style="font-family:'Bebas Neue',sans-serif;font-size:32px;letter-spacing:5px;color:#ffdd00;filter:drop-shadow(0 0 12px rgba(255,220,0,.8));line-height:1;">HEADSHOT</div>
    <div style="font-family:'Space Mono',monospace;font-size:10px;color:#ffaa00;letter-spacing:2px;margin-top:2px;">3x DAMAGE · +16 CREDITS</div>
  </div>
  <div class="wave-announce" id="waveAnnounce">
    <div class="wn-label">INCOMING</div>
    <div class="wn-num" id="waveNum">WAVE 1</div>
    <div class="wn-count" id="waveCount">5 ZOMBIES</div>
  </div>
</div>

<!-- PAUSE -->
<div id="pauseMenu" class="overlay">
  <h2>PAUSED</h2>
  <button class="btn-play" id="btnResume" style="font-size:20px;padding:11px 56px">RESUME</button>
  <button class="btn-sm-sec" id="btnQuit">QUIT TO MENU</button>
</div>

<!-- GAME OVER -->
<div id="gameOverScreen" class="overlay">
  <h2>YOU DIED</h2>
  <div class="go-stats" id="goStats"></div>
  <button class="btn-play" id="btnGoMenu" style="font-size:18px;padding:11px 48px">MAIN MENU</button>
</div>

<!-- SKIN PREVIEW -->
<div id="skinOverlay" class="overlay">
  <div class="pv-title" id="pvTitle">GOLDEN EAGLE</div>
  <div class="pv-rarity" id="pvRarity">Pistol</div>
  <canvas id="pvCanvas" width="480" height="340"></canvas>
  <div class="pv-hint">drag to rotate</div>
  <div class="pv-btns">
    <button class="btn-equip-big" id="pvEquipBtn">EQUIP</button>
    <button class="btn-close" id="pvCloseBtn">CLOSE</button>
  </div>
</div>

<!-- UNLOCK BANNER -->
<div id="unlockBanner" class="overlay">
  <div class="ub-inner">
    <div class="ub-label">NEW SKIN UNLOCKED</div>
    <div class="ub-title" id="ubTitle">Golden Eagle</div>
    <div class="ub-sub" id="ubSub">Pistol</div>
    <canvas id="ubCanvas" width="300" height="200"></canvas>
    <div class="ub-hint">drag to rotate</div>
    <div class="ub-btns">
      <button class="btn-ub-equip" id="ubEquipBtn">EQUIP</button>
      <button class="btn-ub-close" id="ubCloseBtn">CLOSE</button>
    </div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
'use strict';

// ══ DATA ══
const RC={common:'#aaaaaa',rare:'#5588ff',epic:'#cc44ff',legendary:'#ff8800'};
const WDEFS={
  // ── PRIMARIES (slot 1) ──
  smg:{name:'MP5',slot:'primary',type:'gun',price:3000,hitsToKill:10,fireMs:110,startAmmo:50,resAmmo:150,boxCost:60,boxAmt:50,skins:[
    {id:'default',name:'Default',      kills:0,  colors:[0x666666,0x333333],rarity:'common',   emoji:'?'},
    {id:'golden', name:'Gold Plated',  kills:10, colors:[0xFFD700,0xDAA520],rarity:'rare',     emoji:'G'},
    {id:'cobalt', name:'Cobalt Blue',  kills:50, colors:[0x4169E1,0x000088],rarity:'epic',     emoji:'C'},
    {id:'atomic', name:'Void Reactor', kills:100,colors:[0x00ffee,0x000820],rarity:'legendary',emoji:'V'},
  ]},
  shotgun:{name:'Shotgun',slot:'primary',type:'gun',price:2500,hitsToKill:1,fireMs:1100,startAmmo:8,resAmmo:24,boxCost:80,boxAmt:8,skins:[
    {id:'default',name:'Default',      kills:0,  colors:[0x8B4513,0x555555],rarity:'common',   emoji:'?'},
    {id:'tiger',  name:'Tiger Tooth',  kills:10, colors:[0xFF8C00,0x111111],rarity:'epic',     emoji:'T'},
    {id:'fade',   name:'Inferno',      kills:50, colors:[0xFF2200,0xFF8800],rarity:'legendary',emoji:'I'},
  ]},
  sniper:{name:'Sniper',slot:'primary',type:'gun',price:4000,hitsToKill:1,fireMs:1000,startAmmo:5,resAmmo:15,boxCost:120,boxAmt:5,isBoltAction:true,skins:[
    {id:'default', name:'Default',          kills:0,  colors:[0x3a3a3a,0x1a2a1a],rarity:'common',   emoji:'?'},
    {id:'arctic',  name:'Arctic Warfare',   kills:10, colors:[0xeeeeff,0x9ab0c8],rarity:'rare',     emoji:'A'},
    {id:'ghillie', name:'Ghillie Strike',   kills:20, colors:[0x3a5a1a,0x1a3008],rarity:'epic',     emoji:'G'},
    {id:'crimson', name:'Crimson Reaper',   kills:35, colors:[0xaa1111,0x330000],rarity:'epic',     emoji:'C'},
    {id:'phantom', name:'Phantom Protocol', kills:50, colors:[0x0a0a1a,0x4400aa],rarity:'legendary',emoji:'P'},
  ]},
  // ── SECONDARIES (slot 2) ──
  pistol:{name:'Pistol',slot:'secondary',type:'gun',hitsToKill:3,fireMs:480,startAmmo:30,resAmmo:90,boxCost:50,boxAmt:30,skins:[
    {id:'default',name:'Default',       kills:0,  colors:[0x888888,0x333333],rarity:'common',   emoji:'?'},
    {id:'golden', name:'Golden Eagle',  kills:7,  colors:[0xFFD700,0xAA7700],rarity:'rare',     emoji:'G'},
    {id:'crimson',name:'Crimson Web',  kills:14, colors:[0xCC2222,0x660000],rarity:'epic',     emoji:'C'},
    {id:'fade',   name:'Aurora Blight',kills:21, colors:[0x00FF88,0x9400D3],rarity:'legendary',emoji:'A'},
    {id:'neon',   name:'Neon Rider',   kills:28, colors:[0x00FFCC,0xFF00AA],rarity:'legendary',emoji:'N'},
  ]},
  revolver:{name:'Revolver',slot:'secondary',type:'gun',price:800,hitsToKill:2,fireMs:650,startAmmo:6,resAmmo:32,boxCost:55,boxAmt:12,skins:[
    {id:'default', name:'Default',       kills:0,  colors:[0x666666,0x444444],rarity:'common',   emoji:'?'},
    {id:'nickel',  name:'Nickel Plated', kills:7,  colors:[0xCCCCCC,0x888888],rarity:'rare',     emoji:'N'},
    {id:'serpent', name:'Serpent Scale', kills:14, colors:[0x2d6e2d,0x0a2a0a],rarity:'epic',     emoji:'S'},
    {id:'wildwest',name:'Wild West',     kills:21, colors:[0xC8860A,0x7a4400],rarity:'legendary',emoji:'W'},
  ]},
  deagle:{name:'Desert Eagle',slot:'secondary',type:'gun',price:1500,hitsToKill:2,fireMs:380,startAmmo:8,resAmmo:32,boxCost:65,boxAmt:16,skins:[
    {id:'default',name:'Default',         kills:0,  colors:[0x555555,0x222222],rarity:'common',   emoji:'?'},
    {id:'golden', name:'Blaze',           kills:7,  colors:[0xFF6600,0xCC3300],rarity:'rare',     emoji:'B'},
    {id:'oxide',  name:'Prismatic Storm', kills:14, colors:[0x00AAFF,0xFF00AA],rarity:'legendary',emoji:'P'},
  ]},
  compact:{name:'MAC-10',slot:'secondary',type:'gun',price:600,hitsToKill:6,fireMs:80,startAmmo:30,resAmmo:90,boxCost:45,boxAmt:30,skins:[
    {id:'default',name:'Default',       kills:0,  colors:[0x444444,0x222222],rarity:'common',emoji:'?'},
    {id:'rust',   name:'Rust Coat',     kills:7,  colors:[0x8B3A0F,0x5a1a00],rarity:'rare',  emoji:'R'},
    {id:'hotrod', name:'Dark Vortex',   kills:14, colors:[0x110022,0x8800FF],rarity:'legendary',emoji:'D'},
  ]},
  // ── KNIVES (slot 3) ──
  knife:{name:'Knife',slot:'knife',type:'melee',hitsToKill:2,cooldown:1500,skins:[
    {id:'default',   name:'Standard',          kills:0,  model:'std',       colors:[0xCCCCCC,0x777777],rarity:'common',   emoji:'K'},
    {id:'karambit',  name:'Bayonet | Forest',   kills:5,  model:'bayonet',   colors:[0x3A7A22,0x1A4A10],rarity:'rare',     emoji:'🗡️'},
    {id:'butterfly', name:'Butterfly | Fade',   kills:10, model:'butterfly', colors:[0xFF69B4,0x8800CC],rarity:'epic',     emoji:'B'},
    {id:'shadow',    name:'Crimson Phantom',    kills:15, model:'shadow',    colors:[0x330000,0xFF0033],rarity:'legendary',emoji:'X'},
  ]},
  sledge:{name:'Sledgehammer',slot:'knife',type:'melee',price:1200,hitsToKill:1,cooldown:5000,skins:[
    {id:'default',name:'Default',       kills:0,  model:'sledge',      colors:[0x555555,0x8B4513],rarity:'common',   emoji:'🔨'},
    {id:'rusted', name:'Rusty Crusher', kills:5,  model:'sledge',      colors:[0x8B3A0F,0x444444],rarity:'rare',     emoji:'🦀'},
    {id:'gold',   name:'Gold Rush',     kills:10, model:'sledge',      colors:[0xFFD700,0xAA5500],rarity:'epic',     emoji:'⭐'},
    {id:'void',   name:'Void Smash',    kills:15, model:'sledge_void', colors:[0x110022,0x8800FF],rarity:'legendary',emoji:'💜'},
  ]},
};
const WAVES=[5,10,22,45,90];
const HEAL_COST=75,HEAL_AMT=50,SKIN_BONUS=50;
const CODES={'free1000':{type:'credits',amount:1000,msg:'+1000 CREDITS!'},'Dev':{type:'devmode',msg:'ALL UNLOCKED!'}};

// ══ STATE ══
const S={
  screen:'menu',credits:0,health:100,kits:0,
  weapons:{
    smg:    {owned:false,ammo:0,  res:0,   kills:0,skin:'default',lastFire:0},
    shotgun:{owned:false,ammo:0,  res:0,   kills:0,skin:'default',lastFire:0},
    sniper: {owned:false,ammo:0,  res:0,   kills:0,skin:'default',lastFire:0},
    pistol: {owned:true, ammo:30, res:90,  kills:0,skin:'default',lastFire:0},
    revolver:{owned:false,ammo:0, res:0,   kills:0,skin:'default',lastFire:0},
    deagle: {owned:false,ammo:0,  res:0,   kills:0,skin:'default',lastFire:0},
    compact:{owned:false,ammo:0,  res:0,   kills:0,skin:'default',lastFire:0},
    knife:  {owned:true,          kills:0, skin:'default',lastFire:0,cd:0},
    sledge: {owned:false,         kills:0, skin:'default',lastFire:0,cd:0},
  },
  held:'pistol',
  heldPrimary:'smg', heldSecondary:'pistol', heldKnife:'knife',
  usedCodes:[],
  unlocked:{smg:['default'],shotgun:['default'],sniper:['default'],pistol:['default'],revolver:['default'],deagle:['default'],compact:['default'],knife:['default'],sledge:['default']},
  zombies:[],totalKills:0,
  wave:0,waveActive:false,waveKilled:0,waveSize:0,spawnQueue:0,spawnTimer:0,
  invSlot:'primary',
  unlockQueue:[],showingUnlock:false,
  autoFire:null,
};

// ══ THREE SETUP ══
const cvs=document.getElementById('c');
const renderer=new THREE.WebGLRenderer({canvas:cvs,antialias:true});
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.setSize(innerWidth,innerHeight);
renderer.shadowMap.enabled=true;
renderer.shadowMap.type=THREE.PCFSoftShadowMap;

const scene=new THREE.Scene();
scene.background=new THREE.Color(0x070b12);
scene.fog=new THREE.FogExp2(0x070b12,0.025);

const camera=new THREE.PerspectiveCamera(72,innerWidth/innerHeight,0.05,300);
camera.position.set(0,1.75,0);
camera.rotation.order='YXZ';
scene.add(camera);

scene.add(new THREE.AmbientLight(0x304060,0.8));
const sun=new THREE.DirectionalLight(0xfff0d0,1.0);
sun.position.set(15,25,10);sun.castShadow=true;
sun.shadow.mapSize.set(2048,2048);sun.shadow.camera.far=130;
scene.add(sun);
const rl=new THREE.PointLight(0xff2200,1.5,40);rl.position.set(0,6,0);scene.add(rl);
const bl=new THREE.PointLight(0x2244ff,0.6,50);bl.position.set(0,4,0);scene.add(bl);

let weaponGroup=new THREE.Group();
camera.add(weaponGroup);

// ══ MAP ══
function mkMesh(geo,color,rx,ry,rz){
  const m=new THREE.Mesh(geo,new THREE.MeshLambertMaterial({color:color}));
  if(rx||ry||rz)m.rotation.set(rx||0,ry||0,rz||0);
  m.castShadow=true;m.receiveShadow=true;
  return m;
}
const COL=[];
function addBox(w,h,d,color,x,y,z,ry,col){
  const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),new THREE.MeshLambertMaterial({color}));
  m.position.set(x,y,z);if(ry)m.rotation.y=ry;
  m.castShadow=true;m.receiveShadow=true;scene.add(m);
  if(col){const p=0.3;COL.push({type:'box',minX:x-w/2-p,maxX:x+w/2+p,minZ:z-d/2-p,maxZ:z+d/2+p,maxY:y+h/2});}
  return m;
}
function addCyl(r,h,color,x,y,z){
  const m=new THREE.Mesh(new THREE.CylinderGeometry(r,r,h,8),new THREE.MeshLambertMaterial({color}));
  m.position.set(x,y,z);m.castShadow=true;m.receiveShadow=true;scene.add(m);
  COL.push({type:'cyl',x,z,r:r+0.3,maxY:y+h/2});
  return m;
}
function addRamp(w,d,color,x,y,z,rx,ry){
  const m=new THREE.Mesh(new THREE.BoxGeometry(w,0.22,d),new THREE.MeshLambertMaterial({color}));
  m.position.set(x,y,z);m.rotation.set(rx||0,ry||0,0);
  m.castShadow=true;m.receiveShadow=true;scene.add(m);
}

function buildMap(){
  const W=0x1e2b42,RAMP=0x263450,CONC=0x18243a,METAL=0x2a3c55,CRATE=0x5a3e1e,BARREL=0x3a2a1a;
  const WALLH=4.5,WT=0.6;

  // ── Ground ──
  const gnd=mkMesh(new THREE.PlaneGeometry(100,100),0x0c1020,-Math.PI/2);
  scene.add(gnd);
  scene.add(new THREE.GridHelper(100,50,0x0d1424,0x0d1424));

  // ── Outer walls with window gaps ──
  function outerWallAxis(axis,cval,len){
    const segs=[{a:0,b:len*0.28},{a:len*0.36,b:len*0.52},{a:len*0.60,b:len*0.76},{a:len*0.84,b:len}];
    segs.forEach(({a,b})=>{
      const sl=b-a,off=(a+b)/2-len/2;
      const geo=axis==='x'?new THREE.BoxGeometry(sl,WALLH,WT):new THREE.BoxGeometry(WT,WALLH,sl);
      const m=new THREE.Mesh(geo,new THREE.MeshLambertMaterial({color:W}));
      m.position.set(axis==='x'?off:cval,WALLH/2,axis==='x'?cval:off);
      m.castShadow=m.receiveShadow=true;scene.add(m);
      const p=0.3;
      if(axis==='x') COL.push({type:'box',minX:off-sl/2-p,maxX:off+sl/2+p,minZ:cval-WT/2-p,maxZ:cval+WT/2+p,maxY:WALLH});
      else            COL.push({type:'box',minX:cval-WT/2-p,maxX:cval+WT/2+p,minZ:off-sl/2-p,maxZ:off+sl/2+p,maxY:WALLH});
    });
    [{a:len*0.28,b:len*0.36},{a:len*0.52,b:len*0.60},{a:len*0.76,b:len*0.84}].forEach(({a,b})=>{
      const sl=b-a,off=(a+b)/2-len/2;
      const geo=axis==='x'?new THREE.BoxGeometry(sl,0.18,WT*1.6):new THREE.BoxGeometry(WT*1.6,0.18,sl);
      const m=new THREE.Mesh(geo,new THREE.MeshLambertMaterial({color:METAL}));
      m.position.set(axis==='x'?off:cval,1.1,axis==='x'?cval:off);m.castShadow=true;scene.add(m);
    });
  }
  outerWallAxis('x',-38,76);outerWallAxis('x',38,76);
  outerWallAxis('z',-38,76);outerWallAxis('z',38,76);

  // ── Central bunker (open south side) ──
  addBox(8,0.3,8,CONC,0,0.15,0,0,false);
  addBox(8,2.2,0.4,W,0,1.25,-4,0,true);
  addBox(0.4,2.2,8,W,-4,1.25,0,0,true);
  addBox(0.4,2.2,8,W,4,1.25,0,0,true);
  addBox(6,1.3,0.4,W,0,0.65,9,0,true);

  // ── Interior cover walls ──
  [[14,10,6,1.1,0.4],[14,-10,6,1.1,0.4],[-14,10,6,1.1,0.4],[-14,-10,6,1.1,0.4],
   [10,0,0.4,1.1,5],[-10,0,0.4,1.1,5],[0,14,6,1.1,0.4],[0,-14,6,1.1,0.4],
   [22,0,0.4,1.1,4],[-22,0,0.4,1.1,4],
  ].forEach(([x,z,w,h,d])=>addBox(w,h,d,METAL,x,h/2,z,0,true));

  // ── RAMP 1 – NE corner, runs N along Z ──
  const ra=Math.atan2(1.6,5.5);
  addRamp(4,5.5,RAMP, 22,0.8,-21,-ra,0);
  addBox(4,0.3,7,RAMP, 22,1.72,-27.5,0,false);
  addBox(4,1.6,0.4,W,  22,2.5,-31,  0,true);
  addBox(0.4,1.6,7,W,  24,2.5,-27.5,0,true);
  addBox(0.4,1.6,7,W,  20,2.5,-27.5,0,true);

  // ── RAMP 2 – SW corner, runs S along Z ──
  addRamp(4,5.5,RAMP,-22,0.8,21, ra,0);
  addBox(4,0.3,7,RAMP,-22,1.72,27.5,0,false);
  addBox(4,1.6,0.4,W, -22,2.5,31,  0,true);
  addBox(0.4,1.6,7,W, -24,2.5,27.5,0,true);
  addBox(0.4,1.6,7,W, -20,2.5,27.5,0,true);

  // ── RAMP 3 – NW corner, runs W along X ──
  addRamp(5.5,3.5,RAMP,-21,0.8,-22,0,-ra);
  addBox(7,0.3,3.5,RAMP,-27.5,1.72,-22,0,false);
  addBox(0.4,1.6,3.5,W,-31,2.5,-22,  0,true);
  addBox(7,1.6,0.4,W,-27.5,2.5,-24, 0,true);
  addBox(7,1.6,0.4,W,-27.5,2.5,-20, 0,true);

  // ── Pillars ──
  [[-10,-10],[10,-10],[-10,10],[10,10],[0,-20],[0,20],[-20,0],[20,0],[18,18],[-18,18],[18,-18],[-18,-18]].forEach(([x,z])=>{
    addCyl(0.55,4.5,CONC,x,2.25,z);
  });

  // ── Crates & barrels ──
  [[6,-13],[13,6],[-6,13],[-13,-6],[16,-16],[-16,16],[24,10],[-24,-10],
   [10,24],[-10,-24],[3,19],[-3,-19],[20,-5],[-20,5],[8,-28],[28,-8]].forEach(([x,z],i)=>{
    if(i%2===0){
      // Detailed crate: wood planks + metal banding
      const crate=addBox(1.2,1.2,1.2,CRATE,x,0.6,z,0,true);
      // Plank lines
      for(let p=0;p<3;p++){
        const plk=new THREE.Mesh(new THREE.BoxGeometry(1.22,0.01,1.22),new THREE.MeshLambertMaterial({color:0x3a2208}));
        plk.position.set(x,0.2+p*0.35,z);plk.rotation.y=Math.random()*0.05;scene.add(plk);
      }
      // Metal corner brackets
      [[0.58,0.58],[0.58,-0.58],[-0.58,0.58],[-0.58,-0.58]].forEach(([bx,bz])=>{
        const b=new THREE.Mesh(new THREE.BoxGeometry(0.08,1.24,0.08),new THREE.MeshLambertMaterial({color:0x888866}));
        b.position.set(x+bx,0.6,z+bz);scene.add(b);
      });
      // Stencil marking (flat dark rect to simulate)
      const stencil=new THREE.Mesh(new THREE.PlaneGeometry(0.5,0.22),new THREE.MeshBasicMaterial({color:0x1a1000}));
      stencil.rotation.y=Math.PI/2; stencil.position.set(x+0.61,0.65,z); scene.add(stencil);
    } else {
      // Detailed barrel: drum with rust bands and bung
      const barrel=addCyl(0.45,1.3,BARREL,x,0.65,z);
      // Rust band rings (3)
      for(let r=0;r<3;r++){
        const band=new THREE.Mesh(new THREE.CylinderGeometry(0.46,0.46,0.045,10),new THREE.MeshLambertMaterial({color:0x5a3a10}));
        band.position.set(x,0.15+r*0.4,z);scene.add(band);
      }
      // Bung hole (top)
      const bung=new THREE.Mesh(new THREE.CylinderGeometry(0.06,0.06,0.04,8),new THREE.MeshLambertMaterial({color:0x666644}));
      bung.position.set(x+0.2,1.32,z);scene.add(bung);
      // Rust streak
      const rust=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.5,0.01),new THREE.MeshLambertMaterial({color:0x8B2500}));
      rust.position.set(x+0.44,0.55,z);scene.add(rust);
    }
  });

  // ── Blood pools on floor ──
  [[5,-8,1.2,0.9],[-3,6,0.9,1.1],[-8,-4,1.3,0.8],[12,3,0.7,1.0],[0,-5,1.1,0.9],[-5,15,0.8,0.7]].forEach(([px,pz,sx,sz])=>{
    if(typeof px!=='number')return;
    const pool=new THREE.Mesh(new THREE.PlaneGeometry(sx,sz),new THREE.MeshBasicMaterial({color:0x3a0000,transparent:true,opacity:0.75}));
    pool.rotation.x=-Math.PI/2; pool.position.set(px,0.001,pz); scene.add(pool);
    // Blood splatter dots around pool
    for(let i=0;i<5;i++){
      const dot=new THREE.Mesh(new THREE.PlaneGeometry(0.1+Math.random()*0.15,0.08+Math.random()*0.1),new THREE.MeshBasicMaterial({color:0x2a0000,transparent:true,opacity:0.6}));
      dot.rotation.x=-Math.PI/2; dot.rotation.z=Math.random()*Math.PI;
      dot.position.set(px+(Math.random()-0.5)*2,0.001,pz+(Math.random()-0.5)*2); scene.add(dot);
    }
  });

  // ── Broken glass shards in window gaps (decorative) ──
  function addWindowGlass(axis,cval,gaps){
    gaps.forEach(({a,b})=>{
      const midOff=(a+b)/2-76/2, gapW=b-a;
      // Hanging glass shards (triangular prisms approximated with tilted boxes)
      for(let i=0;i<4;i++){
        const sw=0.05+Math.random()*0.08, sh=0.12+Math.random()*0.2;
        const shard=new THREE.Mesh(new THREE.BoxGeometry(sw,sh,0.008),new THREE.MeshLambertMaterial({color:0x88bbcc,transparent:true,opacity:0.4}));
        const ox=midOff+(Math.random()-0.5)*(gapW*0.6);
        shard.rotation.z=(Math.random()-0.5)*0.5;
        if(axis==='x') shard.position.set(ox, 2.2-Math.random()*0.4, cval);
        else            shard.position.set(cval, 2.2-Math.random()*0.4, ox);
        scene.add(shard);
      }
      // Glass shards on floor under window
      for(let i=0;i<6;i++){
        const gs=new THREE.Mesh(new THREE.PlaneGeometry(0.06+Math.random()*0.1,0.04+Math.random()*0.06),new THREE.MeshBasicMaterial({color:0x99ccdd,transparent:true,opacity:0.5}));
        gs.rotation.x=-Math.PI/2; gs.rotation.z=Math.random()*Math.PI;
        const ox=midOff+(Math.random()-0.5)*(gapW*0.8);
        const oz=cval+(Math.random()-0.5)*0.5;
        if(axis==='x') gs.position.set(ox,0.002,oz);
        else            gs.position.set(oz,0.002,ox);
        scene.add(gs);
      }
    });
  }
  const winGaps=[{a:76*0.28,b:76*0.36},{a:76*0.52,b:76*0.60},{a:76*0.76,b:76*0.84}];
  addWindowGlass('x',-38,winGaps); addWindowGlass('x',38,winGaps);
  addWindowGlass('z',-38,winGaps); addWindowGlass('z',38,winGaps);

  // ── Wall-mounted emergency lights (flickering red) ──
  [[-35,0],[-35,18],[-35,-18],[35,0],[35,18],[35,-18],[0,-35],[0,35]].forEach(([lx,lz])=>{
    const bracket=new THREE.Mesh(new THREE.BoxGeometry(0.12,0.12,0.18),new THREE.MeshLambertMaterial({color:0x333333}));
    const isX=Math.abs(lx)>Math.abs(lz);
    bracket.position.set(lx*(isX?0.95:1),3.0,lz*(isX?1:0.95));
    scene.add(bracket);
    const bulb=new THREE.Mesh(new THREE.SphereGeometry(0.08,8,6),new THREE.MeshBasicMaterial({color:0xff1100}));
    bulb.position.copy(bracket.position);
    bulb.position.y=2.85; scene.add(bulb);
    // Light cone pointing down
    const cone=new THREE.Mesh(new THREE.ConeGeometry(0.6,1.5,8,1,true),new THREE.MeshBasicMaterial({color:0xff0000,transparent:true,opacity:0.04,side:THREE.DoubleSide}));
    cone.position.set(bulb.position.x,bulb.position.y-0.75,bulb.position.z); scene.add(cone);
  });

  // ── Graffiti panels on bunker walls ──
  const grafColors=[0x880000,0x006600,0x000088,0x886600];
  ['HELP','RUN','DEAD','666'].forEach((txt,i)=>{
    const panel=new THREE.Mesh(new THREE.PlaneGeometry(1.4,0.55),new THREE.MeshBasicMaterial({color:grafColors[i],transparent:true,opacity:0.35}));
    panel.position.set(i<2?-3.8:3.8, 1.4, i%2===0?-3.8:3.8);
    panel.rotation.y=i<2?Math.PI/2:0;
    scene.add(panel);
  });

  // ── Rubble piles near cover walls ──
  [[14,10],[-14,10],[10,0],[-10,0],[0,14],[0,-14]].forEach(([rx,rz])=>{
    if(typeof rx!=='number')return;
    for(let i=0;i<5;i++){
      const rs=0.06+Math.random()*0.14;
      const rb=new THREE.Mesh(new THREE.BoxGeometry(rs,rs*0.6,rs*0.8),new THREE.MeshLambertMaterial({color:0x2a3040}));
      rb.position.set(rx+(Math.random()-0.5)*1.2, rs*0.3, rz+(Math.random()-0.5)*1.2);
      rb.rotation.y=Math.random()*Math.PI; scene.add(rb);
    }
  });

  // ── Scattered spent shell casings on floor ──
  for(let i=0;i<30;i++){
    const casing=new THREE.Mesh(new THREE.CylinderGeometry(0.014,0.014,0.05,6),new THREE.MeshLambertMaterial({color:0xcc9900}));
    casing.rotation.z=Math.PI/2+Math.random()*0.5;
    casing.position.set((Math.random()-0.5)*24, 0.014, (Math.random()-0.5)*24);
    scene.add(casing);
  }

  // ── Caution stripe tape on floor (near ramp bases) ──
  [[22,-18],[-22,18]].forEach(([tx,tz])=>{
    for(let i=0;i<6;i++){
      const stripe=new THREE.Mesh(new THREE.PlaneGeometry(4,0.12),new THREE.MeshBasicMaterial({color:i%2===0?0xffcc00:0x111111}));
      stripe.rotation.x=-Math.PI/2; stripe.rotation.z=0.7;
      stripe.position.set(tx+(i-2.5)*0.14*0.7, 0.002, tz+(i-2.5)*0.14); scene.add(stripe);
    }
  });
}

// ── BULLET HOLE DECALS ──
const DECAL_MAX=80;
const bulletHoles=[];
let HOLE_MAT=null, HOLE_GEO=null;

function initDecals(){
  const c=document.createElement('canvas'); c.width=64; c.height=64;
  const ctx=c.getContext('2d');
  ctx.beginPath(); ctx.arc(32,32,28,0,Math.PI*2);
  ctx.fillStyle='rgba(0,0,0,0.88)'; ctx.fill();
  ctx.beginPath(); ctx.arc(32,32,10,0,Math.PI*2);
  ctx.fillStyle='#050505'; ctx.fill();
  ctx.strokeStyle='rgba(0,0,0,0.65)'; ctx.lineWidth=1.5;
  for(let i=0;i<8;i++){
    const a=i/8*Math.PI*2+Math.random()*0.2;
    const len=10+Math.random()*10;
    ctx.beginPath();
    ctx.moveTo(32+Math.cos(a)*11,32+Math.sin(a)*11);
    ctx.lineTo(32+Math.cos(a)*( 11+len),32+Math.sin(a)*(11+len));
    ctx.stroke();
  }
  ctx.beginPath(); ctx.arc(32,32,28,0,Math.PI*2);
  ctx.strokeStyle='rgba(160,100,40,0.45)'; ctx.lineWidth=2; ctx.stroke();
  const tex=new THREE.CanvasTexture(c);
  HOLE_MAT=new THREE.MeshBasicMaterial({map:tex,transparent:true,depthWrite:false,polygonOffset:true,polygonOffsetFactor:-4,polygonOffsetUnits:-4});
  HOLE_GEO=new THREE.PlaneGeometry(0.22,0.22);
}

function spawnDecal(point,normal){
  if(!HOLE_MAT) return;
  if(bulletHoles.length>=DECAL_MAX){const old=bulletHoles.shift();scene.remove(old);}
  const mesh=new THREE.Mesh(HOLE_GEO,HOLE_MAT.clone());
  mesh.position.copy(point).addScaledVector(normal,0.012);
  // Align plane to face along normal
  const up=new THREE.Vector3(0,1,0);
  if(Math.abs(normal.dot(up))>0.99) up.set(1,0,0);
  mesh.quaternion.setFromUnitVectors(new THREE.Vector3(0,0,1),normal);
  mesh.rotateZ(Math.random()*Math.PI*2);
  mesh.userData.spawnT=performance.now();
  scene.add(mesh);
  bulletHoles.push(mesh);
}

function tickDecals(now){
  for(let i=bulletHoles.length-1;i>=0;i--){
    const h=bulletHoles[i];
    const age=(now-h.userData.spawnT)/1000;
    const life=15;
    if(age>life){scene.remove(h);bulletHoles.splice(i,1);continue;}
    if(age>life-3) h.material.opacity=Math.max(0,(life-age)/3);
  }
}

// ══ WEAPON MODELS ══
// Material helpers
const mm =c=>new THREE.MeshLambertMaterial({color:c});
const mph=(c,s=80,em=0)=>new THREE.MeshPhongMaterial({color:c,shininess:s,specular:new THREE.Color(0x666666),emissive:em?new THREE.Color(c).multiplyScalar(em):new THREE.Color(0)});
const mDark =(c)=>mph(c,110,0);   // polished dark metal
const mMetal=(c)=>mph(c,90,0);    // standard metal
const mBrt  =(c)=>mph(c,160,0);   // bright polished
const mPoly =(c)=>mph(c,18,0);    // polymer/grip
const mWood =(c)=>mph(c,12,0);    // wood stock
const EDGE=0xdddddd, DARK_EDGE=0x888888;

// Lathe barrel profile helper: returns a Group with a lathe-profiled barrel on Z axis
function mkBarrel(profile,segs,mat,x,y,z){
  const pts=profile.map(([r,d])=>new THREE.Vector2(r,d));
  const geo=new THREE.LatheGeometry(pts,segs);
  const m=new THREE.Mesh(geo,mat);
  // LatheGeometry revolves around Y; rotate so barrel points along -Z
  m.rotation.x=Math.PI/2; m.position.set(x,y,z); return m;
}

// Chamfer edge strip - thin bright line along an edge for metal catch-light effect
function edge(g,w,h,d,col,x,y,z){
  const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),mBrt(col));
  m.position.set(x,y,z); g.add(m);
}

function mkPistol(c1,c2){
  const g=new THREE.Group();
  const sM=mDark(c1), fM=mPoly(c2), mM=mMetal(0xaaaaaa), dM=mph(0x111111,20);
  // ── Slide ──
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.092,0.365),sM); slide.position.set(0,0.054,0); g.add(slide);
  // Slide top chamfer edges
  edge(g,0.004,0.004,0.365,EDGE, 0.034, 0.1,  0);
  edge(g,0.004,0.004,0.365,EDGE,-0.034, 0.1,  0);
  // Rear serrations (9 cuts on back 1/3 of slide)
  for(let i=0;i<9;i++){
    const s=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.094,0.005),dM);
    s.position.set(0,0.054, 0.125+i*0.013); g.add(s);
  }
  // Front cocking serrations (4 cuts)
  for(let i=0;i<4;i++){
    const s=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.094,0.005),dM);
    s.position.set(0,0.054,-0.135+i*0.013); g.add(s);
  }
  // Ejection port (recessed dark cut on right side)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.034,0.09),dM);
  ep.position.set(0.036,0.054,-0.03); g.add(ep);
  // Loaded chamber indicator bump
  const lci=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.008,0.018),sM);
  lci.position.set(0.036,0.086,0.04); g.add(lci);
  // ── Frame / lower receiver ──
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.066,0.052,0.3),fM); fr.position.set(0,-0.001,0.017); g.add(fr);
  // Dust cover (below barrel on frame front)
  const dc=new THREE.Mesh(new THREE.BoxGeometry(0.066,0.03,0.1),fM); dc.position.set(0,-0.011,-0.12); g.add(dc);
  // Underbarrel rail (Picatinny)
  const rail=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.01,0.088),mMetal(0x222222)); rail.position.set(0,-0.027,-0.12); g.add(rail);
  for(let i=0;i<4;i++){
    const t=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.005,0.004),dM); t.position.set(0,-0.022,-0.15+i*0.02); g.add(t);
  }
  // ── Barrel (lathe profile) ──
  const brlPts=[[0,0],[0.024,0],[0.024,0.04],[0.018,0.06],[0.018,0.23],[0.02,0.245],[0.02,0.27]];
  const brl=mkBarrel(brlPts,10,mDark(c1),0,0.048,-0.26);
  g.add(brl);
  // Muzzle crown ring
  const crown=new THREE.Mesh(new THREE.TorusGeometry(0.021,0.004,8,16),mBrt(EDGE));
  crown.rotation.x=Math.PI/2; crown.position.set(0,0.048,-0.39); g.add(crown);
  // ── Grip ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.22,0.128),fM); gr.position.set(0,-0.112,0.095); gr.rotation.x=0.13; g.add(gr);
  // Grip side panels (slightly raised texture look)
  [-0.032,0.032].forEach(sx=>{
    const panel=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.19,0.105),mph(new THREE.Color(c2).addScalar(-0.08).getHex(),10));
    panel.position.set(sx,-0.108,0.095); panel.rotation.x=0.13; g.add(panel);
    // Grip stippling rows
    for(let i=0;i<12;i++){
      const row=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.003,0.1),dM);
      row.position.set(sx,-0.03-i*0.014,0.092); row.rotation.x=0.13; g.add(row);
    }
  });
  // Backstrap
  const bs=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.19,0.018),fM); bs.position.set(0,-0.11,0.158); bs.rotation.x=0.13; g.add(bs);
  // Magazine release button
  const mr=new THREE.Mesh(new THREE.CylinderGeometry(0.009,0.009,0.012,8),mMetal(0x444444));
  mr.rotation.z=Math.PI/2; mr.position.set(0.038,-0.024,0.06); g.add(mr);
  // ── Trigger guard ──
  // Front bar
  const tgF=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.044,0.012),fM); tgF.position.set(0,-0.025,-0.025); g.add(tgF);
  // Bottom arc
  const tgArc=new THREE.Mesh(new THREE.TorusGeometry(0.028,0.006,5,14,Math.PI),fM);
  tgArc.rotation.x=Math.PI/2; tgArc.position.set(0,-0.02,0.016); g.add(tgArc);
  // Trigger finger groove
  const tfg=new THREE.Mesh(new THREE.BoxGeometry(0.068,0.014,0.022),dM); tfg.position.set(0,-0.038,-0.025); g.add(tfg);
  // ── Trigger ──
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.03,0.01),mMetal(0x888888));
  trig.position.set(0,-0.016,0.014); trig.rotation.x=0.32; g.add(trig);
  const trigCurve=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.014,0.018),mMetal(0x888888));
  trigCurve.position.set(0,-0.028,0.003); g.add(trigCurve);
  // ── Sights ──
  // Front sight post with hood
  const fs=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.02,0.01),mM);  fs.position.set(0,0.102,-0.165); g.add(fs);
  const fsHood=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.005,0.012),mM); fsHood.position.set(0,0.112,-0.162); g.add(fsHood);
  // Rear U-notch sight
  const rsL=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.018,0.01),mM); rsL.position.set(-0.016,0.1,0.155); g.add(rsL);
  const rsR=rsL.clone(); rsR.position.x=0.016; g.add(rsR);
  const rsB=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.008,0.01),mM); rsB.position.set(0,0.09,0.155); g.add(rsB);
  // Tritium dot (green glow)
  const dot=new THREE.Mesh(new THREE.SphereGeometry(0.004,6,4),mph(0x00ff44,20,0.8)); dot.position.set(0,0.104,-0.165); g.add(dot);
  // ── Hammer ──
  const hm=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.024,0.016),sM); hm.position.set(0,0.065,0.168); g.add(hm);
  const hmSpur=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.01,0.012),sM); hmSpur.position.set(0,0.08,0.162); g.add(hmSpur);
  // ── Magazine ──
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.19,0.118),mDark(new THREE.Color(c2).addScalar(-0.15).getHex()));
  mag.position.set(0,-0.207,0.09); mag.rotation.x=0.13; g.add(mag);
  // Witness holes on magazine
  for(let i=0;i<4;i++){
    const wh=new THREE.Mesh(new THREE.CylinderGeometry(0.005,0.005,0.06,6),dM);
    wh.rotation.z=Math.PI/2; wh.position.set(0,-0.118-i*0.025,0.09+i*0.003); wh.rotation.x=0.13; g.add(wh);
  }
  // Mag base plate
  const mbp=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.012,0.128),mMetal(0x444444));
  mbp.position.set(0,-0.307,0.09); mbp.rotation.x=0.13; g.add(mbp);
  return g;
}

function mkKnife(model,c1,c2){
  const g=new THREE.Group();
  const bladeM=mDark(c1), edgeM=mBrt(EDGE), grpM=mPoly(c2), metM=mMetal(0x999999);
  const dM=mph(0x0a0a0a,20);

  if(model==='karambit'){
    // ═══ KARAMBIT ═══ - curved claw blade
    // Recurved blade body (built from 3 rotated segments to fake the curve)
    const segs=[
      {w:0.024,h:0.007,d:0.1,  rx:0,    rz:0,    x:0.032, y:0.025,z:-0.01},
      {w:0.022,h:0.006,d:0.1,  rx:0.2,  rz:-0.3, x:0.018, y:0.028,z:-0.1},
      {w:0.016,h:0.005,d:0.08, rx:0.45, rz:-0.6, x:-0.005,y:0.018,z:-0.175},
    ];
    segs.forEach(({w,h,d,rx,rz,x,y,z})=>{
      const b=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),bladeM); b.position.set(x,y,z); b.rotation.set(rx,0,rz); g.add(b);
      // Edge bevel (bright line on cutting edge)
      const be=new THREE.Mesh(new THREE.BoxGeometry(w*0.8,0.002,d),edgeM); be.position.set(x,y-h/2+0.001,z); be.rotation.set(rx,0,rz); g.add(be);
    });
    // Blade tip
    const tip=new THREE.Mesh(new THREE.ConeGeometry(0.008,0.04,4),bladeM); tip.rotation.set(Math.PI*0.9,0,-0.85); tip.position.set(-0.022,0.01,-0.218); g.add(tip);
    // Fuller groove along top of blade
    const full=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.003,0.22),mph(new THREE.Color(c1).addScalar(0.2).getHex(),120));
    full.position.set(0.025,0.028,-0.08); g.add(full);
    // Spine serrations (5 serrations on spine)
    for(let i=0;i<5;i++){
      const sr=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.008,0.007),bladeM);
      sr.position.set(0.025,0.032,-0.04-i*0.02); sr.rotation.z=0.5; g.add(sr);
    }
    // Guard / finger groove
    const grd=new THREE.Mesh(new THREE.CylinderGeometry(0.018,0.014,0.04,10),metM);
    grd.rotation.x=Math.PI/2; grd.position.set(0,0.01,0.04); g.add(grd);
    // Handle (cylinder with flare)
    const hnd=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.022,0.15,10),grpM);
    hnd.rotation.x=Math.PI/2; hnd.position.set(0,0,0.125); g.add(hnd);
    // Handle texture rings
    for(let i=0;i<6;i++){
      const r=new THREE.Mesh(new THREE.TorusGeometry(0.027,0.003,6,12),mph(new THREE.Color(c2).addScalar(-0.15).getHex(),20));
      r.rotation.x=Math.PI/2; r.position.set(0,0,0.055+i*0.022); g.add(r);
    }
    // Signature karambit ring
    const ring=new THREE.Mesh(new THREE.TorusGeometry(0.032,0.007,10,22),metM); ring.position.set(0,0,0.206); g.add(ring);
    // Ring inner knurl
    for(let i=0;i<8;i++){
      const k=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.006,0.012),mph(0x666666,60));
      k.rotation.x=Math.PI/2; k.rotation.y=i*Math.PI/4;
      k.position.set(Math.cos(i*Math.PI/4)*0.032, Math.sin(i*Math.PI/4)*0.032, 0.206); g.add(k);
    }

  } else if(model==='butterfly'){
    // ═══ BALISONG / BUTTERFLY KNIFE ═══
    // Blade – clip-point, narrow
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.004,0.33),bladeM); bld.position.set(0,0.003,-0.1); g.add(bld);
    // Blade bevel (bright flat)
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.002,0.3),edgeM); bev.position.set(0,0.001,-0.09); g.add(bev);
    // Clip point taper
    const clip=new THREE.Mesh(new THREE.ConeGeometry(0.008,0.045,4),bladeM); clip.rotation.x=Math.PI/2; clip.rotation.z=Math.PI/4; clip.position.set(0,0.003,-0.285); g.add(clip);
    // Ricasso
    const ric=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.009,0.038),bladeM); ric.position.set(0,0.004,0.065); g.add(ric);
    // Thumb stud
    const ts=new THREE.Mesh(new THREE.CylinderGeometry(0.005,0.005,0.018,8),metM); ts.rotation.z=Math.PI/2; ts.position.set(0,0.009,0.04); g.add(ts);

    // Two skeletonized handles — balisong open/fanned position
    const makeHandle=(side)=>{
      const sg=side===1?1:-1;
      const hg=new THREE.Group();
      // Main channel bar
      const bar=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.046,0.27),grpM);
      bar.position.set(0,0,0);
      // Skeleton holes (3 large lightening holes)
      for(let i=0;i<3;i++){
        const hole=new THREE.Mesh(new THREE.CylinderGeometry(0.013,0.013,0.019,10),dM);
        hole.rotation.z=Math.PI/2; hole.position.set(0,0,-0.068+i*0.055); bar.add(hole);
      }
      hg.add(bar);
      // Spine (slightly different shade)
      const spine=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.05,0.27),mBrt(new THREE.Color(c2).addScalar(0.15).getHex()));
      spine.position.set(sg*0.0105,0,0); hg.add(spine);
      // Tang pins (3 structural pins)
      for(let i=0;i<3;i++){
        const pin=new THREE.Mesh(new THREE.CylinderGeometry(0.003,0.003,0.022,6),metM);
        pin.rotation.z=Math.PI/2; pin.position.set(0,0,-0.05+i*0.07); hg.add(pin);
      }
      // Latch at butt
      const latch=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.012,0.022),metM); latch.position.set(0,-0.026,0.128); hg.add(latch);
      // Butt cap
      const cap=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.05,0.018),metM); cap.position.set(0,0,0.138); hg.add(cap);

      hg.position.set(sg*0.026,0,0.122);
      hg.rotation.z=sg*0.14;
      hg.children[0].userData.bfH=side;
      g.add(hg);
    };
    makeHandle(1); makeHandle(-1);
    // Pivot pins
    [0.063,0.075].forEach(pz=>{
      const pin=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.08,8),metM);
      pin.rotation.z=Math.PI/2; pin.position.set(0,0,pz); g.add(pin);
    });

  } else if(model==='shadow'){
    // ═══ SHADOW DAGGER – sleek, tanto-like dark blade ═══
    // Main blade body
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.006,0.29),bladeM); bld.position.set(0,0.01,-0.08); g.add(bld);
    // Edge bevel
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.002,0.27),edgeM); bev.position.set(0,0.007,-0.075); g.add(bev);
    // Tanto flat (secondary bevel near tip, forms the tanto angle)
    const tanto=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.006,0.07),bladeM); tanto.position.set(0,0.01,-0.225); tanto.rotation.x=-0.25; g.add(tanto);
    const tantoEdge=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.002,0.065),edgeM); tantoEdge.position.set(0,0.008,-0.225); tantoEdge.rotation.x=-0.25; g.add(tantoEdge);
    // Tanto tip
    const tip=new THREE.Mesh(new THREE.ConeGeometry(0.009,0.035,4),bladeM); tip.rotation.x=Math.PI/2; tip.rotation.z=Math.PI/4; tip.position.set(0,0.01,-0.264); g.add(tip);
    // Blood groove / fuller
    const full=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.003,0.24),mph(new THREE.Color(c2).addScalar(0.3).getHex(),140)); full.position.set(0,0.014,-0.075); g.add(full);
    // Spine ridge
    const spine=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.006,0.27),mDark(new THREE.Color(c1).addScalar(0.15).getHex())); spine.position.set(0,0.016,-0.08); g.add(spine);
    // Ricasso with thumb ramp
    const ric=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.014,0.04),bladeM); ric.position.set(0,0.01,0.06); g.add(ric);
    const thumbRamp=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.006,0.024),bladeM); thumbRamp.position.set(0,0.019,0.052); thumbRamp.rotation.x=-0.3; g.add(thumbRamp);
    // Guard with quillons
    const grd=new THREE.Mesh(new THREE.BoxGeometry(0.078,0.014,0.02),metM); grd.position.set(0,0.009,0.083); g.add(grd);
    const qL=new THREE.Mesh(new THREE.ConeGeometry(0.006,0.028,6),metM); qL.position.set(-0.044,0.009,0.083); qL.rotation.z=-Math.PI/2; g.add(qL);
    const qR=qL.clone(); qR.position.x=0.044; qR.rotation.z=Math.PI/2; g.add(qR);
    // Handle with G10 texture look
    const h=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.022,0.195),grpM); h.position.set(0,0,0.186); g.add(h);
    // Exposed liners (metal side plates)
    [-0.015,0.015].forEach(sx=>{
      const liner=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.024,0.195),metM); liner.position.set(sx,0,0.186); g.add(liner);
    });
    // Grip texture rows
    for(let i=0;i<7;i++){
      const r=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.004,0.014),dM); r.position.set(0,0.013,0.097+i*0.026); g.add(r);
    }
    // Pommel with lanyard hole
    const pm=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.028,0.026),metM); pm.position.set(0,0,0.289); g.add(pm);
    const lh=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.036,8),dM); lh.rotation.z=Math.PI/2; lh.position.set(0,0,0.289); g.add(lh);

  } else {
    // ═══ STANDARD KNIFE – tactical drop-point ═══
    // Blade body
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.007,0.31),bladeM); bld.position.set(0,0.012,-0.08); g.add(bld);
    // Hollow grind (bright edge bevel)
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.002,0.28),edgeM); bev.position.set(0,0.008,-0.076); g.add(bev);
    // Drop point tip
    const tipRamp=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.007,0.075),bladeM); tipRamp.position.set(0,0.012,-0.243); tipRamp.rotation.x=0.28; g.add(tipRamp);
    const tip=new THREE.Mesh(new THREE.ConeGeometry(0.01,0.052,5),bladeM); tip.rotation.x=Math.PI/2; tip.rotation.z=Math.PI*0.2; tip.position.set(0,0.012,-0.275); g.add(tip);
    // Fuller groove
    const full=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.003,0.25),mph(0xdddddd,140)); full.position.set(0,0.015,-0.073); g.add(full);
    // Spine
    const spine=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.006,0.3),mDark(new THREE.Color(c1).addScalar(0.12).getHex())); spine.position.set(0,0.018,-0.077); g.add(spine);
    // Thumb jimping on spine (5 notches)
    for(let i=0;i<5;i++){
      const j=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.008,0.007),dM); j.position.set(0,0.022,-0.08+i*0.022); j.rotation.z=0.5; g.add(j);
    }
    // Ricasso
    const ric=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.016,0.042),bladeM); ric.position.set(0,0.011,0.055); g.add(ric);
    // Guard
    const grd=new THREE.Mesh(new THREE.BoxGeometry(0.09,0.014,0.022),metM); grd.position.set(0,0.009,0.078); g.add(grd);
    // Guard bolsters (rounded ends)
    [-0.048,0.048].forEach(gx=>{
      const b=new THREE.Mesh(new THREE.SphereGeometry(0.012,8,6),metM); b.position.set(gx,0.009,0.078); g.add(b);
    });
    // Handle with integral texture
    const h=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.024,0.195),grpM); h.position.set(0,0,0.178); g.add(h);
    // Bolster front
    const bolF=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.026,0.02),metM); bolF.position.set(0,0,0.082); g.add(bolF);
    // Handle grip scales (two side panels)
    [-0.016,0.016].forEach(sx=>{
      const sc=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.026,0.17),mph(new THREE.Color(c2).addScalar(-0.1).getHex(),12));
      sc.position.set(sx,0,0.172); g.add(sc);
      for(let i=0;i<8;i++){
        const r=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.004,0.012),dM); r.position.set(sx,0.013,0.092+i*0.02); g.add(r);
      }
    });
    // Pommel / butt cap
    const pm=new THREE.Mesh(new THREE.BoxGeometry(0.036,0.028,0.024),metM); pm.position.set(0,0,0.276); g.add(pm);
    edge(g,0.004,0.004,0.024,EDGE, 0.018,0.014,0.276);
    edge(g,0.004,0.004,0.024,EDGE,-0.018,0.014,0.276);
  }
  return g;
}

function mkSMG(c1,c2){
  const g=new THREE.Group();
  const rcvM=mDark(c1), grpM=mPoly(c2), metM=mMetal(0x888888), dM=mph(0x0d0d0d,20), rlM=mMetal(0x333333);
  // ── Upper receiver ──
  const upper=new THREE.Mesh(new THREE.BoxGeometry(0.066,0.078,0.38),rcvM); g.add(upper);
  // Upper receiver top chamfer
  edge(g,0.004,0.004,0.38,DARK_EDGE, 0.033, 0.039, 0);
  edge(g,0.004,0.004,0.38,DARK_EDGE,-0.033, 0.039, 0);
  // Cocking tube (on left side – MP5 style knob slot)
  const ckSlot=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.018,0.12),dM); ckSlot.position.set(-0.033,0.015,-0.05); g.add(ckSlot);
  const ckKnob=new THREE.Mesh(new THREE.CylinderGeometry(0.012,0.012,0.018,8),metM); ckKnob.rotation.z=Math.PI/2; ckKnob.position.set(-0.048,0.015,-0.05); g.add(ckKnob);
  // Ejection port (right side)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.032,0.066),dM); ep.position.set(0.033,0.01,-0.02); g.add(ep);
  // ── Barrel & shroud ──
  // Main barrel (lathe profile – MP5 exposed barrel section)
  const brlPts=[[0,0],[0.022,0],[0.022,0.02],[0.019,0.04],[0.019,0.21],[0.021,0.22],[0.021,0.24]];
  const brl=mkBarrel(brlPts,10,mDark(c1),0,0.012,-0.34); g.add(brl);
  // Three-lug muzzle device
  for(let i=0;i<3;i++){
    const lug=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.008,0.022),metM);
    const a=i/3*Math.PI*2; lug.rotation.x=a; lug.position.set(Math.sin(a)*0.024,0.012+Math.cos(a)*0.024,-0.43); g.add(lug);
  }
  // Barrel nut
  const bn=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.025,0.022,10),metM); bn.rotation.x=Math.PI/2; bn.position.set(0,0.012,-0.2); g.add(bn);
  // Handguard (polymer, vented)
  const hg=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.052,0.135),grpM); hg.position.set(0,0.008,-0.213); g.add(hg);
  // Handguard vents (4 pairs)
  for(let i=0;i<4;i++){
    const v=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.012,0.011),dM); v.position.set(0,-0.01,-0.173+i*0.028); g.add(v);
    const vT=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.016,0.011),dM); vT.position.set(0,0.021,-0.173+i*0.028); g.add(vT);
  }
  // ── Picatinny top rail ──
  const rail=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.008,0.36),rlM); rail.position.set(0,0.043,0.01); g.add(rail);
  for(let i=0;i<12;i++){
    const t=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.005,0.005),dM); t.position.set(0,0.046,-0.15+i*0.03); g.add(t);
  }
  // Flip-up front sight
  const fsa=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.024,0.014),metM); fsa.position.set(0,0.052,-0.3); g.add(fsa);
  const fsp=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.02,0.005),metM); fsp.position.set(0,0.07,-0.3); g.add(fsp);
  // Rear aperture sight
  const rs=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.02,0.014),metM); rs.position.set(0,0.052,0.17); g.add(rs);
  const ra=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.016,10),dM); ra.rotation.x=Math.PI/2; ra.position.set(0,0.062,0.17); g.add(ra);
  // ── Lower receiver ──
  const lower=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.042,0.28),grpM); lower.position.set(0,-0.06,0.05); g.add(lower);
  // Trigger group housing
  const tgh=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.046,0.09),grpM); tgh.position.set(0,-0.062,-0.02); g.add(tgh);
  // Selector switch (left side)
  const sel=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.008,0.014,6),metM); sel.rotation.z=Math.PI/2; sel.position.set(-0.037,-0.048,-0.006); g.add(sel);
  const selLever=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.005,0.022),metM); selLever.position.set(-0.044,-0.044,-0.006); g.add(selLever);
  // Trigger guard (polymer)
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.026,0.006,5,14,Math.PI),grpM); tg.rotation.x=Math.PI/2; tg.position.set(0,-0.054,-0.019); g.add(tg);
  const tgF=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.042,0.012),grpM); tgF.position.set(0,-0.05,-0.055); g.add(tgF);
  // Trigger (curved)
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.028,0.012),metM); trig.position.set(0,-0.044,-0.02); trig.rotation.x=0.22; g.add(trig);
  // ── Pistol grip ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.112,0.072),grpM); gr.position.set(0,-0.114,-0.038); gr.rotation.x=0.14; g.add(gr);
  // Grip finger grooves
  for(let i=0;i<4;i++){
    const fg=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.008,0.018),dM); fg.position.set(0,-0.065-i*0.022,-0.025); g.add(fg);
  }
  // Grip cap with screw
  const gc=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.014,0.076),grpM); gc.position.set(0,-0.172,-0.035); g.add(gc);
  // ── Curved magazine ──
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.035,0.175,0.064),grpM); mag.position.set(0,-0.168,-0.038); g.add(mag);
  // Magazine curve
  const mc=new THREE.Mesh(new THREE.BoxGeometry(0.035,0.022,0.068),grpM); mc.position.set(0,-0.245,-0.03); mc.rotation.x=0.2; g.add(mc);
  // Witness holes
  for(let i=0;i<3;i++){
    const wh=new THREE.Mesh(new THREE.CylinderGeometry(0.005,0.005,0.038,6),dM); wh.rotation.z=Math.PI/2; wh.position.set(0,-0.11-i*0.03,-0.04); g.add(wh);
  }
  // Mag baseplate
  const mbp=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.012,0.07),metM); mbp.position.set(0,-0.263,-0.032); g.add(mbp);
  // ── Stock (retractable skeleton style) ──
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.014,0.19),grpM); stk.position.set(0,-0.018,0.285); g.add(stk);
  const stkT=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.055,0.016),grpM); stkT.position.set(0,0.014,0.38); g.add(stkT);
  const stkB=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.055,0.016),grpM); stkB.position.set(0,-0.01,0.38); g.add(stkB);
  // Stock spine rod
  const sr=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.19,6),metM); sr.rotation.x=Math.PI/2; sr.position.set(0,0.025,0.285); g.add(sr);
  // Butt pad
  const bp=new THREE.Mesh(new THREE.BoxGeometry(0.042,0.072,0.014),mph(0x1a1a1a,10)); bp.position.set(0,0.006,0.393); g.add(bp);
  return g;
}

function mkShotgun(c1,c2){
  const g=new THREE.Group();
  const rcvM=mDark(c1), wdM=mWood(c2), metM=mMetal(0x888888), dM=mph(0x0d0d0d,20), blueM=mMetal(0x334466);
  // ── Receiver ──
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.075,0.085,0.245),rcvM); rcv.position.set(0,0,0.078); g.add(rcv);
  // Receiver top flat with rib channel
  const ribCh=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.006,0.22),dM); ribCh.position.set(0,0.046,0.082); g.add(ribCh);
  // Receiver side engravings (decorative inlays)
  [-0.038,0.038].forEach(sx=>{
    const eng=new THREE.Mesh(new THREE.BoxGeometry(0.003,0.058,0.18),mph(new THREE.Color(c1).addScalar(0.18).getHex(),120));
    eng.position.set(sx,0.004,0.082); g.add(eng);
  });
  // Ejection ports (both sides)
  [-0.038,0.038].forEach(sx=>{
    const ep=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.028,0.072),dM); ep.position.set(sx,0.01,0.06); g.add(ep);
  });
  // ── Over/Under twin barrels ──
  // Bottom barrel
  const brl1pts=[[0,0],[0.021,0],[0.021,0.03],[0.018,0.05],[0.018,0.42],[0.02,0.43],[0.02,0.46]];
  const brl1=mkBarrel(brl1pts,10,blueM,0,-0.01,-0.485); g.add(brl1);
  // Top barrel
  const brl2=mkBarrel(brl1pts,10,blueM,0,0.032,-0.485); g.add(brl2);
  // Muzzle rings (both barrels)
  [-0.01,0.032].forEach(by=>{
    const mr=new THREE.Mesh(new THREE.TorusGeometry(0.022,0.004,8,16),metM); mr.rotation.x=Math.PI/2; mr.position.set(0,by,-0.53); g.add(mr);
  });
  // Center rib between barrels (ventilated)
  const rib=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.007,0.44),metM); rib.position.set(0,0.011,-0.27); g.add(rib);
  // Rib ventilation cuts
  for(let i=0;i<9;i++){
    const rv=new THREE.Mesh(new THREE.BoxGeometry(0.009,0.004,0.008),dM); rv.position.set(0,0.013,-0.07-i*0.04); g.add(rv);
  }
  // Barrel selector bead (top of rib at muzzle end)
  const bead=new THREE.Mesh(new THREE.SphereGeometry(0.007,8,6),mph(0xffd700,160)); bead.position.set(0,0.04,-0.49); g.add(bead);
  // Barrel-to-receiver wedge
  const wed=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.052,0.025),rcvM); wed.position.set(0,0.011,-0.078); g.add(wed);
  // Locking bolt (top lever concept, simplified)
  const lv=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.01,0.04),metM); lv.position.set(0,0.048,0.1); g.add(lv);
  const lvN=new THREE.Mesh(new THREE.SphereGeometry(0.008,6,5),metM); lvN.position.set(0,0.052,0.12); g.add(lvN);
  // ── Forend ──
  const fore=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.045,0.18),wdM); fore.position.set(0,-0.01,-0.18); g.add(fore);
  // Forend checkering
  for(let i=0;i<5;i++){
    const r=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.005,0.012),dM); r.position.set(0,-0.014,-0.105+i*0.03); g.add(r);
  }
  // Forend to barrel latch
  const flt=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.008,0.062,8),metM); flt.rotation.z=Math.PI/2; flt.position.set(0,-0.015,-0.075); g.add(flt);
  // ── Pistol grip ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.052,0.11,0.072),wdM); gr.position.set(0,-0.066,0.098); gr.rotation.x=0.16; g.add(gr);
  // Grip checkering panels
  [-0.027,0.027].forEach(sx=>{
    for(let i=0;i<6;i++){
      const r=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.008,0.014),dM); r.position.set(sx,-0.028-i*0.016,0.097); r.rotation.x=0.16; g.add(r);
    }
  });
  // Grip cap
  const gc=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.014,0.076),rcvM); gc.position.set(0,-0.124,0.096); gc.rotation.x=0.16; g.add(gc);
  // ── Full stock ──
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.078,0.32),wdM); stk.position.set(0,-0.007,0.24); g.add(stk);
  // Stock comb (raised cheekpiece)
  const comb=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.032,0.22),wdM); comb.position.set(0,0.053,0.26); g.add(comb);
  // Stock toe (narrowing at butt)
  const toe=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.068,0.12),wdM); toe.position.set(0,-0.01,0.35); g.add(toe);
  // Butt plate (black rubber)
  const bp=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.09,0.016),mph(0x111111,8)); bp.position.set(0,-0.002,0.41); g.add(bp);
  // Stock bolt recess (metal trim)
  const sbr=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.008,0.018,8),metM); sbr.rotation.x=Math.PI/2; sbr.position.set(0,0.02,0.405); g.add(sbr);
  // ── Triggers (double) ──
  const t1=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.028,0.012),metM); t1.position.set(0,-0.04,0.07); t1.rotation.x=0.28; g.add(t1);
  const t2=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.026,0.012),metM); t2.position.set(0,-0.04,0.088); t2.rotation.x=0.28; g.add(t2);
  // Trigger guard (ornate)
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.03,0.006,6,16,Math.PI),metM); tg.rotation.x=Math.PI/2; tg.position.set(0,-0.033,0.082); g.add(tg);
  // Trigger plate (floor plate)
  const tp=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.008,0.1),rcvM); tp.position.set(0,-0.048,0.075); g.add(tp);
  return g;
}
function mkRevolver(c1,c2){
  const g=new THREE.Group();
  const frM=mDark(c1), grpM=mPoly(c2), metM=mMetal(0x999999), dM=mph(0x0a0a0a,20), cylM=mMetal(c1);
  // ── Barrel (full underlug style) ──
  // Main barrel tube
  const brlPts=[[0,0],[0.022,0],[0.022,0.015],[0.02,0.025],[0.02,0.25],[0.022,0.265],[0.022,0.28]];
  const brl=mkBarrel(brlPts,10,mDark(c1),0,0.042,-0.12); g.add(brl);
  // Underlug (thick rib beneath barrel)
  const ulug=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.032,0.27),mDark(c1)); ulug.position.set(0,0.008,-0.115); g.add(ulug);
  // Ejector rod inside underlug
  const ejr=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.007,0.22,8),metM); ejr.rotation.x=Math.PI/2; ejr.position.set(0,0.01,-0.105); g.add(ejr);
  const ejrT=new THREE.Mesh(new THREE.CylinderGeometry(0.012,0.012,0.016,8),metM); ejrT.rotation.x=Math.PI/2; ejrT.position.set(0,0.01,-0.016); g.add(ejrT);
  // Muzzle crown + muzzle brake ribs
  const crown=new THREE.Mesh(new THREE.TorusGeometry(0.023,0.004,8,18),metM); crown.rotation.x=Math.PI/2; crown.position.set(0,0.042,-0.26); g.add(crown);
  for(let i=0;i<3;i++){
    const r=new THREE.Mesh(new THREE.CylinderGeometry(0.025,0.025,0.008,14),metM); r.rotation.x=Math.PI/2; r.position.set(0,0.042,-0.2-i*0.02); g.add(r);
  }
  // Front sight (ramp) 
  const fsRamp=new THREE.Mesh(new THREE.BoxGeometry(0.009,0.014,0.024),metM); fsRamp.position.set(0,0.066,-0.225); g.add(fsRamp);
  const fsPost=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.022,0.005),mph(0xff4400,60,0.3)); fsPost.position.set(0,0.072,-0.23); g.add(fsPost);
  // ── Frame (top strap and side plates) ──
  const ts=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.016,0.31),frM); ts.position.set(0,0.06,-0.07); g.add(ts);
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.075,0.19),frM); fr.position.set(0,0.01,0.045); g.add(fr);
  // Side plate
  [-0.03,0.03].forEach(sx=>{
    const sp=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.065,0.17),mph(new THREE.Color(c1).addScalar(0.1).getHex(),100));
    sp.position.set(sx,0.01,0.05); g.add(sp);
  });
  // Crane (cylinder swing-out arm)
  const crane=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.008,0.072,8),metM); crane.rotation.z=Math.PI/2; crane.position.set(0,0.038,0.1); g.add(crane);
  // ── Cylinder ──
  const cyl=new THREE.Mesh(new THREE.CylinderGeometry(0.052,0.052,0.076,14),cylM); cyl.rotation.x=Math.PI/2; cyl.position.set(0,0.035,0.1); g.add(cyl);
  // 6 chambers
  for(let i=0;i<6;i++){
    const a=i/6*Math.PI*2;
    const ch=new THREE.Mesh(new THREE.CylinderGeometry(0.011,0.011,0.08,8),dM);
    ch.rotation.x=Math.PI/2; ch.position.set(Math.cos(a)*0.034, 0.035+Math.sin(a)*0.034, 0.1); g.add(ch);
    // Cartridge rim (brass)
    const rim=new THREE.Mesh(new THREE.TorusGeometry(0.013,0.003,6,12),mph(0xcc8800,80));
    rim.rotation.x=Math.PI/2; rim.position.set(Math.cos(a)*0.034, 0.035+Math.sin(a)*0.034, 0.14); g.add(rim);
  }
  // Cylinder flutes (between chambers)
  for(let i=0;i<6;i++){
    const a=(i+0.5)/6*Math.PI*2;
    const fl=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.008,0.08),dM);
    fl.position.set(Math.cos(a)*0.05, 0.035+Math.sin(a)*0.05, 0.1); g.add(fl);
  }
  // Cylinder latch
  const cl=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.012,0.018),metM); cl.position.set(-0.034,0.04,0.14); g.add(cl);
  // ── Hammer ──
  const hm=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.038,0.022),frM); hm.position.set(0,0.072,0.15); g.add(hm);
  const hmSpur=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.012,0.028),frM); hmSpur.position.set(0,0.092,0.158); g.add(hmSpur);
  // Hammer checkering
  for(let i=0;i<3;i++){
    const hc=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.004,0.024),dM); hc.position.set(0,0.094+i*0.004,0.16-i*0.003); g.add(hc);
  }
  // ── Trigger + guard ──
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.032,0.012),metM); trig.position.set(0,-0.018,0.09); trig.rotation.x=0.28; g.add(trig);
  const tgArc=new THREE.Mesh(new THREE.TorusGeometry(0.026,0.006,6,16,Math.PI),frM); tgArc.rotation.x=Math.PI/2; tgArc.position.set(0,-0.018,0.096); g.add(tgArc);
  // ── Grip (Hogue-style rubber) ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.21,0.125),grpM); gr.position.set(0,-0.108,0.158); gr.rotation.x=-0.14; g.add(gr);
  // Grip finger grooves
  for(let i=0;i<3;i++){
    const fg=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.012,0.022),mph(new THREE.Color(c2).addScalar(-0.15).getHex(),10));
    fg.position.set(0,-0.04-i*0.042,0.148); fg.rotation.x=-0.14; g.add(fg);
  }
  // Backstrap (separate metal piece)
  const bks=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.19,0.018),frM); bks.position.set(0,-0.11,0.218); bks.rotation.x=-0.14; g.add(bks);
  // Grip cap (steel)
  const gcp=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.016,0.13),frM); gcp.position.set(0,-0.215,0.156); gcp.rotation.x=-0.14; g.add(gcp);
  // Rear sight (adjustable)
  const rsB=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.008,0.018),metM); rsB.position.set(0,0.074,0.15); g.add(rsB);
  const rsL=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.018,0.012),metM); rsL.position.set(-0.014,0.079,0.15); g.add(rsL);
  const rsR=rsL.clone(); rsR.position.x=0.014; g.add(rsR);
  // Red insert rear sight
  const rsi=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.006,0.014),mph(0xff3300,60,0.4)); rsi.position.set(0,0.081,0.15); g.add(rsi);
  return g;
}
function mkDeagle(c1,c2){
  const g=new THREE.Group();
  const slM=mDark(c1), frM=mPoly(c2), metM=mMetal(0x999999), dM=mph(0x0d0d0d,20);
  // ── Slide – large, angular, distinctive ──
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.076,0.098,0.34),slM); slide.position.set(0,0.062,0); g.add(slide);
  // Slide top flat with rib
  const stRib=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.007,0.32),mDark(new THREE.Color(c1).addScalar(-0.1).getHex())); stRib.position.set(0,0.113,0.005); g.add(stRib);
  // Top chamfer edges
  edge(g,0.004,0.004,0.34,DARK_EDGE, 0.038, 0.111, 0.005);
  edge(g,0.004,0.004,0.34,DARK_EDGE,-0.038, 0.111, 0.005);
  // Front serrations (7 angled cuts)
  for(let i=0;i<7;i++){
    const s=new THREE.Mesh(new THREE.BoxGeometry(0.078,0.1,0.006),dM); s.position.set(0,0.062,-0.12+i*0.014); g.add(s);
  }
  // Rear serrations (6 wide cuts)
  for(let i=0;i<6;i++){
    const s=new THREE.Mesh(new THREE.BoxGeometry(0.078,0.1,0.007),dM); s.position.set(0,0.062,0.115+i*0.015); g.add(s);
  }
  // Gas-operated rotating barrel port (unique DE feature)
  const gasPort=new THREE.Mesh(new THREE.CylinderGeometry(0.009,0.009,0.022,8),dM); gasPort.position.set(0,0.102,-0.04); g.add(gasPort);
  // Ejection port (large, right side)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.042,0.11),dM); ep.position.set(0.039,0.056,-0.02); g.add(ep);
  // ── Barrel (lathe – thick, rotating style) ──
  const brlPts=[[0,0],[0.028,0],[0.028,0.02],[0.024,0.04],[0.024,0.24],[0.026,0.26],[0.026,0.28]];
  const brl=mkBarrel(brlPts,12,mDark(c1),0,0.044,-0.27); g.add(brl);
  // Muzzle brake – triangular crown
  for(let i=0;i<4;i++){
    const v=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.012,0.018),dM); v.position.set(0,0.044+((i%2===0)?0.03:-0.03),-0.41-i*0.008); g.add(v);
  }
  const crown=new THREE.Mesh(new THREE.TorusGeometry(0.027,0.005,8,18),metM); crown.rotation.x=Math.PI/2; crown.position.set(0,0.044,-0.39); g.add(crown);
  // ── Frame ──
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.054,0.31),frM); fr.position.set(0,0.002,0.01); g.add(fr);
  // Dust cover with integral rail
  const dc=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.032,0.12),frM); dc.position.set(0,-0.013,-0.11); g.add(dc);
  const rail=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.01,0.1),mMetal(0x222222)); rail.position.set(0,-0.024,-0.11); g.add(rail);
  for(let i=0;i<5;i++){const t=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.005,0.005),dM); t.position.set(0,-0.019,-0.155+i*0.018); g.add(t);}
  // ── Grip – large, aggressive ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.228,0.135),frM); gr.position.set(0,-0.117,0.102); gr.rotation.x=0.1; g.add(gr);
  // Grip panels (both sides)
  [-0.033,0.033].forEach(sx=>{
    const panel=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.22,0.12),mph(new THREE.Color(c2).addScalar(-0.12).getHex(),14));
    panel.position.set(sx,-0.115,0.102); panel.rotation.x=0.1; g.add(panel);
    // Grip stipple rows
    for(let i=0;i<10;i++){
      const row=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.004,0.11),dM); row.position.set(sx,-0.035-i*0.019,0.1); row.rotation.x=0.1; g.add(row);
    }
  });
  // Backstrap (thick, metal)
  const bs=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.21,0.022),slM); bs.position.set(0,-0.115,0.164); bs.rotation.x=0.1; g.add(bs);
  // Mag release (large button)
  const mr=new THREE.Mesh(new THREE.CylinderGeometry(0.011,0.011,0.014,8),metM); mr.rotation.z=Math.PI/2; mr.position.set(0.042,-0.028,0.068); g.add(mr);
  // ── Sights (high) ──
  const fs=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.025,0.012),metM); fs.position.set(0,0.124,-0.16); g.add(fs);
  const fsGlow=new THREE.Mesh(new THREE.SphereGeometry(0.005,6,4),mph(0x00ff44,20,0.9)); fsGlow.position.set(0,0.127,-0.162); g.add(fsGlow);
  const rsL=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.022,0.012),metM); rsL.position.set(-0.018,0.122,0.153); g.add(rsL);
  const rsR=rsL.clone(); rsR.position.x=0.018; g.add(rsR);
  const rsGlowL=new THREE.Mesh(new THREE.SphereGeometry(0.004,5,4),mph(0x00ff44,20,0.9)); rsGlowL.position.set(-0.018,0.128,0.153); g.add(rsGlowL);
  const rsGlowR=rsGlowL.clone(); rsGlowR.position.x=0.018; g.add(rsGlowR);
  // ── Trigger + guard ──
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.033,0.007,5,14,Math.PI),frM); tg.rotation.x=Math.PI/2; tg.position.set(0,-0.028,0.042); g.add(tg);
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.032,0.013),metM); trig.position.set(0,-0.02,0.018); trig.rotation.x=0.3; g.add(trig);
  // ── Magazine ──
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.21,0.128),mDark(new THREE.Color(c2).addScalar(-0.2).getHex()));
  mag.position.set(0,-0.228,0.104); mag.rotation.x=0.1; g.add(mag);
  for(let i=0;i<5;i++){
    const wh=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.062,6),dM); wh.rotation.z=Math.PI/2; wh.position.set(0,-0.12-i*0.03,0.1); wh.rotation.x=0.1; g.add(wh);
  }
  const mbp=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.014,0.136),mMetal(0x444444)); mbp.position.set(0,-0.336,0.104); mbp.rotation.x=0.1; g.add(mbp);
  return g;
}
function mkCompact(c1,c2){
  const g=new THREE.Group();
  const rcvM=mDark(c1), grpM=mPoly(c2), metM=mMetal(0x888888), dM=mph(0x0a0a0a,20);
  // ── Main receiver ──
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.066,0.088,0.3),rcvM); rcv.position.set(0,0.01,0.01); g.add(rcv);
  // Top carry handle / sight rail
  const tch=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.018,0.28),rcvM); tch.position.set(0,0.056,0.01); g.add(tch);
  // Top handle aperture sight
  for(let i=0;i<3;i++){
    const ap=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.007,0.065,8),dM); ap.rotation.z=Math.PI/2; ap.position.set(0,0.063,-0.05+i*0.06); g.add(ap);
  }
  // Ejection port (top-eject style)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.008,0.072),dM); ep.position.set(0,0.057,0.02); g.add(ep);
  // ── Short, ported barrel ──
  const brlPts=[[0,0],[0.02,0],[0.02,0.02],[0.017,0.03],[0.017,0.12],[0.019,0.13],[0.019,0.145]];
  const brl=mkBarrel(brlPts,10,mDark(c1),0,0.02,-0.22); g.add(brl);
  // Ported compensator (3 ports on top)
  for(let i=0;i<3;i++){
    const port=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.007,0.072,8),dM); port.position.set(0,0.04,-0.16-i*0.022); g.add(port);
  }
  // Muzzle device
  const mzD=new THREE.Mesh(new THREE.CylinderGeometry(0.022,0.018,0.028,10),metM); mzD.rotation.x=Math.PI/2; mzD.position.set(0,0.02,-0.29); g.add(mzD);
  // ── Cocking handle (side, top) ──
  const ckH=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.02,0.058),rcvM); ckH.position.set(0.039,0.04,-0.06); g.add(ckH);
  const ckT=new THREE.Mesh(new THREE.CylinderGeometry(0.011,0.011,0.018,8),metM); ckT.rotation.z=Math.PI/2; ckT.position.set(0.052,0.04,-0.04); g.add(ckT);
  // ── Pistol grip (very short) ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.096,0.068),grpM); gr.position.set(0,-0.055,0.076); gr.rotation.x=0.08; g.add(gr);
  [-0.028,0.028].forEach(sx=>{
    for(let i=0;i<5;i++){
      const r=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.008,0.014),dM); r.position.set(sx,-0.015-i*0.016,0.076); r.rotation.x=0.08; g.add(r);
    }
  });
  // ── Extended curved magazine ──
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.235,0.06),grpM); mag.position.set(0,-0.183,0.062); g.add(mag);
  const magC=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.022,0.064),grpM); magC.position.set(0,-0.265,0.054); magC.rotation.x=0.18; g.add(magC);
  for(let i=0;i<3;i++){
    const wh=new THREE.Mesh(new THREE.CylinderGeometry(0.005,0.005,0.041,6),dM); wh.rotation.z=Math.PI/2; wh.position.set(0,-0.115-i*0.036,0.063); g.add(wh);
  }
  const mbp=new THREE.Mesh(new THREE.BoxGeometry(0.042,0.013,0.066),metM); mbp.position.set(0,-0.301,0.062); g.add(mbp);
  // ── Folded wire stock ──
  const stkBar=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.16,6),metM); stkBar.rotation.x=Math.PI/2; stkBar.position.set(0,-0.02,0.21); g.add(stkBar);
  const stkTop=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.16,6),metM); stkTop.rotation.x=Math.PI/2; stkTop.position.set(0,0.02,0.21); g.add(stkTop);
  const stkBend=new THREE.Mesh(new THREE.BoxGeometry(0.014,0.048,0.018),metM); stkBend.position.set(0,0,0.295); g.add(stkBend);
  // ── Trigger + guard ──
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.024,0.006,5,12,Math.PI),grpM); tg.rotation.x=Math.PI/2; tg.position.set(0,-0.026,0.026); g.add(tg);
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.026,0.01),metM); trig.position.set(0,-0.014,0.022); trig.rotation.x=0.25; g.add(trig);
  return g;
}
// ══ LEGENDARY SKIN MODELS ══

// MP5 "Void Reactor" — sci-fi energy rifle with glowing core orb and particle vents
function mkSMG_VoidReactor(c1,c2){
  const g=new THREE.Group();
  const glow=new THREE.MeshLambertMaterial({color:c1,emissive:new THREE.Color(c1).multiplyScalar(0.6)});
  const dark=new THREE.MeshLambertMaterial({color:0x040a14});
  const mid=new THREE.MeshLambertMaterial({color:0x112233});
  // Sleek angular receiver
  const body=new THREE.Mesh(new THREE.BoxGeometry(0.065,0.085,0.44),dark);g.add(body);
  // Glowing top channel
  const ch=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.014,0.42),glow);ch.position.set(0,0.05,0.01);g.add(ch);
  // Top rail trim strips (flush)
  const tr1=new THREE.Mesh(new THREE.BoxGeometry(0.067,0.006,0.42),glow);tr1.position.set(0,0.046,0.01);g.add(tr1);
  // Reactor core orb (central sphere)
  const core=new THREE.Mesh(new THREE.SphereGeometry(0.038,10,8),new THREE.MeshLambertMaterial({color:c1,emissive:new THREE.Color(c1).multiplyScalar(0.9)}));
  core.position.set(0,0.02,-0.02);g.add(core);
  // Outer rings around core
  for(let i=0;i<3;i++){
    const ring=new THREE.Mesh(new THREE.TorusGeometry(0.052+i*0.012,0.005,5,18),glow);
    ring.rotation.y=i*Math.PI/3;ring.position.set(0,0.02,-0.02);g.add(ring);
  }
  // Energy barrel — hexagonal prism
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.018,0.022,0.26,6),mid);brl.rotation.x=Math.PI/2;brl.position.set(0,0.02,-0.3);g.add(brl);
  // Barrel emitter tip
  const tip=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.018,0.04,6),glow);tip.rotation.x=Math.PI/2;tip.position.set(0,0.02,-0.44);g.add(tip);
  // Vent fins (3 pairs)
  for(let i=0;i<4;i++){
    const f=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.022,0.012),glow);f.position.set(0,0.055,-0.1+i*0.06);g.add(f);
  }
  // Grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.1,0.068),mid);gr.position.set(0,-0.065,-0.04);gr.rotation.x=0.12;g.add(gr);
  // Magazine — glowing slab
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.16,0.06),new THREE.MeshLambertMaterial({color:0x001122}));mag.position.set(0,-0.14,-0.05);g.add(mag);
  const magEdge=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.16,0.006),glow);magEdge.position.set(0,-0.14,-0.08);g.add(magEdge);
  // Stock spine
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.04,0.06,0.19),mid);stk.position.set(0,-0.008,0.3);g.add(stk);
  const stkGlow=new THREE.Mesh(new THREE.BoxGeometry(0.042,0.01,0.19),glow);stkGlow.position.set(0,0.032,0.3);g.add(stkGlow);
  return g;
}

// Shotgun "Inferno" — dragon head barrel, scales, bone stock
function mkShotgun_Inferno(c1,c2){
  const g=new THREE.Group();
  const fireM=new THREE.MeshLambertMaterial({color:0xFF3300});
  const scaleM=new THREE.MeshLambertMaterial({color:0xAA1100});
  const boneM=new THREE.MeshLambertMaterial({color:0xDDCC99});
  const darkM=new THREE.MeshLambertMaterial({color:0x1a0500});
  // Main body — thick reptilian receiver
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.085,0.1,0.3),scaleM);rcv.position.set(0,0,0.05);g.add(rcv);
  // Scale texture plates
  for(let i=0;i<6;i++){
    const sc=new THREE.Mesh(new THREE.BoxGeometry(0.09,0.034,0.042),new THREE.MeshLambertMaterial({color:i%2===0?0xCC2200:0x991100}));
    sc.position.set(0,0.052+Math.sin(i*0.5)*0.005,-0.06+i*0.04);sc.rotation.x=Math.sin(i*0.4)*0.08;g.add(sc);
  }
  // DRAGON HEAD at muzzle end
  // Snout
  const snout=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.06,0.14),scaleM);snout.position.set(0,0.015,-0.27);g.add(snout);
  // Snout taper
  const sntip=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.04,0.06),fireM);sntip.position.set(0,0.01,-0.36);g.add(sntip);
  // Upper jaw ridge
  const jaw=new THREE.Mesh(new THREE.BoxGeometry(0.065,0.025,0.1),scaleM);jaw.position.set(0,0.05,-0.27);g.add(jaw);
  // Dragon nostrils (barrel openings)
  const n1=new THREE.Mesh(new THREE.CylinderGeometry(0.016,0.018,0.04,6),darkM);n1.rotation.x=Math.PI/2;n1.position.set(0.022,0.018,-0.375);g.add(n1);
  const n2=n1.clone();n2.position.set(-0.022,0.018,-0.375);g.add(n2);
  // Teeth row (bottom)
  for(let i=0;i<5;i++){
    const t=new THREE.Mesh(new THREE.ConeGeometry(0.008,0.022,4),boneM);t.position.set(-0.02+i*0.01,-0.015,-0.28-i*0.012);t.rotation.z=Math.PI;g.add(t);
  }
  // Top horns/spikes on head
  const h1=new THREE.Mesh(new THREE.ConeGeometry(0.009,0.048,4),scaleM);h1.position.set(0.022,0.072,-0.22);h1.rotation.z=-0.3;g.add(h1);
  const h2=new THREE.Mesh(new THREE.ConeGeometry(0.009,0.048,4),scaleM);h2.position.set(-0.022,0.072,-0.22);h2.rotation.z=0.3;g.add(h2);
  // Eye sockets (glowing)
  const eyeM=new THREE.MeshBasicMaterial({color:0xFF8800});
  const e1=new THREE.Mesh(new THREE.SphereGeometry(0.012,6,5),eyeM);e1.position.set(0.034,0.034,-0.24);g.add(e1);
  const e2=e1.clone();e2.position.set(-0.034,0.034,-0.24);g.add(e2);
  // Flame vent glow strips
  for(let i=0;i<3;i++){
    const fl=new THREE.Mesh(new THREE.BoxGeometry(0.015,0.01,0.008),new THREE.MeshLambertMaterial({color:0xFF6600}));
    fl.position.set(0.044,0.0,-0.16+i*0.04);g.add(fl);
    const fl2=fl.clone();fl2.position.x=-0.044;g.add(fl2);
  }
  // Bone-wrapped stock
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.078,0.28),boneM);stk.position.set(0,-0.003,0.26);g.add(stk);
  const stkCrv=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.028,0.18),boneM);stkCrv.position.set(0,0.052,0.28);g.add(stkCrv);
  // Bone knuckle joints on stock
  for(let i=0;i<4;i++){const k=new THREE.Mesh(new THREE.SphereGeometry(0.016,6,5),boneM);k.position.set(0,-0.003,0.13+i*0.062);g.add(k);}
  // Grip with scale texture
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.1,0.072),scaleM);gr.position.set(0,-0.063,0.1);gr.rotation.x=0.15;g.add(gr);
  // Trigger guard (claws)
  const cl1=new THREE.Mesh(new THREE.ConeGeometry(0.007,0.032,4),boneM);cl1.position.set(0.022,-0.06,0.05);cl1.rotation.z=-Math.PI/2+0.4;g.add(cl1);
  const cl2=cl1.clone();cl2.position.x=-0.022;cl2.rotation.z=Math.PI/2-0.4;g.add(cl2);
  return g;
}

// Pistol "Aurora Blight" — crystalline bio-organic pistol with spine and tendril barrel
function mkPistol_Aurora(c1,c2){
  const g=new THREE.Group();
  const crystalM=new THREE.MeshLambertMaterial({color:c1});
  const darkM=new THREE.MeshLambertMaterial({color:0x050510});
  const purpM=new THREE.MeshLambertMaterial({color:c2});
  const glowM=new THREE.MeshLambertMaterial({color:0x00ffaa,emissive:new THREE.Color(0x00ffaa).multiplyScalar(0.4)});
  // Organic curved slide
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.095,0.34),crystalM);slide.position.set(0,0.05,0);g.add(slide);
  // Crystal facet panels on slide
  const fac1=new THREE.Mesh(new THREE.BoxGeometry(0.075,0.03,0.12),purpM);fac1.position.set(0,0.075,-0.08);fac1.rotation.z=0.15;g.add(fac1);
  const fac2=new THREE.Mesh(new THREE.BoxGeometry(0.075,0.03,0.1),purpM);fac2.position.set(0,0.075,0.06);fac2.rotation.z=-0.1;g.add(fac2);
  // Glowing vein lines
  for(let i=0;i<4;i++){
    const v=new THREE.Mesh(new THREE.BoxGeometry(0.074,0.006,0.04),glowM);v.position.set(0,0.04+i*0.015,-0.14+i*0.06);g.add(v);
  }
  // Tendril barrel (twisted organic tube)
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.017,0.02,0.22,7),crystalM);brl.rotation.x=Math.PI/2;brl.position.set(0,0.045,-0.25);g.add(brl);
  // Barbed tip
  const tip=new THREE.Mesh(new THREE.ConeGeometry(0.022,0.055,5),purpM);tip.rotation.x=Math.PI/2;tip.position.set(0,0.045,-0.37);g.add(tip);
  // Spine ridge on top
  for(let i=0;i<6;i++){const s=new THREE.Mesh(new THREE.ConeGeometry(0.007,0.025,4),purpM);s.position.set(0,0.108,-0.12+i*0.04);g.add(s);}
  // Bio-frame
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.065,0.045,0.24),darkM);fr.position.set(0,0.0,0.03);g.add(fr);
  // Tendril grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.2,0.115),purpM);gr.position.set(0,-0.105,0.1);gr.rotation.x=0.12;g.add(gr);
  // Vein lines on grip
  for(let i=0;i<3;i++){const v=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.18,0.004),glowM);v.position.set(-0.018+i*0.018,-0.1,0.155);g.add(v);}
  // Trigger (bone-like)
  const tr=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.005,0.03,5),crystalM);tr.rotation.x=Math.PI/2-0.4;tr.position.set(0,-0.02,0.01);g.add(tr);
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.03,0.006,4,10,Math.PI),darkM);tg.rotation.x=Math.PI/2;tg.position.set(0,-0.025,0.04);g.add(tg);
  return g;
}

// Pistol "Neon Rider" — cyberpunk chrome pistol with neon trim and visor sight
function mkPistol_NeonRider(c1,c2){
  const g=new THREE.Group();
  const chromeM=new THREE.MeshLambertMaterial({color:0xccddee});
  const neon1=new THREE.MeshLambertMaterial({color:c1,emissive:new THREE.Color(c1).multiplyScalar(0.5)});
  const neon2=new THREE.MeshLambertMaterial({color:c2,emissive:new THREE.Color(c2).multiplyScalar(0.5)});
  const darkM=new THREE.MeshLambertMaterial({color:0x080810});
  // Angular chrome slide
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.092,0.38),chromeM);slide.position.set(0,0.055,0);g.add(slide);
  // Neon stripe top
  const ns=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.008,0.38),neon1);ns.position.set(0,0.103,0);g.add(ns);
  // Side panel cutouts filled with neon
  const p1=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.04,0.22),neon2);p1.position.set(0.037,0.055,-0.02);g.add(p1);
  const p2=p1.clone();p2.position.x=-0.037;g.add(p2);
  // Compensator slots
  for(let i=0;i<4;i++){const sl=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.018,0.012),darkM);sl.position.set(0,0.07,-0.16+i*0.028);g.add(sl);}
  // Barrel with neon tip
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.016,0.016,0.15,8),chromeM);brl.rotation.x=Math.PI/2;brl.position.set(0,0.05,-0.27);g.add(brl);
  const btip=new THREE.Mesh(new THREE.CylinderGeometry(0.02,0.016,0.03,8),neon1);btip.rotation.x=Math.PI/2;btip.position.set(0,0.05,-0.35);g.add(btip);
  // Visor holosight
  const vs=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.038,0.05),darkM);vs.position.set(0,0.118,-0.1);g.add(vs);
  const vsLens=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.022,0.003),neon2);vsLens.position.set(0,0.118,-0.074);g.add(vsLens);
  // Frame
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.046,0.28),darkM);fr.position.set(0,0.002,0.02);g.add(fr);
  // Angular grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.2,0.12),darkM);gr.position.set(0,-0.105,0.1);gr.rotation.x=0.12;g.add(gr);
  // Neon grip lines
  for(let i=0;i<3;i++){const nl=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.005,0.12),i%2===0?neon1:neon2);nl.position.set(0,-0.04-i*0.04,0.1);g.add(nl);}
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.03,0.006,4,12,Math.PI),chromeM);tg.rotation.x=Math.PI/2;tg.position.set(0,-0.024,0.04);g.add(tg);
  return g;
}

// Desert Eagle "Prismatic Storm" — arcane crystal handcannon with prism barrel and rune engravings
function mkDeagle_Prismatic(c1,c2){
  const g=new THREE.Group();
  const crystalM=new THREE.MeshLambertMaterial({color:c1});
  const deepM=new THREE.MeshLambertMaterial({color:0x000815});
  const glowM=new THREE.MeshLambertMaterial({color:c2,emissive:new THREE.Color(c2).multiplyScalar(0.45)});
  const silverM=new THREE.MeshLambertMaterial({color:0xaabbcc});
  // Massive faceted slide (prism-like)
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.08,0.11,0.34),crystalM);slide.position.set(0,0.065,0);g.add(slide);
  // Angled facets (chamfered edges)
  const cf1=new THREE.Mesh(new THREE.BoxGeometry(0.082,0.04,0.34),glowM);cf1.position.set(0,0.11,0);cf1.rotation.z=0.3;g.add(cf1);
  const cf2=new THREE.Mesh(new THREE.BoxGeometry(0.082,0.04,0.34),glowM);cf2.position.set(0,0.11,0);cf2.rotation.z=-0.3;g.add(cf2);
  // Prism barrel (triangular cross-section via 3-sided cyl)
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.026,0.28,3),crystalM);brl.rotation.x=Math.PI/2;brl.position.set(0,0.042,-0.25);g.add(brl);
  // Crystal prism tip — actual prism shape
  const pTip=new THREE.Mesh(new THREE.CylinderGeometry(0,0.03,0.06,3),glowM);pTip.rotation.x=-Math.PI/2;pTip.position.set(0,0.042,-0.42);g.add(pTip);
  // Rune engravings (thin glowing lines on slide)
  [[0.06,-0.08],[0.055,0.0],[0.058,0.08]].forEach(([h,z],i)=>{
    const rune=new THREE.Mesh(new THREE.BoxGeometry(0.082,0.005,0.03+i*0.01),glowM);rune.position.set(0,h,z);g.add(rune);
  });
  // Frame
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.054,0.3),deepM);fr.position.set(0,0.006,0.01);g.add(fr);
  // Crystal grip panels
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.065,0.23,0.13),deepM);gr.position.set(0,-0.12,0.1);gr.rotation.x=0.1;g.add(gr);
  const grCrystal=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.22,0.12),crystalM);grCrystal.position.set(0.033,-0.12,0.1);g.add(grCrystal);
  const grCrystal2=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.22,0.12),crystalM);grCrystal2.position.set(-0.033,-0.12,0.1);g.add(grCrystal2);
  // Glowing rune inset on grip side
  const grRune=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.06,0.06),glowM);grRune.position.set(0.034,-0.1,0.1);g.add(grRune);
  // Trigger + guard (crystal)
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.034,0.007,4,12,Math.PI),crystalM);tg.rotation.x=Math.PI/2;tg.position.set(0,-0.026,0.04);g.add(tg);
  const tr=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.032,0.012),silverM);tr.position.set(0,-0.02,0.02);tr.rotation.x=0.28;g.add(tr);
  return g;
}

// MAC-10 "Dark Vortex" — void-themed SMG with spiral barrel, tentacle magazine and eye motif
function mkCompact_DarkVortex(c1,c2){
  const g=new THREE.Group();
  const voidM=new THREE.MeshLambertMaterial({color:0x080010});
  const purpM=new THREE.MeshLambertMaterial({color:c2,emissive:new THREE.Color(c2).multiplyScalar(0.35)});
  const rimM=new THREE.MeshLambertMaterial({color:0x6600cc});
  // Dark boxy receiver
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.1,0.3),voidM);rcv.position.set(0,0.01,0.01);g.add(rcv);
  // Purple trim plates
  const tp=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.012,0.3),purpM);tp.position.set(0,0.063,0.01);g.add(tp);
  const sp=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.1,0.3),rimM);sp.position.set(0.037,0.01,0.01);g.add(sp);
  const sp2=sp.clone();sp2.position.x=-0.037;g.add(sp2);
  // Void eye on side (circle + slit)
  const eyeO=new THREE.Mesh(new THREE.TorusGeometry(0.028,0.007,6,16),purpM);eyeO.rotation.y=Math.PI/2;eyeO.position.set(0.038,0.01,-0.04);g.add(eyeO);
  const eyeP=new THREE.Mesh(new THREE.PlaneGeometry(0.01,0.032),new THREE.MeshBasicMaterial({color:0x8800ff}));eyeP.rotation.y=Math.PI/2;eyeP.position.set(0.039,0.01,-0.04);g.add(eyeP);
  // Spiral barrel (multiple offset cylinders creating twist)
  for(let i=0;i<8;i++){
    const t=i/8, ang=t*Math.PI*2;
    const seg=new THREE.Mesh(new THREE.CylinderGeometry(0.012,0.012,0.04,5),i===0?purpM:rimM);
    seg.rotation.x=Math.PI/2;
    seg.position.set(Math.cos(ang)*0.006,0.018+Math.sin(ang)*0.006,-0.16-t*0.12);g.add(seg);
  }
  // Muzzle crown
  const mc=new THREE.Mesh(new THREE.TorusGeometry(0.022,0.007,5,12),purpM);mc.rotation.x=Math.PI/2;mc.position.set(0,0.018,-0.285);g.add(mc);
  // Tentacle magazine
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.036,0.2,0.056),voidM);mag.position.set(0,-0.13,0.08);g.add(mag);
  for(let i=0;i<3;i++){
    const t=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.003,0.055,5),purpM);
    t.position.set(-0.01+i*0.01,-0.235+i*0.005,0.08+Math.sin(i*2)*0.01);t.rotation.z=(-0.3+i*0.3);g.add(t);
  }
  // Grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.11,0.074),voidM);gr.position.set(0,-0.055,0.08);gr.rotation.x=0.08;g.add(gr);
  // Vortex lines on grip
  for(let i=0;i<4;i++){const v=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.004,0.074),purpM);v.position.set(0,-0.01-i*0.024,0.08);g.add(v);}
  // Stock
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.06,0.18),rimM);stk.position.set(0,-0.02,0.21);g.add(stk);
  return g;
}

// Knife "Crimson Phantom" — spectral curved blade with ghostly ribs and blood channel
function mkKnife_CrimsonPhantom(c1,c2){
  const g=new THREE.Group();
  const bloodM=new THREE.MeshLambertMaterial({color:0xFF0033});
  const ghostM=new THREE.MeshLambertMaterial({color:0x550011,emissive:new THREE.Color(0x330000).multiplyScalar(0.6)});
  const boneM=new THREE.MeshLambertMaterial({color:0xDDCCBB});
  const darkM=new THREE.MeshLambertMaterial({color:0x0a0000});
  // Wide curved phantom blade
  const bld=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.005,0.34),ghostM);bld.position.set(0,0.014,-0.08);g.add(bld);
  // Blood channel (fuller) — glowing red
  const full=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.003,0.28),bloodM);full.position.set(0,0.017,-0.07);g.add(full);
  // Phantom ribs (translucent fin-like protrusions from spine)
  for(let i=0;i<6;i++){
    const rib=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.032+i*0.003,0.01),ghostM);
    rib.position.set(0,0.025+i*0.003,-0.22+i*0.032);rib.rotation.z=i%2===0?0.25:-0.25;g.add(rib);
  }
  // Curved swept tip
  const swp=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.004,0.07),bloodM);swp.position.set(0,0.016,-0.25);swp.rotation.x=-0.3;g.add(swp);
  const tip=new THREE.Mesh(new THREE.ConeGeometry(0.009,0.05,4),bloodM);tip.rotation.x=Math.PI/2;tip.rotation.z=Math.PI/4;tip.position.set(0,0.016,-0.3);g.add(tip);
  // Bone guard with claw hooks
  const grd=new THREE.Mesh(new THREE.BoxGeometry(0.09,0.012,0.022),boneM);grd.position.set(0,0.01,0.07);g.add(grd);
  const cl1=new THREE.Mesh(new THREE.ConeGeometry(0.007,0.028,4),boneM);cl1.position.set(0.044,0.024,0.07);cl1.rotation.z=-0.6;g.add(cl1);
  const cl2=cl1.clone();cl2.position.x=-0.044;cl2.rotation.z=0.6;g.add(cl2);
  // Handle — wrapped in phantom cloth strips
  const hnd=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.022,0.19),darkM);hnd.position.set(0,0,0.17);g.add(hnd);
  for(let i=0;i<6;i++){
    const wr=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.007,0.015),i%3===0?bloodM:boneM);
    wr.position.set(0,0.015,0.076+i*0.028);g.add(wr);
  }
  // Skull pommel
  const sk=new THREE.Mesh(new THREE.SphereGeometry(0.02,7,6),boneM);sk.position.set(0,0.002,0.268);g.add(sk);
  // Skull eye sockets
  const se1=new THREE.Mesh(new THREE.SphereGeometry(0.007,5,4),new THREE.MeshBasicMaterial({color:0xFF0000}));se1.position.set(0.009,0.006,0.28);g.add(se1);
  const se2=se1.clone();se2.position.x=-0.009;g.add(se2);
  return g;
}

function mkSledge(c1,c2){
  const g=new THREE.Group();
  const headM=new THREE.MeshLambertMaterial({color:c1});
  const shaftM=new THREE.MeshLambertMaterial({color:c2});
  const bandM=new THREE.MeshLambertMaterial({color:0x333333});
  // Handle — long wooden shaft
  const shaft=new THREE.Mesh(new THREE.CylinderGeometry(0.022,0.026,0.68,8),shaftM);
  shaft.rotation.x=Math.PI/2; shaft.position.set(0,0,0.18); g.add(shaft);
  // Grip wraps
  for(let i=0;i<4;i++){
    const wr=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.028,0.022,8),bandM);
    wr.rotation.x=Math.PI/2; wr.position.set(0,0,0.36+i*0.04); g.add(wr);
  }
  // Head — massive square maul head
  const head=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.16,0.22),headM);
  head.position.set(0,0,-0.13); g.add(head);
  // Head bevel top
  const bev=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.014,0.22),new THREE.MeshLambertMaterial({color:0x888888}));
  bev.position.set(0,0.087,-0.13); g.add(bev);
  // Strike face detail
  const face=new THREE.Mesh(new THREE.BoxGeometry(0.165,0.145,0.012),new THREE.MeshLambertMaterial({color:0x444444}));
  face.position.set(0,0,-0.241); g.add(face);
  // Head-shaft weld ring (flush at collar)
  const weld=new THREE.Mesh(new THREE.CylinderGeometry(0.038,0.038,0.008,8),new THREE.MeshLambertMaterial({color:0x111111}));
  weld.rotation.x=Math.PI/2; weld.position.set(0,0,0.04); g.add(weld);
  // Collar where head meets shaft
  const collar=new THREE.Mesh(new THREE.CylinderGeometry(0.035,0.035,0.04,8),bandM);
  collar.rotation.x=Math.PI/2; collar.position.set(0,0,0.07); g.add(collar);
  // Pommel end cap
  const pom=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.022,0.025,8),bandM);
  pom.rotation.x=Math.PI/2; pom.position.set(0,0,0.53); g.add(pom);
  return g;
}

function mkSledge_Void(c1,c2){
  const g=new THREE.Group();
  const voidM=new THREE.MeshLambertMaterial({color:0x080010});
  const purpM=new THREE.MeshLambertMaterial({color:c2,emissive:new THREE.Color(c2).multiplyScalar(0.4)});
  const crystalM=new THREE.MeshLambertMaterial({color:c1,emissive:new THREE.Color(c1).multiplyScalar(0.3)});
  // Floating void-crystal head (faceted)
  const head=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.17,0.23),voidM);
  head.position.set(0,0,-0.13); g.add(head);
  // Crystal face plates
  const fp=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.17,0.014),crystalM);
  fp.position.set(0,0,-0.244); g.add(fp);
  const fp2=fp.clone(); fp2.position.z=-0.016+0.23; g.add(fp2); // back face of head
  // Glowing rune lines on head
  [[0.07,0],[0,-0.06],[0.06,0.06],[-0.06,0.04]].forEach(([x,y])=>{
    const r=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.004,0.235),purpM);
    r.position.set(x,y,-0.13); g.add(r);
  });
  // Void tendrils around head
  for(let i=0;i<6;i++){
    const ang=i/6*Math.PI*2;
    const t=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.003,0.06,5),purpM);
    t.position.set(Math.cos(ang)*0.11,Math.sin(ang)*0.1,-0.13);
    t.rotation.x=Math.PI/2+Math.cos(ang)*0.4; t.rotation.y=ang; g.add(t);
  }
  // Dark prismatic shaft
  const shaft=new THREE.Mesh(new THREE.CylinderGeometry(0.02,0.025,0.66,6),voidM);
  shaft.rotation.x=Math.PI/2; shaft.position.set(0,0,0.18); g.add(shaft);
  // Glowing shaft bands
  for(let i=0;i<6;i++){
    const band=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.026,0.018,6),purpM);
    band.rotation.x=Math.PI/2; band.position.set(0,0,0.05+i*0.075); g.add(band);
  }
  // Floating void orb at base
  const orb=new THREE.Mesh(new THREE.SphereGeometry(0.034,8,7),crystalM);
  orb.position.set(0,0,0.52); g.add(orb);
  const orbRing=new THREE.Mesh(new THREE.TorusGeometry(0.044,0.007,5,14),purpM);
  orbRing.rotation.x=Math.PI/2; orbRing.position.set(0,0,0.52); g.add(orbRing);
  return g;
}
function mkSniper(c1,c2){
  const g=new THREE.Group();
  const rcvM=mDark(c1), stkM=mWood(c2), metM=mMetal(0x999999), dM=mph(0x0a0a0a,20), rlM=mMetal(0x222222), rubM=mph(0x111111,8);
  // ── Long heavy barrel ──
  const brlPts=[[0,0],[0.024,0],[0.024,0.02],[0.02,0.04],[0.02,0.52],[0.022,0.54],[0.022,0.56]];
  const brl=mkBarrel(brlPts,12,mDark(c1),0,0.016,-0.62); g.add(brl);
  // Muzzle brake (3-port style)
  const mb=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.024,0.05,8),metM); mb.rotation.x=Math.PI/2; mb.position.set(0,0.016,-0.84); g.add(mb);
  for(let i=0;i<3;i++){
    const port=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.01,0.018),dM); port.position.set(0,0.026+i*0.01,-0.828); g.add(port);
  }
  // ── Receiver ──
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.075,0.32),rcvM); g.add(rcv);
  edge(g,0.004,0.004,0.32,DARK_EDGE, 0.031,0.038,0);
  edge(g,0.004,0.004,0.32,DARK_EDGE,-0.031,0.038,0);
  // Ejection port (large, right side)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.034,0.085),dM); ep.position.set(0.032,0.004,-0.018); g.add(ep);
  // Bolt handle (right side — the defining feature of a bolt-action)
  const boltBase=new THREE.Mesh(new THREE.CylinderGeometry(0.014,0.014,0.04,8),metM); boltBase.rotation.z=Math.PI/2; boltBase.position.set(0.05,0.015,0.04); g.add(boltBase);
  const boltRod=new THREE.Mesh(new THREE.CylinderGeometry(0.008,0.008,0.055,6),metM); boltRod.rotation.z=Math.PI/2; boltRod.position.set(0.075,0.015,0.04); g.add(boltRod);
  const boltKnob=new THREE.Mesh(new THREE.SphereGeometry(0.018,10,8),metM); boltKnob.position.set(0.1,0.015,0.04); g.add(boltKnob);
  boltBase.userData.boltHandle=true; boltRod.userData.boltHandle=true; boltKnob.userData.boltHandle=true;
  // ── Scope mount (two rings on top rail) ──
  const topRail=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.008,0.28),rlM); topRail.position.set(0,0.042,0.01); g.add(topRail);
  for(let i=0;i<6;i++){const t=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.004,0.004),dM);t.position.set(0,0.045,-0.1+i*0.04);g.add(t);}
  [-0.08,0.1].forEach(rz=>{
    const ring=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.034,0.026),metM); ring.position.set(0,0.058,rz); g.add(ring);
    const clamp=new THREE.Mesh(new THREE.BoxGeometry(0.032,0.01,0.022),metM); clamp.position.set(0,0.076,rz); g.add(clamp);
    const bolt1=new THREE.Mesh(new THREE.CylinderGeometry(0.004,0.004,0.014,6),metM); bolt1.position.set(0.014,0.07,rz); g.add(bolt1);
    const bolt2=bolt1.clone(); bolt2.position.set(-0.014,0.07,rz); g.add(bolt2);
  });
  // ── Scope body ──
  const scopeBody=new THREE.Mesh(new THREE.CylinderGeometry(0.022,0.022,0.32,12),mph(0x1a1a1a,60)); scopeBody.rotation.x=Math.PI/2; scopeBody.position.set(0,0.08,0.01); g.add(scopeBody);
  // Objective bell (front of scope, wider)
  const objBell=new THREE.Mesh(new THREE.CylinderGeometry(0.03,0.022,0.06,12),mph(0x1a1a1a,60)); objBell.rotation.x=Math.PI/2; objBell.position.set(0,0.08,-0.19); g.add(objBell);
  // Ocular bell (rear, where you look)
  const ocuBell=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.022,0.045,12),mph(0x1a1a1a,60)); ocuBell.rotation.x=Math.PI/2; ocuBell.position.set(0,0.08,0.195); g.add(ocuBell);
  // Elevation/windage turrets (two adjustment knobs on top and side)
  const elevTurret=new THREE.Mesh(new THREE.CylinderGeometry(0.013,0.013,0.028,8),metM); elevTurret.position.set(0,0.114,0.04); g.add(elevTurret);
  const elevCap=new THREE.Mesh(new THREE.CylinderGeometry(0.015,0.015,0.008,8),metM); elevCap.position.set(0,0.128,0.04); g.add(elevCap);
  const windTurret=new THREE.Mesh(new THREE.CylinderGeometry(0.013,0.013,0.028,8),metM); windTurret.rotation.z=Math.PI/2; windTurret.position.set(0.048,0.08,0.04); g.add(windTurret);
  const windCap=new THREE.Mesh(new THREE.CylinderGeometry(0.015,0.015,0.008,8),metM); windCap.rotation.z=Math.PI/2; windCap.position.set(0.062,0.08,0.04); g.add(windCap);
  // Scope lens (glassy front)
  const lens=new THREE.Mesh(new THREE.CylinderGeometry(0.021,0.021,0.008,12),mph(0x003322,60,0.2)); lens.rotation.x=Math.PI/2; lens.position.set(0,0.08,-0.225); g.add(lens);
  // ── Trigger group ──
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.024,0.006,5,14,Math.PI),rcvM); tg.rotation.x=Math.PI/2; tg.position.set(0,-0.042,0.08); g.add(tg);
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.026,0.011),metM); trig.position.set(0,-0.03,0.062); trig.rotation.x=0.3; g.add(trig);
  const tgPlate=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.008,0.14),rcvM); tgPlate.position.set(0,-0.046,0.07); g.add(tgPlate);
  // ── Pistol grip ──
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.115,0.075),mph(c2,14,0)); gr.position.set(0,-0.095,0.1); gr.rotation.x=0.16; g.add(gr);
  [-0.025,0.025].forEach(sx=>{
    for(let i=0;i<6;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.008,0.014),dM);r.position.set(sx,-0.038-i*0.017,0.1);r.rotation.x=0.16;g.add(r);}
  });
  const gc=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.012,0.08),rcvM); gc.position.set(0,-0.153,0.099); gc.rotation.x=0.16; g.add(gc);
  // ── Thumbhole stock ──
  const stkMain=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.095,0.42),stkM); stkMain.position.set(0,-0.002,0.33); g.add(stkMain);
  // Cheekpiece
  const cheek=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.044,0.22),stkM); cheek.position.set(0,0.062,0.3); g.add(cheek);
  // Thumbhole cutout (dark box to simulate)
  const thole=new THREE.Mesh(new THREE.BoxGeometry(0.052,0.04,0.1),dM); thole.position.set(0,-0.028,0.16); g.add(thole);
  // Butt with rubber pad
  const butt=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.108,0.018),rubM); butt.position.set(0,-0.0,0.545); g.add(butt);
  // Adjustable LOP spacers
  for(let i=0;i<2;i++){const sp=new THREE.Mesh(new THREE.BoxGeometry(0.052,0.11,0.01),rcvM);sp.position.set(0,0,0.53+i*0.01);g.add(sp);}
  // ── Bipod legs (folded) ──
  [-0.022,0.022].forEach(bx=>{
    const leg=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.12,0.01),metM); leg.position.set(bx,-0.04,-0.3); leg.rotation.x=0.3; g.add(leg);
    const foot=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.015,0.025),metM); foot.position.set(bx,-0.1,-0.27); g.add(foot);
  });
  const bipodBase=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.012,0.022),metM); bipodBase.position.set(0,0.016,-0.3); g.add(bipodBase);
  // ── Internal magazine (box, sits under receiver) ──
  const magBox=new THREE.Mesh(new THREE.BoxGeometry(0.04,0.065,0.09),rcvM); magBox.position.set(0,-0.068,-0.04); g.add(magBox);
  const magFloor=new THREE.Mesh(new THREE.BoxGeometry(0.042,0.01,0.094),metM); magFloor.position.set(0,-0.1,-0.04); g.add(magFloor);
  // ── Front sling swivel ──
  const fs=new THREE.Mesh(new THREE.TorusGeometry(0.014,0.004,5,10),metM); fs.rotation.y=Math.PI/2; fs.position.set(0,-0.04,-0.36); g.add(fs);
  return g;
}

function getMesh(wid,skinId){
  const def=WDEFS[wid],skin=def.skins.find(s=>s.id===skinId)||def.skins[0];
  const [c1,c2]=skin.colors;
  // Route legendary skins to unique models
  if(wid==='smg'&&skinId==='atomic')    return mkSMG_VoidReactor(c1,c2);
  if(wid==='shotgun'&&skinId==='fade')  return mkShotgun_Inferno(c1,c2);
  if(wid==='pistol'&&skinId==='fade')   return mkPistol_Aurora(c1,c2);
  if(wid==='pistol'&&skinId==='neon')   return mkPistol_NeonRider(c1,c2);
  if(wid==='deagle'&&skinId==='oxide')  return mkDeagle_Prismatic(c1,c2);
  if(wid==='compact'&&skinId==='hotrod')return mkCompact_DarkVortex(c1,c2);
  if(wid==='knife'&&skinId==='shadow')  return mkKnife_CrimsonPhantom(c1,c2);
  if(wid==='sledge'&&skinId==='void')   return mkSledge_Void(c1,c2);
  if(wid==='pistol')  return mkPistol(c1,c2);
  if(wid==='knife')   return mkKnife(skin.model||'std',c1,c2);
  if(wid==='smg')     return mkSMG(c1,c2);
  if(wid==='shotgun') return mkShotgun(c1,c2);
  if(wid==='sniper')  return mkSniper(c1,c2);
  if(wid==='revolver')return mkRevolver(c1,c2);
  if(wid==='deagle')  return mkDeagle(c1,c2);
  if(wid==='compact') return mkCompact(c1,c2);
  if(wid==='sledge')  return mkSledge(c1,c2);
}
function rebuildWeapon(){
  while(weaponGroup.children.length)weaponGroup.remove(weaponGroup.children[0]);
  const m=getMesh(S.held,S.weapons[S.held].skin);
  if(m)weaponGroup.add(m);
  weaponGroup.position.set(0.28,-0.32,-0.48);
  weaponGroup.rotation.set(0,0.1,0);
}

// ══ WAVE SYSTEM ══
function waveSize(w){
  if(w<=WAVES.length)return WAVES[w-1];
  return WAVES[WAVES.length-1]*Math.pow(2,w-WAVES.length);
}
function startWave(){
  S.wave++;S.waveSize=waveSize(S.wave);S.waveKilled=0;S.spawnQueue=S.waveSize;S.spawnTimer=0;S.waveActive=true;
  document.getElementById('waveNum').textContent='WAVE '+S.wave;
  const mix=zombieTypeLabel(S.wave);
  let typeTxt=S.waveSize+' ZOMBIES';
  if(mix.runner>0||mix.brute>0){
    const parts=[];
    if(mix.normal>0) parts.push(Math.round(mix.normal*S.waveSize)+' NORMAL');
    if(mix.runner>0) parts.push(Math.round(mix.runner*S.waveSize)+' RUNNERS');
    if(mix.brute>0)  parts.push(Math.round(mix.brute*S.waveSize)+' BRUTES');
    typeTxt=parts.join(' · ');
  }
  document.getElementById('waveCount').textContent=typeTxt;
  const wa=document.getElementById('waveAnnounce');
  wa.style.opacity='1';setTimeout(()=>wa.style.opacity='0',2800);
  updateHUD();
}
function tickWave(dt){
  if(!S.waveActive)return;
  if(S.spawnQueue>0){
    S.spawnTimer+=dt;
    const interval=Math.max(250,900-S.wave*40);
    if(S.spawnTimer>=interval){S.spawnTimer=0;spawnZombie();S.spawnQueue--;}
  }
  if(S.spawnQueue===0&&S.zombies.filter(z=>!z.userData.dead).length===0){
    S.waveActive=false;
    setTimeout(()=>{if(S.screen==='game')startWave();},3500);
  }
}

// ══ ZOMBIES ══
const SPAWNS=[
  [0,-36],[0,36],[-36,0],[36,0],
  [-25,-25],[25,-25],[-25,25],[25,25],
  [-36,-18],[36,-18],[-36,18],[36,18],
  [-18,-36],[18,-36],[-18,36],[18,36],
];
// ── ZOMBIE TYPE LABEL in HUD ──
function zombieTypeLabel(w){
  const brute=w>=5?0.2:0;
  const runner=w>=3?0.3:0;
  return {brute,runner,normal:1-brute-runner};
}
function pickType(){
  const r=Math.random(),mix=zombieTypeLabel(S.wave);
  if(r<mix.brute) return 'brute';
  if(r<mix.brute+mix.runner) return 'runner';
  return 'normal';
}

function spawnZombie(){
  const type=pickType();
  const g=new THREE.Group();
  g.userData={dead:false,type,atkTimer:0,deathT:0};

  if(type==='brute'){
    // ── BRUTE: massive mutant, orange-brown rot, chains, exposed bone ──
    g.userData.maxHp=200; g.userData.hp=200;
    g.userData.speed=Math.min(0.7+S.wave*0.04+Math.random()*0.2,1.8);
    g.userData.dmg=18;
    const skinM =new THREE.MeshLambertMaterial({color:0xb84400});
    const darkM =new THREE.MeshLambertMaterial({color:0x7a2a00});
    const boneM =new THREE.MeshLambertMaterial({color:0xddccaa});
    const rottM =new THREE.MeshLambertMaterial({color:0x3a1800});
    const eyeM  =new THREE.MeshBasicMaterial({color:0xff6600});
    const bloodM=new THREE.MeshLambertMaterial({color:0x6a0000});
    const clothM=new THREE.MeshLambertMaterial({color:0x1a1208});
    const chainM=new THREE.MeshLambertMaterial({color:0x888888});
    // Massive torso with gut swell
    const torso=new THREE.Mesh(new THREE.BoxGeometry(0.82,0.82,0.52),skinM);torso.position.y=1.08;torso.castShadow=true;g.add(torso);
    const gut  =new THREE.Mesh(new THREE.BoxGeometry(0.72,0.38,0.58),darkM);gut.position.y=0.82;gut.castShadow=true;g.add(gut);
    // Torn shirt remnants (two strips on shoulders)
    [-0.22,0.22].forEach(sx=>{
      const rag=new THREE.Mesh(new THREE.BoxGeometry(0.26,0.12,0.54),clothM);rag.position.set(sx,1.34,0);g.add(rag);
    });
    // Exposed ribcage (partial)
    for(let i=0;i<4;i++){
      const rib=new THREE.Mesh(new THREE.BoxGeometry(0.7,0.04,0.012),boneM);rib.position.set(0,1.18-i*0.1,0.25);g.add(rib);
    }
    // Wound gashes on torso
    [[-0.15,1.15,0.27],[0.2,0.95,0.27],[0.0,1.3,0.27]].forEach(([x,y,z])=>{
      const w=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.05,0.01),bloodM);w.position.set(x,y,z);g.add(w);
    });
    // Thick neck
    const neck=new THREE.Mesh(new THREE.CylinderGeometry(0.18,0.2,0.22,8),skinM);neck.position.y=1.55;g.add(neck);
    // Massive misshapen head
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.58,0.52,0.50),darkM);head.position.y=1.88;head.castShadow=true;g.add(head);
    // Brow ridge (protruding bone)
    const brow=new THREE.Mesh(new THREE.BoxGeometry(0.56,0.1,0.12),rottM);brow.position.set(0,2.07,0.22);g.add(brow);
    // Cheekbones
    [-0.22,0.22].forEach(sx=>{
      const cb=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.07,0.1),boneM);cb.position.set(sx,1.88,0.22);g.add(cb);
    });
    // Jaw (drooping, massive)
    const jaw=new THREE.Mesh(new THREE.BoxGeometry(0.44,0.16,0.42),rottM);jaw.position.set(0,1.7,0.04);jaw.rotation.x=0.2;g.add(jaw);
    // Exposed teeth (top and bottom row)
    for(let i=0;i<6;i++){
      const t=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.07,0.04),boneM);t.position.set(-0.13+i*0.05,1.77,0.22);g.add(t);
    }
    // Eyes (wide, glowing orange, one larger than the other = mutation)
    [[-0.14,1.95,0.25,0.12,0.09],[0.16,1.97,0.25,0.09,0.09]].forEach(([x,y,z,w,h])=>{
      const eye=new THREE.Mesh(new THREE.BoxGeometry(w,h,0.01),eyeM);eye.position.set(x,y,z);g.add(eye);
      const pupil=new THREE.Mesh(new THREE.BoxGeometry(w*0.5,h*0.5,0.012),new THREE.MeshBasicMaterial({color:0x220000}));pupil.position.set(x,y,z);g.add(pupil);
    });
    // Ear/lobe remnants
    const earL=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.08,0.06),darkM);earL.position.set(-0.31,1.9,0.04);g.add(earL);
    // Arms - thick, one normal one mutated bigger
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.22,0.78,0.26),skinM);lA.position.set(-0.55,1.08,0.02);lA.rotation.x=-0.35;lA.userData.l='la';g.add(lA);
    // Exposed forearm bone on left arm
    const lBone=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.3,0.04),boneM);lBone.position.set(-0.55,0.72,0.06);g.add(lBone);
    // Mutated right arm - larger
    const rA=new THREE.Mesh(new THREE.BoxGeometry(0.30,0.92,0.32),skinM);rA.position.set(0.58,1.05,0.02);rA.rotation.x=-0.25;rA.userData.l='ra';g.add(rA);
    // Massive right fist
    const rFist=new THREE.Mesh(new THREE.BoxGeometry(0.28,0.24,0.26),rottM);rFist.position.set(0.58,0.5,-0.12);g.add(rFist);
    // Finger claws on right fist
    for(let i=0;i<4;i++){
      const claw=new THREE.Mesh(new THREE.ConeGeometry(0.025,0.09,4),boneM);claw.position.set(0.44+i*0.06,0.38,-0.22);claw.rotation.x=0.5;g.add(claw);
    }
    // Legs - massive, torn pants
    const lL=new THREE.Mesh(new THREE.BoxGeometry(0.32,0.82,0.32),clothM);lL.position.set(-0.2,0.38,0);lL.userData.l='ll';g.add(lL);
    const rL=lL.clone();rL.position.x=0.2;rL.userData.l='rl';g.add(rL);
    // Bare feet (no shoes)
    [-0.2,0.2].forEach(fx=>{
      const foot=new THREE.Mesh(new THREE.BoxGeometry(0.2,0.1,0.32),skinM);foot.position.set(fx,-0.04,0.06);g.add(foot);
    });
    // Chain around torso
    for(let i=0;i<8;i++){
      const link=new THREE.Mesh(new THREE.TorusGeometry(0.04,0.012,4,8),chainM);
      link.rotation.y=i*Math.PI/4;link.position.set(Math.cos(i*Math.PI/4)*0.45,1.18,Math.sin(i*Math.PI/4)*0.3);g.add(link);
    }
    // Hitboxes
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.86,1.65,0.60),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.82;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.60,0.55,0.52),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.88;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    // HP bar
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.9,0.09),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=2.65;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.9,0.09),new THREE.MeshBasicMaterial({color:0xff6600}));hpFg.position.set(0,2.651,0.001);hpFg.userData.bar=true;g.add(hpFg);

  } else if(type==='runner'){
    // ── RUNNER: emaciated, hunched, tattered, cyan eyes, skeletal ──
    g.userData.maxHp=50; g.userData.hp=50;
    g.userData.speed=Math.min(3.4+S.wave*0.1+Math.random()*0.6,7.0);
    g.userData.dmg=5;
    const skinM =new THREE.MeshLambertMaterial({color:0x2244aa});
    const darkM =new THREE.MeshLambertMaterial({color:0x112266});
    const boneM =new THREE.MeshLambertMaterial({color:0xaabbcc});
    const rottM =new THREE.MeshLambertMaterial({color:0x0a1a44});
    const eyeM  =new THREE.MeshBasicMaterial({color:0x00ffff});
    const bloodM=new THREE.MeshLambertMaterial({color:0x003366});
    const clothM=new THREE.MeshLambertMaterial({color:0x080814});
    // Slim torso, hunched forward
    const torso=new THREE.Mesh(new THREE.BoxGeometry(0.34,0.50,0.22),skinM);torso.position.set(0,0.82,0);torso.rotation.x=-0.22;torso.castShadow=true;g.add(torso);
    // Visible spine bumps
    for(let i=0;i<5;i++){
      const s=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.05,0.04),boneM);s.position.set(0,0.95-i*0.08,-0.11);s.rotation.x=-0.22;g.add(s);
    }
    // Torn sleeveless shirt
    const shirt=new THREE.Mesh(new THREE.BoxGeometry(0.36,0.32,0.24),clothM);shirt.position.set(0,0.9,0);shirt.rotation.x=-0.22;g.add(shirt);
    // Collar bone exposed
    const colL=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.022,0.028),boneM);colL.position.set(-0.12,1.06,0.1);g.add(colL);
    const colR=colL.clone();colR.position.x=0.12;g.add(colR);
    // Thin neck, stretched
    const neck=new THREE.Mesh(new THREE.CylinderGeometry(0.06,0.075,0.2,6),skinM);neck.position.set(0,1.16,0);neck.rotation.x=-0.1;g.add(neck);
    // Head with hollow cheeks
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.28,0.28,0.26),rottM);head.position.set(0,1.34,-0.04);head.castShadow=true;g.add(head);
    // Cheek hollows (recessed)
    [-0.1,0.1].forEach(cx=>{
      const ch=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.07,0.04),darkM);ch.position.set(cx,1.33,0.1);g.add(ch);
    });
    // Brow jutting forward (hunched look)
    const brow=new THREE.Mesh(new THREE.BoxGeometry(0.28,0.06,0.08),rottM);brow.position.set(0,1.46,0.1);g.add(brow);
    // Glowing cyan eyes (slightly too large, unsettling)
    [[-0.075,1.36,0.14],[0.075,1.36,0.14]].forEach(([x,y,z])=>{
      const eye=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.055,0.01),eyeM);eye.position.set(x,y,z);g.add(eye);
      // Eye glow halo
      const halo=new THREE.Mesh(new THREE.PlaneGeometry(0.1,0.08),new THREE.MeshBasicMaterial({color:0x00ffff,transparent:true,opacity:0.25}));halo.position.set(x,y,z+0.01);g.add(halo);
    });
    // Nose/mouth area - sunken
    const maw=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.045,0.03),darkM);maw.position.set(0,1.25,0.13);g.add(maw);
    // Teeth showing (grimacing)
    for(let i=0;i<5;i++){
      const t=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.038,0.018),boneM);t.position.set(-0.056+i*0.028,1.27,0.13);g.add(t);
    }
    // Long thin arms outstretched (reaching)
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.09,0.44,0.12),skinM);lA.position.set(-0.26,0.82,0.04);lA.rotation.set(-0.7,0,-0.15);lA.userData.l='la';g.add(lA);
    const lFore=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.36,0.09),boneM);lFore.position.set(-0.32,0.56,-0.1);lFore.rotation.set(-0.9,0,-0.1);g.add(lFore);
    // Claw hand left
    for(let i=0;i<3;i++){
      const f=new THREE.Mesh(new THREE.ConeGeometry(0.016,0.065,4),boneM);f.position.set(-0.28+i*0.02,0.32,-0.22);f.rotation.x=0.9;g.add(f);
    }
    const rA=new THREE.Mesh(new THREE.BoxGeometry(0.09,0.44,0.12),skinM);rA.position.set(0.26,0.82,0.04);rA.rotation.set(-0.7,0,0.15);rA.userData.l='ra';g.add(rA);
    const rFore=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.36,0.09),boneM);rFore.position.set(0.32,0.56,-0.1);rFore.rotation.set(-0.9,0,0.1);g.add(rFore);
    for(let i=0;i<3;i++){
      const f=new THREE.Mesh(new THREE.ConeGeometry(0.016,0.065,4),boneM);f.position.set(0.24+i*0.02,0.32,-0.22);f.rotation.x=0.9;g.add(f);
    }
    // Legs - sprint crouch position look via pants
    const lL=new THREE.Mesh(new THREE.BoxGeometry(0.13,0.46,0.15),clothM);lL.position.set(-0.09,0.26,0);lL.userData.l='ll';g.add(lL);
    const rL=lL.clone();rL.position.x=0.09;rL.userData.l='rl';g.add(rL);
    // Bare bone lower leg
    [-0.09,0.09].forEach(lx=>{
      const shin=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.2,0.07),boneM);shin.position.set(lx,0.05,0.01);g.add(shin);
    });
    // Torn rags hanging off legs
    for(let i=0;i<3;i++){
      const r=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.15,0.01),clothM);r.position.set(-0.06+i*0.06,0.18,0.08);r.rotation.x=0.15*(i-1);g.add(r);
    }
    // Hitboxes
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.4,1.1,0.32),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.55;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.3,0.3,0.28),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.34;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.45,0.06),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=1.75;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.45,0.06),new THREE.MeshBasicMaterial({color:0x00ccff}));hpFg.position.set(0,1.751,0.001);hpFg.userData.bar=true;g.add(hpFg);

  } else {
    // ── NORMAL: ragged undead civilian, green-grey skin, torn clothes ──
    g.userData.maxHp=100; g.userData.hp=100;
    g.userData.speed=Math.min(1.3+S.wave*0.1+Math.random()*0.5,4.0);
    g.userData.dmg=9;
    // Random variation per zombie
    const skinTone=Math.random()>0.5?0x2a5520:0x384830;
    const skinM =new THREE.MeshLambertMaterial({color:skinTone});
    const darkM =new THREE.MeshLambertMaterial({color:0x1a3015});
    const boneM =new THREE.MeshLambertMaterial({color:0xccbc98});
    const eyeM  =new THREE.MeshBasicMaterial({color:0xff1100});
    const bloodM=new THREE.MeshLambertMaterial({color:0x550000});
    // Randomised clothing colour (survivor clothing)
    const clothColors=[0x2a1a08,0x0a1a2a,0x1a0a0a,0x0a1408];
    const clothM=new THREE.MeshLambertMaterial({color:clothColors[Math.floor(Math.random()*clothColors.length)]});
    const clothM2=new THREE.MeshLambertMaterial({color:0x111111});
    // Torso
    const torso=new THREE.Mesh(new THREE.BoxGeometry(0.5,0.65,0.3),clothM);torso.position.y=0.97;torso.castShadow=true;g.add(torso);
    // Shirt collar/neckline detail
    const collar=new THREE.Mesh(new THREE.BoxGeometry(0.22,0.06,0.32),new THREE.MeshLambertMaterial({color:0x222222}));collar.position.set(0,1.28,0);g.add(collar);
    // Torn hole in shirt
    const tear=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.1,0.01),new THREE.MeshLambertMaterial({color:0x111111}));tear.position.set(-0.06,0.88,0.155);g.add(tear);
    // Blood stain on shirt
    const stain=new THREE.Mesh(new THREE.BoxGeometry(0.2,0.18,0.01),bloodM);stain.position.set(0.06,1.04,0.156);g.add(stain);
    // Exposed flesh through tear
    const flesh=new THREE.Mesh(new THREE.BoxGeometry(0.13,0.09,0.01),skinM);flesh.position.set(-0.06,0.88,0.157);g.add(flesh);
    // Neck
    const neck=new THREE.Mesh(new THREE.CylinderGeometry(0.07,0.085,0.18,7),skinM);neck.position.set(0,1.36,0);g.add(neck);
    // Neck wound
    const neckWound=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.04,0.005),bloodM);neckWound.position.set(0.05,1.38,0.085);g.add(neckWound);
    // Head
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.34,0.34,0.3),skinM);head.position.y=1.58;head.castShadow=true;g.add(head);
    // Scalp hair (flat patch)
    const hair=new THREE.Mesh(new THREE.BoxGeometry(0.34,0.04,0.3),new THREE.MeshLambertMaterial({color:0x0a0a0a}));hair.position.set(0,1.77,0);g.add(hair);
    // Hair tufts (partial, falling out)
    [[-0.08,0.02],[0.1,-0.02],[-0.14,-0.01]].forEach(([hx,hz])=>{
      const tuft=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.04,0.04),new THREE.MeshLambertMaterial({color:0x080808}));tuft.position.set(hx,1.8,hz);g.add(tuft);
    });
    // Brow ridge
    const brow=new THREE.Mesh(new THREE.BoxGeometry(0.32,0.045,0.05),darkM);brow.position.set(0,1.7,0.15);g.add(brow);
    // Cheeks (slightly sunken)
    [-0.1,0.1].forEach(cx=>{
      const ch=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.08,0.04),darkM);ch.position.set(cx,1.56,0.14);g.add(ch);
    });
    // Eyes (glowing red)
    [[-0.09,1.63,0.155],[0.09,1.63,0.155]].forEach(([x,y,z])=>{
      const eye=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.048,0.01),eyeM);eye.position.set(x,y,z);g.add(eye);
      const pupil=new THREE.Mesh(new THREE.SphereGeometry(0.018,6,4),new THREE.MeshBasicMaterial({color:0x000000}));pupil.position.set(x,y,z+0.005);g.add(pupil);
    });
    // Nose (stub)
    const nose=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.045,0.04),darkM);nose.position.set(0,1.56,0.165);g.add(nose);
    // Mouth (open, showing teeth)
    const mouth=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.042,0.02),new THREE.MeshBasicMaterial({color:0x0a0000}));mouth.position.set(0,1.495,0.155);g.add(mouth);
    for(let i=0;i<5;i++){
      const t=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.028,0.014),boneM);t.position.set(-0.044+i*0.022,1.506,0.155);g.add(t);
    }
    // Ear stubs
    [-0.175,0.175].forEach(ex=>{
      const ear=new THREE.Mesh(new THREE.BoxGeometry(0.025,0.055,0.05),skinM);ear.position.set(ex,1.59,0.01);g.add(ear);
    });
    // Arms with torn sleeves
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.55,0.18),clothM);lA.position.set(-0.35,0.97,0.02);lA.rotation.x=-0.5;lA.userData.l='la';g.add(lA);
    // Torn sleeve end
    const lSleeve=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.04,0.18),new THREE.MeshLambertMaterial({color:0x1a1a1a}));lSleeve.position.set(-0.35,0.68,0.1);lSleeve.rotation.x=-0.5;g.add(lSleeve);
    // Bare forearm
    const lFore=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.24,0.13),skinM);lFore.position.set(-0.35,0.52,0.17);lFore.rotation.x=-0.5;g.add(lFore);
    // Forearm vein
    const lVein=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.22,0.005),new THREE.MeshLambertMaterial({color:0x004400}));lVein.position.set(-0.36,0.52,0.22);lVein.rotation.x=-0.5;g.add(lVein);
    const rA=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.55,0.18),clothM);rA.position.set(0.35,0.97,0.02);rA.rotation.x=-0.5;rA.userData.l='ra';g.add(rA);
    const rSleeve=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.04,0.18),new THREE.MeshLambertMaterial({color:0x1a1a1a}));rSleeve.position.set(0.35,0.68,0.1);rSleeve.rotation.x=-0.5;g.add(rSleeve);
    const rFore=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.24,0.13),skinM);rFore.position.set(0.35,0.52,0.17);rFore.rotation.x=-0.5;g.add(rFore);
    // Hand claws
    [-0.35,0.35].forEach((hx,hi)=>{
      for(let f=0;f<4;f++){
        const fi=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.07,0.018),skinM);fi.position.set(hx-0.033+f*0.022,0.36,0.3+(hi===0?0.03:0.03));fi.rotation.x=0.7;g.add(fi);
        const fc=new THREE.Mesh(new THREE.ConeGeometry(0.01,0.03,4),boneM);fc.position.set(hx-0.033+f*0.022,0.295,0.32+(hi===0?0.04:0.04));fc.rotation.x=0.7;g.add(fc);
      }
    });
    // Pants (jeans / trousers)
    const pantsL=new THREE.Mesh(new THREE.BoxGeometry(0.2,0.58,0.22),clothM2);pantsL.position.set(-0.12,0.33,0);pantsL.userData.l='ll';g.add(pantsL);
    const pantsR=pantsL.clone();pantsR.position.x=0.12;pantsR.userData.l='rl';g.add(pantsR);
    // Belt
    const belt=new THREE.Mesh(new THREE.BoxGeometry(0.5,0.045,0.32),new THREE.MeshLambertMaterial({color:0x3a2800}));belt.position.y=0.64;g.add(belt);
    const buckle=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.055,0.01),boneM);buckle.position.set(0,0.64,0.162);g.add(buckle);
    // Torn trouser leg
    const ragL=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.08,0.01),clothM2);ragL.position.set(-0.12,0.07,0.11);ragL.rotation.x=0.2;g.add(ragL);
    // Shoes (one on, one off - classic zombie look)
    const shoeR=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.1,0.28),new THREE.MeshLambertMaterial({color:0x2a1a08}));shoeR.position.set(0.12,-0.02,0.04);g.add(shoeR);
    // Bare foot left (stumpy)
    const footL=new THREE.Mesh(new THREE.BoxGeometry(0.16,0.08,0.22),skinM);footL.position.set(-0.12,-0.02,0.03);g.add(footL);
    const toeL=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.05,0.06),skinM);toeL.position.set(-0.12,-0.04,0.14);g.add(toeL);
    // Hitboxes
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.56,1.35,0.58),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.67;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.38,0.38,0.34),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.58;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.62,0.07),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=2.1;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.62,0.07),new THREE.MeshBasicMaterial({color:0x44ff44}));hpFg.position.set(0,2.101,0.001);hpFg.userData.bar=true;g.add(hpFg);
  }

  const sp=SPAWNS[Math.floor(Math.random()*SPAWNS.length)];
  g.userData.spawnAge=0; // ms alive — collision disabled for first 1.5s
  g.position.set(sp[0]+(Math.random()-0.5)*3, 0, sp[1]+(Math.random()-0.5)*3);
  scene.add(g);S.zombies.push(g);
}
const RAY=new THREE.Raycaster();
function tickZombies(dt){
  let alive=0;
  for(let i=S.zombies.length-1;i>=0;i--){
    const z=S.zombies[i];
    if(z.userData.dead){
      z.userData.deathT+=dt;z.rotation.z+=dt*0.003;z.position.y-=dt*0.0005;
      if(z.userData.deathT>1400){scene.remove(z);S.zombies.splice(i,1);}
      continue;
    }
    alive++;
    z.userData.spawnAge=(z.userData.spawnAge||0)+dt;
    const graceOver=z.userData.spawnAge>1500; // 1.5s grace period

    const dx=camera.position.x-z.position.x,dz=camera.position.z-z.position.z;
    const dist=Math.sqrt(dx*dx+dz*dz);
    z.rotation.y=Math.atan2(dx,dz);
    z.children.forEach(c=>{if(c.userData.bar)c.lookAt(camera.position);});
    if(dist>0.9){
      const sp=z.userData.speed*dt*0.001;
      let nx=z.position.x+dx/dist*sp;
      let nz=z.position.z+dz/dist*sp;
      nx=Math.max(-36,Math.min(36,nx));
      nz=Math.max(-36,Math.min(36,nz));
      if(graceOver){
        const [rx,rz]=resolveCollision(nx,nz);
        // If collision pushed us far from where we wanted to go → stuck
        const stuck=Math.abs(rx-nx)>0.5||Math.abs(rz-nz)>0.5;
        if(stuck){z.userData.dead=true;continue;} // kill stuck zombie
        nx=rx;nz=rz;
      }
      z.position.x=nx;z.position.z=nz;
      const tw=performance.now()*0.004*z.userData.speed;
      z.children.forEach(c=>{
        if(c.userData.l==='ll')c.rotation.x=Math.sin(tw)*0.5;
        if(c.userData.l==='rl')c.rotation.x=Math.sin(tw+Math.PI)*0.5;
        if(c.userData.l==='la')c.rotation.x=-0.5+Math.sin(tw+Math.PI)*0.35;
        if(c.userData.l==='ra')c.rotation.x=-0.5+Math.sin(tw)*0.35;
      });
    }
    if(dist<1.1){
      z.userData.atkTimer+=dt;
      if(z.userData.atkTimer>900){z.userData.atkTimer=0;S.health=Math.max(0,S.health-z.userData.dmg);updateHUD();flashDmg();if(S.health<=0){gameOver();return;}}
    }
  }
  document.getElementById('hudZombies').textContent='ZOMBIES: '+alive;
}
function hitZombie(z,wid,isHead){
  if(!z||z.userData.dead)return;
  let dmg;
  if(wid==='sledge'){
    // Sledge: 1-shot normal/runner, 4-hit brute (brute has 200hp so 50 per hit)
    dmg = z.userData.type==='brute' ? 50 : z.userData.maxHp;
  } else {
    const base=z.userData.maxHp/WDEFS[wid].hitsToKill;
    dmg=isHead?base*3:base;
  }
  z.userData.hp-=dmg;
  z.children.forEach(c=>{
    if(c.material&&!c.userData.hitbox&&!c.userData.bar){
      const oc=c.material.color&&c.material.color.getHex();
      if(oc!=null){c.material.color.set(0xffffff);setTimeout(()=>{if(c.material)c.material.color.setHex(oc);},75);}
    }
  });
  const bar=z.children.find(c=>c.userData.bar);
  if(bar){const p=Math.max(0,z.userData.hp/z.userData.maxHp);bar.scale.x=p;bar.position.x=-(1-p)*0.31;bar.material.color.setHex(p>0.5?bar.material.color.getHex():p>0.25?0xffaa00:0xff2222);}
  const cred=z.userData.type==='brute'?30:z.userData.type==='runner'?5:8;
  if(z.userData.hp<=0){z.userData.dead=true;S.weapons[wid].kills++;S.totalKills++;S.credits+=isHead?cred*2:cred;updateHUD();hitMark(isHead);checkUnlock(wid);}
  else if(isHead) showHeadshot();
}

// ══ SHOOTING ══
function shoot(){
  if(reloading)return;
  if(boltAnimating)return; // can't shoot during bolt cycle
  const wid=S.held,def=WDEFS[wid],ws=S.weapons[wid];
  const now=performance.now();if(now-ws.lastFire<def.fireMs)return;ws.lastFire=now;
  if(def.type==='gun'){
    if(ws.ammo<=0){notify('OUT OF AMMO - buy more in Shop');soundEmpty();return;}
    ws.ammo--;updateHUD();playGunSound(wid);
    const fl=new THREE.PointLight(0xff9900,6,4);fl.position.set(0,0.05,-0.8);camera.add(fl);setTimeout(()=>camera.remove(fl),40);
    RAY.setFromCamera(new THREE.Vector2(0,0),camera);
    // Sniper hip-fire spread: only 5% chance of accurate shot when not scoped
    if(wid==='sniper'&&!scoped){
      if(Math.random()>0.05){
        const spread=0.35;
        const sx=(Math.random()*2-1)*spread;
        const sy=(Math.random()*2-1)*spread;
        RAY.setFromCamera(new THREE.Vector2(sx,sy),camera);
      }
    }
    const hbs=[];S.zombies.forEach(z=>{z.children.forEach(c=>{if(c.userData.hitbox)hbs.push(c);});});
    const hits=RAY.intersectObjects(hbs);
    // Shotgun has limited range; sniper has extreme range + headshot bonus
    const maxRange=wid==='shotgun'?8:Infinity;
    const hitZombie_=hits.length&&hits[0].distance<=maxRange;
    if(hitZombie_){const h=hits[0].object;hitZombie(h.parent,wid,h.userData.isHead===true);}
    // Bullet hole on world geometry when shot misses or passes through
    if(!hitZombie_||wid==='sniper'){
      const worldHits=RAY.intersectObjects(scene.children,false);
      for(const wh of worldHits){
        const obj=wh.object;
        if(obj.userData.hitbox||obj===weaponGroup||!wh.face) continue;
        if(obj.type==='GridHelper'||!obj.geometry||obj.geometry.type==='PlaneGeometry'&&obj.position.y<0.01) continue;
        if(wh.distance>60) break;
        const normal=wh.face.normal.clone().applyQuaternion(obj.quaternion).normalize();
        spawnDecal(wh.point,normal);
        break;
      }
    }
    weaponGroup.rotation.x-=0.07;setTimeout(()=>weaponGroup.rotation.x+=0.07,90);
    // Bolt-action: start the cycling animation after firing
    if(def.isBoltAction && ws.ammo>0){
      setTimeout(()=>{
        if(S.held==='sniper') startBoltAnim();
      }, 80);
    }
    // Exit scope briefly on sniper shot (scope jump)
    if(wid==='sniper'&&scoped){
      exitScope();
      setTimeout(()=>{ if(S.held==='sniper'&&S.screen==='game') enterScope(); }, BOLT_DUR+120);
    }
  } else {
    // Melee — use per-weapon cooldown
    const meleeCd=def.cooldown||1500;
    if(ws.cd>0) return;
    weaponGroup.rotation.x-=0.32;weaponGroup.rotation.y-=0.14;
    setTimeout(()=>{weaponGroup.rotation.x+=0.32;weaponGroup.rotation.y+=0.14;},150);
    soundKnife();
    ws.cd=meleeCd;
    S.zombies.forEach(z=>{
      if(!z.userData.dead&&camera.position.distanceTo(z.position)<(wid==='sledge'?3.2:2.8)){
        hitZombie(z,wid,false);
      }
    });
  }
}
function hitMark(isHead){
  const h=document.getElementById('hitMarker');
  h.innerHTML=isHead
    ?'<svg width="20" height="20"><line x1="0" y1="0" x2="6" y2="6" stroke="#ffdd00" stroke-width="2"/><line x1="20" y1="0" x2="14" y2="6" stroke="#ffdd00" stroke-width="2"/><line x1="0" y1="20" x2="6" y2="14" stroke="#ffdd00" stroke-width="2"/><line x1="20" y1="20" x2="14" y2="14" stroke="#ffdd00" stroke-width="2"/></svg>'
    :'<svg width="16" height="16"><line x1="0" y1="0" x2="5" y2="5" stroke="#ff3333" stroke-width="1.5"/><line x1="16" y1="0" x2="11" y2="5" stroke="#ff3333" stroke-width="1.5"/><line x1="0" y1="16" x2="5" y2="11" stroke="#ff3333" stroke-width="1.5"/><line x1="16" y1="16" x2="11" y2="11" stroke="#ff3333" stroke-width="1.5"/></svg>';
  h.style.opacity='1';clearTimeout(h._t);h._t=setTimeout(()=>h.style.opacity='0',isHead?180:110);
  if(isHead)showHeadshot();
}
function showHeadshot(){
  const el=document.getElementById('hsPopup');
  el.style.opacity='1';el.style.transform='translate(-50%,-50%) scale(1.1)';
  clearTimeout(el._t);
  el._t=setTimeout(()=>{el.style.opacity='0';el.style.transform='translate(-50%,-50%) scale(0.9)';},700);
}
function flashDmg(){
  const f=document.getElementById('dmgFlash');f.style.opacity='1';setTimeout(()=>f.style.opacity='0',170);
  document.getElementById('hudVign').style.opacity=S.health<30?'1':'0';
}


// ══ INSPECT ══
let inspecting=false, inspectT0=0, inspectDur=2600;
// Detached magazine mesh for SMG mag-check animation
let inspectMag=null;

// Easing helpers
const easeInOut=t=>t<0.5?2*t*t:1-Math.pow(-2*t+2,2)/2;
const easeOut  =t=>1-Math.pow(1-t,3);
const easeIn   =t=>t*t*t;
// Smooth step between two values
const lerp=(a,b,t)=>a+(b-a)*t;

function startInspect(){
  if(inspecting) return;
  inspecting=true;
  inspectT0=performance.now();
  const wid=S.held;

  // Per-weapon duration
  if(wid==='smg')     inspectDur=3200; // mag check takes longer
  else if(wid==='sledge') inspectDur=2800;
  else if(wid==='shotgun') inspectDur=2600;
  else if(wid==='sniper')  inspectDur=3000;
  else if(wid==='knife')   inspectDur=2200;
  else                     inspectDur=2400;

  // Exit scope when inspecting
  if(scoped) exitScope();

  // Build a detached magazine for SMG mag-check
  if(wid==='smg'){
    // Clone the mag geometry from the SMG — use same skin colors
    const ws=S.weapons[wid];
    const def=WDEFS[wid];
    const skin=def.skins.find(s=>s.id===ws.skin)||def.skins[0];
    const [,c2]=skin.colors;
    inspectMag=new THREE.Group();
    const magBody=new THREE.Mesh(new THREE.BoxGeometry(0.035,0.175,0.064),new THREE.MeshLambertMaterial({color:c2}));
    inspectMag.add(magBody);
    // Bullet tips visible at top
    for(let i=0;i<3;i++){
      const bullet=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.007,0.028,6),new THREE.MeshLambertMaterial({color:0xcc8800}));
      bullet.position.set(-0.008+i*0.008, 0.1, 0);
      inspectMag.add(bullet);
    }
    // Base plate
    const base=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.012,0.068),new THREE.MeshLambertMaterial({color:0x444444}));
    base.position.set(0,-0.094,0); inspectMag.add(base);
    camera.add(inspectMag);
    // Start it inside the weapon (will animate out)
    inspectMag.position.set(0.28,-0.32,-0.48);
    inspectMag.visible=false;
  }
}

function tickInspect(now){
  if(!inspecting) return;
  const raw=Math.min((now-inspectT0)/inspectDur, 1);
  const wid=S.held, skin=S.weapons[wid].skin;

  // ─── SLEDGEHAMMER ─────────────────────────────────────────────────────────
  // Phase 0–0.15: lower/swing back as if winding up
  // Phase 0.15–0.55: throw upward — weapon leaves hand (moves up and away, spins 2× on Z)
  // Phase 0.55–0.85: falls back down, still spinning
  // Phase 0.85–1.0: catch and settle back to idle
  if(wid==='sledge'){
    const t=easeInOut(raw);
    if(raw<0.12){
      // Wind-up: dip down and tilt back
      const p=raw/0.12;
      weaponGroup.position.y=lerp(-0.32,-0.55,easeIn(p));
      weaponGroup.position.z=lerp(-0.48,-0.35,easeIn(p));
      weaponGroup.rotation.x=lerp(0, 0.5,easeIn(p));
    } else if(raw<0.55){
      // Throw phase: arc upward, spin 2 full rotations (4π)
      const p=(raw-0.12)/0.43;
      const arc=Math.sin(p*Math.PI); // 0→1→0 arc
      weaponGroup.position.y=lerp(-0.55, 0.55, p) + arc*0.18;
      weaponGroup.position.z=lerp(-0.35,-0.62, easeIn(p));
      weaponGroup.position.x=lerp(0.28, 0.36, Math.sin(p*Math.PI)*0.5);
      weaponGroup.rotation.z=p*Math.PI*4; // two full flips
      weaponGroup.rotation.x=lerp(0.5, -0.3, p);
    } else if(raw<0.88){
      // Fall back down, still rotating through second flip
      const p=(raw-0.55)/0.33;
      weaponGroup.position.y=lerp(0.55,-0.32,easeIn(p));
      weaponGroup.position.z=lerp(-0.62,-0.48,p);
      weaponGroup.position.x=lerp(0.36, 0.28, p);
      weaponGroup.rotation.z=lerp(Math.PI*4, Math.PI*5, easeOut(p)); // finishes extra half-spin on catch
      weaponGroup.rotation.x=lerp(-0.3, 0, easeOut(p));
    } else {
      // Catch and settle
      const p=(raw-0.88)/0.12;
      weaponGroup.rotation.z=lerp(Math.PI*5,0.1,easeOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.32,p);
      weaponGroup.position.z=-0.48;
      weaponGroup.position.x=0.28;
      weaponGroup.rotation.x=lerp(0,0,p);
    }

  // ─── BUTTERFLY KNIFE ──────────────────────────────────────────────────────
  // Elaborate balisong routine: opening flick, aerial spin, closing snap
  } else if(wid==='knife' && skin==='butterfly'){
    const m=weaponGroup.children[0]; // the knife mesh group
    // Phase 0–0.2: opening flick — handles fan open with a snap
    // Phase 0.2–0.65: spin the open knife on finger (rotation on Y)
    // Phase 0.65–0.85: aerial toss and catch
    // Phase 0.85–1.0: handles close with snap
    if(raw<0.2){
      const p=raw/0.2;
      weaponGroup.position.y=-0.32+easeOut(p)*0.04;
      weaponGroup.rotation.z=Math.sin(p*Math.PI)*0.12;
      if(m) m.children.forEach(c=>{
        if(c.userData&&c.userData.bfH===1) c.rotation.z=lerp(0.12, Math.PI*1.05, easeOut(p));
        if(c.userData&&c.userData.bfH===2) c.rotation.z=lerp(-0.12,-Math.PI*1.05,easeOut(p));
      });
    } else if(raw<0.65){
      const p=(raw-0.2)/0.45;
      // Spin open knife like a pro — rotate the whole group on Y
      weaponGroup.rotation.y=0.1+p*Math.PI*2.5;
      weaponGroup.rotation.x=Math.sin(p*Math.PI)*0.2;
      weaponGroup.position.y=-0.32+Math.sin(p*Math.PI)*0.05;
      if(m) m.children.forEach(c=>{
        if(c.userData&&c.userData.bfH===1) c.rotation.z= Math.PI*1.05;
        if(c.userData&&c.userData.bfH===2) c.rotation.z=-Math.PI*1.05;
      });
    } else if(raw<0.85){
      const p=(raw-0.65)/0.2;
      // Aerial toss: go up and forward, full barrel roll on Z
      weaponGroup.rotation.y=lerp(0.1+Math.PI*2.5*0.45/0.45, 0.1, easeOut(p));
      weaponGroup.rotation.z=p*Math.PI*2;
      weaponGroup.position.y=lerp(-0.32, -0.32+0.12*Math.sin(p*Math.PI), p);
      weaponGroup.position.z=lerp(-0.48,-0.58,Math.sin(p*Math.PI));
    } else {
      // Close with a satisfying snap
      const p=(raw-0.85)/0.15;
      weaponGroup.rotation.z=lerp(Math.PI*2, 0, easeOut(p));
      weaponGroup.rotation.y=lerp(weaponGroup.rotation.y, 0.1, easeOut(p));
      weaponGroup.position.y=lerp(-0.2,-0.32,easeOut(p));
      weaponGroup.position.z=-0.48;
      if(m) m.children.forEach(c=>{
        if(c.userData&&c.userData.bfH===1) c.rotation.z=lerp(Math.PI*1.05, 0.12, easeOut(p));
        if(c.userData&&c.userData.bfH===2) c.rotation.z=lerp(-Math.PI*1.05,-0.12,easeOut(p));
      });
    }

  // ─── KARAMBIT ─────────────────────────────────────────────────────────────
  // Spins fast on ring finger, then flips up and does a vertical aerial loop
  } else if(wid==='knife' && skin==='karambit'){
    if(raw<0.3){
      // Fast finger-spin on ring — rapid Z rotation with slight drift
      const p=raw/0.3;
      weaponGroup.rotation.z=p*Math.PI*6; // 3 full spins
      weaponGroup.position.y=-0.32+Math.sin(p*Math.PI*6)*0.008; // tiny vibration
      weaponGroup.rotation.x=Math.sin(p*Math.PI*6)*0.05;
    } else if(raw<0.6){
      // Aerial flip: toss up while doing a forward loop on X
      const p=(raw-0.3)/0.3;
      const arc=Math.sin(p*Math.PI);
      weaponGroup.position.y=-0.32+arc*0.28;
      weaponGroup.position.z=-0.48-arc*0.14;
      weaponGroup.rotation.x=p*Math.PI*2; // full forward loop
      weaponGroup.rotation.z=lerp(Math.PI*6, Math.PI*6+Math.PI, easeInOut(p));
    } else if(raw<0.85){
      // Catch and do a reverse half-spin to show the blade
      const p=(raw-0.6)/0.25;
      weaponGroup.position.y=lerp(-0.32+0,-0.32,easeOut(p));
      weaponGroup.position.z=-0.48;
      weaponGroup.rotation.x=lerp(Math.PI*2, 0, easeOut(p));
      weaponGroup.rotation.z=lerp(Math.PI*7, Math.PI*8, easeOut(p)); // one more dramatic spin
      weaponGroup.rotation.y=lerp(0.1, -0.3, Math.sin(p*Math.PI)); // tilt to show blade to camera
    } else {
      // Settle
      const p=(raw-0.85)/0.15;
      weaponGroup.rotation.z=lerp(Math.PI*8, 0.1, easeOut(p));
      weaponGroup.rotation.x=lerp(0, 0, p);
      weaponGroup.rotation.y=lerp(-0.3, 0.1, easeOut(p));
    }

  // ─── SHADOW DAGGER ────────────────────────────────────────────────────────
  // Reverse-grip twirl, figure-8 wrist motion, dramatic stab-forward then pull back
  } else if(wid==='knife' && skin==='shadow'){
    if(raw<0.25){
      // Flip to reverse grip (rotate 180° on X)
      const p=raw/0.25;
      weaponGroup.rotation.x=lerp(0, Math.PI, easeOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.28,Math.sin(p*Math.PI));
    } else if(raw<0.55){
      // Wrist figure-8: combined X+Z oscillation
      const p=(raw-0.25)/0.3;
      weaponGroup.rotation.x=Math.PI+Math.sin(p*Math.PI*3)*0.35;
      weaponGroup.rotation.z=Math.cos(p*Math.PI*3)*0.4;
      weaponGroup.rotation.y=0.1+Math.sin(p*Math.PI*2)*0.25;
      weaponGroup.position.y=-0.28+Math.sin(p*Math.PI*4)*0.02;
    } else if(raw<0.75){
      // Stab forward dramatically
      const p=(raw-0.55)/0.2;
      weaponGroup.position.z=lerp(-0.48,-0.7,easeOut(p));
      weaponGroup.rotation.x=lerp(Math.PI, 0.1, easeOut(p)); // back to forward grip
      weaponGroup.rotation.z=lerp(Math.cos(0.3*Math.PI*3)*0.4, 0, easeOut(p));
    } else {
      // Pull back to idle
      const p=(raw-0.75)/0.25;
      weaponGroup.position.z=lerp(-0.7,-0.48,easeOut(p));
      weaponGroup.rotation.x=lerp(0.1,0,p);
    }

  // ─── STANDARD KNIFE ───────────────────────────────────────────────────────
  // Tosses blade up, does a half-flip, catches by blade side, then reverse-flips to handle
  } else if(wid==='knife'){
    if(raw<0.35){
      // Toss and flip on Z
      const p=raw/0.35;
      const arc=Math.sin(p*Math.PI);
      weaponGroup.position.y=-0.32+arc*0.2;
      weaponGroup.position.z=-0.48-arc*0.1;
      weaponGroup.rotation.z=p*Math.PI; // half flip
      weaponGroup.rotation.y=0.1+Math.sin(p*Math.PI)*0.3;
    } else if(raw<0.55){
      // Catch (pause at top of flip — blade pointing down toward hand)
      const p=(raw-0.35)/0.2;
      weaponGroup.rotation.z=lerp(Math.PI, Math.PI, p); // hold upside down
      weaponGroup.position.y=-0.32+lerp(0.2,0.05,easeIn(p));
      weaponGroup.position.z=-0.48;
      // Tilt to "look at the blade"
      weaponGroup.rotation.x=lerp(0, 0.4, easeInOut(p));
      weaponGroup.rotation.y=lerp(0.1+0.3,0.6,easeInOut(p));
    } else if(raw<0.8){
      // Flip back right-way round
      const p=(raw-0.55)/0.25;
      weaponGroup.rotation.z=lerp(Math.PI, Math.PI*2, easeOut(p));
      weaponGroup.position.y=lerp(-0.27,-0.32,easeOut(p));
      weaponGroup.rotation.x=lerp(0.4,0,easeOut(p));
      weaponGroup.rotation.y=lerp(0.6,0.1,easeOut(p));
    } else {
      // Settle: one small satisfying wrist flick
      const p=(raw-0.8)/0.2;
      weaponGroup.rotation.z=lerp(Math.PI*2, 0.1, easeOut(p));
      weaponGroup.rotation.y=0.1+Math.sin(p*Math.PI)*0.12;
    }

  // ─── MP5 MAG CHECK ────────────────────────────────────────────────────────
  // Phase 0–0.2:  tilt gun left and lower slightly
  // Phase 0.2–0.4: mag drops out — separate mesh falls down
  // Phase 0.4–0.65: hold mag up, tilt it to see bullets
  // Phase 0.65–0.8: re-insert mag with firm push
  // Phase 0.8–1.0: tilt gun back up, rack the charging handle
  } else if(wid==='smg'){
    if(raw<0.18){
      // Tilt the SMG to the side to access the mag well
      const p=raw/0.18;
      weaponGroup.rotation.z=lerp(0, 0.55, easeInOut(p));
      weaponGroup.rotation.y=lerp(0.1,-0.2, easeInOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.42,easeInOut(p));
    } else if(raw<0.35){
      // Mag ejects — make it visible and animate it down
      const p=(raw-0.18)/0.17;
      if(inspectMag){
        inspectMag.visible=true;
        // Start at the mag well position (approx) and drop down
        inspectMag.position.set(
          lerp(0.3, 0.32, p),
          lerp(-0.42,-0.42-0.28*easeIn(p), p),
          lerp(-0.48,-0.36,p)
        );
        inspectMag.rotation.z=lerp(0, 0.55+0.05, p);
        inspectMag.rotation.x=lerp(0, 0.1, p);
      }
      weaponGroup.rotation.z=0.55;
      weaponGroup.rotation.y=-0.2;
      weaponGroup.position.y=-0.42;
    } else if(raw<0.62){
      // Hold mag up and examine — tilt it to see the rounds
      const p=(raw-0.35)/0.27;
      if(inspectMag){
        inspectMag.visible=true;
        // Float mag up to eye level
        inspectMag.position.set(
          lerp(0.32, 0.15, easeOut(p)),
          lerp(-0.42-0.28, -0.08, easeOut(p)),
          lerp(-0.36,-0.62, easeOut(p))
        );
        // Tilt mag to show bullets at top
        inspectMag.rotation.x=lerp(0.1, -0.6, easeInOut(p));
        inspectMag.rotation.z=lerp(0.6, 0.2, easeOut(p));
        inspectMag.rotation.y=lerp(0, 0.3, easeOut(p));
      }
      // Gun just held to the side
      weaponGroup.rotation.z=lerp(0.55, 0.45, easeInOut(p));
      weaponGroup.rotation.y=lerp(-0.2,-0.3,easeInOut(p));
    } else if(raw<0.78){
      // Re-insert mag — bring it back down to mag well with a solid click
      const p=(raw-0.62)/0.16;
      if(inspectMag){
        inspectMag.visible=true;
        inspectMag.position.set(
          lerp(0.15, 0.30, easeIn(p)),
          lerp(-0.08, -0.42, easeIn(p)),
          lerp(-0.62,-0.48,easeIn(p))
        );
        inspectMag.rotation.x=lerp(-0.6, 0.0, easeIn(p));
        inspectMag.rotation.z=lerp(0.2,0.55,easeIn(p));
        inspectMag.rotation.y=lerp(0.3,0,easeIn(p));
      }
      weaponGroup.rotation.z=lerp(0.45,0.55,p);
    } else if(raw<0.9){
      // Hide mag (now back in gun), tilt gun back up
      const p=(raw-0.78)/0.12;
      if(inspectMag){ inspectMag.visible=false; }
      weaponGroup.rotation.z=lerp(0.55, 0, easeOut(p));
      weaponGroup.rotation.y=lerp(-0.2, 0.1, easeOut(p));
      weaponGroup.position.y=lerp(-0.42,-0.32,easeOut(p));
    } else {
      // Rack the charging handle — quick forward-back snap on X
      const p=(raw-0.9)/0.1;
      if(inspectMag){ inspectMag.visible=false; }
      weaponGroup.rotation.z=lerp(0, 0.1, easeOut(p));
      weaponGroup.position.z=-0.48+Math.sin(p*Math.PI)*0.04; // forward-back racking motion
      weaponGroup.rotation.y=0.1;
      weaponGroup.position.y=-0.32;
    }

  // ─── SHOTGUN ──────────────────────────────────────────────────────────────
  // Break-open inspection: tilt down, crack the action, peer through barrels, snap closed
  } else if(wid==='shotgun'){
    if(raw<0.22){
      // Bring the gun up and tilt it across the body
      const p=raw/0.22;
      weaponGroup.rotation.z=lerp(0, -0.7, easeInOut(p));
      weaponGroup.rotation.y=lerp(0.1, -0.5, easeInOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.18,easeInOut(p));
    } else if(raw<0.45){
      // Break action open — rotate the barrels down on X (hinge point near receiver)
      const p=(raw-0.22)/0.23;
      weaponGroup.rotation.x=lerp(0, 0.7, easeInOut(p)); // barrels drop
      weaponGroup.position.y=-0.18+lerp(0,-0.06,easeInOut(p));
    } else if(raw<0.65){
      // Peer through the barrels — hold open, maybe a slight lean
      const p=(raw-0.45)/0.2;
      weaponGroup.rotation.x=0.7;
      weaponGroup.rotation.y=lerp(-0.5,-0.65,Math.sin(p*Math.PI)*0.3);
      weaponGroup.position.z=-0.48+Math.sin(p*Math.PI)*0.03;
    } else if(raw<0.82){
      // Snap it shut — quick close
      const p=(raw-0.65)/0.17;
      weaponGroup.rotation.x=lerp(0.7, 0, easeOut(p));
      weaponGroup.position.y=lerp(-0.24,-0.32,easeOut(p));
    } else {
      // Return to idle
      const p=(raw-0.82)/0.18;
      weaponGroup.rotation.z=lerp(-0.7, 0, easeOut(p));
      weaponGroup.rotation.y=lerp(-0.5, 0.1, easeOut(p));
    }

  // ─── SNIPER BULLET INSPECT ────────────────────────────────────────────────
  // Phase 0–0.2:  tilt gun across body, reach to bolt
  // Phase 0.2–0.4: pull bolt back, eject a cartridge upward
  // Phase 0.4–0.65: catch and hold the cartridge up — examine it
  // Phase 0.65–0.8: push cartridge back in, close bolt
  // Phase 0.8–1.0:  return to idle
  } else if(wid==='sniper'){
    if(!inspectMag){
      // Create a bullet cartridge mesh
      inspectMag=new THREE.Group();
      const case_=new THREE.Mesh(new THREE.CylinderGeometry(0.012,0.011,0.075,8),new THREE.MeshLambertMaterial({color:0xcc8800}));
      inspectMag.add(case_);
      const tip=new THREE.Mesh(new THREE.ConeGeometry(0.01,0.03,8),new THREE.MeshLambertMaterial({color:0xdddddd}));
      tip.position.y=0.052; inspectMag.add(tip);
      const rim=new THREE.Mesh(new THREE.CylinderGeometry(0.014,0.014,0.006,8),new THREE.MeshLambertMaterial({color:0x888866}));
      rim.position.y=-0.04; inspectMag.add(rim);
      inspectMag.visible=false;
      camera.add(inspectMag);
    }
    if(raw<0.2){
      const p=raw/0.2;
      weaponGroup.rotation.z=lerp(0,-0.45,easeInOut(p));
      weaponGroup.rotation.y=lerp(0.1,-0.3,easeInOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.24,easeInOut(p));
    } else if(raw<0.38){
      // Bolt handle moves back (animate bolt group)
      const p=(raw-0.2)/0.18;
      weaponGroup.rotation.z=-0.45; weaponGroup.rotation.y=-0.3;
      const mesh=weaponGroup.children[0];
      if(mesh) mesh.children.forEach(c=>{
        if(c.userData.boltHandle){
          c.position.y=0.015+easeOut(p)*0.025;
          c.position.z=0.04-easeOut(p)*0.06;
        }
      });
      // Cartridge pops out at end of bolt pull
      if(p>0.7){
        inspectMag.visible=true;
        inspectMag.position.set(0.28+lerp(0,0.05,p),-0.32+lerp(0,0.1,p),-0.48);
        inspectMag.rotation.set(0,0,0.45);
      }
    } else if(raw<0.62){
      // Lift cartridge up to eye level and tilt to inspect
      const p=(raw-0.38)/0.24;
      inspectMag.visible=true;
      inspectMag.position.set(
        lerp(0.33, 0.06, easeOut(p)),
        lerp(-0.22,  0.04, easeOut(p)),
        lerp(-0.48, -0.68, easeOut(p))
      );
      inspectMag.rotation.set(lerp(0,-0.6,easeOut(p)), lerp(0,0.4,easeOut(p)), lerp(0.45,-0.2,easeOut(p)));
      weaponGroup.rotation.z=-0.45; weaponGroup.rotation.y=-0.3;
    } else if(raw<0.78){
      // Slide cartridge back into chamber, close bolt
      const p=(raw-0.62)/0.16;
      inspectMag.position.set(
        lerp(0.06, 0.28, easeIn(p)),
        lerp(0.04,-0.32, easeIn(p)),
        lerp(-0.68,-0.48, easeIn(p))
      );
      inspectMag.rotation.set(lerp(-0.6,0,p),lerp(0.4,0,p),lerp(-0.2,0,p));
      // Close bolt
      const mesh=weaponGroup.children[0];
      if(mesh) mesh.children.forEach(c=>{
        if(c.userData.boltHandle){
          c.position.y=lerp(0.04,0.015,easeOut(p));
          c.position.z=lerp(-0.02,0.04,easeOut(p));
        }
      });
      if(p>0.8) inspectMag.visible=false;
    } else {
      // Return to idle
      const p=(raw-0.78)/0.22;
      if(inspectMag) inspectMag.visible=false;
      weaponGroup.rotation.z=lerp(-0.45,0,easeOut(p));
      weaponGroup.rotation.y=lerp(-0.3,0.1,easeOut(p));
      weaponGroup.position.y=lerp(-0.24,-0.32,easeOut(p));
    }

  // ─── PISTOL / REVOLVER / DEAGLE / COMPACT / DEFAULT ──────────────────────
  // Chamber check: tilt to look at ejection port, rack slide, return
  } else {
    if(raw<0.22){
      // Bring up and tilt right to see the ejection port
      const p=raw/0.22;
      weaponGroup.rotation.z=lerp(0,-0.55,easeInOut(p));
      weaponGroup.rotation.y=lerp(0.1,-0.4,easeInOut(p));
      weaponGroup.position.y=lerp(-0.32,-0.22,easeInOut(p));
    } else if(raw<0.45){
      // Rack the slide — forward and back
      const p=(raw-0.22)/0.23;
      weaponGroup.position.z=lerp(-0.48,-0.62,easeOut(p));
      weaponGroup.rotation.x=lerp(0,-0.15,easeOut(p));
      // Also push index finger up (simulated by Y jog)
      weaponGroup.position.y=-0.22+Math.sin(p*Math.PI)*0.03;
    } else if(raw<0.62){
      const p=(raw-0.45)/0.17;
      weaponGroup.position.z=lerp(-0.62,-0.48,easeOut(p));
      weaponGroup.rotation.x=lerp(-0.15,0,easeOut(p));
    } else if(raw<0.82){
      // Tilt further to show round in chamber — rotate toward camera
      const p=(raw-0.62)/0.2;
      weaponGroup.rotation.z=lerp(-0.55,-0.3,Math.sin(p*Math.PI)*0.4);
      weaponGroup.rotation.y=lerp(-0.4, 0.2, easeInOut(p));
    } else {
      // Return to idle
      const p=(raw-0.82)/0.18;
      weaponGroup.rotation.z=lerp(-0.3, 0, easeOut(p));
      weaponGroup.rotation.y=lerp(0.2, 0.1, easeOut(p));
      weaponGroup.position.y=lerp(-0.22,-0.32,easeOut(p));
    }
  }

  if(raw>=1){
    inspecting=false;
    weaponGroup.position.set(0.28,-0.32,-0.48);
    weaponGroup.rotation.set(0,0.1,0);
    // Clean up mag object
    if(inspectMag){
      camera.remove(inspectMag);
      inspectMag=null;
    }
  }
}


// ── BOLT ACTION ──
function startBoltAnim(){ boltAnimating=true; boltT0=performance.now(); }
function tickBolt(now){
  if(!boltAnimating) return;
  const t=Math.min((now-boltT0)/BOLT_DUR,1);
  // Phase 0–0.3: lift and pull bolt back (Y up, Z back)
  // Phase 0.3–0.6: push bolt forward (chamber new round)
  // Phase 0.6–1.0: push bolt down and lock
  if(S.held!=='sniper'){boltAnimating=false;return;}
  const mesh=weaponGroup.children[0];
  if(!mesh){boltAnimating=false;return;}
  // Animate the bolt handle pieces
  mesh.children.forEach(c=>{
    if(!c.userData.boltHandle) return;
    if(t<0.3){
      const p=t/0.3;
      // Lift up (rotate handle up) then pull back
      c.position.y=0.015+easeOut(p)*0.025;
      c.position.z=0.04-easeOut(p)*0.06;
    } else if(t<0.6){
      const p=(t-0.3)/0.3;
      // Push forward
      c.position.y=0.04-easeIn(p)*0.025;
      c.position.z=-0.02+easeIn(p)*0.06;
    } else {
      const p=(t-0.6)/0.4;
      // Lock down
      c.position.y=lerp(0.015,0.015,p);
      c.position.z=lerp(0.04,0.04,p);
    }
  });
  // Whole gun also has a slight recoil lift and recovery during bolt
  if(t<0.15){
    weaponGroup.position.z=-0.48+easeOut(t/0.15)*0.03;
  } else if(t<0.5){
    weaponGroup.position.z=lerp(-0.45,-0.48,easeOut((t-0.15)/0.35));
  }
  if(t>=1){
    boltAnimating=false;
    weaponGroup.position.set(0.28,-0.32,-0.48);
    weaponGroup.rotation.set(0,0.1,0);
    // Restore bolt handle positions
    if(mesh) mesh.children.forEach(c=>{
      if(c.userData.boltHandle){c.position.y=0.015;c.position.z=0.04;}
    });
  }
}


const keys={};let yaw=0,pitch=0,locked=false,hbob=0,lastT=0;
const SETTINGS={fov:72,sensitivity:1.0,brightness:100};
function applySetting(key,val){
  if(key==='fov'){SETTINGS.fov=parseInt(val);camera.fov=SETTINGS.fov;camera.updateProjectionMatrix();document.getElementById('valFov').textContent=val+'°';}
  if(key==='sensitivity'){SETTINGS.sensitivity=parseFloat(val)/9;document.getElementById('valSens').textContent=(SETTINGS.sensitivity).toFixed(1)+'×';}
  if(key==='brightness'){SETTINGS.brightness=parseInt(val);document.body.style.filter=val==100?'':'brightness('+val/100+')';document.getElementById('valBright').textContent=val+'%';}
}
window.applySetting=applySetting;
document.addEventListener('keydown',e=>{
  keys[e.code]=true;
  if(S.screen==='paused'&&(e.code==='Escape'||e.code==='KeyP')){resume();return;}
  if(S.screen!=='game')return;
  if(e.code==='Digit1') swSlot('primary');
  if(e.code==='Digit2') swSlot('secondary');
  if(e.code==='Digit3'||e.code==='KeyQ') swSlot('knife');
  if(e.code==='KeyF')startInspect();
  if(e.code==='KeyH')useKit();
  if(e.code==='KeyR')doReload();
  if(e.code==='Space'&&S.screen==='game'&&onGround){velY=JUMP_VEL;onGround=false;}
});
document.addEventListener('keyup',e=>keys[e.code]=false);
document.addEventListener('mousemove',e=>{
  if(S.screen==='game'&&locked){
    yaw-=e.movementX*0.0018*SETTINGS.sensitivity;
    pitch=Math.max(-1.3,Math.min(1.3,pitch-e.movementY*0.0018*SETTINGS.sensitivity));
    camera.rotation.y=yaw;camera.rotation.x=pitch;
  }
});
let lockRequested=false;
cvs.addEventListener('click',()=>{
  if((S.screen==='game'||S.screen==='paused')&&!locked){
    lockRequested=true;
    cvs.requestPointerLock();
  }
});
document.addEventListener('pointerlockchange',()=>{
  locked=document.pointerLockElement===cvs;
  if(locked){
    lockRequested=false;
    // Re-entering lock while paused → resume
    if(S.screen==='paused') resume();
  } else {
    // Lock lost — only pause if it was a genuine mid-game loss, not us requesting it
    if(!lockRequested && S.screen==='game'){
      setTimeout(()=>{
        // Double-check after 80ms: if still no lock and still playing, pause
        if(!locked && S.screen==='game') pause();
      }, 80);
    }
    lockRequested=false;
  }
});
document.addEventListener('mousedown',e=>{
  // If game is running but not locked, clicking should re-acquire (don't shoot yet)
  if(S.screen==='game'&&!locked&&e.button===0){lockRequested=true;cvs.requestPointerLock();return;}
  if(S.screen!=='game'||!locked)return;
  if(e.button===2){ enterScope(); return; }
  if(e.button!==0)return;
  shoot();
  if(WDEFS[S.held].fireMs<200&&WDEFS[S.held].type==='gun'){clearInterval(S.autoFire);S.autoFire=setInterval(()=>{if(S.screen==='game')shoot();else clearInterval(S.autoFire);},WDEFS[S.held].fireMs);}
});
document.addEventListener('mouseup',e=>{
  if(e.button===0)clearInterval(S.autoFire);
  if(e.button===2&&S.screen==='game') exitScope();
});

// ── SCOPE / ADS ──
let scoped=false;
const SCOPE_FOV=14, NORMAL_FOV=()=>SETTINGS.fov;

function updateScopeReticle(){
  const cx=innerWidth/2, cy=innerHeight/2;
  const r=Math.min(innerWidth,innerHeight)*0.28;
  const gap=20;
  // Main crosshair lines (stop at centre gap)
  document.getElementById('sL1').setAttribute('x1',cx-r);document.getElementById('sL1').setAttribute('y1',cy);document.getElementById('sL1').setAttribute('x2',cx-gap);document.getElementById('sL1').setAttribute('y2',cy);
  document.getElementById('sL2').setAttribute('x1',cx+gap);document.getElementById('sL2').setAttribute('y1',cy);document.getElementById('sL2').setAttribute('x2',cx+r);document.getElementById('sL2').setAttribute('y2',cy);
  document.getElementById('sL3').setAttribute('x1',cx);document.getElementById('sL3').setAttribute('y1',cy-r);document.getElementById('sL3').setAttribute('x2',cx);document.getElementById('sL3').setAttribute('y2',cy-gap);
  document.getElementById('sL4').setAttribute('x1',cx);document.getElementById('sL4').setAttribute('y1',cy+gap);document.getElementById('sL4').setAttribute('x2',cx);document.getElementById('sL4').setAttribute('y2',cy+r);
  // Mil-dot marks along horizontal at -3/-2/+2/+3 mrad positions
  [r*0.3, r*0.55, r*0.3, r*0.55].forEach((dist,i)=>{
    const sign=i<2?-1:1;
    const el=document.getElementById(['sM1','sM2','sM3','sM4'][i]);
    el.setAttribute('x1',cx+sign*dist); el.setAttribute('y1',cy-6);
    el.setAttribute('x2',cx+sign*dist); el.setAttribute('y2',cy+6);
  });
  // Circle radius — use pixel value (SVG absolute coords)
  document.getElementById('scopeCircle').setAttribute('r', r);
  document.getElementById('scopeRing').setAttribute('r', r);
  document.getElementById('scopeCircle').setAttribute('cx', cx);
  document.getElementById('scopeCircle').setAttribute('cy', cy);
  document.getElementById('scopeRing').setAttribute('cx', cx);
  document.getElementById('scopeRing').setAttribute('cy', cy);
  document.getElementById('sDot').setAttribute('cx', cx);
  document.getElementById('sDot').setAttribute('cy', cy);
}

function enterScope(){
  if(S.held!=='sniper'||inspecting||boltAnimating) return;
  scoped=true;
  camera.fov=SCOPE_FOV; camera.updateProjectionMatrix();
  document.getElementById('scopeOverlay').style.display='block';
  document.querySelector('.crosshair').style.display='none';
  // Hide the gun while scoped (realistic)
  weaponGroup.visible=false;
  updateScopeReticle();
}
function exitScope(){
  if(!scoped) return;
  scoped=false;
  camera.fov=NORMAL_FOV(); camera.updateProjectionMatrix();
  document.getElementById('scopeOverlay').style.display='none';
  document.querySelector('.crosshair').style.display='block';
  weaponGroup.visible=true;
}
window.addEventListener('resize',()=>{ if(scoped) updateScopeReticle(); });
function swSlot(slot){
  const ids=Object.keys(WDEFS).filter(id=>WDEFS[id].slot===slot&&S.weapons[id]&&S.weapons[id].owned);
  if(!ids.length){notify('No '+slot+' weapon owned!');return;}
  // Already holding a weapon from this slot — do nothing
  if(WDEFS[S.held]&&WDEFS[S.held].slot===slot) return;
  // Switch to the tracked active weapon for this slot
  const cur=slot==='primary'?S.heldPrimary:slot==='secondary'?S.heldSecondary:S.heldKnife;
  const target=ids.includes(cur)?cur:ids[0];
  sw(target);
}
cvs.addEventListener('contextmenu',e=>e.preventDefault());

function sw(id){
  if(!S.weapons[id]||!S.weapons[id].owned){notify('Not owned!');return;}
  exitScope(); // always exit scope when switching weapons
  S.held=id;
  if(WDEFS[id].slot==='primary')   S.heldPrimary=id;
  if(WDEFS[id].slot==='secondary') S.heldSecondary=id;
  if(WDEFS[id].slot==='knife')     S.heldKnife=id;
  reloading=false;reloadTimer=0;
  rebuildWeapon();updateHUD();
}
function useKit(){if(S.kits<=0){notify('No kits! Buy in Shop');return;}if(S.health>=100){notify('Already at full HP');return;}S.kits--;S.health=Math.min(100,S.health+HEAL_AMT);updateHUD();notify('+'+HEAL_AMT+' HP');}
function doReload(){
  const wid=S.held,ws=S.weapons[wid],def=WDEFS[wid];
  if(def.type==='melee')return;
  if(reloading)return;
  if(ws.ammo>=def.startAmmo){notify('Magazine already full!');return;}
  if(ws.res<=0){notify('No reserve ammo!');return;}
  reloading=true;reloadTimer=3000;reloadWid=wid;
  clearInterval(S.autoFire);
}

// ══ LOOP ══
const GRAVITY=0.000028, JUMP_VEL=0.009, GROUND_Y=1.75;
// Stamina: 5s to drain, 7s to regen
const STAM_MAX=5000;
const STAM_DRAIN_RATE = STAM_MAX/5000;  // 1 unit/ms → depletes in 5000ms
const STAM_REGEN_RATE = STAM_MAX/7000;  // ~0.714 unit/ms → regens in 7000ms

let velY=0, onGround=true;
let stamina=STAM_MAX, stamDepleted=false;
let reloading=false, reloadTimer=0, reloadWid='';
let boltAnimating=false, boltT0=0;
const BOLT_DUR=900; // ms for bolt cycle animation

function resolveCollision(px,pz,playerY){
  // Clamp to arena
  px=Math.max(-37,Math.min(37,px));
  pz=Math.max(-37,Math.min(37,pz));
  // feetY: how high above ground the player's feet are (0 on ground, >0 while airborne)
  const feetY = (playerY!==undefined) ? playerY - GROUND_Y : 0;
  for(const c of COL){
    // Skip collision if player's feet have cleared the top of this obstacle
    if(c.maxY!==undefined && feetY >= c.maxY - 0.05) continue;
    if(c.type==='box'){
      if(px>c.minX&&px<c.maxX&&pz>c.minZ&&pz<c.maxZ){
        const overL=px-c.minX, overR=c.maxX-px, overF=pz-c.minZ, overB=c.maxZ-pz;
        const minO=Math.min(overL,overR,overF,overB);
        if(minO===overL) px=c.minX;
        else if(minO===overR) px=c.maxX;
        else if(minO===overF) pz=c.minZ;
        else pz=c.maxZ;
      }
    } else {
      const dx=px-c.x, dz=pz-c.z, dist=Math.sqrt(dx*dx+dz*dz);
      if(dist<c.r&&dist>0.001){ px+=dx/dist*(c.r-dist); pz+=dz/dist*(c.r-dist); }
    }
  }
  return [px,pz];
}

function loop(ts){
  requestAnimationFrame(loop);
  if(S.screen==='game'){
    const dt=Math.min(ts-lastT,150);lastT=ts;

    // ── Sprint & stamina ──
    const wantSprint=keys['ShiftLeft']&&!stamDepleted;
    if(wantSprint){
      stamina=Math.max(0,stamina-STAM_DRAIN_RATE*dt);
      if(stamina<=0){stamina=0;stamDepleted=true;}
    } else {
      stamina=Math.min(STAM_MAX,stamina+STAM_REGEN_RATE*dt);
      if(stamDepleted&&stamina>=STAM_MAX) stamDepleted=false;
    }
    const spd=(wantSprint?0.009:0.005)*dt;
    const stamPct=stamina/STAM_MAX;
    const sf=document.getElementById('stamFill');
    sf.style.width=(stamPct*100)+'%';
    sf.className='stam-bar-fill'+(stamDepleted?' depleted':wantSprint?'':' recharging');

    // ── Movement ──
    const fwd=new THREE.Vector3(-Math.sin(yaw),0,-Math.cos(yaw));
    const rgt=new THREE.Vector3(Math.cos(yaw),0,-Math.sin(yaw));
    let mv=false;
    let nx=camera.position.x, nz=camera.position.z;
    if(keys['KeyW']){nx+=fwd.x*spd;nz+=fwd.z*spd;mv=true;}
    if(keys['KeyS']){nx-=fwd.x*spd;nz-=fwd.z*spd;mv=true;}
    if(keys['KeyA']){nx-=rgt.x*spd;nz-=rgt.z*spd;mv=true;}
    if(keys['KeyD']){nx+=rgt.x*spd;nz+=rgt.z*spd;mv=true;}
    [nx,nz]=resolveCollision(nx,nz,camera.position.y);
    camera.position.x=nx; camera.position.z=nz;

    // ── Jump & gravity ──
    if(!onGround) velY-=GRAVITY*dt;
    camera.position.y+=velY*dt;
    if(camera.position.y<=GROUND_Y){
      camera.position.y=GROUND_Y; velY=0; onGround=true;
    }

    // ── Head bob ──
    if(mv&&!inspecting){
      const bobSpd=wantSprint?0.008:0.005;
      hbob+=dt*bobSpd;
      weaponGroup.position.y=-0.32+Math.sin(hbob*2)*0.012;
      weaponGroup.position.x=0.28+Math.sin(hbob)*0.008;
    }

    tickInspect(ts); tickBolt(ts); tickZombies(dt);tickWave(dt); tickDecals(ts);

    // ── Melee cooldown bar / Sniper bolt bar ──
    const def=WDEFS[S.held];
    const ws=S.weapons[S.held];
    if(def.type==='melee'){
      if(ws.cd>0){
        const meleeCd=def.cooldown||1500;
        ws.cd=Math.max(0,ws.cd-dt);
        const bar=document.getElementById('knifeCoolBar');
        const fill=document.getElementById('knifeCoolFill');
        bar.style.opacity='1';
        fill.style.width=(ws.cd/meleeCd*100)+'%';
        if(ws.cd<=0) bar.style.opacity='0';
      }
    } else if(S.held==='sniper'&&boltAnimating){
      // Show bolt-cycle progress bar under crosshair
      const elapsed=performance.now()-boltT0;
      const pct=Math.max(0,1-elapsed/BOLT_DUR);
      const bar=document.getElementById('knifeCoolBar');
      const fill=document.getElementById('knifeCoolFill');
      bar.style.opacity='1';
      fill.style.width=(pct*100)+'%';
      fill.style.background='linear-gradient(90deg,#00ccff,#0066ff)';
    } else {
      document.getElementById('knifeCoolBar').style.opacity='0';
    }

    // ── Reload timer ──
    if(reloading){
      reloadTimer-=dt;
      if(reloadTimer<=0){
        reloading=false;
        const rws=S.weapons[reloadWid],rdef=WDEFS[reloadWid];
        const need=rdef.startAmmo-rws.ammo,got=Math.min(need,rws.res);
        rws.ammo+=got;rws.res-=got;
        updateHUD();
      } else {
        const pct=1-reloadTimer/3000;
        notify('RELOADING... '+(pct*100|0)+'%');
      }
    }
  } else {
    lastT=ts;
  }
  // Always render every frame regardless of screen state
  renderer.render(scene,camera);
}

// ══ HUD ══
function updateHUD(){
  const wid=S.held,def=WDEFS[wid],ws=S.weapons[wid];
  const skin=def.skins.find(s=>s.id===ws.skin)||def.skins[0];
  document.getElementById('hudCred').textContent='CREDITS: '+S.credits;
  document.getElementById('hudKills').textContent='KILLS: '+S.totalKills;
  document.getElementById('hudKits').textContent='KITS: '+S.kits;
  document.getElementById('hudWepName').textContent=def.name.toUpperCase();
  document.getElementById('hudSkinName').textContent=skin.name;
  document.getElementById('hudWave').textContent='WAVE '+S.wave;
  const hp=Math.max(0,S.health);
  document.getElementById('hpFill').style.width=hp+'%';
  document.getElementById('hpText').textContent=Math.ceil(hp);
  document.getElementById('hudVign').style.opacity=hp<30?'0.9':'0';
  if(def.type==='gun'){document.getElementById('hudAmmoBlock').style.display='block';document.getElementById('ammoCur').textContent=ws.ammo;document.getElementById('ammoRes').textContent='/ '+ws.res;}
  else document.getElementById('hudAmmoBlock').style.display='none';
  // Live skin progress bar
  const nextSkin=def.skins.find(s=>!S.unlocked[wid].includes(s.id));
  const prog=document.getElementById('hudSkinProg');
  if(nextSkin){
    const prevKills=def.skins.reduce((mx,s)=>S.unlocked[wid].includes(s.id)?Math.max(mx,s.kills):mx,0);
    const pct=prevKills===nextSkin.kills?0:Math.min(1,(ws.kills-prevKills)/(nextSkin.kills-prevKills));
    document.getElementById('hudSpFill').style.width=(pct*100).toFixed(1)+'%';
    document.getElementById('hudSpLabel').textContent=nextSkin.name.toUpperCase();
    document.getElementById('hudSpPct').textContent=Math.round(pct*100)+'%';
    prog.style.display='flex';
  } else {
    prog.style.display='none'; // all skins unlocked
  }
}

// ══ SKIN UNLOCK ══
function checkUnlock(wid){
  const kills=S.weapons[wid].kills;
  WDEFS[wid].skins.forEach(sk=>{
    if(sk.id==='default')return;
    if(!S.unlocked[wid].includes(sk.id)&&kills>=sk.kills){S.unlocked[wid].push(sk.id);S.credits+=SKIN_BONUS;S.unlockQueue.push({wid,skin:sk});}
  });
  if(!S.showingUnlock&&S.unlockQueue.length)showNextUnlock();
}
let ubRend=null,ubScene=null,ubCam=null,ubMesh=null,ubMX=false,ubLX=0,ubLY=0,ubRX=0,ubRY=0;
function showNextUnlock(){
  if(!S.unlockQueue.length){S.showingUnlock=false;return;}
  S.showingUnlock=true;
  const {wid,skin}=S.unlockQueue.shift();
  document.getElementById('ubTitle').textContent=skin.name.toUpperCase();
  document.getElementById('ubSub').textContent=WDEFS[wid].name+' '+skin.rarity.toUpperCase()+' +'+SKIN_BONUS+' CREDITS';
  document.getElementById('ubSub').style.color=RC[skin.rarity];
  if(!ubRend){
    const c2=document.getElementById('ubCanvas');
    ubRend=new THREE.WebGLRenderer({canvas:c2,antialias:true,alpha:true});
    ubRend.setSize(300,200);ubRend.setPixelRatio(Math.min(devicePixelRatio,2));
    ubScene=new THREE.Scene();ubScene.background=new THREE.Color(0x080814);
    ubCam=new THREE.PerspectiveCamera(38,300/200,0.01,100);ubCam.position.set(0,0.05,3.2);ubCam.lookAt(0,0,0);
    ubScene.add(new THREE.AmbientLight(0x506080,1.4));
    const dl=new THREE.DirectionalLight(0xffffff,2.2);dl.position.set(2,3,2);ubScene.add(dl);
    const dl2=new THREE.DirectionalLight(0x4488ff,0.6);dl2.position.set(-2,1,-1);ubScene.add(dl2);
    const pl=new THREE.PointLight(0xff6600,0.7,6);pl.position.set(0.5,0.5,1.5);ubScene.add(pl);
    c2.addEventListener('mousedown',e=>{ubMX=true;ubLX=e.clientX;ubLY=e.clientY;});
    c2.addEventListener('mousemove',e=>{if(!ubMX)return;ubRY+=(e.clientX-ubLX)*0.012;ubRX+=(e.clientY-ubLY)*0.012;ubLX=e.clientX;ubLY=e.clientY;});
    c2.addEventListener('mouseup',()=>ubMX=false);c2.addEventListener('mouseleave',()=>ubMX=false);
    (function a(){requestAnimationFrame(a);if(ubMesh){ubMesh.rotation.y=ubRY+performance.now()*0.0006;ubMesh.rotation.x=ubRX;}if(ubRend)ubRend.render(ubScene,ubCam);})();
  }
  ubScene.children.filter(c=>c.userData.pv).forEach(c=>ubScene.remove(c));
  ubMesh=getMesh(wid,skin.id);if(ubMesh){ubMesh.scale.setScalar(7.5);ubMesh.userData.pv=true;ubScene.add(ubMesh);}
  ubRX=0;ubRY=0;
  const wasScreen=S.screen;
  if(S.screen==='game'){
    S.screen='popup';
    if(document.pointerLockElement) document.exitPointerLock();
  }
  showEl('unlockBanner');
  const resume=()=>{hideEl('unlockBanner');if(wasScreen==='game'){S.screen='game';lockRequested=true;cvs.requestPointerLock();}S.showingUnlock=false;setTimeout(showNextUnlock,200);};
  document.getElementById('ubEquipBtn').onclick=()=>{S.weapons[wid].skin=skin.id;if(S.held===wid)rebuildWeapon();resume();};
  document.getElementById('ubCloseBtn').onclick=resume;
}

// ══ PREVIEW ══
let pvRend=null,pvScene=null,pvCam=null,pvMesh=null,pvMX=false,pvLX=0,pvLY=0,pvRX=0,pvRY=0;
function openPreview(wid,skinId){
  const def=WDEFS[wid],sk=def.skins.find(s=>s.id===skinId);
  document.getElementById('pvTitle').textContent=sk.name.toUpperCase();
  document.getElementById('pvRarity').textContent=def.name+' '+sk.rarity.toUpperCase();
  document.getElementById('pvRarity').style.color=RC[sk.rarity];
  if(!pvRend){
    const c2=document.getElementById('pvCanvas');
    pvRend=new THREE.WebGLRenderer({canvas:c2,antialias:true,alpha:true});
    pvRend.setSize(480,340);pvRend.setPixelRatio(Math.min(devicePixelRatio,2));
    pvScene=new THREE.Scene();pvScene.background=new THREE.Color(0x080814);
    pvCam=new THREE.PerspectiveCamera(36,480/340,0.01,100);pvCam.position.set(0,0.05,3.2);pvCam.lookAt(0,0,0);
    pvScene.add(new THREE.AmbientLight(0x506080,1.4));
    const dl=new THREE.DirectionalLight(0xffffff,2.2);dl.position.set(2,3,2);pvScene.add(dl);
    const dl2=new THREE.DirectionalLight(0x4488ff,0.6);dl2.position.set(-2,1,-1);pvScene.add(dl2);
    const pl=new THREE.PointLight(0xff6600,0.7,6);pl.position.set(0.5,0.5,1.5);pvScene.add(pl);
    c2.addEventListener('mousedown',e=>{pvMX=true;pvLX=e.clientX;pvLY=e.clientY;});
    c2.addEventListener('mousemove',e=>{if(!pvMX)return;pvRY+=(e.clientX-pvLX)*0.012;pvRX+=(e.clientY-pvLY)*0.012;pvLX=e.clientX;pvLY=e.clientY;});
    c2.addEventListener('mouseup',()=>pvMX=false);c2.addEventListener('mouseleave',()=>pvMX=false);
    (function a(){requestAnimationFrame(a);if(pvMesh){pvMesh.rotation.y=pvRY+performance.now()*0.0006;pvMesh.rotation.x=pvRX;}if(pvRend)pvRend.render(pvScene,pvCam);})();
  }
  pvScene.children.filter(c=>c.userData.pv).forEach(c=>pvScene.remove(c));
  pvMesh=getMesh(wid,skinId);if(pvMesh){pvMesh.scale.setScalar(8.5);pvMesh.userData.pv=true;pvScene.add(pvMesh);}
  pvRX=0;pvRY=0;
  showEl('skinOverlay');
  document.getElementById('pvEquipBtn').onclick=()=>{equipSkin(wid,skinId);hideEl('skinOverlay');};
  document.getElementById('pvCloseBtn').onclick=()=>hideEl('skinOverlay');
}

// ══ OVERLAY HELPERS ══
function showEl(id){document.getElementById(id).classList.add('show');}
function hideEl(id){document.getElementById(id).classList.remove('show');}

// ══ GAME FLOW ══
function startGame(){
  S.zombies.forEach(z=>scene.remove(z));S.zombies.length=0;
  S.health=100;S.screen='game';S.wave=0;S.waveActive=false;S.totalKills=0;
  S.held='pistol';S.heldSecondary='pistol';
  // Preserve knife selection but fall back to 'knife' if selected weapon isn't owned
  if(!S.weapons[S.heldKnife]||!S.weapons[S.heldKnife].owned) S.heldKnife='knife';
  // Preserve primary selection but fall back if selected weapon isn't owned
  if(!S.weapons[S.heldPrimary]||!S.weapons[S.heldPrimary].owned){
    const firstPrimary=Object.keys(WDEFS).find(id=>WDEFS[id].slot==='primary'&&S.weapons[id]&&S.weapons[id].owned);
    if(firstPrimary) S.heldPrimary=firstPrimary;
  }
  camera.position.set(0,1.75,18);camera.rotation.order='YXZ';camera.rotation.set(0,0,0);
  yaw=0;pitch=0;hbob=0;lastT=performance.now();
  velY=0;onGround=true;stamina=STAM_MAX;stamDepleted=false;reloading=false;reloadTimer=0;
  boltAnimating=false; scoped=false;
  document.getElementById('scopeOverlay').style.display='none';
  document.querySelector('.crosshair').style.display='block';
  // Reset per-weapon timers
  Object.keys(S.weapons).forEach(wid=>{ S.weapons[wid].lastFire=0; if(S.weapons[wid].cd!==undefined) S.weapons[wid].cd=0; });
  S.weapons.pistol.ammo=30;S.weapons.pistol.res=90;
  Object.keys(S.weapons).forEach(wid=>{if(wid!=='pistol'&&wid!=='knife'&&S.weapons[wid].owned){S.weapons[wid].ammo=WDEFS[wid].startAmmo;S.weapons[wid].res=WDEFS[wid].resAmmo;}});
  hideEl('menuScreen');
  document.getElementById('hud').style.display='block';
  updateHUD();rebuildWeapon();
  lockRequested=true;cvs.requestPointerLock();
  setTimeout(startWave,1500);
}
function pause(){if(S.screen!=='game')return;exitScope();S.screen='paused';showEl('pauseMenu');}
function resume(){if(S.screen!=='paused')return;hideEl('pauseMenu');S.screen='game';weaponGroup.visible=true;lockRequested=true;cvs.requestPointerLock();}
function gameOver(){
  exitScope();
  S.screen='gameover';if(document.pointerLockElement)document.exitPointerLock();
  document.getElementById('hud').style.display='none';
  document.getElementById('goStats').innerHTML='Total Kills: '+S.totalKills+'<br>Credits: '+S.credits+'<br>Wave: '+S.wave;
  showEl('gameOverScreen');S.zombies.forEach(z=>scene.remove(z));S.zombies.length=0;
}
function backToMenu(){
  exitScope();
  S.zombies.forEach(z=>scene.remove(z));S.zombies.length=0;
  S.screen='menu';['pauseMenu','gameOverScreen'].forEach(hideEl);
  document.getElementById('hud').style.display='none';
  showEl('menuScreen');document.getElementById('menuCredits').textContent=S.credits;
  if(document.pointerLockElement)document.exitPointerLock();
}

// ══ SHOP ══
function renderShop(){
  const wg=document.getElementById('shopWeapons');wg.innerHTML='';
  // Purchasable weapons (have a price)
  const icons={smg:'MP5',shotgun:'SHT',revolver:'REV',deagle:'DEG',compact:'MAC',sledge:'SLG'};
  Object.keys(WDEFS).forEach(wid=>{
    const def=WDEFS[wid],ws=S.weapons[wid];
    if(!def.price)return;
    const c=document.createElement('div');c.className='shop-card';
    const slotTag=`<span style="font-size:9px;background:rgba(255,100,0,.15);border:1px solid rgba(255,100,0,.3);border-radius:2px;padding:1px 5px;color:#ff8040;letter-spacing:1px;font-family:'Space Mono',monospace">${def.slot.toUpperCase()}</span>`;
    c.innerHTML=`<div class="icon">${icons[wid]||'GUN'}</div><h3>${def.name}</h3><div style="margin-bottom:6px">${slotTag}</div><div class="desc">${def.hitsToKill} hits/kill · ${def.fireMs<200?'Full-auto':def.fireMs>900?'Pump':'Semi'}</div><div class="price">CREDITS: ${def.price}</div>${ws.owned?'<div class="owned-badge">OWNED</div>':`<button class="btn-buy" ${S.credits>=def.price?'':'disabled'} onclick="buyWeapon('${wid}')">BUY</button>`}`;
    wg.appendChild(c);
  });
  const sg=document.getElementById('shopSupplies');sg.innerHTML='';
  Object.keys(WDEFS).forEach(wid=>{
    const def=WDEFS[wid],ws=S.weapons[wid];
    if(!ws||!ws.owned||def.type==='melee')return;
    const c=document.createElement('div');c.className='shop-card';
    c.innerHTML=`<div class="icon">AMO</div><h3>${def.name} Ammo</h3><div class="desc">+${def.boxAmt} rounds</div><div class="price">CREDITS: ${def.boxCost}</div><button class="btn-buy" ${S.credits>=def.boxCost?'':'disabled'} onclick="buyAmmo('${wid}')">BUY</button>`;
    sg.appendChild(c);
  });
  const hc=document.createElement('div');hc.className='shop-card';
  hc.innerHTML=`<div class="icon">MED</div><h3>Heal Kit</h3><div class="desc">+${HEAL_AMT} HP (H key)</div><div class="price">CREDITS: ${HEAL_COST}</div><button class="btn-buy" ${S.credits>=HEAL_COST?'':'disabled'} onclick="buyKit()">BUY</button>`;
  sg.appendChild(hc);
}
window.buyWeapon=wid=>{
  const def=WDEFS[wid];if(S.credits<def.price)return;
  S.credits-=def.price;S.weapons[wid].owned=true;S.weapons[wid].ammo=def.startAmmo;S.weapons[wid].res=def.resAmmo;
  // Auto-set as active for slot
  if(def.slot==='primary')   S.heldPrimary=wid;
  if(def.slot==='secondary') S.heldSecondary=wid;
  renderShop();document.getElementById('menuCredits').textContent=S.credits;notify(def.name+' purchased & set active!');
};
window.buyAmmo=wid=>{const def=WDEFS[wid];if(S.credits<def.boxCost)return;S.credits-=def.boxCost;S.weapons[wid].res+=def.boxAmt;renderShop();document.getElementById('menuCredits').textContent=S.credits;notify('+'+def.boxAmt+' ammo');};
window.buyKit=()=>{if(S.credits<HEAL_COST)return;S.credits-=HEAL_COST;S.kits++;renderShop();document.getElementById('menuCredits').textContent=S.credits;notify('Heal kit added ('+S.kits+' total)');};

// ══ CODES ══
document.getElementById('btnRedeem').onclick=redeemCode;
document.getElementById('codeInput').onkeydown=e=>{if(e.key==='Enter')redeemCode();};
function redeemCode(){
  const v=document.getElementById('codeInput').value.trim(),msg=document.getElementById('codeMsg');
  if(!v)return;
  if(S.usedCodes.includes(v)){msg.innerHTML='<div class="code-msg-err">Already used.</div>';return;}
  const code=CODES[v];
  if(!code){msg.innerHTML='<div class="code-msg-err">Invalid code.</div>';return;}
  S.usedCodes.push(v);document.getElementById('codeInput').value='';
  if(code.type==='credits')S.credits+=code.amount;
  else if(code.type==='devmode'){Object.keys(S.weapons).forEach(wid=>{if(!WDEFS[wid])return;S.weapons[wid].owned=true;if(WDEFS[wid].type==='gun'){S.weapons[wid].ammo=WDEFS[wid].startAmmo;S.weapons[wid].res=WDEFS[wid].resAmmo*3;}WDEFS[wid].skins.forEach(sk=>{if(!S.unlocked[wid].includes(sk.id))S.unlocked[wid].push(sk.id);});});}
  msg.innerHTML='<div class="code-msg-ok">'+code.msg+'</div>';
  document.getElementById('menuCredits').textContent=S.credits;renderShop();renderInventory();
}

// ══ INVENTORY ══
function renderInventory(){
  const slot=S.invSlot||'primary';
  const container=document.getElementById('invWeaponList');
  if(!container) return;
  container.innerHTML='';
  const wids=Object.keys(WDEFS).filter(id=>WDEFS[id].slot===slot&&S.weapons[id]&&S.weapons[id].owned);
  if(!wids.length){
    container.innerHTML='<div style="color:#444;font-family:\'Space Mono\',monospace;font-size:11px;padding:28px 0">No '+slot+' weapons owned yet.<br>Visit the SHOP to buy some.</div>';
    return;
  }
  wids.forEach((wid,wi)=>{
    const def=WDEFS[wid],ws=S.weapons[wid];
    const isActiveSlot=(slot==='primary'&&S.heldPrimary===wid)||(slot==='secondary'&&S.heldSecondary===wid)||(slot==='knife'&&S.heldKnife===wid);
    // Weapon header row
    const hdrRow=document.createElement('div');
    hdrRow.style.cssText='display:flex;align-items:center;gap:10px;margin:'+(wi>0?'22px':'4px')+' 0 10px';
    hdrRow.innerHTML=
      '<span style="font-family:\'Bebas Neue\',sans-serif;font-size:20px;letter-spacing:3px;color:'+(isActiveSlot?'#ff8040':'#555')+'">'+def.name+'</span>'
      +(isActiveSlot
        ? '<span style="font-size:9px;background:rgba(255,100,0,.18);border:1px solid rgba(255,100,0,.4);border-radius:2px;padding:2px 7px;color:#ff8040;font-family:\'Space Mono\',monospace;letter-spacing:1px">ACTIVE</span>'
        : '<button class="btn-equip-sm" style="font-size:9px;padding:3px 10px" onclick="setActiveWeapon(\''+wid+'\')">SET ACTIVE</button>'
      );
    container.appendChild(hdrRow);
    // Progress bar toward next skin
    const next=def.skins.find(s=>!S.unlocked[wid].includes(s.id));
    if(next){
      const prev=def.skins.reduce((mx,s)=>S.unlocked[wid].includes(s.id)?Math.max(mx,s.kills):mx,0);
      const pct=prev===next.kills?0:Math.min(1,(ws.kills-prev)/(next.kills-prev));
      const d=document.createElement('div');d.className='prog-box';
      d.innerHTML='<div class="prog-label">Next: <b style="color:'+RC[next.rarity]+'">'+next.name+'</b></div>'
        +'<div class="prog-bar-bg"><div class="prog-bar-fill" style="width:'+Math.round(pct*100)+'%"></div></div>'
        +'<div class="prog-kills">'+ws.kills+' / '+next.kills+' kills ('+Math.round(pct*100)+'%)</div>';
      container.appendChild(d);
    }
    // Skin list
    def.skins.forEach(skin=>{
      const unlocked=S.unlocked[wid].includes(skin.id),equipped=ws.skin===skin.id;
      const row=document.createElement('div');
      row.className='skin-row'+(equipped?' sk-equipped':'')+(unlocked?'':' sk-locked');
      const h1='#'+skin.colors[0].toString(16).padStart(6,'0');
      const h2='#'+skin.colors[1].toString(16).padStart(6,'0');
      row.innerHTML='<div class="skin-thumb" style="background:linear-gradient(135deg,'+h1+','+h2+')">'+(unlocked?skin.emoji:'?')+'</div>'
        +'<div class="skin-info"><div class="srarity" style="color:'+RC[skin.rarity]+'">'+skin.rarity+'</div>'
        +'<div class="sname" style="color:'+RC[skin.rarity]+'">'+skin.name+'</div>'
        +'<div class="sstatus">'+(equipped?'✅ EQUIPPED':unlocked?'UNLOCKED':'🔒 '+skin.kills+' kills')+'</div></div>'
        +(unlocked?'<div class="skin-actions"><button class="btn-view-sk" onclick="openPreview(\''+wid+'\',\''+skin.id+'\')">VIEW</button>'+(equipped?'':'<button class="btn-equip-sm" onclick="equipSkin(\''+wid+'\',\''+skin.id+'\')">EQUIP</button>')+'</div>':'');
      container.appendChild(row);
    });
    if(wi<wids.length-1){
      const hr=document.createElement('hr');hr.style.cssText='border:none;border-top:1px solid rgba(255,255,255,.05);margin:14px 0';container.appendChild(hr);
    }
  });
}
function renderSkins(){renderInventory();}
function equipSkin(wid,skinId){
  S.weapons[wid].skin=skinId;if(S.held===wid)rebuildWeapon();renderInventory();
  notify('Equipped: '+WDEFS[wid].skins.find(s=>s.id===skinId).name);
}
function setActiveWeapon(wid){
  const slot=WDEFS[wid].slot;
  if(slot==='primary')   S.heldPrimary=wid;
  if(slot==='secondary') S.heldSecondary=wid;
  if(slot==='knife')     S.heldKnife=wid;
  notify(WDEFS[wid].name+' set as active '+slot);
  renderInventory();
}
window.openPreview=openPreview;window.equipSkin=equipSkin;window.setActiveWeapon=setActiveWeapon;

// ══ MENU NAV ══
document.querySelectorAll('.nav-tab').forEach(tab=>{
  tab.onclick=()=>{
    document.querySelectorAll('.nav-tab').forEach(t=>t.classList.remove('active'));
    document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
    tab.classList.add('active');
    const tid='tab'+tab.dataset.tab.charAt(0).toUpperCase()+tab.dataset.tab.slice(1);
    document.getElementById(tid).classList.add('active');
    if(tab.dataset.tab==='shop')renderShop();
    if(tab.dataset.tab==='inventory')renderInventory();
    if(tab.dataset.tab==='settings'){
      // Sync sliders to current settings
      document.getElementById('settingFov').value=SETTINGS.fov;
      document.getElementById('valFov').textContent=SETTINGS.fov+'°';
      document.getElementById('settingSens').value=Math.round(SETTINGS.sensitivity*9);
      document.getElementById('valSens').textContent=SETTINGS.sensitivity.toFixed(1)+'×';
      document.getElementById('settingBright').value=SETTINGS.brightness;
      document.getElementById('valBright').textContent=SETTINGS.brightness+'%';
    }
    document.getElementById('menuCredits').textContent=S.credits;
  };
});
// Inventory sub-tab clicks
document.querySelectorAll('.inv-subtab').forEach(btn=>{
  btn.onclick=()=>{
    document.querySelectorAll('.inv-subtab').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    S.invSlot=btn.dataset.islot;
    renderInventory();
  };
});
document.getElementById('btnPlay').onclick=startGame;
document.getElementById('btnResume').onclick=resume;
document.getElementById('btnQuit').onclick=backToMenu;
document.getElementById('btnGoMenu').onclick=backToMenu;

// ══ NOTIFY ══
let nT;
function notify(m){const el=document.getElementById('notif');el.textContent=m;el.classList.add('show');clearTimeout(nT);nT=setTimeout(()=>el.classList.remove('show'),2100);}

// ══ RESIZE ══
window.addEventListener('resize',()=>{renderer.setSize(innerWidth,innerHeight);camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();});

// ══ AUDIO ══
let audioCtx=null;
function getAudio(){
  if(!audioCtx) audioCtx=new(window.AudioContext||window.webkitAudioContext)();
  if(audioCtx.state==='suspended') audioCtx.resume();
  return audioCtx;
}
function soundPistol(){
  const ctx=getAudio(),now=ctx.currentTime;
  // Main bang - noise burst
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.18,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,2.5);
  const src=ctx.createBufferSource(); src.buffer=buf;
  const gain=ctx.createGain(); gain.gain.setValueAtTime(1.4,now); gain.gain.exponentialRampToValueAtTime(0.001,now+0.18);
  const lp=ctx.createBiquadFilter(); lp.type='lowpass'; lp.frequency.setValueAtTime(3200,now); lp.frequency.exponentialRampToValueAtTime(400,now+0.12);
  src.connect(lp); lp.connect(gain); gain.connect(ctx.destination); src.start(now);
  // Punch tone
  const osc=ctx.createOscillator(); osc.type='sine'; osc.frequency.setValueAtTime(180,now); osc.frequency.exponentialRampToValueAtTime(55,now+0.08);
  const og=ctx.createGain(); og.gain.setValueAtTime(0.6,now); og.gain.exponentialRampToValueAtTime(0.001,now+0.1);
  osc.connect(og); og.connect(ctx.destination); osc.start(now); osc.stop(now+0.1);
}
function soundSMG(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.1,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,3);
  const src=ctx.createBufferSource(); src.buffer=buf;
  const gain=ctx.createGain(); gain.gain.setValueAtTime(1.0,now); gain.gain.exponentialRampToValueAtTime(0.001,now+0.1);
  const hp=ctx.createBiquadFilter(); hp.type='highpass'; hp.frequency.value=900;
  const lp=ctx.createBiquadFilter(); lp.type='lowpass'; lp.frequency.setValueAtTime(4000,now);
  src.connect(hp); hp.connect(lp); lp.connect(gain); gain.connect(ctx.destination); src.start(now);
  const osc=ctx.createOscillator(); osc.frequency.setValueAtTime(140,now); osc.frequency.exponentialRampToValueAtTime(60,now+0.06);
  const og=ctx.createGain(); og.gain.setValueAtTime(0.4,now); og.gain.exponentialRampToValueAtTime(0.001,now+0.07);
  osc.connect(og); og.connect(ctx.destination); osc.start(now); osc.stop(now+0.07);
}
function soundShotgun(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.38,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,1.4);
  const src=ctx.createBufferSource(); src.buffer=buf;
  const gain=ctx.createGain(); gain.gain.setValueAtTime(2.2,now); gain.gain.exponentialRampToValueAtTime(0.001,now+0.38);
  const lp=ctx.createBiquadFilter(); lp.type='lowpass'; lp.frequency.setValueAtTime(2200,now); lp.frequency.exponentialRampToValueAtTime(200,now+0.25);
  src.connect(lp); lp.connect(gain); gain.connect(ctx.destination); src.start(now);
  const osc=ctx.createOscillator(); osc.type='sine'; osc.frequency.setValueAtTime(95,now); osc.frequency.exponentialRampToValueAtTime(30,now+0.15);
  const og=ctx.createGain(); og.gain.setValueAtTime(1.0,now); og.gain.exponentialRampToValueAtTime(0.001,now+0.18);
  osc.connect(og); og.connect(ctx.destination); osc.start(now); osc.stop(now+0.18);
}
function soundKnife(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.14,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++){ const t=i/d.length; d[i]=(Math.random()*2-1)*Math.sin(t*Math.PI)*0.6; }
  const src=ctx.createBufferSource(); src.buffer=buf;
  const gain=ctx.createGain(); gain.gain.setValueAtTime(0.5,now); gain.gain.exponentialRampToValueAtTime(0.001,now+0.14);
  const hp=ctx.createBiquadFilter(); hp.type='highpass'; hp.frequency.value=2400;
  src.connect(hp); hp.connect(gain); gain.connect(ctx.destination); src.start(now);
}
function soundEmpty(){
  const ctx=getAudio(),now=ctx.currentTime;
  const osc=ctx.createOscillator(); osc.type='square'; osc.frequency.value=220;
  const g=ctx.createGain(); g.gain.setValueAtTime(0.15,now); g.gain.exponentialRampToValueAtTime(0.001,now+0.06);
  osc.connect(g); g.connect(ctx.destination); osc.start(now); osc.stop(now+0.06);
}
function soundRevolver(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.32,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,1.8)*0.9;
  const src=ctx.createBufferSource();src.buffer=buf;
  const gain=ctx.createGain();gain.gain.setValueAtTime(1.8,now);gain.gain.exponentialRampToValueAtTime(0.001,now+0.32);
  const lp=ctx.createBiquadFilter();lp.type='lowpass';lp.frequency.setValueAtTime(2400,now);lp.frequency.exponentialRampToValueAtTime(300,now+0.22);
  src.connect(lp);lp.connect(gain);gain.connect(ctx.destination);src.start(now);
  const osc=ctx.createOscillator();osc.type='sine';osc.frequency.setValueAtTime(140,now);osc.frequency.exponentialRampToValueAtTime(45,now+0.12);
  const og=ctx.createGain();og.gain.setValueAtTime(0.9,now);og.gain.exponentialRampToValueAtTime(0.001,now+0.14);
  osc.connect(og);og.connect(ctx.destination);osc.start(now);osc.stop(now+0.14);
  // Cylinder click
  const oc2=ctx.createOscillator();oc2.type='square';oc2.frequency.value=800;
  const og2=ctx.createGain();og2.gain.setValueAtTime(0.1,now+0.05);og2.gain.exponentialRampToValueAtTime(0.001,now+0.08);
  oc2.connect(og2);og2.connect(ctx.destination);oc2.start(now+0.05);oc2.stop(now+0.08);
}
function soundDeagle(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.25,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,2.2);
  const src=ctx.createBufferSource();src.buffer=buf;
  const gain=ctx.createGain();gain.gain.setValueAtTime(1.9,now);gain.gain.exponentialRampToValueAtTime(0.001,now+0.25);
  const lp=ctx.createBiquadFilter();lp.type='lowpass';lp.frequency.setValueAtTime(2800,now);lp.frequency.exponentialRampToValueAtTime(350,now+0.18);
  src.connect(lp);lp.connect(gain);gain.connect(ctx.destination);src.start(now);
  const osc=ctx.createOscillator();osc.type='sine';osc.frequency.setValueAtTime(160,now);osc.frequency.exponentialRampToValueAtTime(50,now+0.1);
  const og=ctx.createGain();og.gain.setValueAtTime(0.8,now);og.gain.exponentialRampToValueAtTime(0.001,now+0.12);
  osc.connect(og);og.connect(ctx.destination);osc.start(now);osc.stop(now+0.12);
}
function soundCompact(){
  const ctx=getAudio(),now=ctx.currentTime;
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.07,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.pow(1-i/d.length,4)*0.8;
  const src=ctx.createBufferSource();src.buffer=buf;
  const gain=ctx.createGain();gain.gain.setValueAtTime(0.8,now);gain.gain.exponentialRampToValueAtTime(0.001,now+0.07);
  const hp=ctx.createBiquadFilter();hp.type='highpass';hp.frequency.value=1200;
  const lp=ctx.createBiquadFilter();lp.type='lowpass';lp.frequency.value=5000;
  src.connect(hp);hp.connect(lp);lp.connect(gain);gain.connect(ctx.destination);src.start(now);
}
function soundSniper(){
  const ctx=getAudio(),now=ctx.currentTime;
  // Deep, massive low-end crack
  const buf=ctx.createBuffer(1,ctx.sampleRate*0.55,ctx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++){
    const env=Math.pow(1-i/d.length,1.8);
    d[i]=(Math.random()*2-1)*env*0.9;
  }
  const src=ctx.createBufferSource(); src.buffer=buf;
  const gain=ctx.createGain(); gain.gain.setValueAtTime(2.2,now); gain.gain.exponentialRampToValueAtTime(0.001,now+0.55);
  const lp=ctx.createBiquadFilter(); lp.type='lowpass'; lp.frequency.setValueAtTime(5500,now); lp.frequency.exponentialRampToValueAtTime(200,now+0.35);
  src.connect(lp); lp.connect(gain); gain.connect(ctx.destination); src.start(now);
  // Deep sub boom
  const osc=ctx.createOscillator(); osc.type='sine'; osc.frequency.setValueAtTime(90,now); osc.frequency.exponentialRampToValueAtTime(22,now+0.3);
  const og=ctx.createGain(); og.gain.setValueAtTime(1.2,now); og.gain.exponentialRampToValueAtTime(0.001,now+0.35);
  osc.connect(og); og.connect(ctx.destination); osc.start(now); osc.stop(now+0.35);
  // High crack transient
  const buf2=ctx.createBuffer(1,ctx.sampleRate*0.06,ctx.sampleRate);
  const d2=buf2.getChannelData(0);
  for(let i=0;i<d2.length;i++) d2[i]=(Math.random()*2-1)*Math.pow(1-i/d2.length,3)*1.4;
  const src2=ctx.createBufferSource(); src2.buffer=buf2;
  const hp=ctx.createBiquadFilter(); hp.type='highpass'; hp.frequency.setValueAtTime(4000,now);
  const g2=ctx.createGain(); g2.gain.setValueAtTime(1.5,now); g2.gain.exponentialRampToValueAtTime(0.001,now+0.06);
  src2.connect(hp); hp.connect(g2); g2.connect(ctx.destination); src2.start(now);
}
function playGunSound(wid){
  if(wid==='pistol')soundPistol();
  else if(wid==='smg')soundSMG();
  else if(wid==='shotgun')soundShotgun();
  else if(wid==='sniper')soundSniper();
  else if(wid==='revolver')soundRevolver();
  else if(wid==='deagle')soundDeagle();
  else if(wid==='compact')soundCompact();
  else soundPistol();
}
buildMap();
initDecals();
rebuildWeapon();
requestAnimationFrame(loop);
</script>
</body>
</html>
