<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EduGuard — Student Dropout Prevention System</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,400&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/chart.umd.min.js"></script>
<style>
:root {
  --bg: #f8f7f4;
  --surface: #ffffff;
  --surface2: #f1efe8;
  --border: #e5e3da;
  --border2: #d3d1c7;
  --text: #1a1a18;
  --muted: #5f5e5a;
  --faint: #888780;
  --accent: #1d9e75;
  --accent-bg: #e1f5ee;
  --accent-text: #085041;
  --danger: #d85a30;
  --danger-bg: #faece7;
  --danger-text: #4a1b0c;
  --warn: #ba7517;
  --warn-bg: #faeeda;
  --warn-text: #412402;
  --blue: #185fa5;
  --blue-bg: #e6f1fb;
  --blue-text: #042c53;
  --sidebar-w: 240px;
  --header-h: 60px;
  --radius: 10px;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'DM Sans', sans-serif; background: var(--bg); color: var(--text); font-size: 14px; display: flex; flex-direction: column; min-height: 100vh; }

/* ── TOPBAR ── */
.topbar {
  height: var(--header-h); background: var(--surface);
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; padding: 0 24px;
  position: fixed; top: 0; left: 0; right: 0; z-index: 100;
  gap: 16px;
}
.logo { display: flex; align-items: center; gap: 10px; }
.logo-icon {
  width: 32px; height: 32px; background: var(--text); border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  font-family: 'Syne', sans-serif; font-weight: 800; font-size: 14px; color: #fff;
}
.logo-name { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 16px; }
.logo-name span { color: var(--accent); }
.topbar-right { margin-left: auto; display: flex; align-items: center; gap: 12px; }
.topbar-badge {
  background: var(--danger-bg); color: var(--danger-text);
  border: 1px solid #f0997b; border-radius: 6px;
  font-size: 12px; padding: 4px 10px; font-weight: 500;
}
.avatar {
  width: 32px; height: 32px; border-radius: 50%;
  background: var(--accent-bg); color: var(--accent-text);
  display: flex; align-items: center; justify-content: center;
  font-weight: 500; font-size: 12px;
}

/* ── SIDEBAR ── */
.sidebar {
  width: var(--sidebar-w); background: var(--surface);
  border-right: 1px solid var(--border);
  position: fixed; top: var(--header-h); bottom: 0; left: 0;
  overflow-y: auto; padding: 16px 0;
  display: flex; flex-direction: column;
}
.nav-group { padding: 0 12px; margin-bottom: 4px; }
.nav-group-label {
  font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase;
  color: var(--faint); font-family: 'DM Mono', monospace;
  padding: 8px 8px 4px;
}
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 8px 12px; border-radius: 8px; cursor: pointer;
  font-size: 13.5px; color: var(--muted); font-weight: 400;
  transition: background 0.15s, color 0.15s; margin-bottom: 2px;
}
.nav-item:hover { background: var(--surface2); color: var(--text); }
.nav-item.active { background: var(--text); color: #fff; }
.nav-item svg { width: 16px; height: 16px; flex-shrink: 0; stroke: currentColor; fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
.nav-badge {
  margin-left: auto; font-size: 11px; background: var(--danger-bg);
  color: var(--danger-text); border-radius: 4px; padding: 1px 6px;
}
.nav-badge.green { background: var(--accent-bg); color: var(--accent-text); }

/* ── MAIN ── */
.main {
  margin-left: var(--sidebar-w); margin-top: var(--header-h);
  padding: 28px; flex: 1;
}
.page { display: none; }
.page.active { display: block; }
.page-header { margin-bottom: 24px; }
.page-title { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 22px; }
.page-sub { color: var(--muted); font-size: 13px; margin-top: 3px; }

/* ── STAT CARDS ── */
.stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin-bottom: 28px; }
.stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 18px 20px; }
.stat-label { font-size: 12px; color: var(--muted); font-family: 'DM Mono', monospace; letter-spacing: 0.5px; margin-bottom: 8px; }
.stat-val { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 28px; line-height: 1; }
.stat-trend { font-size: 12px; margin-top: 6px; }
.stat-card.danger .stat-val { color: var(--danger); }
.stat-card.warn .stat-val { color: var(--warn); }
.stat-card.green .stat-val { color: var(--accent); }
.stat-card.blue .stat-val { color: var(--blue); }

/* ── CONTENT GRID ── */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-60-40 { display: grid; grid-template-columns: 1.5fr 1fr; gap: 20px; margin-bottom: 20px; }
.full { grid-column: 1 / -1; }

/* ── CARD ── */
.card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; }
.card-title { font-family: 'Syne', sans-serif; font-weight: 600; font-size: 14px; margin-bottom: 16px; display: flex; align-items: center; justify-content: space-between; }
.card-title span { color: var(--muted); font-size: 12px; font-weight: 400; font-family: 'DM Sans', sans-serif; }

/* ── TABLE ── */
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; font-size: 13px; }
th { text-align: left; font-size: 11px; color: var(--muted); font-family: 'DM Mono', monospace; letter-spacing: 0.5px; padding: 8px 10px; border-bottom: 1px solid var(--border); font-weight: 400; }
td { padding: 11px 10px; border-bottom: 1px solid var(--border); vertical-align: middle; }
tr:last-child td { border-bottom: none; }
tr:hover td { background: var(--bg); }

