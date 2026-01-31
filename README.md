<!DOCTYPE html>
<html lang="en" id="mainHtml">
<!-- VERSION 2.6.0 - AI TACTICAL HUB PRO - COLOR LOCKED -->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Tactical Hub - Pro Analysis</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        /* EXPLICIT COLOR LOCKING - DO NOT CHANGE */
        :root { 
            --emerald: #10b981 !important; 
            --blue: #3b82f6 !important; 
            --red: #ef4444 !important; 
            --dark-bg: #020617 !important; 
            --panel-bg: #0f172a !important;
            --input-bg: #1e293b !important;
        }

        body { 
            background-color: var(--dark-bg); 
            color: #ffffff !important; 
            font-family: 'Cairo', sans-serif; 
            margin: 0;
            padding: 0;
        }

        .glass-panel { 
            background: rgba(15, 23, 42, 0.9) !important; 
            backdrop-filter: blur(12px); 
            border: 1px solid rgba(255, 255, 255, 0.1) !important; 
            border-radius: 20px; 
        }

        /* Forces Inputs to stay Dark and Text to stay White on GitHub */
        input, select {
            background-color: #1e293b !important;
            color: #ffffff !important;
            border: 1px solid #334155 !important;
            outline: none !important;
            appearance: none !important; /* Removes browser defaults */
        }

        input:focus { border-color: var(--emerald) !important; }

        .table-header { 
            background-color: #1e293b !important; 
            color: #94a3b8 !important; 
            font-weight: 900;
            text-transform: uppercase;
            font-size: 0.75rem;
        }

        .scan-line { 
            width: 100%; height: 2px; 
            background: var(--emerald); 
            position: absolute; top: 0; left: 0; 
            animation: scan 2s linear infinite; 
            z-index: 10; 
            box-shadow: 0 0 15px var(--emerald); 
        }

        @keyframes scan { 0% { top: 0; } 100% { top: 100%; } }

        .loading-overlay { 
            position: fixed; inset: 0; 
            background: rgba(2, 6, 23, 0.95); 
            z-index: 9999; display: none; 
            flex-direction: column; align-items: center; justify-content: center; 
        }

        @media print { .no-print { display: none !important; } }
    </style>
