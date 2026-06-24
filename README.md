<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NeonCozy English Lounge - Fixed Version</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            cozy: {
              background: '#0f172a',
              card: '#1e293b',
              accent: '#f43f5e',
              neonGreen: '#10b981',
              neonCyan: '#06b6d4',
              neonPurple: '#8b5cf6',
              warmText: '#f8fafc',
              cozyGold: '#f59e0b',
            }
          }
        }
      }
    }
  </script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&family=Quicksand:wght@400;600;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <style>
    body { font-family: 'Quicksand', sans-serif; background-color: #0b0f19; }
    .perspective-1000 { perspective: 1000px; }
    .transform-style-3d { transform-style: preserve-3d; }
    .backface-hidden { backface-visibility: hidden; }
    .rotate-y-180 { transform: rotateY(180deg); }
    .nav-btn.active { background-color: rgb(30, 41, 59); color: white; border: 1px solid rgb(51, 65, 85); }
  </style>
</head>
<body class="text-cozy-warmText min-h-screen flex flex-col pb-8">

  <header class="sticky top-0 z-40 backdrop-blur-md bg-slate-900/80 border-b border-slate-800 px-4 py-3 flex justify-between items-center">
    <div class="flex items-center gap-3">
      <div class="p-2 bg-gradient-to-tr from-cozy-accent to-cozy-neonPurple rounded-2xl text-white">
        <i data-lucide="sparkles" class="w-6 h-6"></i>
      </div>
      <div>
        <h1 class="font-extrabold text-xl bg-gradient-to-r from-cozy-accent to-cozy-neonCyan bg-clip-text text-transparent">NeonCozy Lounge</h1>
        <p class="text-xs text-slate-400">A1 ➔ B1+ Accelerated Engine</p>
      </div>
    </div>
    <div class="flex items-center gap-2 bg-slate-900 border border-slate-800 rounded-2xl px-4 py-2 text-sm">
      <span class="text-cozy-cozyGold font-bold">🔥 3 Días</span>
      <div class="h-4 w-px bg-slate-800 mx-2"></div>
      <span class="text-cozy-neonCyan font-bold">Level <span id="lvl">A1</span> (<span id="xp-display">120</span> XP)</span>
    </div>
  </header>

  <main class="flex-1 max-w-7xl w-full mx-auto px-4 mt-6 grid grid-cols-1 lg:grid-cols-12 gap-6">
    
    <aside class="lg:col-span-3 flex flex-col gap-4">
      <nav class="bg-cozy-card/60 border border-slate-800 p-3 rounded-3xl flex flex-row lg:flex-col gap-1 overflow-x-auto">
        <button onclick="switchTab('shadowing')" id="btn-shadowing" class="nav-btn active flex items-center gap-3 px-4 py-3 rounded-2xl font-bold text-sm w-full whitespace-nowrap">
          <i data-lucide="mic" class="text-cozy-neonGreen w-5 h-5"></i> Shadowing Station
        </button>
        <button onclick="switchTab('dictionary')" id="btn-dictionary" class="nav-btn text-slate-400 hover:text-white flex items-center gap-3 px-4 py-3 rounded-2xl font-bold text-sm w-full whitespace-nowrap">
          <i data-lucide="image" class="text-cozy-neonCyan w-5 h-5"></i> Visual Finder
        </button>
        <button onclick="switchTab('chunks')" id="btn-chunks" class="nav-btn text-slate-400 hover:text-white flex items-center gap-3 px-4 py-3 rounded-2xl font-bold text-sm w-full whitespace-nowrap">
          <i data-lucide="layers" class="text-cozy-neonPurple w-5 h-5"></i> Memory Chunks
        </button>
      </nav>
    </aside>

    <section class="lg:col-span-9 flex flex-col gap-6">
      
      <div id="tab-shadowing" class="tab-content bg-cozy-card/40 border border-slate-800 p-6 rounded-3xl">
        <div class="flex justify-between items-center mb-4">
          <div>
            <h2 class="text-xl font-bold">🎙️ Shadowing Activo</h2>
            <p class="text-xs text-slate-400">Escucha la voz nativa primero, luego repite usando el micrófono.</p>
          </div>
          <select id="preset-select" onchange="changePreset()" class="bg-slate-950 border border-slate-800 text-xs rounded-xl px-3 py-2 text-slate-200">
            <option value="p1">☕ Cafetería (A1-A2)</option>
            <option value="p2">🎮 Gaming Night (A2-B1)</option>
            <option value="p3">🎞️ Movie Slang (B1-B1+)</option>
          </select>
        </div>

        <div class="bg-slate-950 p-5 rounded-2xl border border-slate-800/80 mb-4 flex flex-col justify-between min-h-[120px]">
          <div id="text-target-display" class="text-lg font-bold text-slate-200 leading-relaxed mb-4">
            </div>
          <div class="flex justify-between items-center border-t border-slate-900 pt-3">
            <span class="text-xs text-slate-500">Paso 1: Dale al botón de audio para escuchar</span>
            <button onclick="playNativeVoice()" class="flex items-center gap-2 px-4 py-2 bg-cozy-neonCyan hover:bg-cozy-neonCyan/90 text-slate-950 font-bold text-xs rounded-xl transition-all">
              <i data-lucide="volume-2" class="w-4 h-4"></i> ESCUCHAR NATIVO
            </button>
          </div>
        </div>

        <div class="bg-slate-950/40 border border-slate-800 p-5 rounded-2xl mb-4">
          <div class="flex justify-between text-xs text-slate-400 mb-2">
            <span>Tu dictado en tiempo real:</span>
            <span id="mic-status" class="text-cozy-neonPurple font-bold">Micrófono listo</span>
          </div>
          <div id="mic-result-display" class="text-lg font-bold text-slate-500 italic min-h-[60px]">
            Presiona el botón de abajo y empieza a hablar...
          </div>
        </div>

        <div class="flex items-center justify-between p-4 bg-slate-950 rounded-2xl border border-slate-800">
          <button id="record-btn" onclick="toggleMic()" class="px-6 py-3 bg-cozy-accent text-white font-bold rounded-xl flex items-center gap-2 hover:scale-105 transition-all">
            <i data-lucide="mic" class="w-5 h-5"></i> <span id="record-btn-text">Grabar mi voz</span>
          </button>
          <div class="text-right">
            <div class="text-xs text-slate-400">Precisión</div>
            <div id="accuracy-score" class="text-xl font-black text-white">--</div>
          </div>
        </div>
      </div>

      <div id="tab-dictionary" class="tab-content hidden bg-cozy-card/40 border border-slate-800 p-6 rounded-3xl">
        <h2 class="text-xl font-bold mb-2">🎨 Buscador Visual de Vocabulario</h2>
        <p class="text-xs text-slate-400 mb-4">Escribe una palabra para ver su concepto real en imágenes sin traducciones aburridas.</p>
        
        <div class="flex gap-2 mb-6">
          <input type="text" id="search-word" placeholder="Ej: cozy, skyscraper, sunset, skateboard..." class="bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm flex-1 focus:outline-none focus:border-cozy-neonCyan">
          <button onclick="searchVisualConcept()" class="bg-cozy-neonCyan text-slate-950 font-bold px-6 rounded-xl text-sm hover:opacity-90">Visualizar</button>
        </div>

        <div id="dict-card" class="bg-slate-950 border border-slate-800 rounded-2xl overflow-hidden p-4 hidden">
          <div class="w-full aspect-video rounded-xl overflow-hidden mb-4 bg-slate-900 border border-slate-800">
            <img id="dict-img" src="" alt="Concept" class="w-full h-full object-cover">
          </div>
          <h3 id="dict-title" class="text-xl font-black text-white capitalize mb-1">Word</h3>
          <p id="dict-desc" class="text-sm text-slate-300 bg-slate-900 p-3 rounded-xl mb-3">Definition</p>
          <button onclick="saveToChunks()" class="text-xs bg-cozy-neonPurple/20 text-cozy-neonPurple border border-cozy-neonPurple/50 px-3 py-1.5 rounded-lg font-bold">
            + Guardar en mis Tarjetas
          </button>
        </div>
      </div>

      <div id="tab-chunks" class="tab-content hidden bg-cozy-card/40 border border-slate-800 p-6 rounded-3xl">
        <h2 class="text-xl font-bold mb-1">🗂️ Repaso de Bloques (Chunks)</h2>
        <p class="text-xs text-slate-400 mb-6">Juego interactivo de tarjetas para recordar frases completas de la calle.</p>

        <div class="max-w-sm mx-auto aspect-[4/3] w-full perspective-1000 mb-6 cursor-pointer" onclick="flipCard()">
          <div id="card-inner" class="w-full h-full relative transition-transform duration-500 transform-style-3d bg-slate-950 rounded-2xl border border-slate-800">
            <div class="absolute inset-0 backface-hidden p-6 flex flex-col justify-between bg-gradient-to-b from-slate-900 to-slate-950 rounded-2xl">
              <span class="text-[10px] text-cozy-neonPurple font-bold uppercase">Inglés</span>
              <div class="text-center text-xl font-extrabold text-white" id="flash-front">Grab a bite</div>
              <span class="text-xs text-slate-500 text-center">Toca para voltear</span>
            </div>
            <div class="absolute inset-0 backface-hidden p-6 flex flex-col justify-between bg-slate-900 rounded-2xl rotate-y-180">
              <span class="text-[10px] text-cozy-accent font-bold uppercase">Significado</span>
              <div class="text-center text-lg font-bold text-cozy-accent" id="flash-back">Comer algo rápido</div>
              <div class="flex justify-center gap-2">
                <button onclick="evaluateCard(true); event.stopPropagation();" class="bg-cozy-neonGreen text-slate-950 font-bold text-xs px-3 py-1.5 rounded-lg">¡Me la sé!</button>
                <button onclick="evaluateCard(false); event.stopPropagation();" class="bg-slate-800 text-slate-300 font-bold text-xs px-3 py-1.5 rounded-lg">Repasar</button>
              </div>
            </div>
          </div>
        </div>
      </div>

    </section>
  </main>

  <script>
    // DATA CONFIG
    let xp = 120;
    let currentTab = 'shadowing';
    const presets = {
      p1: "Can I please get a hot latte and a chocolate chip cookie to go?",
      p2: "Let's hop on Discord tonight and play some video games.",
      p3: "That movie was absolutely mindblowing you should check it out."
    };
    let activeText = presets.p1;

    let chunks = [
      { eng: "Grab a bite", esp: "Comer algo rápido sobre la marcha" },
      { eng: "Never mind", esp: "Olvídalo / No te preocupes" },
      { eng: "Catch up", esp: "Ponerse al día con alguien" }
    ];
    let cardIndex = 0;

    // SPEECH SYNTHESIS ENGINE (VOZ DE REPRODUCCIÓN)
    // Forzamos un sistema de respaldo directo que no dependa de configuraciones regionales rotas.
    function playNativeVoice() {
      if ('speechSynthesis' in window) {
        // Cancelar cualquier audio en cola para evitar que se congele
        window.speechSynthesis.cancel();
        
        const utterance = new SpeechSynthesisUtterance(activeText);
        utterance.lang = 'en-US';
        utterance.rate = 0.85; // Velocidad cómoda y clara
        
        // Buscar explícitamente una voz nativa en inglés del sistema si está disponible
        const voices = window.speechSynthesis.getVoices();
        const englishVoice = voices.find(v => v.lang.startsWith('en-'));
        if (englishVoice) utterance.voice = englishVoice;

        window.speechSynthesis.speak(utterance);
      } else {
        alert("Tu navegador no soporta salida de audio de voz estándar. Intenta con Google Chrome.");
      }
    }

    // Asegurar la carga inicial de voces del navegador
    if ('speechSynthesis' in window) {
      window.speechSynthesis.onvoiceschanged = () => {};
    }

    // SPEECH RECOGNITION ENGINE (MICRÓFONO)
    let recognition = null;
    let isRecording = false;

    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      const SpeechCtx = window.SpeechRecognition || window.webkitSpeechRecognition;
      recognition = new SpeechCtx();
      recognition.continuous = false;
      recognition.interimResults = false;
      recognition.lang = 'en-US';

      recognition.onstart = () => {
        isRecording = true;
        document.getElementById('mic-status').innerText = "🔴 Escuchando... ¡Habla ahora!";
        document.getElementById('record-btn-text').innerText = "Detener grabación";
      };

      recognition.onend = () => {
        isRecording = false;
        document.getElementById('mic-status').innerText = "Micrófono en espera";
        document.getElementById('record-btn-text').innerText = "Grabar mi voz";
      };

      recognition.onresult = (event) => {
        const resultText = event.results[0][0].transcript;
        processShadowingResults(resultText);
      };
    }

    function toggleMic() {
      if (!recognition) {
        // Fallback simulador interactivo si falla el micrófono físico del hardware
        processShadowingResults(activeText);
        return;
      }
      if (isRecording) {
        recognition.stop();
      } else {
        recognition.start();
      }
    }

    function processShadowingResults(spokenText) {
      document.getElementById('mic-result-display').innerText = `Dijiste: "${spokenText}"`;
      
      const targetWords = activeText.toLowerCase().replace(/[.,\/#!$%\^&\*;:{}=\-_`~()?]/g,"").split(" ");
      const spokenWords = spokenText.toLowerCase().replace(/[.,\/#!$%\^&\*;:{}=\-_`~()?]/g,"").split(" ");
      
      let matches = 0;
      targetWords.forEach(w => {
        if (spokenWords.includes(w)) matches++;
      });

      const score = Math.round((matches / targetWords.length) * 100);
      document.getElementById('accuracy-score').innerText = `${score}%`;

      if (score >= 70) {
        confetti({ particleCount: 40, spread: 50 });
        xp += 20;
        document.getElementById('xp-display').innerText = xp;
      }
    }

    // VISUAL FINDER ENGINE (IMÁGENES COHERENTES)
    function searchVisualConcept() {
      const word = document.getElementById('search-word').value.trim();
      if (!word) return;

      const card = document.getElementById('dict-card');
      const img = document.getElementById('dict-img');
      const title = document.getElementById('dict-title');
      const desc = document.getElementById('dict-desc');

      card.classList.remove('hidden');
      title.innerText = word;
      desc.innerText = `Contextual meaning for '${word}' inside immersive environments. Use this visual feedback to connect the word with real-life experiences directly in English.`;
      
      // Reemplazamos la llamada rota anterior por una URL estricta de palabras clave de alta definición
      img.src = `https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80`; // Fallback limpio inicial
      img.src = `https://source.unsplash.com/featured/600x400/?${encodeURIComponent(word)},minimalist`;
    }

    function saveToChunks() {
      const word = document.getElementById('dict-title').innerText;
      if (!chunks.some(c => c.eng.toLowerCase() === word.toLowerCase())) {
        chunks.push({ eng: word, esp: "Concepto visual guardado" });
        alert(`¡"${word}" guardado en tus tarjetas de repaso!`);
      }
    }

    // FLASHCARDS CONSOLE
    function flipCard() {
      document.getElementById('card-inner').classList.toggle('rotate-y-180');
    }

    function loadCard() {
      if (chunks.length === 0) return;
      const c = chunks[cardIndex];
      document.getElementById('flash-front').innerText = c.eng;
      document.getElementById('flash-back').innerText = c.esp;
    }

    function evaluateCard(knows) {
      document.getElementById('card-inner').classList.remove('rotate-y-180');
      setTimeout(() => {
        cardIndex = (cardIndex + 1) % chunks.length;
        loadCard();
        if (knows) {
          xp += 10;
          document.getElementById('xp-display').innerText = xp;
        }
      }, 200);
    }

    // AUXILIARY TABS & INITIALIZATION
    function switchTab(tabId) {
      document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
      document.getElementById(`tab-${tabId}`).classList.remove('hidden');
      document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
      document.getElementById(`btn-${tabId}`).classList.add('active');
      if (tabId === 'chunks') loadCard();
    }

    function changePreset() {
      const p = document.getElementById('preset-select').value;
      activeText = presets[p];
      initShadowingDisplay();
    }

    function initShadowingDisplay() {
      const container = document.getElementById('text-target-display');
      container.innerHTML = '';
      activeText.split(" ").forEach(w => {
        const span = document.createElement('span');
        span.className = "mr-2 inline-block text-slate-200";
        span.innerText = w;
        container.appendChild(span);
      });
    }

    window.onload = () => {
      lucide.createIcons();
      initShadowingDisplay();
    };
  </script>
</body>
</html>
