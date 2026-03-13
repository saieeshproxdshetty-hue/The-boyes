<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SQUAD_HQ // SECURE TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700;800&family=Inter:wght@400;700;900&display=swap');
        
        :root { --emerald: #10b981; --bg: #050505; --card: rgba(20, 20, 20, 0.8); }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--bg); 
            color: #e2e8f0; 
            overflow-x: hidden; 
            margin: 0;
            background-image: radial-gradient(circle at 50% 50%, rgba(16, 185, 129, 0.05) 0%, transparent 80%);
        }

        .mono { font-family: 'JetBrains Mono', monospace; }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: gearShift 0.4s ease-out; }
        
        @keyframes gearShift { 
            from { opacity: 0; transform: translateY(10px); filter: blur(5px); } 
            to { opacity: 1; transform: translateY(0); filter: blur(0); } 
        }

        .glass-card { 
            background: var(--card);
            backdrop-filter: blur(20px); 
            border: 1px solid rgba(255, 255, 255, 0.05); 
            transition: all 0.3s ease;
        }

        #aim-canvas-container { 
            width: 100%; height: 60vh; background: #000; border-radius: 2rem; overflow: hidden; cursor: crosshair; border: 1px solid rgba(16, 185, 129, 0.2); 
        }

        /* Setup Overlay */
        #setup-overlay {
            position: fixed; inset: 0; background: black; z-index: 200;
            display: flex; align-items: center; justify-content: center;
        }
    </style>