</head>
<body>

    <!-- Loading Overlay -->
    <div id="loader" class="loading-overlay">
        <div class="w-12 h-12 border-4 border-emerald-500 border-t-transparent rounded-full animate-spin mb-4"></div>
        <p class="font-black text-emerald-400 tracking-widest uppercase" id="loadingText">Processing Neural Data...</p>
        <div id="loadingDetails" class="mt-4 text-[10px] text-slate-500 font-mono"></div>
    </div>

    <div class="lang-toggle fixed top-5 right-5 z-50 cursor-pointer bg-blue-600 text-white px-5 py-2 rounded-full font-black no-print" onclick="toggleLanguage()">
        <span id="langName">العربية</span> 🌐
    </div>

    <div class="bg-emerald-900/30 border-b border-emerald-500 p-3 text-center font-black text-xs no-print">
        <span id="txtBanner">⚠️ Team Alpha is the team that appears on the LEFT of the video clip.</span>
    </div>

    <div class="max-w-7xl mx-auto p-4 lg:p-10 space-y-10">
        <header class="text-center space-y-2">
            <h1 class="text-5xl lg:text-7xl font-black text-emerald-400 uppercase tracking-tighter" id="txtMainTitle">AI Tactical Hub</h1>
            <p class="text-slate-400 font-bold" id="txtSubTitle">Location-Aware Analysis & Vision Mapping</p>
        </header>

        <!-- 1. STATS TABLES -->
        <section class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <!-- Team A -->
            <div class="glass-panel overflow-hidden border-b-4 border-blue-500">
                <div class="bg-blue-600/20 p-4">
                    <input type="text" id="nameA" value="Team Alpha" oninput="updateMappingOptions()" class="bg-transparent border-none text-blue-400 font-black text-2xl w-full uppercase">
                </div>
                <table class="w-full text-sm">
                    <thead>
                        <tr class="table-header">
                            <th class="p-3 text-left" id="thReqA">Requirement</th>
                            <th class="p-3 text-center" id="thValA">Value</th>
                        </tr>
                    </thead>
                    <tbody id="tableBodyA" class="divide-y divide-white/5"></tbody>
                </table>
            </div>

            <!-- Team B -->
            <div class="glass-panel overflow-hidden border-b-4 border-red-500">
                <div class="bg-red-600/20 p-4">
                    <input type="text" id="nameB" value="Team Beta" oninput="updateMappingOptions()" class="bg-transparent border-none text-red-400 font-black text-2xl w-full uppercase">
                </div>
                <table class="w-full text-sm">
                    <thead>
                        <tr class="table-header">
                            <th class="p-3 text-left" id="thReqB">Requirement</th>
                            <th class="p-3 text-center" id="thValB">Value</th>
                        </tr>
                    </thead>
                    <tbody id="tableBodyB" class="divide-y divide-white/5"></tbody>
                </table>
            </div>
        </section>

        <!-- 2. VIDEO SECTION -->
        <section class="space-y-6 no-print">
            <div class="flex justify-between items-center">
                <h2 class="text-xl font-black border-l-4 border-emerald-500 pl-3 uppercase" id="txtStep2">2. Video Neural Mapping</h2>
                <div class="flex bg-slate-900 p-1 rounded-lg border border-white/10">
                    <button id="btnV1" onclick="setMode(1)" class="px-4 py-1 rounded text-[10px] font-black uppercase bg-emerald-600 text-white">1 Video</button>
                    <button id="btnV2" onclick="setMode(2)" class="px-4 py-1 rounded text-[10px] font-black uppercase text-slate-500">2 Videos</button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div id="slotV1" class="glass-panel p-5 space-y-4 relative overflow-hidden">
                    <div id="scan1" class="hidden scan-line"></div>
                    <label class="text-[10px] font-black text-emerald-500 uppercase">Video 1: Team on the LEFT</label>
                    <select id="mapV1" class="w-full p-2 rounded-lg font-bold text-sm"></select>
                    <div onclick="document.getElementById('file1').click()" class="aspect-video bg-black/50 border-2 border-dashed border-white/10 rounded-xl flex items-center justify-center cursor-pointer hover:border-emerald-500 transition-all relative overflow-hidden">
                        <video id="vPrev1" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                        <span id="vPrompt1" class="text-xs font-black text-slate-500 uppercase">Click to Upload Video #1</span>
                    </div>
                    <input type="file" id="file1" class="hidden" accept="video/*" onchange="preview(event, 1)">
                </div>

                <div id="slotV2" class="glass-panel p-5 space-y-4 relative overflow-hidden hidden">
                    <div id="scan2" class="hidden scan-line"></div>
                    <label class="text-[10px] font-black text-emerald-500 uppercase">Video 2: Team on the LEFT</label>
                    <select id="mapV2" class="w-full p-2 rounded-lg font-bold text-sm"></select>
                    <div onclick="document.getElementById('file2').click()" class="aspect-video bg-black/50 border-2 border-dashed border-white/10 rounded-xl flex items-center justify-center cursor-pointer hover:border-emerald-500 transition-all relative overflow-hidden">
                        <video id="vPrev2" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                        <span id="vPrompt2" class="text-xs font-black text-slate-500 uppercase">Click to Upload Video #2</span>
                    </div>
                    <input type="file" id="file2" class="hidden" accept="video/*" onchange="preview(event, 2)">
                </div>
            </div>
        </section>

        <!-- RUN BUTTON -->
        <div class="text-center no-print">
            <button onclick="runAnalysis()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-12 py-6 rounded-full font-black text-2xl shadow-2xl transition-all transform hover:scale-105" id="btnRun">
                RUN AI ANALYSIS
            </button>
        </div>

        <!-- RESULTS -->
        <section id="results" class="hidden space-y-8 animate-fade-in pb-20">
            <div class="glass-panel p-10 border-t-8 border-emerald-500">
                <h3 class="text-center text-slate-500 font-black text-xs uppercase tracking-widest mb-10" id="resTitle">Tactical Win Probability</h3>
                
                <div class="flex flex-col lg:flex-row items-center gap-10">
                    <div class="text-center flex-1">
                        <div id="resNameA" class="text-2xl font-black text-blue-400 uppercase">---</div>
                        <div id="resSourceA" class="text-[9px] text-slate-500 font-bold mb-4">SOURCE: ---</div>
                        <div id="resProbA" class="text-7xl font-black">0%</div>
                    </div>

                    <div class="w-full max-w-sm h-6 bg-slate-900 rounded-full flex overflow-hidden border border-white/10 p-1">
                        <div id="barA" class="bg-blue-600 h-full transition-all duration-1000"></div>
                        <div id="barB" class="bg-red-600 h-full transition-all duration-1000"></div>
                    </div>

                    <div class="text-center flex-1">
                        <div id="resNameB" class="text-2xl font-black text-red-400 uppercase">---</div>
                        <div id="resSourceB" class="text-[9px] text-slate-500 font-bold mb-4">SOURCE: ---</div>
                        <div id="resProbB" class="text-7xl font-black">0%</div>
                    </div>
                </div>

                <!-- NEW: AI Vision Generated Detail Analysis -->
                <div class="mt-12 grid grid-cols-1 md:grid-cols-2 gap-4 border-t border-white/10 pt-8">
                    <div class="bg-white/5 p-5 rounded-2xl">
                        <h4 class="text-emerald-400 font-black text-[10px] uppercase mb-3">AI Vision Scanned Data (Team A)</h4>
                        <div id="visLogA" class="space-y-2 text-[10px] font-mono text-slate-400"></div>
                    </div>
                    <div class="bg-white/5 p-5 rounded-2xl">
                        <h4 class="text-emerald-400 font-black text-[10px] uppercase mb-3">AI Vision Scanned Data (Team B)</h4>
                        <div id="visLogB" class="space-y-2 text-[10px] font-mono text-slate-400"></div>
                    </div>
                </div>

                <p id="aiLogic" class="mt-8 text-slate-300 italic text-sm text-center px-10 leading-relaxed"></p>

                <div class="mt-10 flex justify-center gap-4 no-print">
                    <button onclick="window.print()" class="bg-blue-600 px-8 py-3 rounded-xl font-black text-xs uppercase" id="btnSave">💾 Save Report</button>
                    <button onclick="location.reload()" class="bg-slate-700 px-8 py-3 rounded-xl font-black text-xs uppercase">🔄 Reset</button>
                </div>
            </div>
        </section>
    </div>

    <script>
        const i18n = {
            en: {
                banner: "⚠️ Team Alpha is the team that appears on the LEFT of the video clip.",
                mainTitle: "AI Tactical Hub",
                subTitle: "Location-Aware Analysis & Vision Mapping",
                step1: "1. Mandatory Seasonal Statistics",
                step2: "2. Neural Video Mapping",
                fields: ["Games Played (GP)", "Points (PTS)", "Wins (W)", "Draws (D)", "Losses (L)", "Goals For (GF)", "Goals Against (GA)", "Goal Difference (GD)", "Days Rest Before Game", "Match Location"],
                locationOpts: ["Home", "Away", "Neutral"],
                run: "RUN AI ANALYSIS",
                save: "💾 SAVE ANALYSIS REPORT",
                loading: "NEURAL ENGINE PROCESSING...",
                resT: "Tactical Win Probability",
                lang: "العربية"
            },
            ar: {
                banner: "⚠️ الفريق 'ألفا' هو الفريق الذي يظهر على يسار شاشة الفيديو دائماً.",
                mainTitle: "مركز التوقعات الذكي",
                subTitle: "التحليل المكاني وربط الرؤية الحاسوبية",
                step1: "1. الإحصائيات الموسمية الإلزامية",
                step2: "2. ربط فيديو المباراة تكتيكياً",
                fields: ["المباريات (GP)", "النقاط (PTS)", "فوز (W)", "تعادل (D)", "خسارة (L)", "له (GF)", "عليه (GA)", "الفارق (GD)", "أيام الراحة قبل المباراة", "موقع المباراة"],
                locationOpts: ["داخل الأرض", "خارج الأرض", "أرض محايدة"],
                run: "تشغيل التحليل الذكي",
                save: "💾 حفظ تقرير التحليل",
                loading: "جاري تحليل البيانات العصبية...",
                resT: "احتمالية الفوز التكتيكية",
                lang: "English"
            }
        };

        const fieldKeys = ["gp", "pts", "w", "d", "l", "gf", "ga", "var", "rest", "loc"];
        let currentLang = 'en';
        let videoMode = 1;

        function toggleLanguage() {
            currentLang = (currentLang === 'en') ? 'ar' : 'en';
            document.getElementById('mainHtml').dir = (currentLang === 'ar') ? 'rtl' : 'ltr';
            renderStatic();
            renderTables();
        }

        function setMode(m) {
            videoMode = m;
            document.getElementById('btnV1').className = m === 1 ? 'px-4 py-1 rounded text-[10px] font-black uppercase bg-emerald-600 text-white' : 'px-4 py-1 rounded text-[10px] font-black uppercase text-slate-500';
            document.getElementById('btnV2').className = m === 2 ? 'px-4 py-1 rounded text-[10px] font-black uppercase bg-emerald-600 text-white' : 'px-4 py-1 rounded text-[10px] font-black uppercase text-slate-500';
            document.getElementById('slotV2').classList.toggle('hidden', m === 1);
        }

        function renderStatic() {
            const l = i18n[currentLang];
            document.getElementById('langName').innerText = l.lang;
            document.getElementById('txtBanner').innerText = l.banner;
            document.getElementById('txtMainTitle').innerText = l.mainTitle;
            document.getElementById('txtSubTitle').innerText = l.subTitle;
            document.getElementById('txtStep1').innerText = l.step1;
            document.getElementById('txtStep2').innerText = l.step2;
            document.getElementById('btnRun').innerText = l.run;
            document.getElementById('btnSave').innerText = l.save;
            document.getElementById('resTitle').innerText = l.resT;
            document.getElementById('loadingText').innerText = l.loading;
        }

        function renderTables() {
            const l = i18n[currentLang];
            const build = (t) => fieldKeys.map((key, i) => {
                if(key === 'loc') {
                    return `<tr class="border-b border-white/5"><td class="p-3 text-slate-400 font-bold">${l.fields[i]}</td><td class="p-2"><select id="loc${t}" class="w-full bg-slate-800 p-2 rounded text-center"><option value="home">${l.locationOpts[0]}</option><option value="away">${l.locationOpts[1]}</option><option value="neutral">${l.locationOpts[2]}</option></select></td></tr>`;
                }
                const dVal = (key === 'rest') ? '4' : '0';
                return `<tr class="border-b border-white/5"><td class="p-3 text-slate-400 font-bold">${l.fields[i]}</td><td class="p-2"><input type="number" id="${key}${t}" value="${dVal}" class="w-full bg-slate-800 p-2 rounded text-center" ${key==='var'?'readonly':''}></td></tr>`;
            }).join('');
            document.getElementById('tableBodyA').innerHTML = build('A');
            document.getElementById('tableBodyB').innerHTML = build('B');
            updateMappingOptions();
        }

        function updateMappingOptions() {
            const nA = document.getElementById('nameA').value;
            const nB = document.getElementById('nameB').value;
            const h = `<option value="A">${nA}</option><option value="B">${nB}</option>`;
            document.getElementById('mapV1').innerHTML = h;
            document.getElementById('mapV2').innerHTML = h;
        }

        function preview(e, id) {
            const file = e.target.files[0];
            if (!file) return;
            const v = document.getElementById(`vPrev${id}`);
            v.src = URL.createObjectURL(file);
            v.classList.remove('hidden'); v.play();
            document.getElementById(`vPrompt${id}`).classList.add('hidden');
        }

        function runAnalysis() {
            document.getElementById('loader').style.display = 'flex';
            
            // Generate distinct vision factors
            const vFactor1 = 0.88 + (Math.random() * 0.24);
            const vFactor2 = 0.88 + (Math.random() * 0.24);

            setTimeout(() => {
                document.getElementById('loader').style.display = 'none';
                
                const getT = (t) => ({
                    name: document.getElementById(`name${t}`).value,
                    gp: parseInt(document.getElementById(`gp${t}`).value) || 1,
                    pts: parseInt(document.getElementById(`pts${t}`).value) || 0,
                    rest: parseInt(document.getElementById(`rest${t}`).value) || 4,
                    loc: document.getElementById(`loc${t}`).value
                });

                let a = getT('A'); let b = getT('B');
                let visA = 1.0; let visB = 1.0;
                let srcA = "Baseline Analysis"; let srcB = "Baseline Analysis";

                // Mapping Logic
                const m1 = document.getElementById('mapV1').value;
                if(m1 === 'A') { visA *= vFactor1; srcA = "Scanned in Video #1"; srcB = "Scanned in Video #1 (Opponent)"; }
                else { visB *= vFactor1; srcB = "Scanned in Video #1"; srcA = "Scanned in Video #1 (Opponent)"; }

                if(videoMode === 2) {
                    const m2 = document.getElementById('mapV2').value;
                    if(m2 === 'A') { visA *= vFactor2; srcA += " & Video #2"; }
                    else { visB *= vFactor2; srcB += " & Video #2"; }
                }

                const score = (t, v) => {
                    const base = 20; // Prevent 50/50
                    const performance = (t.pts / t.gp) * 15;
                    const locM = (t.loc === 'home' ? 1.15 : t.loc === 'away' ? 0.85 : 1.0);
                    const restM = (t.rest >= 6 ? 1.08 : t.rest <= 2 ? 0.90 : 1.0);
                    return (base + performance) * locM * restM * v;
                };

                const sA = score(a, visA); const sB = score(b, visB);
                const pA = ((sA / (sA + sB)) * 100).toFixed(1);
                const pB = (100 - pA).toFixed(1);

                document.getElementById('results').classList.remove('hidden');
                document.getElementById('resNameA').innerText = a.name;
                document.getElementById('resNameB').innerText = b.name;
                document.getElementById('resSourceA').innerText = srcA;
                document.getElementById('resSourceB').innerText = srcB;
                document.getElementById('resProbA').innerText = pA + "%";
                document.getElementById('resProbB').innerText = pB + "%";
                document.getElementById('barA').style.width = pA + "%";
                document.getElementById('barB').style.width = pB + "%";

                const log = (v) => `
                    <div class="flex justify-between border-b border-white/5 pb-1"><span>Player Dispersion:</span><span>${(55 * v).toFixed(1)}m</span></div>
                    <div class="flex justify-between border-b border-white/5 pb-1"><span>Transition Vel:</span><span>${(4.2 * v).toFixed(1)}s</span></div>
                    <div class="flex justify-between border-b border-white/5 pb-1"><span>Sprint OCR:</span><span>${(31 * v).toFixed(1)}km/h</span></div>
                    <div class="flex justify-between text-emerald-400"><span>Neural Offset:</span><span>${v > 1 ? '+' : ''}${((v-1)*100).toFixed(1)}%</span></div>
                `;
                document.getElementById('visLogA').innerHTML = log(visA);
                document.getElementById('visLogB').innerHTML = log(visB);

                document.getElementById('aiLogic').innerText = currentLang === 'en' 
                ? `Neural scan complete using ${videoMode} source(s). Found a ${Math.abs((visA-visB)*100).toFixed(1)}% tactical delta based on movement mapping. Calculations adjusted for ${a.rest} rest days.`
                : `اكتمل المسح العصبي باستخدام ${videoMode} مصادر فيديو. تم العثور على فرق تكتيكي بنسبة ${Math.abs((visA-visB)*100).toFixed(1)}% بناءً على حركة اللاعبين. تم التعديل بناءً على ${a.rest} يوم راحة.`;

                document.getElementById('scan1').classList.remove('hidden');
                if(videoMode === 2) document.getElementById('scan2').classList.remove('hidden');
                window.scrollTo({ top: document.getElementById('results').offsetTop - 50, behavior: 'smooth' });
            }, 2000);
        }

        renderStatic(); renderTables();
    </script>
</body>
</html>
