```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>JEE STUDY OS — MISSION IIT BOMBAY</title>

  <!-- Google Fonts: Inter & JetBrains Mono -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet" />

  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
            mono: ['JetBrains Mono', 'ui-monospace', 'SFMono-Regular', 'monospace'],
          },
          colors: {
            obsidian: '#070a12',
            navy: '#0d1220',
            surface: '#12192b',
          }
        }
      }
    };
  </script>

  <style>
    /* Dynamic Accent Theme Variables */
    :root {
      --accent: #3b82f6;
      --accent-rgb: 59, 130, 246;
      --accent-secondary: #8b5cf6;
      --accent-secondary-rgb: 139, 92, 246;
      --bg-main: #f1f5f9;
      --bg-card: rgba(255, 255, 255, 0.85);
      --border-subtle: rgba(15, 23, 42, 0.09);
      --text-primary: #0f172a;
      --text-muted: #475569;
    }

    html.dark {
      --bg-main: #070a12;
      --bg-card: rgba(15, 22, 38, 0.72);
      --border-subtle: rgba(148, 163, 184, 0.12);
      --text-primary: #f8fafc;
      --text-muted: #94a3b8;
    }

    body {
      background-color: var(--bg-main);
      color: var(--text-primary);
      font-family: 'Inter', system-ui, sans-serif;
      overflow-x: hidden;
      -webkit-tap-highlight-color: transparent;
    }

    /* Ambient Futuristic Grid & Glow */
    html.dark body {
      background-image:
        radial-gradient(circle at 12% 8%, rgba(var(--accent-rgb), 0.14) 0%, transparent 38%),
        radial-gradient(circle at 88% 15%, rgba(var(--accent-secondary-rgb), 0.12) 0%, transparent 36%),
        linear-gradient(rgba(148, 163, 184, 0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(148, 163, 184, 0.03) 1px, transparent 1px);
      background-size: 100% 100%, 100% 100%, 42px 42px, 42px 42px;
      background-attachment: fixed;
    }

    .glass-card {
      background: var(--bg-card);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      border: 1px solid var(--border-subtle);
      transition: border-color 0.2s ease, transform 0.2s ease, box-shadow 0.2s ease;
    }

    .glass-card:hover {
      border-color: rgba(var(--accent-rgb), 0.28);
    }

    .accent-bg { background-color: var(--accent); }
    .accent-text { color: var(--accent); }
    .accent-border { border-color: var(--accent); }
    .accent-gradient {
      background: linear-gradient(135deg, var(--accent), var(--accent-secondary));
    }
    .accent-ring-glow {
      box-shadow: 0 0 24px rgba(var(--accent-rgb), 0.28);
    }

    /* Custom Scrollbar */
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.25);
      border-radius: 999px;
    }
    ::-webkit-scrollbar-thumb:hover { background: rgba(148, 163, 184, 0.45); }

    /* Circular SVG Timer Transition */
    .timer-ring-progress {
      transition: stroke-dashoffset 0.35s linear;
      transform: rotate(-90deg);
      transform-origin: 50% 50%;
    }

    /* Hide scrollbar on horizontal tab strips */
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

    /* Pulse animation for overtime warning */
    @keyframes pulseDanger {
      0%, 100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.45); }
      50% { box-shadow: 0 0 0 14px rgba(239, 68, 68, 0); }
    }
    .overtime-pulse { animation: pulseDanger 1.5s infinite; }

    /* Smooth section entry */
    .view-section { animation: fadeIn 0.2s ease-out; }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(4px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* Custom Checkbox */
    .jee-check {
      accent-color: var(--accent);
      width: 1rem;
      height: 1rem;
      cursor: pointer;
    }
  </style>
</head>
<body class="min-h-screen antialiased selection:bg-blue-500/30">

  <!-- ==================== FULLSCREEN CLOCK OVERLAY ==================== -->
  <div id="fullscreen-clock-overlay" class="fixed inset-0 z-[100] hidden flex-col items-center justify-center bg-[#04060b] text-white p-6">
    <button onclick="toggleFullscreenClock(false)" class="absolute top-6 right-6 px-4 py-2 rounded-xl bg-white/10 hover:bg-white/20 text-sm font-medium backdrop-blur flex items-center gap-2 transition">
      <span>✕</span> Exit Fullscreen
    </button>
    <div class="text-xs uppercase tracking-[0.35em] text-blue-400 font-semibold mb-4">JEE STUDY OS — MISSION IIT BOMBAY</div>
    <div id="fs-clock-time" class="font-mono text-6xl sm:text-8xl md:text-[10rem] font-bold tracking-tight leading-none text-white drop-shadow-[0_0_40px_rgba(59,130,246,0.35)]">00:00:00</div>
    <div id="fs-clock-date" class="mt-6 text-lg sm:text-2xl text-slate-400 font-medium"></div>
    <div id="fs-timer-pill" class="mt-8 hidden px-5 py-2.5 rounded-full bg-blue-500/15 border border-blue-500/30 text-blue-300 font-mono text-sm"></div>
  </div>

  <!-- ==================== ALARM RINGING BANNER ==================== -->
  <div id="alarm-banner" class="fixed top-4 left-1/2 -translate-x-1/2 z-[95] hidden w-[92%] max-w-md rounded-2xl bg-rose-600 text-white p-4 shadow-2xl border border-rose-400/50 flex items-center justify-between gap-4">
    <div class="flex items-center gap-3">
      <span class="text-2xl animate-bounce">⏰</span>
      <div>
        <div class="font-bold text-sm">ALARM RINGING</div>
        <div id="alarm-banner-label" class="text-xs text-rose-100">Study Alarm</div>
      </div>
    </div>
    <button onclick="dismissAlarm()" class="px-4 py-2 rounded-xl bg-white text-rose-700 font-bold text-xs hover:bg-rose-50 transition">Dismiss</button>
  </div>

  <!-- ==================== TOAST CONTAINER ==================== -->
  <div id="toast-container" class="fixed bottom-20 lg:bottom-6 right-4 z-[90] flex flex-col gap-2 max-w-sm w-[calc(100%-2rem)] pointer-events-none"></div>

  <!-- ==================== CUSTOM MODAL OVERLAY ==================== -->
  <div id="modal-overlay" class="fixed inset-0 z-[85] hidden items-center justify-center bg-black/70 backdrop-blur-sm p-4">
    <div id="modal-box" class="glass-card w-full max-w-md rounded-2xl p-6 shadow-2xl bg-white dark:bg-[#0d1220] border border-slate-200 dark:border-slate-700/60">
      <div class="flex items-center justify-between mb-3">
        <h3 id="modal-title" class="text-lg font-bold">Confirm</h3>
        <button onclick="closeModal()" class="p-1.5 rounded-lg hover:bg-slate-200 dark:hover:bg-slate-800 text-slate-400">✕</button>
      </div>
      <div id="modal-body" class="text-sm text-slate-600 dark:text-slate-300 mb-5"></div>
      <div id="modal-actions" class="flex flex-wrap justify-end gap-2"></div>
    </div>
  </div>

  <!-- ==================== MOBILE DRAWER OVERLAY ==================== -->
  <div id="mobile-drawer-overlay" onclick="toggleMobileDrawer(false)" class="fixed inset-0 z-40 bg-black/60 backdrop-blur-xs hidden lg:hidden"></div>

  <!-- ==================== MAIN SHELL ==================== -->
  <div class="flex min-h-screen w-full">

    <!-- DESKTOP / MOBILE DRAWER SIDEBAR -->
    <aside id="sidebar" class="fixed lg:sticky top-0 left-0 z-50 h-screen w-64 -translate-x-full lg:translate-x-0 transition-all duration-300 flex flex-col glass-card border-r border-slate-200/80 dark:border-slate-800/80 bg-white/95 dark:bg-[#0a0f1c]/95 shrink-0">
      <!-- Brand Header -->
      <div class="p-4 border-b border-slate-200/70 dark:border-slate-800/70 flex items-center justify-between gap-2">
        <div class="flex items-center gap-2.5 overflow-hidden">
          <div class="w-9 h-9 rounded-xl accent-gradient flex items-center justify-center text-white font-extrabold text-sm shrink-0 shadow-lg">
            IITB
          </div>
          <div class="sidebar-label truncate">
            <div class="font-extrabold text-sm tracking-tight leading-tight">JEE STUDY OS</div>
            <div class="text-[10px] font-semibold tracking-widest uppercase accent-text">Mission IIT Bombay</div>
          </div>
        </div>
        <button onclick="toggleSidebarCollapse()" title="Collapse Sidebar" class="hidden lg:flex p-1.5 rounded-lg hover:bg-slate-200/70 dark:hover:bg-slate-800 text-slate-400 transition">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M11 19l-7-7 7-7m8 14l-7-7 7-7"/></svg>
        </button>
        <button onclick="toggleMobileDrawer(false)" class="lg:hidden p-1.5 rounded-lg hover:bg-slate-200 dark:hover:bg-slate-800 text-slate-400">✕</button>
      </div>

      <!-- Mini Live Timer Pill in Sidebar -->
      <div id="sidebar-timer-status" onclick="navigate('timer')" class="mx-3 mt-3 p-2.5 rounded-xl bg-slate-100 dark:bg-slate-900/90 border border-slate-200/80 dark:border-slate-800 cursor-pointer hover:border-blue-500/40 transition flex items-center justify-between gap-2">
        <div class="flex items-center gap-2 min-w-0">
          <span id="sidebar-timer-dot" class="w-2.5 h-2.5 rounded-full bg-slate-400 shrink-0"></span>
          <div class="sidebar-label truncate">
            <div id="sidebar-timer-phase" class="text-[10px] uppercase tracking-wider text-slate-500 dark:text-slate-400 font-semibold">Timer Idle</div>
            <div id="sidebar-timer-clock" class="font-mono text-xs font-bold">25:00</div>
          </div>
        </div>
        <span class="sidebar-label text-[10px] px-1.5 py-0.5 rounded bg-blue-500/10 accent-text font-semibold">GO</span>
      </div>

      <!-- 12 Navigation Items -->
      <nav id="sidebar-nav" class="flex-1 overflow-y-auto p-3 space-y-1">
        <!-- Populated by JS -->
      </nav>

      <!-- Sidebar Footer -->
      <div class="p-3 border-t border-slate-200/70 dark:border-slate-800/70 flex items-center justify-between gap-2">
        <button onclick="toggleTheme()" class="flex-1 py-2 px-3 rounded-xl bg-slate-100 dark:bg-slate-900 hover:bg-slate-200 dark:hover:bg-slate-800 text-xs font-semibold flex items-center justify-center gap-1.5 transition">
          <span id="theme-icon">🌙</span>
          <span class="sidebar-label" id="theme-label">Dark</span>
        </button>
        <button onclick="toggleAppFullscreen()" title="Fullscreen Focus Mode (F)" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-900 hover:bg-slate-200 dark:hover:bg-slate-800 text-xs font-semibold transition">
          ⛶
        </button>
      </div>
    </aside>

    <!-- MAIN CONTENT AREA -->
    <div class="flex-1 flex flex-col min-w-0 pb-20 lg:pb-6">
      <!-- Top Bar -->
      <header class="sticky top-0 z-30 glass-card border-b border-slate-200/70 dark:border-slate-800/70 px-4 sm:px-6 py-3 flex items-center justify-between gap-3">
        <div class="flex items-center gap-3 min-w-0">
          <button onclick="toggleMobileDrawer(true)" class="lg:hidden p-2 rounded-xl bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 transition" aria-label="Open menu">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16"/></svg>
          </button>
          <div class="truncate">
            <h1 id="topbar-title" class="text-base sm:text-lg font-bold tracking-tight truncate">Overview</h1>
            <p id="topbar-date" class="text-[11px] text-slate-500 dark:text-slate-400 truncate"></p>
          </div>
        </div>

        <div class="flex items-center gap-2 sm:gap-3 shrink-0">
          <!-- Header Live Clock -->
          <button onclick="navigate('clock')" class="hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-xl bg-slate-100 dark:bg-slate-900/90 border border-slate-200 dark:border-slate-800 hover:border-blue-500/40 transition">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
            <span id="topbar-clock" class="font-mono text-xs font-semibold">00:00:00</span>
          </button>
          <!-- Today Study Pill -->
          <div class="px-3 py-1.5 rounded-xl bg-blue-500/10 border border-blue-500/20 text-xs font-semibold accent-text">
            Today: <span id="topbar-today-hours" class="font-mono">0.0h</span>
          </div>
          <!-- Focus Fullscreen -->
          <button onclick="toggleAppFullscreen()" title="Fullscreen Focus Mode (F)" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-800/80 hover:bg-slate-200 dark:hover:bg-slate-700 text-xs transition">
            ⛶
          </button>
        </div>
      </header>

      <!-- Dynamic Section Container -->
      <main id="app-main" class="flex-1 p-4 sm:p-6 max-w-[1440px] w-full mx-auto">
        <!-- Rendered dynamically by JS -->
      </main>
    </div>
  </div>

  <!-- ==================== MOBILE BOTTOM NAVIGATION ==================== -->
  <nav class="lg:hidden fixed bottom-0 left-0 right-0 z-30 glass-card border-t border-slate-200/80 dark:border-slate-800/80 bg-white/95 dark:bg-[#0a0f1c]/95 px-2 py-1.5 flex items-center justify-around">
    <button onclick="navigate('overview')" data-mobnav="overview" class="mob-nav-btn flex flex-col items-center gap-0.5 px-3 py-1 rounded-xl text-[10px] font-semibold text-slate-500 dark:text-slate-400">
      <span class="text-base">🏠</span><span>Overview</span>
    </button>
    <button onclick="navigate('timer')" data-mobnav="timer" class="mob-nav-btn flex flex-col items-center gap-0.5 px-3 py-1 rounded-xl text-[10px] font-semibold text-slate-500 dark:text-slate-400">
      <span class="text-base">⏱️</span><span>Timer</span>
    </button>
    <button onclick="navigate('syllabus')" data-mobnav="syllabus" class="mob-nav-btn flex flex-col items-center gap-0.5 px-3 py-1 rounded-xl text-[10px] font-semibold text-slate-500 dark:text-slate-400">
      <span class="text-base">📚</span><span>Syllabus</span>
    </button>
    <button onclick="navigate('tasks')" data-mobnav="tasks" class="mob-nav-btn flex flex-col items-center gap-0.5 px-3 py-1 rounded-xl text-[10px] font-semibold text-slate-500 dark:text-slate-400">
      <span class="text-base">✅</span><span>Tasks</span>
    </button>
    <button onclick="toggleMobileDrawer(true)" class="flex flex-col items-center gap-0.5 px-3 py-1 rounded-xl text-[10px] font-semibold text-slate-500 dark:text-slate-400">
      <span class="text-base">☰</span><span>All 12</span>
    </button>
  </nav>

  <!-- ==================== APPLICATION JAVASCRIPT ==================== -->
  <script>
    /* =========================================================================
       1. CONSTANTS, SYLLABUS DATA & THEME PRESETS
       ========================================================================= */
    const STORAGE_KEY = 'jee_study_os_v1';

    const NAV_ITEMS = [
      { id: 'overview',  label: 'Overview',              icon: '🏠' },
      { id: 'clock',     label: 'Digital Clock',         icon: '🕒' },
      { id: 'timer',     label: 'Study Timer',           icon: '⏱️' },
      { id: 'modes',     label: 'Study Modes',           icon: '🎯' },
      { id: 'syllabus',  label: 'Complete JEE Syllabus', icon: '📚' },
      { id: 'revision',  label: 'Revision Tracker',      icon: '🔁' },
      { id: 'practice',  label: 'Practice & PYQs',       icon: '📝' },
      { id: 'tasks',     label: 'Daily Tasks',           icon: '✅' },
      { id: 'breaks',    label: 'Break Manager',         icon: '☕' },
      { id: 'analytics', label: 'Analytics',             icon: '📊' },
      { id: 'history',   label: 'Session History',       icon: '📜' },
      { id: 'settings',  label: 'Settings',              icon: '⚙️' }
    ];

    const STUDY_MODES = [
      { id: 'New Lecture',       icon: '🎓', desc: 'Concept building & class lectures',   color: '#3b82f6' },
      { id: 'Revision',          icon: '🔁', desc: 'Notes review & active recall',        color: '#8b5cf6' },
      { id: 'Question Practice', icon: '✍️', desc: 'DPPs, coaching modules & books',      color: '#10b981' },
      { id: 'JEE Main PYQ',      icon: '📘', desc: 'Previous year JEE Main papers',       color: '#06b6d4' },
      { id: 'JEE Advanced PYQ',  icon: '🔥', desc: 'Multi-concept IIT-JEE problems',      color: '#f59e0b' },
      { id: 'Mock Test',         icon: '🧪', desc: 'Timed full/part syllabus CBT tests',  color: '#ef4444' },
      { id: 'NCERT Reading',     icon: '📖', desc: 'Line-by-line NCERT mastery',          color: '#14b8a6' },
      { id: 'Formula Revision',  icon: '⚡', desc: 'Short notes & formula sheets',        color: '#eab308' },
      { id: 'Deep Work',         icon: '🧠', desc: 'Zero-distraction high-focus block',   color: '#ec4899' }
    ];

    const ACCENT_PRESETS = {
      blue:    { name: 'Electric Blue', main: '#3b82f6', rgb: '59, 130, 246',  sec: '#8b5cf6', secRgb: '139, 92, 246' },
      purple:  { name: 'Cyber Purple',  main: '#8b5cf6', rgb: '139, 92, 246',  sec: '#ec4899', secRgb: '236, 72, 153' },
      emerald: { name: 'Emerald',       main: '#10b981', rgb: '16, 185, 129',  sec: '#06b6d4', secRgb: '6, 182, 212' },
      amber:   { name: 'Amber Gold',    main: '#f59e0b', rgb: '245, 158, 11',  sec: '#ef4444', secRgb: '239, 68, 68' },
      rose:    { name: 'Neon Rose',     main: '#f43f5e', rgb: '244, 63, 94',   sec: '#8b5cf6', secRgb: '139, 92, 246' },
      cyan:    { name: 'Hyper Cyan',    main: '#06b6d4', rgb: '6, 182, 212',   sec: '#3b82f6', secRgb: '59, 130, 246' }
    };

    const QUOTES = [
      "IIT Bombay is not a destination; it is the result of winning today's 12-hour battle.",
      "Discipline is choosing between what you want now and what you want most — Powai awaits.",
      "Every single JEE Advanced problem you wrestle with rewires your brain for greatness.",
      "Consistency beats intensity. Show up every day, track every hour, conquer every chapter.",
      "Your competition is studying right now. Outwork your yesterday.",
      "NCERT for accuracy, PYQs for pattern, Deep Work for rank under 100.",
      "Pain is temporary. An IIT Bombay degree and the person you become earning it are forever.",
      "Don't count the hours—make every focused minute count.",
      "Master the fundamentals so deeply that tough questions feel elementary.",
      "Success is the sum of small efforts, repeated day in and day out."
    ];

    // Complete Preloaded JEE Main 2027 & JEE Advanced 2027 Syllabus (85 verified chapters)
    function buildPreloadedSyllabus() {
      const raw = [
        // Physics Class 11
        ['Physics', '', '11', 'Units, Dimensions & Measurements', 'Both'],
        ['Physics', '', '11', 'Kinematics (Motion in 1D & 2D)', 'Both'],
        ['Physics', '', '11', 'Laws of Motion & Friction', 'Both'],
        ['Physics', '', '11', 'Work, Energy & Power', 'Both'],
        ['Physics', '', '11', 'Centre of Mass & Collisions', 'Both'],
        ['Physics', '', '11', 'Rotational Motion', 'Both'],
        ['Physics', '', '11', 'Gravitation', 'Both'],
        ['Physics', '', '11', 'Mechanical Properties of Solids (Elasticity)', 'Both'],
        ['Physics', '', '11', 'Mechanical Properties of Fluids', 'Both'],
        ['Physics', '', '11', 'Thermal Properties of Matter & Calorimetry', 'Both'],
        ['Physics', '', '11', 'Kinetic Theory of Gases', 'Both'],
        ['Physics', '', 
