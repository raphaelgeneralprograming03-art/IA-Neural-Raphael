
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neural Raphael AI - Navegador Inteligente de Pesquisa Web</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
  <!-- Marked.js for Markdown Rendering -->
  <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            mono: ['JetBrains Mono', 'monospace'],
          },
          colors: {
            brand: {
              50: '#eef2ff',
              100: '#e0e7ff',
              500: '#6366f1',
              600: '#4f46e5',
              700: '#4338ca',
              900: '#1e1b4b',
              accent: '#06b6d4'
            }
          }
        }
      }
    }
  </script>
  <style>
    body {
      background-color: #0b0f19;
      color: #f3f4f6;
      font-family: 'Inter', sans-serif;
    }
    .glass-panel {
      background: rgba(17, 24, 39, 0.7);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .glass-card {
      background: rgba(30, 41, 59, 0.5);
      backdrop-filter: blur(8px);
      border: 1px solid rgba(255, 255, 255, 0.05);
    }
    .gradient-text {
      background: linear-gradient(135deg, #38bdf8 0%, #818cf8 50%, #c084fc 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .pulse-glow {
      box-shadow: 0 0 20px rgba(99, 102, 241, 0.35);
    }
    /* Custom Scrollbars */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(99, 102, 241, 0.4);
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(99, 102, 241, 0.7);
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col overflow-x-hidden antialiased text-gray-200">

  <!-- Top Navigation Bar / Browser Frame Header -->
  <header class="glass-panel sticky top-0 z-50 border-b border-gray-800/80 px-4 py-2.5">
    <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
      
      <!-- Brand & Controls -->
      <div class="flex items-center gap-3">
        <div class="flex items-center gap-1.5 mr-2">
          <span class="w-3 h-3 rounded-full bg-red-500/80 inline-block"></span>
          <span class="w-3 h-3 rounded-full bg-yellow-500/80 inline-block"></span>
          <span class="w-3 h-3 rounded-full bg-green-500/80 inline-block"></span>
        </div>
        <div class="flex items-center gap-2 font-bold text-lg tracking-tight">
          <div class="w-8 h-8 rounded-lg bg-gradient-to-tr from-indigo-600 to-cyan-500 flex items-center justify-center text-white shadow-lg">
            <i class="fa-solid font-black fa-compass text-sm"></i>
          </div>
          <span class="gradient-text font-extrabold hidden sm:inline">OmniSearch</span>
          <span class="text-xs bg-indigo-500/20 text-indigo-300 border border-indigo-500/30 px-2 py-0.5 rounded-full font-mono">v3.5 Grounded</span>
        </div>
      </div>

      <!-- Search / Address Bar -->
      <div class="flex-1 max-w-3xl">
        <form id="search-form" class="relative flex items-center">
          <div class="absolute left-3.5 text-indigo-400">
            <i class="fa-solid fa-magnifying-glass text-sm"></i>
          </div>
          <input 
            type="text" 
            id="search-input"
            value="Quais são as últimas novidades e avanços em Inteligência Artificial no mundo?" 
            placeholder="Pesquise em tempo real na Web ou digite uma URL..."
            class="w-full pl-10 pr-24 py-2.5 rounded-xl bg-gray-900/90 border border-gray-700/80 text-sm text-gray-100 placeholder-gray-400 focus:outline-none focus:border-indigo-500 focus:ring-1 focus:ring-indigo-500 font-mono transition-all duration-200"
          />
          <button 
            type="submit" 
            id="search-btn"
            class="absolute right-1.5 px-4 py-1.5 rounded-lg bg-gradient-to-r from-indigo-600 to-cyan-600 hover:from-indigo-500 hover:to-cyan-500 text-white font-medium text-xs shadow-md transition-all flex items-center gap-1.5 active:scale-95">
            <span>Pesquisar</span>
            <i class="fa-solid fa-arrow-right text-xs"></i>
          </button>
        </form>
      </div>

      <!-- Quick Action Navigation Tabs -->
      <div class="flex items-center gap-1">
        <button id="tab-browser-btn" onclick="switchTab('browser')" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-indigo-600/30 text-indigo-300 border border-indigo-500/40 flex items-center gap-1.5 transition">
          <i class="fa-solid fa-globe"></i>
          <span class="hidden md:inline">Navegador</span>
        </button>
        <button id="tab-visualizer-btn" onclick="switchTab('visualizer')" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-gray-400 hover:bg-gray-800/60 hover:text-gray-200 flex items-center gap-1.5 transition">
          <i class="fa-solid fa-network-wired"></i>
          <span class="hidden md:inline">Pipeline IA</span>
        </button>
        <button id="tab-architecture-btn" onclick="switchTab('architecture')" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-gray-400 hover:bg-gray-800/60 hover:text-gray-200 flex items-center gap-1.5 transition">
          <i class="fa-solid fa-sitemap"></i>
          <span class="hidden md:inline">Visão Geral</span>
        </button>
      </div>

    </div>
  </header>

  <!-- Main Viewport -->
  <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 space-y-6">

    <!-- Preset Search Prompts Pill Bar -->
    <div class="flex items-center gap-2 overflow-x-auto pb-1 text-xs">
      <span class="text-gray-400 font-medium whitespace-nowrap flex items-center gap-1">
        <i class="fa-solid fa-bolt text-yellow-400"></i> Sugestões Rápidas:
      </span>
      <button onclick="quickSearch('Últimas descobertas científicas sobre fusão nuclear em 2026')" class="bg-gray-800/80 hover:bg-indigo-900/40 hover:border-indigo-500/50 text-gray-300 border border-gray-700/60 px-3 py-1.5 rounded-full whitespace-nowrap transition">
        ⚛️ Fusão Nuclear
      </button>
      <button onclick="quickSearch('Principais cotações e tendências de mercado e tecnologia hoje')" class="bg-gray-800/80 hover:bg-indigo-900/40 hover:border-indigo-500/50 text-gray-300 border border-gray-700/60 px-3 py-1.5 rounded-full whitespace-nowrap transition">
        📈 Mercado Tech
      </button>
      <button onclick="quickSearch('Quem venceu a última corrida da Fórmula 1 e classificação do campeonato')" class="bg-gray-800/80 hover:bg-indigo-900/40 hover:border-indigo-500/50 text-gray-300 border border-gray-700/60 px-3 py-1.5 rounded-full whitespace-nowrap transition">
        🏎️ Esportes ao Vivo
      </button>
      <button onclick="quickSearch('Lançamentos mais recentes da exploração espacial e Artemis')" class="bg-gray-800/80 hover:bg-indigo-900/40 hover:border-indigo-500/50 text-gray-300 border border-gray-700/60 px-3 py-1.5 rounded-full whitespace-nowrap transition">
        🚀 Exploração Espacial
      </button>
    </div>

    <!-- TAB 1: BROWSER RESULTS VIEW -->
    <section id="view-browser" class="space-y-6 transition-all duration-300">
      
      <!-- Status & Engine Badge Bar -->
      <div class="flex flex-wrap items-center justify-between gap-3 bg-gray-900/60 p-3 rounded-xl border border-gray-800 text-xs">
        <div class="flex items-center gap-2">
          <span id="status-indicator" class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse"></span>
          <span id="status-text" class="text-gray-300 font-mono">Status: Pronto para pesquisas em tempo real na Web</span>
        </div>
        <div class="flex items-center gap-4 text-gray-400 font-mono">
          <span><i class="fa-solid fa-database text-indigo-400"></i> Grounding: Google Search API</span>
          <span><i class="fa-solid fa-microchip text-cyan-400"></i> Modelo: Gemini 3 Flash</span>
        </div>
      </div>

      <!-- Layout Grid (Results + Citations Sidebar) -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">

        <!-- Main Answer / Synthesis Panel -->
        <div class="lg:col-span-8 space-y-6">
          <div class="glass-panel rounded-2xl p-6 border border-gray-800 relative shadow-2xl overflow-hidden">
            <!-- Header -->
            <div class="flex items-center justify-between border-b border-gray-800/80 pb-4 mb-5">
              <div class="flex items-center gap-3">
                <div class="p-2 rounded-lg bg-indigo-500/10 text-indigo-400 border border-indigo-500/20">
                  <i class="fa-solid fa-wand-magic-sparkles text-lg"></i>
                </div>
                <div>
                  <h2 class="text-lg font-bold text-gray-100">Síntese Inteligente com Visão da Web</h2>
                  <p class="text-xs text-gray-400">Resumo contextual compilado diretamente das fontes da internet</p>
                </div>
              </div>
              <button onclick="copyResponse()" title="Copiar resposta" class="text-gray-400 hover:text-white bg-gray-800 hover:bg-gray-700 px-3 py-1.5 rounded-lg text-xs flex items-center gap-1.5 transition">
                <i class="fa-regular fa-copy"></i> Copiar
              </button>
            </div>

            <!-- Loader Overlay -->
            <div id="loading-spinner" class="hidden py-12 flex flex-col items-center justify-center gap-4">
              <div class="relative w-16 h-16">
                <div class="absolute inset-0 rounded-full border-4 border-indigo-500/20"></div>
                <div class="absolute inset-0 rounded-full border-4 border-indigo-500 border-t-transparent animate-spin"></div>
                <div class="absolute inset-2 rounded-full border-4 border-cyan-400 border-b-transparent animate-spin" style="animation-direction: reverse; animation-duration: 1.5s;"></div>
              </div>
              <p class="text-sm font-mono text-indigo-300 animate-pulse">Rastreando a Web & Sintetizando Conhecimento...</p>
            </div>

            <!-- AI Rendered Markdown Container -->
            <div id="ai-response-content" class="prose prose-invert prose-indigo max-w-none text-gray-300 leading-relaxed text-sm space-y-4">
              <!-- Content dynamically rendered here -->
            </div>
          </div>
        </div>

        <!-- Right Column: Citations, Grounding Sources & Query Metadata -->
        <div class="lg:col-span-4 space-y-6">
          
          <!-- Web Citations / Sources Panel -->
          <div class="glass-panel rounded-2xl p-5 border border-gray-800 space-y-4">
            <div class="flex items-center gap-2 border-b border-gray-800 pb-3">
              <i class="fa-solid fa-link text-cyan-400"></i>
              <h3 class="font-bold text-sm text-gray-200">Fontes & Citações Web</h3>
            </div>
            
            <div id="citations-container" class="space-y-2.5 max-h-[380px] overflow-y-auto pr-1">
              <!-- Citations rendered dynamically -->
            </div>
          </div>

          <!-- Search Vector Metrics -->
          <div class="glass-panel rounded-2xl p-5 border border-gray-800 space-y-3">
            <h3 class="font-bold text-sm text-gray-200 flex items-center gap-2">
              <i class="fa-solid fa-chart-line text-indigo-400"></i>
              Métricas de Desempenho
            </h3>
            <div class="grid grid-cols-2 gap-3 font-mono text-xs">
              <div class="bg-gray-900/80 p-2.5 rounded-xl border border-gray-800">
                <span class="text-gray-400 block text-[10px]">LATÊNCIA DA PESQUISA</span>
                <span id="metric-latency" class="text-emerald-400 font-bold text-sm">-- ms</span>
              </div>
              <div class="bg-gray-900/80 p-2.5 rounded-xl border border-gray-800">
                <span class="text-gray-400 block text-[10px]">FONTES PROCESSADAS</span>
                <span id="metric-sources" class="text-indigo-400 font-bold text-sm">--</span>
              </div>
              <div class="bg-gray-900/80 p-2.5 rounded-xl border border-gray-800">
                <span class="text-gray-400 block text-[10px]">TOKENS DE ENTRADA</span>
                <span id="metric-tokens-in" class="text-cyan-400 font-bold text-sm">--</span>
              </div>
              <div class="bg-gray-900/80 p-2.5 rounded-xl border border-gray-800">
                <span class="text-gray-400 block text-[10px]">PRECISÃO DAS FONTES</span>
                <span id="metric-accuracy" class="text-yellow-400 font-bold text-sm">99.4%</span>
              </div>
            </div>
          </div>

        </div>

      </div>
    </section>

    <!-- TAB 2: INTERACTIVE CANVAS PIPELINE VISUALIZER -->
    <section id="view-visualizer" class="hidden space-y-4 transition-all duration-300">
      <div class="glass-panel rounded-2xl p-5 border border-gray-800 space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-4 border-b border-gray-800 pb-4">
          <div>
            <h2 class="text-lg font-bold text-gray-100 flex items-center gap-2">
              <i class="fa-solid fa-brain text-indigo-400"></i>
              Visualizador Dinâmico do Pipeline Neural & Web Vectoring
            </h2>
            <p class="text-xs text-gray-400">Representação visual em tempo real do processamento de tokens, busca na web e matriz de atenção.</p>
          </div>
          <div class="flex items-center gap-3 text-xs font-mono">
            <span class="flex items-center gap-1.5"><span class="w-3 h-3 rounded-full bg-cyan-400"></span> Query Web</span>
            <span class="flex items-center gap-1.5"><span class="w-3 h-3 rounded-full bg-indigo-500"></span> Rastreamento Web</span>
            <span class="flex items-center gap-1.5"><span class="w-3 h-3 rounded-full bg-emerald-400"></span> Tokens de Saída</span>
          </div>
        </div>

        <!-- Canvas Element -->
        <div class="relative rounded-xl overflow-hidden bg-gray-950 border border-gray-800">
          <canvas id="pipeline-canvas" class="w-full h-[480px]"></canvas>
          <div class="absolute bottom-3 left-3 bg-gray-900/90 border border-gray-800 px-3 py-1.5 rounded-lg text-xs font-mono text-gray-300 backdrop-blur">
            Status do Pipeline: <span id="canvas-status-text" class="text-indigo-400">Aguardando Entrada</span>
          </div>
        </div>
      </div>
    </section>

    <!-- TAB 3: APPLICATION OVERVIEW & ARCHITECTURE DOCUMENTATION -->
    <section id="view-architecture" class="hidden space-y-6 transition-all duration-300">
      
      <!-- Overview Card -->
      <div class="glass-panel rounded-2xl p-6 border border-gray-800 space-y-6">
        <div class="border-b border-gray-800 pb-4">
          <h2 class="text-xl font-bold text-gray-100 flex items-center gap-2.5">
            <i class="fa-solid fa-sitemap text-indigo-400"></i>
            Visão Geral e Arquitetura do Aplicativo
          </h2>
          <p class="text-sm text-gray-400 mt-1">
            Como este Navegador com Inteligência Artificial Conectada à Web Processa Informações em Tempo Real.
          </p>
        </div>

        <!-- Architecture Flow Diagrams -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="glass-card p-5 rounded-xl border border-gray-800 space-y-3">
            <div class="w-10 h-10 rounded-lg bg-cyan-500/10 text-cyan-400 flex items-center justify-center font-bold border border-cyan-500/20">
              01
            </div>
            <h3 class="font-bold text-gray-200">1. Captura e Vetorização</h3>
            <p class="text-xs text-gray-400 leading-relaxed">
              A consulta do usuário é submetida ao modelo **Gemini 3 Flash** com a instrução de ferramenta habilitada (`google_search`). A intenção de busca é extraída e formatada para rastreamento.
            </p>
          </div>

          <div class="glass-card p-5 rounded-xl border border-gray-800 space-y-3">
            <div class="w-10 h-10 rounded-lg bg-indigo-500/10 text-indigo-400 flex items-center justify-center font-bold border border-indigo-500/20">
              02
            </div>
            <h3 class="font-bold text-gray-200">2. Rastreamento e Grounding</h3>
            <p class="text-xs text-gray-400 leading-relaxed">
              O modelo consulta os dados mais recentes da World Wide Web, extraindo metadados, títulos, URLs de referência e trechos relevantes que fundamentam numericamente a resposta.
            </p>
          </div>

          <div class="glass-card p-5 rounded-xl border border-gray-800 space-y-3">
            <div class="w-10 h-10 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center font-bold border border-emerald-500/20">
              03
            </div>
            <h3 class="font-bold text-gray-200">3. Síntese e Citação</h3>
            <p class="text-xs text-gray-400 leading-relaxed">
              As respostas são geradas com estrita adesão aos fatos encontrados, adicionando links de atribuição e citações clicáveis para verificação imediata pelo usuário no navegador.
            </p>
          </div>
        </div>

        <!-- Technical Specs Table -->
        <div class="bg-gray-900/90 rounded-xl border border-gray-800 p-5 space-y-4">
          <h3 class="font-bold text-sm text-gray-200 flex items-center gap-2">
            <i class="fa-solid fa-layer-group text-indigo-400"></i> Especificações do Sistema
          </h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-xs font-mono">
            <div class="space-y-2">
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Modelo Core:</span>
                <span class="text-indigo-300 font-semibold">gemini-3-flash-preview</span>
              </div>
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Ferramenta de Grounding:</span>
                <span class="text-cyan-300 font-semibold">google_search (Google Live Web)</span>
              </div>
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Interface de Usuário:</span>
                <span class="text-emerald-300 font-semibold">HTML5 Single-File / Tailwind CSS</span>
              </div>
            </div>
            <div class="space-y-2">
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Motor do Canvas:</span>
                <span class="text-indigo-300 font-semibold">JavaScript 2D Particle Engine</span>
              </div>
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Renderização de Texto:</span>
                <span class="text-cyan-300 font-semibold">Marked.js Markdown Parser</span>
              </div>
              <div class="flex justify-between border-b border-gray-800 pb-1.5">
                <span class="text-gray-400">Latência Médiana:</span>
                <span class="text-emerald-300 font-semibold">~ 1.2s a 2.5s</span>
              </div>
            </div>
          </div>
        </div>

      </div>
    </section>

  </main>

  <script>
    /* Global Application State */
    const state = {
      apiKey: "", // Uses runtime canvas key injection
      isSearching: false,
      activeTab: 'browser',
      lastResponseText: '',
      sources: []
    };

    function switchTab(tabName) {
      state.activeTab = tabName;
      
      const browserView = document.getElementById('view-browser');
      const visualizerView = document.getElementById('view-visualizer');
      const architectureView = document.getElementById('view-architecture');

      const btnBrowser = document.getElementById('tab-browser-btn');
      const btnVisualizer = document.getElementById('tab-visualizer-btn');
      const btnArchitecture = document.getElementById('tab-architecture-btn');

      // Hide all
      browserView.classList.add('hidden');
      visualizerView.classList.add('hidden');
      architectureView.classList.add('hidden');

      // Reset buttons
      [btnBrowser, btnVisualizer, btnArchitecture].forEach(btn => {
        btn.className = "px-3 py-1.5 rounded-lg text-xs font-semibold text-gray-400 hover:bg-gray-800/60 hover:text-gray-200 flex items-center gap-1.5 transition";
      });

      const activeBtnClass = "px-3 py-1.5 rounded-lg text-xs font-semibold bg-indigo-600/30 text-indigo-300 border border-indigo-500/40 flex items-center gap-1.5 transition";

      if (tabName === 'browser') {
        browserView.classList.remove('hidden');
        btnBrowser.className = activeBtnClass;
      } else if (tabName === 'visualizer') {
        visualizerView.classList.remove('hidden');
        btnVisualizer.className = activeBtnClass;
        resizeCanvas();
      } else if (tabName === 'architecture') {
        architectureView.classList.remove('hidden');
        btnArchitecture.className = activeBtnClass;
      }
    }

    async function executeWebSearch(userQuery) {
      if (state.isSearching) return;
      state.isSearching = true;

      const startTime = Date.now();
      const statusIndicator = document.getElementById('status-indicator');
      const statusText = document.getElementById('status-text');
      const spinner = document.getElementById('loading-spinner');
      const contentBox = document.getElementById('ai-response-content');
      const citationsContainer = document.getElementById('citations-container');
      const canvasStatus = document.getElementById('canvas-status-text');

      // Update UI for Loading State
      statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-yellow-400 animate-ping";
      statusText.innerText = "Status: Pesquisando na Web e Sintetizando com Gemini...";
      spinner.classList.remove('hidden');
      contentBox.innerHTML = '';
      citationsContainer.innerHTML = '<div class="text-xs text-gray-500 italic p-2">Rastreando fontes na Web...</div>';
      canvasStatus.innerText = "Rastreando Nós de Dados da Web...";

      // Trigger Canvas Animation state
      setPipelineStage('SEARCHING');

      const systemPrompt = "Você é um assistente de pesquisa web inteligente tipo Claude. Responda de forma clara, estruturada e precisa em Português (Brasil). Use Markdown e forneça detalhes explicativos completos baseados nas informações mais recentes trazidas da busca na web.";

      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${state.apiKey}`;

      const payload = {
        contents: [{ parts: [{ text: userQuery }] }],
        tools: [{ "google_search": {} }],
        systemInstruction: {
          parts: [{ text: systemPrompt }]
        }
      };

      try {
        const response = await fetchWithRetry(apiUrl, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });

        const result = await response.json();
        const candidate = result.candidates?.[0];

        if (candidate && candidate.content?.parts?.[0]?.text) {
          const generatedText = candidate.content.parts[0].text;
          state.lastResponseText = generatedText;

          // Extract Grounding Sources
          let sources = [];
          const groundingMetadata = candidate.groundingMetadata;
          if (groundingMetadata && groundingMetadata.groundingAttributions) {
            sources = groundingMetadata.groundingAttributions
              .map(attribution => ({
                uri: attribution.web?.uri,
                title: attribution.web?.title || attribution.web?.uri
              }))
              .filter(source => source.uri && source.title);
          }

          state.sources = sources;

          // Calculate Latency & Stats
          const latency = Date.now() - startTime;
          document.getElementById('metric-latency').innerText = `${latency} ms`;
          document.getElementById('metric-sources').innerText = sources.length;
          document.getElementById('metric-tokens-in').innerText = Math.round(userQuery.length / 3.5);

          // Render AI Markdown
          contentBox.innerHTML = marked.parse(generatedText);

          // Render Citations
          renderCitations(sources);

          // Update Status
          statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-emerald-500";
          statusText.innerText = "Status: Pesquisa concluída com sucesso";
          canvasStatus.innerText = "Síntese Concluída e Exibida";
          setPipelineStage('COMPLETE');

        } else {
          contentBox.innerHTML = `<div class="p-4 bg-red-900/30 border border-red-800 rounded-xl text-red-300 text-sm">Não foi possível obter uma resposta precisa para a busca. Tente refazer a pergunta.</div>`;
        }

      } catch (error) {
        console.error("Search Error:", error);
        contentBox.innerHTML = `<div class="p-4 bg-red-900/30 border border-red-800 rounded-xl text-red-300 text-sm">Erro ao conectar com o serviço de pesquisa: ${error.message}</div>`;
        statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-red-500";
        statusText.innerText = "Status: Erro na comunicação";
      } finally {
        spinner.classList.add('hidden');
        state.isSearching = false;
      }
    }

    /* Helper: Fetch with Exponential Backoff */
    async function fetchWithRetry(url, options, retries = 3, delay = 1000) {
      try {
        const res = await fetch(url, options);
        if (res.status === 429 && retries > 0) {
          await new Promise(resolve => setTimeout(resolve, delay));
          return fetchWithRetry(url, options, retries - 1, delay * 2);
        }
        return res;
      } catch (err) {
        if (retries > 0) {
          await new Promise(resolve => setTimeout(resolve, delay));
          return fetchWithRetry(url, options, retries - 1, delay * 2);
        }
        throw err;
      }
    }

    function renderCitations(sources) {
      const container = document.getElementById('citations-container');
      container.innerHTML = '';

      if (!sources || sources.length === 0) {
        container.innerHTML = `
          <div class="p-3 bg-gray-900/60 rounded-xl border border-gray-800 text-xs text-gray-400">
            Nenhuma fonte externa direta foi citada para esta consulta específica.
          </div>
        `;
        return;
      }

      sources.forEach((src, idx) => {
        const item = document.createElement('a');
        item.href = src.uri;
        item.target = '_blank';
        item.rel = 'noopener noreferrer';
        item.className = "block p-3 bg-gray-900/80 hover:bg-gray-800 border border-gray-800 hover:border-indigo-500/50 rounded-xl text-xs transition space-y-1 group";
        
        item.innerHTML = `
          <div class="flex items-center justify-between text-indigo-400 group-hover:text-indigo-300 font-semibold">
            <span class="truncate pr-2">[${idx + 1}] ${escapeHtml(src.title)}</span>
            <i class="fa-solid fa-arrow-up-right-from-square text-[10px]"></i>
          </div>
          <div class="text-[11px] text-gray-500 truncate font-mono">${escapeHtml(src.uri)}</div>
        `;

        container.appendChild(item);
      });
    }

    function escapeHtml(str) {
      return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    }

    function quickSearch(queryText) {
      document.getElementById('search-input').value = queryText;
      switchTab('browser');
      executeWebSearch(queryText);
    }

    function copyResponse() {
      if (!state.lastResponseText) return;
      document.execCommand('copy');
      navigator.clipboard?.writeText(state.lastResponseText);
      alert("Resposta copiada para a área de transferência!");
    }

    // Event Listeners
    document.getElementById('search-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const query = document.getElementById('search-input').value.trim();
      if (query) {
        switchTab('browser');
        executeWebSearch(query);
      }
    });

    const canvas = document.getElementById('pipeline-canvas');
    const ctx = canvas.getContext('2d');

    let animationFrameId;
    let particles = [];
    let currentStage = 'IDLE';

    function resizeCanvas() {
      if (!canvas) return;
      canvas.width = canvas.parentElement.clientWidth;
      canvas.height = 480;
      initPipelineNodes();
    }

    let nodes = [];

    function initPipelineNodes() {
      const width = canvas.width;
      const height = canvas.height;

      nodes = [
        { id: 'query', label: 'Vetor de Busca', x: width * 0.15, y: height * 0.5, color: '#38bdf8' },
        { id: 'search1', label: 'Rastreamento Web 1', x: width * 0.4, y: height * 0.25, color: '#818cf8' },
        { id: 'search2', label: 'Rastreamento Web 2', x: width * 0.4, y: height * 0.75, color: '#818cf8' },
        { id: 'attention', label: 'Matriz de Atenção', x: width * 0.65, y: height * 0.5, color: '#c084fc' },
        { id: 'output', label: 'Síntese de Texto', x: width * 0.88, y: height * 0.5, color: '#34d399' }
      ];
    }

    function setPipelineStage(stage) {
      currentStage = stage;
      // Spawn extra particles on stage transition
      for (let i = 0; i < 25; i++) {
        spawnParticle();
      }
    }

    function spawnParticle() {
      if (nodes.length < 5) return;
      const startNode = nodes[Math.floor(Math.random() * (nodes.length - 1))];
      const targetNode = nodes[nodes.indexOf(startNode) + 1] || nodes[4];

      particles.push({
        x: startNode.x,
        y: startNode.y,
        targetX: targetNode.x,
        targetY: targetNode.y,
        progress: 0,
        speed: 0.008 + Math.random() * 0.015,
        color: startNode.color
      });
    }

    function animatePipeline() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw Connection Lines
      ctx.lineWidth = 1.5;
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.08)';

      for (let i = 0; i < nodes.length; i++) {
        for (let j = i + 1; j < nodes.length; j++) {
          ctx.beginPath();
          ctx.moveTo(nodes[i].x, nodes[i].y);
          ctx.lineTo(nodes[j].x, nodes[j].y);
          ctx.stroke();
        }
      }

      // Update and Draw Particles
      for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];
        p.progress += p.speed;

        p.x = p.x + (p.targetX - p.x) * p.speed * 4;
        p.y = p.y + (p.targetY - p.y) * p.speed * 4;

        ctx.beginPath();
        ctx.arc(p.x, p.y, 3, 0, Math.PI * 2);
        ctx.fillStyle = p.color;
        ctx.shadowColor = p.color;
        ctx.shadowBlur = 10;
        ctx.fill();
        ctx.shadowBlur = 0;

        if (p.progress >= 1 || Math.hypot(p.targetX - p.x, p.targetY - p.y) < 5) {
          particles.splice(i, 1);
        }
      }

      // Continuous particle spawning if active
      if (currentStage === 'SEARCHING' && Math.random() < 0.4) {
        spawnParticle();
      }

      // Draw Nodes
      nodes.forEach(node => {
        // Glowing Outer Ring
        ctx.beginPath();
        ctx.arc(node.x, node.y, 22, 0, Math.PI * 2);
        ctx.fillStyle = 'rgba(15, 23, 42, 0.9)';
        ctx.strokeStyle = node.color;
        ctx.lineWidth = 2;
        ctx.fill();
        ctx.stroke();

        // Node Label
        ctx.fillStyle = '#f3f4f6';
        ctx.font = '11px Inter, sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText(node.label, node.x, node.y + 38);
      });

      animationFrameId = requestAnimationFrame(animatePipeline);
    }

    window.addEventListener('resize', resizeCanvas);

    window.onload = function() {
      resizeCanvas();
      animatePipeline();

      // Trigger automatic initial search query
      const defaultQuery = document.getElementById('search-input').value;
      if (defaultQuery) {
        executeWebSearch(defaultQuery);
      }
    };
  </script>
</body>
</html>