/* ── BADGES ── */
.badge { display: inline-flex; align-items: center; gap: 4px; font-size: 11px; font-weight: 500; border-radius: 5px; padding: 3px 8px; }
.badge-high { background: var(--danger-bg); color: var(--danger-text); border: 1px solid #f0997b; }
.badge-medium { background: var(--warn-bg); color: var(--warn-text); border: 1px solid #fac775; }
.badge-low { background: var(--accent-bg); color: var(--accent-text); border: 1px solid #5dcaa5; }
.badge-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }

/* ── BUTTONS ── */
.btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 16px; border-radius: 8px; font-size: 13px; font-weight: 500;
  cursor: pointer; border: 1px solid var(--border); background: var(--surface);
  color: var(--text); transition: all 0.15s; font-family: 'DM Sans', sans-serif;
}
.btn:hover { background: var(--surface2); border-color: var(--border2); }
.btn-primary { background: var(--text); color: #fff; border-color: var(--text); }
.btn-primary:hover { background: #2c2c2a; border-color: #2c2c2a; }
.btn-danger { background: var(--danger-bg); color: var(--danger-text); border-color: #f0997b; }
.btn-sm { padding: 5px 12px; font-size: 12px; }
.btn svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }

/* ── FORM ── */
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.form-group { display: flex; flex-direction: column; gap: 6px; }
.form-group.full { grid-column: 1 / -1; }
label { font-size: 12px; color: var(--muted); font-family: 'DM Mono', monospace; }
input, select, textarea {
  padding: 9px 12px; border: 1px solid var(--border); border-radius: 8px;
  font-size: 13px; font-family: 'DM Sans', sans-serif; background: var(--surface);
  color: var(--text); outline: none; transition: border-color 0.15s;
}
input:focus, select:focus, textarea:focus { border-color: var(--text); }
input[type="range"] { padding: 0; height: 4px; cursor: pointer; accent-color: var(--text); }

/* ── RISK METER ── */
.risk-meter { margin: 16px 0; }
.risk-bar-wrap { background: var(--surface2); border-radius: 99px; height: 10px; overflow: hidden; }
.risk-bar { height: 10px; border-radius: 99px; transition: width 0.5s ease; }
.risk-score-display { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 48px; text-align: center; margin: 16px 0 4px; }
.risk-category { text-align: center; font-size: 14px; font-weight: 500; margin-bottom: 16px; }

/* ── FACTOR BARS ── */
.factor-row { display: flex; align-items: center; gap: 10px; margin-bottom: 8px; }
.factor-label { font-size: 12px; color: var(--muted); width: 120px; flex-shrink: 0; }
.factor-bar-wrap { flex: 1; background: var(--surface2); border-radius: 99px; height: 6px; }
.factor-bar { height: 6px; border-radius: 99px; }
.factor-pct { font-family: 'DM Mono', monospace; font-size: 11px; color: var(--muted); width: 32px; text-align: right; }

/* ── ALERT ITEMS ── */
.alert-item {
  display: flex; align-items: flex-start; gap: 12px;
  padding: 12px; border-radius: 8px; margin-bottom: 8px;
}
.alert-item.high { background: var(--danger-bg); border: 1px solid #f0997b; }
.alert-item.medium { background: var(--warn-bg); border: 1px solid #fac775; }
.alert-icon { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 14px; font-weight: 700; }
.alert-item.high .alert-icon { background: var(--danger); color: #fff; }
.alert-item.medium .alert-icon { background: var(--warn); color: #fff; }
.alert-body { flex: 1; }
.alert-name { font-weight: 500; font-size: 13px; }
.alert-reason { font-size: 12px; color: var(--muted); margin-top: 2px; }

/* ── INTERVENTIONS ── */
.intervention-card {
  border: 1px solid var(--border); border-radius: 8px; padding: 14px;
  margin-bottom: 10px; display: flex; gap: 12px; align-items: flex-start;
}
.int-icon { width: 36px; height: 36px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }
.int-title { font-weight: 500; font-size: 13.5px; }
.int-desc { font-size: 12px; color: var(--muted); margin-top: 2px; }
.int-status { margin-left: auto; }

/* ── MODAL ── */
.modal-overlay {
  position: fixed; inset: 0; background: rgba(0,0,0,0.45); z-index: 200;
  display: flex; align-items: center; justify-content: center; opacity: 0; pointer-events: none; transition: opacity 0.2s;
}
.modal-overlay.open { opacity: 1; pointer-events: all; }
.modal { background: var(--surface); border-radius: 14px; padding: 28px; width: 520px; max-width: 95vw; max-height: 90vh; overflow-y: auto; }
.modal-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 20px; }
.modal-title { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 18px; }
.modal-close { background: none; border: none; cursor: pointer; font-size: 22px; color: var(--muted); line-height: 1; }
.modal-footer { margin-top: 20px; display: flex; gap: 10px; justify-content: flex-end; }

/* ── CHART CONTAINER ── */
.chart-box { position: relative; height: 200px; }
.chart-box.tall { height: 260px; }

/* ── SEARCH ── */
.search-row { display: flex; gap: 10px; margin-bottom: 16px; align-items: center; flex-wrap: wrap; }
.search-box { flex: 1; min-width: 160px; display: flex; align-items: center; gap: 8px; border: 1px solid var(--border); border-radius: 8px; padding: 8px 12px; background: var(--surface); }
.search-box svg { width: 14px; height: 14px; stroke: var(--muted); fill: none; stroke-width: 2; flex-shrink: 0; }
.search-box input { border: none; background: none; outline: none; font-size: 13px; color: var(--text); width: 100%; }

/* ── PROGRESS RING ── */
.prog-ring-wrap { display: flex; align-items: center; justify-content: center; gap: 24px; }

/* TABS */
.tabs { display: flex; gap: 4px; border-bottom: 1px solid var(--border); margin-bottom: 20px; }
.tab { padding: 8px 14px; font-size: 13px; cursor: pointer; color: var(--muted); border-bottom: 2px solid transparent; margin-bottom: -1px; transition: color 0.15s; }
.tab:hover { color: var(--text); }
.tab.active { color: var(--text); border-bottom-color: var(--text); font-weight: 500; }
.tab-content { display: none; }
.tab-content.active { display: block; }
</style>
</head>
<body>

<!-- TOPBAR -->
<header class="topbar">
  <div class="logo">
    <div class="logo-icon">EG</div>
    <div class="logo-name">Edu<span>Guard</span></div>
  </div>
  <div style="flex:1;"></div>
  <div id="topAlert" class="topbar-badge" style="cursor:pointer" onclick="showPage('alerts')">⚠ 4 High-Risk Students Need Attention</div>
  <div class="avatar">AD</div>
</header>

<!-- SIDEBAR -->
<aside class="sidebar">
  <div class="nav-group">
    <div class="nav-group-label">Overview</div>
    <div class="nav-item active" onclick="showPage('dashboard', this)">
      <svg viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
      Dashboard
    </div>
    <div class="nav-item" onclick="showPage('alerts', this)">
      <svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>
      Alerts
      <span class="nav-badge">4</span>
    </div>
  </div>
  <div class="nav-group">
    <div class="nav-group-label">Students</div>
    <div class="nav-item" onclick="showPage('students', this)">
      <svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>
      All Students
      <span class="nav-badge green">124</span>
    </div>
    <div class="nav-item" onclick="showPage('predict', this)">
      <svg viewBox="0 0 24 24"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
      Risk Predictor
    </div>
    <div class="nav-item" onclick="showPage('add', this)">
      <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/></svg>
      Add Student
    </div>
  </div>
  <div class="nav-group">
    <div class="nav-group-label">Analytics</div>
    <div class="nav-item" onclick="showPage('analytics', this)">
      <svg viewBox="0 0 24 24"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
      Reports
    </div>
    <div class="nav-item" onclick="showPage('interventions', this)">
      <svg viewBox="0 0 24 24"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
      Interventions
    </div>
  </div>
</aside>

<!-- MAIN CONTENT -->
<main class="main">

  <!-- ── DASHBOARD ── -->
  <div id="dashboard" class="page active">
    <div class="page-header">
      <div class="page-title">Dashboard</div>
      <div class="page-sub">Academic Year 2025–26 · College of Engineering, Akola</div>
    </div>

    <div class="stats-grid">
      <div class="stat-card blue">
        <div class="stat-label">TOTAL STUDENTS</div>
        <div class="stat-val">124</div>
        <div class="stat-trend" style="color:var(--accent)">↑ 8 enrolled this month</div>
      </div>
      <div class="stat-card danger">
        <div class="stat-label">HIGH RISK</div>
        <div class="stat-val">18</div>
        <div class="stat-trend" style="color:var(--danger)">↑ 3 from last month</div>
      </div>
      <div class="stat-card warn">
        <div class="stat-label">MEDIUM RISK</div>
        <div class="stat-val">34</div>
        <div class="stat-trend" style="color:var(--warn)">↓ 2 improved</div>
      </div>
      <div class="stat-card green">
        <div class="stat-label">LOW RISK</div>
        <div class="stat-val">72</div>
        <div class="stat-trend" style="color:var(--accent)">↑ 5 from last month</div>
      </div>
    </div>

    <div class="grid-60-40">
      <div class="card">
        <div class="card-title">Risk Trend — Last 6 Months <span>Monthly snapshot</span></div>
        <div class="chart-box tall"><canvas id="trendChart"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Risk Distribution <span>Current</span></div>
        <div class="chart-box tall" style="display:flex;align-items:center;justify-content:center;">
          <canvas id="donutChart" style="max-width:200px;max-height:200px;"></canvas>
        </div>
      </div>
    </div>

    <div class="grid-2">
      <div class="card">
        <div class="card-title">High Risk Students — Immediate Attention</div>
        <div id="highRiskList"></div>
      </div>
      <div class="card">
        <div class="card-title">Risk by Department <span>All departments</span></div>
        <div class="chart-box tall"><canvas id="deptChart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- ── ALERTS ── -->
  <div id="alerts" class="page">
    <div class="page-header">
      <div class="page-title">Alerts & Notifications</div>
      <div class="page-sub">Students requiring immediate educator intervention</div>
    </div>

    <div class="tabs">
      <div class="tab active" onclick="switchTab('alertTabs', 'alertAll', this)">All Alerts (8)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertHigh', this)">High Risk (4)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertMed', this)">Medium Risk (3)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertAction', this)">Action Taken (1)</div>
    </div>

    <div id="alertTabs">
      <div id="alertAll" class="tab-content active">
        <div id="alertListAll"></div>
      </div>
      <div id="alertHigh" class="tab-content">
        <div id="alertListHigh"></div>
      </div>
      <div id="alertMed" class="tab-content">
        <div id="alertListMed"></div>
      </div>
      <div id="alertAction" class="tab-content">
        <div class="alert-item medium">
          <div class="alert-icon">✓</div>
          <div class="alert-body">
            <div class="alert-name">Priya Sharma — Counselling Scheduled</div>
            <div class="alert-reason">Academic support session booked for 3rd June. Risk reduced from High → Medium after attendance improvement.</div>
          </div>
          <span class="badge badge-medium">Resolved</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ── STUDENTS ── -->
  <div id="students" class="page">
    <div class="page-header" style="display:flex;align-items:flex-start;justify-content:space-between;flex-wrap:wrap;gap:12px;">
      <div>
        <div class="page-title">All Students</div>
        <div class="page-sub">124 students enrolled · 2025–26</div>
      </div>
      <button class="btn btn-primary" onclick="showPage('add', document.querySelector('.nav-item:nth-child(3)'))">
        <svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
        Add Student
      </button>
    </div>

    <div class="search-row">
      <div class="search-box">
        <svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        <input type="text" placeholder="Search by name, ID, department..." oninput="filterStudents(this.value)" id="studentSearch">
      </div>
      <select onchange="filterStudents()" id="filterRisk" style="width:140px">
        <option value="">All Risk Levels</option>
        <option value="High">High Risk</option>
        <option value="Medium">Medium Risk</option>
        <option value="Low">Low Risk</option>
      </select>
      <select onchange="filterStudents()" id="filterDept" style="width:140px">
        <option value="">All Departments</option>
        <option>Computer Science</option>
        <option>Electronics</option>
        <option>Mechanical</option>
        <option>Civil</option>
      </select>
    </div>

    <div class="card" style="padding:0;">
      <div class="table-wrap">
        <table id="studentTable">
          <thead>
            <tr>
              <th>STUDENT</th><th>DEPT</th><th>ATTENDANCE</th>
              <th>ACADEMIC</th><th>BEHAVIOR</th><th>RISK</th><th>ACTION</th>
            </tr>
          </thead>
          <tbody id="studentTbody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- ── PREDICT ── -->
  <div id="predict" class="page">
    <div class="page-header">
      <div class="page-title">Risk Predictor</div>
      <div class="page-sub">Enter student data to generate an AI-powered dropout risk assessment</div>
    </div>

    <div class="grid-60-40">
      <div class="card">
        <div class="card-title">Student Input Parameters</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;">
          <div>
            <div class="form-group" style="margin-bottom:16px">
              <label>STUDENT NAME</label>
              <input type="text" id="pName" placeholder="Enter name">
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ATTENDANCE RATE</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="72" id="pAttend" oninput="updatePredict()" style="flex:1;">
              <span id="pAttendVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">72%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ACADEMIC SCORE (avg %)</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="58" id="pScore" oninput="updatePredict()" style="flex:1;">
              <span id="pScoreVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">58%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">CLASS PARTICIPATION</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="45" id="pPartic" oninput="updatePredict()" style="flex:1;">
              <span id="pParticVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">45%</span>
            </div>
          </div>

          <div>
            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ASSIGNMENTS SUBMITTED</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="60" id="pAssign" oninput="updatePredict()" style="flex:1;">
              <span id="pAssignVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">60%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">BEHAVIORAL SCORE</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="65" id="pBehav" oninput="updatePredict()" style="flex:1;">
              <span id="pBehavVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">65%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">FINANCIAL AID STATUS</label>
            <select id="pFinancial" onchange="updatePredict()" style="width:100%;margin-bottom:16px;">
              <option value="0">No Financial Aid</option>
              <option value="1" selected>Partial Aid</option>
              <option value="2">Full Scholarship</option>
            </select>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">FAMILY SUPPORT</label>
            <select id="pFamily" onchange="updatePredict()" style="width:100%;margin-bottom:16px;">
              <option value="0">Low</option>
              <option value="1" selected>Medium</option>
              <option value="2">High</option>
            </select>
          </div>
        </div>

        <div style="display:flex;gap:10px;margin-top:8px;">
          <button class="btn btn-primary" onclick="runPredict()">Run Prediction</button>
          <button class="btn" onclick="resetPredict()">Reset</button>
        </div>
      </div>

      <div class="card" id="resultCard">
        <div class="card-title">Risk Assessment Result</div>
        <div class="risk-score-display" id="riskScore">—</div>
        <div class="risk-category" id="riskCategory" style="color:var(--muted)">Run prediction to see result</div>
        <div class="risk-meter">
          <div class="risk-bar-wrap"><div class="risk-bar" id="riskBarEl" style="width:0%;background:var(--muted)"></div></div>
        </div>
        <div style="margin-top:16px;">
          <div style="font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:10px;">FACTOR BREAKDOWN</div>
          <div class="factor-row"><div class="factor-label">Attendance</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb1" style="width:72%;background:var(--warn)"></div></div><div class="factor-pct" id="fp1">72%</div></div>
          <div class="factor-row"><div class="factor-label">Academic Score</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb2" style="width:58%;background:var(--danger)"></div></div><div class="factor-pct" id="fp2">58%</div></div>
          <div class="factor-row"><div class="factor-label">Participation</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb3" style="width:45%;background:var(--danger)"></div></div><div class="factor-pct" id="fp3">45%</div></div>
          <div class="factor-row"><div class="factor-label">Assignments</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb4" style="width:60%;background:var(--warn)"></div></div><div class="factor-pct" id="fp4">60%</div></div>
          <div class="factor-row"><div class="factor-label">Behavior</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb5" style="width:65%;background:var(--warn)"></div></div><div class="factor-pct" id="fp5">65%</div></div>
        </div>
        <div id="riskRecommendations" style="margin-top:16px;display:none;">
          <div style="font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:8px;">RECOMMENDED ACTIONS</div>
          <div id="recList"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ── ADD STUDENT ── -->
  <div id="add" class="page">
    <div class="page-header">
      <div class="page-title">Add New Student</div>
      <div class="page-sub">Enter all relevant information to create a student profile</div>
    </div>
    <div class="card" style="max-width:680px;">
      <div class="form-grid">
        <div class="form-group">
          <label>FULL NAME *</label>
          <input type="text" id="addName" placeholder="e.g. Rahul Patil">
        </div>
        <div class="form-group">
          <label>STUDENT ID *</label>
          <input type="text" id="addId" placeholder="e.g. CSE2026001">
        </div>
        <div class="form-group">
          <label>DEPARTMENT *</label>
          <select id="addDept">
            <option>Computer Science</option>
            <option>Electronics</option>
            <option>Mechanical</option>
            <option>Civil</option>
          </select>
        </div>
        <div class="form-group">
          <label>YEAR / SEMESTER</label>
          <select id="addYear">
            <option>First Year (Sem 1)</option>
            <option>First Year (Sem 2)</option>
            <option>Second Year (Sem 3)</option>
            <option selected>Second Year (Sem 4)</option>
            <option>Third Year (Sem 5)</option>
            <option>Third Year (Sem 6)</option>
            <option>Final Year (Sem 7)</option>
            <option>Final Year (Sem 8)</option>
          </select>
        </div>
        <div class="form-group">
          <label>EMAIL</label>
          <input type="email" id="addEmail" placeholder="student@college.edu">
        </div>
        <div class="form-group">
          <label>PHONE</label>
          <input type="tel" id="addPhone" placeholder="+91 XXXXX XXXXX">
        </div>

        <div class="form-group full" style="border-top:1px solid var(--border);padding-top:16px;margin-top:4px;">
          <div style="font-size:13px;font-weight:500;margin-bottom:12px;">Academic & Behavioral Data</div>
        </div>

        <div class="form-group">
          <label>ATTENDANCE RATE (%)</label>
          <input type="number" id="addAttend" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>AVERAGE ACADEMIC SCORE (%)</label>
          <input type="number" id="addScore" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>CLASS PARTICIPATION (%)</label>
          <input type="number" id="addPartic" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>ASSIGNMENTS SUBMITTED (%)</label>
          <input type="number" id="addAssign" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>BEHAVIORAL SCORE (%)</label>
          <input type="number" id="addBehav" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>FINANCIAL AID</label>
          <select id="addFinancial">
            <option value="0">None</option>
            <option value="1">Partial</option>
            <option value="2">Full Scholarship</option>
          </select>
        </div>
        <div class="form-group">
          <label>FAMILY SUPPORT</label>
          <select id="addFamily">
            <option value="0">Low</option>
            <option value="1">Medium</option>
            <option value="2">High</option>
          </select>
        </div>
        <div class="form-group full">
          <label>NOTES / REMARKS</label>
          <textarea id="addNotes" rows="3" placeholder="Any observations, previous counselling notes..."></textarea>
        </div>
        <div class="form-group full" style="flex-direction:row;gap:10px;">
          <button class="btn btn-primary" onclick="addStudent()">
            <svg viewBox="0 0 24 24"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
            Save & Predict Risk
          </button>
          <button class="btn" onclick="clearAddForm()">Clear</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ── ANALYTICS ── -->
  <div id="analytics" class="page">
    <div class="page-header">
      <div class="page-title">Reports & Analytics</div>
      <div class="page-sub">Institutional dropout risk analysis and trends</div>
    </div>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-label">DROPOUT RATE (2024)</div>
        <div class="stat-val" style="color:var(--danger)">9.2%</div>
        <div class="stat-trend" style="color:var(--accent)">↓ 2.1% from 2023</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">INTERVENTIONS DONE</div>
        <div class="stat-val" style="color:var(--blue)">47</div>
        <div class="stat-trend" style="color:var(--muted)">This semester</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">SUCCESS RATE</div>
        <div class="stat-val" style="color:var(--accent)">78%</div>
        <div class="stat-trend" style="color:var(--accent)">Of interventions prevented dropout</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">AVG RISK SCORE</div>
        <div class="stat-val" style="color:var(--warn)">41</div>
        <div class="stat-trend" style="color:var(--accent)">↓ 4 pts from last semester</div>
      </div>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-title">Monthly Dropout Risk Trend <span>2025</span></div>
        <div class="chart-box tall"><canvas id="monthlyChart"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Risk Factors Impact <span>Weighted analysis</span></div>
        <div class="chart-box tall"><canvas id="radarChart"></canvas></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">Year-wise Risk Distribution</div>
      <div class="chart-box" style="height:180px;"><canvas id="yearChart"></canvas></div>
    </div>
  </div>

  <!-- ── INTERVENTIONS ── -->
  <div id="interventions" class="page">
    <div class="page-header">
      <div class="page-title">Interventions</div>
      <div class="page-sub">Preventive actions assigned to at-risk students</div>
    </div>
    <div class="card">
      <div class="card-title">Active Intervention Plans</div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--danger-bg);">🎓</div>
        <div>
          <div class="int-title">Academic Support — Arjun Deshmukh</div>
          <div class="int-desc">Weekly tutoring sessions with faculty mentor. Focus: Mathematics & Data Structures. Started 20 May.</div>
        </div>
        <div class="int-status"><span class="badge badge-high">High Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--warn-bg);">🧠</div>
        <div>
          <div class="int-title">Counselling — Sneha Kulkarni</div>
          <div class="int-desc">Bi-weekly counselling to address family-related stress. Sessions with Dr. Meera Joshi.</div>
        </div>
        <div class="int-status"><span class="badge badge-medium">Medium Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--blue-bg);">💰</div>
        <div>
          <div class="int-title">Financial Aid Review — Ravi Thakur</div>
          <div class="int-desc">Scholarship application submitted. Awaiting approval from University. Emergency fund approved for this semester.</div>
        </div>
        <div class="int-status"><span class="badge badge-high">High Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--accent-bg);">👥</div>
        <div>
          <div class="int-title">Peer Mentoring — 8 Students</div>
          <div class="int-desc">Medium-risk students paired with senior student mentors. Monthly check-ins scheduled.</div>
        </div>
        <div class="int-status"><span class="badge badge-medium">Ongoing</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--accent-bg);">📞</div>
        <div>
          <div class="int-title">Parent Communication — 6 Students</div>
          <div class="int-desc">Letters sent to parents of high-risk students. Phone follow-ups scheduled for next week.</div>
        </div>
        <div class="int-status"><span class="badge badge-low">Done</span></div>
      </div>
    </div>

    <div class="card" style="margin-top:20px;">
      <div class="card-title">Log New Intervention</div>
      <div class="form-grid">
        <div class="form-group">
          <label>STUDENT NAME</label>
          <input type="text" placeholder="Search student...">
        </div>
        <div class="form-group">
          <label>INTERVENTION TYPE</label>
          <select>
            <option>Academic Support</option>
            <option>Counselling</option>
            <option>Financial Aid</option>
            <option>Peer Mentoring</option>
            <option>Parent Communication</option>
            <option>Medical Referral</option>
          </select>
        </div>
        <div class="form-group">
          <label>ASSIGNED TO</label>
          <input type="text" placeholder="Faculty / Counsellor name">
        </div>
        <div class="form-group">
          <label>START DATE</label>
          <input type="date">
        </div>
        <div class="form-group full">
          <label>NOTES</label>
          <textarea rows="2" placeholder="Details of the intervention plan..."></textarea>
        </div>
        <div class="form-group full">
          <button class="btn btn-primary" style="width:fit-content">Log Intervention</button>
        </div>
      </div>
    </div>
  </div>

