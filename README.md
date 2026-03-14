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
(function(){
'use strict';

// ══ DATA ══
const RC={common:'#aaaaaa',rare:'#5588ff',epic:'#cc44ff',legendary:'#ff8800'};
const WDEFS={
  // ── PRIMARIES (slot 1) ──
  smg:{name:'MP5',slot:'primary',type:'gun',price:3000,hitsToKill:10,fireMs:110,startAmmo:50,resAmmo:150,boxCost:60,boxAmt:50,skins:[
    {id:'default',name:'Default',      kills:0,  colors:[0x666666,0x333333],rarity:'common',   emoji:'?'},
    {id:'golden', name:'Gold Plated',  kills:10, colors:[0xFFD700,0xDAA520],rarity:'rare',     emoji:'G'},
    {id:'cobalt', name:'Cobalt Blue',   kills:50, colors:[0x4169E1,0x000088],rarity:'epic',     emoji:'C'},
    {id:'atomic', name:'Void Reactor',  kills:100,colors:[0x00ffee,0x000820],rarity:'legendary',emoji:'V'},
  ]},
  shotgun:{name:'Shotgun',slot:'primary',type:'gun',price:2500,hitsToKill:1,fireMs:1100,startAmmo:8,resAmmo:24,boxCost:80,boxAmt:8,skins:[
    {id:'default',name:'Default',      kills:0,  colors:[0x8B4513,0x555555],rarity:'common',   emoji:'?'},
    {id:'tiger',  name:'Tiger Tooth',  kills:10, colors:[0xFF8C00,0x111111],rarity:'epic',     emoji:'T'},
    {id:'fade',   name:'Inferno',       kills:50, colors:[0xFF2200,0xFF8800],rarity:'legendary',emoji:'I'},
  ]},
  // ── SECONDARIES (slot 2) ──
  pistol:{name:'Pistol',slot:'secondary',type:'gun',hitsToKill:3,fireMs:480,startAmmo:30,resAmmo:90,boxCost:50,boxAmt:30,skins:[
    {id:'default',name:'Default',       kills:0,  colors:[0x888888,0x333333],rarity:'common',   emoji:'?'},
    {id:'golden', name:'Golden Eagle',  kills:7,  colors:[0xFFD700,0xAA7700],rarity:'rare',     emoji:'G'},
    {id:'crimson',name:'Crimson Web',   kills:14, colors:[0xCC2222,0x660000],rarity:'epic',     emoji:'C'},
    {id:'fade',   name:'Aurora Blight', kills:21, colors:[0x00FF88,0x9400D3],rarity:'legendary',emoji:'A'},
    {id:'neon',   name:'Neon Rider',    kills:28, colors:[0x00FFCC,0xFF00AA],rarity:'legendary',emoji:'N'},
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
    {id:'oxide',  name:'Prismatic Storm', kills:21, colors:[0x00AAFF,0xFF00AA],rarity:'legendary',emoji:'P'},
  ]},
  compact:{name:'MAC-10',slot:'secondary',type:'gun',price:600,hitsToKill:6,fireMs:80,startAmmo:30,resAmmo:90,boxCost:45,boxAmt:30,skins:[
    {id:'default',name:'Default',       kills:0,  colors:[0x444444,0x222222],rarity:'common',emoji:'?'},
    {id:'rust',   name:'Rust Coat',     kills:7,  colors:[0x8B3A0F,0x5a1a00],rarity:'rare',  emoji:'R'},
    {id:'hotrod', name:'Dark Vortex',   kills:14, colors:[0x110022,0x8800FF],rarity:'legendary',emoji:'D'},
  ]},
  // ── KNIFE (slot 3) ── first:5, then +5 each
  knife:{name:'Knife',slot:'knife',type:'melee',hitsToKill:2,cooldown:1500,skins:[
    {id:'default',   name:'Standard',          kills:0,  model:'std',       colors:[0xCCCCCC,0x777777],rarity:'common',   emoji:'K'},
    {id:'bayonet',   name:'Bayonet | Forest',   kills:5,  model:'bayonet',   colors:[0x3A7A22,0x1A4A10],rarity:'rare',     emoji:'B'},
    {id:'butterfly', name:'Butterfly | Fade',   kills:10, model:'butterfly', colors:[0xFF69B4,0x8800CC],rarity:'epic',     emoji:'F'},
    {id:'shadow',    name:'Crimson Phantom',    kills:15, model:'shadow',    colors:[0x330000,0xFF0033],rarity:'legendary',emoji:'X'},
  ]},
  sledge:{name:'Sledgehammer',slot:'knife',type:'melee',price:1200,hitsToKill:1,cooldown:5000,skins:[
    {id:'default',name:'Default',       kills:0,  model:'sledge',      colors:[0x555555,0x8B4513],rarity:'common',   emoji:'H'},
    {id:'rusted', name:'Rusty Crusher', kills:5,  model:'sledge',      colors:[0x8B3A0F,0x444444],rarity:'rare',     emoji:'R'},
    {id:'gold',   name:'Gold Rush',     kills:10, model:'sledge',      colors:[0xFFD700,0xAA5500],rarity:'epic',     emoji:'G'},
    {id:'void',   name:'Void Smash',    kills:15, model:'sledge_void', colors:[0x110022,0x8800FF],rarity:'legendary',emoji:'V'},
  ]},
};
const WAVES=[5,10,22,45,90];
const HEAL_COST=75,HEAL_AMT=50,SKIN_BONUS=50;
const CODES={'free1000':{type:'credits',amount:1000,msg:'+1000 CREDITS!'},'Dev':{type:'devmode',msg:'ALL UNLOCKED!'}};

// ══ STATE ══
const S={
  screen:'menu',credits:0,health:100,kits:0,
  weapons:{
    smg:    {owned:false,ammo:0,  res:0,   kills:0,skin:'default'},
    shotgun:{owned:false,ammo:0,  res:0,   kills:0,skin:'default'},
    pistol: {owned:true, ammo:30, res:90,  kills:0,skin:'default'},
    revolver:{owned:false,ammo:0, res:0,   kills:0,skin:'default'},
    deagle: {owned:false,ammo:0,  res:0,   kills:0,skin:'default'},
    compact:{owned:false,ammo:0,  res:0,   kills:0,skin:'default'},
    knife:  {owned:true,          kills:0,skin:'default'},
    sledge: {owned:false,         kills:0,skin:'default'},
  },
  held:'pistol',
  heldPrimary:'smg', heldSecondary:'pistol', heldKnife:'knife',
  usedCodes:[],
  unlocked:{smg:['default'],shotgun:['default'],pistol:['default'],revolver:['default'],deagle:['default'],compact:['default'],knife:['default'],sledge:['default']},
  zombies:[],totalKills:0,
  wave:0,waveActive:false,waveKilled:0,waveSize:0,spawnQueue:0,spawnTimer:0,
  invSlot:'primary',
  unlockQueue:[],showingUnlock:false,
  lastFire:0,autoFire:null,
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
  if(col){const p=0.3;COL.push({type:'box',minX:x-w/2-p,maxX:x+w/2+p,minZ:z-d/2-p,maxZ:z+d/2+p});}
  return m;
}
function addCyl(r,h,color,x,y,z){
  const m=new THREE.Mesh(new THREE.CylinderGeometry(r,r,h,8),new THREE.MeshLambertMaterial({color}));
  m.position.set(x,y,z);m.castShadow=true;m.receiveShadow=true;scene.add(m);
  COL.push({type:'cyl',x,z,r:r+0.3});
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
      if(axis==='x') COL.push({type:'box',minX:off-sl/2-p,maxX:off+sl/2+p,minZ:cval-WT/2-p,maxZ:cval+WT/2+p});
      else            COL.push({type:'box',minX:cval-WT/2-p,maxX:cval+WT/2+p,minZ:off-sl/2-p,maxZ:off+sl/2+p});
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
    if(i%2===0) addBox(1.2,1.2,1.2,CRATE,x,0.6,z,0,true);
    else        addCyl(0.45,1.3,BARREL,x,0.65,z);
  });
}

