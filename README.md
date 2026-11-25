<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>MITS-DU Central Library | Book Management & Peer Study System</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Inter & Plus Jakarta Sans -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Plus+Jakarta+Sans:wght@600;700;800&display=swap" rel="stylesheet">
  <!-- Font Awesome for clean academic icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" />
  <!-- QRious for clean scannable UPI QR Code -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            display: ['Plus Jakarta Sans', 'sans-serif'],
          },
          colors: {
            mits: {
              navy: '#0B1F3A',
              royal: '#13315C',
              gold: '#D97706',
              amber: '#F59E0B',
              cream: '#FFFBEB',
            }
          }
        }
      }
    }
  </script>
  <style>
    /* Smooth custom scrollbar & subtle transitions */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #f1f5f9;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94a3b8;
    }
    .tab-panel {
      animation: fadeIn 0.25s ease-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(4px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col">

  <!-- Toast Notification Container (Replaces alert/confirm) -->
  <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 max-w-sm w-full pointer-events-none px-4 sm:px-0"></div>

  <!-- Custom Confirmation / Edit Days Modal -->
  <div id="modal-backdrop" class="fixed inset-0 bg-slate-900/60 backdrop-blur-xs z-40 hidden items-center justify-center p-4">
    <div id="modal-box" class="bg-white rounded-2xl shadow-2xl border border-slate-200 max-w-md w-full p-6 transform transition-all">
      <div class="flex items-start justify-between mb-4">
        <div class="flex items-center gap-3">
          <div id="modal-icon-wrap" class="w-10 h-10 rounded-xl bg-amber-100 text-amber-700 flex items-center justify-center text-lg">
            <i class="fa-solid fa-pen-to-square"></i>
          </div>
          <div>
            <h3 id="modal-title" class="font-display font-bold text-slate-900 text-lg">Update Borrow Entry</h3>
            <p id="modal-subtitle" class="text-xs text-slate-500">Modify allowed days or simulate days passed</p>
          </div>
        </div>
        <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 p-1 rounded-lg">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>
      <div id="modal-body" class="space-y-4"></div>
    </div>
  </div>

  <!-- Top Academic Header with Custom MITS-DU Emblem -->
  <header class="bg-gradient-to-r from-mits-navy via-mits-royal to-mits-navy text-white shadow-lg border-b-4 border-amber-500 sticky top-0 z-30">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3.5">
      <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <!-- Logo & University Title -->
        <div class="flex items-center gap-3.5">
          <!-- Official MITS-DU Crest/Emblem (Matches uploaded images.jpg with crisp vector precision) -->
          <div class="shrink-0 bg-white p-1.5 rounded-2xl border-2 border-amber-400/70 shadow-md flex items-center justify-center">
            <svg width="74" height="66" viewBox="0 0 220 195" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Madhav Institute of Technology & Science Official Crest">
              <defs>
                <!-- Top circular arc path for MADHAV INSTITUTE OF TECH & SCIENCE -->
                <path id="mitsTopArc" d="M 43 92 A 67 67 0 1 1 177 92" />
                <!-- Bottom curved path for WORK IS WORSHIP -->
                <path id="mitsRibbonArc" d="M 48 151 Q 110 135 172 151" />
                <!-- Clip path for inner circular emblem -->
                <clipPath id="innerEmblemClip">
                  <circle cx="110" cy="86" r="56" />
                </clipPath>
              </defs>

              <!-- Outer Red Circular Ring -->
              <circle cx="110" cy="86" r="76" fill="#C8281E" stroke="#1E293B" stroke-width="1.5" />
              <!-- Inner Light Blue Sky Circle -->
              <circle cx="110" cy="86" r="56" fill="#CBE4F9" stroke="#1E293B" stroke-width="1.5" />

              <!-- Arched Yellow Text inside Red Ring -->
              <text fill="#FDE047" font-family="Plus Jakarta Sans, Inter, sans-serif" font-weight="800" font-size="11.2" letter-spacing="0.9">
                <textPath href="#mitsTopArc" startOffset="50%" text-anchor="middle">
                  MADHAV INSTITUTE OF TECH &amp; SCIENCE
                </textPath>
              </text>

              <!-- Inner Upper & Lower Crest Elements -->
              <g clip-path="url(#innerEmblemClip)">
                <!-- White Lower Shield Basin -->
                <path d="M 52 80 H 168 L 154 126 L 110 143 L 66 126 Z" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2" />

                <!-- Radiant Red-Rayed Sun at Top Center -->
                <g transform="translate(110, 56)">
                  <!-- Sun Rays -->
                  <g stroke="#DC2626" stroke-width="1.6" stroke-linecap="round">
                    <line x1="0" y1="-15" x2="0" y2="-11" />
                    <line x1="0" y1="11" x2="0" y2="15" />
                    <line x1="-15" y1="0" x2="-11" y2="0" />
                    <line x1="11" y1="0" x2="15" y2="0" />
                    <line x1="-10.6" y1="-10.6" x2="-7.8" y2="-7.8" />
                    <line x1="7.8" y1="7.8" x2="10.6" y2="10.6" />
                    <line x1="10.6" y1="-10.6" x2="7.8" y2="-7.8" />
                    <line x1="-7.8" y1="7.8" x2="-10.6" y2="10.6" />
                    <line x1="-5.7" y1="-13.8" x2="-4.2" y2="-10.2" />
                    <line x1="5.7" y1="-13.8" x2="4.2" y2="-10.2" />
                    <line x1="-13.8" y1="-5.7" x2="-10.2" y2="-4.2" />
                    <line x1="13.8" y1="-5.7" x2="10.2" y2="-4.2" />
                    <line x1="-13.8" y1="5.7" x2="-10.2" y2="4.2" />
                    <line x1="13.8" y1="5.7" x2="10.2" y2="4.2" />
                    <line x1="-5.7" y1="13.8" x2="-4.2" y2="10.2" />
                    <line x1="5.7" y1="13.8" x2="4.2" y2="10.2" />
                  </g>
                  <!-- Sun Face Circle -->
                  <circle cx="0" cy="0" r="9.5" fill="#FFFFFF" stroke="#DC2626" stroke-width="1.4" />
                  <!-- Eyes & Smile -->
                  <circle cx="-3" cy="-2" r="1.1" fill="#DC2626" />
                  <circle cx="3" cy="-2" r="1.1" fill="#DC2626" />
                  <path d="M -3.5 2.5 Q 0 5.5 3.5 2.5" stroke="#DC2626" stroke-width="1.4" fill="none" stroke-linecap="round" />
                </g>

                <!-- Left Symmetrical White/Blue-Outlined Serpent Facing Sun -->
                <path d="M 94 56 C 89 52 82 54 83 60 C 84 65 92 67 91 72 C 90 76 80 77 75 74 C 71 71 72 64 76 62 C 79 61 81 64 78 66" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                <circle cx="90" cy="56" r="1" fill="#1E3A8A" />
                <path d="M 74 76 H 95" stroke="#1E3A8A" stroke-width="2" stroke-linecap="round" />

                <!-- Right Symmetrical White/Blue-Outlined Serpent Facing Sun -->
                <path d="M 126 56 C 131 52 138 54 137 60 C 136 65 128 67 129 72 C 130 76 140 77 145 74 C 149 71 148 64 144 62 C 141 61 139 64 142 66" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                <circle cx="130" cy="56" r="1" fill="#1E3A8A" />
                <path d="M 125 76 H 146" stroke="#1E3A8A" stroke-width="2" stroke-linecap="round" />

                <!-- Blue Wave Line Above Boat -->
                <path d="M 80 91 Q 92 86 104 90 Q 116 94 128 89 Q 135 87 140 90" stroke="#0284C7" stroke-width="3.2" stroke-linecap="round" fill="none" />

                <!-- Blue Boat / Vessel Silhouette -->
                <path d="M 91 97 Q 110 101 129 97 L 125 103 Q 110 106 95 103 Z" fill="#0284C7" stroke="#0369A1" stroke-width="1.2" />
                <!-- Boat Porthole Dots -->
                <circle cx="101" cy="100.5" r="0.9" fill="#FFFFFF" />
                <circle cx="107" cy="101" r="0.9" fill="#FFFFFF" />
                <circle cx="113" cy="101" r="0.9" fill="#FFFFFF" />
                <circle cx="119" cy="100.5" r="0.9" fill="#FFFFFF" />

                <!-- Dashed Blue Water Ripples Below Boat -->
                <g stroke="#0284C7" stroke-width="1.8" stroke-dasharray="5 3" stroke-linecap="round">
                  <line x1="74" y1="110" x2="146" y2="110" />
                  <line x1="78" y1="115" x2="142" y2="115" />
                  <line x1="83" y1="120" x2="137" y2="120" />
                  <line x1="89" y1="125" x2="131" y2="125" />
                  <line x1="96" y1="130" x2="124" y2="130" />
                </g>
              </g>

              <!-- Curved White Ribbon Banner Across Bottom -->
              <!-- Left & Right Ribbon Tails -->
              <path d="M 45 147 L 12 141 L 20 154 L 12 166 L 48 161 Z" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2" stroke-linejoin="round" />
              <path d="M 175 147 L 208 141 L 200 154 L 208 166 L 172 161 Z" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2" stroke-linejoin="round" />
              <!-- Ribbon Fold Shadows -->
              <path d="M 45 158 L 52 165 L 48 161 Z" fill="#1E3A8A" />
              <path d="M 175 158 L 168 165 L 172 161 Z" fill="#1E3A8A" />
              <!-- Main Center Curved Banner -->
              <path d="M 44 139 Q 110 121 176 139 L 172 160 Q 110 142 48 160 Z" fill="#FFFFFF" stroke="#1E3A8A" stroke-width="2.2" stroke-linejoin="round" />

              <!-- Bold Dark Navy Text "WORK IS WORSHIP" inside Ribbon -->
              <text fill="#1E293B" font-family="Plus Jakarta Sans, Inter, sans-serif" font-weight="800" font-size="11.5" letter-spacing="0.7">
                <textPath href="#mitsRibbonArc" startOffset="50%" text-anchor="middle">
                  WORK IS WORSHIP
                </textPath>
              </text>

              <!-- Brown/Gold Text "Estd. 1957" Centered Directly Beneath Ribbon -->
              <text x="110" y="182" text-anchor="middle" fill="#854D0E" font-family="Plus Jakarta Sans, Inter, sans-serif" font-weight="800" font-size="14">
                Estd. 1957
              </text>
            </svg>
          </div>
          <div>
            <div class="flex flex-wrap items-center gap-2">
              <span class="px-2 py-0.5 text-[10px] font-bold uppercase tracking-wider bg-amber-500 text-mits-navy rounded-md">Deemed University</span>
              <span class="text-xs text-amber-200 font-medium hidden sm:inline">Gwalior, Madhya Pradesh • Est. 1957</span>
            </div>
            <h1 class="font-display text-lg sm:text-xl lg:text-2xl font-extrabold tracking-tight text-white leading-tight mt-0.5">
              Madhav Institute of Technology & Science
            </h1>
            <p class="text-xs sm:text-sm text-slate-300 font-medium">
              Central Library Book Management & Peer Doubt-Sharing Portal
            </p>
          </div>
        </div>

        <!-- Right Quick Controls & Fine Rate Pill -->
        <div class="flex flex-wrap items-center gap-2.5 self-start md:self-center">
          <button onclick="openUpiQrModal()" class="bg-emerald-500/20 hover:bg-emerald-500/30 text-white border border-emerald-400/40 px-3 py-1.5 rounded-xl text-xs font-semibold transition flex items-center gap-1.5 cursor-pointer" title="View UPI ID & Scan QR">
            <i class="fa-solid fa-qrcode text-emerald-300"></i>
            <span>UPI: <strong class="text-amber-300 font-mono">9755715173@ibl</strong></span>
          </button>
          <div class="bg-white/10 border border-white/15 rounded-xl px-3 py-1.5 text-xs flex items-center gap-2">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
            <span class="text-slate-200">Late Fine: <strong class="text-amber-300">₹15 / day</strong></span>
          </div>
          <button onclick="confirmResetData()" class="bg-white/10 hover:bg-white/20 text-white border border-white/20 px-3 py-1.5 rounded-xl text-xs font-semibold transition flex items-center gap-1.5 cursor-pointer" title="Restore initial 10 students, 60 books & sample borrows">
            <i class="fa-solid fa-rotate-left text-amber-400"></i>
            <span>Reset Demo</span>
          </button>
        </div>
      </div>

      <!-- Navigation Tabs -->
      <nav class="flex gap-1.5 overflow-x-auto pt-4 pb-1 no-scrollbar" aria-label="Library Sections">
        <button onclick="switchTab('dashboard')" id="tab-btn-dashboard" class="nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 bg-amber-500 text-mits-navy shadow-sm cursor-pointer">
          <i class="fa-solid fa-gauge-high"></i> Dashboard & Issue
        </button>
        <button onclick="switchTab('catalog')" id="tab-btn-catalog" class="nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 text-slate-200 hover:bg-white/10 cursor-pointer">
          <i class="fa-solid fa-book"></i> Book Catalog <span id="nav-badge-books" class="bg-white/15 px-1.5 py-0.2 rounded-md text-[11px]">60</span>
        </button>
        <button onclick="switchTab('students')" id="tab-btn-students" class="nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 text-slate-200 hover:bg-white/10 cursor-pointer">
          <i class="fa-solid fa-user-graduate"></i> Students & Subscription <span class="bg-white/15 px-1.5 py-0.2 rounded-md text-[11px]">10</span>
        </button>
        <button onclick="switchTab('borrows')" id="tab-btn-borrows" class="nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 text-slate-200 hover:bg-white/10 cursor-pointer">
          <i class="fa-solid fa-clock-rotate-left"></i> Borrows & Fines <span id="nav-badge-borrows" class="bg-amber-400/20 text-amber-300 px-1.5 py-0.2 rounded-md text-[11px]">0</span>
        </button>
        <button onclick="switchTab('groups')" id="tab-btn-groups" class="nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 text-slate-200 hover:bg-white/10 cursor-pointer">
          <i class="fa-solid fa-users-rectangle"></i> Peer Study Groups <span id="nav-badge-groups" class="bg-emerald-400/20 text-emerald-300 px-1.5 py-0.2 rounded-md text-[11px]">0</span>
        </button>
      </nav>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

    <!-- Top KPI Stat Cards -->
    <section class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-3 sm:gap-4 mb-6">
      <div class="bg-white rounded-2xl p-4 border border-slate-200/80 shadow-xs flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500">Total Books</p>
          <p id="stat-total-books" class="text-2xl font-display font-extrabold text-mits-navy mt-0.5">60</p>
          <p class="text-[11px] text-slate-400 mt-0.5">6 Engineering Subjects</p>
        </div>
        <div class="w-10 h-10 rounded-xl bg-blue-50 text-blue-700 flex items-center justify-center text-lg">
          <i class="fa-solid fa-books fa-book-open"></i>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-4 border border-slate-200/80 shadow-xs flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500">MITS-DU Students</p>
          <p id="stat-total-students" class="text-2xl font-display font-extrabold text-mits-navy mt-0.5">10</p>
          <p class="text-[11px] text-emerald-600 font-medium mt-0.5">General Sub Active</p>
        </div>
        <div class="w-10 h-10 rounded-xl bg-emerald-50 text-emerald-700 flex items-center justify-center text-lg">
          <i class="fa-solid fa-id-card"></i>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-4 border border-slate-200/80 shadow-xs flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500">Active Borrows</p>
          <p id="stat-active-borrows" class="text-2xl font-display font-extrabold text-mits-navy mt-0.5">0</p>
          <p id="stat-overdue-count" class="text-[11px] text-rose-600 font-medium mt-0.5">0 Overdue</p>
        </div>
        <div class="w-10 h-10 rounded-xl bg-amber-50 text-amber-600 flex items-center justify-center text-lg">
          <i class="fa-solid fa-hand-holding-hand"></i>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-4 border border-slate-200/80 shadow-xs flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500">Peer Study Groups</p>
          <p id="stat-study-groups" class="text-2xl font-display font-extrabold text-indigo-700 mt-0.5">0</p>
          <p class="text-[11px] text-indigo-500 font-medium mt-0.5">Same-Book Readers</p>
        </div>
        <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-lg">
          <i class="fa-solid fa-people-group"></i>
        </div>
      </div>

      <div class="col-span-2 sm:col-span-1 bg-white rounded-2xl p-4 border border-slate-200/80 shadow-xs flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500">Pending Fines</p>
          <p id="stat-total-fines" class="text-2xl font-display font-extrabold text-rose-600 mt-0.5">₹0</p>
          <p class="text-[11px] text-slate-400 mt-0.5">@ ₹15 / extra day</p>
        </div>
        <div class="w-10 h-10 rounded-xl bg-rose-50 text-rose-600 flex items-center justify-center text-lg">
          <i class="fa-solid fa-indian-rupee-sign"></i>
        </div>
      </div>
    </section>

    <!-- ==================== TAB 1: DASHBOARD & ISSUE BOOK ==================== -->
    <div id="panel-dashboard" class="tab-panel space-y-6">
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
        <!-- Issue Book Form Card -->
        <div class="lg:col-span-5 bg-white rounded-2xl border border-slate-200 shadow-xs p-5 sm:p-6">
          <div class="flex items-center gap-3 pb-4 mb-5 border-b border-slate-100">
            <div class="w-10 h-10 rounded-xl bg-mits-navy text-amber-400 flex items-center justify-center text-lg">
              <i class="fa-solid fa-book-medical"></i>
            </div>
            <div>
              <h2 class="font-display font-bold text-lg text-slate-900">Issue Library Book</h2>
              <p class="text-xs text-slate-500">Assign book, set allowed days & auto-match peer groups</p>
            </div>
          </div>

          <form id="issue-book-form" onsubmit="handleIssueBook(event)" class="space-y-4">
            <!-- Select Student -->
            <div>
              <label for="issue-student" class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1.5">
                Select MITS-DU Student
              </label>
              <select id="issue-student" required onchange="updateSelectedStudentPreview()" class="w-full rounded-xl border border-slate-300 bg-slate-50 px-3.5 py-2.5 text-sm text-slate-800 focus:border-mits-royal focus:bg-white focus:outline-none">
                <!-- Populated dynamically -->
              </select>
              <div id="selected-student-info" class="mt-2 p-2.5 rounded-xl bg-slate-50 border border-slate-200/80 text-xs flex items-center justify-between">
                <!-- Dynamic preview -->
              </div>
            </div>

            <!-- Filter Subject for Quick Book Picking -->
            <div>
              <label for="issue-subject-filter" class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1.5">
                Filter Subject (60 Books Available)
              </label>
              <select id="issue-subject-filter" onchange="populateIssueBookDropdown()" class="w-full rounded-xl border border-slate-300 bg-slate-50 px-3.5 py-2 text-sm text-slate-800 focus:border-mits-royal focus:bg-white focus:outline-none">
                <option value="ALL">All 6 Subjects (60 Books)</option>
                <option value="Electronic Devices">Electronic Devices (10 Books)</option>
                <option value="Engineering Mathematics">Engineering Mathematics (10 Books)</option>
                <option value="Fundamentals of C++">Fundamentals of C++ (10 Books)</option>
                <option value="Engineering Physics">Engineering Physics (10 Books)</option>
                <option value="Computer Programming">Computer Programming (10 Books)</option>
                <option value="Digital Sensors">Digital Sensors (10 Books)</option>
              </select>
            </div>

            <!-- Select Book -->
            <div>
              <label for="issue-book" class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1.5">
                Select Book Title / Copy ID
              </label>
              <select id="issue-book" required class="w-full rounded-xl border border-slate-300 bg-slate-50 px-3.5 py-2.5 text-sm text-slate-800 focus:border-mits-royal focus:bg-white focus:outline-none">
                <!-- Populated dynamically -->
              </select>
              <p class="text-[11px] text-indigo-600 mt-1 flex items-center gap-1">
                <i class="fa-solid fa-circle-info"></i>
                <span>Tip: If 2+ students borrow the same book title, they join a Peer Doubt Group!</span>
              </p>
            </div>

            <!-- Days Requested & Simulated Elapsed Days -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label for="issue-days" class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1.5">
                  Borrow Duration (Days)
                </label>
                <input type="number" id="issue-days" min="1" max="60" value="7" required class="w-full rounded-xl border border-slate-300 bg-slate-50 px-3.5 py-2.5 text-sm font-semibold text-slate-800 focus:border-mits-royal focus:bg-white focus:outline-none" />
              </div>
              <div>
                <label for="issue-elapsed" class="block text-xs font-semibold text-slate-700 uppercase tracking-wider mb-1.5">
                  Days Elapsed So Far
                </label>
                <input type="number" id="issue-elapsed" min="0" max="120" value="0" required class="w-full rounded-xl border border-slate-300 bg-slate-50 px-3.5 py-2.5 text-sm font-semibold text-slate-800 focus:border-mits-royal focus:bg-white focus:outline-none" />
              </div>
            </div>

            <div class="p-3 rounded-xl bg-amber-50 border border-amber-200/80 text-xs text-amber-900 flex items-start gap-2.5">
              <i class="fa-solid fa-scale-balanced text-amber-600 mt-0.5"></i>
              <div>
                <strong>MITS-DU Fine Policy:</strong> General Subscription covers regular borrowing within allowed days. Exceeding allowed days incurs <strong>₹15/day</strong> late fine.
              </div>
            </div>

            <button type="submit" class="w-full bg-mits-navy hover:bg-mits-royal text-white font-semibold py-3 px-4 rounded-xl shadow-md hover:shadow-lg transition flex items-center justify-center gap-2 cursor-pointer">
              <i class="fa-solid fa-check-circle text-amber-400"></i>
              <span>Confirm Book Issue</span>
            </button>
          </form>
        </div>

        <!-- Right Side: Live Active Borrows Preview & Peer Groups Highlight -->
        <div class="lg:col-span-7 space-y-6">
          <!-- Active Peer Study Groups Highlight -->
          <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5 sm:p-6">
            <div class="flex flex-wrap items-center justify-between gap-2 pb-4 mb-4 border-b border-slate-100">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-indigo-100 text-indigo-700 flex items-center justify-center text-lg">
                  <i class="fa-solid fa-comments"></i>
                </div>
                <div>
                  <h2 class="font-display font-bold text-lg text-slate-900">Active Same-Book Study Groups</h2>
                  <p class="text-xs text-slate-500">Auto-created when multiple students borrow the same book</p>
                </div>
              </div>
              <button onclick="switchTab('groups')" class="text-xs font-semibold text-indigo-600 hover:text-indigo-800 bg-indigo-50 px-3 py-1.5 rounded-lg transition cursor-pointer">
                Open Doubt Hub <i class="fa-solid fa-arrow-right ml-1"></i>
              </button>
            </div>
            <div id="dashboard-groups-preview" class="space-y-3">
              <!-- Populated dynamically -->
            </div>
          </div>

          <!-- Recent Borrow & Fine Status -->
          <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5 sm:p-6">
            <div class="flex flex-wrap items-center justify-between gap-2 pb-4 mb-4 border-b border-slate-100">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-amber-100 text-amber-700 flex items-center justify-center text-lg">
                  <i class="fa-solid fa-clock"></i>
                </div>
                <div>
                  <h2 class="font-display font-bold text-lg text-slate-900">Active Loans & ₹15/Day Fine Monitor</h2>
                  <p class="text-xs text-slate-500">Update borrow days or return books to settle fines</p>
                </div>
              </div>
              <button onclick="switchTab('borrows')" class="text-xs font-semibold text-amber-700 hover:text-amber-900 bg-amber-50 px-3 py-1.5 rounded-lg transition cursor-pointer">
                Manage All <i class="fa-solid fa-arrow-right ml-1"></i>
              </button>
            </div>
            <div id="dashboard-borrows-preview" class="space-y-3">
              <!-- Populated dynamically -->
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ==================== TAB 2: BOOK CATALOG (60 BOOKS) ==================== -->
    <div id="panel-catalog" class="tab-panel hidden space-y-5">
      <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <h2 class="font-display font-bold text-xl text-slate-900">MITS-DU Book Inventory (60 Volumes)</h2>
            <p class="text-xs text-slate-500 mt-0.5">10 books each across Electronic Devices, Engineering Mathematics, Fundamentals of C++, Engineering Physics, Computer Programming & Digital Sensors</p>
          </div>
          <div class="relative w-full md:w-72">
            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-xs"></i>
            <input type="text" id="catalog-search" oninput="renderCatalog()" placeholder="Search book title, ID, author..." class="w-full pl-9 pr-4 py-2 rounded-xl border border-slate-300 text-sm focus:border-mits-royal focus:outline-none" />
          </div>
        </div>

        <!-- Subject Filter Pills -->
        <div id="subject-filter-pills" class="flex flex-wrap gap-2 mt-4 pt-4 border-t border-slate-100">
          <!-- Populated dynamically -->
        </div>
      </div>

      <!-- Books Grid -->
      <div id="catalog-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <!-- Populated dynamically -->
      </div>
    </div>

    <!-- ==================== TAB 3: STUDENTS & SUBSCRIPTIONS ==================== -->
    <div id="panel-students" class="tab-panel hidden space-y-5">
      <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
        <div>
          <h2 class="font-display font-bold text-xl text-slate-900">Registered Students & General Subscription</h2>
          <p class="text-xs text-slate-500 mt-0.5">Batch BTEL26O1001 – BTEL26O1010 (2 Students per Engineering Department)</p>
        </div>
        <div class="flex flex-wrap items-center gap-2 text-xs">
          <span class="px-3 py-1.5 rounded-xl bg-emerald-50 text-emerald-700 font-semibold border border-emerald-200">
            <i class="fa-solid fa-circle-check mr-1"></i> General Subscription Included
          </span>
        </div>
      </div>

      <div id="students-grid" class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <!-- Populated dynamically -->
      </div>
    </div>

    <!-- ==================== TAB 4: ACTIVE BORROWS & FINE TRACKER ==================== -->
    <div id="panel-borrows" class="tab-panel hidden space-y-5">
      <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5 flex flex-col md:flex-row md:items-center justify-between gap-4">
        <div>
          <h2 class="font-display font-bold text-xl text-slate-900">Borrowed Books & Overdue Fine Ledger</h2>
          <p class="text-xs text-slate-500 mt-0.5">Update allowed days, simulate +1 day elapsed to test ₹15/day fine, or process book returns</p>
        </div>
        <div class="flex flex-wrap gap-2">
          <button onclick="simulateAdvanceAllDays(1)" class="bg-amber-500 hover:bg-amber-600 text-mits-navy font-bold text-xs px-3.5 py-2 rounded-xl shadow-xs transition flex items-center gap-1.5 cursor-pointer">
            <i class="fa-solid fa-forward-step"></i> Simulate +1 Day for All
          </button>
          <button onclick="switchTab('dashboard')" class="bg-mits-navy hover:bg-mits-royal text-white font-semibold text-xs px-3.5 py-2 rounded-xl transition flex items-center gap-1.5 cursor-pointer">
            <i class="fa-solid fa-plus"></i> Issue New Book
          </button>
        </div>
      </div>

      <div id="borrows-list-container" class="space-y-3">
        <!-- Populated dynamically -->
      </div>
    </div>

    <!-- ==================== TAB 5: PEER STUDY GROUPS & EMAIL SHARING ==================== -->
    <div id="panel-groups" class="tab-panel hidden space-y-5">
      <div class="bg-gradient-to-r from-indigo-900 via-mits-royal to-mits-navy text-white rounded-2xl p-6 shadow-md">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <span class="px-2.5 py-1 rounded-lg bg-amber-400/20 text-amber-300 text-xs font-bold uppercase tracking-wider">Collaborative Learning</span>
            <h2 class="font-display font-bold text-xl sm:text-2xl mt-2">Same-Book Peer Study Groups & Doubt Network</h2>
            <p class="text-xs sm:text-sm text-slate-300 mt-1 max-w-2xl">
              Whenever two or more MITS-DU students borrow the exact same book title, the library automatically places them in a shared study group and unlocks their institutional email IDs so they can ask doubts and discuss chapters together.
            </p>
          </div>
          <div class="bg-white/10 border border-white/15 rounded-2xl p-4 text-center shrink-0">
            <p class="text-xs text-slate-300">Active Study Circles</p>
            <p id="groups-hero-count" class="text-3xl font-display font-extrabold text-amber-400 mt-0.5">0</p>
          </div>
        </div>
      </div>

      <div id="groups-list-container" class="grid grid-cols-1 lg:grid-cols-2 gap-5">
        <!-- Populated dynamically -->
      </div>
    </div>

  </main>

  <!-- Footer -->
  <footer class="bg-white border-t border-slate-200 mt-12 py-5 text-center text-xs text-slate-500">
    <div class="max-w-7xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-2">
      <p class="font-semibold text-slate-700">MITS-DU Central Library Management System • Gwalior (M.P.)</p>
      <p>Overdue Penalty: <span class="font-semibold text-rose-600">₹15/day</span> • General Subscription Active for Enrolled Students</p>
    </div>
  </footer>

  <!-- ==================== JAVASCRIPT APPLICATION LOGIC ==================== -->
  <script>
    // 1. INITIAL STUDENTS DATA (10 Students: BTEL26O1001 to BTEL26O1010, 2 per branch)
    const INITIAL_STUDENTS = [
      { id: 'BTEL26O1001', name: 'Krishna', branch: 'Electronics Engineering', email: 'krishna.btel26o1001@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1002', name: 'Anoop', branch: 'Electronics Engineering', email: 'anoop.btel26o1002@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1003', name: 'Anupam', branch: 'Computer Engineering', email: 'anupam.btel26o1003@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1004', name: 'Anuj', branch: 'Computer Engineering', email: 'anuj.btel26o1004@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1005', name: 'Ankita', branch: 'Civil Engineering', email: 'ankita.btel26o1005@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1006', name: 'Sukanya', branch: 'Civil Engineering', email: 'sukanya.btel26o1006@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1007', name: 'Piyush', branch: 'Mechanical Engineering', email: 'piyush.btel26o1007@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1008', name: 'Shubh', branch: 'Mechanical Engineering', email: 'shubh.btel26o1008@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1009', name: 'Yogi', branch: 'Electrical Engineering', email: 'yogi.btel26o1009@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' },
      { id: 'BTEL26O1010', name: 'Soham', branch: 'Electrical Engineering', email: 'soham.btel26o1010@mitsgwalior.in', subscriptionActive: true, subValidTill: '31 May 2027' }
    ];

    // 2. INITIAL 60 BOOKS DATA (10 books per subject across 6 subjects)
    // Giving each book 4 copies so multiple students can borrow the exact same book title to form Peer Study Groups
    function buildInitialBooks() {
      const catalogBlueprints = [
        {
          subject: 'Electronic Devices',
          prefix: 'ED',
          items: [
            { title: 'Electronic Devices and Circuit Theory', author: 'Robert L. Boylestad & Louis Nashelsky' },
            { title: 'Integrated Electronics: Analog & Digital', author: 'Jacob Millman & Christos Halkias' },
            { title: 'Solid State Electronic Devices', author: 'Ben G. Streetman & Sanjay Banerjee' },
            { title: 'Microelectronic Circuits', author: 'Adel S. Sedra & Kenneth C. Smith' },
            { title: 'Electronic Principles', author: 'Albert Malvino & David Bates' },
            { title: 'Semiconductor Physics and Devices', author: 'Donald A. Neamen' },
            { title: 'Basic Electronics & Linear Circuits', author: 'N.N. Bhargava & D.C. Kulshreshtha' },
            { title: 'Fundamentals of Microelectronics', author: 'Behzad Razavi' },
            { title: 'Electronic Devices and Circuits', author: 'J.B. Gupta' },
            { title: 'Op-Amps and Linear Integrated Circuits', author: 'Ramakant A. Gayakwad' }
          ]
        },
        {
          subject: 'Engineering Mathematics',
          prefix: 'EM',
          items: [
            { title: 'Higher Engineering Mathematics', author: 'Dr. B.S. Grewal' },
            { title: 'Advanced Engineering Mathematics', author: 'Erwin Kreyszig' },
            { title: 'Engineering Mathematics Vol. I', author: 'H.K. Dass' },
            { title: 'A Textbook of Engineering Mathematics', author: 'N.P. Bali & Manish Goyal' },
            { title: 'Linear Algebra and Its Applications', author: 'Gilbert Strang' },
            { title: 'Differential Equations & Transforms', author: 'S.L. Ross' },
            { title: 'Probability and Statistics for Engineers', author: 'Ronald E. Walpole' },
            { title: 'Numerical Methods in Engineering', author: 'B.S. Grewal' },
            { title: 'Complex Variables and Applications', author: 'James Ward Brown & Ruel V. Churchill' },
            { title: 'Discrete Mathematics and Its Applications', author: 'Kenneth H. Rosen' }
          ]
        },
        {
          subject: 'Fundamentals of C++',
          prefix: 'CPP',
          items: [
            { title: 'Object-Oriented Programming with C++', author: 'E. Balagurusamy' },
            { title: 'The C++ Programming Language', author: 'Bjarne Stroustrup' },
            { title: 'C++: The Complete Reference', author: 'Herbert Schildt' },
            { title: 'C++ Primer (5th Edition)', author: 'Stanley B. Lippman' },
            { title: 'Effective Modern C++', author: 'Scott Meyers' },
            { title: 'Object-Oriented Programming in C++', author: 'Robert Lafore' },
            { title: 'Let Us C++', author: 'Yashavant Kanetkar' },
            { title: 'Programming: Principles and Practice Using C++', author: 'Bjarne Stroustrup' },
            { title: 'Data Structures and Algorithms in C++', author: 'Adam Drozdek' },
            { title: 'Mastering C++', author: 'K.R. Venugopal' }
          ]
        },
        {
          subject: 'Engineering Physics',
          prefix: 'EP',
          items: [
            { title: 'A Textbook of Engineering Physics', author: 'M.N. Avadhanulu & P.G. Kshirsagar' },
            { title: 'Engineering Physics: Theory & Practical', author: 'R.K. Gaur & S.L. Gupta' },
            { title: 'Engineering Physics', author: 'Hitendra K. Malik & A.K. Singh' },
            { title: 'Concepts of Modern Physics', author: 'Arthur Beiser' },
            { title: 'Optics and Fiber Communication', author: 'Ajoy Ghatak' },
            { title: 'Introduction to Solid State Physics', author: 'Charles Kittel' },
            { title: 'Quantum Mechanics for Engineers', author: 'D.J. Griffiths' },
            { title: 'Laser Systems and Applications', author: 'William T. Silfvast' },
            { title: 'Applied Physics for Engineers', author: 'Neeraj Mehta' },
            { title: 'Electromagnetic Waves & Fields', author: 'David K. Cheng' }
          ]
        },
        {
          subject: 'Computer Programming',
          prefix: 'CP',
          items: [
            { title: 'Let Us C', author: 'Yashavant Kanetkar' },
            { title: 'The C Programming Language', author: 'Brian W. Kernighan & Dennis M. Ritchie' },
            { title: 'Programming in ANSI C', author: 'E. Balagurusamy' },
            { title: 'Introduction to Algorithms (CLRS)', author: 'Thomas H. Cormen' },
            { title: 'Computer Fundamentals and Programming in C', author: 'Reema Thareja' },
            { title: 'How to Solve It by Computer', author: 'R.G. Dromey' },
            { title: 'Data Structures Using C', author: 'Aaron M. Tenenbaum' },
            { title: 'Head First C', author: 'David Griffiths & Dawn Griffiths' },
            { title: 'Problem Solving and Program Design in C', author: 'Jeri R. Hanly' },
            { title: 'Clean Code & Structured Programming', author: 'Robert C. Martin' }
          ]
        },
        {
          subject: 'Digital Sensors',
          prefix: 'DS',
          items: [
            { title: 'Handbook of Modern Sensors', author: 'Jacob Fraden' },
            { title: 'Sensors and Transducers', author: 'D. Patranabis' },
            { title: 'Sensors and Signal Conditioning', author: 'Ramon Pallas-Areny & John G. Webster' },
            { title: 'Smart Sensors and MEMS', author: 'Stoyan Nihtianov' },
            { title: 'Measurement Systems: Application & Design', author: 'Ernest O. Doebelin' },
            { title: 'Industrial Instrumentation and Control', author: 'S.K. Singh' },
            { title: 'IoT Sensors and Actuator Networks', author: 'Daniel Minoli' },
            { title: 'Optical Fiber Sensors: Principles', author: 'John Dakin & Brian Culshaw' },
            { title: 'Biosensors and Chemical Sensors', author: 'Brian R. Eggins' },
            { title: 'Digital Interfacing with Smart Sensors', author: 'V.K. Mehta' }
          ]
        }
      ];

      const books = [];
      catalogBlueprints.forEach(group => {
        group.items.forEach((item, idx) => {
          const num = (idx + 1) * 100 + 1;
          books.push({
            id: `${group.prefix}-${101 + idx}`,
            subject: group.subject,
            title: item.title,
            author: item.author,
            totalCopies: 4
          });
        });
      });
      return books;
    }

    // 3. PRE-POPULATED BORROW RECORDS (Demonstrates Same-Book Peer Groups & ₹15/day Fine immediately)
    const INITIAL_BORROWS = [
      {
        id: 'BRW-1001',
        studentId: 'BTEL26O1001', // Krishna (Electronics)
        bookId: 'ED-101',         // Electronic Devices and Circuit Theory
        borrowDate: '2026-11-18',
        allowedDays: 7,
        elapsedDays: 5            // Within limit (₹0 fine)
      },
      {
        id: 'BRW-1002',
        studentId: 'BTEL26O1002', // Anoop (Electronics) - SAME BOOK AS KRISHNA!
        bookId: 'ED-101',         // Electronic Devices and Circuit Theory
        borrowDate: '2026-11-19',
        allowedDays: 7,
        elapsedDays: 4            // Within limit (₹0 fine)
      },
      {
        id: 'BRW-1003',
        studentId: 'BTEL26O1003', // Anupam (Computer)
        bookId: 'CPP-101',        // Object-Oriented Programming with C++
        borrowDate: '2026-11-12',
        allowedDays: 7,
        elapsedDays: 11           // 4 extra days overdue => 4 * ₹15 = ₹60 fine!
      },
      {
        id: 'BRW-1004',
        studentId: 'BTEL26O1004', // Anuj (Computer) - SAME BOOK AS ANUPAM!
        bookId: 'CPP-101',        // Object-Oriented Programming with C++
        borrowDate: '2026-11-17',
        allowedDays: 10,
        elapsedDays: 6            // Within limit (₹0 fine)
      },
      {
        id: 'BRW-1005',
        studentId: 'BTEL26O1009', // Yogi (Electrical)
        bookId: 'DS-101',         // Handbook of Modern Sensors
        borrowDate: '2026-11-14',
        allowedDays: 5,
        elapsedDays: 8            // 3 extra days overdue => 3 * ₹15 = ₹45 fine!
      },
      {
        id: 'BRW-1006',
        studentId: 'BTEL26O1010', // Soham (Electrical) - SAME BOOK AS YOGI!
        bookId: 'DS-101',         // Handbook of Modern Sensors
        borrowDate: '2026-11-18',
        allowedDays: 7,
        elapsedDays: 4            // Within limit (₹0 fine)
      }
    ];

    const INITIAL_DOUBT_NOTES = {
      'ED-101': [
        { sender: 'Krishna (BTEL26O1001)', text: 'Hey Anoop, did you solve Exercise 4.12 on BJT DC Biasing from Boylestad?' },
        { sender: 'Anoop (BTEL26O1002)', text: 'Yes! Check page 168—load line intersection gives Q-point at 2.4mA. Let us compare notes after lab.' }
      ],
      'CPP-101': [
        { sender: 'Anupam (BTEL26O1003)', text: 'Anyone stuck on Virtual Base Class ambiguity in Chapter 8 of Balagurusamy?' },
        { sender: 'Anuj (BTEL26O1004)', text: 'Using the `virtual` keyword during hybrid inheritance resolves duplicate base copies. Emailed you the snippet!' }
      ]
    };

    const FINE_PER_DAY = 15;
    const LIBRARY_UPI_ID = '9755715173@ibl';
    const STORAGE_KEY = 'mits_du_library_state_v2';

    let state = {
      students: [],
      books: [],
      borrows: [],
      doubtNotes: {},
      selectedSubject: 'ALL',
      activeTab: 'dashboard'
    };

    // Load or initialize state
    function loadState() {
      try {
        const saved = localStorage.getItem(STORAGE_KEY);
        if (saved) {
          const parsed = JSON.parse(saved);
          if (parsed && Array.isArray(parsed.students) && Array.isArray(parsed.books)) {
            state = parsed;
            return;
          }
        }
      } catch (e) {
        console.warn('Could not read localStorage, using defaults.', e);
      }
      resetToDefaults(false);
    }

    function saveState() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      } catch (e) {
        console.warn('Could not save state to localStorage.', e);
      }
    }

    function resetToDefaults(notify = true) {
      state = {
        students: JSON.parse(JSON.stringify(INITIAL_STUDENTS)),
        books: buildInitialBooks(),
        borrows: JSON.parse(JSON.stringify(INITIAL_BORROWS)),
        doubtNotes: JSON.parse(JSON.stringify(INITIAL_DOUBT_NOTES)),
        selectedSubject: 'ALL',
        activeTab: state.activeTab || 'dashboard'
      };
      saveState();
      renderAll();
      if (notify) {
        showToast('Library database reset to MITS-DU initial state.', 'info');
      }
    }

    function confirmResetData() {
      openModal({
        title: 'Reset Demo Data?',
        subtitle: 'Restore 10 MITS-DU students, 60 books & sample borrows',
        icon: 'fa-rotate-left',
        iconBg: 'bg-amber-100 text-amber-700',
        bodyHtml: `
          <p class="text-sm text-slate-600">This will restore all 60 books, reset student subscriptions, and reload the default borrow & peer study group examples.</p>
          <div class="flex justify-end gap-2 pt-3">
            <button onclick="closeModal()" class="px-4 py-2 rounded-xl border border-slate-300 text-xs font-semibold text-slate-700 hover:bg-slate-50 cursor-pointer">Cancel</button>
            <button onclick="resetToDefaults(true); closeModal();" class="px-4 py-2 rounded-xl bg-mits-navy text-white text-xs font-semibold hover:bg-mits-royal cursor-pointer">Yes, Reset Data</button>
          </div>
        `
      });
    }

    // Helper calculations
    function calculateFine(borrow) {
      const extraDays = Math.max(0, borrow.elapsedDays - borrow.allowedDays);
      return {
        extraDays,
        fineAmount: extraDays * FINE_PER_DAY,
        isOverdue: extraDays > 0
      };
    }

    function getAvailableCopies(bookId) {
      const book = state.books.find(b => b.id === bookId);
      if (!book) return 0;
      const borrowedCount = state.borrows.filter(br => br.bookId === bookId).length;
      return Math.max(0, book.totalCopies - borrowedCount);
    }

    // Compute Peer Study Groups (books borrowed by >= 2 distinct students)
    function computePeerStudyGroups() {
      const byBook = {};
      state.borrows.forEach(br => {
        if (!byBook[br.bookId]) byBook[br.bookId] = [];
        const student = state.students.find(s => s.id === br.studentId);
        if (student && !byBook[br.bookId].some(m => m.id === student.id)) {
          byBook[br.bookId].push({
            ...student,
            borrowId: br.id,
            allowedDays: br.allowedDays,
            elapsedDays: br.elapsedDays
          });
        }
      });

      const groups = [];
      Object.keys(byBook).forEach(bookId => {
        if (byBook[bookId].length >= 2) {
          const book = state.books.find(b => b.id === bookId);
          if (book) {
            groups.push({
              book,
              members: byBook[bookId]
            });
          }
        }
      });
      return groups;
    }

    // UI Toast Helper
    function showToast(message, type = 'success') {
      const container = document.getElementById('toast-container');
      const colors = {
        success: 'bg-emerald-900 text-white border-emerald-500',
        warning: 'bg-amber-900 text-white border-amber-400',
        error: 'bg-rose-900 text-white border-rose-500',
        info: 'bg-mits-navy text-white border-amber-400'
      };
      const icons = {
        success: 'fa-circle-check text-emerald-400',
        warning: 'fa-triangle-exclamation text-amber-400',
        error: 'fa-circle-exclamation text-rose-400',
        info: 'fa-circle-info text-amber-400'
      };

      const toast = document.createElement('div');
      toast.className = `pointer-events-auto flex items-center gap-3 px-4 py-3 rounded-xl border-l-4 shadow-xl text-xs font-medium transition-all transform translate-y-2 opacity-0 ${colors[type] || colors.info}`;
      toast.innerHTML = `
        <i class="fa-solid ${icons[type] || icons.info} text-base shrink-0"></i>
        <span class="flex-1 leading-snug">${message}</span>
      `;
      container.appendChild(toast);

      requestAnimationFrame(() => {
        toast.classList.remove('translate-y-2', 'opacity-0');
      });

      setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
      }, 3600);
    }

    // Modal Helpers
    function openModal({ title, subtitle, icon, iconBg, bodyHtml }) {
      document.getElementById('modal-title').textContent = title;
      document.getElementById('modal-subtitle').textContent = subtitle;
      const iconWrap = document.getElementById('modal-icon-wrap');
      iconWrap.className = `w-10 h-10 rounded-xl flex items-center justify-center text-lg ${iconBg || 'bg-amber-100 text-amber-700'}`;
      iconWrap.innerHTML = `<i class="fa-solid ${icon || 'fa-pen-to-square'}"></i>`;
      document.getElementById('modal-body').innerHTML = bodyHtml;

      const backdrop = document.getElementById('modal-backdrop');
      backdrop.classList.remove('hidden');
      backdrop.classList.add('flex');
    }

    function closeModal() {
      const backdrop = document.getElementById('modal-backdrop');
      backdrop.classList.add('hidden');
      backdrop.classList.remove('flex');
    }

    // Simple UPI ID & QR Modal
    function openUpiQrModal(amount = 0, note = 'MITS-DU Library Fine') {
      const amtParam = amount > 0 ? `&am=${Number(amount).toFixed(2)}` : '';
      const upiLink = `upi://pay?pa=${encodeURIComponent(LIBRARY_UPI_ID)}&pn=${encodeURIComponent('MITS-DU Central Library')}${amtParam}&cu=INR&tn=${encodeURIComponent(note)}`;

      openModal({
        title: 'MITS-DU Library UPI & QR',
        subtitle: amount > 0 ? `Amount: ₹${amount} • ${note}` : 'Scan QR or copy UPI ID to pay library dues',
        icon: 'fa-qrcode',
        iconBg: 'bg-emerald-100 text-emerald-700',
        bodyHtml: `
          <div class="flex flex-col items-center text-center space-y-3">
            <div class="p-3 bg-white rounded-2xl border-2 border-slate-200 shadow-xs">
              <canvas id="mits-upi-qr-canvas" width="180" height="180"></canvas>
            </div>
            ${amount > 0 ? `<div class="px-3 py-1 rounded-lg bg-amber-50 border border-amber-200 text-xs font-bold text-amber-900">Payable Fine: ₹${amount}</div>` : ''}
            <div class="w-full p-2.5 rounded-xl bg-slate-50 border border-slate-200 flex items-center justify-between gap-2">
              <div class="text-left">
                <span class="text-[10px] uppercase font-bold text-slate-400 block">UPI ID</span>
                <span class="font-mono text-xs font-bold text-slate-900">${LIBRARY_UPI_ID}</span>
              </div>
              <button type="button" onclick="copyUpiId()" class="px-3 py-1.5 rounded-lg bg-mits-navy hover:bg-mits-royal text-white text-xs font-semibold cursor-pointer">
                <i class="fa-regular fa-copy mr-1"></i> Copy ID
              </button>
            </div>
            <div class="flex w-full gap-2 pt-1">
              <a href="${upiLink}" class="flex-1 py-2 px-3 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-semibold text-center">
                <i class="fa-solid fa-mobile-screen-button mr-1"></i> Open UPI App
              </a>
              <button type="button" onclick="closeModal()" class="px-4 py-2 rounded-xl border border-slate-300 text-xs font-semibold text-slate-700 hover:bg-slate-50 cursor-pointer">
                Close
              </button>
            </div>
          </div>
        `
      });

      setTimeout(() => {
        const canvas = document.getElementById('mits-upi-qr-canvas');
        if (canvas && typeof QRious !== 'undefined') {
          new QRious({
            element: canvas,
            value: upiLink,
            size: 180,
            level: 'M',
            foreground: '#0B1F3A',
            background: '#FFFFFF'
          });
        }
      }, 20);
    }

    function copyUpiId() {
      const tempInput = document.createElement('textarea');
      tempInput.value = LIBRARY_UPI_ID;
      document.body.appendChild(tempInput);
      tempInput.select();
      try {
        document.execCommand('copy');
        showToast(`Copied UPI ID: ${LIBRARY_UPI_ID}`, 'success');
      } catch (err) {
        showToast(`UPI ID: ${LIBRARY_UPI_ID}`, 'info');
      }
      document.body.removeChild(tempInput);
    }

    // Tab Navigation
    function switchTab(tabName) {
      state.activeTab = tabName;
      document.querySelectorAll('.tab-panel').forEach(p => p.classList.add('hidden'));
      const activePanel = document.getElementById(`panel-${tabName}`);
      if (activePanel) activePanel.classList.remove('hidden');

      document.querySelectorAll('.nav-tab').forEach(btn => {
        btn.className = 'nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 text-slate-200 hover:bg-white/10 cursor-pointer';
      });
      const activeBtn = document.getElementById(`tab-btn-${tabName}`);
      if (activeBtn) {
        activeBtn.className = 'nav-tab px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold whitespace-nowrap transition flex items-center gap-2 bg-amber-500 text-mits-navy shadow-sm cursor-pointer';
      }
    }

    // Render KPI Stats & Navigation Badges
    function renderStats() {
      const totalBooks = state.books.length;
      const activeBorrows = state.borrows.length;
      let overdueCount = 0;
      let totalFines = 0;

      state.borrows.forEach(br => {
        const { isOverdue, fineAmount } = calculateFine(br);
        if (isOverdue) overdueCount++;
        totalFines += fineAmount;
      });

      const groups = computePeerStudyGroups();

      document.getElementById('stat-total-books').textContent = totalBooks;
      document.getElementById('stat-total-students').textContent = state.students.length;
      document.getElementById('stat-active-borrows').textContent = activeBorrows;
      document.getElementById('stat-overdue-count').textContent = `${overdueCount} Overdue`;
      document.getElementById('stat-study-groups').textContent = groups.length;
      document.getElementById('stat-total-fines').textContent = `₹${totalFines}`;

      document.getElementById('nav-badge-books').textContent = totalBooks;
      document.getElementById('nav-badge-borrows').textContent = activeBorrows;
      document.getElementById('nav-badge-groups').textContent = groups.length;
      document.getElementById('groups-hero-count').textContent = groups.length;
    }

    // Populate Issue Form Dropdowns
    function populateIssueForm() {
      const studentSelect = document.getElementById('issue-student');
      const currentVal = studentSelect.value;
      studentSelect.innerHTML = state.students.map(s => `
        <option value="${s.id}">${s.name} (${s.id}) — ${s.branch}</option>
      `).join('');
      if (currentVal && state.students.some(s => s.id === currentVal)) {
        studentSelect.value = currentVal;
      }
      updateSelectedStudentPreview();
      populateIssueBookDropdown();
    }

    function updateSelectedStudentPreview() {
      const studentId = document.getElementById('issue-student').value;
      const student = state.students.find(s => s.id === studentId);
      const box = document.getElementById('selected-student-info');
      if (!student || !box) return;

      box.innerHTML = `
        <div class="truncate pr-2">
          <span class="font-semibold text-slate-800">${student.email}</span>
          <span class="text-slate-400 block text-[11px]">${student.branch}</span>
        </div>
        <span class="shrink-0 px-2 py-0.5 rounded-md text-[11px] font-bold ${student.subscriptionActive ? 'bg-emerald-100 text-emerald-800' : 'bg-rose-100 text-rose-800'}">
          ${student.subscriptionActive ? 'Sub: Active' : 'Sub: Expired'}
        </span>
      `;
    }

    function populateIssueBookDropdown() {
      const subjectFilter = document.getElementById('issue-subject-filter').value;
      const bookSelect = document.getElementById('issue-book');
      const filtered = subjectFilter === 'ALL'
        ? state.books
        : state.books.filter(b => b.subject === subjectFilter);

      bookSelect.innerHTML = filtered.map(b => {
        const avail = getAvailableCopies(b.id);
        const activeReaders = state.borrows.filter(br => br.bookId === b.id).length;
        const peerTag = activeReaders > 0 ? ` • [${activeReaders} student${activeReaders > 1 ? 's' : ''} reading]` : '';
        return `<option value="${b.id}" ${avail === 0 ? 'disabled' : ''}>[${b.id}] ${b.title} (${avail}/${b.totalCopies} avail)${peerTag}</option>`;
      }).join('');
    }

    // Handle Book Issue Submission
    function handleIssueBook(e) {
      e.preventDefault();
      const studentId = document.getElementById('issue-student').value;
      const bookId = document.getElementById('issue-book').value;
      const allowedDays = parseInt(document.getElementById('issue-days').value, 10) || 7;
      const elapsedDays = parseInt(document.getElementById('issue-elapsed').value, 10) || 0;

      const student = state.students.find(s => s.id === studentId);
      const book = state.books.find(b => b.id === bookId);

      if (!student || !book) return;

      if (!student.subscriptionActive) {
        showToast(`${student.name}'s General Subscription is inactive. Please activate it in the Students tab first.`, 'error');
        return;
      }

      if (getAvailableCopies(bookId) <= 0) {
        showToast(`All copies of "${book.title}" are currently issued.`, 'warning');
        return;
      }

      // Check if same student already borrowed this exact book
      const alreadyBorrowed = state.borrows.some(br => br.studentId === studentId && br.bookId === bookId);
      if (alreadyBorrowed) {
        showToast(`${student.name} already has an active copy of "${book.title}".`, 'warning');
        return;
      }

      const newBorrow = {
        id: 'BRW-' + Math.floor(1000 + Math.random() * 9000),
        studentId,
        bookId,
        borrowDate: new Date().toISOString().split('T')[0],
        allowedDays,
        elapsedDays
      };

      state.borrows.unshift(newBorrow);
      saveState();
      renderAll();

      // Check if this created or expanded a Peer Study Group
      const coReaders = state.borrows.filter(br => br.bookId === bookId);
      if (coReaders.length >= 2) {
        showToast(`Issued "${book.title}" to ${student.name}! Added to Same-Book Peer Study Group (${coReaders.length} members) with shared emails.`, 'success');
      } else {
        showToast(`Issued "${book.title}" to ${student.name} for ${allowedDays} days.`, 'success');
      }
    }

    // Open Edit / Update Borrow Days Modal
    function openEditBorrowModal(borrowId) {
      const br = state.borrows.find(b => b.id === borrowId);
      if (!br) return;
      const student = state.students.find(s => s.id === br.studentId);
      const book = state.books.find(b => b.id === br.bookId);

      openModal({
        title: `Update Borrow Entry (${br.id})`,
        subtitle: `${student.name} (${student.id}) • ${book.title}`,
        icon: 'fa-calendar-days',
        iconBg: 'bg-blue-100 text-blue-700',
        bodyHtml: `
          <form onsubmit="saveBorrowEdit(event, '${br.id}')" class="space-y-4">
            <div>
              <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Allowed Borrow Days</label>
              <input type="number" id="edit-allowed-days" min="1" max="90" value="${br.allowedDays}" required class="w-full rounded-xl border border-slate-300 px-3.5 py-2 text-sm font-semibold focus:border-mits-royal focus:outline-none" />
              <p class="text-[11px] text-slate-500 mt-1">Extend or shorten the days requested by the student.</p>
            </div>
            <div>
              <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Simulated Days Passed Since Issue</label>
              <input type="number" id="edit-elapsed-days" min="0" max="180" value="${br.elapsedDays}" required class="w-full rounded-xl border border-slate-300 px-3.5 py-2 text-sm font-semibold focus:border-mits-royal focus:outline-none" />
              <p class="text-[11px] text-amber-700 mt-1">Any day beyond Allowed Days incurs <strong>₹15/day</strong> fine.</p>
            </div>
            <div class="flex justify-end gap-2 pt-2">
              <button type="button" onclick="closeModal()" class="px-4 py-2 rounded-xl border border-slate-300 text-xs font-semibold text-slate-700 hover:bg-slate-50 cursor-pointer">Cancel</button>
              <button type="submit" class="px-4 py-2 rounded-xl bg-mits-navy text-white text-xs font-semibold hover:bg-mits-royal cursor-pointer">Save Changes</button>
            </div>
          </form>
        `
      });
    }

    function saveBorrowEdit(e, borrowId) {
      e.preventDefault();
      const br = state.borrows.find(b => b.id === borrowId);
      if (!br) return;
      br.allowedDays = parseInt(document.getElementById('edit-allowed-days').value, 10) || 1;
      br.elapsedDays = parseInt(document.getElementById('edit-elapsed-days').value, 10) || 0;
      saveState();
      closeModal();
      renderAll();

      const { isOverdue, fineAmount, extraDays } = calculateFine(br);
      if (isOverdue) {
        showToast(`Entry updated: Overdue by ${extraDays} day(s). Current fine: ₹${fineAmount}.`, 'warning');
      } else {
        showToast(`Borrow duration updated successfully. No fine due.`, 'success');
      }
    }

    function simulateAdvanceAllDays(daysToAdd = 1) {
      if (state.borrows.length === 0) {
        showToast('No active borrowed books to advance.', 'info');
        return;
      }
      state.borrows.forEach(br => {
        br.elapsedDays += daysToAdd;
      });
      saveState();
      renderAll();
      showToast(`Simulated +${daysToAdd} day(s) across all active loans. Overdue fines recalculated @ ₹15/day.`, 'warning');
    }

    function returnBook(borrowId) {
      const idx = state.borrows.findIndex(b => b.id === borrowId);
      if (idx === -1) return;
      const br = state.borrows[idx];
      const student = state.students.find(s => s.id === br.studentId);
      const book = state.books.find(b => b.id === br.bookId);
      const { fineAmount, extraDays } = calculateFine(br);

      if (fineAmount > 0) {
        openModal({
          title: 'Settle Overdue Fine & Return',
          subtitle: `${student.name} (${student.id}) • ${book.title}`,
          icon: 'fa-indian-rupee-sign',
          iconBg: 'bg-rose-100 text-rose-700',
          bodyHtml: `
            <div class="p-3.5 rounded-xl bg-rose-50 border border-rose-200 text-xs text-rose-900 space-y-1">
              <p><strong>Overdue Duration:</strong> ${extraDays} extra day(s) beyond ${br.allowedDays} allowed days</p>
              <p class="text-base font-display font-extrabold text-rose-700 pt-1">Total Fine Due: ₹${fineAmount} (@ ₹15/day)</p>
            </div>
            <div class="flex flex-col items-center p-3 rounded-xl bg-slate-50 border border-slate-200 text-center space-y-2">
              <canvas id="fine-upi-qr-canvas" width="140" height="140" class="rounded-lg bg-white p-1.5 border border-slate-200"></canvas>
              <div class="flex items-center gap-2 text-xs">
                <span class="text-slate-500">UPI ID:</span>
                <span class="font-mono font-bold text-slate-800">${LIBRARY_UPI_ID}</span>
                <button type="button" onclick="copyUpiId()" class="text-indigo-600 hover:underline font-semibold cursor-pointer">Copy</button>
              </div>
            </div>
            <div class="flex justify-end gap-2 pt-2">
              <button onclick="closeModal()" class="px-4 py-2 rounded-xl border border-slate-300 text-xs font-semibold text-slate-700 hover:bg-slate-50 cursor-pointer">Cancel</button>
              <button onclick="completeBookReturn('${br.id}', ${fineAmount})" class="px-4 py-2 rounded-xl bg-rose-600 hover:bg-rose-700 text-white text-xs font-bold cursor-pointer">Collect ₹${fineAmount} & Return Book</button>
            </div>
          `
        });
        setTimeout(() => {
          const canvas = document.getElementById('fine-upi-qr-canvas');
          if (canvas && typeof QRious !== 'undefined') {
            const upiLink = `upi://pay?pa=${encodeURIComponent(LIBRARY_UPI_ID)}&pn=${encodeURIComponent('MITS-DU Central Library')}&am=${Number(fineAmount).toFixed(2)}&cu=INR&tn=${encodeURIComponent(`Fine ${student.id}`)}`;
            new QRious({
              element: canvas,
              value: upiLink,
              size: 140,
              level: 'M',
              foreground: '#0B1F3A',
              background: '#FFFFFF'
            });
          }
        }, 20);
        return;
      }

      completeBookReturn(borrowId, 0);
    }

    function completeBookReturn(borrowId, paidFine = 0) {
      const idx = state.borrows.findIndex(b => b.id === borrowId);
      if (idx === -1) return;
      const br = state.borrows[idx];
      const student = state.students.find(s => s.id === br.studentId);
      const book = state.books.find(b => b.id === br.bookId);

      state.borrows.splice(idx, 1);
      saveState();
      closeModal();
      renderAll();

      if (paidFine > 0) {
        showToast(`Collected ₹${paidFine} overdue fine and returned "${book.title}" from ${student.name}.`, 'success');
      } else {
        showToast(`Returned "${book.title}" from ${student.name} on time (₹0 fine).`, 'success');
      }
    }

    // Toggle Student General Subscription
    function toggleStudentSubscription(studentId) {
      const student = state.students.find(s => s.id === studentId);
      if (!student) return;
      student.subscriptionActive = !student.subscriptionActive;
      student.subValidTill = student.subscriptionActive ? '31 May 2027' : 'Expired';
      saveState();
      renderAll();
      showToast(`${student.name}'s General Subscription is now ${student.subscriptionActive ? 'ACTIVE' : 'INACTIVE'}.`, 'info');
    }

    // Render Dashboard Previews
    function renderDashboardPreviews() {
      const groupsContainer = document.getElementById('dashboard-groups-preview');
      const groups = computePeerStudyGroups();

      if (groups.length === 0) {
        groupsContainer.innerHTML = `
          <div class="text-center py-6 bg-slate-50 rounded-xl border border-dashed border-slate-300">
            <p class="text-xs text-slate-500">No peer groups yet. Issue the same book to 2+ students to auto-create a doubt-sharing group!</p>
          </div>
        `;
      } else {
        groupsContainer.innerHTML = groups.slice(0, 2).map(g => {
          const emails = g.members.map(m => m.email).join(', ');
          return `
            <div class="p-3.5 rounded-xl bg-indigo-50/60 border border-indigo-200/80 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
              <div>
                <div class="flex items-center gap-2">
                  <span class="px-2 py-0.5 rounded-md bg-indigo-600 text-white text-[10px] font-bold">${g.book.id}</span>
                  <span class="text-xs font-bold text-slate-900">${g.book.title}</span>
                </div>
                <p class="text-[11px] text-slate-600 mt-1">
                  <strong>Members (${g.members.length}):</strong> ${g.members.map(m => `${m.name} (${m.id})`).join(' • ')}
                </p>
                <p class="text-[11px] text-indigo-700 font-medium mt-0.5 truncate max-w-md">
                  <i class="fa-solid fa-envelope mr-1"></i> ${emails}
                </p>
              </div>
              <button onclick="copyGroupEmails('${g.book.id}')" class="shrink-0 px-3 py-1.5 rounded-lg bg-white hover:bg-indigo-600 hover:text-white text-indigo-700 border border-indigo-200 text-xs font-semibold transition cursor-pointer">
                <i class="fa-regular fa-copy mr-1"></i> Copy Emails
              </button>
            </div>
          `;
        }).join('');
      }

      const borrowsContainer = document.getElementById('dashboard-borrows-preview');
      if (state.borrows.length === 0) {
        borrowsContainer.innerHTML = `
          <div class="text-center py-6 bg-slate-50 rounded-xl border border-dashed border-slate-300">
            <p class="text-xs text-slate-500">No books currently borrowed.</p>
          </div>
        `;
      } else {
        borrowsContainer.innerHTML = state.borrows.slice(0, 3).map(br => {
          const student = state.students.find(s => s.id === br.studentId);
          const book = state.books.find(b => b.id === br.bookId);
          if (!student || !book) return '';
          const { isOverdue, extraDays, fineAmount } = calculateFine(br);
          return `
            <div class="p-3.5 rounded-xl border ${isOverdue ? 'bg-rose-50/60 border-rose-200' : 'bg-slate-50 border-slate-200/80'} flex flex-col sm:flex-row sm:items-center justify-between gap-3">
              <div>
                <div class="flex flex-wrap items-center gap-2">
                  <span class="text-xs font-bold text-slate-900">${student.name}</span>
                  <span class="text-[11px] text-slate-500">(${student.id})</span>
                  <span class="px-2 py-0.5 rounded-md text-[10px] font-bold ${isOverdue ? 'bg-rose-600 text-white' : 'bg-emerald-100 text-emerald-800'}">
                    ${isOverdue ? `Overdue +${extraDays}d • Fine ₹${fineAmount}` : `${br.elapsedDays}/${br.allowedDays} Days • ₹0 Fine`}
                  </span>
                </div>
                <p class="text-xs text-slate-600 mt-1">[${book.id}] ${book.title}</p>
              </div>
              <div class="flex items-center gap-2 shrink-0">
                <button onclick="openEditBorrowModal('${br.id}')" class="px-2.5 py-1.5 rounded-lg bg-white border border-slate-300 hover:bg-slate-100 text-slate-700 text-xs font-semibold cursor-pointer">
                  Update Days
                </button>
                <button onclick="returnBook('${br.id}')" class="px-2.5 py-1.5 rounded-lg ${isOverdue ? 'bg-rose-600 hover:bg-rose-700' : 'bg-mits-navy hover:bg-mits-royal'} text-white text-xs font-semibold cursor-pointer">
                  ${isOverdue ? `Settle ₹${fineAmount}` : 'Return'}
                </button>
              </div>
            </div>
          `;
        }).join('');
      }
    }

    // Render Catalog (60 Books across 6 Subjects)
    function renderCatalog() {
      const subjects = [
        'ALL',
        'Electronic Devices',
        'Engineering Mathematics',
        'Fundamentals of C++',
        'Engineering Physics',
        'Computer Programming',
        'Digital Sensors'
      ];

      const pillsContainer = document.getElementById('subject-filter-pills');
      pillsContainer.innerHTML = subjects.map(sub => {
        const count = sub === 'ALL' ? state.books.length : state.books.filter(b => b.subject === sub).length;
        const active = state.selectedSubject === sub;
        return `
          <button onclick="setCatalogSubject('${sub}')" class="px-3 py-1.5 rounded-xl text-xs font-semibold transition cursor-pointer ${
            active
              ? 'bg-mits-navy text-white shadow-xs'
              : 'bg-slate-100 text-slate-700 hover:bg-slate-200'
          }">
            ${sub === 'ALL' ? 'All Subjects' : sub} <span class="opacity-75 ml-1">(${count})</span>
          </button>
        `;
      }).join('');

      const query = (document.getElementById('catalog-search').value || '').toLowerCase().trim();
      const filteredBooks = state.books.filter(b => {
        const matchesSub = state.selectedSubject === 'ALL' || b.subject === state.selectedSubject;
        const matchesQuery = !query ||
          b.title.toLowerCase().includes(query) ||
          b.author.toLowerCase().includes(query) ||
          b.id.toLowerCase().includes(query) ||
          b.subject.toLowerCase().includes(query);
        return matchesSub && matchesQuery;
      });

      const grid = document.getElementById('catalog-grid');
      if (filteredBooks.length === 0) {
        grid.innerHTML = `
          <div class="col-span-full text-center py-12 bg-white rounded-2xl border border-slate-200">
            <p class="text-sm font-semibold text-slate-600">No matching books found in the catalog.</p>
          </div>
        `;
        return;
      }

      const subjectColors = {
        'Electronic Devices': 'bg-blue-50 text-blue-700 border-blue-200',
        'Engineering Mathematics': 'bg-purple-50 text-purple-700 border-purple-200',
        'Fundamentals of C++': 'bg-emerald-50 text-emerald-700 border-emerald-200',
        'Engineering Physics': 'bg-amber-50 text-amber-800 border-amber-200',
        'Computer Programming': 'bg-indigo-50 text-indigo-700 border-indigo-200',
        'Digital Sensors': 'bg-teal-50 text-teal-700 border-teal-200'
      };

      grid.innerHTML = filteredBooks.map(book => {
        const avail = getAvailableCopies(book.id);
        const borrowers = state.borrows
          .filter(br => br.bookId === book.id)
          .map(br => state.students.find(s => s.id === br.studentId)?.name)
          .filter(Boolean);

        return `
          <div class="bg-white rounded-2xl border border-slate-200/90 shadow-xs p-4 flex flex-col justify-between hover:border-mits-royal/40 transition">
            <div>
              <div class="flex items-center justify-between gap-2 mb-2.5">
                <span class="px-2.5 py-0.5 rounded-lg text-[11px] font-bold border ${subjectColors[book.subject] || 'bg-slate-100 text-slate-700'}">
                  ${book.subject}
                </span>
                <span class="text-xs font-mono font-bold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-md">${book.id}</span>
              </div>
              <h3 class="font-display font-bold text-slate-900 text-sm leading-snug">${book.title}</h3>
              <p class="text-xs text-slate-500 mt-1"><i class="fa-solid fa-feather-pointed text-slate-400 mr-1"></i> ${book.author}</p>
            </div>

            <div class="mt-4 pt-3 border-t border-slate-100">
              <div class="flex items-center justify-between text-xs mb-2.5">
                <span class="font-semibold ${avail > 0 ? 'text-emerald-700' : 'text-rose-600'}">
                  <i class="fa-solid fa-layer-group mr-1"></i> ${avail} of ${book.totalCopies} Copies Available
                </span>
                ${borrowers.length >= 2 ? `<span class="px-2 py-0.5 rounded-md bg-indigo-100 text-indigo-700 text-[10px] font-bold">Study Group Active</span>` : ''}
              </div>
              ${borrowers.length > 0 ? `
                <p class="text-[11px] text-slate-500 mb-2.5 truncate">
                  Currently borrowed by: <strong class="text-slate-700">${borrowers.join(', ')}</strong>
                </p>
              ` : ''}
              <button onclick="quickSelectBookToIssue('${book.id}')" ${avail === 0 ? 'disabled' : ''} class="w-full py-2 px-3 rounded-xl text-xs font-semibold transition flex items-center justify-center gap-1.5 cursor-pointer ${
                avail > 0
                  ? 'bg-mits-navy hover:bg-mits-royal text-white'
                  : 'bg-slate-100 text-slate-400 cursor-not-allowed'
              }">
                <i class="fa-solid fa-plus-circle"></i> ${avail > 0 ? 'Select to Borrow' : 'Out of Stock'}
              </button>
            </div>
          </div>
        `;
      }).join('');
    }

    function setCatalogSubject(sub) {
      state.selectedSubject = sub;
      renderCatalog();
    }

    function quickSelectBookToIssue(bookId) {
      const book = state.books.find(b => b.id === bookId);
      if (!book) return;
      document.getElementById('issue-subject-filter').value = book.subject;
      populateIssueBookDropdown();
      document.getElementById('issue-book').value = book.id;
      switchTab('dashboard');
      showToast(`Selected "${book.title}" in the Issue Book form.`, 'info');
    }

    // Render Students & General Subscription Tab
    function renderStudents() {
      const grid = document.getElementById('students-grid');
      const branchBadge = {
        'Electronics Engineering': 'bg-blue-50 text-blue-700 border-blue-200',
        'Computer Engineering': 'bg-indigo-50 text-indigo-700 border-indigo-200',
        'Civil Engineering': 'bg-amber-50 text-amber-800 border-amber-200',
        'Mechanical Engineering': 'bg-slate-100 text-slate-700 border-slate-300',
        'Electrical Engineering': 'bg-emerald-50 text-emerald-700 border-emerald-200'
      };

      grid.innerHTML = state.students.map(student => {
        const studentBorrows = state.borrows.filter(br => br.studentId === student.id);
        const totalStudentFine = studentBorrows.reduce((sum, br) => sum + calculateFine(br).fineAmount, 0);

        return `
          <div class="bg-white rounded-2xl border border-slate-200 shadow-xs p-5 flex flex-col justify-between gap-4">
            <div class="flex items-start justify-between gap-3">
              <div class="flex items-center gap-3.5">
                <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-mits-navy to-mits-royal text-amber-300 font-display font-extrabold text-lg flex items-center justify-center shrink-0 shadow-xs">
                  ${student.name.charAt(0)}
                </div>
                <div>
                  <div class="flex flex-wrap items-center gap-2">
                    <h3 class="font-display font-bold text-base text-slate-900">${student.name}</h3>
                    <span class="font-mono text-xs font-bold px-2 py-0.5 rounded-md bg-slate-100 text-slate-700">${student.id}</span>
                  </div>
                  <span class="inline-block mt-1 px-2.5 py-0.5 rounded-lg text-[11px] font-semibold border ${branchBadge[student.branch] || ''}">
                    ${student.branch}
                  </span>
                  <p class="text-xs text-slate-500 mt-1.5 flex items-center gap-1.5">
                    <i class="fa-solid fa-envelope text-slate-400"></i>
                    <span class="font-medium text-slate-700">${student.email}</span>
                  </p>
                </div>
              </div>
            </div>

            <div class="pt-3 border-t border-slate-100 flex flex-wrap items-center justify-between gap-3">
              <div class="flex items-center gap-3 text-xs">
                <span class="px-2.5 py-1 rounded-lg font-semibold ${student.subscriptionActive ? 'bg-emerald-50 text-emerald-700 border border-emerald-200' : 'bg-rose-50 text-rose-700 border border-rose-200'}">
                  <i class="fa-solid ${student.subscriptionActive ? 'fa-shield-check fa-circle-check' : 'fa-circle-xmark'} mr-1"></i>
                  General Sub: ${student.subscriptionActive ? `Active (${student.subValidTill})` : 'Inactive'}
                </span>
                <span class="text-slate-500">Books: <strong class="text-slate-800">${studentBorrows.length}</strong></span>
                ${totalStudentFine > 0 ? `<span class="text-rose-600 font-bold">Fine: ₹${totalStudentFine}</span>` : ''}
              </div>

              <div class="flex flex-wrap items-center gap-2">
                <button onclick="toggleStudentSubscription('${student.id}')" class="px-3 py-1.5 rounded-xl border border-slate-300 hover:bg-slate-50 text-xs font-semibold text-slate-700 transition cursor-pointer">
                  ${student.subscriptionActive ? 'Deactivate Subscription' : 'Activate Subscription'}
                </button>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    // Render Borrows & Fine Tracker Tab
    function renderBorrows() {
      const container = document.getElementById('borrows-list-container');
      if (state.borrows.length === 0) {
        container.innerHTML = `
          <div class="bg-white rounded-2xl border border-slate-200 p-10 text-center">
            <div class="w-12 h-12 rounded-2xl bg-slate-100 text-slate-400 flex items-center justify-center text-xl mx-auto mb-3">
              <i class="fa-solid fa-book-open"></i>
            </div>
            <h3 class="font-display font-bold text-slate-800">No Active Borrow Records</h3>
            <p class="text-xs text-slate-500 mt-1">Go to Dashboard & Issue to assign books to MITS-DU students.</p>
          </div>
        `;
        return;
      }

      container.innerHTML = state.borrows.map(br => {
        const student = state.students.find(s => s.id === br.studentId);
        const book = state.books.find(b => b.id === br.bookId);
        if (!student || !book) return '';
        const { isOverdue, extraDays, fineAmount } = calculateFine(br);
        const progressPct = Math.min(100, Math.round((br.elapsedDays / Math.max(1, br.allowedDays)) * 100));

        return `
          <div class="bg-white rounded-2xl border ${isOverdue ? 'border-rose-300 ring-1 ring-rose-100' : 'border-slate-200'} shadow-xs p-5">
            <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
              <!-- Student & Book Details -->
              <div class="space-y-1.5">
                <div class="flex flex-wrap items-center gap-2">
                  <span class="px-2 py-0.5 rounded-md bg-slate-100 font-mono text-[11px] font-bold text-slate-600">${br.id}</span>
                  <span class="font-display font-bold text-base text-slate-900">${student.name}</span>
                  <span class="text-xs font-semibold text-mits-royal bg-blue-50 px-2 py-0.5 rounded-md">${student.id} • ${student.branch}</span>
                </div>
                <p class="text-sm font-semibold text-slate-800">
                  <span class="text-amber-600 font-mono mr-1">[${book.id}]</span> ${book.title}
                  <span class="text-xs font-normal text-slate-500">(${book.subject})</span>
                </p>
                <div class="flex flex-wrap items-center gap-4 text-xs text-slate-500 pt-1">
                  <span><i class="fa-regular fa-calendar mr-1"></i> Issued: <strong>${br.borrowDate}</strong></span>
                  <span><i class="fa-solid fa-hourglass-half mr-1"></i> Requested Duration: <strong>${br.allowedDays} Days</strong></span>
                  <span><i class="fa-solid fa-clock mr-1"></i> Elapsed: <strong>${br.elapsedDays} Days</strong></span>
                </div>
              </div>

              <!-- Fine Status & Action Buttons -->
              <div class="flex flex-col sm:flex-row items-start sm:items-center gap-3 lg:justify-end">
                <div class="px-4 py-2.5 rounded-xl border ${isOverdue ? 'bg-rose-50 border-rose-200 text-rose-800' : 'bg-emerald-50 border-emerald-200 text-emerald-800'} min-w-[175px]">
                  <p class="text-[11px] font-bold uppercase tracking-wider">
                    ${isOverdue ? `Overdue by ${extraDays} Day${extraDays > 1 ? 's' : ''}` : 'Within Allowed Days'}
                  </p>
                  <p class="text-lg font-display font-extrabold mt-0.5">
                    ${isOverdue ? `Fine: ₹${fineAmount}` : 'Fine: ₹0'}
                    <span class="text-[11px] font-normal opacity-80">(₹15/day)</span>
                  </p>
                </div>

                <div class="flex flex-wrap gap-2">
                  <button onclick="openEditBorrowModal('${br.id}')" class="px-3.5 py-2.5 rounded-xl border border-slate-300 hover:bg-slate-50 text-xs font-semibold text-slate-700 transition flex items-center gap-1.5 cursor-pointer">
                    <i class="fa-solid fa-pen-to-square text-blue-600"></i> Update Days
                  </button>
                  <button onclick="returnBook('${br.id}')" class="px-3.5 py-2.5 rounded-xl ${isOverdue ? 'bg-rose-600 hover:bg-rose-700' : 'bg-mits-navy hover:bg-mits-royal'} text-white text-xs font-semibold transition flex items-center gap-1.5 cursor-pointer">
                    <i class="fa-solid fa-check-double text-amber-400"></i> ${isOverdue ? `Collect ₹${fineAmount} & Return` : 'Return Book'}
                  </button>
                </div>
              </div>
            </div>

            <!-- Timeline Bar -->
            <div class="mt-4 pt-3 border-t border-slate-100">
              <div class="w-full h-2 bg-slate-100 rounded-full overflow-hidden">
                <div class="h-full rounded-full transition-all ${isOverdue ? 'bg-rose-500' : 'bg-emerald-500'}" style="width: ${progressPct}%"></div>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    // Render Peer Study Groups Tab
    function renderPeerGroups() {
      const container = document.getElementById('groups-list-container');
      const groups = computePeerStudyGroups();

      if (groups.length === 0) {
        container.innerHTML = `
          <div class="col-span-full bg-white rounded-2xl border border-slate-200 p-10 text-center">
            <div class="w-14 h-14 rounded-2xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-2xl mx-auto mb-3">
              <i class="fa-solid fa-users-slash"></i>
            </div>
            <h3 class="font-display font-bold text-lg text-slate-800">No Active Same-Book Groups Yet</h3>
            <p class="text-xs text-slate-500 max-w-md mx-auto mt-1">
              Issue the exact same book title to two or more students from the Dashboard to automatically form a study group and share their email IDs.
            </p>
          </div>
        `;
        return;
      }

      container.innerHTML = groups.map(group => {
        const emailList = group.members.map(m => m.email);
        const mailtoHref = `mailto:${emailList.join(',')}?subject=${encodeURIComponent(`MITS-DU Study Doubt: ${group.book.title} (${group.book.id})`)}&body=${encodeURIComponent(`Hi classmates,\n\nWe all borrowed "${group.book.title}" from the MITS-DU Central Library. I had a quick doubt regarding:\n\n`)}`;
        const notes = state.doubtNotes[group.book.id] || [];

        return `
          <div class="bg-white rounded-2xl border border-indigo-200/90 shadow-xs overflow-hidden flex flex-col justify-between">
            <div>
              <!-- Group Header -->
              <div class="bg-gradient-to-r from-indigo-50 to-blue-50/70 p-5 border-b border-indigo-100">
                <div class="flex items-center justify-between gap-2 mb-1.5">
                  <span class="px-2.5 py-0.5 rounded-lg bg-indigo-600 text-white text-[11px] font-bold">
                    ${group.book.subject}
                  </span>
                  <span class="text-xs font-mono font-bold text-indigo-800 bg-white px-2.5 py-0.5 rounded-md border border-indigo-200">
                    ${group.book.id} • ${group.members.length} Peers
                  </span>
                </div>
                <h3 class="font-display font-bold text-lg text-slate-900 leading-snug">${group.book.title}</h3>
                <p class="text-xs text-slate-500 mt-0.5">Author: ${group.book.author}</p>
              </div>

              <!-- Group Members & Shared Emails -->
              <div class="p-5 space-y-4">
                <div>
                  <div class="flex items-center justify-between mb-2">
                    <h4 class="text-xs font-bold uppercase tracking-wider text-slate-500">Group Members & Shared Email IDs</h4>
                    <div class="flex gap-1.5">
                      <button onclick="copyGroupEmails('${group.book.id}')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold transition cursor-pointer">
                        <i class="fa-regular fa-copy mr-1"></i> Copy All Emails
                      </button>
                      <a href="${mailtoHref}" class="px-2.5 py-1 rounded-lg bg-indigo-600 hover:bg-indigo-700 text-white text-[11px] font-semibold transition">
                        <i class="fa-solid fa-paper-plane mr-1"></i> Ask Doubt via Email
                      </a>
                    </div>
                  </div>

                  <div class="space-y-2">
                    ${group.members.map(m => `
                      <div class="p-2.5 rounded-xl bg-slate-50 border border-slate-200/80 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
                        <div>
                          <p class="text-xs font-bold text-slate-900">${m.name} <span class="font-mono font-normal text-slate-500">(${m.id})</span></p>
                          <p class="text-[11px] text-slate-500">${m.branch}</p>
                        </div>
                        <a href="mailto:${m.email}?subject=${encodeURIComponent(`Doubt on ${group.book.title}`)}" class="text-xs font-semibold text-indigo-700 hover:underline flex items-center gap-1.5">
                          <i class="fa-solid fa-envelope text-indigo-500"></i>
                          <span>${m.email}</span>
                        </a>
                      </div>
                    `).join('')}
                  </div>
                </div>

                <!-- Built-in Quick Doubt Thread -->
                <div class="pt-3 border-t border-slate-100">
                  <h4 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">
                    <i class="fa-solid fa-comments text-amber-500 mr-1"></i> Group Doubt Board
                  </h4>
                  <div class="space-y-2 max-h-36 overflow-y-auto pr-1 mb-3">
                    ${notes.length === 0
                      ? `<p class="text-xs text-slate-400 italic">No doubts posted yet. Start a discussion below!</p>`
                      : notes.map(n => `
                        <div class="p-2.5 rounded-xl bg-amber-50/70 border border-amber-200/70 text-xs">
                          <span class="font-bold text-mits-navy">${n.sender}:</span>
                          <span class="text-slate-700 ml-1">${n.text}</span>
                        </div>
                      `).join('')
                    }
                  </div>
                  <form onsubmit="postGroupDoubt(event, '${group.book.id}')" class="flex gap-2">
                    <select id="doubt-sender-${group.book.id}" class="rounded-xl border border-slate-300 bg-slate-50 px-2.5 py-2 text-xs font-semibold text-slate-700 focus:outline-none">
                      ${group.members.map(m => `<option value="${m.name} (${m.id})">${m.name}</option>`).join('')}
                    </select>
                    <input type="text" id="doubt-input-${group.book.id}" placeholder="Ask a chapter doubt to peers..." required class="flex-1 rounded-xl border border-slate-300 px-3 py-2 text-xs focus:border-indigo-600 focus:outline-none" />
                    <button type="submit" class="px-3.5 py-2 rounded-xl bg-mits-navy hover:bg-mits-royal text-white text-xs font-semibold cursor-pointer">
                      Post
                    </button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    function copyGroupEmails(bookId) {
      const groups = computePeerStudyGroups();
      const group = groups.find(g => g.book.id === bookId);
      if (!group) return;
      const emailString = group.members.map(m => m.email).join(', ');

      // Fallback clipboard copy via temporary textarea (safe in all iframes)
      const tempInput = document.createElement('textarea');
      tempInput.value = emailString;
      document.body.appendChild(tempInput);
      tempInput.select();
      try {
        document.execCommand('copy');
        showToast(`Copied peer emails: ${emailString}`, 'success');
      } catch (err) {
        showToast(`Emails: ${emailString}`, 'info');
      }
      document.body.removeChild(tempInput);
    }

    function postGroupDoubt(e, bookId) {
      e.preventDefault();
      const sender = document.getElementById(`doubt-sender-${bookId}`).value;
      const input = document.getElementById(`doubt-input-${bookId}`);
      const text = input.value.trim();
      if (!text) return;

      if (!state.doubtNotes[bookId]) state.doubtNotes[bookId] = [];
      state.doubtNotes[bookId].push({ sender, text });
      saveState();
      renderPeerGroups();
      showToast('Doubt shared with fellow book borrowers!', 'success');
    }

    // Master Render
    function renderAll() {
      renderStats();
      populateIssueForm();
      renderDashboardPreviews();
      renderCatalog();
      renderStudents();
      renderBorrows();
      renderPeerGroups();
      switchTab(state.activeTab || 'dashboard');
    }

    // Initialize Application
    window.addEventListener('DOMContentLoaded', () => {
      loadState();
      renderAll();
    });
  </script>
</body>
</html>