</main>

<!-- STUDENT DETAIL MODAL -->
<div class="modal-overlay" id="studentModal">
  <div class="modal">
    <div class="modal-header">
      <div class="modal-title" id="modalStudentName">Student Details</div>
      <button class="modal-close" onclick="closeModal()">×</button>
    </div>
    <div id="modalContent"></div>
    <div class="modal-footer">
      <button class="btn" onclick="closeModal()">Close</button>
      <button class="btn btn-primary" onclick="closeModal()">Log Intervention</button>
    </div>
  </div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────────────────────
let students = [
  {id:'CSE001', name:'Arjun Deshmukh', dept:'Computer Science', attend:52, score:41, partic:30, assign:45, behav:50, financial:0, family:0},
  {id:'CSE002', name:'Priya Sharma', dept:'Computer Science', attend:61, score:55, partic:40, assign:58, behav:62, financial:1, family:1},
  {id:'ELE001', name:'Ravi Thakur', dept:'Electronics', attend:48, score:38, partic:25, assign:40, behav:45, financial:0, family:0},
  {id:'MEC001', name:'Sneha Kulkarni', dept:'Mechanical', attend:69, score:58, partic:50, assign:62, behav:55, financial:1, family:0},
  {id:'CSE003', name:'Karan Mehta', dept:'Computer Science', attend:75, score:68, partic:60, assign:70, behav:72, financial:1, family:1},
  {id:'ELE002', name:'Ananya Joshi', dept:'Electronics', attend:82, score:74, partic:70, assign:78, behav:80, financial:2, family:2},
  {id:'CIV001', name:'Rohan Patil', dept:'Civil', attend:55, score:48, partic:35, assign:50, behav:58, financial:0, family:1},
  {id:'MEC002', name:'Dipika Rao', dept:'Mechanical', attend:88, score:82, partic:80, assign:85, behav:88, financial:2, family:2},
  {id:'CSE004', name:'Vikram Soni', dept:'Computer Science', attend:90, score:86, partic:85, assign:88, behav:90, financial:1, family:2},
  {id:'ELE003', name:'Meera Nair', dept:'Electronics', attend:45, score:35, partic:22, assign:38, behav:42, financial:0, family:0},
  {id:'CIV002', name:'Aditya Kumar', dept:'Civil', attend:78, score:71, partic:65, assign:72, behav:75, financial:1, family:1},
  {id:'MEC003', name:'Pooja Singh', dept:'Mechanical', attend:65, score:60, partic:55, assign:63, behav:68, financial:1, family:1},
];

function calcRisk(s) {
  const w = {attend:0.28, score:0.25, partic:0.15, assign:0.15, behav:0.10, financial:0.04, family:0.03};
  const finScore = [40,65,85][+s.financial] || 65;
  const famScore = [40,65,85][+s.family] || 65;
  const raw = (s.attend*w.attend + s.score*w.score + s.partic*w.partic +
               s.assign*w.assign + s.behav*w.behav + finScore*w.financial + famScore*w.family);
  const riskScore = Math.round(100 - raw);
  return riskScore;
}
function riskLevel(score) {
  if (score >= 60) return 'High';
  if (score >= 35) return 'Medium';
  return 'Low';
}
function riskColor(level) {
  return level === 'High' ? 'var(--danger)' : level === 'Medium' ? 'var(--warn)' : 'var(--accent)';
}
function badgeClass(level) {
  return level === 'High' ? 'badge-high' : level === 'Medium' ? 'badge-medium' : 'badge-low';
}

// ── NAVIGATION ──────────────────────────────────────────────────────────
function showPage(id, el) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if (el) el.classList.add('active');
  if (id === 'students') renderStudents();
  if (id === 'analytics') setTimeout(renderAnalyticsCharts, 100);
  if (id === 'dashboard') setTimeout(renderDashboardCharts, 50);
}
function switchTab(groupId, tabId, btn) {
  const group = document.getElementById(groupId);
  group.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.getElementById(tabId).classList.add('active');
  const tabBar = btn.parentElement;
  tabBar.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
}

// ── STUDENTS ────────────────────────────────────────────────────────────
function renderStudents(data) {
  const tbody = document.getElementById('studentTbody');
  const list = data || students;
  tbody.innerHTML = list.map(s => {
    const rs = calcRisk(s);
    const rl = riskLevel(rs);
    return `<tr>
      <td><div style="font-weight:500">${s.name}</div><div style="font-size:11px;color:var(--muted);font-family:'DM Mono',monospace">${s.id}</div></td>
      <td style="color:var(--muted)">${s.dept}</td>
      <td>
        <div style="display:flex;align-items:center;gap:8px">
          <div style="width:50px;background:var(--surface2);border-radius:99px;height:5px">
            <div style="width:${s.attend}%;height:5px;border-radius:99px;background:${s.attend<60?'var(--danger)':s.attend<75?'var(--warn)':'var(--accent)'}"></div>
          </div>
          <span style="font-family:'DM Mono',monospace;font-size:12px">${s.attend}%</span>
        </div>
      </td>
      <td><span style="font-family:'DM Mono',monospace;font-size:12px;font-weight:500;color:${s.score<50?'var(--danger)':s.score<65?'var(--warn)':'var(--accent)'}">${s.score}%</span></td>
      <td><span style="font-family:'DM Mono',monospace;font-size:12px">${s.behav}%</span></td>
      <td><span class="badge ${badgeClass(rl)}">${rl} (${rs})</span></td>
      <td><button class="btn btn-sm" onclick="viewStudent('${s.id}')">View</button></td>
    </tr>`;
  }).join('');
}
function filterStudents(val) {
  const search = (document.getElementById('studentSearch').value||'').toLowerCase();
  const risk = document.getElementById('filterRisk').value;
  const dept = document.getElementById('filterDept').value;
  const filtered = students.filter(s => {
    const rl = riskLevel(calcRisk(s));
    return (!search || s.name.toLowerCase().includes(search) || s.id.toLowerCase().includes(search) || s.dept.toLowerCase().includes(search))
      && (!risk || rl === risk)
      && (!dept || s.dept === dept);
  });
  renderStudents(filtered);
}
function viewStudent(id) {
  const s = students.find(x => x.id === id);
  if (!s) return;
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  document.getElementById('modalStudentName').textContent = s.name;
  const finLabel = ['None','Partial','Full Scholarship'][+s.financial];
  const famLabel = ['Low','Medium','High'][+s.family];
  document.getElementById('modalContent').innerHTML = `
    <div style="display:flex;align-items:center;gap:16px;margin-bottom:20px;">
      <div style="width:56px;height:56px;border-radius:50%;background:var(--surface2);display:flex;align-items:center;justify-content:center;font-family:'Syne',sans-serif;font-weight:700;font-size:20px">${s.name.split(' ').map(n=>n[0]).join('')}</div>
      <div>
        <div style="font-weight:600;font-size:16px">${s.name}</div>
        <div style="color:var(--muted);font-size:13px">${s.id} · ${s.dept}</div>
      </div>
      <span class="badge ${badgeClass(rl)}" style="margin-left:auto;font-size:13px;padding:5px 12px">${rl} Risk — ${rs}/100</span>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px;">
      ${[['Attendance',s.attend+'%'],['Academic Score',s.score+'%'],['Participation',s.partic+'%'],['Assignments',s.assign+'%'],['Behavior',s.behav+'%'],['Financial Aid',finLabel],['Family Support',famLabel]].map(([k,v])=>`
        <div style="background:var(--bg);border-radius:8px;padding:10px 14px;">
          <div style="font-size:11px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:4px">${k.toUpperCase()}</div>
          <div style="font-weight:500;font-size:14px">${v}</div>
        </div>`).join('')}
    </div>
    <div style="background:${rl==='High'?'var(--danger-bg)':rl==='Medium'?'var(--warn-bg)':'var(--accent-bg)'};border-radius:8px;padding:14px;">
      <div style="font-weight:500;margin-bottom:4px">Recommended Action</div>
      <div style="font-size:13px;color:var(--muted)">${getRecommendation(rs, s)}</div>
    </div>`;
  document.getElementById('studentModal').classList.add('open');
}
function closeModal() { document.getElementById('studentModal').classList.remove('open'); }