// ══ WEAPON MODELS ══
const mm=c=>new THREE.MeshLambertMaterial({color:c});
function mkPistol(c1,c2){
  const g=new THREE.Group();
  // Slide (top receiver)
  const sl=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.095,0.36),mm(c1));sl.position.set(0,0.055,0);g.add(sl);
  // Ejection port cut-in (darker inset)
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.076,0.04,0.1),new THREE.MeshLambertMaterial({color:0x111111}));ep.position.set(0,0.055,-0.04);g.add(ep);
  // Frame / lower receiver
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.068,0.048,0.26),mm(c2));fr.position.set(0,0.0,0.03);g.add(fr);
  // Grip – angled
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.21,0.13),mm(c2));gr.position.set(0,-0.105,0.1);gr.rotation.x=0.14;g.add(gr);
  // Grip texture ridges
  for(let i=0;i<4;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.008,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.06-i*0.035,0.14-i*0.007);g.add(r);}
  // Barrel
  const br=new THREE.Mesh(new THREE.CylinderGeometry(0.016,0.016,0.16,8),mm(c1));br.rotation.x=Math.PI/2;br.position.set(0,0.048,-0.26);g.add(br);
  // Muzzle slightly wider
  const mz=new THREE.Mesh(new THREE.CylinderGeometry(0.02,0.016,0.025,8),mm(c1));mz.rotation.x=Math.PI/2;mz.position.set(0,0.048,-0.345);g.add(mz);
  // Front sight
  const fs=new THREE.Mesh(new THREE.BoxGeometry(0.008,0.018,0.008),mm(c2));fs.position.set(0,0.104,-0.17);g.add(fs);
  // Rear sight
  const rs=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.016,0.008),mm(c2));rs.position.set(0,0.104,0.14);g.add(rs);
  // Trigger guard
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.032,0.007,4,12,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.024,0.04);g.add(tg);
  // Trigger
  const tr=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.028,0.01),mm(c2));tr.position.set(0,-0.02,0.01);tr.rotation.x=0.3;g.add(tr);
  // Hammer
  const hm=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.02,0.012),mm(c2));hm.position.set(0,0.06,0.16);g.add(hm);
  // Magazine base
  const mb=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.012,0.11),mm(c2));mb.position.set(0,-0.21,0.1);g.add(mb);
  return g;
}

function mkKnife(model,c1,c2){
  const g=new THREE.Group();

  if(model==='bayonet'){
    // === BAYONET – long double-edged military blade with crossguard and lug ===
    const steelM=mm(c1), handleM=mm(c2), guardM=new THREE.MeshLambertMaterial({color:0x888888});
    // Long flat blade
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.024,0.006,0.38),steelM);bld.position.set(0,0.012,-0.07);g.add(bld);
    // Blade bevel (lighter face)
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.020,0.002,0.34),new THREE.MeshLambertMaterial({color:0xdddddd}));bev.position.set(0,0.008,-0.06);g.add(bev);
    // Blood groove (fuller) running down center
    const full=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.003,0.28),new THREE.MeshLambertMaterial({color:0x555555}));full.position.set(0,0.014,-0.06);g.add(full);
    // Spear tip (double-taper)
    const tip1=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.006,0.06),steelM);tip1.position.set(0,0.012,-0.28);tip1.rotation.x=0.25;g.add(tip1);
    const tip2=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.006,0.06),steelM);tip2.position.set(0,0.012,-0.28);tip2.rotation.x=-0.25;g.add(tip2);
    // Ricasso (unsharpened section at base of blade)
    const ric=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.010,0.05),steelM);ric.position.set(0,0.012,0.07);g.add(ric);
    // Crossguard — the T-shaped quillon
    const guard=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.016,0.024),guardM);guard.position.set(0,0.010,0.1);g.add(guard);
    const guardV=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.036,0.024),guardM);guardV.position.set(0,0.022,0.1);g.add(guardV);
    // Muzzle ring lug (for attaching to rifle barrel)
    const lug=new THREE.Mesh(new THREE.TorusGeometry(0.022,0.007,6,14),guardM);lug.rotation.x=Math.PI/2;lug.position.set(-0.038,0.010,0.1);g.add(lug);
    // Handle — wooden grip with visible grain lines
    const hnd=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.024,0.2),handleM);hnd.position.set(0,0,0.21);g.add(hnd);
    for(let i=0;i<5;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.030,0.005,0.014),new THREE.MeshLambertMaterial({color:0x3a1a00}));r.position.set(0,0.015,0.12+i*0.038);g.add(r);}
    // Pommel cap
    const pom=new THREE.Mesh(new THREE.BoxGeometry(0.030,0.026,0.022),guardM);pom.position.set(0,0,0.315);g.add(pom);

  } else if(model==='butterfly'){
    // === BALISONG / BUTTERFLY KNIFE ===
    // Long thin clip-point blade
    const bldMat=mm(c1), hndMat=mm(c2);
    // Main blade body
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.004,0.32),bldMat);bld.position.set(0,0.003,-0.1);g.add(bld);
    // Clip-point taper
    const clip=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.004,0.07),bldMat);
    clip.position.set(0,0.003,-0.27);clip.rotation.x=0.22;g.add(clip);
    // Blade flat bevel
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.002,0.28),new THREE.MeshLambertMaterial({color:0xdddddd}));
    bev.position.set(0,0.002,-0.09);g.add(bev);
    // Ricasso (unsharpened heel)
    const ric=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.009,0.04),bldMat);ric.position.set(0,0.004,0.06);g.add(ric);

    // Two handles (balisong style – open, slightly fanned)
    // Each handle: long flat bar with simulated holes via cross-pieces
    const makeHandle=(side)=>{
      const hg=new THREE.Group();
      const sign=side===1?1:-1;
      // Main handle bar
      const bar=new THREE.Mesh(new THREE.BoxGeometry(0.015,0.048,0.26),hndMat);g.add(bar);
      // Skeleton cutouts (simulate holes with dark boxes slightly inset)
      for(let i=0;i<5;i++){
        const hole=new THREE.Mesh(new THREE.CylinderGeometry(0.013,0.013,0.018,10),new THREE.MeshLambertMaterial({color:0x080808}));
        hole.rotation.z=Math.PI/2;hole.position.set(0,0,-0.07+i*0.048);bar.add(hole);
      }
      bar.position.set(sign*0.025, 0, 0.12);
      bar.rotation.z=sign*0.15;
      bar.userData.bfH=side;
      g.add(bar);
      // Spine edge (slightly different color)
      const spine=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.052,0.26),new THREE.MeshLambertMaterial({color:0xbbbbbb}));
      spine.position.set(sign*0.034,0,0.12);spine.rotation.z=sign*0.15;g.add(spine);
      // Latch detail at tip
      const latch=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.01,0.02),new THREE.MeshLambertMaterial({color:0x999999}));
      latch.position.set(sign*0.024,-0.026,0.24);g.add(latch);
    };
    makeHandle(1); makeHandle(-1);
    // Pivot pins (2)
    const pinMat=new THREE.MeshLambertMaterial({color:0xbbbbbb});
    [0.065,0.075].forEach(z=>{
      const pin=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.006,0.08,8),pinMat);
      pin.rotation.z=Math.PI/2;pin.position.set(0,0,z);g.add(pin);
    });

  } else if(model==='shadow'){
    // Shadow Dagger – sleek single-edged, dark finish
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.005,0.3),mm(c1));bld.position.set(0,0.01,-0.08);g.add(bld);
    // Blood groove (fuller)
    const full=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.002,0.22),new THREE.MeshLambertMaterial({color:0x6600bb}));full.position.set(0,0.013,-0.08);g.add(full);
    // Swedge (back edge taper at tip)
    const sw=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.003,0.08),mm(c1));sw.position.set(0,0.012,-0.22);sw.rotation.x=-0.25;g.add(sw);
    const tip=new THREE.Mesh(new THREE.ConeGeometry(0.01,0.055,4),mm(c1));tip.rotation.x=Math.PI/2;tip.rotation.z=Math.PI/4;tip.position.set(0,0.01,-0.265);g.add(tip);
    // Guard
    const grd=new THREE.Mesh(new THREE.BoxGeometry(0.072,0.012,0.018),mm(c2));grd.position.set(0,0.008,0.06);g.add(grd);
    const grdSide=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.025,0.018),mm(c2));grdSide.position.set(0,0.02,0.06);g.add(grdSide);
    // Handle
    const h=new THREE.Mesh(new THREE.BoxGeometry(0.026,0.022,0.18),mm(c2));h.position.set(0,0,0.16);g.add(h);
    // Grip wraps
    for(let i=0;i<5;i++){const w=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.007,0.012),new THREE.MeshLambertMaterial({color:0x110022}));w.position.set(0,0.012,0.075+i*0.032);g.add(w);}
    // Pommel
    const pm=new THREE.Mesh(new THREE.SphereGeometry(0.016,8,6),mm(c2));pm.position.set(0,0,0.255);g.add(pm);

  } else {
    // Standard knife – detailed
    const bld=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.005,0.3),mm(c1));bld.position.set(0,0.012,-0.08);g.add(bld);
    // Edge bevel
    const bev=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.002,0.26),new THREE.MeshLambertMaterial({color:0xdddddd}));bev.position.set(0,0.009,-0.07);g.add(bev);
    // Spine serrations
    for(let i=0;i<5;i++){const s=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.006,0.007),mm(c2));s.position.set(0,0.017,-0.12+i*0.025);s.rotation.z=0.3;g.add(s);}
    const tip=new THREE.Mesh(new THREE.ConeGeometry(0.011,0.055,4),mm(c1));tip.rotation.x=Math.PI/2;tip.rotation.z=Math.PI/4;tip.position.set(0,0.012,-0.26);g.add(tip);
    // Guard
    const grd=new THREE.Mesh(new THREE.BoxGeometry(0.085,0.012,0.02),mm(c2));grd.position.set(0,0.008,0.07);g.add(grd);
    const grdV=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.026,0.02),mm(c2));grdV.position.set(0,0.018,0.07);g.add(grdV);
    // Handle
    const h=new THREE.Mesh(new THREE.BoxGeometry(0.028,0.022,0.18),mm(c2));h.position.set(0,0,0.17);g.add(h);
    // Grip texture
    for(let i=0;i<4;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.006,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,0.013,0.085+i*0.038);g.add(r);}
    // Pommel
    const pm=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.026,0.022),mm(c2));pm.position.set(0,0,0.262);g.add(pm);
  }
  return g;
}

