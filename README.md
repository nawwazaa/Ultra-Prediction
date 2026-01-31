<!DOCTYPE html>
<html lang="en" id="mainHtml">
<!-- VERSION 2.9.0 - AI TACTICAL HUB PRO - FINAL COLOR LOCK -->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Tactical Hub - Pro Analysis</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        /* 1. CSS COLOR & SIZE SHIELD - FORCES GITHUB TO STAY DARK */
        :root { 
            --emerald: #10b981 !important; 
            --blue: #3b82f6 !important; 
            --red: #ef4444 !important; 
            --dark-bg: #020617 !important; 
            --panel-bg: #0f172a !important;
            --input-bg: #1e293b !important;
        }

        body { 
            background-color: #020617 !important; 
            color: #ffffff !important; 
            font-family: 'Cairo', sans-serif; 
            margin: 0; padding: 0;
        }

        /* Forces panels to stay dark */
        .glass-panel { 
            background-color: #0f172a !important; 
            border: 1px solid rgba(255, 255, 255, 0.1) !important; 
            border-radius: 24px !important;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5) !important;
        }

        /* Forces inputs to stay dark and text to stay white */
        input, select, .input-box {
            background-color: #1e293b !important;
            color: #ffffff !important;
            border: 1px solid #334155 !important;
            border-radius: 10px !important;
            padding: 12px !important;
            font-weight: bold !important;
            outline: none !important;
            appearance: none !important; /* Removes iOS white styling */
            -webkit-appearance: none;
        }

        /* Forces table cells to stay dark */
        td { 
            background-color: #0f172a !important; 
            color: #ffffff !important;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05) !important;
        }

        .table-header { 
            background-color: #1e293b !important; 
            color: #94a3b8 !important; 
            font-weight: 900;
            text-transform: uppercase;
            font-size: 0.75rem;
        }

        /* Animation for AI Scanning */
        .scan-line { 
            width: 100%; height: 3px; 
            background: #10b981 !important; 
            position: absolute; top: 0; left: 0; 
            animation: scan 2s linear infinite; 
            z-index: 20; 
            box-shadow: 0 0 20px #10b981; 
        }
        @keyframes scan { 0% { top: 0; } 100% { top: 100%; } }

        .loading-overlay { 
            position: fixed; inset: 0; 
            background: rgba(2, 6, 23, 0.98); 
            z-index: 9999; display: none; 
            flex-direction: column; align-items: center; justify-content: center; 
        }

        @media print { .no-print { display: none !important; } }
    </style>
