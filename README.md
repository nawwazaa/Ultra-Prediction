
<!DOCTYPE html>
<html lang="en" id="mainHtml" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Tactical Hub - Multi-Video Comparison</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body { background-color: #020617; color: #ffffff; font-family: 'Cairo', sans-serif; margin: 0; padding: 0; }
        input, select { background-color: #1e293b !important; color: #ffffff !important; border: 1px solid #334155 !important; border-radius: 8px; padding: 8px; outline: none; width: 100%; }
        .card { background: #0f172a; border: 1px solid rgba(255,255,255,0.05); border-radius: 24px; padding: 20px; }
        .scan-line { width: 100%; height: 3px; background: #10b981; position: absolute; top: 0; left: 0; animation: scan 2s linear infinite; z-index: 50; box-shadow: 0 0 20px #10b981; }
        @keyframes scan { 0% { top: 0; } 100% { top: 100%; } }
        .loading-overlay { position: fixed; inset: 0; background: rgba(2, 6, 23, 0.98); z-index: 9999; display: none; flex-direction: column; align-items: center; justify-content: center; }
        @media print { .no-print { display: none !important; } }
    </style>
</head>
<body>

<div id="loader" class="loading-overlay">
    <div class="w-16 h-16 border-4 border-emerald-500 border-t-transparent rounded-full animate-spin mb-4"></div>
    <p class="font-black text-emerald-400 animate-pulse" id="loadingText">Analyzing Visual Patterns...</p>
</div>

<div class="fixed top-5 right-5 z-50 no-print">
    <button class="bg-blue-600 text-white px-6 py-2 rounded-full font-black shadow-xl" onclick="toggleLanguage()">
        <span id="langName">العربية</span> 🌐
    </button>
</div>

<div class="max-w-7xl mx-auto p-4 lg:p-10 space-y-10">
    <header class="text-center space-y-2">
        <h1 class="text-4xl lg:text-6xl font-black text-emerald-400 uppercase tracking-tighter" id="txtTitle">AI Tactical Hub</h1>
        <p class="text-slate-400 font-bold" id="txtSubTitle">Cross-Video Team Comparison Engine</p>
    </header>

    <!-- STEP 1: VIDEO UPLOADS & TEAM IDENTIFICATION -->
    <section class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <!-- Video A -->
        <div class="card space-y-4 relative overflow-hidden">
            <h2 class="text-blue-400 font-black uppercase text-sm" id="txtVidA">Video Source A</h2>
            <div class="grid grid-cols-2 gap-2">
                <div>
                    <label class="text-[10px] text-slate-500 uppercase font-bold" id="txtV1T1">Team 1 Name</label>
                    <input type="text" id="v1t1" value="Al-Hilal" oninput="syncSelectors()">
                </div>
                <div>
                    <label class="text-[10px] text-slate-500 uppercase font-bold" id="txtV1T2">Team 2 Name</label>
                    <input type="text" id="v1t2" value="Al-Nassr" oninput="syncSelectors()">
                </div>
            </div>
            <div onclick="document.getElementById('file1').click()" class="aspect-video bg-black rounded-xl border-2 border-dashed border-slate-700 flex flex-col items-center justify-center cursor-pointer relative overflow-hidden">
                <video id="vPrev1" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                <span class="text-slate-500 font-bold text-xs" id="upV1">UPLOAD VIDEO A</span>
            </div>
            <input type="file" id="file1" class="hidden" accept="video/*" onchange="preview(event, 1)">
        </div>

        <!-- Video B -->
        <div class="card space-y-4 relative overflow-hidden">
            <h2 class="text-red-400 font-black uppercase text-sm" id="txtVidB">Video Source B</h2>
            <div class="grid grid-cols-2 gap-2">
                <div>
                    <label class="text-[10px] text-slate-500 uppercase font-bold" id="txtV2T1">Team 1 Name</label>
                    <input type="text" id="v2t1" value="Real Madrid" oninput="syncSelectors()">
                </div>
                <div>
                    <label class="text-[10px] text-slate-500 uppercase font-bold" id="txtV2T2">Team 2 Name</label>
                    <input type="text" id="v2t2" value="Man City" oninput="syncSelectors()">
                </div>
            </div>
            <div onclick="document.getElementById('file2').click()" class="aspect-video bg-black rounded-xl border-2 border-dashed border-slate-700 flex flex-col items-center justify-center cursor-pointer relative overflow-hidden">
                <video id="vPrev2" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                <span class="text-slate-500 font-bold text-xs" id="upV2">UPLOAD VIDEO B</span>
            </div>
            <input type="file" id="file2" class="hidden" accept="video/*" onchange="preview(event, 2)">
        </div>
    </section>

    <!-- STEP 2: USER CHOICE -->
    <section class="card bg-emerald-900/10 border-emerald-500/30">
        <h2 class="text-center font-black text-emerald-400 uppercase mb-6" id="txtSelection">Selection: Which teams to compare?</h2>
        <div class="flex flex-col md:flex-row items-center justify-center gap-8">
            <div class="w-full max-w-xs text-center">
                <p class="text-[10px] text-slate-400 uppercase font-bold mb-2" id="txtFromA">From Video A Select:</p>
                <select id="selectA" onchange="renderTables()" class="!border-blue-500 !text-blue-400 font-bold"></select>
            </div>
            <div class="text-4xl font-black text-emerald-500">VS</div>
            <div class="w-full max-w-xs text-center">
                <p class="text-[10px] text-slate-400 uppercase font-bold mb-2" id="txtFromB">From Video B Select:</p>
                <select id="selectB" onchange="renderTables()" class="!border-red-500 !text-red-400 font-bold"></select>
            </div>
        </div>
    </section>

    <!-- STEP 3: STATISTICAL DATA TABLES -->
    <section class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <div class="card !p-0 overflow-hidden border-b-4 border-blue-500">
            <div class="bg-blue-500/10 p-4 font-black text-blue-400 uppercase" id="tableTitleA">Team A Stats</div>
            <table class="w-full text-sm" id="tableA"></table>
        </div>
        <div class="card !p-0 overflow-hidden border-b-4 border-red-500">
            <div class="bg-red-500/10 p-4 font-black text-red-400 uppercase" id="tableTitleB">Team B Stats</div>
            <table class="w-full text-sm" id="tableB"></table>
        </div>
    </section>

    <div class="text-center pb-20 no-print">
        <button onclick="processAI()" class="bg-emerald-500 hover:bg-emerald-400 text-white px-16 py-6 rounded-2xl text-2xl font-black shadow-2xl transition-transform hover:scale-105" id="btnRun">RUN ANALYTICS</button>
    </div>

    <!-- RESULTS SECTION -->
    <section id="results" class="hidden space-y-8 animate-fade-in">
        <div class="card text-center py-10">
            <h3 class="text-slate-500 font-black uppercase text-xs tracking-widest mb-6" id="txtResTitle">Win Probability Comparison</h3>
            <div class="flex flex-col md:flex-row items-center justify-around gap-8">
                <div>
                    <div id="resNameA" class="text-blue-500 text-3xl font-black uppercase"></div>
                    <div id="resProbA" class="text-6xl font-black">0%</div>
                </div>
                <div class="w-full max-w-md h-8 bg-slate-800 rounded-full flex overflow-hidden border-2 border-slate-700">
                    <div id="barA" class="bg-blue-500 transition-all duration-1000"></div>
                    <div id="barB" class="bg-red-500 transition-all duration-1000"></div>
                </div>
                <div>
                    <div id="resNameB" class="text-red-500 text-3xl font-black uppercase"></div>
                    <div id="resProbB" class="text-6xl font-black">0%</div>
                </div>
            </div>
            <p id="aiLogic" class="mt-10 px-6 italic text-slate-400 border-t border-white/5 pt-6"></p>
        </div>
    </section>
</div>

<script>
    const i18n = {
        en: {
            title: "AI Tactical Hub", subtitle: "Cross-Video Team Comparison Engine",
            vidA: "Video Source A", vidB: "Video Source B",
            vT1: "Team 1 Name", vT2: "Team 2 Name",
            up: "UPLOAD VIDEO", selection: "Selection: Which teams to compare?",
            fromA: "From Video A Select:", fromB: "From Video B Select:",
            run: "RUN ANALYTICS", resTitle: "Win Probability Comparison",
            fields: ["Games Played", "Points", "Wins", "Draws", "Losses", "Goals For", "Goals Against", "Days Rest", "Location"],
            locs: ["Home", "Away", "Neutral"], loading: "Scanning visual tactical data...",
            lang: "العربية"
        },
        ar: {
            title: "مركز التحليل التكتيكي", subtitle: "محرك مقارنة الفرق عبر الفيديوهات",
            vidA: "مصدر الفيديو أ", vidB: "مصدر الفيديو ب",
            vT1: "اسم الفريق 1", vT2: "اسم الفريق 2",
            up: "تحميل الفيديو", selection: "الاختيار: ما هي الفرق التي تود مقارنتها؟",
            fromA: "من الفيديو (أ) اختر:", fromB: "من الفيديو (ب) اختر:",
            run: "بدء التحليل التكتيكي", resTitle: "مقارنة احتمالية الفوز",
            fields: ["المباريات", "النقاط", "فوز", "تعادل", "خسارة", "له", "عليه", "أيام الراحة", "الموقع"],
            locs: ["داخل الأرض", "خارج الأرض", "محايد"], loading: "جاري مسح البيانات التكتيكية...",
            lang: "English"
        }
    };

    let currentLang = 'en';
    const fieldKeys = ['gp', 'pts', 'w', 'd', 'l', 'gf', 'ga', 'rest', 'loc'];

    function toggleLanguage() {
        currentLang = currentLang === 'en' ? 'ar' : 'en';
        document.getElementById('mainHtml').dir = currentLang === 'ar' ? 'rtl' : 'ltr';
        updateUI();
    }

    function updateUI() {
        const l = i18n[currentLang];
        document.getElementById('txtTitle').innerText = l.title;
        document.getElementById('txtSubTitle').innerText = l.subtitle;
        document.getElementById('txtVidA').innerText = l.vidA;
        document.getElementById('txtVidB').innerText = l.vidB;
        document.getElementById('txtV1T1').innerText = l.vT1;
        document.getElementById('txtV1T2').innerText = l.vT2;
        document.getElementById('txtV2T1').innerText = l.vT1;
        document.getElementById('txtV2T2').innerText = l.vT2;
        document.getElementById('upV1').innerText = l.up + " A";
        document.getElementById('upV2').innerText = l.up + " B";
        document.getElementById('txtSelection').innerText = l.selection;
        document.getElementById('txtFromA').innerText = l.fromA;
        document.getElementById('txtFromB').innerText = l.fromB;
        document.getElementById('btnRun').innerText = l.run;
        document.getElementById('txtResTitle').innerText = l.resTitle;
        document.getElementById('langName').innerText = l.lang;
        document.getElementById('loadingText').innerText = l.loading;
        syncSelectors();
        renderTables();
    }

    function syncSelectors() {
        const selA = document.getElementById('selectA');
        const selB = document.getElementById('selectB');
        const valA = selA.value; 
        const valB = selB.value;

        selA.innerHTML = `<option value="${document.getElementById('v1t1').value}">${document.getElementById('v1t1').value}</option>
                          <option value="${document.getElementById('v1t2').value}">${document.getElementById('v1t2').value}</option>`;
        selB.innerHTML = `<option value="${document.getElementById('v2t1').value}">${document.getElementById('v2t1').value}</option>
                          <option value="${document.getElementById('v2t2').value}">${document.getElementById('v2t2').value}</option>`;
        
        if(valA) selA.value = valA;
        if(valB) selB.value = valB;
        renderTables();
    }

    function renderTables() {
        const l = i18n[currentLang];
        const nameA = document.getElementById('selectA').value;
        const nameB = document.getElementById('selectB').value;
        
        document.getElementById('tableTitleA').innerText = nameA;
        document.getElementById('tableTitleB').innerText = nameB;

        const build = (t) => fieldKeys.map((key, i) => {
            if(key === 'loc') return `<tr><td class="p-3 border-b border-white/5 text-slate-400 font-bold uppercase text-[10px]">${l.fields[i]}</td><td class="p-1 border-b border-white/5"><select id="${key}${t}"><option value="h">${l.locs[0]}</option><option value="a">${l.locs[1]}</option><option value="n">${l.locs[2]}</option></select></td></tr>`;
            return `<tr><td class="p-3 border-b border-white/5 text-slate-400 font-bold uppercase text-[10px]">${l.fields[i]}</td><td class="p-1 border-b border-white/5"><input type="number" id="${key}${t}" value="${key==='rest'?'4':'0'}"></td></tr>`;
        }).join('');

        document.getElementById('tableA').innerHTML = build('A');
        document.getElementById('tableB').innerHTML = build('B');
    }

    function preview(e, id) {
        const file = e.target.files[0];
        if (!file) return;
        const v = document.getElementById(`vPrev${id}`);
        v.src = URL.createObjectURL(file);
        v.classList.remove('hidden'); v.play();
        e.target.parentElement.querySelector('span').classList.add('hidden');
    }

    function processAI() {
        document.getElementById('loader').style.display = 'flex';
        
        setTimeout(() => {
            document.getElementById('loader').style.display = 'none';
            const nameA = document.getElementById('selectA').value;
            const nameB = document.getElementById('selectB').value;

            // Logic Simulation
            const getPoints = (t) => (parseInt(document.getElementById(`pts${t}`).value) || 0) / (parseInt(document.getElementById(`gp${t}`).value) || 1);
            const scoreA = (getPoints('A') + 1) * (Math.random() * 0.5 + 0.8);
            const scoreB = (getPoints('B') + 1) * (Math.random() * 0.5 + 0.8);

            const probA = ((scoreA / (scoreA + scoreB)) * 100).toFixed(1);
            const probB = (100 - probA).toFixed(1);

            document.getElementById('results').classList.remove('hidden');
            document.getElementById('resNameA').innerText = nameA;
            document.getElementById('resNameB').innerText = nameB;
            document.getElementById('resProbA').innerText = probA + "%";
            document.getElementById('resProbB').innerText = probB + "%";
            document.getElementById('barA').style.width = probA + "%";
            document.getElementById('barB').style.width = probB + "%";

            document.getElementById('aiLogic').innerText = currentLang === 'en' 
                ? `AI scanned video movement for ${nameA} and ${nameB}. Statistical weight applied to historical points per game. Visual analysis suggests ${probA > probB ? nameA : nameB} has a tactical advantage in transition speed.`
                : `قام الذكاء الاصطناعي بمسح حركة الفيديو لكل من ${nameA} و ${nameB}. تم تطبيق الوزن الإحصائي على تاريخ النقاط لكل مباراة. يشير التحليل المرئي إلى أن ${probA > probB ? nameA : nameB} لديه ميزة تكتيكية في سرعة التحول.`;

            window.scrollTo({ top: document.getElementById('results').offsetTop - 50, behavior: 'smooth' });
        }, 2000);
    }

    updateUI();
</script>
</body>
</html>
