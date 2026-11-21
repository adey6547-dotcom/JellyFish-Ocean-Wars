<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>JELLYFISH KINGDOM – OCEAN WARS</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Quicksand:wght@500;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --glass-bg: rgba(12, 28, 62, 0.72);
    --glass-border: rgba(125, 249, 255, 0.35);
    --pink-glow: #ff6ec7;
    --cyan-glow: #4df0ff;
    --violet-glow: #a855f7;
    --gold-glow: #fbbf24;
  }
  * {
    box-sizing: border-box;
    user-select: none;
    -webkit-user-select: none;
    -webkit-touch-callout: none;
  }
  html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #041024;
    font-family: 'Quicksand', sans-serif;
    color: #f0f9ff;
    touch-action: none;
  }
  h1, h2, h3, .title-font {
    font-family: 'Fredoka', sans-serif;
  }
  #gameCanvas {
    position: fixed;
    inset: 0;
    width: 100vw;
    height: 100vh;
    display: block;
    z-index: 1;
  }
  .ui-layer {
    position: fixed;
    inset: 0;
    z-index: 10;
    pointer-events: none;
  }
  .pointer-auto {
    pointer-events: auto;
  }
  .glass-panel {
    background: linear-gradient(145deg, rgba(16, 38, 82, 0.82), rgba(28, 18, 68, 0.85));
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    border: 1.5px solid rgba(114, 239, 255, 0.35);
    box-shadow: 0 12px 35px rgba(0, 8, 28, 0.65), inset 0 1px 1px rgba(255, 255, 255, 0.2);
    border-radius: 22px;
  }
  .glass-card {
    background: linear-gradient(160deg, rgba(24, 54, 110, 0.65), rgba(45, 22, 95, 0.72));
    border: 1.5px solid rgba(140, 235, 255, 0.28);
    border-radius: 18px;
    transition: transform 0.18s ease, border-color 0.18s ease, box-shadow 0.18s ease;
  }
  .glass-card:hover {
    transform: translateY(-2px);
    border-color: rgba(255, 110, 199, 0.7);
    box-shadow: 0 8px 24px rgba(255, 110, 199, 0.25);
  }
  .btn-magic {
    background: linear-gradient(135deg, #ff5db1 0%, #8b5cf6 52%, #06b6d4 100%);
    background-size: 200% 200%;
    border: 2px solid rgba(255, 255, 255, 0.45);
    border-radius: 999px;
    color: white;
    font-family: 'Fredoka', sans-serif;
    font-weight: 600;
    letter-spacing: 0.03em;
    box-shadow: 0 6px 20px rgba(236, 72, 153, 0.45), inset 0 2px 4px rgba(255, 255, 255, 0.45);
    transition: all 0.2s ease;
    cursor: pointer;
  }
  .btn-magic:hover:not(:disabled) {
    background-position: right center;
    transform: scale(1.04);
    box-shadow: 0 8px 26px rgba(6, 182, 212, 0.6), inset 0 2px 6px rgba(255, 255, 255, 0.6);
  }
  .btn-magic:active:not(:disabled) {
    transform: scale(0.97);
  }
  .btn-magic:disabled {
    opacity: 0.45;
    cursor: not-allowed;
    filter: grayscale(0.5);
  }
  .btn-ocean {
    background: linear-gradient(135deg, rgba(14, 116, 144, 0.85), rgba(67, 56, 202, 0.85));
    border: 1.5px solid rgba(103, 232, 249, 0.45);
    border-radius: 16px;
    color: #ecfeff;
    font-family: 'Fredoka', sans-serif;
    font-weight: 600;
    box-shadow: 0 4px 14px rgba(6, 182, 212, 0.25);
    transition: all 0.18s ease;
    cursor: pointer;
  }
  .btn-ocean:hover:not(:disabled) {
    background: linear-gradient(135deg, rgba(6, 182, 212, 0.95), rgba(124, 58, 237, 0.95));
    border-color: #ff80df;
    transform: translateY(-2px);
    box-shadow: 0 6px 18px rgba(236, 72, 153, 0.35);
  }
  .btn-ocean:active:not(:disabled) {
    transform: scale(0.96);
  }
  .text-glow-pink {
    text-shadow: 0 0 14px rgba(255, 110, 199, 0.85), 0 2px 6px rgba(0, 0, 0, 0.7);
  }
  .text-glow-cyan {
    text-shadow: 0 0 12px rgba(77, 240, 255, 0.85), 0 2px 5px rgba(0, 0, 0, 0.7);
  }
  .custom-scroll::-webkit-scrollbar {
    width: 6px;
    height: 6px;
  }
  .custom-scroll::-webkit-scrollbar-track {
    background: rgba(8, 20, 48, 0.5);
    border-radius: 10px;
  }
  .custom-scroll::-webkit-scrollbar-thumb {
    background: linear-gradient(180deg, #ff6ec7, #38bdf8);
    border-radius: 10px;
  }
  @keyframes floatSlow {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-7px); }
  }
  .animate-float {
    animation: floatSlow 3.4s ease-in-out infinite;
  }
  @keyframes pulseGlow {
    0%, 100% { box-shadow: 0 0 15px rgba(255, 110, 199, 0.5); }
    50% { box-shadow: 0 0 28px rgba(77, 240, 255, 0.85); }
  }
  .pulse-glow {
    animation: pulseGlow 2.5s infinite;
  }
  /* Touch joystick & action buttons */
  .touch-btn {
    border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, rgba(103, 232, 249, 0.45), rgba(30, 27, 75, 0.72));
    border: 2px solid rgba(165, 243, 252, 0.55);
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.45), inset 0 0 12px rgba(255, 110, 199, 0.35);
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-family: 'Fredoka', sans-serif;
    font-weight: 700;
    backdrop-filter: blur(4px);
    transition: transform 0.08s ease;
  }
  .touch-btn:active, .touch-btn.active {
    transform: scale(0.9);
    background: radial-gradient(circle at 30% 30%, rgba(255, 110, 199, 0.75), rgba(109, 40, 217, 0.85));
    border-color: #fff;
  }
</style>
</head>
<body>

<!-- Main Game & Animated Ocean Background Canvas -->
<canvas id="gameCanvas"></canvas>

<!-- Toast Notification Container -->
<div id="toastContainer" class="fixed top-4 left-1/2 -translate-x-1/2 z-50 flex flex-col items-center gap-2 pointer-events-none w-11/12 max-w-md"></div>

<!-- ==================== TOP CURRENCY BAR (MENUS) ==================== -->
<div id="menuTopBar" class="ui-layer flex justify-between items-start p-3 sm:p-4">
  <div class="pointer-auto flex items-center gap-2 glass-panel px-3 py-1.5 sm:px-4 sm:py-2 rounded-full">
    <span class="text-lg sm:text-xl">🪼</span>
    <div>
      <div id="topHeroName" class="text-xs sm:text-sm font-bold text-pink-300 leading-tight">Pink Warrior</div>
      <div id="topProgressText" class="text-[10px] sm:text-xs text-cyan-200">World 1 • Level 1</div>
    </div>
  </div>

  <div class="pointer-auto flex flex-wrap justify-end items-center gap-1.5 sm:gap-2.5">
    <div class="glass-panel px-2.5 py-1 sm:px-3.5 sm:py-1.5 rounded-full flex items-center gap-1.5" title="Pearls">
      <span class="text-sm sm:text-base">⚪</span>
      <span id="currencyPearls" class="font-bold text-xs sm:text-sm text-cyan-100">0</span>
    </div>
    <div class="glass-panel px-2.5 py-1 sm:px-3.5 sm:py-1.5 rounded-full flex items-center gap-1.5" title="Golden Pearls">
      <span class="text-sm sm:text-base">🟡</span>
      <span id="currencyGolden" class="font-bold text-xs sm:text-sm text-amber-300">0</span>
    </div>
    <div class="glass-panel px-2.5 py-1 sm:px-3.5 sm:py-1.5 rounded-full flex items-center gap-1.5" title="Magical Crystals">
      <span class="text-sm sm:text-base">💎</span>
      <span id="currencyCrystals" class="font-bold text-xs sm:text-sm text-fuchsia-300">0</span>
    </div>
    <button onclick="UI.toggleFullscreen()" class="glass-panel p-1.5 sm:p-2 rounded-full hover:border-pink-400 text-cyan-200 text-xs sm:text-sm" title="Fullscreen">⛶</button>
  </div>
</div>

<!-- ==================== MAIN MENU OVERLAY ==================== -->
<div id="mainMenu" class="ui-layer flex flex-col items-center justify-center p-4 pt-16 sm:pt-14">
  <div class="pointer-auto flex flex-col items-center text-center max-w-4xl w-full">
    <!-- Title Crest -->
    <div class="animate-float mb-3 sm:mb-5">
      <div class="inline-block px-4 py-1 rounded-full bg-gradient-to-r from-pink-500/40 via-purple-500/40 to-cyan-500/40 border border-pink-300/40 text-cyan-200 text-xs sm:text-sm font-bold tracking-widest uppercase mb-1">
        ✨ Magical Underwater Action Adventure ✨
      </div>
      <h1 class="text-3xl sm:text-5xl md:text-6xl font-bold text-transparent bg-clip-text bg-gradient-to-b from-white via-pink-200 to-cyan-300 text-glow-pink leading-tight">
        JELLYFISH KINGDOM
      </h1>
      <div class="text-lg sm:text-2xl md:text-3xl font-bold tracking-wider text-cyan-300 text-glow-cyan -mt-1">
        OCEAN WARS
      </div>
    </div>

    <!-- Primary Play Buttons -->
    <div class="flex flex-wrap justify-center gap-3 sm:gap-4 mb-4 sm:mb-6 w-full max-w-lg">
      <button onclick="Game.continueGame()" id="btnPlayContinue" class="btn-magic pulse-glow px-7 py-3 sm:px-9 sm:py-3.5 text-base sm:text-xl flex-1 min-w-[160px]">
        ▶ PLAY GAME
      </button>
      <button onclick="UI.openModal('worldMapModal')" class="btn-ocean px-6 py-3 sm:px-7 sm:py-3.5 text-sm sm:text-lg flex-1 min-w-[150px]">
        🗺️ WORLD MAP
      </button>
    </div>

    <!-- Navigation Grid -->
    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2.5 sm:gap-3 w-full max-w-3xl">
      <button onclick="UI.openModal('characterModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">🪼</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-pink-200">HEROES (10)</span>
      </button>
      <button onclick="UI.openModal('weaponModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">🫧</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-cyan-200">WEAPONS (8)</span>
      </button>
      <button onclick="UI.openModal('upgradeModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">⚡</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-amber-200">UPGRADES</span>
      </button>
      <button onclick="UI.openModal('achievementsModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">🏆</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-purple-200">ACHIEVEMENTS</span>
      </button>
      <button onclick="UI.openModal('dailyModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer relative">
        <span id="dailyBadge" class="hidden absolute top-1.5 right-2 w-2.5 h-2.5 rounded-full bg-pink-500 animate-ping"></span>
        <span class="text-2xl sm:text-3xl">🎁</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-emerald-200">DAILY REWARDS</span>
      </button>
      <button onclick="UI.openModal('worldMapModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">🏰</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-rose-200">25 LEVELS</span>
      </button>
      <button onclick="UI.openModal('howToPlayModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">📖</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-sky-200">HOW TO PLAY</span>
      </button>
      <button onclick="UI.openModal('settingsModal')" class="glass-card p-2.5 sm:p-3.5 flex flex-col items-center justify-center gap-1 cursor-pointer">
        <span class="text-2xl sm:text-3xl">⚙️</span>
        <span class="title-font text-xs sm:text-sm font-semibold text-indigo-200">SETTINGS</span>
      </button>
    </div>
  </div>
</div>

<!-- ==================== GAMEPLAY HUD ==================== -->
<div id="gameHUD" class="ui-layer hidden flex flex-col justify-between p-2.5 sm:p-4">
  <!-- Top HUD Row -->
  <div class="flex items-start justify-between gap-2 w-full">
    <!-- Left: Player Vitals & Weapon -->
    <div class="pointer-auto glass-panel p-2.5 sm:p-3 rounded-2xl flex flex-col gap-1.5 min-w-[185px] sm:min-w-[245px]">
      <div class="flex items-center justify-between gap-2">
        <span id="hudHeroName" class="title-font text-xs sm:text-sm font-bold text-pink-200 truncate">Pink Warrior</span>
        <span id="hudLevelBadge" class="text-[10px] sm:text-xs px-2 py-0.5 rounded-full bg-cyan-500/30 border border-cyan-300/40 text-cyan-100 font-bold">LV 1</span>
      </div>
      <!-- HP Bar -->
      <div class="w-full">
        <div class="flex justify-between text-[10px] sm:text-xs font-bold mb-0.5">
          <span class="text-rose-300">❤️ HP</span>
          <span id="hudHpText" class="text-white">100 / 100</span>
        </div>
        <div class="w-full h-2.5 sm:h-3 bg-slate-900/80 rounded-full overflow-hidden border border-rose-400/40">
          <div id="hudHpBar" class="h-full bg-gradient-to-r from-rose-500 via-pink-500 to-amber-400 transition-all duration-150" style="width: 100%"></div>
        </div>
      </div>
      <!-- Energy Bar -->
      <div class="w-full">
        <div class="flex justify-between text-[10px] sm:text-xs font-bold mb-0.5">
          <span class="text-cyan-300">⚡ ENERGY</span>
          <span id="hudEnergyText" class="text-cyan-100">100 / 100</span>
        </div>
        <div class="w-full h-2 sm:h-2.5 bg-slate-900/80 rounded-full overflow-hidden border border-cyan-400/40">
          <div id="hudEnergyBar" class="h-full bg-gradient-to-r from-blue-500 via-cyan-400 to-teal-300 transition-all duration-150" style="width: 100%"></div>
        </div>
      </div>
      <!-- Active Weapon & Buffs -->
      <div class="flex items-center justify-between pt-0.5 border-t border-white/10 text-[10px] sm:text-xs">
        <button onclick="Game.cycleWeapon()" class="flex items-center gap-1 text-amber-200 hover:text-white font-bold cursor-pointer" title="Switch Weapon (Q)">
          <span id="hudWeaponIcon">🫧</span>
          <span id="hudWeaponName" class="truncate max-w-[110px] sm:max-w-[140px]">Bubble Blaster</span>
          <span class="text-[9px] bg-white/15 px-1 rounded">Q</span>
        </button>
        <div id="hudBuffs" class="flex items-center gap-1"></div>
      </div>
    </div>

    <!-- Center: Mission Objective & Boss Health Bar -->
    <div class="flex flex-col items-center gap-1.5 flex-1 max-w-md mx-auto">
      <div class="glass-panel px-3 py-1.5 sm:px-4 sm:py-2 rounded-2xl text-center w-full">
        <div id="hudWorldTitle" class="text-[10px] sm:text-xs font-bold uppercase tracking-wider text-cyan-300">World 1: Coral Kingdom</div>
        <div id="hudMissionText" class="text-xs sm:text-sm font-bold text-white leading-snug">Defeat Invading Jellyfish: 0/8</div>
        <div class="w-full h-1.5 bg-slate-900/80 rounded-full overflow-hidden mt-1">
          <div id="hudMissionBar" class="h-full bg-gradient-to-r from-emerald-400 to-cyan-400 transition-all duration-200" style="width: 0%"></div>
        </div>
      </div>

      <!-- Boss Bar (Hidden unless boss active) -->
      <div id="hudBossContainer" class="hidden glass-panel px-3.5 py-2 rounded-2xl w-full border-rose-500/70 bg-rose-950/75">
        <div class="flex justify-between items-center text-xs sm:text-sm font-bold mb-1">
          <span id="hudBossName" class="text-rose-200 title-font tracking-wide">👑 DARK JELLYFISH KING</span>
          <span id="hudBossHpText" class="text-amber-300">100%</span>
        </div>
        <div class="w-full h-3 sm:h-3.5 bg-slate-950 rounded-full overflow-hidden border border-rose-400/60">
          <div id="hudBossBar" class="h-full bg-gradient-to-r from-purple-600 via-rose-500 to-amber-500 transition-all duration-150" style="width: 100%"></div>
        </div>
      </div>
    </div>

    <!-- Right: Score, Currencies & Pause -->
    <div class="pointer-auto flex flex-col items-end gap-1.5">
      <div class="flex items-center gap-1.5">
        <div class="glass-panel px-2.5 py-1 sm:px-3 sm:py-1.5 rounded-xl text-right hidden sm:block">
          <div class="text-[10px] text-cyan-300 font-bold">SCORE</div>
          <div id="hudScore" class="text-xs sm:text-sm font-bold text-white">0</div>
        </div>
        <div class="glass-panel px-2.5 py-1 sm:px-3 sm:py-1.5 rounded-xl flex items-center gap-2 text-xs font-bold">
          <span>⚪ <span id="hudPearls">0</span></span>
          <span>💎 <span id="hudCrystals">0</span></span>
        </div>
        <button onclick="Game.togglePause()" class="glass-panel p-2 sm:p-2.5 rounded-xl hover:border-pink-400 text-white font-bold text-sm sm:text-base cursor-pointer" title="Pause (ESC)">
          ⏸️
        </button>
      </div>

      <!-- Desktop Quick Ability Bar -->
      <div class="hidden md:flex items-center gap-1.5 mt-1">
        <button onclick="Game.triggerSpecial()" class="glass-panel px-2.5 py-1.5 rounded-xl text-xs font-bold text-fuchsia-200 hover:border-fuchsia-400 cursor-pointer" title="Special Attack (Space)">
          ✨ Special [Space]
        </button>
        <button onclick="Game.triggerShield()" class="glass-panel px-2.5 py-1.5 rounded-xl text-xs font-bold text-cyan-200 hover:border-cyan-400 cursor-pointer" title="Bubble Shield (E)">
          🛡️ Shield [E]
        </button>
        <button onclick="Game.triggerDash()" class="glass-panel px-2.5 py-1.5 rounded-xl text-xs font-bold text-amber-200 hover:border-amber-400 cursor-pointer" title="Aqua Dash (Shift)">
          💨 Dash [Shift]
        </button>
      </div>
    </div>
  </div>

  <!-- Boss Cinematic Intro Banner -->
  <div id="bossBanner" class="hidden self-center my-auto glass-panel px-6 py-4 sm:px-10 sm:py-5 border-2 border-rose-400 text-center animate-bounce">
    <div class="text-xs sm:text-sm font-bold uppercase tracking-widest text-rose-300">⚠️ BOSS WARNING ⚠️</div>
    <div id="bossBannerTitle" class="title-font text-2xl sm:text-4xl font-bold text-white text-glow-pink mt-1">DARK JELLYFISH KING</div>
  </div>

  <!-- Bottom Touch Controls (Mobile / Tablet / Touch-Enabled) -->
  <div id="touchControls" class="flex justify-between items-end w-full pb-1 sm:pb-2 md:hidden">
    <!-- Left Virtual Joystick -->
    <div id="joystickZone" class="pointer-auto relative w-32 h-32 sm:w-36 sm:h-36 rounded-full bg-cyan-950/35 border-2 border-cyan-300/35 backdrop-blur-xs flex items-center justify-center">
      <div id="joystickKnob" class="w-14 h-14 rounded-full bg-gradient-to-br from-pink-400/80 to-cyan-400/80 border-2 border-white/80 shadow-lg pointer-events-none"></div>
    </div>

    <!-- Right Action Buttons Cluster -->
    <div class="pointer-auto relative w-44 h-36 sm:w-48 sm:h-40">
      <!-- Shoot (Big Primary) -->
      <button id="btnTouchShoot" class="touch-btn w-18 h-18 sm:w-20 sm:h-20 right-1 bottom-1 absolute text-2xl" style="width:72px;height:72px;">
        🫧
      </button>
      <!-- Special Attack -->
      <button id="btnTouchSpecial" class="touch-btn w-13 h-13 right-20 bottom-1 absolute text-lg" style="width:52px;height:52px;">
        ✨
      </button>
      <!-- Shield -->
      <button id="btnTouchShield" class="touch-btn w-12 h-12 right-16 bottom-16 absolute text-base" style="width:48px;height:48px;">
        🛡️
      </button>
      <!-- Dash -->
      <button id="btnTouchDash" class="touch-btn w-12 h-12 right-2 bottom-20 absolute text-base" style="width:48px;height:48px;">
        💨
      </button>
      <!-- Weapon Switch -->
      <button id="btnTouchWeapon" class="touch-btn w-10 h-10 right-28 bottom-14 absolute text-sm" style="width:42px;height:42px;">
        🔄
      </button>
    </div>
  </div>
</div>