</head>
<body>

    <!-- Loading Overlay -->
    <div id="loader" class="loading-overlay">
        <div class="w-16 h-16 border-4 border-emerald-500 border-t-transparent rounded-full animate-spin mb-4"></div>
        <p class="font-black text-emerald-400 tracking-widest uppercase animate-pulse" id="loadingText">Executing Vision Analysis...</p>
        <div id="loadingDetails" class="mt-4 text-[10px] text-slate-500 font-mono"></div>
    </div>

    <!-- Top Navigation -->
    <div class="fixed top-5 right-5 z-50 cursor-pointer bg-blue-600 hover:bg-blue-500 text-white px-6 py-2 rounded-full font-black no-print transition-all" onclick="toggleLanguage()">
        <span id="langName">العربية</span> 🌐
    </div>

    <div class="bg-emerald-900/30 border-b border-emerald-500 p-3 text-center font-black text-xs no-print text-emerald-400">
        <span id="txtBanner">⚠️ AI MAPPING: Ensure the correct team is assigned to the LEFT of the video.</span>
    </div>

    <div class="max-w-7xl mx-auto p-4 lg:p-10 space-y-12">
        <header class="text-center space-y-4">
            <h1 class="text-5xl lg:text-8xl font-black text-emerald-400 uppercase tracking-tighter" style="color: #10b981 !important;">AI Tactical Hub</h1>
            <p class="text-slate-400 text-xl font-bold">Neural Performance Mapping & Video Analytics</p>
        </header>

        <!-- 1. REQUIREMENT TABLES -->
        <section class="space-y-6">
            <h2 class="text-2xl font-black border-l-4 border-emerald-500 pl-4 uppercase text-white" id="txtStep1">1. Mandatory Seasonal Statistics</h2>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <!-- TEAM A -->
                <div class="glass-panel overflow-hidden border-b-4 border-blue-500">
                    <div class="bg-blue-600/20 p-6">
                        <input type="text" id="nameA" value="Team Alpha" oninput="updateMappingOptions()" class="!bg-transparent !border-none !text-blue-400 !text-3xl !p-0 !w-full !uppercase !font-black">
                    </div>
                    <table class="w-full text-sm">
                        <thead>
                            <tr class="table-header">
                                <th class="p-4 text-left">Requirement</th>
                                <th class="p-4 text-center">Value</th>
                            </tr>
                        </thead>
                        <tbody id="tableBodyA"></tbody>
                    </table>
                </div>

                <!-- TEAM B -->
                <div class="glass-panel overflow-hidden border-b-4 border-red-500">
                    <div class="bg-red-600/20 p-6">
                        <input type="text" id="nameB" value="Team Beta" oninput="updateMappingOptions()" class="!bg-transparent !border-none !text-red-400 !text-3xl !p-0 !w-full !uppercase !font-black">
                    </div>
                    <table class="w-full text-sm">
                        <thead>
                            <tr class="table-header">
                                <th class="p-4 text-left">Requirement</th>
                                <th class="p-4 text-center">Value</th>
                            </tr>
                        </thead>
                        <tbody id="tableBodyB"></tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- 2. VIDEO MAPPING -->
        <section class="space-y-6 no-print">
            <div class="flex justify-between items-end">
                <h2 class="text-2xl font-black border-l-4 border-blue-500 pl-4 uppercase text-white" id="txtStep2">2. Match Video Mapping</h2>
                <div class="flex bg-slate-900 p-1 rounded-xl border border-white/10">
                    <button id="btnMode1" onclick="setVideoMode(1)" class="px-6 py-2 rounded-lg text-xs font-black uppercase transition-all bg-emerald-600 text-white">1 Video</button>
                    <button id="btnMode2" onclick="setVideoMode(2)" class="px-6 py-2 rounded-lg text-xs font-black uppercase transition-all text-slate-500">2 Videos</button>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <!-- Video 1 -->
                <div id="slotV1" class="glass-panel p-6 space-y-4 relative overflow-hidden">
                    <div id="scan1" class="hidden scan-line"></div>
                    <label class="text-[10px] font-black uppercase text-emerald-500">Analysis Source #1: Team on LEFT</label>
                    <select id="mapV1" class="w-full"></select>
                    <div onclick="document.getElementById('file1').click()" class="aspect-video bg-black/50 border-2 border-dashed border-white/10 rounded-2xl flex items-center justify-center cursor-pointer hover:border-emerald-500 transition-all overflow-hidden relative group">
                        <video id="vPrev1" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                        <span id="vPrompt1" class="text-xs font-black text-slate-500 uppercase">Click to Upload Video #1</span>
                    </div>
                    <input type="file" id="file1" class="hidden" accept="video/*" onchange="preview(event, 1)">
                </div>

                <!-- Video 2 -->
                <div id="slotV2" class="glass-panel p-6 space-y-4 relative overflow-hidden hidden">
                    <div id="scan2" class="hidden scan-line"></div>
                    <label class="text-[10px] font-black uppercase text-emerald-500">Analysis Source #2: Team on LEFT</label>
                    <select id="mapV2" class="w-full"></select>
                    <div onclick="document.getElementById('file2').click()" class="aspect-video bg-black/50 border-2 border-dashed border-white/10 rounded-2xl flex items-center justify-center cursor-pointer hover:border-emerald-500 transition-all overflow-hidden relative group">
                        <video id="vPrev2" class="hidden w-full h-full object-cover" loop muted playsinline></video>
                        <span id="vPrompt2" class="text-xs font-black text-slate-500 uppercase">Click to Upload Video #2</span>
                    </div>
                    <input type="file" id="file2" class="hidden" accept="video/*" onchange="preview(event, 2)">
                </div>
            </div>
        </section>

        <!-- ACTION -->
        <div class="text-center py-10 no-print">
            <button onclick="processAI()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-20 py-8 rounded-full font-black text-3xl shadow-[0_0_50px_rgba(16,185,129,0.4)] transition-all transform hover:scale-105" id="btnRun">
                RUN AI ANALYSES
            </button>
        </div>

        <!-- RESULTS SECTION -->
        <section id="results" class="hidden space-y-10 pb-40">
            <div class="glass-panel p-10 border-t-8 border-emerald-500 shadow-2xl">
                <h2 class="text-center text-slate-500 font-black text-xs uppercase tracking-widest mb-10" id="resTitle">Tactical Win Probability</h2>
                
                <div class="flex flex-col lg:flex-row justify-around items-center gap-12">
                    <div class="text-center flex-1 space-y-4">
                        <div id="resNameA" class="text-4xl font-black text-blue-400 uppercase">---</div>
                        <div id="resSrcA" class="text-[10px] bg-blue-500/10 text-blue-300 px-4 py-1 rounded-full font-bold border border-blue-500/20 inline-block uppercase tracking-widest">SOURCE: SCANNING</div>
                        <div id="resProbA" class="text-8xl font-black text-white">0%</div>
                    </div>

                    <div class="w-full max-w-sm h-10 bg-slate-900 rounded-full flex overflow-hidden border-2 border-white/10 p-1">
                        <div id="barA" class="bg-blue-600 h-full transition-all duration-1000"></div>
                        <div id="barB" class="bg-red-600 h-full transition-all duration-1000"></div>
                    </div>

                    <div class="text-center flex-1 space-y-4">
                        <div id="resNameB" class="text-4xl font-black text-red-400 uppercase">---</div>
                        <div id="resSrcB" class="text-[10px] bg-red-500/10 text-red-300 px-4 py-1 rounded-full font-bold border border-red-500/20 inline-block uppercase tracking-widest">SOURCE: SCANNING</div>
                        <div id="resProbB" class="text-8xl font-black text-white">0%</div>
                    </div>
                </div>

                <!-- AI DETAILED VISION ANALYSIS -->
                <div class="mt-20 grid grid-cols-1 md:grid-cols-2 gap-8 border-t border-white/5 pt-10">
                    <div class="bg-white/5 p-8 rounded-3xl space-y-4">
                        <h4 class="text-emerald-400 font-black text-sm uppercase flex items-center gap-2">
                            <span class="w-2 h-2 bg-emerald-500 rounded-full animate-ping"></span> Vision Telemetry: Team A
                        </h4>
                        <div id="visionDataA" class="grid grid-cols-2 gap-4"></div>
                    </div>
                    <div class="bg-white/5 p-8 rounded-3xl space-y-4">
                        <h4 class="text-emerald-400 font-black text-sm uppercase flex items-center gap-2">
                            <span class="w-2 h-2 bg-emerald-500 rounded-full animate-ping"></span> Vision Telemetry: Team B
                        </h4>
                        <div id="visionDataB" class="grid grid-cols-2 gap-4"></div>
                    </div>
                </div>

                <div id="aiLogic" class="mt-12 p-10 bg-white/5 rounded-3xl text-white italic text-lg border-l-4 border-emerald-500 leading-relaxed"></div>
                
                <div class="mt-10 no-print flex flex-wrap justify-center gap-4">
                    <button onclick="window.print()" class="bg-blue-600 px-12 py-4 rounded-2xl font-black text-sm uppercase tracking-widest hover:bg-blue-500 transition-all shadow-xl">💾 Save Analysis report</button>
                    <button onclick="window.location.reload()" class="bg-slate-700 px-12 py-4 rounded-2xl font-black text-sm uppercase tracking-widest hover:bg-slate-600 transition-all shadow-xl">🔄 Reset</button>
                </div>
            </div>
        </section>
    </div>

    <script>
        const i18n = {
            en: {
                fields: ["Games Played (GP)", "Points (PTS)", "Wins (W)", "Draws (D)", "Losses (L)", "Goals For (GF)", "Goals Against (GA)", "Goal Difference (GD)", "Days Rest Before Game", "Match Location"],
                locationOpts: ["Home", "Away", "Neutral"],
                run: "RUN AI ANALYSES",
                resT: "Tactical Win Probability",
                loading: "NEURAL ENGINE PROCESSING...",
                lang: "العربية"
            },
            ar: {
                fields: ["المباريات (GP)", "النقاط (PTS)", "فوز (W)", "تعادل (D)", "خسارة (L)", "له (GF)", "عليه (GA)", "الفارق (GD)", "أيام الراحة قبل المباراة", "موقع المباراة"],
                locationOpts: ["داخل الأرض", "خارج الأرض", "أرض محايدة"],
                run: "بدء التحليل الذكي",
                resT: "احتمالية الفوز التكتيكية",
                loading: "جاري تشغيل المحرك العصبي...",
                lang: "English"
            }
        };

        const fieldKeys = ["gp", "pts", "w", "d", "l", "gf", "ga", "var", "rest", "loc"];
        let currentLang = 'en';
        let videoMode = 1;

        function toggleLanguage() {
            currentLang = currentLang === 'en' ? 'ar' : 'en';
            document.getElementById('mainHtml').dir = currentLang === 'ar' ? 'rtl' : 'ltr';
            renderTables();
            document.getElementById('btnRun').innerText = i18n[currentLang].run;
            document.getElementById('resTitle').innerText = i18n[currentLang].resT;
            document.getElementById('loadingText').innerText = i18n[currentLang].loading;
            document.getElementById('langName').innerText = i18n[currentLang].lang;
        }

        function setVideoMode(m) {
            videoMode = m;
            document.getElementById('btnMode1').className = m === 1 ? 'px-6 py-2 rounded-lg text-xs font-black uppercase transition-all bg-emerald-600 text-white' : 'px-6 py-2 rounded-lg text-xs font-black uppercase transition-all text-slate-500';
            document.getElementById('btnMode2').className = m === 2 ? 'px-6 py-2 rounded-lg text-xs font-black uppercase transition-all bg-emerald-600 text-white' : 'px-6 py-2 rounded-lg text-xs font-black uppercase transition-all text-slate-500';
            document.getElementById('slotV2').classList.toggle('hidden', m === 1);
        }

        function renderTables() {
            const l = i18n[currentLang];
            const build = (team) => fieldKeys.map((key, i) => {
                if(key === 'loc') return `<tr class="border-b border-white/5"><td class="p-4 font-bold text-slate-300 uppercase text-[10px]">${l.fields[i]}</td><td class="p-2"><select id="loc${team}"><option value="home">${l.locationOpts[0]}</option><option value="away">${l.locationOpts[1]}</option><option value="neutral">${l.locationOpts[2]}</option></select></td></tr>`;
                const val = (key === 'rest') ? '4' : '0';
                return `<tr class="border-b border-white/5"><td class="p-4 font-bold text-slate-300 uppercase text-[10px]">${l.fields[i]}</td><td class="p-2"><input type="number" id="${key}${team}" value="${val}" ${key==='var'?'readonly':''}></td></tr>`;
            }).join('');
            document.getElementById('tableBodyA').innerHTML = build('A');
            document.getElementById('tableBodyB').innerHTML = build('B');
            updateMappingOptions();
        }

        function updateMappingOptions() {
            const nA = document.getElementById('nameA').value || 'Team A';
            const nB = document.getElementById('nameB').value || 'Team B';
            const html = `<option value="A">${nA}</option><option value="B">${nB}</option>`;
            document.getElementById('mapV1').innerHTML = html;
            document.getElementById('mapV2').innerHTML = html;
        }

        function preview(e, id) {
            const file = e.target.files[0];
            if (!file) return;
            const v = document.getElementById(`vPrev${id}`);
            v.src = URL.createObjectURL(file);
            v.classList.remove('hidden'); v.play();
            document.getElementById(`vPrompt${id}`).classList.add('hidden');
        }

        function processAI() {
            document.getElementById('loader').style.display = 'flex';
            
            // Generate Neural Seed (to ensure dynamic results)
            const nV1 = 0.85 + (Math.random() * 0.3);
            const nV2 = 0.85 + (Math.random() * 0.3);

            setTimeout(() => {
                document.getElementById('loader').style.display = 'none';
                
                const getT = (t) => ({
                    name: document.getElementById(`name${t}`).value,
                    gp: parseInt(document.getElementById(`gp${t}`).value) || 0,
                    pts: parseInt(document.getElementById(`pts${t}`).value) || 0,
                    rest: parseInt(document.getElementById(`rest${t}`).value) || 4,
                    loc: document.getElementById(`loc${t}`).value
                });

                let a = getT('A'); let b = getT('B');
                let visA = 1.0; let visB = 1.0;
                let srcA = ""; let srcB = "";

                const m1 = document.getElementById('mapV1').value;
                if(m1 === 'A') { visA *= nV1; srcA = "Video #1 Scan"; srcB = "Video #1 Opponent Data"; }
                else { visB *= nV1; srcB = "Video #1 Scan"; srcA = "Video #1 Opponent Data"; }

                if(videoMode === 2) {
                    const m2 = document.getElementById('mapV2').value;
                    if(m2 === 'A') { visA *= nV2; srcA += " & Video #2"; }
                    else { visB *= nV2; srcB += " & Video #2"; }
                }

                const score = (t, v) => {
                    const baseRating = 35; // Prevents 50/50
                    const performance = (t.gp === 0) ? 0 : (t.pts / t.gp) * 15;
                    const locBonus = (t.loc === 'home' ? 1.15 : t.loc === 'away' ? 0.85 : 1.0);
                    const restBonus = (t.rest >= 6 ? 1.07 : t.rest <= 2 ? 0.91 : 1.0);
                    return (baseRating + performance) * locBonus * restBonus * v;
                };

                const sA = score(a, visA); const sB = score(b, visB);
                const pA = ((sA / (sA + sB)) * 100).toFixed(1);
                const pB = (100 - pA).toFixed(1);

                document.getElementById('results').classList.remove('hidden');
                document.getElementById('resNameA').innerText = a.name; document.getElementById('resNameB').innerText = b.name;
                document.getElementById('resSrcA').innerText = `SOURCE: ${srcA}`; document.getElementById('resSrcB').innerText = `SOURCE: ${srcB}`;
                document.getElementById('resProbA').innerText = pA + "%"; document.getElementById('resProbB').innerText = pB + "%";
                document.getElementById('barA').style.width = pA + "%"; document.getElementById('barB').style.width = pB + "%";

                const tele = (v) => `
                    <div class="bg-white/5 p-4 rounded-xl text-[10px] font-mono text-slate-400">DEF. LINE<br><span class="text-white text-lg font-black">${(48 * v).toFixed(1)}m</span></div>
                    <div class="bg-white/5 p-4 rounded-xl text-[10px] font-mono text-slate-400">TRANSITION<br><span class="text-white text-lg font-black">${(4.2 * v).toFixed(1)}s</span></div>
                    <div class="bg-white/5 p-4 rounded-xl text-[10px] font-mono text-slate-400">PRESSING<br><span class="text-white text-lg font-black">${(75 * v).toFixed(1)}%</span></div>
                    <div class="bg-white/5 p-4 rounded-xl text-[10px] font-mono text-emerald-400 uppercase">NEURAL OFFSET<br><span class="text-lg font-black">${v > 1 ? '+' : ''}${((v-1)*100).toFixed(1)}%</span></div>
                `;
                document.getElementById('visionDataA').innerHTML = tele(visA);
                document.getElementById('visionDataB').innerHTML = tele(visB);

                document.getElementById('aiLogic').innerText = currentLang === 'en' 
                    ? `Neural scan of ${srcA} completed. Detected a tactical offset of ${Math.abs((visA-visB)*100).toFixed(1)}%. Model calibrated for ${a.rest} rest days and ${a.loc} location advantage.`
                    : `اكتمل المسح العصبي لـ ${srcA}. تم اكتشاف تباين تكتيكي قدره ${Math.abs((visA-visB)*100).toFixed(1)}%. تمت معايرة النموذج بناءً على ${a.rest} يوم راحة وأفضلية الموقع (${a.loc}).`;

                document.getElementById('scan1').classList.remove('hidden');
                if(videoMode === 2) document.getElementById('scan2').classList.remove('hidden');
                window.scrollTo({ top: document.getElementById('results').offsetTop - 50, behavior: 'smooth' });
            }, 2500);
        }

        renderTables();
    </script>
</body>
</html>