// ── RISK PREDICTOR ────────────────────────────────────────────────────
function updatePredict() {
  ['Attend','Score','Partic','Assign','Behav'].forEach(k => {
    const v = document.getElementById('p'+k).value;
    document.getElementById('p'+k+'Val').textContent = v + '%';
    document.getElementById('fb'+((['Attend','Score','Partic','Assign','Behav'].indexOf(k)+1))).style.width = v+'%';
    document.getElementById('fp'+((['Attend','Score','Partic','Assign','Behav'].indexOf(k)+1))).textContent = v+'%';
  });
  ['fb1','fb2','fb3','fb4','fb5'].forEach((id,i) => {
    const vals = [+document.getElementById('pAttend').value, +document.getElementById('pScore').value,
      +document.getElementById('pPartic').value, +document.getElementById('pAssign').value, +document.getElementById('pBehav').value];
    const v = vals[i];
    document.getElementById(id).style.background = v<50?'var(--danger)':v<70?'var(--warn)':'var(--accent)';
  });
}
function runPredict() {
  const s = {
    attend: +document.getElementById('pAttend').value,
    score: +document.getElementById('pScore').value,
    partic: +document.getElementById('pPartic').value,
    assign: +document.getElementById('pAssign').value,
    behav: +document.getElementById('pBehav').value,
    financial: document.getElementById('pFinancial').value,
    family: document.getElementById('pFamily').value,
  };
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  document.getElementById('riskScore').textContent = rs;
  document.getElementById('riskScore').style.color = riskColor(rl);
  document.getElementById('riskCategory').textContent = rl + ' Risk';
  document.getElementById('riskCategory').style.color = riskColor(rl);
  document.getElementById('riskBarEl').style.width = rs + '%';
  document.getElementById('riskBarEl').style.background = riskColor(rl);
  const recDiv = document.getElementById('riskRecommendations');
  recDiv.style.display = 'block';
  document.getElementById('recList').innerHTML = getRecommendationList(rs, s).map(r=>`<div style="display:flex;gap:8px;margin-bottom:6px;font-size:13px"><span style="color:${riskColor(rl)}">→</span><span>${r}</span></div>`).join('');
}
function resetPredict() {
  ['pAttend','pScore','pPartic','pAssign','pBehav'].forEach(id => {
    document.getElementById(id).value = 70;
  });
  document.getElementById('pFinancial').value = 1;
  document.getElementById('pFamily').value = 1;
  updatePredict();
  document.getElementById('riskScore').textContent = '—';
  document.getElementById('riskCategory').textContent = 'Run prediction to see result';
  document.getElementById('riskCategory').style.color = 'var(--muted)';
  document.getElementById('riskBarEl').style.width = '0%';
  document.getElementById('riskRecommendations').style.display = 'none';
}
function getRecommendation(rs, s) {
  if (rs >= 60) return 'Immediate counselling and academic support required. Contact parents and assign a faculty mentor. Consider financial aid review.';
  if (rs >= 35) return 'Schedule a check-in with the student. Review attendance and assignment submission. Assign a peer mentor.';
  return 'Student is performing well. Continue monitoring monthly. Encourage participation in extracurricular activities.';
}
function getRecommendationList(rs, s) {
  const recs = [];
  if (s.attend < 60) recs.push('Critical: Attendance below threshold — initiate parent contact immediately');
  if (s.score < 50) recs.push('Academic support sessions with subject faculty recommended');
  if (s.partic < 40) recs.push('Encourage participation through peer group activities and group projects');
  if (s.assign < 55) recs.push('Assignment tracking — weekly check-in with class coordinator');
  if (+s.financial === 0) recs.push('Review eligibility for financial aid or scholarship schemes');
  if (+s.family === 0) recs.push('Family counselling or social support program referral advised');
  if (recs.length === 0) recs.push('No immediate action required — maintain monthly monitoring');
  return recs;
}

// ── ADD STUDENT ──────────────────────────────────────────────────────
function addStudent() {
  const name = document.getElementById('addName').value.trim();
  if (!name) { alert('Please enter student name'); return; }
  const s = {
    id: document.getElementById('addId').value || 'STU' + Date.now(),
    name, dept: document.getElementById('addDept').value,
    attend: +document.getElementById('addAttend').value || 70,
    score: +document.getElementById('addScore').value || 65,
    partic: +document.getElementById('addPartic').value || 60,
    assign: +document.getElementById('addAssign').value || 65,
    behav: +document.getElementById('addBehav').value || 70,
    financial: document.getElementById('addFinancial').value,
    family: document.getElementById('addFamily').value,
  };
  students.unshift(s);
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  alert(`✓ Student "${name}" added successfully!\nRisk Level: ${rl} (${rs}/100)\n\n${getRecommendation(rs, s)}`);
  clearAddForm();
  showPage('students', document.querySelector('.nav-item'));
}
function clearAddForm() {
  ['addName','addId','addEmail','addPhone','addAttend','addScore','addPartic','addAssign','addBehav','addNotes'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
}

// ── DASHBOARD CHARTS ─────────────────────────────────────────────────
let trendChartInst, donutChartInst, deptChartInst, monthlyChartInst, radarChartInst, yearChartInst;
function renderDashboardCharts() {
  // High risk list
  const highRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 60).sort((a,b) => b.rs - a.rs).slice(0, 4);
  document.getElementById('highRiskList').innerHTML = highRisk.map(s => `
    <div class="alert-item high" style="cursor:pointer" onclick="viewStudent('${s.id}')">
      <div class="alert-icon">${s.rs}</div>
      <div class="alert-body">
        <div class="alert-name">${s.name}</div>
        <div class="alert-reason">${s.dept} · Attendance: ${s.attend}% · Score: ${s.score}%</div>
      </div>
      <span class="badge badge-high">High</span>
    </div>`).join('');

  // Trend chart
  if (trendChartInst) trendChartInst.destroy();
  trendChartInst = new Chart(document.getElementById('trendChart'), {
    type: 'line',
    data: {
      labels: ['Jan','Feb','Mar','Apr','May','Jun'],
      datasets: [
        { label: 'High Risk', data: [22,20,19,21,18,18], borderColor:'#d85a30', backgroundColor:'rgba(216,90,48,0.08)', fill:true, tension:0.4 },
        { label: 'Medium Risk', data: [30,32,35,33,36,34], borderColor:'#ba7517', backgroundColor:'rgba(186,117,23,0.08)', fill:true, tension:0.4 },
        { label: 'Low Risk', data: [62,65,66,64,70,72], borderColor:'#1d9e75', backgroundColor:'rgba(29,158,117,0.08)', fill:true, tension:0.4 },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{y:{beginAtZero:true,grid:{color:'rgba(0,0,0,0.05)'}},x:{grid:{display:false}}} }
  });

  // Donut
  if (donutChartInst) donutChartInst.destroy();
  const high = students.filter(s => riskLevel(calcRisk(s)) === 'High').length;
  const med = students.filter(s => riskLevel(calcRisk(s)) === 'Medium').length;
  const low = students.filter(s => riskLevel(calcRisk(s)) === 'Low').length;
  donutChartInst = new Chart(document.getElementById('donutChart'), {
    type: 'doughnut',
    data: { labels: ['High','Medium','Low'], datasets: [{ data:[high,med,low], backgroundColor:['#d85a30','#ba7517','#1d9e75'], borderWidth:0 }] },
    options: { responsive:true, maintainAspectRatio:true, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}} }
  });

  // Dept bar
  if (deptChartInst) deptChartInst.destroy();
  deptChartInst = new Chart(document.getElementById('deptChart'), {
    type: 'bar',
    data: {
      labels: ['Computer Science','Electronics','Mechanical','Civil'],
      datasets: [
        { label:'High', data:[5,6,4,3], backgroundColor:'#d85a30' },
        { label:'Medium', data:[10,9,8,7], backgroundColor:'#ba7517' },
        { label:'Low', data:[20,15,18,19], backgroundColor:'#1d9e75' },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{x:{stacked:true,grid:{display:false}},y:{stacked:true,grid:{color:'rgba(0,0,0,0.05)'}}} }
  });
}

// Alerts
function renderAlerts() {
  const highRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 60).sort((a,b) => b.rs - a.rs);
  const medRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 35 && s.rs < 60).sort((a,b) => b.rs - a.rs).slice(0,3);
  const makeAlert = (s, cls) => `
    <div class="alert-item ${cls}" style="cursor:pointer" onclick="viewStudent('${s.id}')">
      <div class="alert-icon">${s.rs}</div>
      <div class="alert-body">
        <div class="alert-name">${s.name} — ${s.dept}</div>
        <div class="alert-reason">Attendance ${s.attend}% · Score ${s.score}% · Risk Score ${s.rs}/100</div>
      </div>
      <button class="btn btn-sm btn-danger" onclick="event.stopPropagation()">Intervene</button>
    </div>`;
  document.getElementById('alertListAll').innerHTML = [...highRisk.slice(0,4).map(s=>makeAlert(s,'high')), ...medRisk.map(s=>makeAlert(s,'medium'))].join('');
  document.getElementById('alertListHigh').innerHTML = highRisk.slice(0,4).map(s=>makeAlert(s,'high')).join('');
  document.getElementById('alertListMed').innerHTML = medRisk.map(s=>makeAlert(s,'medium')).join('');
}

function renderAnalyticsCharts() {
  if (monthlyChartInst) monthlyChartInst.destroy();
  monthlyChartInst = new Chart(document.getElementById('monthlyChart'), {
    type: 'line',
    data: {
      labels: ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'],
      datasets: [{ label:'Risk Score (avg)', data:[52,50,48,51,46,44,47,42,41,40,39,38], borderColor:'#d85a30', backgroundColor:'rgba(216,90,48,0.07)', fill:true, tension:0.4 }]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{display:false}}, scales:{y:{min:30,max:60,grid:{color:'rgba(0,0,0,0.05)'}},x:{grid:{display:false}}} }
  });

  if (radarChartInst) radarChartInst.destroy();
  radarChartInst = new Chart(document.getElementById('radarChart'), {
    type: 'radar',
    data: {
      labels: ['Attendance','Academic Score','Participation','Assignments','Behavior','Financial','Family'],
      datasets: [{ label:'High Risk Students', data:[38,35,30,40,45,25,30], backgroundColor:'rgba(216,90,48,0.15)', borderColor:'#d85a30', pointBackgroundColor:'#d85a30' }]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom'}}, scales:{r:{min:0,max:100,ticks:{font:{size:10}}}} }
  });

  if (yearChartInst) yearChartInst.destroy();
  yearChartInst = new Chart(document.getElementById('yearChart'), {
    type: 'bar',
    data: {
      labels: ['1st Year','2nd Year','3rd Year','Final Year'],
      datasets: [
        { label:'High', data:[8,5,3,2], backgroundColor:'#d85a30' },
        { label:'Medium', data:[12,10,7,5], backgroundColor:'#ba7517' },
        { label:'Low', data:[10,18,22,22], backgroundColor:'#1d9e75' },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{x:{stacked:true,grid:{display:false}},y:{stacked:true,grid:{color:'rgba(0,0,0,0.05)'}}} }
  });
}

// ── INIT ──────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  renderStudents();
  renderAlerts();
  setTimeout(renderDashboardCharts, 100);
});
document.getElementById('studentModal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});
</script>
</body>
</html>  display: flex; align-items: center; justify-content: center;
  font-weight: 500; font-size: 12px;
}