<!-- ==================== MODALS CONTAINER ==================== -->
<div id="modalBackdrop" class="ui-layer hidden bg-slate-950/65 backdrop-blur-sm flex items-center justify-center p-3 sm:p-5 z-30">

  <!-- 1. CHARACTER SELECTION MODAL (10 HEROES) -->
  <div id="characterModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-5xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-pink-300">🪼 JELLYFISH HEROES</h2>
        <p class="text-xs sm:text-sm text-cyan-200">Select or unlock your underwater champion</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div id="characterGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3 overflow-y-auto custom-scroll py-3 pr-1"></div>
  </div>

  <!-- 2. WEAPON INVENTORY & UPGRADES MODAL (8 WEAPONS) -->
  <div id="weaponModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-5xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-cyan-300">🫧 MAGICAL WEAPON ARMORY</h2>
        <p class="text-xs sm:text-sm text-pink-200">Equip & upgrade weapons up to Level 5</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div id="weaponGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3 overflow-y-auto custom-scroll py-3 pr-1"></div>
  </div>

  <!-- 3. CHARACTER UPGRADES MODAL -->
  <div id="upgradeModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-4xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-amber-300">⚡ ROYAL ATTRIBUTE UPGRADES</h2>
        <p class="text-xs sm:text-sm text-cyan-200">Enhance all jellyfish heroes permanently (Max Level 5)</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div id="upgradeGrid" class="grid grid-cols-1 sm:grid-cols-2 gap-3 overflow-y-auto custom-scroll py-3 pr-1"></div>
  </div>

  <!-- 4. WORLD MAP & 25 LEVELS MODAL -->
  <div id="worldMapModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-5xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-cyan-300">🗺️ OCEAN KINGDOMS & 25 LEVELS</h2>
        <p class="text-xs sm:text-sm text-pink-200">Conquer all 5 worlds and defeat the Dark Ocean Emperor</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <!-- World Tabs -->
    <div id="worldTabs" class="flex gap-2 overflow-x-auto py-2.5 border-b border-white/10"></div>
    <!-- Selected World Levels -->
    <div id="levelsGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3 overflow-y-auto custom-scroll py-3 pr-1"></div>
  </div>

  <!-- 5. ACHIEVEMENTS MODAL -->
  <div id="achievementsModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-4xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-purple-300">🏆 HEROIC ACHIEVEMENTS</h2>
        <p class="text-xs sm:text-sm text-cyan-200">Complete milestones to earn Pearls, Golden Pearls & Crystals</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div id="achievementsGrid" class="grid grid-cols-1 sm:grid-cols-2 gap-3 overflow-y-auto custom-scroll py-3 pr-1"></div>
  </div>

  <!-- 6. DAILY REWARDS & CHALLENGES MODAL -->
  <div id="dailyModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-3xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <div>
        <h2 class="title-font text-xl sm:text-3xl font-bold text-emerald-300">🎁 DAILY TREASURES & CHALLENGES</h2>
        <p class="text-xs sm:text-sm text-cyan-200">Claim daily royal gifts and complete bonus ocean quests</p>
      </div>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div id="dailyContent" class="overflow-y-auto custom-scroll py-3 space-y-4"></div>
  </div>

  <!-- 7. SETTINGS MODAL -->
  <div id="settingsModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-lg flex flex-col p-5 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <h2 class="title-font text-xl sm:text-2xl font-bold text-cyan-300">⚙️ OCEAN SETTINGS</h2>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div class="py-4 space-y-4">
      <div>
        <div class="flex justify-between text-sm font-bold mb-1">
          <span>🎵 Underwater Music Volume</span>
          <span id="musicVolLabel">50%</span>
        </div>
        <input id="musicVolSlider" type="range" min="0" max="100" value="50" oninput="AudioEngine.setMusicVolume(this.value)" class="w-full accent-pink-500 cursor-pointer">
      </div>
      <div>
        <div class="flex justify-between text-sm font-bold mb-1">
          <span>🔊 Sound Effects Volume</span>
          <span id="sfxVolLabel">70%</span>
        </div>
        <input id="sfxVolSlider" type="range" min="0" max="100" value="70" oninput="AudioEngine.setSfxVolume(this.value)" class="w-full accent-cyan-400 cursor-pointer">
      </div>
      <div class="flex items-center justify-between pt-2">
        <span class="text-sm font-bold">📳 Screen Shake Effects</span>
        <button id="btnToggleShake" onclick="Settings.toggleShake()" class="btn-ocean px-4 py-1.5 text-xs">ENABLED</button>
      </div>
      <div class="pt-4 border-t border-white/15 flex flex-col gap-2">
        <button id="btnResetSave" onclick="Storage.promptReset()" class="w-full py-2.5 rounded-xl bg-rose-600/80 hover:bg-rose-500 border border-rose-300/50 text-white font-bold text-sm cursor-pointer">
          🗑️ Reset Save Profile
        </button>
        <div id="resetConfirmBox" class="hidden bg-rose-950/90 border border-rose-400 rounded-xl p-3 text-center space-y-2">
          <div class="text-xs text-rose-200 font-bold">Are you sure? All unlocked heroes, weapons & levels will be erased!</div>
          <div class="flex gap-2 justify-center">
            <button onclick="Storage.confirmReset()" class="px-4 py-1 rounded-lg bg-rose-600 text-white text-xs font-bold cursor-pointer">Yes, Reset Everything</button>
            <button onclick="Storage.cancelReset()" class="px-4 py-1 rounded-lg bg-slate-700 text-white text-xs font-bold cursor-pointer">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 8. HOW TO PLAY MODAL -->
  <div id="howToPlayModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-3xl max-h-[88vh] flex flex-col p-4 sm:p-6">
    <div class="flex justify-between items-center pb-3 border-b border-white/15">
      <h2 class="title-font text-xl sm:text-3xl font-bold text-sky-300">📖 HOW TO PLAY</h2>
      <button onclick="UI.closeModals()" class="btn-ocean px-3.5 py-1.5 text-sm">✕ Close</button>
    </div>
    <div class="overflow-y-auto custom-scroll py-3 space-y-3 text-xs sm:text-sm text-cyan-50">
      <div class="glass-card p-3">
        <h3 class="title-font text-base font-bold text-pink-300 mb-1">🖥️ Desktop Controls</h3>
        <ul class="grid grid-cols-1 sm:grid-cols-2 gap-1.5 text-cyan-100">
          <li>• <b>W A S D / Arrow Keys:</b> Swim in any direction</li>
          <li>• <b>Mouse Cursor:</b> Aim magical projectiles</li>
          <li>• <b>Left Click (Hold):</b> Fire primary weapon</li>
          <li>• <b>Spacebar:</b> Hero Special Ability</li>
          <li>• <b>E Key:</b> Activate Protective Bubble Shield</li>
          <li>• <b>Shift Key:</b> Aqua Speed Dash</li>
          <li>• <b>Q or 1-8 Keys:</b> Switch unlocked weapons</li>
          <li>• <b>ESC Key:</b> Pause / Resume battle</li>
        </ul>
      </div>
      <div class="glass-card p-3">
        <h3 class="title-font text-base font-bold text-cyan-300 mb-1">📱 Mobile & Tablet Controls</h3>
        <p>Use the <b>Left Virtual Joystick</b> to swim smoothly. Tap or hold the right-side action buttons to <b>Shoot (🫧)</b>, unleash your <b>Special Attack (✨)</b>, raise a <b>Bubble Shield (🛡️)</b>, <b>Dash (💨)</b>, or <b>Switch Weapons (🔄)</b> simultaneously!</p>
      </div>
      <div class="glass-card p-3">
        <h3 class="title-font text-base font-bold text-amber-300 mb-1">💎 Currencies & Progression</h3>
        <p>Defeat enemies and bosses to collect <b>Pearls (⚪)</b>, <b>Golden Pearls (🟡)</b>, and <b>Magical Crystals (💎)</b>. Spend them in the Armory, Hero Sanctuary, and Upgrade Workshop to unlock all 10 Jellyfish Warriors and 8 Weapons!</p>
      </div>
    </div>
  </div>

  <!-- 9. PAUSE MODAL -->
  <div id="pauseModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-sm flex flex-col items-center text-center p-6 space-y-3">
    <div class="text-4xl">⏸️</div>
    <h2 class="title-font text-2xl sm:text-3xl font-bold text-cyan-200">GAME PAUSED</h2>
    <p class="text-xs text-pink-200">Take a breath beneath the coral waves</p>
    <div class="w-full flex flex-col gap-2.5 pt-2">
      <button onclick="Game.togglePause()" class="btn-magic py-3 text-base w-full">▶ RESUME BATTLE</button>
      <button onclick="Game.restartLevel()" class="btn-ocean py-2.5 text-sm w-full">🔄 RESTART LEVEL</button>
      <button onclick="UI.openModal('settingsModal')" class="btn-ocean py-2.5 text-sm w-full">⚙️ SETTINGS</button>
      <button onclick="Game.exitToMenu()" class="btn-ocean py-2.5 text-sm w-full border-rose-400/50 text-rose-200">🏰 MAIN MENU</button>
    </div>
  </div>

  <!-- 10. VICTORY / DEFEAT RESULT MODAL -->
  <div id="resultModal" class="modal-box hidden pointer-auto glass-panel w-full max-w-md flex flex-col items-center text-center p-6 space-y-4">
    <div id="resultEmoji" class="text-5xl animate-bounce">🏆</div>
    <h2 id="resultTitle" class="title-font text-2xl sm:text-4xl font-bold text-amber-300 text-glow-pink">VICTORY!</h2>
    <p id="resultSubtitle" class="text-xs sm:text-sm text-cyan-100">The Coral Kingdom is safe once more!</p>

    <div class="w-full glass-card p-3.5 space-y-2 text-sm">
      <div class="flex justify-between">
        <span class="text-cyan-200">Final Score:</span>
        <span id="resScore" class="font-bold text-white">0</span>
      </div>
      <div class="flex justify-between">
        <span class="text-cyan-200">Pearls Earned:</span>
        <span id="resPearls" class="font-bold text-cyan-300">+0 ⚪</span>
      </div>
      <div class="flex justify-between">
        <span class="text-cyan-200">Golden Pearls:</span>
        <span id="resGolden" class="font-bold text-amber-300">+0 🟡</span>
      </div>
      <div class="flex justify-between">
        <span class="text-cyan-200">Magical Crystals:</span>
        <span id="resCrystals" class="font-bold text-fuchsia-300">+0 💎</span>
      </div>
    </div>

    <div class="w-full flex flex-col sm:flex-row gap-2.5 pt-1">
      <button id="btnNextLevel" onclick="Game.nextLevel()" class="btn-magic py-3 px-5 text-sm sm:text-base flex-1">NEXT LEVEL ▶</button>
      <button onclick="Game.restartLevel()" class="btn-ocean py-3 px-4 text-sm flex-1">🔄 REPLAY</button>
      <button onclick="Game.exitToMenu()" class="btn-ocean py-3 px-4 text-sm flex-1">🏰 MENU</button>
    </div>
  </div>

</div>

<script>
/**
 * ============================================================================
 * JELLYFISH KINGDOM – OCEAN WARS
 * Modular Single-File HTML5 Canvas Action-Adventure Architecture
 * ============================================================================
 */

/* -------------------- 1. GAME DATA & CONFIGURATION -------------------- */

const HEROES = [
  {
    id: 'pink_warrior',
    name: 'Pink Warrior',
    title: 'Princess of Coral Reef',
    color1: '#ff77cc',
    color2: '#d946ef',
    glow: '#ff9ce3',
    eyeColor: '#2e1065',
    hp: 120,
    speed: 265,
    damage: 1.0,
    energy: 100,
    specialName: 'Heart Bubble Nova',
    specialType: 'nova',
    unlockCost: { type: 'free', amount: 0 },
    desc: 'Agile, balanced protector of the Coral Kingdom with radiant pink energy.'
  },
  {
    id: 'purple_guardian',
    name: 'Purple Guardian',
    title: 'Royal Citadel Defender',
    color1: '#c084fc',
    color2: '#6b21a8',
    glow: '#d8b4fe',
    eyeColor: '#1e1b4b',
    hp: 165,
    speed: 240,
    damage: 1.1,
    energy: 115,
    specialName: 'Aegis Dome Shield',
    specialType: 'shield_burst',
    unlockCost: { type: 'pearls', amount: 180 },
    desc: 'High durability guardian whose special unleashes a defensive shockwave.'
  },
  {
    id: 'blue_knight',
    name: 'Blue Ocean Knight',
    title: 'Tidal Vanguard',
    color1: '#38bdf8',
    color2: '#1d4ed8',
    glow: '#7dd3fc',
    eyeColor: '#082f49',
    hp: 135,
    speed: 310,
    damage: 1.15,
    energy: 110,
    specialName: 'Tidal Wave Slash',
    specialType: 'wave',
    unlockCost: { type: 'pearls', amount: 320 },
    desc: 'Swift warrior slicing through ocean currents with high-velocity wave blasts.'
  },
  {
    id: 'golden_jelly',
    name: 'Golden Jellyfish',
    title: 'Sunbeam Alchemist',
    color1: '#fde047',
    color2: '#d97706',
    glow: '#fef08a',
    eyeColor: '#451a03',
    hp: 145,
    speed: 275,
    damage: 1.25,
    energy: 125,
    specialName: 'Solar Treasure Rain',
    specialType: 'rain',
    unlockCost: { type: 'golden', amount: 8 },
    desc: 'Radiates golden light, attracts treasures from afar, and calls down solar spears.'
  },
  {
    id: 'rainbow_jelly',
    name: 'Rainbow Jellyfish',
    title: 'Prismatic Spirit',
    color1: '#f472b6',
    color2: '#22d3ee',
    glow: '#a5f3fc',
    eyeColor: '#1e1b4b',
    rainbow: true,
    hp: 155,
    speed: 295,
    damage: 1.35,
    energy: 135,
    specialName: 'Prismatic Vortex',
    specialType: 'tornado',
    unlockCost: { type: 'pearls', amount: 650 },
    desc: 'Shimmering multi-hued spirit summoning swirling prismatic ocean whirlpools.'
  },
  {
    id: 'crystal_jelly',
    name: 'Crystal Jellyfish',
    title: 'Abyssal Gem Weaver',
    color1: '#67e8f9',
    color2: '#0891b2',
    glow: '#cffafe',
    eyeColor: '#164e63',
    hp: 150,
    speed: 270,
    damage: 1.48,
    energy: 140,
    specialName: 'Crystal Shatter Storm',
    specialType: 'crystal_rain',
    unlockCost: { type: 'crystals', amount: 35 },
    desc: 'Crystalline carapace amplifies weapon power and fires razor-sharp shards.'
  },
  {
    id: 'electric_jelly',
    name: 'Electric Jellyfish',
    title: 'Tempest Volt Caster',
    color1: '#fef08a',
    color2: '#0284c7',
    glow: '#38bdf8',
    eyeColor: '#0c4a6e',
    hp: 140,
    speed: 325,
    damage: 1.45,
    energy: 150,
    specialName: 'Lightning Chain Storm',
    specialType: 'lightning',
    unlockCost: { type: 'pearls', amount: 950 },
    desc: 'Crackles with high-voltage bio-electricity that zaps multiple nearby foes.'
  },
  {
    id: 'shadow_jelly',
    name: 'Shadow Jellyfish',
    title: 'Phantom of the Trench',
    color1: '#a855f7',
    color2: '#1e1b4b',
    glow: '#c084fc',
    eyeColor: '#f43f5e',
    hp: 160,
    speed: 335,
    damage: 1.65,
    energy: 145,
    specialName: 'Void Eclipse Nova',
    specialType: 'nova',
    unlockCost: { type: 'crystals', amount: 75 },
    desc: 'Master of dark abyssal currents dealing devastating critical strikes.'
  },
  {
    id: 'royal_jelly',
    name: 'Royal Jellyfish',
    title: 'Sovereign of the Seas',
    color1: '#fb7185',
    color2: '#7e22ce',
    glow: '#fda4af',
    eyeColor: '#4c0519',
    crown: true,
    hp: 200,
    speed: 305,
    damage: 1.75,
    energy: 170,
    specialName: 'Royal Decree Wave',
    specialType: 'wave',
    unlockCost: { type: 'golden', amount: 22 },
    desc: 'Crowned monarch of the underwater realm with majestic firepower.'
  },
  {
    id: 'legendary_guardian',
    name: 'Legendary Ocean Guardian',
    title: 'Celestial Leviathan Soul',
    color1: '#34d399',
    color2: '#6366f1',
    glow: '#6ee7b7',
    eyeColor: '#064e3b',
    crown: true,
    rainbow: true,
    hp: 250,
    speed: 345,
    damage: 2.1,
    energy: 200,
    specialName: 'Celestial Ocean Apocalypse',
    specialType: 'lightning',
    unlockCost: { type: 'golden', amount: 40 },
    desc: 'Mythic guardian uniting all five ocean elements into unstoppable harmony.'
  }
];

const WEAPONS = [
  {
    id: 'bubble_blaster',
    name: 'Bubble Blaster',
    icon: '🫧',
    color: '#67e8f9',
    coreColor: '#ffffff',
    damage: 18,
    fireRate: 0.22,
    speed: 560,
    size: 8,
    spread: 1,
    pierce: 0,
    unlockCost: { type: 'free', amount: 0 },
    desc: 'Rapid-fire pressurized magical bubbles.'
  },
  {
    id: 'crystal_shooter',
    name: 'Crystal Shooter',
    icon: '💎',
    color: '#a5f3fc',
    coreColor: '#38bdf8',
    damage: 26,
    fireRate: 0.26,
    speed: 680,
    size: 9,
    spread: 1,
    pierce: 1,
    unlockCost: { type: 'pearls', amount: 150 },
    desc: 'High-velocity crystal shards that pierce through an enemy.'
  },
  {
    id: 'electric_ray',
    name: 'Electric Ray Gun',
    icon: '⚡',
    color: '#fde047',
    coreColor: '#38bdf8',
    damage: 22,
    fireRate: 0.16,
    speed: 760,
    size: 7,
    spread: 1,
    pierce: 0,
    chain: true,
    unlockCost: { type: 'pearls', amount: 300 },
    desc: 'Ultra-fast bio-electric bolts that arc to nearby targets.'
  },
  {
    id: 'coral_cannon',
    name: 'Coral Cannon',
    icon: '🪸',
    color: '#fb7185',
    coreColor: '#fda4af',
    damage: 16,
    fireRate: 0.34,
    speed: 520,
    size: 8,
    spread: 3,
    pierce: 0,
    unlockCost: { type: 'pearls', amount: 480 },
    desc: 'Fires a 3-way spread of enchanted coral blossoms.'
  },
  {
    id: 'pearl_launcher',
    name: 'Pearl Launcher',
    icon: '⚪',
    color: '#f8fafc',
    coreColor: '#f472b6',
    damage: 48,
    fireRate: 0.48,
    speed: 490,
    size: 14,
    spread: 1,
    splash: 75,
    unlockCost: { type: 'crystals', amount: 25 },
    desc: 'Launches heavy luminous pearls that detonate on impact.'
  },
  {
    id: 'plasma_bubble',
    name: 'Plasma Bubble Gun',
    icon: '🔮',
    color: '#e879f9',
    coreColor: '#c084fc',
    damage: 32,
    fireRate: 0.24,
    speed: 610,
    size: 11,
    spread: 2,
    pierce: 1,
    unlockCost: { type: 'crystals', amount: 45 },
    desc: 'Twin ionized plasma orbs with strong piercing force.'
  },
  {
    id: 'ocean_laser',
    name: 'Ocean Laser',
    icon: '🌊',
    color: '#2dd4bf',
    coreColor: '#ccfbf1',
    damage: 28,
    fireRate: 0.13,
    speed: 900,
    size: 9,
    spread: 1,
    pierce: 2,
    unlockCost: { type: 'golden', amount: 12 },
    desc: 'Concentrated hydro-photonic beam slicing through enemy ranks.'
  },
  {
    id: 'royal_blaster',
    name: 'Royal Energy Blaster',
    icon: '👑',
    color: '#fbbf24',
    coreColor: '#ff6ec7',
    damage: 38,
    fireRate: 0.21,
    speed: 720,
    size: 12,
    spread: 3,
    pierce: 2,
    splash: 55,
    unlockCost: { type: 'golden', amount: 25 },
    desc: 'Supreme royal tri-beam infused with starlight and ocean fury.'
  }
];

const UPGRADE_CATEGORIES = [
  { id: 'maxHp', name: 'Maximum Health', icon: '❤️', desc: '+25 Max HP per level', baseCost: 100 },
  { id: 'speed', name: 'Swimming Speed', icon: '💨', desc: '+18 Swim Speed per level', baseCost: 100 },
  { id: 'damage', name: 'Attack Damage', icon: '⚔️', desc: '+15% Weapon Damage per level', baseCost: 120 },
  { id: 'energy', name: 'Energy Capacity', icon: '⚡', desc: '+20 Max Energy & faster regen', baseCost: 100 },
  { id: 'shield', name: 'Shield Strength', icon: '🛡️', desc: '+1.2s Bubble Shield duration', baseCost: 110 },
  { id: 'special', name: 'Special Attack Power', icon: '✨', desc: '+22% Special Ability damage', baseCost: 130 }
];