function mkSMG(c1,c2){
  const g=new THREE.Group();
  // Receiver body
  const body=new THREE.Mesh(new THREE.BoxGeometry(0.068,0.09,0.42),mm(c1));g.add(body);
  // Top rail
  const rail=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.01,0.38),new THREE.MeshLambertMaterial({color:0x222222}));rail.position.set(0,0.05,0.01);g.add(rail);
  // Rail teeth
  for(let i=0;i<8;i++){const t=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.007,0.005),new THREE.MeshLambertMaterial({color:0x111111}));t.position.set(0,0.054,-0.14+i*0.04);g.add(t);}
  // Barrel
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.02,0.02,0.26,8),mm(c1));brl.rotation.x=Math.PI/2;brl.position.set(0,0.018,-0.34);g.add(brl);
  // Barrel shroud / handguard
  const hg=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.055,0.15),mm(c2));hg.position.set(0,0.01,-0.2);g.add(hg);
  // Handguard vents
  for(let i=0;i<3;i++){const v=new THREE.Mesh(new THREE.BoxGeometry(0.06,0.012,0.012),new THREE.MeshLambertMaterial({color:0x111111}));v.position.set(0,-0.008,-0.17+i*0.03);g.add(v);}
  // Grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.048,0.1,0.07),mm(c2));gr.position.set(0,-0.068,-0.04);gr.rotation.x=0.12;g.add(gr);
  for(let i=0;i<3;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.05,0.006,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.04-i*0.025,-0.04);g.add(r);}
  // Magazine
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.16,0.06),mm(c2));mag.position.set(0,-0.14,-0.05);g.add(mag);
  // Magazine curve base
  const magb=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.02,0.065),mm(c2));magb.position.set(0,-0.22,-0.048);magb.rotation.x=0.15;g.add(magb);
  // Stock
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.045,0.065,0.19),mm(c2));stk.position.set(0,-0.008,0.3);g.add(stk);
  // Stock butt plate
  const bp=new THREE.Mesh(new THREE.BoxGeometry(0.045,0.075,0.012),mm(c1));bp.position.set(0,-0.005,0.4);g.add(bp);
  // Trigger
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.025,0.01),mm(c2));trig.position.set(0,-0.032,-0.01);trig.rotation.x=0.25;g.add(trig);
  // Trigger guard
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.028,0.006,4,10,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.03,-0.01);g.add(tg);
  // Front sight post
  const fs=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.016,0.007),mm(c1));fs.position.set(0,0.054,-0.32);g.add(fs);
  return g;
}