</head>
<body class="selection:bg-emerald-500/30">

    <!-- API KEY SETUP OVERLAY -->
    <div id="setup-overlay">
        <div class="max-w-md w-full p-10 glass-card rounded-[3rem] text-center border-emerald-500/30">
            <div class="w-16 h-16 bg-emerald-500/10 rounded-2xl flex items-center justify-center mx-auto mb-6">
                <i data-lucide="shield-check" class="text-emerald-500 w-8 h-8"></i>
            </div>
            <h2 class="text-3xl font-black italic uppercase text-white mb-2">Secure Link</h2>
            <p class="text-slate-400 text-sm mb-8 leading-relaxed">Enter your Gemini API key to activate Neural Scrapping. This is stored locally in your browser only.</p>
            <input type="password" id="api-key-input" placeholder="AIzaSy..." class="w-full bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-emerald-500 mono text-sm mb-4 focus:outline-none focus:border-emerald-500">
            <button onclick="saveKey()" class="w-full bg-emerald-500 text-black py-4 rounded-xl font-black uppercase tracking-widest text-xs hover:scale-105 transition-all">Initialize Terminal</button>
            <p class="text-[10px] text-slate-600 mt-6 mono uppercase">Note: GitHub will never see this key.</p>
        </div>
    </div>

    <!-- MAIN APP (HIDDEN UNTIL KEY SET) -->
    <div id="main-terminal" class="opacity-0 transition-opacity duration-1000">
        <div class="fixed top-0 w-full h-10 border-b border-emerald-500/10 bg-black/50 backdrop-blur-md z-[110] flex items-center px-6 justify-between">
            <span class="mono text-[9px] text-emerald-500 font-bold uppercase tracking-[0.3em] flex items-center gap-2">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span> Terminal_Live
            </span>
            <button onclick="clearKey()" class="text-[9px] mono text-slate-500 hover:text-red-500 uppercase">Reset_Key</button>
        </div>

        <!-- Navigation -->
        <nav class="fixed bottom-0 left-0 w-full lg:top-0 lg:left-0 lg:w-20 h-20 lg:h-full bg-black border-t lg:border-r lg:border-t-0 border-white/10 z-[120] flex lg:flex-col items-center justify-around lg:justify-center lg:gap-8">
            <button onclick="switchTab('dashboard')" class="nav-btn p-4 text-emerald-500" id="btn-dashboard"><i data-lucide="layout-grid"></i></button>
            <button onclick="switchTab('reddit')" class="nav-btn p-4 text-slate-500" id="btn-reddit"><i data-lucide="zap"></i></button>
            <button onclick="switchTab('trainer')" class="nav-btn p-4 text-slate-500" id="btn-trainer"><i data-lucide="target"></i></button>
        </nav>

        <main class="lg:ml-20 pt-16 pb-28 px-6 max-w-7xl mx-auto">
            <!-- DASHBOARD -->
            <section id="dashboard" class="tab-content active pt-10">
                <h1 class="text-7xl md:text-9xl font-black italic uppercase tracking-tighter text-white leading-none mb-12">SQUAD<br><span class="text-emerald-500">HQ_</span></h1>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div onclick="switchTab('reddit')" class="glass-card p-10 rounded-[3rem] cursor-pointer group">
                        <i data-lucide="zap" class="text-orange-500 w-10 h-10 mb-6 group-hover:scale-110 transition-transform"></i>
                        <h3 class="text-4xl font-black italic uppercase text-white">Intel_Feed</h3>
                    </div>
                    <div onclick="switchTab('trainer')" class="glass-card p-10 rounded-[3rem] cursor-pointer group">
                        <i data-lucide="target" class="text-emerald-500 w-10 h-10 mb-6 group-hover:scale-110 transition-transform"></i>
                        <h3 class="text-4xl font-black italic uppercase text-white">Reflex_Sim</h3>
                    </div>
                </div>
            </section>

            <!-- REDDIT -->
            <section id="reddit" class="tab-content space-y-8">
                <div class="flex justify-between items-end">
                    <h2 class="text-5xl font-black uppercase italic text-white">Neural_Intel</h2>
                    <button onclick="fetchReddit()" class="bg-white/5 border border-white/10 px-6 py-3 rounded-xl hover:bg-emerald-500/10 transition-all">
                        <i data-lucide="refresh-cw" id="refresh-icon" class="w-4 h-4 text-emerald-500"></i>
                    </button>
                </div>
                <div id="reddit-container" class="space-y-4 max-w-4xl"></div>
            </section>

            <!-- TRAINER -->
            <section id="trainer" class="tab-content space-y-6">
                <div class="flex justify-between items-end">
                    <h2 class="text-5xl font-black uppercase italic text-white">Reflex_Sim</h2>
                    <div class="text-right">
                        <p class="mono text-[10px] text-emerald-500 uppercase">Score</p>
                        <p id="aim-score" class="text-5xl font-black text-white leading-none">00</p>
                    </div>
                </div>
                <div id="aim-canvas-container">
                    <div id="aim-overlay" class="absolute inset-0 bg-black/90 flex items-center justify-center z-20">
                        <button onclick="startSimulation()" class="bg-emerald-500 text-black px-12 py-4 rounded-xl font-black uppercase">Start_Sim</button>
                    </div>
                </div>
            </section>
        </main>
    </div>

    <script>
        let USER_KEY = localStorage.getItem('squad_hq_key');
        
        // Initial Security Check
        window.onload = () => {
            if (USER_KEY) {
                unlockTerminal();
            }
            lucide.createIcons();
        };

        function saveKey() {
            const input = document.getElementById('api-key-input').value;
            if (input.length < 20) return; // Simple validation
            localStorage.setItem('squad_hq_key', input);
            USER_KEY = input;
            unlockTerminal();
        }

        function clearKey() {
            localStorage.removeItem('squad_hq_key');
            location.reload();
        }

        function unlockTerminal() {
            document.getElementById('setup-overlay').style.display = 'none';
            document.getElementById('main-terminal').classList.remove('opacity-0');
        }

        function switchTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.replace('text-emerald-500', 'text-slate-500'));
            document.getElementById('btn-' + id).classList.replace('text-slate-500', 'text-emerald-500');
            if(id === 'reddit') fetchReddit();
            lucide.createIcons();
        }

        async function fetchReddit() {
            const container = document.getElementById('reddit-container');
            const icon = document.getElementById('refresh-icon');
            icon.classList.add('animate-spin');
            
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${USER_KEY}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: "Top 5 Reddit posts for Warzone and MW3 meta. Output JSON array: [{author, title, content, subreddit}]." }] }],
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });
                const data = await response.json();
                const posts = JSON.parse(data.candidates[0].content.parts[0].text);
                container.innerHTML = posts.map(p => `
                    <div class="glass-card p-6 rounded-3xl">
                        <p class="text-[10px] mono text-emerald-500 uppercase mb-2">${p.subreddit} // ${p.author}</p>
                        <h4 class="text-xl font-bold text-white mb-2">${p.title}</h4>
                        <p class="text-sm text-slate-400 mb-4">${p.content}</p>
                    </div>
                `).join('');
            } catch (e) {
                container.innerHTML = `<p class="mono text-red-500 text-center py-10">Neural Uplink Failed. Check Key.</p>`;
            } finally {
                icon.classList.remove('animate-spin');
            }
        }

        // Simple Aim Trainer logic
        let score = 0, scene, camera, renderer, targets = [];
        function startSimulation() {
            document.getElementById('aim-overlay').style.display = 'none';
            initSim();
        }
        function initSim() {
            const container = document.getElementById('aim-canvas-container');
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, container.clientWidth/container.clientHeight, 0.1, 1000);
            camera.position.z = 10;
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(container.clientWidth, container.clientHeight);
            container.appendChild(renderer.domElement);
            scene.add(new THREE.AmbientLight(0xffffff, 0.5));
            for(let i=0; i<5; i++) spawnTarget();
            const animate = () => {
                requestAnimationFrame(animate);
                targets.forEach(t => t.rotation.y += 0.05);
                renderer.render(scene, camera);
            };
            animate();
            container.addEventListener('mousedown', (e) => {
                const rect = container.getBoundingClientRect();
                const mouse = new THREE.Vector2(((e.clientX - rect.left)/container.clientWidth)*2-1, -((e.clientY - rect.top)/container.clientHeight)*2+1);
                const raycaster = new THREE.Raycaster();
                raycaster.setFromCamera(mouse, camera);
                const hits = raycaster.intersectObjects(targets);
                if(hits.length > 0) {
                    scene.remove(hits[0].object);
                    targets = targets.filter(t => t !== hits[0].object);
                    score++; document.getElementById('aim-score').innerText = score.toString().padStart(2, '0');
                    spawnTarget();
                }
            });
        }
        function spawnTarget() {
            const t = new THREE.Mesh(new THREE.IcosahedronGeometry(0.6, 0), new THREE.MeshPhongMaterial({color: 0x10b981, wireframe: true}));
            t.position.set((Math.random()-0.5)*15, (Math.random()-0.5)*8, 0);
            targets.push(t); scene.add(t);
        }
    </script>
</body>
</html>