const WORLDS = [
  {
    id: 1,
    name: 'Coral Kingdom',
    subtitle: 'Sunlit Gardens & Royal Pink Castle',
    bgTop: '#0b4f8a',
    bgMid: '#0d7490',
    bgBot: '#1e1b4b',
    rayColor: 'rgba(165, 243, 252, 0.14)',
    castleTint: '#ff77cc',
    coralColors: ['#ff6ec7', '#fb923c', '#c084fc', '#fde047', '#34d399']
  },
  {
    id: 2,
    name: 'Crystal Ocean',
    subtitle: 'Luminous Cavern & Prism Tunnels',
    bgTop: '#0c4a6e',
    bgMid: '#1d4ed8',
    bgBot: '#172554',
    rayColor: 'rgba(103, 232, 249, 0.18)',
    castleTint: '#38bdf8',
    coralColors: ['#38bdf8', '#818cf8', '#22d3ee', '#e879f9', '#a5f3fc']
  },
  {
    id: 3,
    name: 'Deep Sea Battlefield',
    subtitle: 'Abyssal Trench & Bioluminescent Ruins',
    bgTop: '#082f49',
    bgMid: '#0f172a',
    bgBot: '#090d16',
    rayColor: 'rgba(45, 212, 191, 0.09)',
    castleTint: '#2dd4bf',
    coralColors: ['#2dd4bf', '#3b82f6', '#6366f1', '#14b8a6', '#a855f7']
  },
  {
    id: 4,
    name: 'Rainbow Reef',
    subtitle: 'Enchanted Flora & Glowing Mushrooms',
    bgTop: '#155e75',
    bgMid: '#6d28d9',
    bgBot: '#311042',
    rayColor: 'rgba(244, 114, 182, 0.16)',
    castleTint: '#f472b6',
    coralColors: ['#f43f5e', '#fbbf24', '#34d399', '#38bdf8', '#e879f9']
  },
  {
    id: 5,
    name: 'Dark Ocean Empire',
    subtitle: 'Shadow Fortress of the Dark Emperor',
    bgTop: '#2e1065',
    bgMid: '#4c0519',
    bgBot: '#090514',
    rayColor: 'rgba(244, 63, 94, 0.12)',
    castleTint: '#9333ea',
    coralColors: ['#be123c', '#7e22ce', '#4338ca', '#e11d48', '#a21caf']
  }
];

const LEVELS = [
  // WORLD 1: CORAL KINGDOM
  { id: 1, world: 1, title: 'Defend the Coral Garden', type: 'eliminate', target: 8, enemyPool: ['evil_pink', 'clownfish'], reward: { pearls: 60, golden: 0, crystals: 2 } },
  { id: 2, world: 1, title: 'Invading Jellyfish Swarm', type: 'eliminate', target: 12, enemyPool: ['evil_pink', 'dark_purple', 'pufferfish'], reward: { pearls: 80, golden: 0, crystals: 3 } },
  { id: 3, world: 1, title: 'Rescue Baby Jellyfish', type: 'rescue', target: 4, enemyPool: ['evil_pink', 'dark_purple', 'sea_urchin'], reward: { pearls: 100, golden: 1, crystals: 4 } },
  { id: 4, world: 1, title: 'Protect the Royal Castle', type: 'eliminate', target: 16, enemyPool: ['dark_purple', 'electric_jelly', 'coral_monster'], reward: { pearls: 120, golden: 1, crystals: 5 } },
  { id: 5, world: 1, title: 'Dark Jellyfish King', type: 'boss', bossId: 'dark_jelly_king', enemyPool: ['evil_pink', 'electric_jelly'], reward: { pearls: 220, golden: 3, crystals: 10 } },

  // WORLD 2: CRYSTAL OCEAN
  { id: 6, world: 2, title: 'Shatter Crystal Machines', type: 'destroy', target: 4, enemyPool: ['crystal_monster', 'electric_jelly'], reward: { pearls: 140, golden: 1, crystals: 6 } },
  { id: 7, world: 2, title: 'Prism Tunnel Ambush', type: 'eliminate', target: 18, enemyPool: ['crystal_monster', 'dark_purple', 'sea_urchin'], reward: { pearls: 160, golden: 1, crystals: 8 } },
  { id: 8, world: 2, title: 'Crystal Guardian Trial', type: 'eliminate', target: 20, enemyPool: ['crystal_monster', 'armoured_puffer', 'squid'], reward: { pearls: 180, golden: 2, crystals: 10 } },
  { id: 9, world: 2, title: 'Rescue Crystal Miners', type: 'rescue', target: 5, enemyPool: ['crystal_monster', 'electric_jelly', 'coral_monster'], reward: { pearls: 200, golden: 2, crystals: 12 } },
  { id: 10, world: 2, title: 'Crystal Sea Monster', type: 'boss', bossId: 'crystal_monster_boss', enemyPool: ['crystal_monster', 'sea_urchin'], reward: { pearls: 320, golden: 4, crystals: 18 } },

  // WORLD 3: DEEP SEA BATTLEFIELD
  { id: 11, world: 3, title: 'Shark Vanguard Assault', type: 'eliminate', target: 20, enemyPool: ['shark_warrior', 'squid', 'dark_purple'], reward: { pearls: 230, golden: 2, crystals: 12 } },
  { id: 12, world: 3, title: 'Destroy Abyssal Outposts', type: 'destroy', target: 5, enemyPool: ['shark_warrior', 'armoured_puffer', 'deep_predator'], reward: { pearls: 250, golden: 2, crystals: 14 } },
  { id: 13, world: 3, title: 'Trench Predator Gauntlet', type: 'eliminate', target: 24, enemyPool: ['deep_predator', 'squid', 'shadow_jelly'], reward: { pearls: 270, golden: 3, crystals: 15 } },
  { id: 14, world: 3, title: 'Rescue Trapped Dolphins', type: 'rescue', target: 5, enemyPool: ['shark_warrior', 'deep_predator', 'electric_jelly'], reward: { pearls: 290, golden: 3, crystals: 16 } },
  { id: 15, world: 3, title: 'Shark Commander', type: 'boss', bossId: 'shark_commander', enemyPool: ['shark_warrior', 'squid'], reward: { pearls: 450, golden: 6, crystals: 22 } },

  // WORLD 4: RAINBOW REEF
  { id: 16, world: 4, title: 'Defend the Rainbow Reef', type: 'eliminate', target: 24, enemyPool: ['coral_monster', 'giant_octopus', 'clownfish'], reward: { pearls: 310, golden: 3, crystals: 18 } },
  { id: 17, world: 4, title: 'Bioluminescent Squads', type: 'eliminate', target: 26, enemyPool: ['electric_jelly', 'shadow_jelly', 'armoured_puffer'], reward: { pearls: 330, golden: 4, crystals: 20 } },
  { id: 18, world: 4, title: 'Destroy Enemy Submarines', type: 'destroy', target: 5, enemyPool: ['giant_octopus', 'shark_warrior', 'coral_monster'], reward: { pearls: 360, golden: 4, crystals: 22 } },
  { id: 19, world: 4, title: 'Rescue Reef Guardians', type: 'rescue', target: 6, enemyPool: ['giant_octopus', 'shadow_jelly', 'squid'], reward: { pearls: 390, golden: 5, crystals: 24 } },
  { id: 20, world: 4, title: 'Mega Octopus', type: 'boss', bossId: 'mega_octopus', enemyPool: ['giant_octopus', 'squid'], reward: { pearls: 600, golden: 8, crystals: 30 } },

  // WORLD 5: DARK OCEAN EMPIRE
  { id: 21, world: 5, title: 'Infiltrate Dark Empire', type: 'eliminate', target: 28, enemyPool: ['shadow_jelly', 'deep_predator', 'dark_purple'], reward: { pearls: 420, golden: 5, crystals: 25 } },
  { id: 22, world: 5, title: 'Shadow Legion Commanders', type: 'eliminate', target: 30, enemyPool: ['shadow_jelly', 'shark_warrior', 'giant_octopus'], reward: { pearls: 460, golden: 6, crystals: 28 } },
  { id: 23, world: 5, title: 'Shatter Fortress Towers', type: 'destroy', target: 6, enemyPool: ['shadow_jelly', 'crystal_monster', 'armoured_puffer'], reward: { pearls: 500, golden: 7, crystals: 30 } },
  { id: 24, world: 5, title: 'Rescue Royal Jellyfish', type: 'rescue', target: 6, enemyPool: ['shadow_jelly', 'deep_predator', 'giant_octopus'], reward: { pearls: 550, golden: 8, crystals: 35 } },
  { id: 25, world: 5, title: 'The Dark Ocean Emperor', type: 'boss', bossId: 'dark_emperor', enemyPool: ['shadow_jelly', 'deep_predator'], reward: { pearls: 1000, golden: 15, crystals: 50 } }
];

const ENEMY_TYPES = {
  evil_pink: { name: 'Evil Pink Jelly', hp: 34, speed: 105, radius: 20, damage: 10, color: '#f43f5e', color2: '#9f1239', ai: 'sine', shoots: false, pearls: 8, score: 100 },
  dark_purple: { name: 'Dark Purple Jelly', hp: 48, speed: 95, radius: 22, damage: 12, color: '#9333ea', color2: '#3b0764', ai: 'chase', shoots: true, fireRate: 2.6, pearls: 11, score: 140 },
  electric_jelly: { name: 'Electric Jelly', hp: 44, speed: 145, radius: 20, damage: 14, color: '#facc15', color2: '#0284c7', ai: 'dash', shoots: true, fireRate: 2.3, pearls: 13, score: 160 },
  pufferfish: { name: 'Angry Pufferfish', hp: 56, speed: 80, radius: 23, damage: 15, color: '#fb923c', color2: '#9a3412', ai: 'puff', shoots: true, fireRate: 2.8, pearls: 12, score: 150 },
  sea_urchin: { name: 'Poison Sea Urchin', hp: 42, speed: 55, radius: 21, damage: 16, color: '#a855f7', color2: '#1e1b4b', ai: 'float', shoots: true, fireRate: 3.1, pearls: 10, score: 130 },
  clownfish: { name: 'Rogue Clownfish', hp: 28, speed: 165, radius: 17, damage: 8, color: '#f97316', color2: '#ffffff', ai: 'wave_rush', shoots: false, pearls: 7, score: 90 },
  coral_monster: { name: 'Coral Golem', hp: 85, speed: 68, radius: 27, damage: 18, color: '#fb7185', color2: '#881337', ai: 'chase', shoots: true, fireRate: 2.4, pearls: 16, score: 200 },
  shark_warrior: { name: 'Shark Warrior', hp: 92, speed: 185, radius: 28, damage: 22, color: '#64748b', color2: '#1e293b', ai: 'shark_charge', shoots: false, pearls: 18, score: 240 },
  giant_octopus: { name: 'Giant Octopus', hp: 110, speed: 85, radius: 30, damage: 20, color: '#e879f9', color2: '#581c87', ai: 'sine', shoots: true, fireRate: 2.1, pearls: 20, score: 260 },
  squid: { name: 'Poisonous Squid', hp: 64, speed: 160, radius: 22, damage: 15, color: '#2dd4bf', color2: '#115e59', ai: 'dash', shoots: true, fireRate: 2.3, pearls: 15, score: 190 },
  armoured_puffer: { name: 'Armoured Puffer', hp: 125, speed: 70, radius: 26, damage: 20, color: '#94a3b8', color2: '#334155', ai: 'puff', shoots: true, fireRate: 2.5, pearls: 19, score: 250 },
  shadow_jelly: { name: 'Shadow Jellyfish', hp: 95, speed: 155, radius: 24, damage: 24, color: '#6366f1', color2: '#090d16', ai: 'teleport', shoots: true, fireRate: 2.0, pearls: 22, score: 280 },
  crystal_monster: { name: 'Crystal Monster', hp: 105, speed: 88, radius: 27, damage: 21, color: '#22d3ee', color2: '#1e3a8a', ai: 'chase', shoots: true, fireRate: 2.2, pearls: 20, score: 260 },
  deep_predator: { name: 'Deep-Sea Predator', hp: 115, speed: 175, radius: 28, damage: 25, color: '#14b8a6', color2: '#042f2e', ai: 'shark_charge', shoots: true, fireRate: 2.4, pearls: 24, score: 300 }
};

const BOSS_TYPES = {
  dark_jelly_king: {
    name: 'DARK JELLYFISH KING',
    hp: 950,
    radius: 62,
    speed: 95,
    color1: '#a855f7',
    color2: '#3b0764',
    damage: 24
  },
  crystal_monster_boss: {
    name: 'CRYSTAL SEA MONSTER',
    hp: 1450,
    radius: 68,
    speed: 85,
    color1: '#22d3ee',
    color2: '#1d4ed8',
    damage: 28
  },
  shark_commander: {
    name: 'SHARK COMMANDER',
    hp: 1950,
    radius: 66,
    speed: 145,
    color1: '#94a3b8',
    color2: '#0f172a',
    damage: 32
  },
  mega_octopus: {
    name: 'MEGA OCTOPUS',
    hp: 2550,
    radius: 74,
    speed: 90,
    color1: '#f43f5e',
    color2: '#4c0519',
    damage: 35
  },
  dark_emperor: {
    name: 'THE DARK OCEAN EMPEROR',
    hp: 3600,
    radius: 82,
    speed: 110,
    color1: '#e11d48',
    color2: '#1e1b4b',
    damage: 40
  }
};

const ACHIEVEMENTS_LIST = [
  { id: 'first_victory', name: 'First Victory', desc: 'Complete your first ocean mission', icon: '🌟', reward: { pearls: 50, golden: 1, crystals: 2 }, check: s => s.stats.levelsWon >= 1 },
  { id: 'jelly_hunter', name: 'Jellyfish Hunter', desc: 'Defeat 50 ocean enemies', icon: '⚔️', reward: { pearls: 100, golden: 1, crystals: 5 }, check: s => s.stats.enemiesDefeated >= 50 },
  { id: 'pearl_collector', name: 'Pearl Collector', desc: 'Accumulate 500 Pearls total', icon: '⚪', reward: { pearls: 150, golden: 2, crystals: 5 }, check: s => s.stats.totalPearls >= 500 },
  { id: 'crystal_master', name: 'Crystal Master', desc: 'Collect 50 Magical Crystals', icon: '💎', reward: { pearls: 200, golden: 2, crystals: 10 }, check: s => s.stats.totalCrystals >= 50 },
  { id: 'coral_defender', name: 'Coral Defender', desc: 'Defeat the Dark Jellyfish King (Level 5)', icon: '🏰', reward: { pearls: 250, golden: 3, crystals: 10 }, check: s => s.completedLevels.includes(5) },
  { id: 'boss_slayer', name: 'Boss Slayer', desc: 'Defeat 3 Ocean Bosses', icon: '👑', reward: { pearls: 400, golden: 5, crystals: 15 }, check: s => s.stats.bossesDefeated >= 3 },
  { id: 'arsenal_master', name: 'Royal Arsenal', desc: 'Unlock at least 4 weapons', icon: '🫧', reward: { pearls: 300, golden: 3, crystals: 12 }, check: s => s.unlockedWeapons.length >= 4 },
  { id: 'hero_squad', name: 'Heroic Sanctuary', desc: 'Unlock at least 4 jellyfish heroes', icon: '🪼', reward: { pearls: 350, golden: 4, crystals: 15 }, check: s => s.unlockedHeroes.length >= 4 },
  { id: 'ocean_explorer', name: 'Ocean Explorer', desc: 'Reach Level 15', icon: '🗺️', reward: { pearls: 500, golden: 6, crystals: 20 }, check: s => s.highestUnlockedLevel >= 15 },
  { id: 'legendary_warrior', name: 'Legendary Ocean Savior', desc: 'Defeat the Dark Ocean Emperor (Level 25)', icon: '🏆', reward: { pearls: 1000, golden: 15, crystals: 40 }, check: s => s.completedLevels.includes(25) }
];

/* -------------------- 2. SAVE & STORAGE SYSTEM -------------------- */

const Storage = {
  KEY: 'jellyfish_kingdom_ocean_wars_v1',
  data: null,

  getDefault() {
    return {
      pearls: 0,
      golden: 0,
      crystals: 0,
      selectedHero: 'pink_warrior',
      unlockedHeroes: ['pink_warrior'],
      selectedWeapon: 'bubble_blaster',
      unlockedWeapons: ['bubble_blaster'],
      weaponLevels: {
        bubble_blaster: 1,
        crystal_shooter: 1,
        electric_ray: 1,
        coral_cannon: 1,
        pearl_launcher: 1,
        plasma_bubble: 1,
        ocean_laser: 1,
        royal_blaster: 1
      },
      upgrades: {
        maxHp: 0,
        speed: 0,
        damage: 0,
        energy: 0,
        shield: 0,
        special: 0
      },
      highestUnlockedLevel: 1,
      completedLevels: [],
      highScores: {},
      claimedAchievements: [],
      lastDailyDate: '',
      dailyStreak: 0,
      dailyChallengeProgress: 0,
      dailyChallengeClaimed: false,
      stats: {
        enemiesDefeated: 0,
        bossesDefeated: 0,
        levelsWon: 0,
        totalPearls: 0,
        totalCrystals: 0
      },
      settings: {
        musicVol: 0.45,
        sfxVol: 0.7,
        screenShake: true
      }
    };
  },

  load() {
    try {
      const raw = localStorage.getItem(this.KEY);
      if (raw) {
        const parsed = JSON.parse(raw);
        this.data = Object.assign(this.getDefault(), parsed);
        this.data.upgrades = Object.assign(this.getDefault().upgrades, parsed.upgrades || {});
        this.data.weaponLevels = Object.assign(this.getDefault().weaponLevels, parsed.weaponLevels || {});
        this.data.stats = Object.assign(this.getDefault().stats, parsed.stats || {});
        this.data.settings = Object.assign(this.getDefault().settings, parsed.settings || {});
      } else {
        this.data = this.getDefault();
        this.save();
      }
    } catch (e) {
      this.data = this.getDefault();
    }
  },

  save() {
    try {
      localStorage.setItem(this.KEY, JSON.stringify(this.data));
    } catch (e) {
      // Ignore storage quota issues
    }
    UI.updateCurrencyDisplays();
  },

  promptReset() {
    document.getElementById('resetConfirmBox').classList.remove('hidden');
  },

  cancelReset() {
    document.getElementById('resetConfirmBox').classList.add('hidden');
  },

  confirmReset() {
    this.data = this.getDefault();
    this.save();
    this.cancelReset();
    UI.closeModals();
    UI.refreshAll();
    UI.toast('Save profile reset to a fresh journey!', 'info');
  }
};

/* -------------------- 3. WEB AUDIO SYNTHESIZER -------------------- */

const AudioEngine = {
  ctx: null,
  musicTimer: null,
  musicMode: 'menu', // 'menu', 'battle', 'boss'
  noteIndex: 0,

  init() {
    if (!this.ctx) {
      const AudioCtx = window.AudioContext || window.webkitAudioContext;
      if (AudioCtx) {
        this.ctx = new AudioCtx();
      }
    }
    if (this.ctx && this.ctx.state === 'suspended') {
      this.ctx.resume().catch(() => {});
    }
    if (!this.musicTimer) {
      this.startMusicLoop();
    }
  },

  setMusicVolume(val) {
    const v = Math.max(0, Math.min(100, Number(val))) / 100;
    Storage.data.settings.musicVol = v;
    document.getElementById('musicVolLabel').textContent = `${Math.round(v * 100)}%`;
    Storage.save();
  },

  setSfxVolume(val) {
    const v = Math.max(0, Math.min(100, Number(val))) / 100;
    Storage.data.settings.sfxVol = v;
    document.getElementById('sfxVolLabel').textContent = `${Math.round(v * 100)}%`;
    Storage.save();
  },

  playTone(freq, endFreq, duration, type = 'sine', volScale = 1.0) {
    const vol = (Storage.data?.settings?.sfxVol ?? 0.7) * volScale;
    if (vol <= 0.01 || !this.ctx) return;
    try {
      const now = this.ctx.currentTime;
      const osc = this.ctx.createOscillator();
      const gain = this.ctx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, now);
      if (endFreq && endFreq !== freq) {
        osc.frequency.exponentialRampToValueAtTime(Math.max(20, endFreq), now + duration);
      }
      gain.gain.setValueAtTime(vol * 0.22, now);
      gain.gain.exponentialRampToValueAtTime(0.001, now + duration);
      osc.connect(gain);
      gain.connect(this.ctx.destination);
      osc.start(now);
      osc.stop(now + duration);
    } catch (e) {}
  },

  sfxShoot(weaponId) {
    if (weaponId === 'electric_ray') {
      this.playTone(920, 260, 0.11, 'sawtooth', 0.55);
    } else if (weaponId === 'crystal_shooter' || weaponId === 'ocean_laser') {
      this.playTone(1150, 480, 0.12, 'triangle', 0.6);
    } else if (weaponId === 'pearl_launcher') {
      this.playTone(320, 110, 0.22, 'sine', 0.8);
    } else {
      // Magical bubble pop shoot
      this.playTone(420 + Math.random() * 140, 880, 0.12, 'sine', 0.65);
    }
  },

  sfxHit() {
    this.playTone(210, 90, 0.09, 'triangle', 0.5);
  },

  sfxExplosion() {
    this.playTone(160, 38, 0.34, 'sawtooth', 0.85);
    this.playTone(110, 30, 0.38, 'sine', 0.8);
  },

  sfxPickup(isCrystal = false) {
    if (isCrystal) {
      this.playTone(780, 1320, 0.18, 'triangle', 0.75);
    } else {
      this.playTone(580, 980, 0.14, 'sine', 0.65);
    }
  },

  sfxSpecial() {
    this.playTone(340, 1280, 0.42, 'sine', 0.9);
    setTimeout(() => this.playTone(520, 1560, 0.35, 'triangle', 0.75), 90);
  },

  sfxShield() {
    this.playTone(280, 620, 0.28, 'sine', 0.75);
  },

  sfxBossWarning() {
    this.playTone(140, 75, 0.45, 'sawtooth', 0.9);
    setTimeout(() => this.playTone(130, 65, 0.5, 'sawtooth', 0.9), 300);
  },

  sfxVictory() {
    const notes = [523.25, 659.25, 783.99, 1046.5];
    notes.forEach((n, i) => {
      setTimeout(() => this.playTone(n, n * 1.02, 0.28, 'triangle', 0.85), i * 120);
    });
  },

  sfxDefeat() {
    const notes = [392.0, 349.23, 311.13, 261.63];
    notes.forEach((n, i) => {
      setTimeout(() => this.playTone(n, n * 0.95, 0.32, 'sine', 0.8), i * 160);
    });
  },

  sfxClick() {
    this.playTone(620, 840, 0.08, 'sine', 0.45);
  },

  setMusicMode(mode) {
    this.musicMode = mode;
    this.noteIndex = 0;
  },

  startMusicLoop() {
    const scales = {
      menu: [523.25, 659.25, 783.99, 987.77, 1046.5, 987.77, 783.99, 659.25],
      battle: [392.0, 466.16, 523.25, 587.33, 698.46, 587.33, 523.25, 466.16],
      boss: [261.63, 311.13, 293.66, 369.99, 349.23, 277.18, 246.94, 261.63]
    };

    this.musicTimer = setInterval(() => {
      const mVol = Storage.data?.settings?.musicVol ?? 0.45;
      if (mVol <= 0.01 || !this.ctx || this.ctx.state !== 'running') return;
      const seq = scales[this.musicMode] || scales.menu;
      const freq = seq[this.noteIndex % seq.length];
      this.noteIndex++;
      try {
        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = this.musicMode === 'boss' ? 'triangle' : 'sine';
        osc.frequency.setValueAtTime(freq, now);
        gain.gain.setValueAtTime(0.001, now);
        gain.gain.linearRampToValueAtTime(mVol * 0.08, now + 0.06);
        gain.gain.exponentialRampToValueAtTime(0.0008, now + 0.48);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(now);
        osc.stop(now + 0.5);
      } catch (e) {}
    }, 420);
  }
};