/* ── SIDEBAR ── */
.sidebar {
  width: var(--sidebar-w); background: var(--surface);
  border-right: 1px solid var(--border);
  position: fixed; top: var(--header-h); bottom: 0; left: 0;
  overflow-y: auto; padding: 16px 0;
  display: flex; flex-direction: column;
}
.nav-group { padding: 0 12px; margin-bottom: 4px; }
.nav-group-label {
  font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase;
  color: var(--faint); font-family: 'DM Mono', monospace;
  padding: 8px 8px 4px;
}
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 8px 12px; border-radius: 8px; cursor: pointer;
  font-size: 13.5px; color: var(--muted); font-weight: 400;
  transition: background 0.15s, color 0.15s; margin-bottom: 2px;
}
.nav-item:hover { background: var(--surface2); color: var(--text); }
.nav-item.active { background: var(--text); color: #fff; }
.nav-item svg { width: 16px; height: 16px; flex-shrink: 0; stroke: currentColor; fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
.nav-badge {
  margin-left: auto; font-size: 11px; background: var(--danger-bg);
  color: var(--danger-text); border-radius: 4px; padding: 1px 6px;
}
.nav-badge.green { background: var(--accent-bg); color: var(--accent-text); }

/* ── MAIN ── */
.main {
  margin-left: var(--sidebar-w); margin-top: var(--header-h);
  padding: 28px; flex: 1;
}
.page { display: none; }
.page.active { display: block; }
.page-header { margin-bottom: 24px; }
.page-title { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 22px; }
.page-sub { color: var(--muted); font-size: 13px; margin-top: 3px; }

/* ── STAT CARDS ── */
.stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 16px; margin-bottom: 28px; }
.stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 18px 20px; }
.stat-label { font-size: 12px; color: var(--muted); font-family: 'DM Mono', monospace; letter-spacing: 0.5px; margin-bottom: 8px; }
.stat-val { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 28px; line-height: 1; }
.stat-trend { font-size: 12px; margin-top: 6px; }
.stat-card.danger .stat-val { color: var(--danger); }
.stat-card.warn .stat-val { color: var(--warn); }
.stat-card.green .stat-val { color: var(--accent); }
.stat-card.blue .stat-val { color: var(--blue); }

/* ── CONTENT GRID ── */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin-bottom: 20px; }
.grid-60-40 { display: grid; grid-template-columns: 1.5fr 1fr; gap: 20px; margin-bottom: 20px; }
.full { grid-column: 1 / -1; }

/* ── CARD ── */
.card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; }
.card-title { font-family: 'Syne', sans-serif; font-weight: 600; font-size: 14px; margin-bottom: 16px; display: flex; align-items: center; justify-content: space-between; }
.card-title span { color: var(--muted); font-size: 12px; font-weight: 400; font-family: 'DM Sans', sans-serif; }

/* ── TABLE ── */
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; font-size: 13px; }
th { text-align: left; font-size: 11px; color: var(--muted); font-family: 'DM Mono', monospace; letter-spacing: 0.5px; padding: 8px 10px; border-bottom: 1px solid var(--border); font-weight: 400; }
td { padding: 11px 10px; border-bottom: 1px solid var(--border); vertical-align: middle; }
tr:last-child td { border-bottom: none; }
tr:hover td { background: var(--bg); }