function mkShotgun(c1,c2){
  const g=new THREE.Group();
  // Receiver
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.076,0.086,0.28),mm(c1));rcv.position.set(0,0,0.06);g.add(rcv);
  // Receiver detail – top
  const rt=new THREE.Mesh(new THREE.BoxGeometry(0.02,0.01,0.26),new THREE.MeshLambertMaterial({color:0x111111}));rt.position.set(0,0.048,0.06);g.add(rt);
  // Ejection port
  const ep=new THREE.Mesh(new THREE.BoxGeometry(0.078,0.032,0.07),new THREE.MeshLambertMaterial({color:0x0a0a0a}));ep.position.set(0,0.015,0.03);g.add(ep);
  // Twin barrels
  [[0.027],[- 0.027]].forEach(([ox])=>{
    const b=new THREE.Mesh(new THREE.CylinderGeometry(0.019,0.019,0.45,8),mm(c1));b.rotation.x=Math.PI/2;b.position.set(ox,0.018,-0.29);g.add(b);
    // Muzzle ring
    const mr=new THREE.Mesh(new THREE.TorusGeometry(0.022,0.005,6,12),mm(c1));mr.rotation.x=Math.PI/2;mr.position.set(ox,0.018,-0.51);g.add(mr);
  });
  // Barrel rib (top rib between barrels)
  const rib=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.005,0.42),new THREE.MeshLambertMaterial({color:0x888888}));rib.position.set(0,0.04,-0.27);g.add(rib);
  // Pump / forend
  const pump=new THREE.Mesh(new THREE.BoxGeometry(0.066,0.052,0.14),mm(c2));pump.position.set(0,0.01,-0.2);g.add(pump);
  // Pump checkering
  for(let i=0;i<4;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.068,0.006,0.01),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,0.006,-0.165+i*0.03);g.add(r);}
  // Stock
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.058,0.075,0.3),mm(c2));stk.position.set(0,-0.003,0.24);g.add(stk);
  // Stock comb taper
  const comb=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.028,0.18),mm(c2));comb.position.set(0,0.05,0.27);g.add(comb);
  // Butt plate
  const bp=new THREE.Mesh(new THREE.BoxGeometry(0.056,0.082,0.013),mm(c1));bp.position.set(0,-0.001,0.395);g.add(bp);
  // Pistol grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.052,0.095,0.068),mm(c2));gr.position.set(0,-0.062,0.1);gr.rotation.x=0.15;g.add(gr);
  for(let i=0;i<3;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.007,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.03-i*0.025,0.1);g.add(r);}
  // Trigger guard
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.03,0.007,4,12,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.028,0.08);g.add(tg);
  // Trigger
  const trig=new THREE.Mesh(new THREE.BoxGeometry(0.006,0.028,0.01),mm(c2));trig.position.set(0,-0.02,0.06);trig.rotation.x=0.25;g.add(trig);
  // Front bead sight
  const bead=new THREE.Mesh(new THREE.SphereGeometry(0.007,6,4),new THREE.MeshLambertMaterial({color:0xff3300}));bead.position.set(0,0.04,-0.51);g.add(bead);
  return g;
}
function mkRevolver(c1,c2){
  const g=new THREE.Group();
  // Barrel
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.018,0.018,0.28,8),mm(c1));brl.rotation.x=Math.PI/2;brl.position.set(0,0.04,-0.1);g.add(brl);
  // Ejector rod under barrel
  const ejr=new THREE.Mesh(new THREE.CylinderGeometry(0.007,0.007,0.2,6),mm(c2));ejr.rotation.x=Math.PI/2;ejr.position.set(0,-0.015,-0.09);g.add(ejr);
  // Cylinder (the iconic revolving chamber)
  const cyl=new THREE.Mesh(new THREE.CylinderGeometry(0.048,0.048,0.07,6),mm(c1));cyl.rotation.x=Math.PI/2;cyl.position.set(0,0.035,0.1);g.add(cyl);
  // Chamber holes (6 chambers)
  for(let i=0;i<6;i++){
    const ang=i*Math.PI/3;
    const ch=new THREE.Mesh(new THREE.CylinderGeometry(0.01,0.01,0.074,6),new THREE.MeshBasicMaterial({color:0x050505}));
    ch.rotation.x=Math.PI/2;ch.position.set(Math.cos(ang)*0.03,0.035+Math.sin(ang)*0.03,0.1);g.add(ch);
  }
  // Frame
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.07,0.22),mm(c2));fr.position.set(0,0.01,0.05);g.add(fr);
  // Top strap (connects barrel to frame)
  const ts=new THREE.Mesh(new THREE.BoxGeometry(0.03,0.015,0.28),mm(c1));ts.position.set(0,0.055,-0.08);g.add(ts);
  // Grip - curved bird's head style
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.052,0.2,0.12),mm(c2));gr.position.set(0,-0.1,0.15);gr.rotation.x=-0.18;g.add(gr);
  // Grip checkering
  for(let i=0;i<4;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.054,0.006,0.01),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.055-i*0.03,0.15);g.add(r);}
  // Hammer
  const hm=new THREE.Mesh(new THREE.BoxGeometry(0.018,0.032,0.022),mm(c1));hm.position.set(0,0.065,0.17);g.add(hm);
  const hmSpur=new THREE.Mesh(new THREE.BoxGeometry(0.016,0.01,0.02),mm(c1));hmSpur.position.set(0,0.082,0.17);g.add(hmSpur);
  // Trigger + guard
  const tr=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.028,0.01),mm(c2));tr.position.set(0,-0.01,0.09);tr.rotation.x=0.3;g.add(tr);
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.026,0.006,4,10,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.018,0.09);g.add(tg);
  // Muzzle crown
  const mz=new THREE.Mesh(new THREE.TorusGeometry(0.02,0.005,5,10),mm(c1));mz.rotation.x=Math.PI/2;mz.position.set(0,0.04,-0.245);g.add(mz);
  return g;
}
function mkDeagle(c1,c2){
  const g=new THREE.Group();
  // Distinctive large triangular slide
  const slide=new THREE.Mesh(new THREE.BoxGeometry(0.075,0.1,0.32),mm(c1));slide.position.set(0,0.06,0);g.add(slide);
  // Slide serrations (rear)
  for(let i=0;i<5;i++){const s=new THREE.Mesh(new THREE.BoxGeometry(0.077,0.05,0.006),new THREE.MeshLambertMaterial({color:0x111111}));s.position.set(0,0.06,0.12+i*0.012);g.add(s);}
  // Barrel - thick, short
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.024,0.024,0.24,8),mm(c1));brl.rotation.x=Math.PI/2;brl.position.set(0,0.04,-0.22);g.add(brl);
  // Muzzle brake
  const mb=new THREE.Mesh(new THREE.BoxGeometry(0.04,0.04,0.025),mm(c1));mb.position.set(0,0.04,-0.345);g.add(mb);
  // Frame
  const fr=new THREE.Mesh(new THREE.BoxGeometry(0.068,0.05,0.28),mm(c2));fr.position.set(0,0.005,0.01);g.add(fr);
  // Grip - large, aggressive
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.062,0.22,0.13),mm(c2));gr.position.set(0,-0.11,0.1);gr.rotation.x=0.1;g.add(gr);
  for(let i=0;i<5;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.064,0.007,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.04-i*0.033,0.14);g.add(r);}
  // Front sight (high)
  const fs=new THREE.Mesh(new THREE.BoxGeometry(0.01,0.022,0.01),mm(c2));fs.position.set(0,0.12,-0.16);g.add(fs);
  const rs=new THREE.Mesh(new THREE.BoxGeometry(0.034,0.018,0.01),mm(c2));rs.position.set(0,0.12,0.14);g.add(rs);
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.032,0.007,4,12,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.025,0.04);g.add(tg);
  const tr=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.03,0.012),mm(c2));tr.position.set(0,-0.02,0.02);tr.rotation.x=0.28;g.add(tr);
  return g;
}
function mkCompact(c1,c2){
  const g=new THREE.Group();
  // Very boxy receiver
  const rcv=new THREE.Mesh(new THREE.BoxGeometry(0.065,0.095,0.28),mm(c1));rcv.position.set(0,0.01,0.01);g.add(rcv);
  // Short barrel with compensator holes
  const brl=new THREE.Mesh(new THREE.CylinderGeometry(0.018,0.018,0.14,8),mm(c1));brl.rotation.x=Math.PI/2;brl.position.set(0,0.02,-0.18);g.add(brl);
  // Comp vents
  for(let i=0;i<3;i++){const v=new THREE.Mesh(new THREE.BoxGeometry(0.022,0.012,0.012),new THREE.MeshLambertMaterial({color:0x111111}));v.position.set(0,0.03,-0.14-i*0.016);g.add(v);}
  // Stubby grip
  const gr=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.11,0.075),mm(c2));gr.position.set(0,-0.055,0.08);gr.rotation.x=0.08;g.add(gr);
  for(let i=0;i<3;i++){const r=new THREE.Mesh(new THREE.BoxGeometry(0.057,0.007,0.012),new THREE.MeshLambertMaterial({color:0x111111}));r.position.set(0,-0.015-i*0.03,0.1);g.add(r);}
  // Extended magazine (iconic MAC-10 look)
  const mag=new THREE.Mesh(new THREE.BoxGeometry(0.038,0.22,0.055),mm(c2));mag.position.set(0,-0.14,0.08);g.add(mag);
  // Mag base
  const mb2=new THREE.Mesh(new THREE.BoxGeometry(0.04,0.012,0.06),mm(c1));mb2.position.set(0,-0.255,0.08);g.add(mb2);
  // Folding stock outline (folded back)
  const stk=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.065,0.16),mm(c2));stk.position.set(0,-0.02,0.2);g.add(stk);
  const stkb=new THREE.Mesh(new THREE.BoxGeometry(0.012,0.015,0.1),mm(c2));stkb.position.set(0,-0.05,0.26);g.add(stkb);
  // Trig guard
  const tg=new THREE.Mesh(new THREE.TorusGeometry(0.025,0.006,4,10,Math.PI),mm(c2));tg.rotation.x=Math.PI/2;tg.position.set(0,-0.025,0.03);g.add(tg);
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
  const vsLens=new THREE.Mesh(new THREE.PlaneGeometry(0.026,0.022),neon2);vsLens.position.set(0,0.118,-0.075);g.add(vsLens);
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
  const grCrystal=new THREE.Mesh(new THREE.BoxGeometry(0.007,0.22,0.12),crystalM);grCrystal.position.set(0.033,-0.12,0.1);gr.rotation.x=0.1;g.add(grCrystal);
  const grCrystal2=grCrystal.clone();grCrystal2.position.x=-0.033;g.add(grCrystal2);
  // Glowing rune on grip
  const grRune=new THREE.Mesh(new THREE.BoxGeometry(0.005,0.06,0.06),glowM);grRune.position.set(0.034,-0.1,0.1);g.add(grRune);
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
  // Long wooden shaft
  const shaft=new THREE.Mesh(new THREE.CylinderGeometry(0.022,0.026,0.68,8),shaftM);
  shaft.rotation.x=Math.PI/2;shaft.position.set(0,0,0.18);g.add(shaft);
  // Grip wraps
  for(let i=0;i<4;i++){const wr=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.028,0.022,8),bandM);wr.rotation.x=Math.PI/2;wr.position.set(0,0,0.36+i*0.04);g.add(wr);}
  // Massive square maul head
  const head=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.16,0.22),headM);head.position.set(0,0,-0.13);g.add(head);
  // Bevel top
  const bev=new THREE.Mesh(new THREE.BoxGeometry(0.18,0.014,0.22),new THREE.MeshLambertMaterial({color:0x888888}));bev.position.set(0,0.087,-0.13);g.add(bev);
  // Strike face detail
  const face=new THREE.Mesh(new THREE.BoxGeometry(0.165,0.145,0.012),new THREE.MeshLambertMaterial({color:0x444444}));face.position.set(0,0,-0.241);g.add(face);
  // Collar ring where head meets shaft
  const collar=new THREE.Mesh(new THREE.CylinderGeometry(0.035,0.035,0.04,8),bandM);collar.rotation.x=Math.PI/2;collar.position.set(0,0,0.07);g.add(collar);
  // Pommel end cap
  const pom=new THREE.Mesh(new THREE.CylinderGeometry(0.028,0.022,0.025,8),bandM);pom.rotation.x=Math.PI/2;pom.position.set(0,0,0.53);g.add(pom);
  return g;
}
function mkSledge_Void(c1,c2){
  const g=new THREE.Group();
  const voidM=new THREE.MeshLambertMaterial({color:0x080010});
  const purpM=new THREE.MeshLambertMaterial({color:c2,emissive:new THREE.Color(c2).multiplyScalar(0.4)});
  const crystalM=new THREE.MeshLambertMaterial({color:c1,emissive:new THREE.Color(c1).multiplyScalar(0.3)});
  // Void-crystal maul head
  const head=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.17,0.23),voidM);head.position.set(0,0,-0.13);g.add(head);
  // Crystal front face plate
  const fp=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.17,0.014),crystalM);fp.position.set(0,0,-0.244);g.add(fp);
  // Crystal back face plate
  const fp2=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.17,0.014),crystalM);fp2.position.set(0,0,-0.016+0.23);g.add(fp2);
  // Glowing rune lines on head
  [[0.07,0],[0,-0.06],[0.06,0.06],[-0.06,0.04]].forEach(([x,y])=>{
    const r=new THREE.Mesh(new THREE.BoxGeometry(0.004,0.004,0.235),purpM);r.position.set(x,y,-0.13);g.add(r);
  });
  // Void tendrils
  for(let i=0;i<6;i++){const ang=i/6*Math.PI*2;const t=new THREE.Mesh(new THREE.CylinderGeometry(0.006,0.003,0.06,5),purpM);t.position.set(Math.cos(ang)*0.11,Math.sin(ang)*0.1,-0.13);t.rotation.x=Math.PI/2+Math.cos(ang)*0.4;t.rotation.y=ang;g.add(t);}
  // Dark shaft
  const shaft=new THREE.Mesh(new THREE.CylinderGeometry(0.02,0.025,0.66,6),voidM);shaft.rotation.x=Math.PI/2;shaft.position.set(0,0,0.18);g.add(shaft);
  for(let i=0;i<6;i++){const band=new THREE.Mesh(new THREE.CylinderGeometry(0.026,0.026,0.018,6),purpM);band.rotation.x=Math.PI/2;band.position.set(0,0,0.05+i*0.075);g.add(band);}
  // Void orb at base
  const orb=new THREE.Mesh(new THREE.SphereGeometry(0.034,8,7),crystalM);orb.position.set(0,0,0.52);g.add(orb);
  const orbRing=new THREE.Mesh(new THREE.TorusGeometry(0.044,0.007,5,14),purpM);orbRing.rotation.x=Math.PI/2;orbRing.position.set(0,0,0.52);g.add(orbRing);
  return g;
}
function getMesh(wid,skinId){
  const def=WDEFS[wid],skin=def.skins.find(s=>s.id===skinId)||def.skins[0];
  const [c1,c2]=skin.colors;
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
    // ── BRUTE: big, orange, slow, double HP ──
    g.userData.maxHp=200; g.userData.hp=200;
    g.userData.speed=Math.min(0.7+S.wave*0.04+Math.random()*0.2, 1.8);
    g.userData.dmg=18;
    const bM=new THREE.MeshLambertMaterial({color:0xcc5500});
    const sM=new THREE.MeshLambertMaterial({color:0xaa3300});
    const legM=new THREE.MeshLambertMaterial({color:0x663300});
    const eMat=new THREE.MeshBasicMaterial({color:0xff6600});
    // Larger body parts
    const body=new THREE.Mesh(new THREE.BoxGeometry(0.75,0.85,0.45),bM);body.position.y=1.1;body.castShadow=true;g.add(body);
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.52,0.48,0.44),sM);head.position.y=1.79;head.castShadow=true;g.add(head);
    [[-0.13,1.83,0.225],[0.13,1.83,0.225]].forEach(([x,y,z])=>{
      const e=new THREE.Mesh(new THREE.BoxGeometry(0.1,0.06,0.01),eMat);e.position.set(x,y,z);g.add(e);
    });
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.2,0.72,0.22),sM);lA.position.set(-0.49,1.1,0.02);lA.rotation.x=-0.4;lA.userData.l='la';g.add(lA);
    const rA=lA.clone();rA.position.x=0.49;rA.userData.l='ra';g.add(rA);
    const lL=new THREE.Mesh(new THREE.BoxGeometry(0.28,0.78,0.28),legM);lL.position.set(-0.18,0.38,0);lL.userData.l='ll';g.add(lL);
    const rL=lL.clone();rL.position.x=0.18;rL.userData.l='rl';g.add(rL);
    // Large hitboxes
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.82,1.55,0.55),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.77;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.56,0.52,0.48),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.79;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    // HP bar (wider)
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.9,0.09),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=2.45;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.9,0.09),new THREE.MeshBasicMaterial({color:0xff6600}));hpFg.position.set(0,2.451,0.001);hpFg.userData.bar=true;g.add(hpFg);

  } else if(type==='runner'){
    // ── RUNNER: slim, blue, very fast, half HP ──
    g.userData.maxHp=50; g.userData.hp=50;
    g.userData.speed=Math.min(3.4+S.wave*0.1+Math.random()*0.6, 7.0);
    g.userData.dmg=5;
    const bM=new THREE.MeshLambertMaterial({color:0x1144cc});
    const sM=new THREE.MeshLambertMaterial({color:0x6688cc});
    const legM=new THREE.MeshLambertMaterial({color:0x0033aa});
    const eMat=new THREE.MeshBasicMaterial({color:0x00ffff});
    const body=new THREE.Mesh(new THREE.BoxGeometry(0.38,0.52,0.24),bM);body.position.y=0.82;body.castShadow=true;g.add(body);
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.28,0.28,0.26),sM);head.position.y=1.24;head.castShadow=true;g.add(head);
    [[-0.07,1.27,0.135],[0.07,1.27,0.135]].forEach(([x,y,z])=>{
      const e=new THREE.Mesh(new THREE.BoxGeometry(0.055,0.036,0.01),eMat);e.position.set(x,y,z);g.add(e);
    });
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.11,0.42,0.14),sM);lA.position.set(-0.27,0.82,0.02);lA.rotation.x=-0.7;lA.userData.l='la';g.add(lA);
    const rA=lA.clone();rA.position.x=0.27;rA.userData.l='ra';g.add(rA);
    const lL=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.46,0.16),legM);lL.position.set(-0.09,0.25,0);lL.userData.l='ll';g.add(lL);
    const rL=lL.clone();rL.position.x=0.09;rL.userData.l='rl';g.add(rL);
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.42,1.1,0.35),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.55;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.32,0.32,0.3),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.24;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.45,0.06),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=1.65;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.45,0.06),new THREE.MeshBasicMaterial({color:0x00ccff}));hpFg.position.set(0,1.651,0.001);hpFg.userData.bar=true;g.add(hpFg);

  } else {
    // ── NORMAL: green, standard ──
    g.userData.maxHp=100; g.userData.hp=100;
    g.userData.speed=Math.min(1.3+S.wave*0.1+Math.random()*0.5,4.0);
    g.userData.dmg=9;
    const bM=new THREE.MeshLambertMaterial({color:0x1d4a18});
    const sM=new THREE.MeshLambertMaterial({color:0x7a6040});
    const eMat=new THREE.MeshBasicMaterial({color:0xff1100});
    const body=new THREE.Mesh(new THREE.BoxGeometry(0.5,0.65,0.3),bM);body.position.y=0.97;body.castShadow=true;g.add(body);
    const head=new THREE.Mesh(new THREE.BoxGeometry(0.34,0.34,0.3),sM);head.position.y=1.54;head.castShadow=true;g.add(head);
    [[-0.09,1.57,0.155],[0.09,1.57,0.155]].forEach(([x,y,z])=>{
      const e=new THREE.Mesh(new THREE.BoxGeometry(0.07,0.045,0.01),eMat);e.position.set(x,y,z);g.add(e);
    });
    const lA=new THREE.Mesh(new THREE.BoxGeometry(0.14,0.52,0.17),sM);lA.position.set(-0.33,0.98,0.02);lA.rotation.x=-0.5;lA.userData.l='la';g.add(lA);
    const rA=lA.clone();rA.position.x=0.33;rA.userData.l='ra';g.add(rA);
    const lL=new THREE.Mesh(new THREE.BoxGeometry(0.19,0.58,0.2),new THREE.MeshLambertMaterial({color:0x1a3a50}));lL.position.set(-0.12,0.33,0);lL.userData.l='ll';g.add(lL);
    const rL=lL.clone();rL.position.x=0.12;rL.userData.l='rl';g.add(rL);
    const hb=new THREE.Mesh(new THREE.BoxGeometry(0.55,1.3,0.55),new THREE.MeshBasicMaterial({visible:false}));hb.position.y=0.65;hb.userData.hitbox=true;hb.userData.isHead=false;g.add(hb);
    const hbH=new THREE.Mesh(new THREE.BoxGeometry(0.38,0.38,0.38),new THREE.MeshBasicMaterial({visible:false}));hbH.position.y=1.54;hbH.userData.hitbox=true;hbH.userData.isHead=true;g.add(hbH);
    const hpBg=new THREE.Mesh(new THREE.PlaneGeometry(0.62,0.07),new THREE.MeshBasicMaterial({color:0x1a1a1a}));hpBg.position.y=2.05;g.add(hpBg);
    const hpFg=new THREE.Mesh(new THREE.PlaneGeometry(0.62,0.07),new THREE.MeshBasicMaterial({color:0x44ff44}));hpFg.position.set(0,2.051,0.001);hpFg.userData.bar=true;g.add(hpFg);
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
    dmg = z.userData.type==='brute' ? 50 : z.userData.maxHp; // 1-shot normal/runner, 4-hit brute
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
  const wid=S.held,def=WDEFS[wid],ws=S.weapons[wid];
  const now=performance.now();if(now-S.lastFire<def.fireMs)return;S.lastFire=now;
  if(def.type==='gun'){
    if(ws.ammo<=0){notify('OUT OF AMMO - buy more in Shop');soundEmpty();return;}
    ws.ammo--;updateHUD();playGunSound(wid);
    const fl=new THREE.PointLight(0xff9900,6,4);fl.position.set(0,0.05,-0.8);camera.add(fl);setTimeout(()=>camera.remove(fl),40);
    RAY.setFromCamera(new THREE.Vector2(0,0),camera);
    const hbs=[];S.zombies.forEach(z=>{z.children.forEach(c=>{if(c.userData.hitbox)hbs.push(c);});});
    const hits=RAY.intersectObjects(hbs);
    if(hits.length){const h=hits[0].object;hitZombie(h.parent,wid,h.userData.isHead===true);}
    weaponGroup.rotation.x-=0.07;setTimeout(()=>weaponGroup.rotation.x+=0.07,90);
  } else {
    const meleeCd=def.cooldown||1500;
    if(knifeCooldown>0) return;
    weaponGroup.rotation.x-=0.32;weaponGroup.rotation.y-=0.14;
    // Sledge has a bigger, slower swing
    const swingBack=wid==='sledge'?0.5:0.32, swingSide=wid==='sledge'?0.2:0.14, swingMs=wid==='sledge'?220:150;
    weaponGroup.rotation.x-=(swingBack-0.32);weaponGroup.rotation.y-=(swingSide-0.14);
    setTimeout(()=>{weaponGroup.rotation.x+=swingBack;weaponGroup.rotation.y+=swingSide;},swingMs);
    soundKnife();
    knifeCooldown=meleeCd;
    const reach=wid==='sledge'?3.4:2.8;
    S.zombies.forEach(z=>{if(!z.userData.dead&&camera.position.distanceTo(z.position)<reach)hitZombie(z,wid,false);});
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
let inspecting=false,inspectT0=0;
function startInspect(){if(inspecting)return;inspecting=true;inspectT0=performance.now();}
function tickInspect(now){
  if(!inspecting)return;
  const t=Math.min((now-inspectT0)/2200,1);
  const wid=S.held,skin=S.weapons[wid].skin;
  if(wid==='knife'&&skin==='butterfly'){
    weaponGroup.rotation.z=Math.sin(t*Math.PI*4)*1.4;weaponGroup.rotation.x=-Math.sin(t*Math.PI*2)*0.3;
    weaponGroup.position.y=-0.32+Math.sin(t*Math.PI*3)*0.06;
    const m=weaponGroup.children[0];
    if(m)m.children.forEach(c=>{if(c.userData.bfH===1)c.rotation.z=0.12+Math.sin(t*Math.PI*4)*1.5;if(c.userData.bfH===2)c.rotation.z=-0.12-Math.sin(t*Math.PI*4)*1.5;});
  } else if(wid==='knife'&&skin==='bayonet'){
    // Bayonet inspect: tilt out, thrust forward, return
    weaponGroup.rotation.y=0.1+Math.sin(t*Math.PI)*0.7;
    weaponGroup.rotation.x=Math.sin(t*Math.PI*2)*-0.35;
    weaponGroup.position.z=-0.48+Math.sin(t*Math.PI*2)*0.1;
  } else if(wid==='knife'){
    weaponGroup.rotation.y=0.1+Math.sin(t*Math.PI*2)*0.9;weaponGroup.rotation.x=Math.sin(t*Math.PI)*0.3;
  } else {
    weaponGroup.rotation.y=0.1+Math.sin(t*Math.PI*2)*0.5;weaponGroup.rotation.z=Math.sin(t*Math.PI)*0.25;
  }
  if(t>=1){inspecting=false;weaponGroup.position.set(0.28,-0.32,-0.48);weaponGroup.rotation.set(0,0.1,0);}
}

// ══ INPUT ══
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
  if(e.code==='Digit3'||e.code==='KeyQ')sw('knife');
  if(e.code==='KeyF')startInspect();
  if(e.code==='KeyH')useKit();
  if(e.code==='KeyR')doReload();
  if(e.code==='Space'&&S.screen==='game'&&onGround){velY=JUMP_VEL;onGround=false;}
});
document.addEventListener('keyup',e=>keys[e.code]=false);
document.addEventListener('mousemove',e=>{
  if(S.screen==='game'&&locked){
    yaw-=e.movementX*0.0018*SETTINGS.sensitivity;pitch=Math.max(-1.3,Math.min(1.3,pitch-e.movementY*0.0018*SETTINGS.sensitivity));
    camera.rotation.order='YXZ';camera.rotation.y=yaw;camera.rotation.x=pitch;
  }
});
let lockRequested=false;
cvs.addEventListener('click',()=>{
  if((S.screen==='game'||S.screen==='paused')&&!locked){lockRequested=true;cvs.requestPointerLock();}
});
document.addEventListener('pointerlockchange',()=>{
  locked=document.pointerLockElement===cvs;
  if(locked){
    lockRequested=false;
    if(S.screen==='paused') resume();
  } else {
    if(!lockRequested&&S.screen==='game'){
      setTimeout(()=>{if(!locked&&S.screen==='game')pause();},80);
    }
    lockRequested=false;
  }
});
document.addEventListener('mousedown',e=>{
  if(S.screen==='game'&&!locked&&e.button===0){lockRequested=true;cvs.requestPointerLock();return;}
  if(S.screen!=='game'||!locked||e.button!==0)return;
  shoot();
  if(WDEFS[S.held].fireMs<200&&WDEFS[S.held].type==='gun'){clearInterval(S.autoFire);S.autoFire=setInterval(()=>{if(S.screen==='game')shoot();else clearInterval(S.autoFire);},WDEFS[S.held].fireMs);}
});
document.addEventListener('mouseup',e=>{if(e.button===0)clearInterval(S.autoFire);});
function swSlot(slot){
  const ids=Object.keys(WDEFS).filter(id=>WDEFS[id].slot===slot&&S.weapons[id]&&S.weapons[id].owned);
  if(!ids.length){notify('No '+slot+' weapon owned!');return;}
  if(WDEFS[S.held]&&WDEFS[S.held].slot===slot) return;
  const cur=slot==='primary'?S.heldPrimary:slot==='secondary'?S.heldSecondary:S.heldKnife;
  const target=ids.includes(cur)?cur:ids[0];
  if(slot==='primary') S.heldPrimary=target;
  else if(slot==='secondary') S.heldSecondary=target;
  else S.heldKnife=target;
  sw(target);
}
function sw(id){
  if(!S.weapons[id]||!S.weapons[id].owned){notify('Not owned!');return;}
  S.held=id;
  if(WDEFS[id].slot==='primary')   S.heldPrimary=id;
  if(WDEFS[id].slot==='secondary') S.heldSecondary=id;
  if(WDEFS[id].slot==='knife')     S.heldKnife=id;
  reloading=false;reloadTimer=0;knifeCooldown=0;
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
const KNIFE_COOLDOWN=1500; // 1.5 seconds
let knifeCooldown=0; // ms remaining

function resolveCollision(px,pz){
  // Clamp to arena
  px=Math.max(-37,Math.min(37,px));
  pz=Math.max(-37,Math.min(37,pz));
  for(const c of COL){
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
    [nx,nz]=resolveCollision(nx,nz);
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

    tickInspect(ts);tickZombies(dt);tickWave(dt);

    // ── Melee cooldown bar ──
    const curDef=WDEFS[S.held];
    if(curDef.type==='melee'&&knifeCooldown>0){
      const meleeCd=curDef.cooldown||1500;
      knifeCooldown=Math.max(0,knifeCooldown-dt);
      const bar=document.getElementById('knifeCoolBar');
      const fill=document.getElementById('knifeCoolFill');
      bar.style.opacity='1';
      fill.style.width=(knifeCooldown/meleeCd*100)+'%';
      if(knifeCooldown<=0) bar.style.opacity='0';
    } else if(curDef.type!=='melee'){
      document.getElementById('knifeCoolBar').style.opacity='0';
    }

    // ── Reload timer ──
    if(reloading){
      reloadTimer-=dt;
      if(reloadTimer<=0){
        reloading=false;
        const ws=S.weapons[reloadWid],def=WDEFS[reloadWid];
        const need=def.startAmmo-ws.ammo,fill=Math.min(need,ws.res);
        ws.ammo+=fill;ws.res-=fill;
        updateHUD();notify('');
      } else {
        const pct=1-reloadTimer/3000;
        notify('RELOADING... '+(pct*100|0)+'%');
      }
    }
  } else {lastT=ts;}
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
  const resume=()=>{hideEl('unlockBanner');if(wasScreen==='game'){S.screen='game';cvs.requestPointerLock();}S.showingUnlock=false;setTimeout(showNextUnlock,200);};
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
  S.held='pistol';S.heldSecondary='pistol';S.heldKnife='knife';
  const firstPrimary=Object.keys(WDEFS).find(id=>WDEFS[id].slot==='primary'&&S.weapons[id]&&S.weapons[id].owned);
  if(firstPrimary) S.heldPrimary=firstPrimary;
  camera.position.set(0,1.75,18);camera.rotation.set(0,0,0);
  yaw=0;pitch=0;hbob=0;lastT=performance.now();
  velY=0;onGround=true;stamina=STAM_MAX;stamDepleted=false;reloading=false;reloadTimer=0;knifeCooldown=0;
  S.weapons.pistol.ammo=30;S.weapons.pistol.res=90;
  Object.keys(S.weapons).forEach(wid=>{
    if(WDEFS[wid]&&WDEFS[wid].type==='gun'&&wid!=='pistol'&&S.weapons[wid].owned){
      S.weapons[wid].ammo=WDEFS[wid].startAmmo;S.weapons[wid].res=WDEFS[wid].resAmmo;
    }
  });
  hideEl('menuScreen');
  document.getElementById('hud').style.display='block';
  updateHUD();rebuildWeapon();
  lockRequested=true;cvs.requestPointerLock();
  setTimeout(startWave,1500);
}
function pause(){if(S.screen!=='game')return;S.screen='paused';showEl('pauseMenu');}
function resume(){if(S.screen!=='paused')return;hideEl('pauseMenu');S.screen='game';lockRequested=true;cvs.requestPointerLock();}
function gameOver(){
  S.screen='gameover';if(document.pointerLockElement)document.exitPointerLock();
  document.getElementById('hud').style.display='none';
  document.getElementById('goStats').innerHTML='Total Kills: '+S.totalKills+'<br>Credits: '+S.credits+'<br>Wave: '+S.wave;
  showEl('gameOverScreen');S.zombies.forEach(z=>scene.remove(z));S.zombies.length=0;
}
function backToMenu(){
  S.zombies.forEach(z=>scene.remove(z));S.zombies.length=0;
  S.screen='menu';['pauseMenu','gameOverScreen'].forEach(hideEl);
  document.getElementById('hud').style.display='none';
  showEl('menuScreen');document.getElementById('menuCredits').textContent=S.credits;
  if(document.pointerLockElement)document.exitPointerLock();
}

// ══ SHOP ══
function renderShop(){
  const wg=document.getElementById('shopWeapons');wg.innerHTML='';
  const icons={smg:'MP5',shotgun:'SHT',revolver:'REV',deagle:'DEG',compact:'MAC',sledge:'SLG'};
  Object.keys(WDEFS).forEach(wid=>{
    const def=WDEFS[wid],ws=S.weapons[wid];
    if(!def.price)return;
    const c=document.createElement('div');c.className='shop-card';
    const slotTag=`<span style="font-size:9px;background:rgba(255,100,0,.15);border:1px solid rgba(255,100,0,.3);border-radius:2px;padding:1px 5px;color:#ff8040;letter-spacing:1px;font-family:'Space Mono',monospace">${def.slot.toUpperCase()}</span>`;
    const descTxt=def.type==='melee'
      ? `${def.cooldown/1000}s cooldown · melee`
      : `${def.hitsToKill} hits/kill · ${def.fireMs<200?'Full-auto':def.fireMs>900?'Pump':'Semi'}`;
    c.innerHTML=`<div class="icon">${icons[wid]||'GUN'}</div><h3>${def.name}</h3><div style="margin-bottom:6px">${slotTag}</div><div class="desc">${descTxt}</div><div class="price">CREDITS: ${def.price}</div>${ws.owned?'<div class="owned-badge">OWNED</div>':`<button class="btn-buy" ${S.credits>=def.price?'':'disabled'} onclick="buyWeapon('${wid}')">BUY</button>`}`;
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
  S.credits-=def.price;S.weapons[wid].owned=true;
  if(def.type==='gun'){S.weapons[wid].ammo=def.startAmmo;S.weapons[wid].res=def.resAmmo;}
  if(def.slot==='primary')   S.heldPrimary=wid;
  if(def.slot==='secondary') S.heldSecondary=wid;
  if(def.slot==='knife')     S.heldKnife=wid;
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
    const isActiveSlot=(slot==='primary'&&S.heldPrimary===wid)||(slot==='secondary'&&S.heldSecondary===wid)||(slot==='knife');
    // Weapon header row
    const hdrRow=document.createElement('div');
    hdrRow.style.cssText='display:flex;align-items:center;gap:10px;margin:'+(wi>0?'22px':'4px')+' 0 10px';
    hdrRow.innerHTML=
      '<span style="font-family:\'Bebas Neue\',sans-serif;font-size:20px;letter-spacing:3px;color:'+(isActiveSlot?'#ff8040':'#555')+'">'+def.name+'</span>'
      +(isActiveSlot
        ? '<span style="font-size:9px;background:rgba(255,100,0,.18);border:1px solid rgba(255,100,0,.4);border-radius:2px;padding:2px 7px;color:#ff8040;font-family:\'Space Mono\',monospace;letter-spacing:1px">ACTIVE</span>'
        : (slot!=='knife'?'<button class="btn-equip-sm" style="font-size:9px;padding:3px 10px" onclick="setActiveWeapon(\''+wid+'\')">SET ACTIVE</button>':'')
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
function playGunSound(wid){
  if(wid==='pistol')soundPistol();
  else if(wid==='smg')soundSMG();
  else if(wid==='shotgun')soundShotgun();
  else if(wid==='revolver')soundRevolver();
  else if(wid==='deagle')soundDeagle();
  else if(wid==='compact')soundCompact();
  else soundPistol();
}
buildMap();
rebuildWeapon();
requestAnimationFrame(loop);

})();
</script>
</body>
</html>