/* -------------------- 4. PROCEDURAL UNDERWATER GRAPHICS -------------------- */

const Renderer = {
  bgBubbles: [],
  dustParticles: [],
  ambientFish: [],
  menuJellies: [],
  corals: [],
  seaweeds: [],

  initEnvironment(w, h) {
    this.bgBubbles = Array.from({ length: 45 }, () => ({
      x: Math.random() * w,
      y: Math.random() * h,
      r: 2 + Math.random() * 6,
      speed: 22 + Math.random() * 45,
      wobble: Math.random() * Math.PI * 2
    }));

    this.dustParticles = Array.from({ length: 55 }, () => ({
      x: Math.random() * w,
      y: Math.random() * h,
      r: 1 + Math.random() * 2.2,
      vx: -10 + Math.random() * 20,
      vy: -8 + Math.random() * 16,
      alpha: 0.25 + Math.random() * 0.55
    }));

    this.ambientFish = Array.from({ length: 12 }, () => ({
      x: Math.random() * w,
      y: h * 0.18 + Math.random() * h * 0.58,
      size: 7 + Math.random() * 9,
      speed: (35 + Math.random() * 55) * (Math.random() < 0.5 ? 1 : -1),
      color: ['#fde047', '#38bdf8', '#fb7185', '#34d399', '#f472b6'][Math.floor(Math.random() * 5)],
      phase: Math.random() * 6.28
    }));

    this.menuJellies = Array.from({ length: 6 }, (_, i) => ({
      x: w * (0.12 + i * 0.15),
      y: h * (0.25 + (i % 3) * 0.18),
      scale: 0.55 + (i % 3) * 0.18,
      phase: i * 1.1,
      hero: HEROES[i % HEROES.length]
    }));

    this.seaweeds = Array.from({ length: 26 }, (_, i) => ({
      x: (w / 25) * i + (Math.random() * 25 - 12),
      height: 55 + Math.random() * 85,
      segments: 6,
      phase: Math.random() * Math.PI * 2,
      color: i % 2 === 0 ? '#10b981' : '#059669'
    }));

    this.corals = Array.from({ length: 18 }, (_, i) => ({
      x: (w / 17) * i + 15,
      type: i % 4, // 0: branching, 1: tube, 2: mushroom, 3: fan
      scale: 0.75 + Math.random() * 0.65,
      colorIdx: i % 5
    }));
  },

  drawBackground(ctx, w, h, time, worldId, isMenu) {
    const world = WORLDS[(worldId || 1) - 1] || WORLDS[0];

    // 1. Deep Ocean Gradient
    const grad = ctx.createLinearGradient(0, 0, 0, h);
    grad.addColorStop(0, world.bgTop);
    grad.addColorStop(0.55, world.bgMid);
    grad.addColorStop(1, world.bgBot);
    ctx.fillStyle = grad;
    ctx.fillRect(0, 0, w, h);

    // 2. Volumetric Sunlight Rays from Surface
    ctx.save();
    for (let i = 0; i < 6; i++) {
      const rayX = w * (0.1 + i * 0.17) + Math.sin(time * 0.4 + i) * 35;
      const topW = 35 + (i % 3) * 20;
      const botW = 140 + (i % 3) * 45;
      const rayGrad = ctx.createLinearGradient(rayX, 0, rayX + 40, h * 0.85);
      rayGrad.addColorStop(0, world.rayColor);
      rayGrad.addColorStop(1, 'rgba(0,0,0,0)');
      ctx.fillStyle = rayGrad;
      ctx.beginPath();
      ctx.moveTo(rayX - topW / 2, 0);
      ctx.lineTo(rayX + topW / 2, 0);
      ctx.lineTo(rayX + botW / 2 + 50, h * 0.88);
      ctx.lineTo(rayX - botW / 2 + 50, h * 0.88);
      ctx.closePath();
      ctx.fill();
    }
    ctx.restore();

    // 3. Distant Silhouette Reef Hills
    ctx.fillStyle = 'rgba(8, 22, 54, 0.45)';
    ctx.beginPath();
    ctx.moveTo(0, h);
    for (let x = 0; x <= w; x += 60) {
      const hillY = h - 95 - Math.sin(x * 0.006 + 1) * 35 - Math.cos(x * 0.015) * 18;
      ctx.lineTo(x, hillY);
    }
    ctx.lineTo(w, h);
    ctx.closePath();
    ctx.fill();

    // 4. Magical Royal Underwater Castle (Inspired by Reference Art)
    this.drawRoyalCastle(ctx, w * (isMenu ? 0.5 : 0.78), h - 52, isMenu ? Math.min(1.05, w / 950) : Math.min(0.75, w / 1250), time, world.castleTint);

    // 5. Ambient Tropical Fish Schools
    this.ambientFish.forEach(f => {
      f.x += f.speed * 0.016;
      if (f.speed > 0 && f.x > w + 40) f.x = -40;
      if (f.speed < 0 && f.x < -40) f.x = w + 40;
      const fy = f.y + Math.sin(time * 2.5 + f.phase) * 8;
      ctx.save();
      ctx.translate(f.x, fy);
      if (f.speed < 0) ctx.scale(-1, 1);
      ctx.fillStyle = f.color;
      ctx.globalAlpha = 0.55;
      // Body
      ctx.beginPath();
      ctx.ellipse(0, 0, f.size, f.size * 0.55, 0, 0, Math.PI * 2);
      ctx.fill();
      // Tail
      ctx.beginPath();
      ctx.moveTo(-f.size * 0.8, 0);
      ctx.lineTo(-f.size * 1.45, -f.size * 0.5);
      ctx.lineTo(-f.size * 1.45, f.size * 0.5);
      ctx.closePath();
      ctx.fill();
      ctx.restore();
    });

    // 6. Seaweed & Coral Floor
    this.drawSeafloor(ctx, w, h, time, world);

    // 7. Floating Dust & Glowing Bubbles
    this.dustParticles.forEach(p => {
      p.x += p.vx * 0.016;
      p.y += p.vy * 0.016;
      if (p.x < 0) p.x = w;
      if (p.x > w) p.x = 0;
      if (p.y < 0) p.y = h;
      if (p.y > h) p.y = 0;
      ctx.fillStyle = `rgba(186, 230, 253, ${p.alpha})`;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fill();
    });

    this.bgBubbles.forEach(b => {
      b.y -= b.speed * 0.016;
      b.wobble += 0.03;
      if (b.y < -15) {
        b.y = h + 15;
        b.x = Math.random() * w;
      }
      const bx = b.x + Math.sin(b.wobble) * 6;
      ctx.strokeStyle = 'rgba(165, 243, 252, 0.45)';
      ctx.fillStyle = 'rgba(255, 255, 255, 0.12)';
      ctx.lineWidth = 1.2;
      ctx.beginPath();
      ctx.arc(bx, b.y, b.r, 0, Math.PI * 2);
      ctx.fill();
      ctx.stroke();
      // Shine dot
      ctx.fillStyle = 'rgba(255,255,255,0.65)';
      ctx.beginPath();
      ctx.arc(bx - b.r * 0.3, b.y - b.r * 0.3, b.r * 0.28, 0, Math.PI * 2);
      ctx.fill();
    });

    // 8. Menu Decorative Swimming Jellyfish
    if (isMenu) {
      this.menuJellies.forEach((mj, idx) => {
        const jx = mj.x + Math.sin(time * 0.8 + mj.phase) * 28;
        const jy = mj.y + Math.cos(time * 1.1 + mj.phase) * 18;
        this.drawJellyfishHero(ctx, jx, jy, 26 * mj.scale, mj.hero, time + idx, 0, false, 0);
      });
    }
  },

  drawRoyalCastle(ctx, cx, baseY, scale, time, tintColor) {
    if (scale <= 0.1) return;
    ctx.save();
    ctx.translate(cx, baseY);
    ctx.scale(scale, scale);
    ctx.globalAlpha = 0.82;

    // Magical Aura behind Castle
    const aura = ctx.createRadialGradient(0, -140, 20, 0, -140, 230);
    aura.addColorStop(0, 'rgba(255, 110, 199, 0.35)');
    aura.addColorStop(0.6, 'rgba(56, 189, 248, 0.14)');
    aura.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = aura;
    ctx.beginPath();
    ctx.arc(0, -140, 230, 0, Math.PI * 2);
    ctx.fill();

    // Castle Towers helper
    const drawTower = (tx, ty, tw, th, roofH) => {
      const tGrad = ctx.createLinearGradient(tx - tw / 2, ty - th, tx + tw / 2, ty);
      tGrad.addColorStop(0, tintColor);
      tGrad.addColorStop(1, '#4c1d95');
      ctx.fillStyle = tGrad;
      ctx.beginPath();
      ctx.roundRect(tx - tw / 2, ty - th, tw, th, [8, 8, 0, 0]);
      ctx.fill();

      // Conical Shell Roof
      ctx.fillStyle = '#f472b6';
      ctx.beginPath();
      ctx.moveTo(tx - tw * 0.65, ty - th);
      ctx.quadraticCurveTo(tx, ty - th - roofH * 1.2, tx, ty - th - roofH);
      ctx.quadraticCurveTo(tx, ty - th - roofH * 1.2, tx + tw * 0.65, ty - th);
      ctx.closePath();
      ctx.fill();

      // Spire Pearl
      ctx.fillStyle = '#fef08a';
      ctx.beginPath();
      ctx.arc(tx, ty - th - roofH - 4, 4, 0, Math.PI * 2);
      ctx.fill();

      // Glowing Window
      const glowPulse = 0.7 + Math.sin(time * 3 + tx) * 0.3;
      ctx.fillStyle = `rgba(254, 240, 138, ${glowPulse})`;
      ctx.beginPath();
      ctx.arc(tx, ty - th * 0.58, tw * 0.22, Math.PI, 0);
      ctx.lineTo(tx + tw * 0.22, ty - th * 0.38);
      ctx.lineTo(tx - tw * 0.22, ty - th * 0.38);
      ctx.closePath();
      ctx.fill();
    };

    // Side Outer Towers
    drawTower(-115, 0, 42, 135, 48);
    drawTower(115, 0, 42, 135, 48);
    // Mid Towers
    drawTower(-65, 0, 52, 175, 58);
    drawTower(65, 0, 52, 175, 58);
    // Main Keep
    drawTower(0, 0, 78, 220, 75);

    // Royal Arched Gate
    ctx.fillStyle = '#1e1b4b';
    ctx.beginPath();
    ctx.arc(0, -32, 24, Math.PI, 0);
    ctx.lineTo(24, 0);
    ctx.lineTo(-24, 0);
    ctx.closePath();
    ctx.fill();

    // Glowing Portal inside Gate
    const gateGrad = ctx.createRadialGradient(0, -20, 2, 0, -20, 24);
    gateGrad.addColorStop(0, '#67e8f9');
    gateGrad.addColorStop(1, 'rgba(168, 85, 247, 0.2)');
    ctx.fillStyle = gateGrad;
    ctx.fill();

    ctx.restore();
  },

  drawSeafloor(ctx, w, h, time, world) {
    // Swaying Seaweed
    this.seaweeds.forEach(sw => {
      ctx.strokeStyle = sw.color;
      ctx.lineWidth = 5;
      ctx.lineCap = 'round';
      ctx.beginPath();
      ctx.moveTo(sw.x, h - 25);
      for (let s = 1; s <= sw.segments; s++) {
        const ratio = s / sw.segments;
        const sway = Math.sin(time * 2.2 + sw.phase + ratio * 2.5) * (14 * ratio);
        ctx.lineTo(sw.x + sway, h - 25 - sw.height * ratio);
      }
      ctx.stroke();
    });

    // Coral Formations
    this.corals.forEach(c => {
      const col = world.coralColors[c.colorIdx % world.coralColors.length];
      ctx.save();
      ctx.translate(c.x, h - 32);
      ctx.scale(c.scale, c.scale);
      ctx.fillStyle = col;
      ctx.strokeStyle = col;

      if (c.type === 0) {
        // Branching coral
        ctx.lineWidth = 6;
        ctx.lineCap = 'round';
        ctx.beginPath();
        ctx.moveTo(0, 0);
        ctx.lineTo(0, -32);
        ctx.moveTo(0, -14);
        ctx.lineTo(-14, -28);
        ctx.moveTo(0, -20);
        ctx.lineTo(14, -34);
        ctx.stroke();
      } else if (c.type === 1) {
        // Yellow/Pink Coral Tubes
        [-10, 0, 10].forEach((ox, idx) => {
          const th = 22 + (idx % 2) * 12;
          ctx.beginPath();
          ctx.roundRect(ox - 4, -th, 8, th, 4);
          ctx.fill();
          ctx.fillStyle = 'rgba(0,0,0,0.35)';
          ctx.beginPath();
          ctx.ellipse(ox, -th + 2, 3, 1.5, 0, 0, Math.PI * 2);
          ctx.fill();
          ctx.fillStyle = col;
        });
      } else if (c.type === 2) {
        // Glowing Sea Mushroom
        ctx.fillStyle = '#fef3c7';
        ctx.fillRect(-4, -18, 8, 18);
        ctx.fillStyle = col;
        ctx.beginPath();
        ctx.arc(0, -18, 16, Math.PI, 0);
        ctx.fill();
        ctx.fillStyle = 'rgba(255,255,255,0.75)';
        ctx.beginPath();
        ctx.arc(-6, -24, 2.5, 0, Math.PI * 2);
        ctx.arc(5, -22, 3, 0, Math.PI * 2);
        ctx.fill();
      } else {
        // Coral Dome
        ctx.beginPath();
        ctx.arc(0, 0, 18, Math.PI, 0);
        ctx.fill();
      }
      ctx.restore();
    });

    // Sandy / Reef Floor Strip
    const floorGrad = ctx.createLinearGradient(0, h - 42, 0, h);
    floorGrad.addColorStop(0, '#312e81');
    floorGrad.addColorStop(0.4, '#1e1b4b');
    floorGrad.addColorStop(1, '#090d16');
    ctx.fillStyle = floorGrad;
    ctx.beginPath();
    ctx.moveTo(0, h - 34);
    for (let x = 0; x <= w; x += 80) {
      ctx.quadraticCurveTo(x + 40, h - 42, x + 80, h - 34);
    }
    ctx.lineTo(w, h);
    ctx.lineTo(0, h);
    ctx.closePath();
    ctx.fill();
  },

  /**
   * Renders any of the 10 Playable Jellyfish Heroes with animated bell,
   * expressive eyes, cute smile, flowing multi-segment tentacles, and aura!
   */
  drawJellyfishHero(ctx, x, y, radius, hero, time, tiltAngle = 0, hasShield = false, hurtFlash = 0) {
    ctx.save();
    ctx.translate(x, y);
    ctx.rotate(tiltAngle * 0.25);

    const pulse = Math.sin(time * 6.5);
    const bellW = radius * (1.08 + pulse * 0.07);
    const bellH = radius * (0.92 - pulse * 0.06);

    // Rainbow color cycling if applicable
    let c1 = hero.color1;
    let c2 = hero.color2;
    if (hero.rainbow) {
      const hue = (time * 90) % 360;
      c1 = `hsl(${hue}, 90%, 68%)`;
      c2 = `hsl(${(hue + 75) % 360}, 85%, 52%)`;
    }
    if (hurtFlash > 0) {
      c1 = '#ffffff';
      c2 = '#fda4af';
    }

    // Outer Magical Bioluminescent Glow
    const glowGrad = ctx.createRadialGradient(0, 0, radius * 0.3, 0, 0, radius * 1.9);
    glowGrad.addColorStop(0, hero.glow + '66');
    glowGrad.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = glowGrad;
    ctx.beginPath();
    ctx.arc(0, 0, radius * 1.9, 0, Math.PI * 2);
    ctx.fill();

    // Animated Flowing Tentacles (6 tentacles)
    const tentacleOffsets = [-0.72, -0.44, -0.16, 0.16, 0.44, 0.72];
    tentacleOffsets.forEach((off, idx) => {
      const startX = off * bellW * 0.85;
      const startY = bellH * 0.25;
      const len = radius * (1.15 + (idx % 2) * 0.25);
      ctx.strokeStyle = idx % 2 === 0 ? c1 : c2;
      ctx.lineWidth = Math.max(2.5, radius * 0.12);
      ctx.lineCap = 'round';
      ctx.beginPath();
      ctx.moveTo(startX, startY);
      for (let seg = 1; seg <= 4; seg++) {
        const t = seg / 4;
        const wave = Math.sin(time * 7 + idx * 0.9 + t * 3.5) * (radius * 0.24 * t);
        ctx.lineTo(startX + wave, startY + len * t);
      }
      ctx.stroke();
    });

    // Translucent Jellyfish Bell Dome
    const bellGrad = ctx.createRadialGradient(-bellW * 0.25, -bellH * 0.4, 2, 0, 0, bellW * 1.15);
    bellGrad.addColorStop(0, '#ffffff');
    bellGrad.addColorStop(0.28, c1);
    bellGrad.addColorStop(1, c2);

    ctx.fillStyle = bellGrad;
    ctx.beginPath();
    ctx.moveTo(-bellW, bellH * 0.22);
    ctx.bezierCurveTo(-bellW * 1.05, -bellH * 1.15, bellW * 1.05, -bellH * 1.15, bellW, bellH * 0.22);
    // Scalloped Ruffled Skirt
    const scallops = 5;
    for (let s = scallops; s >= 0; s--) {
      const sx = -bellW + (bellW * 2 * s) / scallops;
      const sy = bellH * 0.22 + (s % 2 === 0 ? 4 : -3);
      ctx.lineTo(sx, sy);
    }
    ctx.closePath();
    ctx.fill();

    // Rim Highlight
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.65)';
    ctx.lineWidth = 1.6;
    ctx.stroke();

    // Bell Spots / Bioluminescent Dots
    ctx.fillStyle = 'rgba(255, 255, 255, 0.42)';
    ctx.beginPath();
    ctx.arc(-bellW * 0.45, -bellH * 0.42, radius * 0.12, 0, Math.PI * 2);
    ctx.arc(bellW * 0.48, -bellH * 0.35, radius * 0.09, 0, Math.PI * 2);
    ctx.fill();

    // Royal Crown (for Royal & Legendary Jellyfish)
    if (hero.crown) {
      ctx.fillStyle = '#fbbf24';
      ctx.strokeStyle = '#fef08a';
      ctx.lineWidth = 1.2;
      const cy = -bellH * 0.92;
      ctx.beginPath();
      ctx.moveTo(-radius * 0.38, cy);
      ctx.lineTo(-radius * 0.45, cy - radius * 0.38);
      ctx.lineTo(-radius * 0.16, cy - radius * 0.18);
      ctx.lineTo(0, cy - radius * 0.46);
      ctx.lineTo(radius * 0.16, cy - radius * 0.18);
      ctx.lineTo(radius * 0.45, cy - radius * 0.38);
      ctx.lineTo(radius * 0.38, cy);
      ctx.closePath();
      ctx.fill();
      ctx.stroke();
    }

    // Large Expressive Cute Eyes
    const eyeX = radius * 0.34;
    const eyeY = -bellH * 0.12;
    const eyeR = radius * 0.2;

    [-1, 1].forEach(dir => {
      // Eye white/iris base
      ctx.fillStyle = hero.eyeColor || '#1e1b4b';
      ctx.beginPath();
      ctx.arc(dir * eyeX, eyeY, eyeR, 0, Math.PI * 2);
      ctx.fill();

      // Sparkle highlights
      ctx.fillStyle = '#ffffff';
      ctx.beginPath();
      ctx.arc(dir * eyeX - eyeR * 0.28, eyeY - eyeR * 0.3, eyeR * 0.38, 0, Math.PI * 2);
      ctx.arc(dir * eyeX + eyeR * 0.25, eyeY + eyeR * 0.22, eyeR * 0.18, 0, Math.PI * 2);
      ctx.fill();

      // Rosy Cheeks
      ctx.fillStyle = 'rgba(255, 110, 199, 0.55)';
      ctx.beginPath();
      ctx.ellipse(dir * (eyeX + eyeR * 0.7), eyeY + eyeR * 1.15, eyeR * 0.65, eyeR * 0.38, 0, 0, Math.PI * 2);
      ctx.fill();
    });

    // Cute Animated Smiling Mouth
    ctx.strokeStyle = hero.eyeColor || '#1e1b4b';
    ctx.lineWidth = Math.max(1.8, radius * 0.08);
    ctx.lineCap = 'round';
    ctx.beginPath();
    ctx.arc(0, eyeY + eyeR * 0.95, radius * 0.14, 0.15 * Math.PI, 0.85 * Math.PI);
    ctx.stroke();

    // Active Bubble Shield Dome
    if (hasShield) {
      const shGrad = ctx.createRadialGradient(0, 0, radius * 0.8, 0, 0, radius * 1.65);
      shGrad.addColorStop(0, 'rgba(103, 232, 249, 0.05)');
      shGrad.addColorStop(0.8, 'rgba(103, 232, 249, 0.32)');
      shGrad.addColorStop(1, 'rgba(255, 255, 255, 0.85)');
      ctx.fillStyle = shGrad;
      ctx.strokeStyle = '#67e8f9';
      ctx.lineWidth = 2.5;
      ctx.beginPath();
      ctx.arc(0, 0, radius * 1.62, 0, Math.PI * 2);
      ctx.fill();
      ctx.stroke();
    }

    ctx.restore();
  },

  /**
   * Renders Enemy Creatures with distinct visual silhouettes & animations
   */
  drawEnemy(ctx, e, time) {
    ctx.save();
    ctx.translate(e.x, e.y);
    const r = e.radius * (e.puffScale || 1);
    const c1 = e.hurtFlash > 0 ? '#ffffff' : e.color;
    const c2 = e.hurtFlash > 0 ? '#fecdd3' : e.color2;

    // Glow
    ctx.fillStyle = c1 + '33';
    ctx.beginPath();
    ctx.arc(0, 0, r * 1.35, 0, Math.PI * 2);
    ctx.fill();

    if (e.typeKey.includes('jelly')) {
      // Evil / Electric / Shadow Jellyfish
      for (let i = -2; i <= 2; i++) {
        ctx.strokeStyle = c1;
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(i * r * 0.32, r * 0.2);
        ctx.quadraticCurveTo(
          i * r * 0.35 + Math.sin(time * 8 + i) * 7,
          r * 0.85,
          i * r * 0.3,
          r * 1.35
        );
        ctx.stroke();
      }
      const g = ctx.createRadialGradient(0, -r * 0.3, 2, 0, 0, r);
      g.addColorStop(0, c1);
      g.addColorStop(1, c2);
      ctx.fillStyle = g;
      ctx.beginPath();
      ctx.arc(0, r * 0.15, r, Math.PI, 0);
      ctx.closePath();
      ctx.fill();
      // Angry Eyes
      ctx.fillStyle = '#fef08a';
      ctx.beginPath();
      ctx.arc(-r * 0.32, -r * 0.18, r * 0.16, 0, Math.PI * 2);
      ctx.arc(r * 0.32, -r * 0.18, r * 0.16, 0, Math.PI * 2);
      ctx.fill();
    } else if (e.typeKey.includes('puffer') || e.typeKey === 'sea_urchin') {
      // Spiky Pufferfish / Urchin
      const spikes = e.typeKey === 'sea_urchin' ? 14 : 10;
      ctx.strokeStyle = c1;
      ctx.lineWidth = 3;
      for (let s = 0; s < spikes; s++) {
        const ang = (s / spikes) * Math.PI * 2 + time * 1.5;
        ctx.beginPath();
        ctx.moveTo(Math.cos(ang) * r * 0.85, Math.sin(ang) * r * 0.85);
        ctx.lineTo(Math.cos(ang) * r * 1.32, Math.sin(ang) * r * 1.32);
        ctx.stroke();
      }
      ctx.fillStyle = c2;
      ctx.beginPath();
      ctx.arc(0, 0, r * 0.92, 0, Math.PI * 2);
      ctx.fill();
      ctx.fillStyle = c1;
      ctx.beginPath();
      ctx.arc(0, -r * 0.1, r * 0.72, 0, Math.PI * 2);
      ctx.fill();
      // Eyes
      ctx.fillStyle = '#ffffff';
      ctx.beginPath();
      ctx.arc(-r * 0.3, -r * 0.15, r * 0.2, 0, Math.PI * 2);
      ctx.arc(r * 0.3, -r * 0.15, r * 0.2, 0, Math.PI * 2);
      ctx.fill();
      ctx.fillStyle = '#090d16';
      ctx.beginPath();
      ctx.arc(-r * 0.3, -r * 0.15, r * 0.09, 0, Math.PI * 2);
      ctx.arc(r * 0.3, -r * 0.15, r * 0.09, 0, Math.PI * 2);
      ctx.fill();
    } else if (e.typeKey.includes('shark') || e.typeKey === 'deep_predator' || e.typeKey === 'clownfish') {
      // Streamlined Fish / Shark Warrior
      const dir = e.vx > 0 ? 1 : -1;
      ctx.scale(dir, 1);
      ctx.fillStyle = c1;
      ctx.beginPath();
      ctx.ellipse(0, 0, r * 1.25, r * 0.65, 0, 0, Math.PI * 2);
      ctx.fill();
      // Dorsal Fin
      ctx.fillStyle = c2;
      ctx.beginPath();
      ctx.moveTo(-r * 0.2, -r * 0.55);
      ctx.lineTo(r * 0.1, -r * 1.15);
      ctx.lineTo(r * 0.35, -r * 0.5);
      ctx.closePath();
      ctx.fill();
      // Tail
      ctx.beginPath();
      ctx.moveTo(-r * 1.1, 0);
      ctx.lineTo(-r * 1.65, -r * 0.55);
      ctx.lineTo(-r * 1.65, r * 0.55);
      ctx.closePath();
      ctx.fill();
      // Fierce Eye
      ctx.fillStyle = '#f43f5e';
      ctx.beginPath();
      ctx.arc(r * 0.55, -r * 0.15, r * 0.15, 0, Math.PI * 2);
      ctx.fill();
    } else {
      // Octopus / Squid / Coral & Crystal Golems
      ctx.fillStyle = c1;
      ctx.beginPath();
      ctx.arc(0, 0, r, 0, Math.PI * 2);
      ctx.fill();
      ctx.strokeStyle = c2;
      ctx.lineWidth = 3;
      ctx.stroke();
      // Glowing Eyes
      ctx.fillStyle = '#fef08a';
      ctx.beginPath();
      ctx.arc(-r * 0.32, -r * 0.15, r * 0.18, 0, Math.PI * 2);
      ctx.arc(r * 0.32, -r * 0.15, r * 0.18, 0, Math.PI * 2);
      ctx.fill();
    }

    // Mini Enemy Health Bar
    if (e.hp < e.maxHp) {
      const bw = r * 1.8;
      ctx.fillStyle = 'rgba(0,0,0,0.65)';
      ctx.fillRect(-bw / 2, -r - 12, bw, 5);
      ctx.fillStyle = '#fb7185';
      ctx.fillRect(-bw / 2, -r - 12, bw * Math.max(0, e.hp / e.maxHp), 5);
    }

    ctx.restore();
  },

  /**
   * Renders Multi-Phase Ocean Bosses
   */
  drawBoss(ctx, b, time) {
    ctx.save();
    ctx.translate(b.x, b.y);
    const r = b.radius;
    const c1 = b.hurtFlash > 0 ? '#ffffff' : b.color1;
    const c2 = b.hurtFlash > 0 ? '#fecdd3' : b.color2;

    // Massive Boss Aura
    const aura = ctx.createRadialGradient(0, 0, r * 0.4, 0, 0, r * 2.1);
    aura.addColorStop(0, c1 + '55');
    aura.addColorStop(1, 'rgba(0,0,0,0)');
    ctx.fillStyle = aura;
    ctx.beginPath();
    ctx.arc(0, 0, r * 2.1, 0, Math.PI * 2);
    ctx.fill();

    // 8 Giant Animated Tentacles / Appendages
    for (let i = 0; i < 8; i++) {
      const baseAng = (i / 8) * Math.PI * 2 + Math.sin(time * 2 + i) * 0.18;
      ctx.strokeStyle = i % 2 === 0 ? c1 : c2;
      ctx.lineWidth = 8;
      ctx.lineCap = 'round';
      ctx.beginPath();
      ctx.moveTo(Math.cos(baseAng) * r * 0.7, Math.sin(baseAng) * r * 0.7);
      const midX = Math.cos(baseAng + 0.3) * r * 1.35;
      const midY = Math.sin(baseAng + 0.3) * r * 1.35;
      const endX = Math.cos(baseAng - 0.25) * r * 1.85;
      const endY = Math.sin(baseAng - 0.25) * r * 1.85;
      ctx.quadraticCurveTo(midX, midY, endX, endY);
      ctx.stroke();
    }

    // Boss Core Body
    const bodyGrad = ctx.createRadialGradient(-r * 0.25, -r * 0.25, 6, 0, 0, r);
    bodyGrad.addColorStop(0, c1);
    bodyGrad.addColorStop(1, c2);
    ctx.fillStyle = bodyGrad;
    ctx.beginPath();
    ctx.arc(0, 0, r, 0, Math.PI * 2);
    ctx.fill();
    ctx.strokeStyle = '#fef08a';
    ctx.lineWidth = 3;
    ctx.stroke();

    // Boss Crown / Horns
    ctx.fillStyle = '#fbbf24';
    ctx.beginPath();
    ctx.moveTo(-r * 0.45, -r * 0.85);
    ctx.lineTo(-r * 0.55, -r * 1.3);
    ctx.lineTo(-r * 0.2, -r * 0.98);
    ctx.lineTo(0, -r * 1.4);
    ctx.lineTo(r * 0.2, -r * 0.98);
    ctx.lineTo(r * 0.55, -r * 1.3);
    ctx.lineTo(r * 0.45, -r * 0.85);
    ctx.closePath();
    ctx.fill();

    // Boss Glowing Eyes
    ctx.fillStyle = '#fef08a';
    ctx.beginPath();
    ctx.arc(-r * 0.32, -r * 0.12, r * 0.18, 0, Math.PI * 2);
    ctx.arc(r * 0.32, -r * 0.12, r * 0.18, 0, Math.PI * 2);
    ctx.fill();
    ctx.fillStyle = '#e11d48';
    ctx.beginPath();
    ctx.arc(-r * 0.32, -r * 0.12, r * 0.08, 0, Math.PI * 2);
    ctx.arc(r * 0.32, -r * 0.12, r * 0.08, 0, Math.PI * 2);
    ctx.fill();

    ctx.restore();
  }
};