/* ── BADGES ── */
.badge { display: inline-flex; align-items: center; gap: 4px; font-size: 11px; font-weight: 500; border-radius: 5px; padding: 3px 8px; }
.badge-high { background: var(--danger-bg); color: var(--danger-text); border: 1px solid #f0997b; }
.badge-medium { background: var(--warn-bg); color: var(--warn-text); border: 1px solid #fac775; }
.badge-low { background: var(--accent-bg); color: var(--accent-text); border: 1px solid #5dcaa5; }
.badge-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }

/* ── BUTTONS ── */
.btn {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 16px; border-radius: 8px; font-size: 13px; font-weight: 500;
  cursor: pointer; border: 1px solid var(--border); background: var(--surface);
  color: var(--text); transition: all 0.15s; font-family: 'DM Sans', sans-serif;
}
.btn:hover { background: var(--surface2); border-color: var(--border2); }
.btn-primary { background: var(--text); color: #fff; border-color: var(--text); }
.btn-primary:hover { background: #2c2c2a; border-color: #2c2c2a; }
.btn-danger { background: var(--danger-bg); color: var(--danger-text); border-color: #f0997b; }
.btn-sm { padding: 5px 12px; font-size: 12px; }
.btn svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }

/* ── FORM ── */
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.form-group { display: flex; flex-direction: column; gap: 6px; }
.form-group.full { grid-column: 1 / -1; }
label { font-size: 12px; color: var(--muted); font-family: 'DM Mono', monospace; }
input, select, textarea {
  padding: 9px 12px; border: 1px solid var(--border); border-radius: 8px;
  font-size: 13px; font-family: 'DM Sans', sans-serif; background: var(--surface);
  color: var(--text); outline: none; transition: border-color 0.15s;
}
input:focus, select:focus, textarea:focus { border-color: var(--text); }
input[type="range"] { padding: 0; height: 4px; cursor: pointer; accent-color: var(--text); }

/* ── RISK METER ── */
.risk-meter { margin: 16px 0; }
.risk-bar-wrap { background: var(--surface2); border-radius: 99px; height: 10px; overflow: hidden; }
.risk-bar { height: 10px; border-radius: 99px; transition: width 0.5s ease; }
.risk-score-display { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 48px; text-align: center; margin: 16px 0 4px; }
.risk-category { text-align: center; font-size: 14px; font-weight: 500; margin-bottom: 16px; }

/* ── FACTOR BARS ── */
.factor-row { display: flex; align-items: center; gap: 10px; margin-bottom: 8px; }
.factor-label { font-size: 12px; color: var(--muted); width: 120px; flex-shrink: 0; }
.factor-bar-wrap { flex: 1; background: var(--surface2); border-radius: 99px; height: 6px; }
.factor-bar { height: 6px; border-radius: 99px; }
.factor-pct { font-family: 'DM Mono', monospace; font-size: 11px; color: var(--muted); width: 32px; text-align: right; }

/* ── ALERT ITEMS ── */
.alert-item {
  display: flex; align-items: flex-start; gap: 12px;
  padding: 12px; border-radius: 8px; margin-bottom: 8px;
}
.alert-item.high { background: var(--danger-bg); border: 1px solid #f0997b; }
.alert-item.medium { background: var(--warn-bg); border: 1px solid #fac775; }
.alert-icon { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 14px; font-weight: 700; }
.alert-item.high .alert-icon { background: var(--danger); color: #fff; }
.alert-item.medium .alert-icon { background: var(--warn); color: #fff; }
.alert-body { flex: 1; }
.alert-name { font-weight: 500; font-size: 13px; }
.alert-reason { font-size: 12px; color: var(--muted); margin-top: 2px; }

/* ── INTERVENTIONS ── */
.intervention-card {
  border: 1px solid var(--border); border-radius: 8px; padding: 14px;
  margin-bottom: 10px; display: flex; gap: 12px; align-items: flex-start;
}
.int-icon { width: 36px; height: 36px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }
.int-title { font-weight: 500; font-size: 13.5px; }
.int-desc { font-size: 12px; color: var(--muted); margin-top: 2px; }
.int-status { margin-left: auto; }

/* ── MODAL ── */
.modal-overlay {
  position: fixed; inset: 0; background: rgba(0,0,0,0.45); z-index: 200;
  display: flex; align-items: center; justify-content: center; opacity: 0; pointer-events: none; transition: opacity 0.2s;
}
.modal-overlay.open { opacity: 1; pointer-events: all; }
.modal { background: var(--surface); border-radius: 14px; padding: 28px; width: 520px; max-width: 95vw; max-height: 90vh; overflow-y: auto; }
.modal-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 20px; }
.modal-title { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 18px; }
.modal-close { background: none; border: none; cursor: pointer; font-size: 22px; color: var(--muted); line-height: 1; }
.modal-footer { margin-top: 20px; display: flex; gap: 10px; justify-content: flex-end; }

/* ── CHART CONTAINER ── */
.chart-box { position: relative; height: 200px; }
.chart-box.tall { height: 260px; }

/* ── SEARCH ── */
.search-row { display: flex; gap: 10px; margin-bottom: 16px; align-items: center; flex-wrap: wrap; }
.search-box { flex: 1; min-width: 160px; display: flex; align-items: center; gap: 8px; border: 1px solid var(--border); border-radius: 8px; padding: 8px 12px; background: var(--surface); }
.search-box svg { width: 14px; height: 14px; stroke: var(--muted); fill: none; stroke-width: 2; flex-shrink: 0; }
.search-box input { border: none; background: none; outline: none; font-size: 13px; color: var(--text); width: 100%; }

/* ── PROGRESS RING ── */
.prog-ring-wrap { display: flex; align-items: center; justify-content: center; gap: 24px; }

/* TABS */
.tabs { display: flex; gap: 4px; border-bottom: 1px solid var(--border); margin-bottom: 20px; }
.tab { padding: 8px 14px; font-size: 13px; cursor: pointer; color: var(--muted); border-bottom: 2px solid transparent; margin-bottom: -1px; transition: color 0.15s; }
.tab:hover { color: var(--text); }
.tab.active { color: var(--text); border-bottom-color: var(--text); font-weight: 500; }
.tab-content { display: none; }
.tab-content.active { display: block; }
</style>
</head>
<body>

<!-- TOPBAR -->
<header class="topbar">
  <div class="logo">
    <div class="logo-icon">EG</div>
    <div class="logo-name">Edu<span>Guard</span></div>
  </div>
  <div style="flex:1;"></div>
  <div id="topAlert" class="topbar-badge" style="cursor:pointer" onclick="showPage('alerts')">⚠ 4 High-Risk Students Need Attention</div>
  <div class="avatar">AD</div>
</header>

<!-- SIDEBAR -->
<aside class="sidebar">
  <div class="nav-group">
    <div class="nav-group-label">Overview</div>
    <div class="nav-item active" onclick="showPage('dashboard', this)">
      <svg viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
      Dashboard
    </div>
    <div class="nav-item" onclick="showPage('alerts', this)">
      <svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>
      Alerts
      <span class="nav-badge">4</span>
    </div>
  </div>
  <div class="nav-group">
    <div class="nav-group-label">Students</div>
    <div class="nav-item" onclick="showPage('students', this)">
      <svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>
      All Students
      <span class="nav-badge green">124</span>
    </div>
    <div class="nav-item" onclick="showPage('predict', this)">
      <svg viewBox="0 0 24 24"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
      Risk Predictor
    </div>
    <div class="nav-item" onclick="showPage('add', this)">
      <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="20" y1="8" x2="20" y2="14"/><line x1="23" y1="11" x2="17" y2="11"/></svg>
      Add Student
    </div>
  </div>
  <div class="nav-group">
    <div class="nav-group-label">Analytics</div>
    <div class="nav-item" onclick="showPage('analytics', this)">
      <svg viewBox="0 0 24 24"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
      Reports
    </div>
    <div class="nav-item" onclick="showPage('interventions', this)">
      <svg viewBox="0 0 24 24"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
      Interventions
    </div>
  </div>
</aside>

<!-- MAIN CONTENT -->
<main class="main">

  <!-- ── DASHBOARD ── -->
  <div id="dashboard" class="page active">
    <div class="page-header">
      <div class="page-title">Dashboard</div>
      <div class="page-sub">Academic Year 2025–26 · College of Engineering, Akola</div>
    </div>

    <div class="stats-grid">
      <div class="stat-card blue">
        <div class="stat-label">TOTAL STUDENTS</div>
        <div class="stat-val">124</div>
        <div class="stat-trend" style="color:var(--accent)">↑ 8 enrolled this month</div>
      </div>
      <div class="stat-card danger">
        <div class="stat-label">HIGH RISK</div>
        <div class="stat-val">18</div>
        <div class="stat-trend" style="color:var(--danger)">↑ 3 from last month</div>
      </div>
      <div class="stat-card warn">
        <div class="stat-label">MEDIUM RISK</div>
        <div class="stat-val">34</div>
        <div class="stat-trend" style="color:var(--warn)">↓ 2 improved</div>
      </div>
      <div class="stat-card green">
        <div class="stat-label">LOW RISK</div>
        <div class="stat-val">72</div>
        <div class="stat-trend" style="color:var(--accent)">↑ 5 from last month</div>
      </div>
    </div>

    <div class="grid-60-40">
      <div class="card">
        <div class="card-title">Risk Trend — Last 6 Months <span>Monthly snapshot</span></div>
        <div class="chart-box tall"><canvas id="trendChart"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Risk Distribution <span>Current</span></div>
        <div class="chart-box tall" style="display:flex;align-items:center;justify-content:center;">
          <canvas id="donutChart" style="max-width:200px;max-height:200px;"></canvas>
        </div>
      </div>
    </div>

    <div class="grid-2">
      <div class="card">
        <div class="card-title">High Risk Students — Immediate Attention</div>
        <div id="highRiskList"></div>
      </div>
      <div class="card">
        <div class="card-title">Risk by Department <span>All departments</span></div>
        <div class="chart-box tall"><canvas id="deptChart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- ── ALERTS ── -->
  <div id="alerts" class="page">
    <div class="page-header">
      <div class="page-title">Alerts & Notifications</div>
      <div class="page-sub">Students requiring immediate educator intervention</div>
    </div>

    <div class="tabs">
      <div class="tab active" onclick="switchTab('alertTabs', 'alertAll', this)">All Alerts (8)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertHigh', this)">High Risk (4)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertMed', this)">Medium Risk (3)</div>
      <div class="tab" onclick="switchTab('alertTabs', 'alertAction', this)">Action Taken (1)</div>
    </div>

    <div id="alertTabs">
      <div id="alertAll" class="tab-content active">
        <div id="alertListAll"></div>
      </div>
      <div id="alertHigh" class="tab-content">
        <div id="alertListHigh"></div>
      </div>
      <div id="alertMed" class="tab-content">
        <div id="alertListMed"></div>
      </div>
      <div id="alertAction" class="tab-content">
        <div class="alert-item medium">
          <div class="alert-icon">✓</div>
          <div class="alert-body">
            <div class="alert-name">Priya Sharma — Counselling Scheduled</div>
            <div class="alert-reason">Academic support session booked for 3rd June. Risk reduced from High → Medium after attendance improvement.</div>
          </div>
          <span class="badge badge-medium">Resolved</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ── STUDENTS ── -->
  <div id="students" class="page">
    <div class="page-header" style="display:flex;align-items:flex-start;justify-content:space-between;flex-wrap:wrap;gap:12px;">
      <div>
        <div class="page-title">All Students</div>
        <div class="page-sub">124 students enrolled · 2025–26</div>
      </div>
      <button class="btn btn-primary" onclick="showPage('add', document.querySelector('.nav-item:nth-child(3)'))">
        <svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
        Add Student
      </button>
    </div>

    <div class="search-row">
      <div class="search-box">
        <svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        <input type="text" placeholder="Search by name, ID, department..." oninput="filterStudents(this.value)" id="studentSearch">
      </div>
      <select onchange="filterStudents()" id="filterRisk" style="width:140px">
        <option value="">All Risk Levels</option>
        <option value="High">High Risk</option>
        <option value="Medium">Medium Risk</option>
        <option value="Low">Low Risk</option>
      </select>
      <select onchange="filterStudents()" id="filterDept" style="width:140px">
        <option value="">All Departments</option>
        <option>Computer Science</option>
        <option>Electronics</option>
        <option>Mechanical</option>
        <option>Civil</option>
      </select>
    </div>

    <div class="card" style="padding:0;">
      <div class="table-wrap">
        <table id="studentTable">
          <thead>
            <tr>
              <th>STUDENT</th><th>DEPT</th><th>ATTENDANCE</th>
              <th>ACADEMIC</th><th>BEHAVIOR</th><th>RISK</th><th>ACTION</th>
            </tr>
          </thead>
          <tbody id="studentTbody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- ── PREDICT ── -->
  <div id="predict" class="page">
    <div class="page-header">
      <div class="page-title">Risk Predictor</div>
      <div class="page-sub">Enter student data to generate an AI-powered dropout risk assessment</div>
    </div>

    <div class="grid-60-40">
      <div class="card">
        <div class="card-title">Student Input Parameters</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;">
          <div>
            <div class="form-group" style="margin-bottom:16px">
              <label>STUDENT NAME</label>
              <input type="text" id="pName" placeholder="Enter name">
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ATTENDANCE RATE</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="72" id="pAttend" oninput="updatePredict()" style="flex:1;">
              <span id="pAttendVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">72%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ACADEMIC SCORE (avg %)</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="58" id="pScore" oninput="updatePredict()" style="flex:1;">
              <span id="pScoreVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">58%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">CLASS PARTICIPATION</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="45" id="pPartic" oninput="updatePredict()" style="flex:1;">
              <span id="pParticVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">45%</span>
            </div>
          </div>

          <div>
            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">ASSIGNMENTS SUBMITTED</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="60" id="pAssign" oninput="updatePredict()" style="flex:1;">
              <span id="pAssignVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">60%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">BEHAVIORAL SCORE</label>
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:16px;">
              <input type="range" min="0" max="100" value="65" id="pBehav" oninput="updatePredict()" style="flex:1;">
              <span id="pBehavVal" style="font-family:'DM Mono',monospace;font-size:13px;min-width:36px;font-weight:500;">65%</span>
            </div>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">FINANCIAL AID STATUS</label>
            <select id="pFinancial" onchange="updatePredict()" style="width:100%;margin-bottom:16px;">
              <option value="0">No Financial Aid</option>
              <option value="1" selected>Partial Aid</option>
              <option value="2">Full Scholarship</option>
            </select>

            <label style="display:block;margin-bottom:8px;font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;">FAMILY SUPPORT</label>
            <select id="pFamily" onchange="updatePredict()" style="width:100%;margin-bottom:16px;">
              <option value="0">Low</option>
              <option value="1" selected>Medium</option>
              <option value="2">High</option>
            </select>
          </div>
        </div>

        <div style="display:flex;gap:10px;margin-top:8px;">
          <button class="btn btn-primary" onclick="runPredict()">Run Prediction</button>
          <button class="btn" onclick="resetPredict()">Reset</button>
        </div>
      </div>

      <div class="card" id="resultCard">
        <div class="card-title">Risk Assessment Result</div>
        <div class="risk-score-display" id="riskScore">—</div>
        <div class="risk-category" id="riskCategory" style="color:var(--muted)">Run prediction to see result</div>
        <div class="risk-meter">
          <div class="risk-bar-wrap"><div class="risk-bar" id="riskBarEl" style="width:0%;background:var(--muted)"></div></div>
        </div>
        <div style="margin-top:16px;">
          <div style="font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:10px;">FACTOR BREAKDOWN</div>
          <div class="factor-row"><div class="factor-label">Attendance</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb1" style="width:72%;background:var(--warn)"></div></div><div class="factor-pct" id="fp1">72%</div></div>
          <div class="factor-row"><div class="factor-label">Academic Score</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb2" style="width:58%;background:var(--danger)"></div></div><div class="factor-pct" id="fp2">58%</div></div>
          <div class="factor-row"><div class="factor-label">Participation</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb3" style="width:45%;background:var(--danger)"></div></div><div class="factor-pct" id="fp3">45%</div></div>
          <div class="factor-row"><div class="factor-label">Assignments</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb4" style="width:60%;background:var(--warn)"></div></div><div class="factor-pct" id="fp4">60%</div></div>
          <div class="factor-row"><div class="factor-label">Behavior</div><div class="factor-bar-wrap"><div class="factor-bar" id="fb5" style="width:65%;background:var(--warn)"></div></div><div class="factor-pct" id="fp5">65%</div></div>
        </div>
        <div id="riskRecommendations" style="margin-top:16px;display:none;">
          <div style="font-size:12px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:8px;">RECOMMENDED ACTIONS</div>
          <div id="recList"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ── ADD STUDENT ── -->
  <div id="add" class="page">
    <div class="page-header">
      <div class="page-title">Add New Student</div>
      <div class="page-sub">Enter all relevant information to create a student profile</div>
    </div>
    <div class="card" style="max-width:680px;">
      <div class="form-grid">
        <div class="form-group">
          <label>FULL NAME *</label>
          <input type="text" id="addName" placeholder="e.g. Rahul Patil">
        </div>
        <div class="form-group">
          <label>STUDENT ID *</label>
          <input type="text" id="addId" placeholder="e.g. CSE2026001">
        </div>
        <div class="form-group">
          <label>DEPARTMENT *</label>
          <select id="addDept">
            <option>Computer Science</option>
            <option>Electronics</option>
            <option>Mechanical</option>
            <option>Civil</option>
          </select>
        </div>
        <div class="form-group">
          <label>YEAR / SEMESTER</label>
          <select id="addYear">
            <option>First Year (Sem 1)</option>
            <option>First Year (Sem 2)</option>
            <option>Second Year (Sem 3)</option>
            <option selected>Second Year (Sem 4)</option>
            <option>Third Year (Sem 5)</option>
            <option>Third Year (Sem 6)</option>
            <option>Final Year (Sem 7)</option>
            <option>Final Year (Sem 8)</option>
          </select>
        </div>
        <div class="form-group">
          <label>EMAIL</label>
          <input type="email" id="addEmail" placeholder="student@college.edu">
        </div>
        <div class="form-group">
          <label>PHONE</label>
          <input type="tel" id="addPhone" placeholder="+91 XXXXX XXXXX">
        </div>

        <div class="form-group full" style="border-top:1px solid var(--border);padding-top:16px;margin-top:4px;">
          <div style="font-size:13px;font-weight:500;margin-bottom:12px;">Academic & Behavioral Data</div>
        </div>

        <div class="form-group">
          <label>ATTENDANCE RATE (%)</label>
          <input type="number" id="addAttend" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>AVERAGE ACADEMIC SCORE (%)</label>
          <input type="number" id="addScore" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>CLASS PARTICIPATION (%)</label>
          <input type="number" id="addPartic" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>ASSIGNMENTS SUBMITTED (%)</label>
          <input type="number" id="addAssign" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>BEHAVIORAL SCORE (%)</label>
          <input type="number" id="addBehav" placeholder="0–100" min="0" max="100">
        </div>
        <div class="form-group">
          <label>FINANCIAL AID</label>
          <select id="addFinancial">
            <option value="0">None</option>
            <option value="1">Partial</option>
            <option value="2">Full Scholarship</option>
          </select>
        </div>
        <div class="form-group">
          <label>FAMILY SUPPORT</label>
          <select id="addFamily">
            <option value="0">Low</option>
            <option value="1">Medium</option>
            <option value="2">High</option>
          </select>
        </div>
        <div class="form-group full">
          <label>NOTES / REMARKS</label>
          <textarea id="addNotes" rows="3" placeholder="Any observations, previous counselling notes..."></textarea>
        </div>
        <div class="form-group full" style="flex-direction:row;gap:10px;">
          <button class="btn btn-primary" onclick="addStudent()">
            <svg viewBox="0 0 24 24"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>
            Save & Predict Risk
          </button>
          <button class="btn" onclick="clearAddForm()">Clear</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ── ANALYTICS ── -->
  <div id="analytics" class="page">
    <div class="page-header">
      <div class="page-title">Reports & Analytics</div>
      <div class="page-sub">Institutional dropout risk analysis and trends</div>
    </div>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-label">DROPOUT RATE (2024)</div>
        <div class="stat-val" style="color:var(--danger)">9.2%</div>
        <div class="stat-trend" style="color:var(--accent)">↓ 2.1% from 2023</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">INTERVENTIONS DONE</div>
        <div class="stat-val" style="color:var(--blue)">47</div>
        <div class="stat-trend" style="color:var(--muted)">This semester</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">SUCCESS RATE</div>
        <div class="stat-val" style="color:var(--accent)">78%</div>
        <div class="stat-trend" style="color:var(--accent)">Of interventions prevented dropout</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">AVG RISK SCORE</div>
        <div class="stat-val" style="color:var(--warn)">41</div>
        <div class="stat-trend" style="color:var(--accent)">↓ 4 pts from last semester</div>
      </div>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-title">Monthly Dropout Risk Trend <span>2025</span></div>
        <div class="chart-box tall"><canvas id="monthlyChart"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Risk Factors Impact <span>Weighted analysis</span></div>
        <div class="chart-box tall"><canvas id="radarChart"></canvas></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">Year-wise Risk Distribution</div>
      <div class="chart-box" style="height:180px;"><canvas id="yearChart"></canvas></div>
    </div>
  </div>

  <!-- ── INTERVENTIONS ── -->
  <div id="interventions" class="page">
    <div class="page-header">
      <div class="page-title">Interventions</div>
      <div class="page-sub">Preventive actions assigned to at-risk students</div>
    </div>
    <div class="card">
      <div class="card-title">Active Intervention Plans</div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--danger-bg);">🎓</div>
        <div>
          <div class="int-title">Academic Support — Arjun Deshmukh</div>
          <div class="int-desc">Weekly tutoring sessions with faculty mentor. Focus: Mathematics & Data Structures. Started 20 May.</div>
        </div>
        <div class="int-status"><span class="badge badge-high">High Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--warn-bg);">🧠</div>
        <div>
          <div class="int-title">Counselling — Sneha Kulkarni</div>
          <div class="int-desc">Bi-weekly counselling to address family-related stress. Sessions with Dr. Meera Joshi.</div>
        </div>
        <div class="int-status"><span class="badge badge-medium">Medium Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--blue-bg);">💰</div>
        <div>
          <div class="int-title">Financial Aid Review — Ravi Thakur</div>
          <div class="int-desc">Scholarship application submitted. Awaiting approval from University. Emergency fund approved for this semester.</div>
        </div>
        <div class="int-status"><span class="badge badge-high">High Risk</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--accent-bg);">👥</div>
        <div>
          <div class="int-title">Peer Mentoring — 8 Students</div>
          <div class="int-desc">Medium-risk students paired with senior student mentors. Monthly check-ins scheduled.</div>
        </div>
        <div class="int-status"><span class="badge badge-medium">Ongoing</span></div>
      </div>
      <div class="intervention-card">
        <div class="int-icon" style="background:var(--accent-bg);">📞</div>
        <div>
          <div class="int-title">Parent Communication — 6 Students</div>
          <div class="int-desc">Letters sent to parents of high-risk students. Phone follow-ups scheduled for next week.</div>
        </div>
        <div class="int-status"><span class="badge badge-low">Done</span></div>
      </div>
    </div>

    <div class="card" style="margin-top:20px;">
      <div class="card-title">Log New Intervention</div>
      <div class="form-grid">
        <div class="form-group">
          <label>STUDENT NAME</label>
          <input type="text" placeholder="Search student...">
        </div>
        <div class="form-group">
          <label>INTERVENTION TYPE</label>
          <select>
            <option>Academic Support</option>
            <option>Counselling</option>
            <option>Financial Aid</option>
            <option>Peer Mentoring</option>
            <option>Parent Communication</option>
            <option>Medical Referral</option>
          </select>
        </div>
        <div class="form-group">
          <label>ASSIGNED TO</label>
          <input type="text" placeholder="Faculty / Counsellor name">
        </div>
        <div class="form-group">
          <label>START DATE</label>
          <input type="date">
        </div>
        <div class="form-group full">
          <label>NOTES</label>
          <textarea rows="2" placeholder="Details of the intervention plan..."></textarea>
        </div>
        <div class="form-group full">
          <button class="btn btn-primary" style="width:fit-content">Log Intervention</button>
        </div>
      </div>
    </div>
  </div>

</main>

<!-- STUDENT DETAIL MODAL -->
<div class="modal-overlay" id="studentModal">
  <div class="modal">
    <div class="modal-header">
      <div class="modal-title" id="modalStudentName">Student Details</div>
      <button class="modal-close" onclick="closeModal()">×</button>
    </div>
    <div id="modalContent"></div>
    <div class="modal-footer">
      <button class="btn" onclick="closeModal()">Close</button>
      <button class="btn btn-primary" onclick="closeModal()">Log Intervention</button>
    </div>
  </div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────────────────────
let students = [
  {id:'CSE001', name:'Arjun Deshmukh', dept:'Computer Science', attend:52, score:41, partic:30, assign:45, behav:50, financial:0, family:0},
  {id:'CSE002', name:'Priya Sharma', dept:'Computer Science', attend:61, score:55, partic:40, assign:58, behav:62, financial:1, family:1},
  {id:'ELE001', name:'Ravi Thakur', dept:'Electronics', attend:48, score:38, partic:25, assign:40, behav:45, financial:0, family:0},
  {id:'MEC001', name:'Sneha Kulkarni', dept:'Mechanical', attend:69, score:58, partic:50, assign:62, behav:55, financial:1, family:0},
  {id:'CSE003', name:'Karan Mehta', dept:'Computer Science', attend:75, score:68, partic:60, assign:70, behav:72, financial:1, family:1},
  {id:'ELE002', name:'Ananya Joshi', dept:'Electronics', attend:82, score:74, partic:70, assign:78, behav:80, financial:2, family:2},
  {id:'CIV001', name:'Rohan Patil', dept:'Civil', attend:55, score:48, partic:35, assign:50, behav:58, financial:0, family:1},
  {id:'MEC002', name:'Dipika Rao', dept:'Mechanical', attend:88, score:82, partic:80, assign:85, behav:88, financial:2, family:2},
  {id:'CSE004', name:'Vikram Soni', dept:'Computer Science', attend:90, score:86, partic:85, assign:88, behav:90, financial:1, family:2},
  {id:'ELE003', name:'Meera Nair', dept:'Electronics', attend:45, score:35, partic:22, assign:38, behav:42, financial:0, family:0},
  {id:'CIV002', name:'Aditya Kumar', dept:'Civil', attend:78, score:71, partic:65, assign:72, behav:75, financial:1, family:1},
  {id:'MEC003', name:'Pooja Singh', dept:'Mechanical', attend:65, score:60, partic:55, assign:63, behav:68, financial:1, family:1},
];

function calcRisk(s) {
  const w = {attend:0.28, score:0.25, partic:0.15, assign:0.15, behav:0.10, financial:0.04, family:0.03};
  const finScore = [40,65,85][+s.financial] || 65;
  const famScore = [40,65,85][+s.family] || 65;
  const raw = (s.attend*w.attend + s.score*w.score + s.partic*w.partic +
               s.assign*w.assign + s.behav*w.behav + finScore*w.financial + famScore*w.family);
  const riskScore = Math.round(100 - raw);
  return riskScore;
}
function riskLevel(score) {
  if (score >= 60) return 'High';
  if (score >= 35) return 'Medium';
  return 'Low';
}
function riskColor(level) {
  return level === 'High' ? 'var(--danger)' : level === 'Medium' ? 'var(--warn)' : 'var(--accent)';
}
function badgeClass(level) {
  return level === 'High' ? 'badge-high' : level === 'Medium' ? 'badge-medium' : 'badge-low';
}

// ── NAVIGATION ──────────────────────────────────────────────────────────
function showPage(id, el) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if (el) el.classList.add('active');
  if (id === 'students') renderStudents();
  if (id === 'analytics') setTimeout(renderAnalyticsCharts, 100);
  if (id === 'dashboard') setTimeout(renderDashboardCharts, 50);
}
function switchTab(groupId, tabId, btn) {
  const group = document.getElementById(groupId);
  group.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.getElementById(tabId).classList.add('active');
  const tabBar = btn.parentElement;
  tabBar.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
}

// ── STUDENTS ────────────────────────────────────────────────────────────
function renderStudents(data) {
  const tbody = document.getElementById('studentTbody');
  const list = data || students;
  tbody.innerHTML = list.map(s => {
    const rs = calcRisk(s);
    const rl = riskLevel(rs);
    return `<tr>
      <td><div style="font-weight:500">${s.name}</div><div style="font-size:11px;color:var(--muted);font-family:'DM Mono',monospace">${s.id}</div></td>
      <td style="color:var(--muted)">${s.dept}</td>
      <td>
        <div style="display:flex;align-items:center;gap:8px">
          <div style="width:50px;background:var(--surface2);border-radius:99px;height:5px">
            <div style="width:${s.attend}%;height:5px;border-radius:99px;background:${s.attend<60?'var(--danger)':s.attend<75?'var(--warn)':'var(--accent)'}"></div>
          </div>
          <span style="font-family:'DM Mono',monospace;font-size:12px">${s.attend}%</span>
        </div>
      </td>
      <td><span style="font-family:'DM Mono',monospace;font-size:12px;font-weight:500;color:${s.score<50?'var(--danger)':s.score<65?'var(--warn)':'var(--accent)'}">${s.score}%</span></td>
      <td><span style="font-family:'DM Mono',monospace;font-size:12px">${s.behav}%</span></td>
      <td><span class="badge ${badgeClass(rl)}">${rl} (${rs})</span></td>
      <td><button class="btn btn-sm" onclick="viewStudent('${s.id}')">View</button></td>
    </tr>`;
  }).join('');
}
function filterStudents(val) {
  const search = (document.getElementById('studentSearch').value||'').toLowerCase();
  const risk = document.getElementById('filterRisk').value;
  const dept = document.getElementById('filterDept').value;
  const filtered = students.filter(s => {
    const rl = riskLevel(calcRisk(s));
    return (!search || s.name.toLowerCase().includes(search) || s.id.toLowerCase().includes(search) || s.dept.toLowerCase().includes(search))
      && (!risk || rl === risk)
      && (!dept || s.dept === dept);
  });
  renderStudents(filtered);
}
function viewStudent(id) {
  const s = students.find(x => x.id === id);
  if (!s) return;
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  document.getElementById('modalStudentName').textContent = s.name;
  const finLabel = ['None','Partial','Full Scholarship'][+s.financial];
  const famLabel = ['Low','Medium','High'][+s.family];
  document.getElementById('modalContent').innerHTML = `
    <div style="display:flex;align-items:center;gap:16px;margin-bottom:20px;">
      <div style="width:56px;height:56px;border-radius:50%;background:var(--surface2);display:flex;align-items:center;justify-content:center;font-family:'Syne',sans-serif;font-weight:700;font-size:20px">${s.name.split(' ').map(n=>n[0]).join('')}</div>
      <div>
        <div style="font-weight:600;font-size:16px">${s.name}</div>
        <div style="color:var(--muted);font-size:13px">${s.id} · ${s.dept}</div>
      </div>
      <span class="badge ${badgeClass(rl)}" style="margin-left:auto;font-size:13px;padding:5px 12px">${rl} Risk — ${rs}/100</span>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px;">
      ${[['Attendance',s.attend+'%'],['Academic Score',s.score+'%'],['Participation',s.partic+'%'],['Assignments',s.assign+'%'],['Behavior',s.behav+'%'],['Financial Aid',finLabel],['Family Support',famLabel]].map(([k,v])=>`
        <div style="background:var(--bg);border-radius:8px;padding:10px 14px;">
          <div style="font-size:11px;color:var(--muted);font-family:'DM Mono',monospace;margin-bottom:4px">${k.toUpperCase()}</div>
          <div style="font-weight:500;font-size:14px">${v}</div>
        </div>`).join('')}
    </div>
    <div style="background:${rl==='High'?'var(--danger-bg)':rl==='Medium'?'var(--warn-bg)':'var(--accent-bg)'};border-radius:8px;padding:14px;">
      <div style="font-weight:500;margin-bottom:4px">Recommended Action</div>
      <div style="font-size:13px;color:var(--muted)">${getRecommendation(rs, s)}</div>
    </div>`;
  document.getElementById('studentModal').classList.add('open');
}
function closeModal() { document.getElementById('studentModal').classList.remove('open'); }

// ── RISK PREDICTOR ────────────────────────────────────────────────────
function updatePredict() {
  ['Attend','Score','Partic','Assign','Behav'].forEach(k => {
    const v = document.getElementById('p'+k).value;
    document.getElementById('p'+k+'Val').textContent = v + '%';
    document.getElementById('fb'+((['Attend','Score','Partic','Assign','Behav'].indexOf(k)+1))).style.width = v+'%';
    document.getElementById('fp'+((['Attend','Score','Partic','Assign','Behav'].indexOf(k)+1))).textContent = v+'%';
  });
  ['fb1','fb2','fb3','fb4','fb5'].forEach((id,i) => {
    const vals = [+document.getElementById('pAttend').value, +document.getElementById('pScore').value,
      +document.getElementById('pPartic').value, +document.getElementById('pAssign').value, +document.getElementById('pBehav').value];
    const v = vals[i];
    document.getElementById(id).style.background = v<50?'var(--danger)':v<70?'var(--warn)':'var(--accent)';
  });
}
function runPredict() {
  const s = {
    attend: +document.getElementById('pAttend').value,
    score: +document.getElementById('pScore').value,
    partic: +document.getElementById('pPartic').value,
    assign: +document.getElementById('pAssign').value,
    behav: +document.getElementById('pBehav').value,
    financial: document.getElementById('pFinancial').value,
    family: document.getElementById('pFamily').value,
  };
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  document.getElementById('riskScore').textContent = rs;
  document.getElementById('riskScore').style.color = riskColor(rl);
  document.getElementById('riskCategory').textContent = rl + ' Risk';
  document.getElementById('riskCategory').style.color = riskColor(rl);
  document.getElementById('riskBarEl').style.width = rs + '%';
  document.getElementById('riskBarEl').style.background = riskColor(rl);
  const recDiv = document.getElementById('riskRecommendations');
  recDiv.style.display = 'block';
  document.getElementById('recList').innerHTML = getRecommendationList(rs, s).map(r=>`<div style="display:flex;gap:8px;margin-bottom:6px;font-size:13px"><span style="color:${riskColor(rl)}">→</span><span>${r}</span></div>`).join('');
}
function resetPredict() {
  ['pAttend','pScore','pPartic','pAssign','pBehav'].forEach(id => {
    document.getElementById(id).value = 70;
  });
  document.getElementById('pFinancial').value = 1;
  document.getElementById('pFamily').value = 1;
  updatePredict();
  document.getElementById('riskScore').textContent = '—';
  document.getElementById('riskCategory').textContent = 'Run prediction to see result';
  document.getElementById('riskCategory').style.color = 'var(--muted)';
  document.getElementById('riskBarEl').style.width = '0%';
  document.getElementById('riskRecommendations').style.display = 'none';
}
function getRecommendation(rs, s) {
  if (rs >= 60) return 'Immediate counselling and academic support required. Contact parents and assign a faculty mentor. Consider financial aid review.';
  if (rs >= 35) return 'Schedule a check-in with the student. Review attendance and assignment submission. Assign a peer mentor.';
  return 'Student is performing well. Continue monitoring monthly. Encourage participation in extracurricular activities.';
}
function getRecommendationList(rs, s) {
  const recs = [];
  if (s.attend < 60) recs.push('Critical: Attendance below threshold — initiate parent contact immediately');
  if (s.score < 50) recs.push('Academic support sessions with subject faculty recommended');
  if (s.partic < 40) recs.push('Encourage participation through peer group activities and group projects');
  if (s.assign < 55) recs.push('Assignment tracking — weekly check-in with class coordinator');
  if (+s.financial === 0) recs.push('Review eligibility for financial aid or scholarship schemes');
  if (+s.family === 0) recs.push('Family counselling or social support program referral advised');
  if (recs.length === 0) recs.push('No immediate action required — maintain monthly monitoring');
  return recs;
}

// ── ADD STUDENT ──────────────────────────────────────────────────────
function addStudent() {
  const name = document.getElementById('addName').value.trim();
  if (!name) { alert('Please enter student name'); return; }
  const s = {
    id: document.getElementById('addId').value || 'STU' + Date.now(),
    name, dept: document.getElementById('addDept').value,
    attend: +document.getElementById('addAttend').value || 70,
    score: +document.getElementById('addScore').value || 65,
    partic: +document.getElementById('addPartic').value || 60,
    assign: +document.getElementById('addAssign').value || 65,
    behav: +document.getElementById('addBehav').value || 70,
    financial: document.getElementById('addFinancial').value,
    family: document.getElementById('addFamily').value,
  };
  students.unshift(s);
  const rs = calcRisk(s);
  const rl = riskLevel(rs);
  alert(`✓ Student "${name}" added successfully!\nRisk Level: ${rl} (${rs}/100)\n\n${getRecommendation(rs, s)}`);
  clearAddForm();
  showPage('students', document.querySelector('.nav-item'));
}
function clearAddForm() {
  ['addName','addId','addEmail','addPhone','addAttend','addScore','addPartic','addAssign','addBehav','addNotes'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
}

// ── DASHBOARD CHARTS ─────────────────────────────────────────────────
let trendChartInst, donutChartInst, deptChartInst, monthlyChartInst, radarChartInst, yearChartInst;
function renderDashboardCharts() {
  // High risk list
  const highRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 60).sort((a,b) => b.rs - a.rs).slice(0, 4);
  document.getElementById('highRiskList').innerHTML = highRisk.map(s => `
    <div class="alert-item high" style="cursor:pointer" onclick="viewStudent('${s.id}')">
      <div class="alert-icon">${s.rs}</div>
      <div class="alert-body">
        <div class="alert-name">${s.name}</div>
        <div class="alert-reason">${s.dept} · Attendance: ${s.attend}% · Score: ${s.score}%</div>
      </div>
      <span class="badge badge-high">High</span>
    </div>`).join('');

  // Trend chart
  if (trendChartInst) trendChartInst.destroy();
  trendChartInst = new Chart(document.getElementById('trendChart'), {
    type: 'line',
    data: {
      labels: ['Jan','Feb','Mar','Apr','May','Jun'],
      datasets: [
        { label: 'High Risk', data: [22,20,19,21,18,18], borderColor:'#d85a30', backgroundColor:'rgba(216,90,48,0.08)', fill:true, tension:0.4 },
        { label: 'Medium Risk', data: [30,32,35,33,36,34], borderColor:'#ba7517', backgroundColor:'rgba(186,117,23,0.08)', fill:true, tension:0.4 },
        { label: 'Low Risk', data: [62,65,66,64,70,72], borderColor:'#1d9e75', backgroundColor:'rgba(29,158,117,0.08)', fill:true, tension:0.4 },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{y:{beginAtZero:true,grid:{color:'rgba(0,0,0,0.05)'}},x:{grid:{display:false}}} }
  });

  // Donut
  if (donutChartInst) donutChartInst.destroy();
  const high = students.filter(s => riskLevel(calcRisk(s)) === 'High').length;
  const med = students.filter(s => riskLevel(calcRisk(s)) === 'Medium').length;
  const low = students.filter(s => riskLevel(calcRisk(s)) === 'Low').length;
  donutChartInst = new Chart(document.getElementById('donutChart'), {
    type: 'doughnut',
    data: { labels: ['High','Medium','Low'], datasets: [{ data:[high,med,low], backgroundColor:['#d85a30','#ba7517','#1d9e75'], borderWidth:0 }] },
    options: { responsive:true, maintainAspectRatio:true, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}} }
  });

  // Dept bar
  if (deptChartInst) deptChartInst.destroy();
  deptChartInst = new Chart(document.getElementById('deptChart'), {
    type: 'bar',
    data: {
      labels: ['Computer Science','Electronics','Mechanical','Civil'],
      datasets: [
        { label:'High', data:[5,6,4,3], backgroundColor:'#d85a30' },
        { label:'Medium', data:[10,9,8,7], backgroundColor:'#ba7517' },
        { label:'Low', data:[20,15,18,19], backgroundColor:'#1d9e75' },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{x:{stacked:true,grid:{display:false}},y:{stacked:true,grid:{color:'rgba(0,0,0,0.05)'}}} }
  });
}

// Alerts
function renderAlerts() {
  const highRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 60).sort((a,b) => b.rs - a.rs);
  const medRisk = students.map(s => ({...s, rs: calcRisk(s)})).filter(s => s.rs >= 35 && s.rs < 60).sort((a,b) => b.rs - a.rs).slice(0,3);
  const makeAlert = (s, cls) => `
    <div class="alert-item ${cls}" style="cursor:pointer" onclick="viewStudent('${s.id}')">
      <div class="alert-icon">${s.rs}</div>
      <div class="alert-body">
        <div class="alert-name">${s.name} — ${s.dept}</div>
        <div class="alert-reason">Attendance ${s.attend}% · Score ${s.score}% · Risk Score ${s.rs}/100</div>
      </div>
      <button class="btn btn-sm btn-danger" onclick="event.stopPropagation()">Intervene</button>
    </div>`;
  document.getElementById('alertListAll').innerHTML = [...highRisk.slice(0,4).map(s=>makeAlert(s,'high')), ...medRisk.map(s=>makeAlert(s,'medium'))].join('');
  document.getElementById('alertListHigh').innerHTML = highRisk.slice(0,4).map(s=>makeAlert(s,'high')).join('');
  document.getElementById('alertListMed').innerHTML = medRisk.map(s=>makeAlert(s,'medium')).join('');
}

