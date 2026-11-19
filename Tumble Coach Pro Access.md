<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Tumble Coach Pro - Sovereign Strength Master Suite</title>
<style>
  :root {
    --deep-slate: #0B0F19;
    --electric-teal: #00E5FF;
    --card-bg: #1A2235;
    --card-border: #2A364F;
    --text-main: #E0E6ED;
    --text-muted: #8C9BAB;
    --danger-red: #FF3366;
    --gold: #FFD700;
    --green: #00FF66;
    --yellow: #FFEE00;
    --blue: #0055FF;
  }
  body, html {
    margin: 0; padding: 0; width: 100%; height: 100%;
    background-color: var(--deep-slate);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    overflow: hidden; color: var(--text-main);
  }

  /* Portal Mode Switcher Banner */
  #switcher-bar {
    position: fixed; top: 0; left: 0; width: 100%; height: 32px; background: #151D30; border-bottom: 1px solid var(--card-border);
    display: flex; justify-content: center; align-items: center; gap: 20px; z-index: 1000; font-size: 0.75rem; font-weight: bold;
  }
  .mode-btn { background: none; border: none; color: var(--text-muted); cursor: pointer; padding: 2px 8px; border-radius: 4px; }
  .mode-btn.active { background: var(--electric-teal); color: var(--deep-slate); }

  /* Login / Intro Screens */
  .auth-screen {
    display: flex; flex-direction: column; justify-content: center; align-items: center;
    height: calc(100vh - 32px); margin-top: 32px; padding: 30px; box-sizing: border-box; text-align: center;
    position: absolute; top: 0; left: 0; width: 100%; z-index: 100;
    background: radial-gradient(circle at center, #151D30 0%, var(--deep-slate) 70%);
  }

  .login-logo-container { margin-bottom: 25px; }
  .login-logo { 
    font-size: 4.4rem; 
    font-weight: 900; 
    letter-spacing: 3px; 
    color: white; 
    margin: 0; 
    font-family: 'Arial Black', Gadget, sans-serif;
    font-style: italic;
    text-transform: uppercase;
    background: linear-gradient(135deg, #0055FF 20%, #00E5FF 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 20px rgba(0, 85, 255, 0.7));
  }
  .login-logo span { 
    color: var(--electric-teal); 
    text-shadow: 0 0 35px rgba(0, 229, 255, 0.9);
    -webkit-text-fill-color: var(--electric-teal);
  }
  .login-subtitle { color: var(--text-muted); font-size: 0.9rem; margin-top: 4px; letter-spacing: 0.5px; }

  .login-card {
    background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 16px;
    padding: 25px; width: 100%; max-width: 380px; box-sizing: border-box;
    box-shadow: 0 8px 30px rgba(0,0,0,0.5);
  }

  .input-group { margin-bottom: 16px; text-align: left; }
  .input-label { display: block; font-size: 0.75rem; color: var(--text-muted); margin-bottom: 6px; font-weight: 500; }
  .input-field {
    width: 100%; background: var(--deep-slate); border: 1px solid var(--card-border); color: white;
    padding: 12px 14px; border-radius: 8px; font-size: 0.95rem; box-sizing: border-box; outline: none; transition: border-color 0.2s;
  }
  .input-field:focus { border-color: var(--electric-teal); }

  .btn-login {
    width: 100%; background: var(--electric-teal); color: var(--deep-slate); border: none;
    padding: 18px; border-radius: 10px; font-size: 1.15rem; font-weight: bold; cursor: pointer; margin-top: 12px;
    box-shadow: 0 0 20px rgba(0, 229, 255, 0.4); transition: background 0.2s, transform 0.2s;
  }
  .btn-login:hover { background: #33ecff; transform: translateY(-2px); }

  /* App Containers */
  .app-container { display: none; width: 100%; height: calc(100vh - 32px); margin-top: 32px; }

  /* Dashboards & Sections with Extended Scroll Padding */
  .dashboard-screen { display: flex; flex-direction: column; height: 100%; padding: 20px 20px 100px 20px; box-sizing: border-box; overflow-y: auto; scroll-behavior: smooth; }
  .sub-section { display: none; flex-direction: column; height: 100%; padding: 20px 20px 100px 20px; box-sizing: border-box; overflow-y: auto; scroll-behavior: smooth; }

  .profile-header {
    background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 14px; padding: 20px;
    margin-bottom: 15px; display: flex; align-items: center; gap: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  .profile-avatar {
    width: 60px; height: 60px; border-radius: 50%; background: var(--deep-slate); border: 2px solid var(--electric-teal);
    display: flex; align-items: center; justify-content: center; font-size: 1.4rem; font-weight: bold; color: var(--electric-teal);
  }
  .profile-info h2 { margin: 0 0 5px 0; color: white; font-size: 1.2rem; }
  .profile-info p { margin: 0; color: var(--text-muted); font-size: 0.85rem; }

  .nav-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 20px; }
  .nav-card {
    background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 12px; padding: 15px;
    cursor: pointer; transition: transform 0.2s, border-color 0.2s; text-align: center; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 85px;
  }
  .nav-card:hover { border-color: var(--electric-teal); transform: translateY(-2px); }
  .nav-card-icon { font-size: 1.5rem; margin-bottom: 6px; color: var(--electric-teal); }
  .nav-card-title { font-weight: bold; font-size: 0.9rem; color: white; }
  .nav-card-sub { font-size: 0.7rem; color: var(--text-muted); margin-top: 3px; }

  /* Calendar / Schedule Widget */
  .schedule-container { background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 14px; padding: 16px; margin-bottom: 20px; }
  .schedule-header-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; border-bottom: 1px solid var(--card-border); padding-bottom: 10px; }
  .calendar-toggles { display: flex; background: var(--deep-slate); border: 1px solid var(--card-border); border-radius: 6px; padding: 2px; }
  .cal-toggle-btn { background: transparent; border: none; color: var(--text-muted); padding: 5px 10px; font-size: 0.75rem; font-weight: 600; cursor: pointer; border-radius: 4px; }
  .cal-toggle-btn.active { background: var(--electric-teal); color: var(--deep-slate); }
  
  .schedule-item-row {
    display: flex; justify-content: space-between; align-items: center; background: var(--deep-slate);
    border: 1px solid var(--card-border); border-radius: 8px; padding: 10px 12px; margin-bottom: 8px; font-size: 0.85rem;
  }
  .schedule-info { display: flex; flex-direction: column; gap: 2px; }
  .schedule-time { color: var(--electric-teal); font-weight: bold; font-size: 0.8rem; }
  .schedule-actions { display: flex; gap: 6px; }
  .action-btn { background: var(--card-border); border: none; color: white; padding: 5px 8px; border-radius: 4px; font-size: 0.75rem; cursor: pointer; }
  .action-btn.delete { color: var(--danger-red); }

  .add-lesson-form { display: flex; gap: 8px; margin-top: 12px; border-top: 1px solid var(--card-border); padding-top: 12px; }
  .add-input { flex: 1; background: var(--deep-slate); border: 1px solid var(--card-border); color: white; padding: 8px 10px; border-radius: 6px; font-size: 0.8rem; outline: none; }
  .add-btn { background: var(--electric-teal); color: var(--deep-slate); border: none; padding: 8px 12px; border-radius: 6px; font-weight: bold; font-size: 0.8rem; cursor: pointer; }

  @media screen and (orientation: landscape) {
    #landscape-scrubber { display: block !important; width: 100vw; height: calc(100vh - 32px); position: relative; }
    .dashboard-screen, .sub-section { display: none !important; }
  }
  @media screen and (orientation: portrait) {
    #landscape-scrubber { display: none !important; }
  }

  .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid var(--card-border); }
  .logo { font-weight: bold; font-size: 1.2rem; cursor: pointer; letter-spacing: 0.5px; }
  .logo span { color: var(--electric-teal); }
  
  .card { background: var(--card-bg); border: 1px solid var(--card-border); border-radius: 12px; padding: 20px; margin-bottom: 20px; box-shadow: 0 4px 20px rgba(0,0,0,0.4); cursor: pointer; transition: border-color 0.2s; }
  .card:hover { border-color: var(--electric-teal); }
  .card-title { font-size: 1.3rem; font-weight: bold; margin: 0 0 5px 0; color: white; }
  .card-sub { color: var(--electric-teal); font-size: 0.9rem; margin-bottom: 12px; }

  .tabs { display: flex; gap: 12px; margin-top: 15px; border-bottom: 1px solid var(--card-border); padding-bottom: 10px; flex-wrap: wrap; }
  .tab { color: var(--text-muted); font-size: 0.85rem; cursor: pointer; font-weight: 500; transition: color 0.2s; }
  .tab.active { color: var(--electric-teal); font-weight: bold; }
  
  .tab-content { padding-top: 15px; font-size: 0.9rem; line-height: 1.5; }
  .schedule-item { display: flex; justify-content: space-between; margin-bottom: 12px; padding-bottom: 12px; border-bottom: 1px solid var(--card-border); align-items: center; }
  
  .btn-group { display: flex; gap: 12px; margin-top: 20px; }
  .btn { flex: 1; background: var(--card-border); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; font-weight: 600; font-size: 0.9rem; transition: background 0.2s; }
  .btn:hover { background: #354563; }
  .btn.primary { background: var(--electric-teal); color: var(--deep-slate); }
  .btn.primary:hover { background: #33ecff; }

  /* Landscape Scrubber UI */
  .grid-bg { width: 100%; height: 100%; background-image: linear-gradient(rgba(255,255,255,0.04) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.04) 1px, transparent 1px); background-size: 40px 40px; position: absolute; top: 0; left: 0; z-index: 1; }
  .svg-layer { width: 100%; height: 100%; position: absolute; top: 0; left: 0; z-index: 2; }
  
  .vertical-slider { -webkit-appearance: none; width: 80vh; height: 6px; background: var(--card-border); position: absolute; right: -38vh; top: 50%; transform: translateY(-50%) rotate(90deg); z-index: 10; border-radius: 3px; outline: none; }
  .vertical-slider::-webkit-slider-thumb { -webkit-appearance: none; width: 28px; height: 28px; border-radius: 50%; background: var(--electric-teal); cursor: pointer; box-shadow: 0 0 15px rgba(0, 229, 255, 0.7); }

  .top-hud { position: absolute; top: 15px; left: 20px; z-index: 5; display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
  .hud-badge { background: rgba(26, 34, 53, 0.85); border: 1px solid var(--card-border); padding: 6px 12px; border-radius: 20px; font-size: 0.75rem; backdrop-filter: blur(5px); }
  
  .color-palette { display: flex; gap: 6px; align-items: center; background: rgba(26, 34, 53, 0.9); padding: 4px 8px; border-radius: 16px; border: 1px solid var(--card-border); }
  .color-swatch { width: 16px; height: 16px; border-radius: 50%; cursor: pointer; border: 2px solid transparent; }
  .color-swatch.active { border-color: white; transform: scale(1.2); }

  .split-screen { display: none; width: 100%; height: 100%; position: absolute; top: 0; left: 0; z-index: 3; pointer-events: none; }
  .split-line { position: absolute; left: 50%; top: 0; bottom: 0; width: 2px; background: var(--electric-teal); box-shadow: 0 0 10px var(--electric-teal); }
  .split-label { position: absolute; top: 20px; background: rgba(11, 15, 25, 0.8); padding: 4px 10px; border-radius: 4px; font-size: 0.75rem; border: 1px solid var(--card-border); }

  #video-modal, #camera-modal {
    display: none; position: fixed; top: 32px; left: 0; width: 100vw; height: calc(100vh - 32px);
    background: rgba(11, 15, 25, 0.95); z-index: 2000; flex-direction: column; padding: 20px; box-sizing: border-box; overflow-y: auto;
  }
  .video-container {
    width: 100%; max-width: 700px; height: 350px; background: #000; border: 1px solid var(--card-border); border-radius: 12px;
    margin: 0 auto 20px auto; display: flex; align-items: center; justify-content: center; position: relative; overflow: hidden;
  }
</style>
</head>
<body>

  <!-- Splash Intro Screen -->
  <div id="splash-screen" style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh; width: 100vw; position: absolute; top: 0; left: 0; background: radial-gradient(circle at center, #151D30 0%, var(--deep-slate) 80%); z-index: 3000; padding: 30px; box-sizing: border-box; text-align: center;">
    <div style="margin-bottom: 25px; animation: fadeIn 1s ease;">
      <svg width="380" height="75" viewBox="0 0 340 64" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6 6H48V18H30V56H18V18H6V6Z" fill="#00E5FF"/>
        <path d="M20 14H54V26H42V56H30V26H20V14Z" fill="#0B0F19" stroke="#00E5FF" stroke-width="3"/>
        <circle cx="36" cy="11" r="3" fill="#FFFFFF"/>
        <text x="68" y="30" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">TUMBLE</text>
        <text x="68" y="56" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#00E5FF" letter-spacing="1.5">COACH PRO</text>
      </svg>
      <div style="font-size: 1.4rem; font-weight: 700; color: white; margin-top: 6px; letter-spacing: 0.5px;">Tumble Coach Pro</div>
      <div style="color: var(--text-muted); font-size: 0.85rem; letter-spacing: 1px; text-transform: uppercase; margin-top: 2px;">Sovereign Strength Master Suite</div>
    </div>

    <div style="background: rgba(26, 34, 53, 0.85); border: 1px solid var(--card-border); border-left: 4px solid var(--electric-teal); border-radius: 12px; padding: 20px 24px; max-width: 440px; margin-bottom: 35px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); min-height: 120px; display: flex; flex-direction: column; justify-content: center;">
      <div style="color: var(--electric-teal); font-size: 0.75rem; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; transition: opacity 0.3s ease;" id="splash-quote-title">STRENGTH</div>
      <p style="color: var(--text-main); font-size: 0.92rem; line-height: 1.6; margin: 0; font-style: italic; transition: opacity 0.3s ease;" id="splash-quote-text">
        &ldquo;The quality or state of being physically and mentally strong; the capacity of an athlete to exert force and execute flawless power under pressure.&rdquo;
      </p>
    </div>

    <button onclick="dismissSplash()" style="background: var(--electric-teal); color: var(--deep-slate); border: none; padding: 18px 38px; border-radius: 10px; font-size: 1.15rem; font-weight: 800; cursor: pointer; box-shadow: 0 0 20px rgba(0, 229, 255, 0.4); transition: transform 0.2s, background 0.2s;">
      ENTER LOGIN PORTAL →
    </button>
  </div>

<!-- Mode Switcher Demo Bar -->
<div id="switcher-bar">
  <span style="color:var(--text-muted);">SUITE MODE:</span>
  <button class="mode-btn active" id="btn-mode-coach" onclick="switchSuiteMode('coach')">Coach Edition (Flip Smith)</button>
  <button class="mode-btn" id="btn-mode-athlete" onclick="switchSuiteMode('athlete')">TCP <span style="font-family: 'Brush Script MT', cursive; font-size: 0.85rem; color: var(--gold);">Athlete Edition</span> (Olivia)</button>
  <button class="mode-btn" id="btn-mode-parent" onclick="switchSuiteMode('parent')">Parent Portal (Sarah Hurley)</button>
</div>

<!-- COACH EDITION -->
<div id="coach-suite" class="app-container" style="display: block;">
  <div id="coach-login" class="auth-screen">
    <div class="login-logo-container">
      <svg width="280" height="55" viewBox="0 0 340 64" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6 6H48V18H30V56H18V18H6V6Z" fill="#00E5FF"/>
        <path d="M20 14H54V26H42V56H30V26H20V14Z" fill="#0B0F19" stroke="#00E5FF" stroke-width="3"/>
        <circle cx="36" cy="11" r="3" fill="#FFFFFF"/>
        <text x="68" y="30" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">TUMBLE</text>
        <text x="68" y="56" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#00E5FF" letter-spacing="1.5">COACH PRO</text>
      </svg>
      <div style="font-size: 1.5rem; font-weight: 700; color: white; margin-top: 4px;">Tumble Coach Pro</div>
      <div class="login-subtitle">Sovereign Strength Master Suite</div>
    </div>
    <div class="login-card">
      <div class="input-group">
        <label class="input-label">COACH EMAIL</label>
        <input type="email" class="input-field" value="coach@sovereignstrength.com">
      </div>
      <div class="input-group">
        <label class="input-label">PASSWORD</label>
        <input type="password" class="input-field" value="••••••••••••">
      </div>
      <button class="btn-login" onclick="loginCoach()">LAUNCH COACH PORTAL</button>
    </div>
  </div>

  <div id="coach-main" style="display: none; width: 100%; height: 100%;">
    <div id="c-screen-profile" class="dashboard-screen">
      <div class="header">
        <div class="logo" onclick="logoutCoach()">
          <svg width="150" height="28" viewBox="0 0 340 64" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M6 6H48V18H30V56H18V18H6V6Z" fill="#00E5FF"/>
            <path d="M20 14H54V26H42V56H30V26H20V14Z" fill="#0B0F19" stroke="#00E5FF" stroke-width="3"/>
            <circle cx="36" cy="11" r="3" fill="#FFFFFF"/>
            <text x="68" y="30" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">TUMBLE</text>
            <text x="68" y="56" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#00E5FF" letter-spacing="1.5">COACH PRO</text>
          </svg>
        </div>
        <div style="font-size: 0.85rem; color: var(--text-muted);">Tumble Coach Pro: <span style="font-family: 'Brush Script MT', cursive; font-size: 0.85rem; color: var(--green);">Coach's Edition</span></div>
      </div>

      <div class="profile-header">
        <div class="profile-avatar">CF</div>
        <div class="profile-info">
          <h2>Coach Flip Smith</h2>
          <p>Elite Gymnastics & Tumbling Coach</p>
          <p style="color: var(--electric-teal); font-size: 0.75rem; margin-top: 3px;">Sovereign Strength Studio</p>
        </div>
      </div>

      <div class="card" style="padding: 15px; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center; background: linear-gradient(135deg, #1A2235 0%, #151D30 100%); cursor:pointer;" onclick="openCameraRecorder()">
        <div>
          <div style="font-weight: bold; color: white; font-size: 0.95rem;">🔴 Live Screen / Camera Recorder</div>
          <div style="font-size: 0.75rem; color: var(--electric-teal); margin-top: 2px;">Link to phone camera for instant session recording</div>
        </div>
        <button class="btn primary" style="padding: 8px 14px; font-size: 0.8rem; flex:none;">Record Live</button>
      </div>

      <div class="card" style="padding: 15px; margin-bottom: 15px; background: linear-gradient(135deg, #1A2235 0%, #151D30 100%); border-left: 3px solid var(--electric-teal);">
        <div style="font-weight: bold; color: white; font-size: 0.95rem; margin-bottom: 4px;">🎬 Biomechanical Video Scrubber Suite</div>
        <div style="font-size: 0.8rem; color: var(--text-muted); line-height: 1.4;">
          Please <strong>rotate your phone to horizontal (landscape view)</strong> to launch the 3-phase trajectory, split-screen comparison, and color-palette drawing analysis suite!
        </div>
      </div>

      <div class="card" style="padding: 15px; margin-bottom: 15px; background: var(--card-bg);">
        <div style="font-weight: bold; color: white; font-size: 0.9rem; margin-bottom: 8px;">💬 Coach Broadcast Message Box</div>
        <textarea id="coach-broadcast-msg" style="width:100%; height:60px; background:var(--deep-slate); border:1px solid var(--card-border); color:white; padding:8px; border-radius:6px; resize:none;" placeholder="Type broadcast note to all athletes and parents..."></textarea>
        <button class="btn primary" style="margin-top: 8px; padding: 6px 12px; font-size: 0.8rem;" onclick="alert('Broadcast message successfully sent to all active students and parent portals!')">Send Broadcast</button>
      </div>

      <div style="font-size: 0.8rem; color: var(--text-muted); margin-bottom: 8px; font-weight: 600; text-transform: uppercase;">Navigation Menu</div>
      <div class="nav-grid">
        <div class="nav-card" onclick="openCoachSection('c-sec-directory')">
          <div class="nav-card-icon">👥</div>
          <div class="nav-card-title">Athlete Directory</div>
          <div class="nav-card-sub">Active Rosters (5)</div>
        </div>
        <div class="nav-card" onclick="openCoachSection('c-sec-videos')">
          <div class="nav-card-icon">🎬</div>
          <div class="nav-card-title">Videos & Scrubber</div>
          <div class="nav-card-sub">Biomechanical Suite</div>
        </div>
        <div class="nav-card" onclick="openCoachSection('c-sec-drills')">
          <div class="nav-card-icon">⚡</div>
          <div class="nav-card-title">Drills & Conditioning</div>
          <div class="nav-card-sub">Training Library</div>
        </div>
        <div class="nav-card" onclick="openCoachSection('c-sec-levels')">
          <div class="nav-card-icon">📈</div>
          <div class="nav-card-title">Skill Levels</div>
          <div class="nav-card-sub">USAG / Level Breakdown</div>
        </div>
      </div>

      <div class="schedule-container">
        <div class="schedule-header-row">
          <span style="font-weight: bold; font-size: 0.9rem; color: white;">Private Lesson Schedule</span>
          <div class="calendar-toggles">
            <button class="cal-toggle-btn active" id="c-btn-day" onclick="setCalView('day')">Day</button>
            <button class="cal-toggle-btn" id="c-btn-week" onclick="setCalView('week')">Week</button>
            <button class="cal-toggle-btn" id="c-btn-month" onclick="setCalView('month')">Month</button>
          </div>
        </div>
        <div id="c-schedule-list"></div>
        <div class="add-lesson-form">
          <input type="text" class="add-input" id="c-new-name" placeholder="Athlete & Focus...">
          <input type="text" class="add-input" id="c-new-time" placeholder="Time (e.g. 4:00 PM)" style="max-width: 120px;">
          <button class="add-btn" onclick="addLesson()">Add</button>
        </div>
      </div>
    </div>

    <!-- Athlete Directory Section -->
    <div id="c-sec-directory" class="sub-section">
      <div class="header">
        <div class="logo" onclick="openCoachSection('c-screen-profile')">← Back to Profile</div>
        <div style="font-size: 0.85rem; color: var(--electric-teal);">Athlete Directory</div>
      </div>
      <div id="c-athlete-list"></div>
    </div>

    <!-- Athlete Detail View -->
    <div id="c-sec-athlete-detail" class="sub-section">
      <div class="header">
        <div class="logo" onclick="openCoachSection('c-sec-directory')">← Back to Directory</div>
        <div style="font-size: 0.85rem; color: var(--electric-teal);" id="c-detail-title">Profile</div>
      </div>
      <div class="card" style="cursor:default;">
        <h2 style="margin:0 0 5px 0; color:white;" id="c-ath-name">Name</h2>
        <div style="color:var(--electric-teal); font-size:0.85rem;" id="c-ath-level">Level</div>
        <div class="tabs">
          <div class="tab active" onclick="switchAthTab('focus')" id="atab-focus">Focus / Notes</div>
          <div class="tab" onclick="switchAthTab('workout')" id="atab-workout">Home Workout</div>
          <div class="tab" onclick="switchAthTab('billing')" id="atab-billing">Billing & Packages</div>
          <div class="tab" onclick="switchAthTab('dm')" id="atab-dm">Direct Message</div>
        </div>
        <div id="ath-pane-focus" class="tab-content">
          <strong>Current Focus:</strong> <span id="c-ath-focus"></span><br><br>
          <label style="font-size: 0.75rem; color: var(--text-muted); display: block; margin-bottom: 4px;">COACH'S NOTES:</label>
          <textarea style="width:100%; height:75px; background:var(--deep-slate); border:1px solid var(--card-border); color:white; padding:8px; border-radius:6px; resize:none;">Emphasized core tension and hard-floor power transfer.</textarea>
        </div>
        <div id="ath-pane-workout" class="tab-content" style="display:none;">
          <div style="font-weight:bold; color:white; margin-bottom:8px;">3-Day Home Strength Conditioning:</div>
          <div id="c-ath-workout-list"></div>
          <button class="btn primary" style="margin-top:12px; padding:8px;" onclick="alert('Workout plan sent to athlete & parent!')">Send Plan</button>
        </div>
        <div id="ath-pane-billing" class="tab-content" style="display:none;">
          <div style="font-weight:bold; color:white; margin-bottom:8px;">Billing & Private Lesson Package:</div>
          <div class="schedule-item"><span><strong>Package Status:</strong> Active (4 Sessions Remaining)</span><span style="color:var(--green)">Paid in Full</span></div>
          <div class="schedule-item"><span><strong>Hourly Rate:</strong> $75.00 / Private Session</span><span>Auto-Pay: On</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span><strong>Last Invoice:</strong> Nov 01, 2026 ($300.00)</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="alert('Receipt emailed successfully!')">Resend Receipt</span></div>
          <button class="btn primary" style="margin-top:12px; padding:8px;" onclick="alert('Invoice for next 4-pack generated and sent!')">Charge / Renew Package</button>
        </div>
        <div id="ath-pane-dm" class="tab-content" style="display:none;">
          <div style="font-weight:bold; color:white; margin-bottom:6px;">Direct Message:</div>
          <textarea style="width:100%; height:75px; background:var(--deep-slate); border:1px solid var(--card-border); color:white; padding:8px; border-radius:6px; resize:none;" placeholder="Type message to athlete/parent..."></textarea>
          <div class="btn-group" style="margin-top:10px;">
            <button class="btn primary" onclick="alert('Message sent successfully!')">Send Message</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Videos & Scrubber Section -->
    <div id="c-sec-videos" class="sub-section">
      <div class="header">
        <div class="logo" onclick="openCoachSection('c-screen-profile')">← Back to Profile</div>
        <div style="font-size: 0.85rem; color: var(--electric-teal);">Video Library & Scrubber</div>
      </div>
      <div class="card" style="cursor:default;">
        <div style="font-weight:bold; color:white; margin-bottom:4px;" id="c-active-ath-video">Active Athlete: Olivia Hurley</div>
        <div style="color:var(--electric-teal); font-size:0.85rem; margin-bottom:12px;">Level 4 Tumbling</div>

        <div style="margin-bottom: 15px; display: flex; gap: 10px;">
          <input type="file" id="coach-video-upload-input" accept="video/*" style="display:none;" onchange="handleVideoUpload(this)">
          <button class="btn primary" onclick="document.getElementById('coach-video-upload-input').click()">📁 Upload Video from Phone</button>
        </div>

        <div class="schedule-item"><span>Oct 30 - Running Layout Form</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="openVideoPlayer('Oct 30 - Running Layout Form')">▶ Playback</span></div>
        <div class="schedule-item" style="border-bottom:none;"><span>Oct 23 - BHS Block Power</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="openVideoPlayer('Oct 23 - BHS Block Power')">▶ Playback</span></div>
      </div>

      <div class="card" style="cursor:default; margin-top: auto; max-height: 250px; overflow-y: auto;">
        <div style="font-weight:bold; color:var(--electric-teal); margin-bottom:8px; font-size:0.9rem;">Recently Uploaded Videos (Scrollable):</div>
        <div class="uploaded-videos-container">
          <div style="font-size:0.8rem; color:var(--text-muted); font-style:italic;">No uploaded videos yet. Use upload above to add clips.</div>
        </div>
      </div>
    </div>

    <!-- Drills & Conditioning -->
    <div id="c-sec-drills" class="sub-section">
      <div class="header">
        <div class="logo" onclick="openCoachSection('c-screen-profile')">← Back to Profile</div>
        <div style="font-size: 0.85rem; color: var(--electric-teal);">Drills & Conditioning Library</div>
      </div>
      <div class="card" style="cursor:default;">
        <div class="tabs">
          <div class="tab active" onclick="switchDrillTab('drills')" id="dtab-drills">Tumbling Drills</div>
          <div class="tab" onclick="switchDrillTab('conditioning')" id="dtab-cond">Strength Conditioning</div>
        </div>
        <div id="drill-pane-drills" class="tab-content">
          <div style="font-weight:bold; color:white; margin-bottom:8px;">Essential Tumbling Progressions:</div>
          <div class="schedule-item"><span>Hollow Body Rockers</span><span style="color:var(--electric-teal)">Core & Shape</span></div>
          <div class="schedule-item"><span>Handstand Snap-Downs</span><span style="color:var(--electric-teal)">BHS Power Block</span></div>
          <div class="schedule-item"><span>Rebound Tucks on Trampoline</span><span style="color:var(--electric-teal)">Height & Set</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span>Standing Back Tuck Wall Drills</span><span style="color:var(--electric-teal)">Rotation</span></div>
        </div>
        <div id="drill-pane-cond" class="tab-content" style="display:none;">
          <div style="font-weight:bold; color:white; margin-bottom:8px;">Gymnastics Power & Body Strength:</div>
          <div class="schedule-item"><span>Pike Compression Holds</span><span style="color:var(--green)">3 sets x 30 sec</span></div>
          <div class="schedule-item"><span>V-Ups (Straddle & Pike)</span><span style="color:var(--green)">3 sets x 15 reps</span></div>
          <div class="schedule-item"><span>Weighted Dips & Overhead Press</span><span style="color:var(--green)">Hypertrophy Program</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span>Ab Roller Extensions</span><span style="color:var(--green)">3 sets x 12 reps</span></div>
        </div>
      </div>
    </div>

    <!-- Skill Levels -->
    <div id="c-sec-levels" class="sub-section">
      <div class="header">
        <div class="logo" onclick="openCoachSection('c-screen-profile')">← Back to Profile</div>
        <div style="font-size: 0.85rem; color: var(--electric-teal);">USASF All Star Cheer Tumbling & Skill Library</div>
      </div>
      <div class="card" style="cursor:default;">
        <div class="tabs">
          <div class="tab active" onclick="switchLevelTab('chart')" id="ltab-chart">USASF Levels 1-7</div>
          <div class="tab" onclick="switchLevelTab('definitions')" id="ltab-defs">Skill Definition Library</div>
        </div>
        
        <div id="lpane-chart" class="tab-content">
          <div style="font-weight:bold; color:white; margin-bottom:10px;">USASF All Star Cheer Tumbling Progression (Levels 1 - 7):</div>
          <div class="schedule-item"><span><strong>Level 1:</strong> Cartwheel, Forward/Backward Roll, Handstand Forward Roll, Roundoff</span><span style="color:var(--electric-teal)">Basics</span></div>
          <div class="schedule-item"><span><strong>Level 2:</strong> Standing Back Handspring (BHS), Roundoff BHS, Series BHS</span><span style="color:var(--electric-teal)">Connection</span></div>
          <div class="schedule-item"><span><strong>Level 3:</strong> Standing Back Tuck, Roundoff BHS Tuck, Jump to BHS Tuck</span><span style="color:var(--electric-teal)">Aerials & Tucks</span></div>
          <div class="schedule-item"><span><strong>Level 4:</strong> Standing BHS Tuck, Roundoff BHS Layout, Standing Layout</span><span style="color:var(--electric-teal)">Layouts</span></div>
          <div class="schedule-item"><span><strong>Level 5:</strong> Standing Full, Roundoff BHS Full, Standing Arabesque/BHS to Full</span><span style="color:var(--electric-teal)">Full Twisting</span></div>
          <div class="schedule-item"><span><strong>Level 6:</strong> Standing Double Full, Roundoff BHS Double Full, Specialty to Full</span><span style="color:var(--electric-teal)">Double Twisting</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span><strong>Level 7:</strong> Standing Triple Full, Specialized Elite Tumbling Passes</span><span style="color:var(--gold); font-weight:bold;">Elite World Class</span></div>
        </div>

        <div id="lpane-defs" class="tab-content" style="display:none;">
          <div style="font-weight:bold; color:white; margin-bottom:10px;">Biomechanical & Tumbling Skill Definitions:</div>
          <div class="schedule-item"><div><strong style="color:var(--electric-teal);">Block:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">The explosive upward push through the shoulders and arms off the floor or apparatus to generate height.</span></div></div>
          <div class="schedule-item"><div><strong style="color:var(--electric-teal);">Hurdle:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">A dynamic stepping motion transitioning horizontal running momentum into vertical rebound force.</span></div></div>
          <div class="schedule-item"><div><strong style="color:var(--electric-teal);">Set:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">The instant of maximum upward extension before initiating rotation or twisting.</span></div></div>
          <div class="schedule-item"><div><strong style="color:var(--electric-teal);">Snap-Down:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">The rapid whipping of legs downward from a handstand to create an instantaneous rebound block.</span></div></div>
          <div class="schedule-item"><div><strong style="color:var(--electric-teal);">Core Tension:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">Isomorphic engagement of abdominals and glutes to maintain a rigid hollow or arch body shape during flight.</span></div></div>
          <div class="schedule-item" style="border-bottom:none;"><div><strong style="color:var(--electric-teal);">Layout:</strong><br><span style="font-size:0.8rem; color:var(--text-muted);">A fully extended, straight-body flip executed without pike or tuck separation.</span></div></div>
        </div>
      </div>
    </div>

    <!-- Landscape Video Scrubber Suite -->
    <div id="landscape-scrubber" style="display:none; width:100vw; height:calc(100vh - 32px); position:relative;">
      <div class="grid-bg"></div>
      <div class="top-hud" style="justify-content: space-between; width: calc(100vw - 40px);">
        <div style="display: flex; gap: 8px; align-items: center; flex-wrap: wrap;">
          <div class="hud-badge" style="color: var(--electric-teal); font-weight: bold;" id="hud-ath-name">OLIVIA HURLEY</div>
          <div class="hud-badge" id="phase-indicator">Phase: Hurdle</div>
          <button class="hud-badge" onclick="toggleSplitScreen()" id="split-btn" style="cursor:pointer; background:var(--card-border);">Split: Off</button>
          <button class="hud-badge" onclick="document.getElementById('scrubber-video-upload').click()" style="cursor:pointer; background:var(--card-border);">📁 Upload</button>
          <button class="hud-badge" onclick="openCameraRecorder()" style="cursor:pointer; background:var(--card-border); color:var(--danger-red);">🔴 Record</button>
          <input type="file" id="scrubber-video-upload" accept="video/*" style="display:none;" onchange="handleVideoUpload(this)">
          
          <button class="hud-badge" onclick="toggleDrawingMode()" id="draw-btn" style="cursor:pointer; background:var(--card-border);">Draw: Off</button>
          <div class="color-palette" id="palette-bar" style="display:none;">
            <div class="color-swatch active" style="background:var(--danger-red);" onclick="setDrawingColor('#FF3366', this)"></div>
            <div class="color-swatch" style="background:var(--gold);" onclick="setDrawingColor('#FFD700', this)"></div>
            <div class="color-swatch" style="background:var(--green);" onclick="setDrawingColor('#00FF66', this)"></div>
            <div class="color-swatch" style="background:var(--yellow);" onclick="setDrawingColor('#FFEE00', this)"></div>
            <div class="color-swatch" style="background:var(--blue);" onclick="setDrawingColor('#0055FF', this)"></div>
          </div>
          <button class="hud-badge" onclick="clearDrawings()" style="cursor:pointer; background:var(--card-border); color:var(--danger-red);">Clear</button>
        </div>
        <div class="hud-badge" style="background: rgba(0, 229, 255, 0.15); border: 1px solid var(--electric-teal); color: var(--electric-teal); font-weight: bold; font-size: 0.85rem;" id="hud-top-right-name">
          ATHLETE: OLIVIA HURLEY
        </div>
      </div>

      <div class="split-screen" id="split-overlay">
        <div class="split-line"></div>
        <div class="split-label" style="left: 25%;">Reference Run (Oct 30)</div>
        <div class="split-label" style="right: 25%;">Live Analysis</div>
      </div>
      
      <canvas id="drawing-canvas" class="svg-layer" style="z-index: 4; pointer-events: none;"></canvas>
      
      <svg class="svg-layer" id="trajectory-svg" viewBox="0 0 1000 500" preserveAspectRatio="xMidYMid meet">
        <line x1="100" y1="400" x2="900" y2="400" stroke="#E0E6ED" stroke-width="3" />
        <g id="athlete-hinge">
          <circle cx="0" cy="-68" r="13" fill="#E0E6ED" />
          <path id="torso-path" d="M 0 -10 Q -10 -35 0 -55" stroke="#E0E6ED" stroke-width="10" stroke-linecap="round" fill="none" />
          <circle cx="0" cy="0" r="8" fill="#E0E6ED" />
          <circle cx="0" cy="-52" r="6" fill="#E0E6ED" />
          <g stroke="#E0E6ED" stroke-width="6" stroke-linecap="round" stroke-linejoin="round" fill="none">
            <path id="left-arm" d="M 0 -52 Q 20 -70 40 -95" />
            <path id="right-arm" d="M 0 -52 Q 10 -65 25 -85" />
          </g>
          <g stroke="#E0E6ED" stroke-width="7" stroke-linecap="round" stroke-linejoin="round" fill="none">
            <path id="legs-path" d="M 0 0 Q 25 35 50 70" />
            <path id="arms" d="M 0 0 Q 15 30 35 60" style="display:none;" />
          </g>
        </g>
      </svg>
      <input type="range" min="0" max="100" value="0" class="vertical-slider" id="scrubber" oninput="updateScrubber()">
    </div>

  </div>
</div>

<!-- ========================================== -->
<!-- ATHLETE EDITION: TCP ATHLETE               -->
<!-- ========================================== -->
<div id="athlete-suite" class="app-container" style="display: none;">
  <div id="athlete-login" class="auth-screen">
    <div class="login-logo-container">
      <h1 class="login-logo">TCP <span style="font-family: 'Brush Script MT', cursive; color: var(--gold);">Athlete Edition</span></h1>
      <div style="font-size: 1.4rem; font-weight: 700; color: white; margin-top: 4px;">Athlete Portal</div>
      <div class="login-subtitle">Sovereign Strength Training & Biomechanics</div>

      <div style="margin: 15px auto 0 auto; max-width: 380px; background: rgba(26, 34, 53, 0.7); border: 1px solid var(--card-border); border-left: 3px solid var(--electric-teal); padding: 10px 14px; border-radius: 8px; font-size: 0.8rem; font-style: italic; color: var(--text-main); text-align: left;">
        <span style="color: var(--electric-teal); font-style: normal; font-weight: bold; display: block; margin-bottom: 2px; font-size: 0.7rem; text-transform: uppercase;">Daily Motivation</span>
        "Push taller through your toes and trust your core tension on the set!"
      </div>
    </div>

    <div class="login-card">
      <div class="input-group">
        <label class="input-label">ATHLETE USERNAME</label>
        <input type="text" class="input-field" value="olivia.hurley">
      </div>
      <div class="input-group">
        <label class="input-label">PASSWORD</label>
        <input type="password" class="input-field" value="••••••••••••">
      </div>
      <button class="btn-login" onclick="loginAthlete()">ACCESS MY PORTAL</button>
    </div>
  </div>

  <div id="athlete-main" style="display: none; width: 100%; height: 100%;">
    <div class="dashboard-screen">
      <div class="header">
        <div class="logo" onclick="logoutAthlete()">TCP <span style="font-family: 'Brush Script MT', cursive; font-size: 0.85rem; color: var(--gold);">Athlete Edition</span></div>
        <div style="font-size: 0.85rem; color: var(--text-muted); cursor:pointer;" onclick="logoutAthlete()">Sign Out</div>
      </div>

      <div class="profile-header">
        <div class="profile-avatar">OH</div>
        <div class="profile-info">
          <h2>Olivia Hurley</h2>
          <p>Level 4 Tumbling &bull; Private Student</p>
          <p style="color: var(--electric-teal); font-size: 0.75rem; margin-top: 3px;">Coach: Coach Flip Smith (Sovereign Strength)</p>
        </div>
      </div>

      <div class="card" style="padding: 15px; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center; background: linear-gradient(135deg, #1A2235 0%, #151D30 100%); cursor:pointer;" onclick="openCameraRecorder()">
        <div>
          <div style="font-weight: bold; color: white; font-size: 0.95rem;">🔴 Record Live Practice Run</div>
          <div style="font-size: 0.75rem; color: var(--electric-teal); margin-top: 2px;">Record directly with your phone camera</div>
        </div>
        <button class="btn primary" style="padding: 8px 14px; font-size: 0.8rem; flex:none;">Open Camera</button>
      </div>

      <div class="card" style="cursor:default;">
        <div class="tabs">
          <div class="tab active" onclick="switchAthPortalTab('focus')" id="ath-tab-f-btn">Current Focus</div>
          <div class="tab" onclick="switchAthPortalTab('videos')" id="ath-tab-v-btn">Video Library</div>
          <div class="tab" onclick="switchAthPortalTab('workout')" id="ath-tab-w-btn">Home Workout</div>
          <div class="tab" onclick="switchAthPortalTab('library')" id="ath-tab-l-btn">Drills & USASF Chart</div>
          <div class="tab" onclick="switchAthPortalTab('billing')" id="ath-tab-b-btn">Billing & Account</div>
          <div class="tab" onclick="switchAthPortalTab('message')" id="ath-tab-m-btn">Message Coach</div>
        </div>

        <div id="ath-tab-focus" class="tab-content">
          <strong>Active Training Goal:</strong> Hard-floor back tuck power, running layout mastery.<br><br>
          <div style="background: var(--deep-slate); border: 1px solid var(--card-border); padding: 12px; border-radius: 8px;">
            <strong style="color: var(--electric-teal);">Coach Flip Smith's Latest Note:</strong><br>
            "Emphasized core tension through set phase. Excellent snap-down progression on hard floor session!"
          </div>
        </div>

        <div id="ath-tab-videos" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Progress Tracking & Video Library:</div>

          <div style="margin-bottom: 15px; display: flex; gap: 10px;">
            <input type="file" id="video-upload-input" accept="video/*" style="display:none;" onchange="handleVideoUpload(this)">
            <button class="btn primary" onclick="document.getElementById('video-upload-input').click()">📁 Upload Video from Phone</button>
          </div>
          <div class="uploaded-videos-container">
            <div style="font-size:0.8rem; color:var(--text-muted); font-style:italic; margin-bottom:10px;">No uploaded videos yet.</div>
          </div>

          <div class="schedule-item">
            <div><strong style="color:white;">Oct 30 - Running Layout Form</strong><br><span style="font-size:0.75rem; color:var(--electric-teal);">Hard Floor Session &bull; Form Verified</span></div>
            <button class="btn primary" style="padding: 6px 12px; font-size: 0.8rem; flex:none;" onclick="openVideoPlayer('Oct 30 - Running Layout Form')">▶ Playback</button>
          </div>
          <div class="schedule-item" style="border-bottom:none;">
            <div><strong style="color:white;">Oct 23 - BHS Block Power</strong><br><span style="font-size:0.75rem; color:var(--electric-teal);">Spring Floor &bull; Block Angle Mastered</span></div>
            <button class="btn primary" style="padding: 6px 12px; font-size: 0.8rem; flex:none;" onclick="openVideoPlayer('Oct 23 - BHS Block Power')">▶ Playback</button>
          </div>
        </div>

        <div id="ath-tab-workout" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Assigned 3-Day Home Strength Conditioning:</div>
          <div class="schedule-item"><span><strong>Day 1 (Mon):</strong> Hollow Body Rockers</span><span>3 sets x 30 sec</span></div>
          <div class="schedule-item"><span><strong>Day 3 (Wed):</strong> Handstand Snap-Downs</span><span>3 sets x 10 reps</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span><strong>Day 5 (Fri):</strong> Explosive Tuck Jumps</span><span>3 sets x 8 reps</span></div>
          <button class="btn primary" style="margin-top: 15px;" onclick="alert('Workout marked as completed for today! Great job Olivia!')">Mark Workout Complete ✓</button>
        </div>

        <div id="ath-tab-library" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Complete Drills, USASF Chart & Skill Library:</div>
          
          <div style="background:var(--deep-slate); border:1px solid var(--card-border); padding:12px; border-radius:8px; margin-bottom:12px;">
            <div style="color:var(--electric-teal); font-weight:bold; margin-bottom:6px;">⚡ Tumbling Drills & Conditioning</div>
            <div class="schedule-item"><span>Hollow Body Rockers</span><span style="color:var(--electric-teal)">Core & Shape</span></div>
            <div class="schedule-item"><span>Handstand Snap-Downs</span><span style="color:var(--electric-teal)">BHS Power Block</span></div>
            <div class="schedule-item" style="border-bottom:none;"><span>Pike Compression Holds</span><span style="color:var(--green)">3 sets x 30 sec</span></div>
          </div>

          <div style="background:var(--deep-slate); border:1px solid var(--card-border); padding:12px; border-radius:8px;">
            <div style="color:var(--electric-teal); font-weight:bold; margin-bottom:6px;">📈 USASF Levels 1-7 Summary</div>
            <div style="font-size:0.8rem; color:var(--text-muted); line-height:1.4;">
              Level 1 (Basics) &bull; Level 2 (BHS) &bull; Level 3 (Tucks) &bull; Level 4 (Layouts) &bull; Level 5 (Fulls) &bull; Level 6 (Double Fulls) &bull; Level 7 (Elite World Class).
            </div>
          </div>
        </div>

        <div id="ath-tab-billing" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Account Billing & Packages:</div>
          <div class="schedule-item"><span><strong>Active Package:</strong> Level 4 Private Lessons (4 sessions left)</span><span style="color:var(--green)">Current</span></div>
          <div class="schedule-item"><span><strong>Card on File:</strong> Visa ending in •••• 4242</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="alert('Update card modal opened')">Edit Card</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span><strong>Auto-Renewal:</strong> Enabled ($300 / 4-pack)</span><span style="color:var(--text-muted)">Active</span></div>
        </div>

        <div id="ath-tab-message" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 8px;">Direct Message Coach Flip Smith:</div>
          <textarea style="width: 100%; height: 80px; background: var(--deep-slate); border: 1px solid var(--card-border); color: white; padding: 10px; border-radius: 6px; resize: none;" placeholder="Ask Coach Flip a question..."></textarea>
          <div class="btn-group" style="margin-top:10px;">
            <button class="btn primary" onclick="alert('Message sent to Coach Flip Smith successfully!')">Send to Coach</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ========================================== -->
<!-- PARENT PORTAL EDITION: SARAH HURLEY        -->
<!-- ========================================== -->
<div id="parent-suite" class="app-container" style="display: none;">
  <div id="parent-login" class="auth-screen">
    <div class="login-logo-container">
      <svg width="280" height="55" viewBox="0 0 340 64" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M6 6H48V18H30V56H18V18H6V6Z" fill="#00E5FF"/>
        <path d="M20 14H54V26H42V56H30V26H20V14Z" fill="#0B0F19" stroke="#00E5FF" stroke-width="3"/>
        <circle cx="36" cy="11" r="3" fill="#FFFFFF"/>
        <text x="68" y="30" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">TUMBLE</text>
        <text x="68" y="56" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#00E5FF" letter-spacing="1.5">PARENT PORTAL</text>
      </svg>
      <div style="font-size: 1.4rem; font-weight: 700; color: white; margin-top: 4px;">Sarah Hurley Portal</div>
      <div class="login-subtitle">Parent Account & Student Progress Tracker</div>

      <div style="margin: 15px auto 0 auto; max-width: 380px; background: rgba(26, 34, 53, 0.7); border: 1px solid var(--card-border); border-left: 3px solid var(--electric-teal); padding: 10px 14px; border-radius: 8px; font-size: 0.8rem; font-style: italic; color: var(--text-main); text-align: left;">
        <span style="color: var(--electric-teal); font-style: normal; font-weight: bold; display: block; margin-bottom: 2px; font-size: 0.7rem; text-transform: uppercase;">Parent Notice</span>
        "Track Olivia's private lesson packages, view video progress, and coordinate directly with Coach Flip Smith."
      </div>
    </div>

    <div class="login-card">
      <div class="input-group">
        <label class="input-label">PARENT EMAIL</label>
        <input type="email" class="input-field" value="sarah.hurley@email.com">
      </div>
      <div class="input-group">
        <label class="input-label">PASSWORD</label>
        <input type="password" class="input-field" value="••••••••••••">
      </div>
      <button class="btn-login" onclick="loginParent()">ACCESS PARENT PORTAL</button>
    </div>
  </div>

  <div id="parent-main" style="display: none; width: 100%; height: 100%;">
    <div class="dashboard-screen">
      <div class="header">
        <div class="logo" onclick="logoutParent()">
          <svg width="150" height="28" viewBox="0 0 340 64" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M6 6H48V18H30V56H18V18H6V6Z" fill="#00E5FF"/>
            <path d="M20 14H54V26H42V56H30V26H20V14Z" fill="#0B0F19" stroke="#00E5FF" stroke-width="3"/>
            <circle cx="36" cy="11" r="3" fill="#FFFFFF"/>
            <text x="68" y="30" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">TUMBLE</text>
            <text x="68" y="56" font-family="'Arial Black', sans-serif" font-weight="900" font-size="22" fill="#FFFFFF" letter-spacing="1.5">COACH PRO</text>
          </svg>
        </div>
        <div style="font-size: 0.85rem; color: var(--text-muted); cursor:pointer;" onclick="logoutParent()">Sign Out</div>
      </div>

      <div class="profile-header">
        <div class="profile-avatar" style="background: rgba(0, 229, 255, 0.2); color: var(--electric-teal);">SH</div>
        <div class="profile-info">
          <h2>Sarah Hurley (Parent)</h2>
          <p>Linked Student: <strong>Olivia Hurley</strong> (Level 4 Tumbling)</p>
          <p style="color: var(--electric-teal); font-size: 0.75rem; margin-top: 3px;">Coach: Coach Flip Smith (Sovereign Strength)</p>
        </div>
      </div>

      <!-- Quick Action: Billing & Lesson Schedule -->
      <div class="card" style="cursor:default;">
        <div class="tabs">
          <div class="tab active" onclick="switchParentTab('progress')" id="ptab-prog-btn">Olivia's Progress</div>
          <div class="tab" onclick="switchParentTab('schedule')" id="ptab-sched-btn">Lesson Schedule</div>
          <div class="tab" onclick="switchParentTab('billing')" id="ptab-bill-btn">Billing & Invoices</div>
          <div class="tab" onclick="switchParentTab('message')" id="ptab-msg-btn">Message Coach</div>
        </div>

        <div id="parent-pane-progress" class="tab-content">
          <strong>Student Progression & Coach Feedback:</strong><br><br>
          <div style="background: var(--deep-slate); border: 1px solid var(--card-border); padding: 12px; border-radius: 8px; margin-bottom: 12px;">
            <strong style="color: var(--electric-teal);">Coach Flip Smith's Session Note (Oct 30):</strong><br>
            "Olivia demonstrated excellent core tension during her running layout hard-floor session. Rebound height is up 15%!"
          </div>
          <div style="background: var(--deep-slate); border: 1px solid var(--card-border); padding: 12px; border-radius: 8px;">
            <strong style="color: var(--electric-teal);">Home Conditioning Streak:</strong><br>
            3-Day Strength Conditioning Completed for this week (Hollow Body Rockers & Snap-Downs ✓).
          </div>
        </div>

        <div id="parent-pane-schedule" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Upcoming Private Lesson Schedule:</div>
          <div class="schedule-item"><span>Nov 10, 4:00 PM - 4:30 PM</span><span style="color:var(--electric-teal)">Confirmed</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span>Nov 17, 4:00 PM - 4:30 PM</span><span style="color:var(--electric-teal)">Confirmed</span></div>
          <button class="btn primary" style="margin-top: 15px;" onclick="alert('Lesson reschedule request sent to Coach Flip!')">Request Schedule Change</button>
        </div>

        <div id="parent-pane-billing" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 10px;">Billing & Lesson Packages:</div>
          <div class="schedule-item"><span><strong>Active Package:</strong> Level 4 Private Lessons (4 sessions remaining)</span><span style="color:var(--green)">Paid ($300)</span></div>
          <div class="schedule-item"><span><strong>Payment Method:</strong> Visa ending in •••• 4242</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="alert('Card update modal opened')">Update Card</span></div>
          <div class="schedule-item" style="border-bottom:none;"><span><strong>Invoice History:</strong> Nov 01, 2026</span><span style="color:var(--electric-teal); cursor:pointer;" onclick="alert('Downloading invoice PDF receipt...')">Download PDF</span></div>
          <button class="btn primary" style="margin-top: 15px;" onclick="alert('Package auto-renewed successfully!')">Renew 4-Session Package</button>
        </div>

        <div id="parent-pane-message" class="tab-content" style="display:none;">
          <div style="font-weight: bold; color:white; margin-bottom: 8px;">Direct Message Coach Flip Smith:</div>
          <textarea style="width: 100%; height: 80px; background: var(--deep-slate); border: 1px solid var(--card-border); color: white; padding: 10px; border-radius: 6px; resize: none;" placeholder="Message Coach Flip regarding schedule, billing, or progress..."></textarea>
          <div class="btn-group" style="margin-top:10px;">
            <button class="btn primary" onclick="alert('Message sent to Coach Flip Smith successfully!')">Send to Coach</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Video Playback Modal -->
<div id="video-modal">
  <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
    <h3 id="modal-video-title" style="margin:0; color:white;">Video Playback</h3>
    <button class="btn" style="flex:none; padding: 6px 14px;" onclick="closeVideoPlayer()">✕ Close</button>
  </div>
  <div class="video-container">
    <video id="modal-video-element" controls playsinline style="width:100%; height:100%; object-fit:contain; display:none;"></video>
    <div id="modal-sim-view" style="text-align:center; color: var(--electric-teal);">
      <div style="font-size: 3rem; margin-bottom: 10px;">▶</div>
      <div style="font-size: 0.9rem; color: white;" id="modal-sub-title">Playing session recording...</div>
      <div style="font-size: 0.75rem; color: var(--text-muted); margin-top: 5px;">Biomechanical Telemetry Active</div>
    </div>
  </div>
</div>

<!-- Live Camera Recorder Modal (Links to Phone Camera) -->
<div id="camera-modal">
  <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
    <h3 style="margin:0; color:white;">Live Camera Session Recorder</h3>
    <button class="btn" style="flex:none; padding: 6px 14px;" onclick="closeCameraRecorder()">✕ Close</button>
  </div>
  <div class="video-container" style="background:#111; flex-direction: column;">
    <video id="live-cam-feed" autoplay playsinline style="width:100%; height:100%; object-fit:cover; display:none;"></video>
    <div id="cam-placeholder" style="text-align:center; padding: 20px;">
      <div style="font-size: 3rem; margin-bottom: 10px;">📹</div>
      <div style="font-size: 1rem; color: white; margin-bottom: 15px;">Tap below to connect your device camera for live testing</div>
      <input type="file" accept="video/*" capture="environment" id="native-cam-input" style="display:none;" onchange="handleNativeVideoRecorded(this)">
      <button class="btn primary" style="padding: 12px 20px; margin-bottom: 10px;" onclick="startDeviceCameraWithLimit()">Start Live Camera Feed</button>
      <br>
      <button class="btn" style="padding: 10px 16px;" onclick="document.getElementById('native-cam-input').click()">Open Phone Native Recorder / Camera</button>
    </div>
  </div>
  <div style="text-align:center; color: var(--text-muted); font-size: 0.85rem;" id="cam-status">
    <i>Ready to record live tumbling runs and sync to video scrubber.</i>
  </div>
</div>

<script>
  /* Suite Mode Switching */
  function switchSuiteMode(mode) {
    document.getElementById('btn-mode-coach').classList.remove('active');
    document.getElementById('btn-mode-athlete').classList.remove('active');
    document.getElementById('btn-mode-parent').classList.remove('active');
    document.getElementById('btn-mode-' + mode).classList.add('active');

    document.getElementById('coach-suite').style.display = (mode === 'coach') ? 'block' : 'none';
    document.getElementById('athlete-suite').style.display = (mode === 'athlete') ? 'block' : 'none';
    document.getElementById('parent-suite').style.display = (mode === 'parent') ? 'block' : 'none';
  }

  function loginParent() {
    document.getElementById('parent-login').style.display = 'none';
    document.getElementById('parent-main').style.display = 'block';
  }

  function logoutParent() {
    document.getElementById('parent-main').style.display = 'none';
    document.getElementById('parent-login').style.display = 'flex';
  }

  function switchParentTab(name) {
    ['progress', 'schedule', 'billing', 'message'].forEach(t => {
      const pane = document.getElementById('parent-pane-' + t);
      const btn = document.getElementById('ptab-' + t[0] + '-btn') || document.getElementById('ptab-' + t.substring(0,4) + '-btn');
      if (pane) pane.style.display = (t === name) ? 'block' : 'none';
    });
    ['progress', 'schedule', 'billing', 'message'].forEach(t => {
      const mapKey = { progress: 'prog', schedule: 'sched', billing: 'bill', message: 'msg' };
      const btn = document.getElementById('ptab-' + mapKey[t] + '-btn');
      if (btn) {
        if (t === name) btn.classList.add('active');
        else btn.classList.remove('active');
      }
    });
  }

  /* Coach Suite Logic */
  const motivationalQuotes = [
    "Excellence is not an act, but a habit. Perfect practice makes permanent.",
    "Biomechanical precision turns hard work into soaring execution.",
    "Push taller through the toes, lock the core, and trust the rotation.",
    "Champions are built in the details of the block and the set.",
    "Every repetition is a vote for the athlete you are becoming."
  ];

  function loginCoach() {
    document.getElementById('coach-login').style.display = 'none';
    document.getElementById('coach-main').style.display = 'block';
    openCoachSection('c-screen-profile');
    renderCoachDirectory();
    renderCoachSchedule();
  }

  function logoutCoach() {
    document.getElementById('coach-main').style.display = 'none';
    document.getElementById('coach-login').style.display = 'flex';
    const rand = motivationalQuotes[Math.floor(Math.random() * motivationalQuotes.length)];
    document.getElementById('coach-quote-text').innerText = rand;
  }

  function openCoachSection(secId) {
    const screens = ['c-screen-profile', 'c-sec-directory', 'c-sec-athlete-detail', 'c-sec-videos', 'c-sec-drills', 'c-sec-levels'];
    screens.forEach(s => {
      const el = document.getElementById(s);
      if (el) el.style.display = 'none';
    });
    document.getElementById(secId).style.display = 'flex';
  }

  let athletes = [
    { 
      id: 1, name: "Olivia Hurley", level: "Level 4 Tumbling", 
      focus: "Hard-floor back tuck power, running layout mastery.",
      workout: [
        { day: "Day 1 (Mon)", name: "Hollow Body Rockers", sets: "3 sets x 30 sec" },
        { day: "Day 3 (Wed)", name: "Handstand Snap-Downs", sets: "3 sets x 10 reps" },
        { day: "Day 5 (Fri)", name: "Explosive Tuck Jumps", sets: "3 sets x 8 reps" }
      ]
    },
    { 
      id: 2, name: "Marcus Vance", level: "Level 5 Tumbling", 
      focus: "Standing full twist & layout-full connection.",
      workout: [
        { day: "Day 1 (Mon)", name: "V-Ups (Pike & Straddle)", sets: "3 sets x 15 reps" },
        { day: "Day 3 (Wed)", name: "Depth Drop Rebounds", sets: "3 sets x 8 reps" },
        { day: "Day 5 (Fri)", name: "Single-Leg Calf Raises", sets: "3 sets x 20 reps/leg" }
      ]
    },
    { 
      id: 3, name: "Chloe Bennett", level: "Level 3 Tumbling", 
      focus: "Series back handspring & rebound height.",
      workout: [
        { day: "Day 1 (Mon)", name: "Bridge Push-ups", sets: "3 sets x 10 reps" },
        { day: "Day 3 (Wed)", name: "Pike Compression Holds", sets: "3 sets x 30 sec" },
        { day: "Day 5 (Fri)", name: "Jump Squats", sets: "3 sets x 12 reps" }
      ]
    },
    { 
      id: 4, name: "Liam O'Connor", level: "Level 4 Tumbling", 
      focus: "Running layout technique & tight core tension.",
      workout: [
        { day: "Day 1 (Mon)", name: "Plank Shoulder Taps", sets: "3 sets x 40 reps" },
        { day: "Day 3 (Wed)", name: "Broad Jumps", sets: "3 sets x 8 reps" },
        { day: "Day 5 (Fri)", name: "Arch Body Holds", sets: "3 sets x 30 sec" }
      ]
    },
    { 
      id: 5, name: "Maya Lin", level: "Level 2 Tumbling", 
      focus: "Roundoff back handspring drills & cartwheel stability.",
      workout: [
        { day: "Day 1 (Mon)", name: "Wall Sit Holds", sets: "3 sets x 45 sec" },
        { day: "Day 3 (Wed)", name: "Standing Long Jumps", sets: "3 sets x 10 reps" },
        { day: "Day 5 (Fri)", name: "Supermans (Arch Lifts)", sets: "3 sets x 15 reps" }
      ]
    }
  ];

  function renderCoachDirectory() {
    const list = document.getElementById('c-athlete-list');
    list.innerHTML = `
      <div style="margin-bottom: 12px; display: flex; justify-content: space-between; align-items: center;">
        <span style="font-size: 0.85rem; font-weight: bold; color: var(--text-muted);">ACTIVE ROSTER (${athletes.length})</span>
        <button class="btn primary" style="padding: 6px 12px; font-size: 0.8rem; flex:none;" onclick="addNewAthletePrompt()">+ Add Athlete</button>
      </div>
    `;
    athletes.forEach((ath, idx) => {
      list.innerHTML += `
        <div class="card" style="position:relative;">
          <div onclick="viewCoachAthlete(${idx})">
            <div class="card-title">${ath.name}</div>
            <div class="card-sub">${ath.level}</div>
            <div style="font-size:0.85rem; color:var(--text-muted);">Current Focus: ${ath.focus}<br><span style="color:var(--electric-teal);">Open profile, workouts, billing & DM →</span></div>
          </div>
          <button onclick="deleteAthlete(${idx})" style="position:absolute; top:15px; right:15px; background:rgba(255,51,102,0.2); border:1px solid var(--danger-red); color:var(--danger-red); padding:4px 8px; border-radius:6px; font-size:0.75rem; cursor:pointer;">Delete</button>
        </div>
      `;
    });
  }

  function addNewAthletePrompt() {
    const name = prompt("Enter new athlete name:");
    if (!name) return;
    const level = prompt("Enter athlete level (e.g. Level 4 Tumbling):", "Level 3 Tumbling");
    const focus = prompt("Enter current training focus:", "Roundoff back handspring form.");
    
    athletes.push({
      id: Date.now(),
      name: name,
      level: level || "Level 3 Tumbling",
      focus: focus || "Core tension & tumbling power.",
      workout: [
        { day: "Day 1 (Mon)", name: "Hollow Body Holds", sets: "3 sets x 30 sec" },
        { day: "Day 3 (Wed)", name: "Handstand Snap-Downs", sets: "3 sets x 10 reps" },
        { day: "Day 5 (Fri)", name: "Jump Squats", sets: "3 sets x 12 reps" }
      ]
    });
    renderCoachDirectory();
    alert(`Athlete "${name}" successfully added to directory!`);
  }


  function deleteAthlete(idx) {
    const athleteName = athletes[idx].name;
    if (confirm(`Are you sure you want to delete ${athleteName} from the active roster?\n\nClick OK to confirm deletion or Cancel to keep athlete.`)) {
      athletes.splice(idx, 1);
      renderCoachDirectory();
      alert(`Athlete ${athleteName} has been successfully removed.`);
    } else {
      // Cancelled
      console.log("Deletion cancelled.");
    }
  }


  function viewCoachAthlete(idx) {
    const ath = athletes[idx];
    document.getElementById('c-detail-title').innerText = ath.name + " Profile";
    document.getElementById('c-ath-name').innerText = ath.name;
    document.getElementById('c-ath-level').innerText = ath.level;
    document.getElementById('c-ath-focus').innerText = ath.focus;

    const wList = document.getElementById('c-ath-workout-list');
    wList.innerHTML = '';
    ath.workout.forEach(w => {
      wList.innerHTML += `<div class="schedule-item"><span><strong>${w.day}:</strong> ${w.name}</span><span>${w.sets}</span></div>`;
    });

    document.getElementById('hud-ath-name').innerText = ath.name.toUpperCase();
    document.getElementById('hud-top-right-name').innerText = 'ATHLETE: ' + ath.name.toUpperCase();
    document.getElementById('c-active-ath-video').innerText = `Active Athlete: ${ath.name}`;
    openCoachSection('c-sec-athlete-detail');
    switchAthTab('focus');
  }

  function switchAthTab(tabName) {
    ['focus', 'workout', 'billing', 'dm'].forEach(t => {
      const pane = document.getElementById('ath-pane-' + t);
      const tab = document.getElementById('atab-' + t);
      if (pane) pane.style.display = (t === tabName) ? 'block' : 'none';
      if (tab) {
        if (t === tabName) tab.classList.add('active');
        else tab.classList.remove('active');
      }
    });
  }

  /* Schedule CRUD & Views */
  let lessons = [
    { id: 1, name: "Olivia Hurley (Private 30m)", time: "Nov 10, 4:00 PM" },
    { id: 2, name: "Marcus Vance (Private 30m)", time: "Nov 11, 5:00 PM" },
    { id: 3, name: "Chloe Bennett (Private 30m)", time: "Nov 12, 4:00 PM" },
    { id: 4, name: "Liam O'Connor (Private 30m)", time: "Nov 14, 3:30 PM" },
    { id: 5, name: "Maya Lin (Private 30m)", time: "Nov 15, 4:30 PM" }
  ];
  let calView = 'week';

  function setCalView(v) {
    calView = v;
    document.getElementById('c-btn-day').classList.remove('active');
    document.getElementById('c-btn-week').classList.remove('active');
    document.getElementById('c-btn-month').classList.remove('active');
    document.getElementById('c-btn-' + v).classList.add('active');
    renderCoachSchedule();
  }

  function renderCoachSchedule() {
    const container = document.getElementById('c-schedule-list');
    container.innerHTML = '';
    
    if (calView === 'day') {
      container.innerHTML = '<div style="font-weight:bold; color:var(--electric-teal); margin-bottom:8px; font-size:0.85rem;">Today\'s Schedule (Nov 5):</div>';
      container.innerHTML += `<div class="schedule-item-row"><div class="schedule-info"><span style="font-weight:bold; color:white;">Olivia Hurley (Private 30m)</span><span class="schedule-time">5:00 PM - 5:30 PM</span></div></div>`;
    } else if (calView === 'week') {
      container.innerHTML = '<div style="font-weight:bold; color:var(--electric-teal); margin-bottom:8px; font-size:0.85rem;">7-Day Private Lesson Schedule:</div>';
      const weekDays = [
        { day: "Thursday (Nov 05)", desc: "Olivia Hurley - 5:00 PM" },
        { day: "Friday (Nov 06)", desc: "Marcus Vance - 4:00 PM" },
        { day: "Saturday (Nov 07)", desc: "Open Gym Private Block - 8:00 PM" },
        { day: "Sunday (Nov 08)", desc: "Rest Day / Video Review" },
        { day: "Monday (Nov 09)", desc: "Chloe Bennett - 4:00 PM" },
        { day: "Tuesday (Nov 10)", desc: "Olivia Hurley - 4:00 PM" },
        { day: "Wednesday (Nov 11)", desc: "Liam O'Connor - 3:30 PM" }
      ];
      weekDays.forEach(wd => {
        container.innerHTML += `
          <div class="schedule-item-row">
            <div class="schedule-info">
              <span style="font-weight:bold; color:white;">${wd.day}</span>
              <span class="schedule-time">${wd.desc}</span>
            </div>
          </div>
        `;
      });
    } else if (calView === 'month') {
      container.innerHTML = `
        <div style="background:var(--deep-slate); border:1px solid var(--card-border); padding:12px; border-radius:8px; text-align:center;">
          <div style="font-weight:bold; color:var(--electric-teal); margin-bottom:8px; font-size:0.95rem;">November 2026 Master Calendar</div>
          <div style="display:grid; grid-template-columns: repeat(7, 1fr); gap:4px; font-size:0.75rem; color:var(--text-muted); margin-bottom:6px;">
            <div>Su</div><div>Mo</div><div>Tu</div><div>We</div><div>Th</div><div>Fr</div><div>Sa</div>
            <div>1</div><div>2</div><div>3</div><div>4</div><div style="color:var(--electric-teal); font-weight:bold;">5*</div><div>6</div><div>7</div>
            <div>8</div><div>9</div><div>10</div><div>11</div><div>12</div><div>13</div><div>14</div>
            <div>15</div><div>16</div><div>17</div><div>18</div><div>19</div><div>20</div><div>21</div>
            <div>22</div><div>23</div><div>24</div><div>25</div><div>26</div><div>27</div><div>28</div>
            <div>29</div><div>30</div><div></div><div></div><div></div><div></div><div></div>
          </div>
          <div style="font-size:0.75rem; color:white; margin-top:6px;">* Active Session Today &bull; 22 Private Lessons Booked this Month</div>
        </div>
      `;
    }
  }

  function addLesson() {
    const n = document.getElementById('c-new-name').value;
    const t = document.getElementById('c-new-time').value;
    if (!n || !t) { alert('Please enter both name and time.'); return; }
    lessons.push({ id: Date.now(), name: n, time: t });
    document.getElementById('c-new-name').value = '';
    document.getElementById('c-new-time').value = '';
    renderCoachSchedule();
  }

  function editLesson(id) {
    const l = lessons.find(x => x.id === id);
    if (!l) return;
    const nt = prompt("Adjust lesson time:", l.time);
    if (nt) { l.time = nt; renderCoachSchedule(); }
  }

  function deleteLesson(id) {
    lessons = lessons.filter(x => x.id !== id);
    renderCoachSchedule();
  }

  /* Athlete Suite Logic */
  function loginAthlete() {
    document.getElementById('athlete-login').style.display = 'none';
    document.getElementById('athlete-main').style.display = 'block';
  }

  function logoutAthlete() {
    document.getElementById('athlete-main').style.display = 'none';
    document.getElementById('athlete-login').style.display = 'flex';
  }

  function switchAthPortalTab(name) {
    ['focus', 'videos', 'workout', 'library', 'billing', 'message'].forEach(t => {
      const pane = document.getElementById('ath-tab-' + t);
      const btn = document.getElementById('ath-tab-' + t[0] + '-btn');
      if (pane) pane.style.display = (t === name) ? 'block' : 'none';
      if (btn) {
        if (t === name) btn.classList.add('active');
        else btn.classList.remove('active');
      }
    });
  }


  let uploadedVideosList = [];


  function launchScrubberWithVideo(title, videoUrl) {
    // Hide portrait screens and show landscape scrubber suite
    document.getElementById('coach-main').style.display = 'none';
    document.getElementById('athlete-main').style.display = 'none';
    
    const scrubberDiv = document.getElementById('landscape-scrubber');
    scrubberDiv.style.display = 'block';
    
    // Update hud title
    document.getElementById('hud-ath-name').innerText = title.toUpperCase();
    document.getElementById('hud-top-right-name').innerText = 'CUSTOM UPLOAD: ' + title.toUpperCase();
  }


  function renderUploadedVideos() {
    // We can inject or update a container for uploaded videos
    const containers = document.querySelectorAll('.uploaded-videos-container');
    containers.forEach(container => {
      if (uploadedVideosList.length === 0) {
        container.innerHTML = '<div style="font-size:0.8rem; color:var(--text-muted); font-style:italic; margin-bottom:10px;">No uploaded videos yet.</div>';
        return;
      }
      let html = '<div style="font-weight:bold; color:var(--electric-teal); margin: 10px 0 6px 0; font-size:0.85rem;">Uploaded Videos Section:</div>';
      uploadedVideosList.forEach((v, index) => {
        html += `
          <div class="schedule-item" style="background: rgba(0, 229, 255, 0.05); padding: 8px 10px; border-radius: 6px; margin-bottom: 6px;">
            <div><strong style="color:white;">${v.name}</strong><br><span style="font-size:0.7rem; color:var(--text-muted);">Uploaded on ${v.date}</span></div>
            <button class="btn primary" style="padding: 5px 10px; font-size: 0.75rem; flex:none;" onclick="openVideoPlaybackUrl('${v.name}', '${v.url}')">▶ Play</button>
          </div>
        `;
      });
      container.innerHTML = html;
    });
  }



  function openVideoPlaybackUrl(title, videoUrl) {
    document.getElementById('modal-video-title').innerText = title;
    document.getElementById('modal-sub-title').innerText = `Playback: ${title}`;
    
    const videoEl = document.getElementById('modal-video-element');
    const simView = document.getElementById('modal-sim-view');
    
    if (videoUrl) {
      videoEl.src = videoUrl;
      videoEl.style.display = 'block';
      simView.style.display = 'none';
      videoEl.load();
      videoEl.play().catch(e => {
        console.log("Autoplay prevented by browser policy, controls enabled for manual play:", e);
      });
    } else {
      videoEl.style.display = 'none';
      simView.style.display = 'block';
    }
    document.getElementById('video-modal').style.display = 'flex';
  }


  function openVideoPlayer(title) {
    openVideoPlaybackUrl(title, null);
  }

  function closeVideoPlayer() {
    const videoEl = document.getElementById('modal-video-element');
    videoEl.pause();
    videoEl.src = '';
    document.getElementById('video-modal').style.display = 'none';
  }

  /* Camera Live Recorder Link */
  function openCameraRecorder() {
    document.getElementById('camera-modal').style.display = 'flex';
  }

  function closeCameraRecorder() {
    document.getElementById('camera-modal').style.display = 'none';
    const video = document.getElementById('live-cam-feed');
    if (video.srcObject) {
      video.srcObject.getTracks().forEach(track => track.stop());
    }
    video.style.display = 'none';
    document.getElementById('cam-placeholder').style.display = 'block';
  }

  async function startDeviceCamera() {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' }, audio: true });
      const video = document.getElementById('live-cam-feed');
      video.srcObject = stream;
      video.style.display = 'block';
      document.getElementById('cam-placeholder').style.display = 'none';
      document.getElementById('cam-status').innerHTML = '<i>🔴 Live camera streaming active. Ready to capture tumbling run!</i>';
    } catch (err) {
      alert('Camera access denied or unavailable. Use the phone native recorder button below.');
    }
  }

  function handleNativeVideoRecorded(input) {
    if (input.files && input.files[0]) {
      alert('Video file successfully recorded and linked to session library!');
      closeCameraRecorder();
    }
  }

  /* Scrubber & Drawing Engine with Movable Trajectory & Color Palette */
  let splitActive = false;
  function toggleSplitScreen() {
    splitActive = !splitActive;
    const overlay = document.getElementById('split-overlay');
    const btn = document.getElementById('split-btn');
    if (splitActive) {
      overlay.style.display = 'block';
      btn.style.background = 'var(--electric-teal)';
      btn.style.color = 'var(--deep-slate)';
      btn.innerText = 'Split: On';
    } else {
      overlay.style.display = 'none';
      btn.style.background = 'var(--card-border)';
      btn.style.color = 'var(--text-main)';
      btn.innerText = 'Split: Off';
    }
  }

  let trajectoryVisible = true;
  function toggleTrajectory() {
    trajectoryVisible = !trajectoryVisible;
    const group = document.getElementById('trajectory-group');
    const btn = document.getElementById('traj-btn');
    if (trajectoryVisible) {
      group.style.display = 'inline';
      btn.style.background = 'var(--electric-teal)';
      btn.style.color = 'var(--deep-slate)';
      btn.innerText = 'Trajectory: On';
    } else {
      group.style.display = 'none';
      btn.style.background = 'var(--card-border)';
      btn.style.color = 'var(--text-main)';
      btn.innerText = 'Trajectory: Off';
    }
  }

  /* Movable Trajectory Dragging Logic */
  let isDraggingTraj = false;
  let dragStartX = 0, dragStartY = 0;
  let trajTranslateX = 0, trajTranslateY = 0;

  function startDragTrajectory(e) {
    isDraggingTraj = true;
    const clientX = e.clientX || (e.touches && e.touches[0].clientX);
    const clientY = e.clientY || (e.touches && e.touches[0].clientY);
    dragStartX = clientX - trajTranslateX;
    dragStartY = clientY - trajTranslateY;
    e.stopPropagation();
  }

  window.addEventListener('mousemove', (e) => {
    if (!isDraggingTraj) return;
    const clientX = e.clientX;
    const clientY = e.clientY;
    trajTranslateX = clientX - dragStartX;
    trajTranslateY = clientY - dragStartY;
    const group = document.getElementById('trajectory-group');
    group.setAttribute('transform', `translate(${trajTranslateX}, ${trajTranslateY})`);
  });

  window.addEventListener('mouseup', () => isDraggingTraj = false);

  function updateScrubber() {
    const val = document.getElementById('scrubber').value / 100;
    const athlete = document.getElementById('athlete-hinge');
    const torsoPath = document.getElementById('torso-path');
    const legsPath = document.getElementById('legs-path');
    const arms = document.getElementById('arms');
    const phaseLabel = document.getElementById('phase-indicator');
    
    let x = 200 + trajTranslateX, y = 400 + trajTranslateY, globalRotation = 0;
    let tCurve = "-15 -25", lCurve = "15 35";
    let armX2 = 35, armY2 = -95;

    if (val < 0.33) {
      let p = val / 0.33;
      x = 200 + trajTranslateX + (200 * p);
      y = 400 + trajTranslateY - (50 * Math.sin(p * Math.PI));
      globalRotation = 110 * p;
      let archFactor = 1 - p;
      tCurve = `${-25 * archFactor} ${-25 * archFactor}`;
      lCurve = `${25 * archFactor} ${35 * archFactor}`;
      if(phaseLabel) phaseLabel.innerText = "Phase: 1. Hurdle to Straight Line";
    } else if (val < 0.66) {
      let p = (val - 0.33) / 0.33;
      x = 400 + trajTranslateX + (200 * p);
      y = 350 + trajTranslateY - (150 * Math.sin(p * Math.PI));
      globalRotation = 110 + (180 * p);
      let deepArch = 35 * Math.sin(p * Math.PI);
      tCurve = `${-35 - deepArch} -25`;
      lCurve = `${35 + deepArch} 35`;
      armX2 = 35 + (25 * p);
      armY2 = -95 - (15 * p);
      if(phaseLabel) phaseLabel.innerText = "Phase: 2. Deep Arch BHS Block";
    } else {
      let p = (val - 0.66) / 0.34;
      x = 600 + trajTranslateX + (100 * p);
      y = 400 + trajTranslateY - (200 * p);
      globalRotation = 360 + (15 * p);
      tCurve = "0 -25";
      lCurve = "0 35";
      armX2 = 60; armY2 = -110;
      if(phaseLabel) phaseLabel.innerText = "Phase: 3. Upright Takeoff & Set";
    }

    if (athlete) {
      athlete.setAttribute('transform', `translate(${x}, ${y}) rotate(${globalRotation})`);
      torsoPath.setAttribute('d', `M 0 0 Q ${tCurve} 0 -50`);
      legsPath.setAttribute('d', `M 0 0 Q ${lCurve} 0 75`);
      arms.setAttribute('x2', armX2);
      arms.setAttribute('y2', armY2);
    }
  }

  let drawingActive = false;
  let isDrawing = false;
  let lastX = 0;
  let lastY = 0;
  const canvas = document.getElementById('drawing-canvas');
  const ctx = canvas.getContext('2d');

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight - 32;
    ctx.strokeStyle = '#FF3366';
    ctx.lineWidth = 4;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
  }
  window.addEventListener('resize', resizeCanvas);
  window.addEventListener('load', resizeCanvas);

  function toggleDrawingMode() {
    drawingActive = !drawingActive;
    const btn = document.getElementById('draw-btn');
    const palette = document.getElementById('palette-bar');
    if (drawingActive) {
      canvas.style.pointerEvents = 'auto';
      btn.style.background = 'var(--electric-teal)';
      btn.style.color = 'var(--deep-slate)';
      btn.innerText = 'Draw: On';
      palette.style.display = 'flex';
    } else {
      canvas.style.pointerEvents = 'none';
      btn.style.background = 'var(--card-border)';
      btn.style.color = 'var(--text-main)';
      btn.innerText = 'Draw: Off';
      palette.style.display = 'none';
    }
  }

  function setDrawingColor(colorHex, swatchElement) {
    ctx.strokeStyle = colorHex;
    document.querySelectorAll('.color-swatch').forEach(s => s.classList.remove('active'));
    swatchElement.classList.add('active');
  }

  function clearDrawings() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }

  canvas.addEventListener('mousedown', (e) => {
    if (!drawingActive) return;
    isDrawing = true;
    [lastX, lastY] = [e.clientX, e.clientY];
  });

  canvas.addEventListener('mousemove', (e) => {
    if (!drawingActive || !isDrawing) return;
    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(e.clientX, e.clientY);
    ctx.stroke();
    [lastX, lastY] = [e.clientX, e.clientY];
  });

  canvas.addEventListener('mouseup', () => isDrawing = false);
  canvas.addEventListener('mouseout', () => isDrawing = false);

  canvas.addEventListener('touchstart', (e) => {
    if (!drawingActive) return;
    isDrawing = true;
    const touch = e.touches[0];
    const rect = canvas.getBoundingClientRect();
    lastX = touch.clientX - rect.left;
    lastY = touch.clientY - rect.top;
    e.preventDefault();
  });

  canvas.addEventListener('touchmove', (e) => {
    if (!drawingActive || !isDrawing) return;
    const touch = e.touches[0];
    const rect = canvas.getBoundingClientRect();
    const currentX = touch.clientX - rect.left;
    const currentY = touch.clientY - rect.top;

    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(currentX, currentY);
    ctx.stroke();
    [lastX, lastY] = [currentX, currentY];
    e.preventDefault();
  });

  canvas.addEventListener('touchend', () => isDrawing = false);

  // Auto-detect orientation or guide user to rotate horizontal for video scrubber
  window.addEventListener("orientationchange", function() {
    if (window.orientation === 90 || window.orientation === -90 || window.innerWidth > window.innerHeight) {
      console.log("Landscape orientation detected - Video scrubber active.");
    } else {
      console.log("Portrait orientation active - Rotate horizontal to access video scrubber suite.");
    }
  });


  // 60-second duration validator for uploaded videos
  function handleVideoUpload(input) {
    if (input.files && input.files[0]) {
      const file = input.files[0];
      const url = URL.createObjectURL(file);
      
      const tempVideo = document.createElement('video');
      tempVideo.src = url;
      tempVideo.onloadedmetadata = function() {
        window.URL.revokeObjectURL(tempVideo.src);
        if (tempVideo.duration > 65) { // allow slight tolerance or strict 60s
          alert(`Warning: The uploaded video is ${Math.round(tempVideo.duration)} seconds long. Tumble Coach Pro optimizes analysis for clips under 60 seconds. Please upload a clip 60 seconds or shorter.`);
        }
        
        uploadedVideosList.push({ name: file.name, url: url, date: new Date().toLocaleDateString(), duration: Math.round(tempVideo.duration) });
        renderUploadedVideos();
        
        if (confirm(`Video "${file.name}" uploaded successfully! Launch in Biometric Video Scrubber Suite for direct markup?`)) {
          launchScrubberWithVideo(file.name, url);
        } else {
          openVideoPlaybackUrl(file.name, url);
        }
      };
    }
  }

  // Live recording 60-second limit notice / countdown simulator
  let recordingTimer = null;
  let recordingSeconds = 0;

  function startDeviceCameraWithLimit() {
    startDeviceCamera();
    recordingSeconds = 0;
    if (recordingTimer) clearInterval(recordingTimer);
    
    recordingTimer = setInterval(() => {
      recordingSeconds++;
      const statusEl = document.getElementById('cam-status');
      if (statusEl) {
        statusEl.innerHTML = `<i>🔴 Live recording active... (${recordingSeconds}/60s max limit)</i>`;
      }
      if (recordingSeconds >= 60) {
        clearInterval(recordingTimer);
        alert("60-second recording limit reached. Stopping live recording automatically.");
        closeCameraRecorder();
      }
    }, 1000);
  }


  function dismissSplash() {
    const splash = document.getElementById('splash-screen');
    splash.style.transition = 'opacity 0.4s ease';
    splash.style.opacity = '0';
    setTimeout(() => {
      splash.style.display = 'none';
    }, 400);
  }


  // Rotating motivational words and definitions for the splash screen
  const splashQuotes = [
    {
      title: "Strength",
      definition: "The quality or state of being physically and mentally strong; the capacity of an athlete to exert force and execute flawless power under pressure."
    },
    {
      title: "Determination",
      definition: "Firmness of purpose; the unwavering resolve to master difficult tumbling progressions, stick every landing, and push past physical limits."
    },
    {
      title: "Excellence",
      definition: "The quality of being outstanding; a daily habit of perfect practice where repetition turns technique into permanent mastery."
    },
    {
      title: "Commitment",
      definition: "The state of being dedicated to a cause or athletic goal; showing up consistently for every private lesson, conditioning circuit, and mental rehearsal."
    },
    {
      title: "Biomechanical Analysis",
      definition: "The detailed scientific study and measurement of movement mechanics, optimizing athletic power output, flight trajectory, and form execution."
    }
  ];

  let currentQuoteIndex = 0;

  function rotateSplashQuote() {
    currentQuoteIndex = (currentQuoteIndex + 1) % splashQuotes.length;
    const q = splashQuotes[currentQuoteIndex];
    
    const titleEl = document.getElementById('splash-quote-title');
    const textEl = document.getElementById('splash-quote-text');
    
    if (titleEl && textEl) {
      titleEl.style.opacity = '0';
      textEl.style.opacity = '0';
      
      setTimeout(() => {
        titleEl.innerText = q.title.toUpperCase();
        textEl.innerText = `“${q.definition}”`;
        titleEl.style.opacity = '1';
        titleEl.style.opacity = '1';
      }, 300);
    }
  }

  // Auto rotate every 4.5 seconds
  setInterval(rotateSplashQuote, 10000);


  function switchDrillTab(tabName) {
    const drillsPane = document.getElementById('drill-pane-drills');
    const condPane = document.getElementById('drill-pane-cond');
    const drillsTab = document.getElementById('dtab-drills');
    const condTab = document.getElementById('dtab-cond');

    if (tabName === 'drills') {
      drillsPane.style.display = 'block';
      condPane.style.display = 'none';
      drillsTab.classList.add('active');
      condTab.classList.remove('active');
    } else {
      drillsPane.style.display = 'none';
      condPane.style.display = 'block';
      drillsTab.classList.remove('active');
      condTab.classList.add('active');
    }
  }


  function switchLevelTab(name) {
    const chartPane = document.getElementById('lpane-chart');
    const defsPane = document.getElementById('lpane-defs');
    const chartTab = document.getElementById('ltab-chart');
    const defsTab = document.getElementById('ltab-defs');

    if (name === 'chart') {
      chartPane.style.display = 'block';
      defsPane.style.display = 'none';
      chartTab.classList.add('active');
      defsTab.classList.remove('active');
    } else {
      chartPane.style.display = 'none';
      defsPane.style.display = 'block';
      chartTab.classList.remove('active');
      defsTab.classList.add('active');
    }
  }

</script>
</body>
</html>