/* -------------------- 5. CORE GAMEPLAY ENGINE -------------------- */

const Game = {
  canvas: null,
  ctx: null,
  width: 0,
  height: 0,
  state: 'menu', // 'menu', 'playing', 'paused', 'gameover', 'victory'
  lastTime: 0,
  elapsedTime: 0,
  shakeTime: 0,
  shakeIntensity: 0,

  // Active Level & Mission State
  currentLevelId: 1,
  levelData: null,
  missionProgress: 0,
  missionTarget: 10,
  levelScore: 0,
  levelPearls: 0,
  levelGolden: 0,
  levelCrystals: 0,
  spawnTimer: 0,

  // Entities
  player: null,
  projectiles: [],
  enemyProjectiles: [],
  enemies: [],
  boss: null,
  objectives: [], // Rescue cages or destructible structures
  pickups: [],
  particles: [],
  floatingTexts: [],
  lightningArcs: [],

  // Input State
  keys: {},
  mouse: { x: 0, y: 0, down: false },
  touch: {
    joyActive: false,
    joyId: null,
    joyStartX: 0,
    joyStartY: 0,
    joyDx: 0,
    joyDy: 0,
    shooting: false
  },

  init() {
    this.canvas = document.getElementById('gameCanvas');
    this.ctx = this.canvas.getContext('2d');
    this.resize();
    window.addEventListener('resize', () => this.resize());

    Storage.load();
    this.setupInputs();
    UI.init();

    requestAnimationFrame(ts => this.loop(ts));
  },

  resize() {
    this.width = window.innerWidth;
    this.height = window.innerHeight;
    this.canvas.width = this.width;
    this.canvas.height = this.height;
    Renderer.initEnvironment(this.width, this.height);
  },

  setupInputs() {
    window.addEventListener('keydown', e => {
      this.keys[e.code] = true;
      if (this.state === 'playing') {
        if (e.code === 'Space') {
          e.preventDefault();
          this.triggerSpecial();
        } else if (e.code === 'KeyE') {
          this.triggerShield();
        } else if (e.code === 'ShiftLeft' || e.code === 'ShiftRight') {
          this.triggerDash();
        } else if (e.code === 'KeyQ') {
          this.cycleWeapon();
        } else if (e.code.startsWith('Digit')) {
          const idx = parseInt(e.code.replace('Digit', ''), 10) - 1;
          if (idx >= 0 && idx < WEAPONS.length) {
            const wId = WEAPONS[idx].id;
            if (Storage.data.unlockedWeapons.includes(wId)) {
              Storage.data.selectedWeapon = wId;
              Storage.save();
              UI.updateHUD();
            }
          }
        } else if (e.code === 'Escape') {
          this.togglePause();
        }
      } else if (this.state === 'paused' && e.code === 'Escape') {
        this.togglePause();
      }
    });

    window.addEventListener('keyup', e => {
      this.keys[e.code] = false;
    });

    this.canvas.addEventListener('mousemove', e => {
      this.mouse.x = e.clientX;
      this.mouse.y = e.clientY;
    });

    this.canvas.addEventListener('mousedown', e => {
      if (e.button === 0) {
        AudioEngine.init();
        this.mouse.down = true;
        this.mouse.x = e.clientX;
        this.mouse.y = e.clientY;
      }
    });

    window.addEventListener('mouseup', () => {
      this.mouse.down = false;
    });

    // Virtual Joystick Multi-Touch
    const joyZone = document.getElementById('joystickZone');
    const joyKnob = document.getElementById('joystickKnob');

    joyZone.addEventListener('touchstart', e => {
      e.preventDefault();
      AudioEngine.init();
      const t = e.changedTouches[0];
      const rect = joyZone.getBoundingClientRect();
      this.touch.joyActive = true;
      this.touch.joyId = t.identifier;
      this.touch.joyStartX = rect.left + rect.width / 2;
      this.touch.joyStartY = rect.top + rect.height / 2;
      this.updateJoystick(t.clientX, t.clientY, joyKnob);
    }, { passive: false });

    window.addEventListener('touchmove', e => {
      if (!this.touch.joyActive) return;
      for (let i = 0; i < e.changedTouches.length; i++) {
        const t = e.changedTouches[i];
        if (t.identifier === this.touch.joyId) {
          this.updateJoystick(t.clientX, t.clientY, joyKnob);
        }
      }
    }, { passive: false });

    const endJoy = e => {
      for (let i = 0; i < e.changedTouches.length; i++) {
        if (e.changedTouches[i].identifier === this.touch.joyId) {
          this.touch.joyActive = false;
          this.touch.joyId = null;
          this.touch.joyDx = 0;
          this.touch.joyDy = 0;
          joyKnob.style.transform = 'translate(0px, 0px)';
        }
      }
    };
    window.addEventListener('touchend', endJoy);
    window.addEventListener('touchcancel', endJoy);

    // Touch Action Buttons
    const bindHoldBtn = (id, onDown, onUp) => {
      const el = document.getElementById(id);
      el.addEventListener('touchstart', e => {
        e.preventDefault();
        AudioEngine.init();
        el.classList.add('active');
        onDown();
      }, { passive: false });
      el.addEventListener('touchend', e => {
        e.preventDefault();
        el.classList.remove('active');
        if (onUp) onUp();
      }, { passive: false });
    };

    bindHoldBtn('btnTouchShoot', () => { this.touch.shooting = true; }, () => { this.touch.shooting = false; });
    bindHoldBtn('btnTouchSpecial', () => this.triggerSpecial());
    bindHoldBtn('btnTouchShield', () => this.triggerShield());
    bindHoldBtn('btnTouchDash', () => this.triggerDash());
    bindHoldBtn('btnTouchWeapon', () => this.cycleWeapon());
  },

  updateJoystick(cx, cy, knobEl) {
    const maxDist = 42;
    let dx = cx - this.touch.joyStartX;
    let dy = cy - this.touch.joyStartY;
    const dist = Math.hypot(dx, dy);
    if (dist > maxDist) {
      dx = (dx / dist) * maxDist;
      dy = (dy / dist) * maxDist;
    }
    this.touch.joyDx = dx / maxDist;
    this.touch.joyDy = dy / maxDist;
    knobEl.style.transform = `translate(${dx}px, ${dy}px)`;
  },

  continueGame() {
    AudioEngine.init();
    AudioEngine.sfxClick();
    this.startLevel(Storage.data.highestUnlockedLevel || 1);
  },

  startLevel(levelId) {
    AudioEngine.init();
    UI.closeModals();

    const lvl = LEVELS.find(l => l.id === levelId) || LEVELS[0];
    this.currentLevelId = lvl.id;
    this.levelData = lvl;
    this.state = 'playing';

    // Reset Level Counters
    this.missionProgress = 0;
    this.missionTarget = lvl.type === 'boss' ? 1 : lvl.target;
    this.levelScore = 0;
    this.levelPearls = 0;
    this.levelGolden = 0;
    this.levelCrystals = 0;
    this.spawnTimer = 0.6;

    // Clear Entities
    this.projectiles = [];
    this.enemyProjectiles = [];
    this.enemies = [];
    this.boss = null;
    this.objectives = [];
    this.pickups = [];
    this.particles = [];
    this.floatingTexts = [];
    this.lightningArcs = [];

    // Initialize Player from Selected Hero + Upgrades
    const hero = HEROES.find(h => h.id === Storage.data.selectedHero) || HEROES[0];
    const upg = Storage.data.upgrades;

    const maxHp = hero.hp + upg.maxHp * 25;
    const maxEnergy = hero.energy + upg.energy * 20;
    const moveSpeed = hero.speed + upg.speed * 18;

    this.player = {
      hero,
      x: this.width * 0.18,
      y: this.height * 0.5,
      vx: 0,
      vy: 0,
      radius: 24,
      hp: maxHp,
      maxHp,
      energy: maxEnergy,
      maxEnergy,
      speed: moveSpeed,
      fireCooldown: 0,
      shieldTimer: 0,
      dashTimer: 0,
      invulnTimer: 1.0,
      hurtFlash: 0,
      doubleDamageTimer: 0,
      rapidFireTimer: 0,
      speedBoostTimer: 0
    };

    // Spawn Mission-Specific Objects (Rescue Cages / Destructible Towers)
    if (lvl.type === 'rescue' || lvl.type === 'destroy') {
      for (let i = 0; i < lvl.target; i++) {
        this.objectives.push({
          kind: lvl.type,
          x: this.width * (0.45 + Math.random() * 0.45),
          y: this.height * (0.18 + ((i + 0.5) / lvl.target) * 0.62),
          radius: 26,
          hp: lvl.type === 'destroy' ? 140 + lvl.id * 12 : 1,
          maxHp: lvl.type === 'destroy' ? 140 + lvl.id * 12 : 1,
          pulse: Math.random() * 6
        });
      }
    }

    // Spawn Boss if Boss Level
    if (lvl.type === 'boss') {
      this.spawnBoss(lvl.bossId);
      AudioEngine.setMusicMode('boss');
    } else {
      AudioEngine.setMusicMode('battle');
    }

    // Toggle UI Panels
    document.getElementById('mainMenu').classList.add('hidden');
    document.getElementById('menuTopBar').classList.add('hidden');
    document.getElementById('gameHUD').classList.remove('hidden');
    UI.updateHUD();
  },

  spawnBoss(bossId) {
    const template = BOSS_TYPES[bossId] || BOSS_TYPES.dark_jelly_king;
    this.boss = {
      id: bossId,
      name: template.name,
      x: this.width * 0.82,
      y: this.height * 0.5,
      vx: 0,
      vy: template.speed,
      radius: template.radius,
      hp: template.hp,
      maxHp: template.hp,
      speed: template.speed,
      color1: template.color1,
      color2: template.color2,
      damage: template.damage,
      attackTimer: 1.8,
      chargeTimer: 4.5,
      isCharging: false,
      hurtFlash: 0,
      phase: 1
    };

    AudioEngine.sfxBossWarning();
    const banner = document.getElementById('bossBanner');
    document.getElementById('bossBannerTitle').textContent = template.name;
    banner.classList.remove('hidden');
    setTimeout(() => banner.classList.add('hidden'), 2600);
  },

  restartLevel() {
    this.startLevel(this.currentLevelId);
  },

  nextLevel() {
    const nextId = Math.min(25, this.currentLevelId + 1);
    this.startLevel(nextId);
  },

  exitToMenu() {
    this.state = 'menu';
    UI.closeModals();
    document.getElementById('gameHUD').classList.add('hidden');
    document.getElementById('bossBanner').classList.add('hidden');
    document.getElementById('mainMenu').classList.remove('hidden');
    document.getElementById('menuTopBar').classList.remove('hidden');
    AudioEngine.setMusicMode('menu');
    UI.refreshAll();
  },

  togglePause() {
    if (this.state === 'playing') {
      this.state = 'paused';
      UI.openModal('pauseModal');
    } else if (this.state === 'paused') {
      this.state = 'playing';
      UI.closeModals();
      this.lastTime = performance.now();
    }
  },

  cycleWeapon() {
    const unlocked = Storage.data.unlockedWeapons;
    if (unlocked.length <= 1) return;
    const idx = unlocked.indexOf(Storage.data.selectedWeapon);
    const next = unlocked[(idx + 1) % unlocked.length];
    Storage.data.selectedWeapon = next;
    Storage.save();
    AudioEngine.sfxClick();
    UI.updateHUD();
    const wObj = WEAPONS.find(w => w.id === next);
    if (wObj) this.addFloatingText(this.player.x, this.player.y - 35, `${wObj.icon} ${wObj.name}`, '#67e8f9');
  },

  triggerSpecial() {
    if (this.state !== 'playing' || !this.player) return;
    const cost = 45;
    if (this.player.energy < cost) {
      this.addFloatingText(this.player.x, this.player.y - 30, 'Not enough Energy!', '#fda4af');
      return;
    }
    this.player.energy -= cost;
    AudioEngine.sfxSpecial();
    this.addShake(0.35, 9);

    const upgMult = 1 + Storage.data.upgrades.special * 0.22;
    const dmg = Math.round(95 * this.player.hero.damage * upgMult);
    const specType = this.player.hero.specialType;

    this.addFloatingText(this.player.x, this.player.y - 42, `✨ ${this.player.hero.specialName}!`, '#fde047');

    // Expanding Magical Shockwave Ring
    this.spawnRingParticle(this.player.x, this.player.y, this.player.hero.color1, 260);

    if (specType === 'shield_burst') {
      this.player.shieldTimer = 4.5 + Storage.data.upgrades.shield * 1.2;
    }

    // Radial Burst of Homing/Piercing Energy Orbs
    const count = 14;
    for (let i = 0; i < count; i++) {
      const ang = (i / count) * Math.PI * 2;
      this.projectiles.push({
        x: this.player.x,
        y: this.player.y,
        vx: Math.cos(ang) * 640,
        vy: Math.sin(ang) * 640,
        radius: 12,
        damage: dmg * 0.65,
        color: this.player.hero.color1,
        coreColor: '#ffffff',
        pierce: 2,
        splash: 60,
        life: 1.2
      });
    }

    // Direct Lightning / Area Damage to All Visible Enemies & Boss
    this.enemies.forEach(e => {
      if (Math.hypot(e.x - this.player.x, e.y - this.player.y) < 420) {
        this.damageEnemy(e, dmg);
        this.lightningArcs.push({ x1: this.player.x, y1: this.player.y, x2: e.x, y2: e.y, life: 0.28, color: this.player.hero.glow });
      }
    });
    if (this.boss) {
      this.damageBoss(dmg * 1.25);
      this.lightningArcs.push({ x1: this.player.x, y1: this.player.y, x2: this.boss.x, y2: this.boss.y, life: 0.35, color: '#fde047' });
    }
  },

  triggerShield() {
    if (this.state !== 'playing' || !this.player) return;
    const cost = 30;
    if (this.player.energy < cost) {
      this.addFloatingText(this.player.x, this.player.y - 30, 'Low Energy!', '#fda4af');
      return;
    }
    this.player.energy -= cost;
    this.player.shieldTimer = 3.8 + Storage.data.upgrades.shield * 1.2;
    AudioEngine.sfxShield();
    this.addFloatingText(this.player.x, this.player.y - 35, '🛡️ Bubble Shield!', '#67e8f9');
  },

  triggerDash() {
    if (this.state !== 'playing' || !this.player) return;
    const cost = 20;
    if (this.player.energy < cost || this.player.dashTimer > 0) return;
    this.player.energy -= cost;
    this.player.dashTimer = 0.28;
    this.player.invulnTimer = Math.max(this.player.invulnTimer, 0.35);
    AudioEngine.playTone(300, 720, 0.16, 'sine', 0.6);
    for (let i = 0; i < 10; i++) {
      this.spawnBubbleParticle(this.player.x, this.player.y, '#67e8f9');
    }
  },

  addShake(duration, intensity) {
    if (!Storage.data.settings.screenShake) return;
    this.shakeTime = Math.max(this.shakeTime, duration);
    this.shakeIntensity = Math.max(this.shakeIntensity, intensity);
  },

  /* -------------------- MAIN UPDATE LOOP -------------------- */
  loop(timestamp) {
    const dt = Math.min(0.05, (timestamp - this.lastTime) / 1000 || 0.016);
    this.lastTime = timestamp;
    this.elapsedTime += dt;

    if (this.state === 'playing') {
      this.update(dt);
    }
    this.render();

    requestAnimationFrame(ts => this.loop(ts));
  },

  update(dt) {
    const p = this.player;
    if (!p) return;

    // Screen shake decay
    if (this.shakeTime > 0) this.shakeTime -= dt;

    // Regenerate Energy
    const energyRegen = 11 + Storage.data.upgrades.energy * 2.5;
    p.energy = Math.min(p.maxEnergy, p.energy + energyRegen * dt);

    // Timers
    if (p.fireCooldown > 0) p.fireCooldown -= dt;
    if (p.shieldTimer > 0) p.shieldTimer -= dt;
    if (p.dashTimer > 0) p.dashTimer -= dt;
    if (p.invulnTimer > 0) p.invulnTimer -= dt;
    if (p.hurtFlash > 0) p.hurtFlash -= dt;
    if (p.doubleDamageTimer > 0) p.doubleDamageTimer -= dt;
    if (p.rapidFireTimer > 0) p.rapidFireTimer -= dt;
    if (p.speedBoostTimer > 0) p.speedBoostTimer -= dt;

    // Movement Input (Keyboard + Virtual Joystick)
    let moveX = 0;
    let moveY = 0;
    if (this.keys['KeyW'] || this.keys['ArrowUp']) moveY -= 1;
    if (this.keys['KeyS'] || this.keys['ArrowDown']) moveY += 1;
    if (this.keys['KeyA'] || this.keys['ArrowLeft']) moveX -= 1;
    if (this.keys['KeyD'] || this.keys['ArrowRight']) moveX += 1;

    if (this.touch.joyActive) {
      moveX = this.touch.joyDx;
      moveY = this.touch.joyDy;
    }

    const mag = Math.hypot(moveX, moveY);
    if (mag > 1) {
      moveX /= mag;
      moveY /= mag;
    }

    let currentSpeed = p.speed;
    if (p.speedBoostTimer > 0) currentSpeed *= 1.35;
    if (p.dashTimer > 0) currentSpeed *= 2.4;

    p.vx = moveX * currentSpeed;
    p.vy = moveY * currentSpeed;
    p.x += p.vx * dt;
    p.y += p.vy * dt;

    // Clamp Player inside Ocean Playfield
    const pad = p.radius + 12;
    p.x = Math.max(pad, Math.min(this.width - pad, p.x));
    p.y = Math.max(pad + 55, Math.min(this.height - pad - 35, p.y));

    // Swimming trail bubbles
    if (mag > 0.2 && Math.random() < 0.35) {
      this.spawnBubbleParticle(p.x - moveX * 14, p.y + 10, p.hero.glow);
    }

    // Shooting Input (Mouse Hold OR Touch Shoot Button)
    if ((this.mouse.down || this.touch.shooting) && p.fireCooldown <= 0) {
      this.firePlayerWeapon();
    }

    // Enemy Spawning
    this.updateSpawner(dt);

    // Update Player Projectiles
    this.updatePlayerProjectiles(dt);

    // Update Enemy Projectiles
    this.updateEnemyProjectiles(dt);

    // Update Enemies
    this.updateEnemies(dt);

    // Update Boss
    if (this.boss) {
      this.updateBoss(dt);
    }

    // Update Mission Objectives (Rescue Cages / Destructibles)
    this.updateObjectives(dt);

    // Update Pickups (Pearls, Crystals, Power-ups)
    this.updatePickups(dt);

    // Update Visual Particles & Floating Texts
    this.updateEffects(dt);

    // Refresh HUD
    UI.updateHUD();
  },

  firePlayerWeapon() {
    const p = this.player;
    const wData = WEAPONS.find(w => w.id === Storage.data.selectedWeapon) || WEAPONS[0];
    const wLevel = Storage.data.weaponLevels[wData.id] || 1;

    // Aim Direction: Towards Mouse on Desktop, or Nearest Enemy / Rightward on Touch
    let aimAngle = 0;
    if (this.touch.shooting && !this.mouse.down) {
      const target = this.findNearestTarget(p.x, p.y);
      if (target) {
        aimAngle = Math.atan2(target.y - p.y, target.x - p.x);
      } else if (Math.hypot(this.touch.joyDx, this.touch.joyDy) > 0.2) {
        aimAngle = Math.atan2(this.touch.joyDy, this.touch.joyDx);
      }
    } else {
      aimAngle = Math.atan2(this.mouse.y - p.y, this.mouse.x - p.x);
    }

    const rateMult = (1 + (wLevel - 1) * 0.1) * (p.rapidFireTimer > 0 ? 1.65 : 1);
    p.fireCooldown = wData.fireRate / rateMult;

    const dmgMult = p.hero.damage * (1 + Storage.data.upgrades.damage * 0.15) * (1 + (wLevel - 1) * 0.22) * (p.doubleDamageTimer > 0 ? 2 : 1);
    const dmg = Math.round(wData.damage * dmgMult);
    const projSize = wData.size + (wLevel - 1) * 1.1;

    const count = wData.spread;
    const spreadStep = 0.16;

    for (let i = 0; i < count; i++) {
      const offset = (i - (count - 1) / 2) * spreadStep;
      const ang = aimAngle + offset;
      this.projectiles.push({
        x: p.x + Math.cos(ang) * (p.radius + 8),
        y: p.y + Math.sin(ang) * (p.radius + 8),
        vx: Math.cos(ang) * wData.speed,
        vy: Math.sin(ang) * wData.speed,
        radius: projSize,
        damage: dmg,
        color: wData.color,
        coreColor: wData.coreColor,
        pierce: wData.pierce || 0,
        splash: wData.splash || 0,
        chain: wData.chain || false,
        life: 2.0
      });
    }

    AudioEngine.sfxShoot(wData.id);
  },

  findNearestTarget(x, y) {
    let best = null;
    let bestDist = Infinity;
    this.enemies.forEach(e => {
      const d = Math.hypot(e.x - x, e.y - y);
      if (d < bestDist) {
        bestDist = d;
        best = e;
      }
    });
    if (this.boss) {
      const d = Math.hypot(this.boss.x - x, this.boss.y - y);
      if (d < bestDist) best = this.boss;
    }
    this.objectives.forEach(o => {
      if (o.kind === 'destroy') {
        const d = Math.hypot(o.x - x, o.y - y);
        if (d < bestDist) {
          bestDist = d;
          best = o;
        }
      }
    });
    return best;
  },

  updateSpawner(dt) {
    const maxConcurrent = this.levelData.type === 'boss' ? 4 : Math.min(11, 5 + Math.floor(this.currentLevelId / 3));
    if (this.enemies.length >= maxConcurrent) return;

    this.spawnTimer -= dt;
    if (this.spawnTimer <= 0) {
      this.spawnTimer = Math.max(0.85, 2.2 - this.currentLevelId * 0.045);
      const pool = this.levelData.enemyPool;
      const key = pool[Math.floor(Math.random() * pool.length)];
      this.spawnEnemy(key);
    }
  },

  spawnEnemy(typeKey) {
    const t = ENEMY_TYPES[typeKey] || ENEMY_TYPES.evil_pink;
    const scale = 1 + (this.currentLevelId - 1) * 0.045;
    const spawnY = 75 + Math.random() * (this.height - 140);

    this.enemies.push({
      typeKey,
      name: t.name,
      x: this.width + 35,
      y: spawnY,
      vx: -t.speed,
      vy: 0,
      radius: t.radius,
      hp: Math.round(t.hp * scale),
      maxHp: Math.round(t.hp * scale),
      speed: t.speed,
      damage: Math.round(t.damage * scale),
      color: t.color,
      color2: t.color2,
      ai: t.ai,
      shoots: t.shoots,
      fireTimer: 1.2 + Math.random() * (t.fireRate || 2.5),
      fireRate: t.fireRate || 2.5,
      pearls: t.pearls,
      score: t.score,
      phase: Math.random() * Math.PI * 2,
      hurtFlash: 0,
      puffScale: 1
    });
  },

  updatePlayerProjectiles(dt) {
    for (let i = this.projectiles.length - 1; i >= 0; i--) {
      const pr = this.projectiles[i];
      pr.x += pr.vx * dt;
      pr.y += pr.vy * dt;
      pr.life -= dt;

      if (pr.life <= 0 || pr.x < -40 || pr.x > this.width + 40 || pr.y < -40 || pr.y > this.height + 40) {
        this.projectiles.splice(i, 1);
        continue;
      }

      let hit = false;

      // Check Enemies
      for (let j = this.enemies.length - 1; j >= 0; j--) {
        const e = this.enemies[j];
        if (Math.hypot(pr.x - e.x, pr.y - e.y) < pr.radius + e.radius) {
          this.damageEnemy(e, pr.damage);
          if (pr.splash > 0) this.applySplashDamage(pr.x, pr.y, pr.splash, pr.damage * 0.6);
          if (pr.chain) this.applyChainLightning(e, pr.damage * 0.55);
          hit = true;
          if (pr.pierce > 0) {
            pr.pierce--;
            hit = false;
          }
          break;
        }
      }

      // Check Boss
      if (!hit && this.boss) {
        if (Math.hypot(pr.x - this.boss.x, pr.y - this.boss.y) < pr.radius + this.boss.radius) {
          this.damageBoss(pr.damage);
          if (pr.splash > 0) this.applySplashDamage(pr.x, pr.y, pr.splash, pr.damage * 0.5);
          hit = true;
        }
      }

      // Check Destructible Objectives
      if (!hit) {
        for (let k = this.objectives.length - 1; k >= 0; k--) {
          const obj = this.objectives[k];
          if (obj.kind === 'destroy' && Math.hypot(pr.x - obj.x, pr.y - obj.y) < pr.radius + obj.radius) {
            obj.hp -= pr.damage;
            this.spawnExplosion(pr.x, pr.y, '#38bdf8', 5);
            AudioEngine.sfxHit();
            if (obj.hp <= 0) {
              this.spawnExplosion(obj.x, obj.y, '#22d3ee', 22);
              AudioEngine.sfxExplosion();
              this.addShake(0.25, 7);
              this.objectives.splice(k, 1);
              this.advanceMission(1);
            }
            hit = true;
            break;
          }
        }
      }

      if (hit) {
        this.projectiles.splice(i, 1);
      }
    }
  },

  applySplashDamage(x, y, radius, dmg) {
    this.spawnRingParticle(x, y, '#f472b6', radius);
    this.enemies.forEach(e => {
      if (Math.hypot(e.x - x, e.y - y) <= radius + e.radius) {
        this.damageEnemy(e, dmg);
      }
    });
    if (this.boss && Math.hypot(this.boss.x - x, this.boss.y - y) <= radius + this.boss.radius) {
      this.damageBoss(dmg);
    }
  },

  applyChainLightning(sourceEnemy, dmg) {
    for (let i = 0; i < this.enemies.length; i++) {
      const other = this.enemies[i];
      if (other !== sourceEnemy && Math.hypot(other.x - sourceEnemy.x, other.y - sourceEnemy.y) < 165) {
        this.damageEnemy(other, dmg);
        this.lightningArcs.push({ x1: sourceEnemy.x, y1: sourceEnemy.y, x2: other.x, y2: other.y, life: 0.18, color: '#fde047' });
        break;
      }
    }
  },

  updateEnemyProjectiles(dt) {
    const p = this.player;
    for (let i = this.enemyProjectiles.length - 1; i >= 0; i--) {
      const ep = this.enemyProjectiles[i];
      ep.x += ep.vx * dt;
      ep.y += ep.vy * dt;
      ep.life -= dt;

      if (ep.life <= 0 || ep.x < -50 || ep.x > this.width + 50 || ep.y < -50 || ep.y > this.height + 50) {
        this.enemyProjectiles.splice(i, 1);
        continue;
      }

      if (Math.hypot(ep.x - p.x, ep.y - p.y) < ep.radius + p.radius * 0.85) {
        this.damagePlayer(ep.damage);
        this.enemyProjectiles.splice(i, 1);
      }
    }
  },

  updateEnemies(dt) {
    const p = this.player;
    for (let i = this.enemies.length - 1; i >= 0; i--) {
      const e = this.enemies[i];
      e.phase += dt * 3;
      if (e.hurtFlash > 0) e.hurtFlash -= dt;

      // AI Movement Behaviors
      const dx = p.x - e.x;
      const dy = p.y - e.y;
      const dist = Math.hypot(dx, dy) || 1;

      if (e.ai === 'sine') {
        e.vx = -e.speed;
        e.vy = Math.sin(e.phase) * 95;
      } else if (e.ai === 'chase') {
        e.vx = (dx / dist) * e.speed;
        e.vy = (dy / dist) * e.speed;
      } else if (e.ai === 'shark_charge') {
        e.vx = (dx / dist) * e.speed * 1.15;
        e.vy = (dy / dist) * e.speed * 0.85;
      } else if (e.ai === 'puff') {
        e.vx = -e.speed * 0.75;
        e.vy = Math.cos(e.phase) * 55;
        e.puffScale = 1 + Math.max(0, Math.sin(e.phase * 1.2)) * 0.35;
      } else if (e.ai === 'teleport') {
        e.vx = -e.speed * 0.9;
        e.vy = Math.sin(e.phase * 1.5) * 110;
        if (Math.random() < 0.006 && e.x > this.width * 0.35) {
          e.y = 80 + Math.random() * (this.height - 150);
          this.spawnExplosion(e.x, e.y, '#a855f7', 8);
        }
      } else {
        e.vx = -e.speed;
        e.vy = Math.sin(e.phase) * 70;
      }

      e.x += e.vx * dt;
      e.y += e.vy * dt;
      e.y = Math.max(65, Math.min(this.height - 55, e.y));

      // Wrap around if swims off left edge
      if (e.x < -50) {
        e.x = this.width + 40;
        e.y = 80 + Math.random() * (this.height - 150);
      }

      // Enemy Ranged Attack
      if (e.shoots) {
        e.fireTimer -= dt;
        if (e.fireTimer <= 0 && e.x < this.width - 30) {
          e.fireTimer = e.fireRate;
          const ang = Math.atan2(p.y - e.y, p.x - e.x);
          this.enemyProjectiles.push({
            x: e.x,
            y: e.y,
            vx: Math.cos(ang) * 280,
            vy: Math.sin(ang) * 280,
            radius: 7,
            damage: e.damage,
            color: e.color,
            life: 3.5
          });
        }
      }

      // Collision with Player
      if (Math.hypot(e.x - p.x, e.y - p.y) < e.radius + p.radius * 0.8) {
        this.damagePlayer(e.damage);
      }
    }
  },

  updateBoss(dt) {
    const b = this.boss;
    const p = this.player;
    if (b.hurtFlash > 0) b.hurtFlash -= dt;

    // Phase 2 Rage below 50% HP
    if (b.hp < b.maxHp * 0.5 && b.phase === 1) {
      b.phase = 2;
      this.addFloatingText(b.x, b.y - b.radius - 20, '🔥 BOSS ENRAGED!', '#f43f5e');
      this.addShake(0.4, 10);
    }

    const speedMult = b.phase === 2 ? 1.35 : 1;

    // Boss Movement
    if (b.isCharging) {
      b.x += b.vx * dt;
      b.y += b.vy * dt;
      if (b.x < this.width * 0.2 || b.x > this.width * 0.88 || b.y < 90 || b.y > this.height - 80) {
        b.isCharging = false;
        b.x = Math.max(this.width * 0.22, Math.min(this.width * 0.86, b.x));
      }
    } else {
      const targetX = this.width * 0.78 + Math.sin(this.elapsedTime * 1.4) * 65;
      b.x += (targetX - b.x) * 2 * dt;
      b.y += b.vy * speedMult * dt;
      if (b.y < 110) {
        b.y = 110;
        b.vy = Math.abs(b.speed);
      } else if (b.y > this.height - 95) {
        b.y = this.height - 95;
        b.vy = -Math.abs(b.speed);
      }
    }

    // Boss Attack Patterns
    b.attackTimer -= dt * speedMult;
    if (b.attackTimer <= 0) {
      b.attackTimer = 1.65;
      const baseAng = Math.atan2(p.y - b.y, p.x - b.x);
      const fanCount = b.phase === 2 ? 5 : 3;
      for (let i = 0; i < fanCount; i++) {
        const ang = baseAng + (i - (fanCount - 1) / 2) * 0.24;
        this.enemyProjectiles.push({
          x: b.x - b.radius * 0.5,
          y: b.y,
          vx: Math.cos(ang) * 340,
          vy: Math.sin(ang) * 340,
          radius: 10,
          damage: b.damage,
          color: b.color1,
          life: 4.0
        });
      }
    }

    // Boss Charge Lunge
    b.chargeTimer -= dt * speedMult;
    if (b.chargeTimer <= 0) {
      b.chargeTimer = 5.2;
      b.isCharging = true;
      const ang = Math.atan2(p.y - b.y, p.x - b.x);
      b.vx = Math.cos(ang) * 430;
      b.vy = Math.sin(ang) * 430;
      this.addFloatingText(b.x, b.y - b.radius, '⚡ CHARGE!', '#fbbf24');
    }

    // Boss Body Collision
    if (Math.hypot(b.x - p.x, b.y - p.y) < b.radius + p.radius * 0.8) {
      this.damagePlayer(b.damage);
    }
  },

  updateObjectives(dt) {
    const p = this.player;
    for (let i = this.objectives.length - 1; i >= 0; i--) {
      const obj = this.objectives[i];
      obj.pulse += dt * 3;
      if (obj.kind === 'rescue') {
        if (Math.hypot(obj.x - p.x, obj.y - p.y) < obj.radius + p.radius + 10) {
          AudioEngine.sfxPickup(true);
          this.spawnExplosion(obj.x, obj.y, '#34d399', 18);
          this.addFloatingText(obj.x, obj.y - 25, '🪼 Baby Jellyfish Rescued!', '#6ee7b7');
          this.levelScore += 250;
          this.objectives.splice(i, 1);
          this.advanceMission(1);
        }
      }
    }
  },

  updatePickups(dt) {
    const p = this.player;
    const magnetRange = p.hero.id === 'golden_jelly' ? 230 : 120;

    for (let i = this.pickups.length - 1; i >= 0; i--) {
      const pk = this.pickups[i];
      pk.life -= dt;
      pk.bob += dt * 4;
      if (pk.life <= 0) {
        this.pickups.splice(i, 1);
        continue;
      }

      const dx = p.x - pk.x;
      const dy = p.y - pk.y;
      const dist = Math.hypot(dx, dy) || 1;
      if (dist < magnetRange) {
        pk.x += (dx / dist) * 320 * dt;
        pk.y += (dy / dist) * 320 * dt;
      }

      if (dist < p.radius + pk.radius + 8) {
        this.collectPickup(pk);
        this.pickups.splice(i, 1);
      }
    }
  },

  collectPickup(pk) {
    const p = this.player;
    if (pk.type === 'pearl') {
      this.levelPearls += pk.value;
      Storage.data.pearls += pk.value;
      Storage.data.stats.totalPearls += pk.value;
      AudioEngine.sfxPickup(false);
    } else if (pk.type === 'golden') {
      this.levelGolden += pk.value;
      Storage.data.golden += pk.value;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, `+${pk.value} Golden Pearl!`, '#fde047');
    } else if (pk.type === 'crystal') {
      this.levelCrystals += pk.value;
      Storage.data.crystals += pk.value;
      Storage.data.stats.totalCrystals += pk.value;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, `+${pk.value} Crystal!`, '#e879f9');
    } else if (pk.type === 'heal') {
      p.hp = Math.min(p.maxHp, p.hp + Math.round(p.maxHp * 0.35));
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, '❤️ Health Restored!', '#fb7185');
    } else if (pk.type === 'energy') {
      p.energy = p.maxEnergy;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, '⚡ Max Energy!', '#38bdf8');
    } else if (pk.type === 'double_dmg') {
      p.doubleDamageTimer = 8.0;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, '🔥 Double Damage (8s)!', '#fb923c');
    } else if (pk.type === 'rapid_fire') {
      p.rapidFireTimer = 8.0;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, '⚡ Rapid Fire (8s)!', '#fde047');
    } else if (pk.type === 'speed_boost') {
      p.speedBoostTimer = 8.0;
      AudioEngine.sfxPickup(true);
      this.addFloatingText(p.x, p.y - 30, '💨 Turbo Swim (8s)!', '#34d399');
    }
  },

  damageEnemy(e, amount) {
    const dmg = Math.round(amount);
    e.hp -= dmg;
    e.hurtFlash = 0.1;
    AudioEngine.sfxHit();
    this.spawnBubbleParticle(e.x, e.y, e.color);

    if (e.hp <= 0) {
      const idx = this.enemies.indexOf(e);
      if (idx !== -1) {
        this.enemies.splice(idx, 1);
        this.onEnemyDefeated(e);
      }
    }
  },

  onEnemyDefeated(e) {
    AudioEngine.sfxExplosion();
    this.spawnExplosion(e.x, e.y, e.color, 16);
    this.levelScore += e.score;
    Storage.data.stats.enemiesDefeated++;
    Storage.data.dailyChallengeProgress++;

    // Drop Pearls & Occasional Crystals / Power-ups
    this.pickups.push({ type: 'pearl', x: e.x, y: e.y, radius: 10, value: e.pearls, life: 14, bob: 0 });
    if (Math.random() < 0.28) {
      this.pickups.push({ type: 'crystal', x: e.x + 12, y: e.y - 8, radius: 11, value: 1, life: 14, bob: 1 });
    }
    if (Math.random() < 0.16) {
      const buffs = ['heal', 'energy', 'double_dmg', 'rapid_fire', 'speed_boost'];
      const bType = buffs[Math.floor(Math.random() * buffs.length)];
      this.pickups.push({ type: bType, x: e.x - 10, y: e.y + 8, radius: 13, value: 1, life: 14, bob: 2 });
    }

    if (this.levelData.type === 'eliminate') {
      this.advanceMission(1);
    }
  },

  damageBoss(amount) {
    const b = this.boss;
    if (!b) return;
    b.hp -= Math.round(amount);
    b.hurtFlash = 0.08;
    AudioEngine.sfxHit();

    if (b.hp <= 0) {
      this.spawnExplosion(b.x, b.y, b.color1, 45);
      this.spawnRingParticle(b.x, b.y, '#fde047', 320);
      AudioEngine.sfxExplosion();
      this.addShake(0.65, 14);
      this.levelScore += 2500;
      Storage.data.stats.bossesDefeated++;
      this.boss = null;
      this.advanceMission(1);
    }
  },

  damagePlayer(amount) {
    const p = this.player;
    if (!p || p.invulnTimer > 0) return;

    if (p.shieldTimer > 0) {
      // Blocked by Bubble Shield!
      p.invulnTimer = 0.35;
      this.spawnRingParticle(p.x, p.y, '#67e8f9', 65);
      AudioEngine.sfxShield();
      return;
    }

    p.hp = Math.max(0, p.hp - Math.round(amount));
    p.hurtFlash = 0.18;
    p.invulnTimer = 0.75;
    this.addShake(0.25, 8);
    this.addFloatingText(p.x, p.y - 28, `-${Math.round(amount)}`, '#f43f5e');
    AudioEngine.sfxHit();

    if (p.hp <= 0) {
      this.onDefeat();
    }
  },

  advanceMission(step) {
    if (this.state !== 'playing') return;
    this.missionProgress = Math.min(this.missionTarget, this.missionProgress + step);
    if (this.missionProgress >= this.missionTarget) {
      this.onVictory();
    }
  },

  onVictory() {
    this.state = 'victory';
    AudioEngine.sfxVictory();

    const rew = this.levelData.reward;
    Storage.data.pearls += rew.pearls;
    Storage.data.golden += rew.golden;
    Storage.data.crystals += rew.crystals;
    Storage.data.stats.totalPearls += rew.pearls;
    Storage.data.stats.totalCrystals += rew.crystals;
    Storage.data.stats.levelsWon++;

    if (!Storage.data.completedLevels.includes(this.currentLevelId)) {
      Storage.data.completedLevels.push(this.currentLevelId);
    }
    if (this.currentLevelId >= Storage.data.highestUnlockedLevel && this.currentLevelId < 25) {
      Storage.data.highestUnlockedLevel = this.currentLevelId + 1;
    }
    const prevBest = Storage.data.highScores[this.currentLevelId] || 0;
    if (this.levelScore > prevBest) {
      Storage.data.highScores[this.currentLevelId] = this.levelScore;
    }

    Storage.save();
    UI.checkAchievements();

    document.getElementById('resultEmoji').textContent = '🏆';
    document.getElementById('resultTitle').textContent = 'VICTORY!';
    document.getElementById('resultTitle').className = 'title-font text-2xl sm:text-4xl font-bold text-amber-300 text-glow-pink';
    document.getElementById('resultSubtitle').textContent = `${this.levelData.title} Completed!`;
    document.getElementById('resScore').textContent = this.levelScore.toLocaleString();
    document.getElementById('resPearls').textContent = `+${this.levelPearls + rew.pearls} ⚪`;
    document.getElementById('resGolden').textContent = `+${this.levelGolden + rew.golden} 🟡`;
    document.getElementById('resCrystals').textContent = `+${this.levelCrystals + rew.crystals} 💎`;
    document.getElementById('btnNextLevel').classList.toggle('hidden', this.currentLevelId >= 25);

    UI.openModal('resultModal');
  },

  onDefeat() {
    this.state = 'gameover';
    AudioEngine.sfxDefeat();
    Storage.save();

    document.getElementById('resultEmoji').textContent = '💫';
    document.getElementById('resultTitle').textContent = 'DEFEATED';
    document.getElementById('resultTitle').className = 'title-font text-2xl sm:text-4xl font-bold text-rose-400';
    document.getElementById('resultSubtitle').textContent = 'The ocean currents overwhelmed your jellyfish hero.';
    document.getElementById('resScore').textContent = this.levelScore.toLocaleString();
    document.getElementById('resPearls').textContent = `+${this.levelPearls} ⚪`;
    document.getElementById('resGolden').textContent = `+${this.levelGolden} 🟡`;
    document.getElementById('resCrystals').textContent = `+${this.levelCrystals} 💎`;
    document.getElementById('btnNextLevel').classList.add('hidden');

    UI.openModal('resultModal');
  },

  /* -------------------- PARTICLE & VISUAL EFFECTS -------------------- */
  spawnBubbleParticle(x, y, color) {
    this.particles.push({
      type: 'bubble',
      x,
      y,
      vx: -30 + Math.random() * 60,
      vy: -30 - Math.random() * 50,
      r: 3 + Math.random() * 5,
      color,
      life: 0.55 + Math.random() * 0.35,
      maxLife: 0.9
    });
  },

  spawnExplosion(x, y, color, count = 16) {
    for (let i = 0; i < count; i++) {
      const ang = Math.random() * Math.PI * 2;
      const spd = 60 + Math.random() * 220;
      this.particles.push({
        type: 'spark',
        x,
        y,
        vx: Math.cos(ang) * spd,
        vy: Math.sin(ang) * spd,
        r: 3 + Math.random() * 5,
        color: i % 2 === 0 ? color : '#ffffff',
        life: 0.4 + Math.random() * 0.35,
        maxLife: 0.75
      });
    }
  },

  spawnRingParticle(x, y, color, maxRadius) {
    this.particles.push({
      type: 'ring',
      x,
      y,
      r: 10,
      maxR: maxRadius,
      color,
      life: 0.35,
      maxLife: 0.35
    });
  },

  addFloatingText(x, y, text, color = '#ffffff') {
    this.floatingTexts.push({ x, y, text, color, life: 1.05 });
  },

  updateEffects(dt) {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const pt = this.particles[i];
      pt.life -= dt;
      if (pt.life <= 0) {
        this.particles.splice(i, 1);
        continue;
      }
      if (pt.type === 'ring') {
        pt.r += (pt.maxR - pt.r) * 10 * dt;
      } else {
        pt.x += pt.vx * dt;
        pt.y += pt.vy * dt;
      }
    }

    for (let i = this.floatingTexts.length - 1; i >= 0; i--) {
      const ft = this.floatingTexts[i];
      ft.y -= 38 * dt;
      ft.life -= dt;
      if (ft.life <= 0) this.floatingTexts.splice(i, 1);
    }

    for (let i = this.lightningArcs.length - 1; i >= 0; i--) {
      this.lightningArcs[i].life -= dt;
      if (this.lightningArcs[i].life <= 0) this.lightningArcs.splice(i, 1);
    }
  },

  /* -------------------- MAIN RENDER LOOP -------------------- */
  render() {
    const ctx = this.ctx;
    ctx.save();

    // Screen Shake Transform
    if (this.shakeTime > 0 && this.state === 'playing') {
      const sx = (Math.random() - 0.5) * this.shakeIntensity * 2;
      const sy = (Math.random() - 0.5) * this.shakeIntensity * 2;
      ctx.translate(sx, sy);
    }

    const isMenu = this.state === 'menu';
    const worldId = isMenu ? 1 : (this.levelData?.world || 1);
    Renderer.drawBackground(ctx, this.width, this.height, this.elapsedTime, worldId, isMenu);

    if (!isMenu && this.player) {
      // 1. Render Objectives (Rescue Bubbles / Crystal Towers)
      this.objectives.forEach(obj => {
        ctx.save();
        ctx.translate(obj.x, obj.y + Math.sin(obj.pulse) * 6);
        if (obj.kind === 'rescue') {
          // Glowing Rescue Bubble with Baby Jellyfish
          ctx.strokeStyle = '#6ee7b7';
          ctx.fillStyle = 'rgba(16, 185, 129, 0.25)';
          ctx.lineWidth = 3;
          ctx.beginPath();
          ctx.arc(0, 0, obj.radius, 0, Math.PI * 2);
          ctx.fill();
          ctx.stroke();
          Renderer.drawJellyfishHero(ctx, 0, 0, 12, HEROES[0], this.elapsedTime * 1.4);
        } else {
          // Enemy Crystal Tower / Submarine Core
          ctx.fillStyle = '#0e7490';
          ctx.strokeStyle = '#22d3ee';
          ctx.lineWidth = 3;
          ctx.beginPath();
          ctx.moveTo(0, -obj.radius * 1.2);
          ctx.lineTo(obj.radius * 0.9, 0);
          ctx.lineTo(0, obj.radius * 1.2);
          ctx.lineTo(-obj.radius * 0.9, 0);
          ctx.closePath();
          ctx.fill();
          ctx.stroke();
          // HP bar
          const bw = 44;
          ctx.fillStyle = 'rgba(0,0,0,0.6)';
          ctx.fillRect(-bw / 2, -obj.radius - 14, bw, 5);
          ctx.fillStyle = '#22d3ee';
          ctx.fillRect(-bw / 2, -obj.radius - 14, bw * (obj.hp / obj.maxHp), 5);
        }
        ctx.restore();
      });

      // 2. Render Pickups (Pearls, Golden Pearls, Crystals, Power-ups)
      this.pickups.forEach(pk => {
        ctx.save();
        ctx.translate(pk.x, pk.y + Math.sin(pk.bob) * 4);
        if (pk.type === 'pearl') {
          const g = ctx.createRadialGradient(-3, -3, 1, 0, 0, pk.radius);
          g.addColorStop(0, '#ffffff');
          g.addColorStop(1, '#67e8f9');
          ctx.fillStyle = g;
          ctx.beginPath();
          ctx.arc(0, 0, pk.radius, 0, Math.PI * 2);
          ctx.fill();
        } else if (pk.type === 'golden') {
          ctx.fillStyle = '#fbbf24';
          ctx.beginPath();
          ctx.arc(0, 0, pk.radius + 2, 0, Math.PI * 2);
          ctx.fill();
          ctx.strokeStyle = '#fef08a';
          ctx.lineWidth = 2;
          ctx.stroke();
        } else if (pk.type === 'crystal') {
          ctx.fillStyle = '#e879f9';
          ctx.beginPath();
          ctx.moveTo(0, -pk.radius);
          ctx.lineTo(pk.radius * 0.75, 0);
          ctx.lineTo(0, pk.radius);
          ctx.lineTo(-pk.radius * 0.75, 0);
          ctx.closePath();
          ctx.fill();
        } else {
          // Power-up Orb
          const icons = { heal: '❤️', energy: '⚡', double_dmg: '🔥', rapid_fire: '✨', speed_boost: '💨' };
          ctx.fillStyle = 'rgba(255, 255, 255, 0.25)';
          ctx.strokeStyle = '#fde047';
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.arc(0, 0, pk.radius + 3, 0, Math.PI * 2);
          ctx.fill();
          ctx.stroke();
          ctx.font = '14px sans-serif';
          ctx.textAlign = 'center';
          ctx.textBaseline = 'middle';
          ctx.fillText(icons[pk.type] || '⭐', 0, 1);
        }
        ctx.restore();
      });

      // 3. Render Enemies & Boss
      this.enemies.forEach(e => Renderer.drawEnemy(ctx, e, this.elapsedTime));
      if (this.boss) {
        Renderer.drawBoss(ctx, this.boss, this.elapsedTime);
      }

      // 4. Render Player Jellyfish Hero
      const p = this.player;
      const tilt = (p.vy / (p.speed || 250)) * 0.65;
      if (!(p.invulnTimer > 0 && p.shieldTimer <= 0 && Math.floor(this.elapsedTime * 18) % 2 === 0)) {
        Renderer.drawJellyfishHero(ctx, p.x, p.y, p.radius, p.hero, this.elapsedTime, tilt, p.shieldTimer > 0, p.hurtFlash);
      }

      // 5. Render Player Projectiles
      this.projectiles.forEach(pr => {
        ctx.save();
        ctx.fillStyle = pr.color;
        ctx.shadowColor = pr.color;
        ctx.shadowBlur = 10;
        ctx.beginPath();
        ctx.arc(pr.x, pr.y, pr.radius, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = pr.coreColor || '#ffffff';
        ctx.beginPath();
        ctx.arc(pr.x, pr.y, pr.radius * 0.48, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      });

      // 6. Render Enemy Projectiles
      this.enemyProjectiles.forEach(ep => {
        ctx.save();
        ctx.fillStyle = ep.color;
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 1.5;
        ctx.beginPath();
        ctx.arc(ep.x, ep.y, ep.radius, 0, Math.PI * 2);
        ctx.fill();
        ctx.stroke();
        ctx.restore();
      });

      // 7. Render Lightning Arcs
      this.lightningArcs.forEach(arc => {
        ctx.save();
        ctx.strokeStyle = arc.color;
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(arc.x1, arc.y1);
        const mx = (arc.x1 + arc.x2) / 2 + (Math.random() - 0.5) * 30;
        const my = (arc.y1 + arc.y2) / 2 + (Math.random() - 0.5) * 30;
        ctx.lineTo(mx, my);
        ctx.lineTo(arc.x2, arc.y2);
        ctx.stroke();
        ctx.restore();
      });

      // 8. Render Particles & Floating Texts
      this.particles.forEach(pt => {
        ctx.save();
        ctx.globalAlpha = Math.max(0, pt.life / pt.maxLife);
        if (pt.type === 'ring') {
          ctx.strokeStyle = pt.color;
          ctx.lineWidth = 3.5;
          ctx.beginPath();
          ctx.arc(pt.x, pt.y, pt.r, 0, Math.PI * 2);
          ctx.stroke();
        } else {
          ctx.fillStyle = pt.color;
          ctx.beginPath();
          ctx.arc(pt.x, pt.y, pt.r, 0, Math.PI * 2);
          ctx.fill();
        }
        ctx.restore();
      });

      this.floatingTexts.forEach(ft => {
        ctx.save();
        ctx.globalAlpha = Math.min(1, ft.life * 1.4);
        ctx.font = 'bold 14px Fredoka, sans-serif';
        ctx.textAlign = 'center';
        ctx.fillStyle = '#090d16';
        ctx.fillText(ft.text, ft.x + 1, ft.y + 1);
        ctx.fillStyle = ft.color;
        ctx.fillText(ft.text, ft.x, ft.y);
        ctx.restore();
      });
    }

    ctx.restore();
  }
};

/* -------------------- 6. SETTINGS & UI CONTROLLER -------------------- */

const Settings = {
  toggleShake() {
    Storage.data.settings.screenShake = !Storage.data.settings.screenShake;
    Storage.save();
    this.syncUI();
  },
  syncUI() {
    const s = Storage.data.settings;
    document.getElementById('musicVolSlider').value = Math.round(s.musicVol * 100);
    document.getElementById('musicVolLabel').textContent = `${Math.round(s.musicVol * 100)}%`;
    document.getElementById('sfxVolSlider').value = Math.round(s.sfxVol * 100);
    document.getElementById('sfxVolLabel').textContent = `${Math.round(s.sfxVol * 100)}%`;
    document.getElementById('btnToggleShake').textContent = s.screenShake ? 'ENABLED' : 'DISABLED';
  }
};

const UI = {
  selectedWorldTab: 1,

  init() {
    Settings.syncUI();
    this.refreshAll();
    this.checkDailyBadge();
  },

  refreshAll() {
    this.updateCurrencyDisplays();
    this.renderCharacterGrid();
    this.renderWeaponGrid();
    this.renderUpgradeGrid();
    this.renderWorldMap();
    this.renderAchievements();
    this.renderDailyModal();
  },

  toast(msg, type = 'info') {
    const container = document.getElementById('toastContainer');
    const el = document.createElement('div');
    const borderCol = type === 'error' ? 'border-rose-400 text-rose-100' : 'border-cyan-300 text-cyan-50';
    el.className = `glass-panel px-4 py-2 text-xs sm:text-sm font-bold shadow-lg ${borderCol} transition-all duration-300`;
    el.textContent = msg;
    container.appendChild(el);
    setTimeout(() => {
      el.style.opacity = '0';
      el.style.transform = 'translateY(-10px)';
      setTimeout(() => el.remove(), 300);
    }, 2400);
  },

  toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen().catch(() => {});
    } else {
      document.exitFullscreen().catch(() => {});
    }
  },

  openModal(modalId) {
    AudioEngine.init();
    AudioEngine.sfxClick();
    this.refreshAll();
    document.getElementById('modalBackdrop').classList.remove('hidden');
    document.querySelectorAll('.modal-box').forEach(m => m.classList.add('hidden'));
    const target = document.getElementById(modalId);
    if (target) target.classList.remove('hidden');
  },

  closeModals() {
    document.getElementById('modalBackdrop').classList.add('hidden');
    document.querySelectorAll('.modal-box').forEach(m => m.classList.add('hidden'));
  },

  updateCurrencyDisplays() {
    const d = Storage.data;
    document.getElementById('currencyPearls').textContent = d.pearls.toLocaleString();
    document.getElementById('currencyGolden').textContent = d.golden.toLocaleString();
    document.getElementById('currencyCrystals').textContent = d.crystals.toLocaleString();

    const hero = HEROES.find(h => h.id === d.selectedHero) || HEROES[0];
    document.getElementById('topHeroName').textContent = hero.name;
    const wNum = Math.ceil(d.highestUnlockedLevel / 5);
    document.getElementById('topProgressText').textContent = `World ${wNum} • Level ${d.highestUnlockedLevel}`;
    document.getElementById('btnPlayContinue').textContent = d.highestUnlockedLevel > 1 ? `▶ CONTINUE (LV ${d.highestUnlockedLevel})` : '▶ PLAY GAME';
  },

  updateHUD() {
    const p = Game.player;
    if (!p) return;

    document.getElementById('hudHeroName').textContent = p.hero.name;
    document.getElementById('hudLevelBadge').textContent = `LV ${Game.currentLevelId}`;
    document.getElementById('hudHpText').textContent = `${Math.ceil(p.hp)} / ${p.maxHp}`;
    document.getElementById('hudHpBar').style.width = `${Math.max(0, (p.hp / p.maxHp) * 100)}%`;

    document.getElementById('hudEnergyText').textContent = `${Math.floor(p.energy)} / ${p.maxEnergy}`;
    document.getElementById('hudEnergyBar').style.width = `${Math.max(0, (p.energy / p.maxEnergy) * 100)}%`;

    const wObj = WEAPONS.find(w => w.id === Storage.data.selectedWeapon) || WEAPONS[0];
    const wLv = Storage.data.weaponLevels[wObj.id] || 1;
    document.getElementById('hudWeaponIcon').textContent = wObj.icon;
    document.getElementById('hudWeaponName').textContent = `${wObj.name} Lv.${wLv}`;

    // Active Buff Icons
    const buffs = [];
    if (p.shieldTimer > 0) buffs.push('🛡️');
    if (p.doubleDamageTimer > 0) buffs.push('🔥');
    if (p.rapidFireTimer > 0) buffs.push('⚡');
    if (p.speedBoostTimer > 0) buffs.push('💨');
    document.getElementById('hudBuffs').textContent = buffs.join('');

    // Mission Objective
    const world = WORLDS[(Game.levelData.world || 1) - 1];
    document.getElementById('hudWorldTitle').textContent = `World ${world.id}: ${world.name}`;
    document.getElementById('hudMissionText').textContent = `${Game.levelData.title} (${Game.missionProgress}/${Game.missionTarget})`;
    document.getElementById('hudMissionBar').style.width = `${Math.min(100, (Game.missionProgress / Game.missionTarget) * 100)}%`;

    // Boss Bar
    const bossBox = document.getElementById('hudBossContainer');
    if (Game.boss) {
      bossBox.classList.remove('hidden');
      document.getElementById('hudBossName').textContent = `👑 ${Game.boss.name}`;
      const pct = Math.max(0, Math.ceil((Game.boss.hp / Game.boss.maxHp) * 100));
      document.getElementById('hudBossHpText').textContent = `${pct}%`;
      document.getElementById('hudBossBar').style.width = `${pct}%`;
    } else {
      bossBox.classList.add('hidden');
    }

    document.getElementById('hudScore').textContent = Game.levelScore.toLocaleString();
    document.getElementById('hudPearls').textContent = Storage.data.pearls;
    document.getElementById('hudCrystals').textContent = Storage.data.crystals;
  },

  /* ---------- Render Character Selection (10 Heroes) ---------- */
  renderCharacterGrid() {
    const grid = document.getElementById('characterGrid');
    grid.innerHTML = '';

    HEROES.forEach(h => {
      const unlocked = Storage.data.unlockedHeroes.includes(h.id);
      const selected = Storage.data.selectedHero === h.id;

      const card = document.createElement('div');
      card.className = `glass-card p-3.5 flex flex-col justify-between gap-2 ${selected ? 'border-pink-400 ring-2 ring-pink-400/50' : ''}`;

      const costIcon = h.unlockCost.type === 'pearls' ? '⚪' : h.unlockCost.type === 'golden' ? '🟡' : '💎';
      const actionBtn = selected
        ? `<button disabled class="w-full py-2 rounded-xl bg-emerald-600/80 text-white text-xs font-bold">✓ SELECTED HERO</button>`
        : unlocked
          ? `<button onclick="UI.selectHero('${h.id}')" class="btn-ocean w-full py-2 text-xs">SELECT HERO</button>`
          : `<button onclick="UI.unlockHero('${h.id}')" class="btn-magic w-full py-2 text-xs">🔓 UNLOCK (${h.unlockCost.amount} ${costIcon})</button>`;

      card.innerHTML = `
        <div class="flex items-center gap-3">
          <canvas id="preview_${h.id}" width="64" height="68" class="rounded-xl bg-slate-950/60 border border-white/10 shrink-0"></canvas>
          <div class="min-w-0">
            <div class="title-font text-sm sm:text-base font-bold text-pink-200 truncate">${h.name}</div>
            <div class="text-[11px] text-cyan-300 truncate">${h.title}</div>
            <div class="text-[10px] text-amber-200 mt-0.5">✨ ${h.specialName}</div>
          </div>
        </div>
        <p class="text-[11px] text-cyan-100/85 leading-snug">${h.desc}</p>
        <div class="grid grid-cols-3 gap-1 text-center bg-slate-950/45 rounded-lg p-1.5 text-[10px] font-bold">
          <div>❤️ ${h.hp}</div>
          <div>💨 ${h.speed}</div>
          <div>⚔️ ${Math.round(h.damage * 100)}%</div>
        </div>
        ${actionBtn}
      `;
      grid.appendChild(card);

      // Draw Live Procedural Jellyfish Preview
      const c = document.getElementById(`preview_${h.id}`);
      if (c) {
        const pctx = c.getContext('2d');
        Renderer.drawJellyfishHero(pctx, 32, 30, 17, h, 1.2);
      }
    });
  },

  selectHero(id) {
    Storage.data.selectedHero = id;
    Storage.save();
    AudioEngine.sfxClick();
    this.renderCharacterGrid();
  },

  unlockHero(id) {
    const h = HEROES.find(x => x.id === id);
    if (!h) return;
    const { type, amount } = h.unlockCost;
    if (Storage.data[type] < amount) {
      this.toast(`Not enough ${type} to unlock ${h.name}!`, 'error');
      return;
    }
    Storage.data[type] -= amount;
    Storage.data.unlockedHeroes.push(id);
    Storage.data.selectedHero = id;
    Storage.save();
    this.checkAchievements();
    AudioEngine.sfxVictory();
    this.toast(`Unlocked ${h.name}!`, 'info');
    this.renderCharacterGrid();
  },

  /* ---------- Render Weapon Inventory & Upgrades (8 Weapons) ---------- */
  renderWeaponGrid() {
    const grid = document.getElementById('weaponGrid');
    grid.innerHTML = '';

    WEAPONS.forEach(w => {
      const unlocked = Storage.data.unlockedWeapons.includes(w.id);
      const equipped = Storage.data.selectedWeapon === w.id;
      const lv = Storage.data.weaponLevels[w.id] || 1;
      const upgCost = lv * 90;

      const costIcon = w.unlockCost.type === 'pearls' ? '⚪' : w.unlockCost.type === 'golden' ? '🟡' : '💎';

      let buttonsHtml = '';
      if (!unlocked) {
        buttonsHtml = `<button onclick="UI.unlockWeapon('${w.id}')" class="btn-magic w-full py-2 text-xs">🔓 UNLOCK (${w.unlockCost.amount} ${costIcon})</button>`;
      } else {
        const equipBtn = equipped
          ? `<button disabled class="flex-1 py-1.5 rounded-xl bg-emerald-600/80 text-white text-[11px] font-bold">✓ EQUIPPED</button>`
          : `<button onclick="UI.selectWeapon('${w.id}')" class="btn-ocean flex-1 py-1.5 text-[11px]">EQUIP</button>`;
        const upgBtn = lv >= 5
          ? `<button disabled class="flex-1 py-1.5 rounded-xl bg-amber-600/50 text-amber-200 text-[11px] font-bold">MAX LV 5</button>`
          : `<button onclick="UI.upgradeWeapon('${w.id}')" class="btn-magic flex-1 py-1.5 text-[11px]">⬆ UPGRADE (${upgCost} ⚪)</button>`;
        buttonsHtml = `<div class="flex gap-1.5">${equipBtn}${upgBtn}</div>`;
      }

      const card = document.createElement('div');
      card.className = `glass-card p-3.5 flex flex-col justify-between gap-2 ${equipped ? 'border-cyan-300 ring-2 ring-cyan-300/40' : ''}`;
      card.innerHTML = `
        <div>
          <div class="flex items-center justify-between">
            <span class="text-2xl">${w.icon}</span>
            <span class="text-[11px] px-2 py-0.5 rounded-full bg-white/10 font-bold text-amber-300">Level ${lv}/5</span>
          </div>
          <div class="title-font text-sm sm:text-base font-bold text-cyan-200 mt-1">${w.name}</div>
          <p class="text-[11px] text-cyan-100/80 mt-0.5 leading-snug">${w.desc}</p>
        </div>
        <div class="bg-slate-950/45 rounded-lg p-1.5 text-[10px] font-bold flex justify-between text-pink-200">
          <span>💥 DMG: ${Math.round(w.damage * (1 + (lv - 1) * 0.22))}</span>
          <span>⚡ SPD: ${Math.round((1 / w.fireRate) * 10) / 10}/s</span>
        </div>
        ${buttonsHtml}
      `;
      grid.appendChild(card);
    });
  },

  selectWeapon(id) {
    Storage.data.selectedWeapon = id;
    Storage.save();
    AudioEngine.sfxClick();
    this.renderWeaponGrid();
  },

  unlockWeapon(id) {
    const w = WEAPONS.find(x => x.id === id);
    if (!w) return;
    const { type, amount } = w.unlockCost;
    if (Storage.data[type] < amount) {
      this.toast(`Not enough ${type} to unlock ${w.name}!`, 'error');
      return;
    }
    Storage.data[type] -= amount;
    Storage.data.unlockedWeapons.push(id);
    Storage.data.selectedWeapon = id;
    Storage.save();
    this.checkAchievements();
    AudioEngine.sfxVictory();
    this.toast(`Unlocked ${w.name}!`, 'info');
    this.renderWeaponGrid();
  },

  upgradeWeapon(id) {
    const lv = Storage.data.weaponLevels[id] || 1;
    if (lv >= 5) return;
    const cost = lv * 90;
    if (Storage.data.pearls < cost) {
      this.toast(`Need ${cost} Pearls to upgrade!`, 'error');
      return;
    }
    Storage.data.pearls -= cost;
    Storage.data.weaponLevels[id] = lv + 1;
    Storage.save();
    AudioEngine.sfxPickup(true);
    this.toast(`Upgraded to Level ${lv + 1}!`, 'info');
    this.renderWeaponGrid();
  },

  /* ---------- Render Character Attribute Upgrades ---------- */
  renderUpgradeGrid() {
    const grid = document.getElementById('upgradeGrid');
    grid.innerHTML = '';

    UPGRADE_CATEGORIES.forEach(cat => {
      const lv = Storage.data.upgrades[cat.id] || 0;
      const cost = cat.baseCost * (lv + 1);
      const pips = Array.from({ length: 5 }, (_, i) =>
        `<span class="h-2 flex-1 rounded-full ${i < lv ? 'bg-gradient-to-r from-pink-400 to-cyan-400' : 'bg-slate-800'}"></span>`
      ).join('');

      const card = document.createElement('div');
      card.className = 'glass-card p-4 flex flex-col justify-between gap-2.5';
      card.innerHTML = `
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2.5">
            <span class="text-2xl">${cat.icon}</span>
            <div>
              <div class="title-font text-base font-bold text-amber-200">${cat.name}</div>
              <div class="text-xs text-cyan-200">${cat.desc}</div>
            </div>
          </div>
          <span class="text-xs font-bold text-pink-300">Lv ${lv}/5</span>
        </div>
        <div class="flex gap-1.5">${pips}</div>
        ${lv >= 5
          ? `<button disabled class="w-full py-2 rounded-xl bg-emerald-600/60 text-white text-xs font-bold">✓ MAXED OUT</button>`
          : `<button onclick="UI.buyAttributeUpgrade('${cat.id}')" class="btn-magic w-full py-2 text-xs">⬆ UPGRADE (${cost} ⚪)</button>`}
      `;
      grid.appendChild(card);
    });
  },

  buyAttributeUpgrade(catId) {
    const cat = UPGRADE_CATEGORIES.find(c => c.id === catId);
    const lv = Storage.data.upgrades[catId] || 0;
    if (!cat || lv >= 5) return;
    const cost = cat.baseCost * (lv + 1);
    if (Storage.data.pearls < cost) {
      this.toast(`Need ${cost} Pearls for ${cat.name}!`, 'error');
      return;
    }
    Storage.data.pearls -= cost;
    Storage.data.upgrades[catId] = lv + 1;
    Storage.save();
    AudioEngine.sfxPickup(true);
    this.toast(`${cat.name} upgraded to Level ${lv + 1}!`, 'info');
    this.renderUpgradeGrid();
  },

  /* ---------- Render World Map & 25 Levels ---------- */
  renderWorldMap() {
    const tabs = document.getElementById('worldTabs');
    tabs.innerHTML = '';

    WORLDS.forEach(w => {
      const active = this.selectedWorldTab === w.id;
      const btn = document.createElement('button');
      btn.className = `px-3.5 py-1.5 rounded-full text-xs sm:text-sm font-bold whitespace-nowrap cursor-pointer transition ${
        active ? 'btn-magic' : 'btn-ocean opacity-80'
      }`;
      btn.textContent = `World ${w.id}: ${w.name}`;
      btn.onclick = () => {
        this.selectedWorldTab = w.id;
        this.renderWorldMap();
      };
      tabs.appendChild(btn);
    });

    const grid = document.getElementById('levelsGrid');
    grid.innerHTML = '';
    const worldLevels = LEVELS.filter(l => l.world === this.selectedWorldTab);

    worldLevels.forEach(lvl => {
      const unlocked = lvl.id <= Storage.data.highestUnlockedLevel;
      const completed = Storage.data.completedLevels.includes(lvl.id);
      const isBoss = lvl.type === 'boss';

      const card = document.createElement('div');
      card.className = `glass-card p-4 flex flex-col justify-between gap-2.5 ${isBoss ? 'border-rose-400/70' : ''} ${!unlocked ? 'opacity-60' : ''}`;
      card.innerHTML = `
        <div>
          <div class="flex justify-between items-center">
            <span class="text-xs font-bold px-2.5 py-0.5 rounded-full ${isBoss ? 'bg-rose-500/40 text-rose-200' : 'bg-cyan-500/30 text-cyan-200'}">
              ${isBoss ? '👑 BOSS STAGE' : `LEVEL ${lvl.id}`}
            </span>
            <span class="text-xs font-bold ${completed ? 'text-emerald-300' : 'text-slate-400'}">
              ${completed ? '★ CLEARED' : unlocked ? 'READY' : '🔒 LOCKED'}
            </span>
          </div>
          <div class="title-font text-base font-bold text-white mt-1.5">${lvl.title}</div>
          <div class="text-xs text-cyan-200 mt-0.5">
            Reward: ${lvl.reward.pearls} ⚪ • ${lvl.reward.golden} 🟡 • ${lvl.reward.crystals} 💎
          </div>
        </div>
        ${unlocked
          ? `<button onclick="Game.startLevel(${lvl.id})" class="btn-magic w-full py-2 text-xs">▶ PLAY LEVEL ${lvl.id}</button>`
          : `<button disabled class="w-full py-2 rounded-xl bg-slate-800 text-slate-400 text-xs font-bold">🔒 COMPLETE LEVEL ${lvl.id - 1}</button>`}
      `;
      grid.appendChild(card);
    });
  },

  /* ---------- Render Achievements ---------- */
  checkAchievements() {
    let newlyClaimed = 0;
    ACHIEVEMENTS_LIST.forEach(ach => {
      if (!Storage.data.claimedAchievements.includes(ach.id) && ach.check(Storage.data)) {
        Storage.data.claimedAchievements.push(ach.id);
        Storage.data.pearls += ach.reward.pearls;
        Storage.data.golden += ach.reward.golden;
        Storage.data.crystals += ach.reward.crystals;
        newlyClaimed++;
        this.toast(`🏆 Achievement Unlocked: ${ach.name}!`, 'info');
      }
    });
    if (newlyClaimed > 0) Storage.save();
  },

  renderAchievements() {
    this.checkAchievements();
    const grid = document.getElementById('achievementsGrid');
    grid.innerHTML = '';

    ACHIEVEMENTS_LIST.forEach(ach => {
      const done = Storage.data.claimedAchievements.includes(ach.id);
      const card = document.createElement('div');
      card.className = `glass-card p-3.5 flex items-center justify-between gap-3 ${done ? 'border-amber-300/70' : 'opacity-75'}`;
      card.innerHTML = `
        <div class="flex items-center gap-3">
          <span class="text-3xl">${ach.icon}</span>
          <div>
            <div class="title-font text-sm sm:text-base font-bold ${done ? 'text-amber-200' : 'text-white'}">${ach.name}</div>
            <div class="text-xs text-cyan-200">${ach.desc}</div>
            <div class="text-[11px] text-pink-200 mt-0.5">Reward: +${ach.reward.pearls} ⚪ +${ach.reward.golden} 🟡 +${ach.reward.crystals} 💎</div>
          </div>
        </div>
        <span class="text-xs font-bold px-2.5 py-1 rounded-full ${done ? 'bg-emerald-500/30 text-emerald-200' : 'bg-slate-800 text-slate-400'}">
          ${done ? '✓ EARNED' : 'IN PROGRESS'}
        </span>
      `;
      grid.appendChild(card);
    });
  },

  /* ---------- Daily Rewards & Challenges ---------- */
  checkDailyBadge() {
    const today = new Date().toDateString();
    const canClaim = Storage.data.lastDailyDate !== today;
    document.getElementById('dailyBadge').classList.toggle('hidden', !canClaim);
  },

  renderDailyModal() {
    const today = new Date().toDateString();
    const canClaim = Storage.data.lastDailyDate !== today;
    const prog = Math.min(25, Storage.data.dailyChallengeProgress);
    const challengeReady = prog >= 25 && !Storage.data.dailyChallengeClaimed;

    const container = document.getElementById('dailyContent');
    container.innerHTML = `
      <div class="glass-card p-4 flex flex-col sm:flex-row items-center justify-between gap-3">
        <div>
          <div class="title-font text-lg font-bold text-amber-300">🎁 Daily Royal Ocean Gift</div>
          <div class="text-xs text-cyan-100">Claim +120 Pearls ⚪, +2 Golden Pearls 🟡 & +5 Crystals 💎 every day!</div>
        </div>
        ${canClaim
          ? `<button onclick="UI.claimDailyGift()" class="btn-magic px-6 py-2.5 text-sm shrink-0">CLAIM GIFT</button>`
          : `<button disabled class="px-5 py-2.5 rounded-full bg-emerald-700/60 text-emerald-200 text-xs font-bold shrink-0">✓ CLAIMED TODAY</button>`}
      </div>

      <div class="glass-card p-4 flex flex-col sm:flex-row items-center justify-between gap-3">
        <div class="w-full">
          <div class="title-font text-lg font-bold text-pink-300">⚔️ Daily Bounty: Defeat 25 Ocean Invaders</div>
          <div class="text-xs text-cyan-100 mb-2">Progress: ${prog} / 25 Enemies • Reward: +200 ⚪ & +3 🟡</div>
          <div class="w-full h-2.5 bg-slate-900 rounded-full overflow-hidden">
            <div class="h-full bg-gradient-to-r from-pink-500 to-cyan-400" style="width: ${(prog / 25) * 100}%"></div>
          </div>
        </div>
        ${Storage.data.dailyChallengeClaimed
          ? `<button disabled class="px-5 py-2.5 rounded-full bg-emerald-700/60 text-emerald-200 text-xs font-bold shrink-0">✓ COMPLETED</button>`
          : challengeReady
            ? `<button onclick="UI.claimDailyChallenge()" class="btn-magic px-6 py-2.5 text-sm shrink-0">CLAIM BOUNTY</button>`
            : `<button disabled class="px-5 py-2.5 rounded-full bg-slate-800 text-slate-400 text-xs font-bold shrink-0">IN PROGRESS</button>`}
      </div>
    `;
  },

  claimDailyGift() {
    const today = new Date().toDateString();
    if (Storage.data.lastDailyDate === today) return;
    Storage.data.lastDailyDate = today;
    Storage.data.pearls += 120;
    Storage.data.golden += 2;
    Storage.data.crystals += 5;
    Storage.save();
    AudioEngine.sfxVictory();
    this.toast('🎁 Claimed Daily Royal Treasure!', 'info');
    this.checkDailyBadge();
    this.renderDailyModal();
  },

  claimDailyChallenge() {
    if (Storage.data.dailyChallengeProgress < 25 || Storage.data.dailyChallengeClaimed) return;
    Storage.data.dailyChallengeClaimed = true;
    Storage.data.pearls += 200;
    Storage.data.golden += 3;
    Storage.save();
    AudioEngine.sfxVictory();
    this.toast('⚔️ Daily Bounty Reward Claimed!', 'info');
    this.renderDailyModal();
  }
};

/* -------------------- BOOTSTRAP GAME ON WINDOW LOAD -------------------- */
window.onload = function () {
  Game.init();
};
</script>
</body>
</html>