function renderAnalyticsCharts() {
  if (monthlyChartInst) monthlyChartInst.destroy();
  monthlyChartInst = new Chart(document.getElementById('monthlyChart'), {
    type: 'line',
    data: {
      labels: ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'],
      datasets: [{ label:'Risk Score (avg)', data:[52,50,48,51,46,44,47,42,41,40,39,38], borderColor:'#d85a30', backgroundColor:'rgba(216,90,48,0.07)', fill:true, tension:0.4 }]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{display:false}}, scales:{y:{min:30,max:60,grid:{color:'rgba(0,0,0,0.05)'}},x:{grid:{display:false}}} }
  });

  if (radarChartInst) radarChartInst.destroy();
  radarChartInst = new Chart(document.getElementById('radarChart'), {
    type: 'radar',
    data: {
      labels: ['Attendance','Academic Score','Participation','Assignments','Behavior','Financial','Family'],
      datasets: [{ label:'High Risk Students', data:[38,35,30,40,45,25,30], backgroundColor:'rgba(216,90,48,0.15)', borderColor:'#d85a30', pointBackgroundColor:'#d85a30' }]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom'}}, scales:{r:{min:0,max:100,ticks:{font:{size:10}}}} }
  });

  if (yearChartInst) yearChartInst.destroy();
  yearChartInst = new Chart(document.getElementById('yearChart'), {
    type: 'bar',
    data: {
      labels: ['1st Year','2nd Year','3rd Year','Final Year'],
      datasets: [
        { label:'High', data:[8,5,3,2], backgroundColor:'#d85a30' },
        { label:'Medium', data:[12,10,7,5], backgroundColor:'#ba7517' },
        { label:'Low', data:[10,18,22,22], backgroundColor:'#1d9e75' },
      ]
    },
    options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{position:'bottom',labels:{boxWidth:10,font:{size:11}}}}, scales:{x:{stacked:true,grid:{display:false}},y:{stacked:true,grid:{color:'rgba(0,0,0,0.05)'}}} }
  });
}

// ── INIT ──────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  renderStudents();
  renderAlerts();
  setTimeout(renderDashboardCharts, 100);
});
document.getElementById('studentModal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});
</script>
</body>
</html># The-Students-Dropout-Prevention-System
 designed to identify and predict students who are at risk of dropping out from educational institutions. In many schools and colleges, student dropout is a major concern that affects academic performance, institutional reputation, and overall student development.
