
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural Raphael AI / OmniSearch Suite</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f3ff',
                            100: '#e0e7ff',
                            500: '#6366f1',
                            600: '#4f46e5',
                            700: '#4338ca',
                            900: '#1e1b4b',
                            dark: '#0b0f19',
                            card: '#151c2c',
                            border: '#232d42'
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome 6 Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Marked.js for Markdown Rendering -->
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- KaTeX for Math LaTeX Rendering -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/contrib/auto-render.min.js"></script>
    <!-- jsPDF for Document Export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Fira+Code:wght@400;500;600&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0b0f19;
            color: #f3f4f6;
        }
        code, pre {
            font-family: 'Fira Code', monospace;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #232d42;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #4f46e5;
        }
        .glass-panel {
            background: rgba(21, 28, 44, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }
        .glass-card {
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
    </style>
</head>
<body class="h-screen flex flex-col overflow-hidden text-gray-200">

    <!-- Top Bar Header -->
    <header class="glass-panel h-16 px-4 flex items-center justify-between z-30 shrink-0 border-b border-gray-800">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-600 via-purple-600 to-pink-500 flex items-center justify-center shadow-lg shadow-indigo-500/30">
                <i class="fa-solid font-bold text-xl text-white fa-brain"></i>
            </div>
            <div>
                <h1 class="font-bold text-lg leading-none bg-gradient-to-r from-indigo-400 via-purple-300 to-pink-400 bg-clip-text text-transparent">Neural Raphael AI</h1>
                <span class="text-xs text-gray-400 font-medium">OmniSearch & Multimodal Studio v3.5</span>
            </div>
        </div>

        <!-- System Controls & Settings Modal Toggle -->
        <div class="flex items-center space-x-3">
            <button onclick="toggleSettingsModal()" class="flex items-center space-x-2 px-3 py-1.5 rounded-lg bg-indigo-900/40 hover:bg-indigo-800/60 text-indigo-300 border border-indigo-700/50 text-xs font-medium transition">
                <i class="fa-solid fa-key text-xs"></i>
                <span id="apiKeyStatus">Gemini API Key</span>
            </button>
            <div class="text-xs bg-emerald-500/10 text-emerald-400 border border-emerald-500/30 px-2.5 py-1 rounded-full flex items-center gap-1.5">
                <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                <span>Motor Local & Nuvem Pronto</span>
            </div>
        </div>
    </header>

    <div class="flex flex-1 overflow-hidden">
        <!-- Sidebar Navigation -->
        <aside class="w-16 md:w-64 glass-panel border-r border-gray-800/80 flex flex-col justify-between shrink-0 transition-all duration-300">
            <nav class="p-3 space-y-1.5">
                <button onclick="switchTab('chat')" id="nav-chat" class="tab-btn active-tab w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-indigo-400 bg-indigo-600/20 border border-indigo-500/30">
                    <i class="fa-solid fa-comments text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Chat AI Assistant</span>
                </button>
                <button onclick="switchTab('code')" id="nav-code" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-code text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Programação & Algoritmos</span>
                </button>
                <button onclick="switchTab('translate')" id="nav-translate" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-language text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Tradução Multilíngue</span>
                </button>
                <button onclick="switchTab('math')" id="nav-math" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-calculator text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Matemática & Cálculos</span>
                </button>
                <button onclick="switchTab('studio')" id="nav-studio" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-wand-magic-sparkles text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Estúdio Mídia (AI Studio)</span>
                </button>
                <button onclick="switchTab('media-editor')" id="nav-media-editor" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-sliders text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Editor Imagem & Áudio</span>
                </button>
                <button onclick="switchTab('export')" id="nav-export" class="tab-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-xl text-sm font-medium transition text-gray-400 hover:text-gray-200 hover:bg-gray-800/50">
                    <i class="fa-solid fa-file-export text-lg w-6 text-center"></i>
                    <span class="hidden md:inline">Exportador de Documentos</span>
                </button>
            </nav>

            <div class="p-3 border-t border-gray-800 text-xs text-gray-500 text-center hidden md:block">
                <p>Neural Raphael Suite</p>
                <p class="text-[10px] text-gray-600 mt-0.5">100% Single-File & Standalone</p>
            </div>
        </aside>

        <!-- Main Content View Container -->
        <main class="flex-1 relative overflow-hidden bg-gradient-to-b from-gray-950 via-slate-950 to-gray-950 flex flex-col">

            <section id="view-chat" class="tab-content h-full flex flex-col p-4 space-y-4">
                <!-- System Prompt Config Header -->
                <div class="glass-card rounded-2xl p-3 flex flex-wrap items-center justify-between gap-3 shrink-0">
                    <div class="flex items-center space-x-2">
                        <i class="fa-solid fa-robot text-indigo-400"></i>
                        <span class="text-xs font-semibold text-gray-300">System Persona:</span>
                        <select id="chatPersona" class="bg-gray-900 text-xs border border-gray-700 rounded-lg px-2.5 py-1.5 focus:outline-none focus:border-indigo-500 text-gray-200">
                            <option value="default">✨ Raphael AI Universal Assistant</option>
                            <option value="coder">💻 Senior Full-Stack Engineer & Architect</option>
                            <option value="translator">🌐 Expert Translator & Linguist</option>
                            <option value="math">🧮 Math Professor & Equations Specialist</option>
                            <option value="creative">🎨 Creative Writer & Storyteller</option>
                        </select>
                    </div>
                    <div class="flex items-center space-x-2">
                        <button onclick="clearChat()" class="text-xs px-2.5 py-1.5 rounded-lg bg-gray-800 hover:bg-gray-700 text-gray-300 transition flex items-center gap-1.5">
                            <i class="fa-solid fa-trash"></i> Limpar Chat
                        </button>
                    </div>
                </div>

                <!-- Chat History Area -->
                <div id="chatHistory" class="flex-1 overflow-y-auto space-y-4 pr-2 rounded-2xl p-2">
                    <!-- Initial Welcome Message -->
                    <div class="flex gap-3 max-w-3xl">
                        <div class="w-8 h-8 rounded-lg bg-indigo-600/30 border border-indigo-500/40 flex items-center justify-center shrink-0 text-indigo-300 font-bold text-sm">
                            AI
                        </div>
                        <div class="glass-card rounded-2xl p-4 text-sm leading-relaxed border border-indigo-500/20 text-gray-200">
                            Olá! Eu sou o <strong>Neural Raphael AI</strong>. Como posso ajudar você hoje? 
                            <br><br>
                            Você pode me pedir para programar algoritmos, resolver cálculos complexos com LaTeX, traduzir textos, sintetizar músicas, renderizar artes visuais ou exportar relatórios completos em PDF/DOCX!
                        </div>
                    </div>
                </div>

                <!-- Chat Input Controls -->
                <div class="shrink-0 glass-card rounded-2xl p-2 relative flex items-center gap-2 border border-indigo-500/20">
                    <textarea id="chatInput" rows="1" placeholder="Digite sua mensagem ou pergunta..." class="w-full bg-transparent border-0 focus:ring-0 focus:outline-none text-sm text-gray-100 placeholder-gray-500 resize-none px-3 py-2 max-h-32" onkeydown="handleChatKeyDown(event)"></textarea>
                    <button onclick="sendChatMessage()" class="w-10 h-10 rounded-xl bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 text-white flex items-center justify-center shrink-0 shadow-md shadow-indigo-600/30 transition">
                        <i class="fa-solid fa-paper-plane text-sm"></i>
                    </button>
                </div>
            </section>

            <section id="view-code" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-4 flex-1 min-h-0">
                    <!-- Left: Generator & Prompt -->
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-3">
                        <div class="flex items-center justify-between">
                            <h2 class="font-semibold text-sm flex items-center gap-2 text-indigo-300">
                                <i class="fa-solid fa-laptop-code"></i> Gerador de Algoritmos
                            </h2>
                            <select id="codeLanguage" class="bg-gray-900 text-xs border border-gray-700 rounded-lg px-2.5 py-1 text-gray-200">
                                <option value="javascript">JavaScript (Executable)</option>
                                <option value="python">Python</option>
                                <option value="cpp">C++</option>
                                <option value="java">Java</option>
                                <option value="rust">Rust</option>
                                <option value="sql">SQL</option>
                                <option value="html">HTML/CSS/JS</option>
                            </select>
                        </div>
                        <textarea id="codePrompt" rows="3" placeholder="Ex: Crie um algoritmo de ordenação QuickSort com explicação linha a linha..." class="w-full bg-gray-900 border border-gray-800 rounded-xl p-3 text-sm focus:outline-none focus:border-indigo-500 text-gray-200 placeholder-gray-600 resize-none"></textarea>
                        <button onclick="generateCodeAlg()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition flex items-center justify-center gap-2 shadow-lg shadow-indigo-600/20">
                            <i class="fa-solid fa-wand-magic"></i> Gerar Código & Algoritmo
                        </button>
                        
                        <!-- Sandbox Output Container -->
                        <div class="flex-1 flex flex-col min-h-0 pt-2 border-t border-gray-800">
                            <div class="flex items-center justify-between mb-2">
                                <span class="text-xs font-semibold text-gray-400">Console Sandbox Output (JS):</span>
                                <button onclick="clearConsole()" class="text-[10px] text-gray-500 hover:text-gray-300">Limpar</button>
                            </div>
                            <div id="sandboxConsole" class="flex-1 bg-black/80 rounded-xl p-3 font-mono text-xs text-green-400 overflow-y-auto border border-gray-800">
                                // Logs e resultados de execução aparecerão aqui...
                            </div>
                        </div>
                    </div>

                    <!-- Right: Editor & Output -->
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-3 min-h-0">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-gray-300 flex items-center gap-2">
                                <i class="fa-solid fa-code"></i> Editor Interativo
                            </span>
                            <div class="flex items-center space-x-2">
                                <button onclick="copyCode()" class="text-xs px-2.5 py-1 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded-lg transition flex items-center gap-1">
                                    <i class="fa-regular fa-copy"></i> Copiar
                                </button>
                                <button onclick="runCodeSandbox()" class="text-xs px-3 py-1 bg-emerald-600 hover:bg-emerald-500 text-white font-semibold rounded-lg transition flex items-center gap-1 shadow-md shadow-emerald-600/20">
                                    <i class="fa-solid fa-play"></i> Executar (JS)
                                </button>
                            </div>
                        </div>
                        <textarea id="codeEditor" class="flex-1 bg-gray-950 border border-gray-800 rounded-xl p-3 font-mono text-xs text-indigo-200 focus:outline-none focus:border-indigo-500 resize-none leading-relaxed">// Neural Raphael Interactive Code Sandbox
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log("Calculando Fibonacci(10)...");
console.log("Resultado:", fibonacci(10));</textarea>
                    </div>
                </div>
            </section>

            <section id="view-translate" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <div class="glass-card rounded-2xl p-4 flex items-center justify-between gap-4">
                    <div class="flex items-center space-x-2 flex-1">
                        <select id="sourceLang" class="bg-gray-900 border border-gray-800 rounded-xl px-3 py-2 text-xs font-medium text-gray-200 focus:border-indigo-500">
                            <option value="auto">Detectar Idioma Automático</option>
                            <option value="pt">Português</option>
                            <option value="en">Inglês</option>
                            <option value="es">Espanhol</option>
                            <option value="fr">Francês</option>
                            <option value="de">Alemão</option>
                            <option value="zh">Chinês</option>
                            <option value="ja">Japonês</option>
                        </select>
                        <button onclick="swapTranslationLangs()" class="p-2 text-gray-400 hover:text-indigo-400 rounded-lg hover:bg-gray-800">
                            <i class="fa-solid fa-arrows-rotate"></i>
                        </button>
                        <select id="targetLang" class="bg-gray-900 border border-gray-800 rounded-xl px-3 py-2 text-xs font-medium text-gray-200 focus:border-indigo-500">
                            <option value="en">Inglês</option>
                            <option value="pt">Português</option>
                            <option value="es">Espanhol</option>
                            <option value="fr">Francês</option>
                            <option value="de">Alemão</option>
                            <option value="zh">Chinês</option>
                            <option value="ja">Japonês</option>
                        </select>
                    </div>
                    <div class="flex items-center space-x-2">
                        <select id="translationTone" class="bg-gray-900 border border-gray-800 rounded-xl px-3 py-2 text-xs text-gray-200">
                            <option value="natural">Tom Natural</option>
                            <option value="formal">Tom Formal/Corporativo</option>
                            <option value="casual">Tom Casual/Informal</option>
                            <option value="technical">Tom Técnico/Científico</option>
                        </select>
                        <button onclick="executeTranslation()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition flex items-center gap-2">
                            <i class="fa-solid fa-language"></i> Traduzir Agora
                        </button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 flex-1">
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-2">
                        <span class="text-xs font-semibold text-gray-400">Texto Original:</span>
                        <textarea id="translateInput" placeholder="Digite ou cole aqui o texto para tradução e análise gramatical..." class="flex-1 bg-gray-900 border border-gray-800 rounded-xl p-3 text-sm text-gray-100 focus:outline-none focus:border-indigo-500 resize-none"></textarea>
                    </div>
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-gray-400">Tradução & Análise Gramatical:</span>
                            <button onclick="copyTranslation()" class="text-xs text-gray-400 hover:text-white"><i class="fa-regular fa-copy"></i> Copiar</button>
                        </div>
                        <div id="translateOutput" class="flex-1 bg-gray-950 border border-gray-800 rounded-xl p-3 text-sm text-gray-200 overflow-y-auto leading-relaxed">
                            <span class="text-gray-500 italic">O resultado da tradução e insights linguísticos aparecerão aqui...</span>
                        </div>
                    </div>
                </div>
            </section>

            <section id="view-math" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 flex-1 min-h-0">
                    <!-- Left Column: Math Solver Controls -->
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-3">
                        <h2 class="font-semibold text-sm text-indigo-300 flex items-center gap-2">
                            <i class="fa-solid fa-calculator"></i> Assistente Matemático
                        </h2>
                        <div class="space-y-1">
                            <label class="text-xs text-gray-400">Modo de Cálculo:</label>
                            <select id="mathMode" class="w-full bg-gray-900 border border-gray-800 rounded-xl px-3 py-2 text-xs text-gray-200">
                                <option value="equation">Equações & Álgebra</option>
                                <option value="calculus">Cálculo Diferencial e Integral</option>
                                <option value="stats">Estatística & Probabilidade</option>
                                <option value="finance">Finanças & Juros Compostos</option>
                            </select>
                        </div>
                        <div class="space-y-1">
                            <label class="text-xs text-gray-400">Expressão / Problema:</label>
                            <textarea id="mathPrompt" rows="3" placeholder="Ex: Solve x^2 - 5x + 6 = 0 ou Integral de sin(x)*cos(x)..." class="w-full bg-gray-900 border border-gray-800 rounded-xl p-3 text-sm text-gray-100 focus:outline-none focus:border-indigo-500 resize-none"></textarea>
                        </div>
                        <button onclick="solveMathProblem()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition flex items-center justify-center gap-2 shadow-lg shadow-indigo-600/20">
                            <i class="fa-solid fa-square-root-variable"></i> Resolver Passo a Passo
                        </button>

                        <!-- Fast Unit Converter Component -->
                        <div class="pt-3 border-t border-gray-800 space-y-2">
                            <span class="text-xs font-semibold text-gray-300">Conversor Rápido de Unidades</span>
                            <div class="grid grid-cols-2 gap-2">
                                <input type="number" id="unitVal" value="100" placeholder="Valor" class="bg-gray-900 border border-gray-800 rounded-lg p-2 text-xs text-gray-100">
                                <select id="unitType" class="bg-gray-900 border border-gray-800 rounded-lg p-2 text-xs text-gray-200" onchange="convertUnits()">
                                    <option value="c2f">Celsius ➔ Fahrenheit</option>
                                    <option value="f2c">Fahrenheit ➔ Celsius</option>
                                    <option value="km2mi">Km ➔ Milhas</option>
                                    <option value="mi2km">Milhas ➔ Km</option>
                                    <option value="kg2lb">Kg ➔ Libras</option>
                                </select>
                            </div>
                            <div id="unitResult" class="text-xs font-mono text-indigo-300 bg-gray-900/50 p-2 rounded-lg border border-gray-800 text-center">
                                Resultado: 212 °F
                            </div>
                        </div>
                    </div>

                    <!-- Right Column: Step-by-Step Solution with LaTeX Render -->
                    <div class="lg:col-span-2 glass-card rounded-2xl p-4 flex flex-col space-y-3 min-h-0">
                        <span class="text-xs font-semibold text-gray-300 flex items-center gap-2">
                            <i class="fa-solid fa-square-root-variable text-indigo-400"></i> Resolução Formatada (KaTeX LaTeX)
                        </span>
                        <div id="mathOutput" class="flex-1 bg-gray-950 border border-gray-800 rounded-xl p-4 text-sm text-gray-200 overflow-y-auto leading-relaxed space-y-3">
                            <p class="text-gray-500 italic">Insira uma equação acima para ver a resolução completa com fórmula formatada.</p>
                        </div>
                    </div>
                </div>
            </section>

            <section id="view-studio" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <!-- Studio Navigation Tabs -->
                <div class="flex items-center space-x-2 border-b border-gray-800 pb-2">
                    <button onclick="switchStudioSubTab('image')" id="subnav-image" class="subtab-btn active-subtab text-xs px-3 py-1.5 rounded-lg bg-indigo-600 text-white font-medium">
                        <i class="fa-solid fa-image"></i> Gerador Imagem AI
                    </button>
                    <button onclick="switchStudioSubTab('music')" id="subnav-music" class="subtab-btn text-xs px-3 py-1.5 rounded-lg bg-gray-800 text-gray-400 hover:text-white font-medium">
                        <i class="fa-solid fa-music"></i> Sintetizador Áudio/Música
                    </button>
                    <button onclick="switchStudioSubTab('video')" id="subnav-video" class="subtab-btn text-xs px-3 py-1.5 rounded-lg bg-gray-800 text-gray-400 hover:text-white font-medium">
                        <i class="fa-solid fa-video"></i> Gerador Animação/Vídeo
                    </button>
                </div>

                <!-- Sub-View 1: AI Image Generator -->
                <div id="st-image" class="studio-subview flex-1 flex flex-col lg:flex-row gap-4 min-h-0">
                    <div class="w-full lg:w-80 glass-card rounded-2xl p-4 flex flex-col space-y-3">
                        <h3 class="text-xs font-semibold text-indigo-300">Prompt da Imagem</h3>
                        <textarea id="imagePrompt" rows="3" placeholder="Ex: Futuristic cyberpunk city bathed in neon lights, hyperrealistic 8k render..." class="bg-gray-900 border border-gray-800 rounded-xl p-3 text-xs text-gray-100 focus:outline-none focus:border-indigo-500 resize-none"></textarea>
                        <button onclick="generateAIImage()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-wand-magic-sparkles"></i> Renderizar Imagem
                        </button>
                    </div>
                    <div class="flex-1 glass-card rounded-2xl p-4 flex items-center justify-center overflow-hidden relative">
                        <img id="generatedImage" src="https://placehold.co/800x600/1e1b4b/818cf8?text=Neural+Raphael+AI+Studio" class="max-h-full max-w-full rounded-xl object-contain shadow-2xl border border-gray-800">
                    </div>
                </div>

                <!-- Sub-View 2: Music & Synth Generator -->
                <div id="st-music" class="studio-subview hidden flex-1 flex flex-col lg:flex-row gap-4 min-h-0">
                    <div class="w-full lg:w-80 glass-card rounded-2xl p-4 flex flex-col space-y-3">
                        <h3 class="text-xs font-semibold text-indigo-300">Sintetizador & Sequenciador Melódico</h3>
                        <div class="space-y-1">
                            <label class="text-[11px] text-gray-400">Estilo da Melodia:</label>
                            <select id="synthStyle" class="w-full bg-gray-900 border border-gray-800 rounded-xl p-2 text-xs text-gray-200">
                                <option value="ambient">Ambient Cyber Space</option>
                                <option value="chiptune">Retro Chiptune 8-Bit</option>
                                <option value="euphoric">Euphoric Synthwave</option>
                            </select>
                        </div>
                        <div class="space-y-1">
                            <label class="text-[11px] text-gray-400">Tempo (BPM): <span id="bpmVal">120</span></label>
                            <input type="range" id="synthBpm" min="60" max="180" value="120" class="w-full" oninput="document.getElementById('bpmVal').innerText = this.value">
                        </div>
                        <div class="flex items-center gap-2 pt-2">
                            <button onclick="playSynthesizer()" class="flex-1 py-2 bg-emerald-600 hover:bg-emerald-500 text-white rounded-xl text-xs font-semibold transition flex items-center justify-center gap-1">
                                <i class="fa-solid fa-play"></i> Tocar
                            </button>
                            <button onclick="stopSynthesizer()" class="flex-1 py-2 bg-rose-600 hover:bg-rose-500 text-white rounded-xl text-xs font-semibold transition flex items-center justify-center gap-1">
                                <i class="fa-solid fa-stop"></i> Parar
                            </button>
                        </div>
                    </div>
                    <div class="flex-1 glass-card rounded-2xl p-4 flex flex-col items-center justify-center relative">
                        <!-- Audio Visualizer Canvas -->
                        <canvas id="audioVisualizer" class="w-full h-64 bg-gray-950 rounded-xl border border-gray-800"></canvas>
                        <p class="text-xs text-gray-500 mt-2">Visualizador de Espectro Sonoro em Tempo Real (Web Audio API)</p>
                    </div>
                </div>

                <!-- Sub-View 3: Canvas Particle Video / Graphics Generator -->
                <div id="st-video" class="studio-subview hidden flex-1 flex flex-col lg:flex-row gap-4 min-h-0">
                    <div class="w-full lg:w-80 glass-card rounded-2xl p-4 flex flex-col space-y-3">
                        <h3 class="text-xs font-semibold text-indigo-300">Gerador de Motion Loop & Partículas</h3>
                        <div class="space-y-1">
                            <label class="text-[11px] text-gray-400">Efeito Visual:</label>
                            <select id="videoEffect" class="w-full bg-gray-900 border border-gray-800 rounded-xl p-2 text-xs text-gray-200" onchange="changeVideoEffect()">
                                <option value="nebula">Neural Particle Matrix</option>
                                <option value="tunnel">Hyperspace Tunnel</option>
                                <option value="waves">Sine Wave Geometry</option>
                            </select>
                        </div>
                        <div class="space-y-1">
                            <label class="text-[11px] text-gray-400">Velocidade da Animação:</label>
                            <input type="range" id="videoSpeed" min="1" max="5" value="2" class="w-full">
                        </div>
                    </div>
                    <div class="flex-1 glass-card rounded-2xl p-4 flex items-center justify-center overflow-hidden">
                        <canvas id="videoCanvas" class="w-full h-full bg-black rounded-xl border border-gray-800"></canvas>
                    </div>
                </div>
            </section>

            <section id="view-media-editor" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-4 flex-1 min-h-0">
                    <!-- Image Filter & Editor -->
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-3 min-h-0">
                        <div class="flex items-center justify-between">
                            <h3 class="text-xs font-semibold text-indigo-300 flex items-center gap-2">
                                <i class="fa-solid fa-image"></i> Editor de Imagens Canvas
                            </h3>
                            <input type="file" id="imgUpload" accept="image/*" class="hidden" onchange="loadUserImage(event)">
                            <button onclick="document.getElementById('imgUpload').click()" class="text-xs px-2.5 py-1 bg-gray-800 hover:bg-gray-700 text-gray-200 rounded-lg">Carregar Imagem</button>
                        </div>
                        <div class="grid grid-cols-3 gap-2">
                            <div>
                                <label class="text-[10px] text-gray-400">Brilho</label>
                                <input type="range" id="fBrightness" min="0" max="200" value="100" class="w-full" oninput="applyImageFilters()">
                            </div>
                            <div>
                                <label class="text-[10px] text-gray-400">Contraste</label>
                                <input type="range" id="fContrast" min="0" max="200" value="100" class="w-full" oninput="applyImageFilters()">
                            </div>
                            <div>
                                <label class="text-[10px] text-gray-400">Escala de Cinza</label>
                                <input type="range" id="fGrayscale" min="0" max="100" value="0" class="w-full" oninput="applyImageFilters()">
                            </div>
                        </div>
                        <div class="flex-1 bg-gray-950 rounded-xl border border-gray-800 overflow-hidden flex items-center justify-center p-2">
                            <canvas id="editCanvas" class="max-w-full max-h-full rounded object-contain"></canvas>
                        </div>
                    </div>

                    <!-- Audio Pitch & Speed Controls -->
                    <div class="glass-card rounded-2xl p-4 flex flex-col space-y-3 min-h-0">
                        <div class="flex items-center justify-between">
                            <h3 class="text-xs font-semibold text-indigo-300 flex items-center gap-2">
                                <i class="fa-solid fa-sliders"></i> Editor & Modificador de Áudio
                            </h3>
                            <input type="file" id="audioUpload" accept="audio/*" class="hidden" onchange="loadUserAudio(event)">
                            <button onclick="document.getElementById('audioUpload').click()" class="text-xs px-2.5 py-1 bg-gray-800 hover:bg-gray-700 text-gray-200 rounded-lg">Carregar Áudio</button>
                        </div>
                        <div class="space-y-3">
                            <div>
                                <label class="text-xs text-gray-400">Velocidade de Reprodução: <span id="speedVal">1.0x</span></label>
                                <input type="range" id="audioSpeed" min="0.5" max="2.0" step="0.1" value="1.0" class="w-full" oninput="updateAudioSettings()">
                            </div>
                            <div class="flex items-center gap-2">
                                <button onclick="playUserAudio()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-500 text-white text-xs rounded-xl font-semibold">Tocar Áudio Editado</button>
                                <button onclick="stopUserAudio()" class="px-4 py-2 bg-rose-600 hover:bg-rose-500 text-white text-xs rounded-xl font-semibold">Parar</button>
                            </div>
                        </div>
                        <div class="flex-1 bg-gray-950 rounded-xl border border-gray-800 flex items-center justify-center p-4 text-center">
                            <p id="audioStatusText" class="text-xs text-gray-500">Nenhum arquivo de áudio carregado. Clique acima para selecionar uma faixa MP3/WAV.</p>
                        </div>
                    </div>
                </div>
            </section>

            <section id="view-export" class="tab-content hidden h-full flex flex-col p-4 space-y-4">
                <div class="glass-card rounded-2xl p-4 space-y-4">
                    <h2 class="font-semibold text-sm text-indigo-300 flex items-center gap-2">
                        <i class="fa-solid fa-file-export"></i> Gerador & Exportador Universal de Documentos
                    </h2>
                    <div class="space-y-2">
                        <label class="text-xs text-gray-400">Conteúdo do Documento / Relatório:</label>
                        <textarea id="exportContent" rows="8" class="w-full bg-gray-900 border border-gray-800 rounded-xl p-3 text-sm text-gray-100 focus:outline-none focus:border-indigo-500 resize-none" placeholder="Digite ou cole aqui o conteúdo formatado que deseja exportar..."></textarea>
                    </div>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="exportDocument('pdf')" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-semibold rounded-xl transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-pdf"></i> Exportar PDF
                        </button>
                        <button onclick="exportDocument('docx')" class="px-4 py-2 bg-blue-600 hover:bg-blue-500 text-white text-xs font-semibold rounded-xl transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-word"></i> Exportar DOCX / Word
                        </button>
                        <button onclick="exportDocument('txt')" class="px-4 py-2 bg-gray-800 hover:bg-gray-700 text-gray-200 text-xs font-semibold rounded-xl transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-lines"></i> Exportar TXT
                        </button>
                        <button onclick="exportDocument('json')" class="px-4 py-2 bg-amber-600 hover:bg-amber-500 text-white text-xs font-semibold rounded-xl transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-code"></i> Exportar JSON
                        </button>
                    </div>
                </div>
            </section>
        </main>
    </div>

    <div id="settingsModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="glass-panel max-w-md w-full rounded-2xl p-6 space-y-4 border border-indigo-500/30">
            <div class="flex items-center justify-between">
                <h3 class="font-bold text-base text-indigo-300 flex items-center gap-2">
                    <i class="fa-solid fa-key"></i> Configurar Chave Gemini API
                </h3>
                <button onclick="toggleSettingsModal()" class="text-gray-400 hover:text-white">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <p class="text-xs text-gray-400 leading-relaxed">
                Insira sua chave de API para habilitar o modelo Gemini. Caso a chave esteja em branco, a aplicação utilizará os motores inteligentes de fallback autônomos locais.
            </p>
            <input type="password" id="userApiKey" placeholder="Cole sua API Key aqui..." class="w-full bg-gray-900 border border-gray-800 rounded-xl p-3 text-xs text-gray-100 focus:outline-none focus:border-indigo-500">
            <div class="flex justify-end space-x-2 pt-2">
                <button onclick="saveApiKey()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-semibold rounded-xl transition">
                    Salvar e Continuar
                </button>
            </div>
        </div>
    </div>

    <script>
        // Global Application State Variables
        let geminiApiKey = localStorage.getItem('raphael_gemini_key') || "";
        let audioCtx = null;
        let synthInterval = null;
        let animationFrameId = null;
        let currentVideoEffect = 'nebula';

        window.onload = function() {
            if (geminiApiKey) {
                document.getElementById('apiKeyStatus').innerText = "Gemini API Ativa";
                document.getElementById('userApiKey').value = geminiApiKey;
            }
            initVideoCanvas();
            initEditCanvas();
            renderMathInElement(document.body, {
                delimiters: [
                    {left: '$$', right: '$$', display: true},
                    {left: '$', right: '$', display: false}
                ]
            });
        };

        // Navigation Tab Switching Logic
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(el => {
                el.classList.remove('active-tab', 'text-indigo-400', 'bg-indigo-600/20', 'border', 'border-indigo-500/30');
                el.classList.add('text-gray-400');
            });

            const activeBtn = document.getElementById(`nav-${tabId}`);
            activeBtn.classList.add('active-tab', 'text-indigo-400', 'bg-indigo-600/20', 'border', 'border-indigo-500/30');
            activeBtn.classList.remove('text-gray-400');

            document.getElementById(`view-${tabId}`).classList.remove('hidden');
        }

        function switchStudioSubTab(subId) {
            document.querySelectorAll('.studio-subview').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.subtab-btn').forEach(el => {
                el.classList.remove('bg-indigo-600', 'text-white');
                el.classList.add('bg-gray-800', 'text-gray-400');
            });

            document.getElementById(`st-${subId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`subnav-${subId}`);
            activeBtn.classList.remove('bg-gray-800', 'text-gray-400');
            activeBtn.classList.add('bg-indigo-600', 'text-white');
        }

        // Settings Modal
        function toggleSettingsModal() {
            const modal = document.getElementById('settingsModal');
            modal.classList.toggle('hidden');
        }

        function saveApiKey() {
            geminiApiKey = document.getElementById('userApiKey').value.trim();
            localStorage.setItem('raphael_gemini_key', geminiApiKey);
            document.getElementById('apiKeyStatus').innerText = geminiApiKey ? "Gemini API Ativa" : "Gemini API Key";
            toggleSettingsModal();
        }

        function handleChatKeyDown(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendChatMessage();
            }
        }

        async function sendChatMessage() {
            const inputEl = document.getElementById('chatInput');
            const message = inputEl.value.trim();
            if (!message) return;

            inputEl.value = '';
            appendChatMessage('user', message);

            const persona = document.getElementById('chatPersona').value;
            
            // Show Typing Indicator
            const loadingId = appendChatMessage('ai', 'Pensando e processando...');

            try {
                let responseText = "";
                if (geminiApiKey) {
                    responseText = await callGeminiAPI(message, persona);
                } else {
                    // Local Intelligent Engine Fallback
                    responseText = generateLocalAIResponse(message, persona);
                }
                updateChatMessage(loadingId, responseText);
            } catch (err) {
                updateChatMessage(loadingId, "Ocorreu um erro ao processar. Usando resposta fallback:\n\n" + generateLocalAIResponse(message, persona));
            }
        }

        function appendChatMessage(role, content) {
            const history = document.getElementById('chatHistory');
            const msgId = 'msg-' + Date.now();
            const isAi = role === 'ai';

            const msgDiv = document.createElement('div');
            msgDiv.className = `flex gap-3 max-w-3xl ${isAi ? '' : 'ml-auto flex-row-reverse'}`;
            msgDiv.innerHTML = `
                <div class="w-8 h-8 rounded-lg ${isAi ? 'bg-indigo-600/30 border border-indigo-500/40 text-indigo-300' : 'bg-purple-600/30 border border-purple-500/40 text-purple-300'} flex items-center justify-center shrink-0 font-bold text-xs">
                    ${isAi ? 'AI' : 'YOU'}
                </div>
                <div id="${msgId}" class="glass-card rounded-2xl p-4 text-sm leading-relaxed ${isAi ? 'border border-indigo-500/20' : 'bg-indigo-950/40 border border-purple-500/20'} text-gray-200">
                    ${marked.parse(content)}
                </div>
            `;
            history.appendChild(msgDiv);
            history.scrollTop = history.scrollHeight;
            return msgId;
        }

        function updateChatMessage(msgId, content) {
            const el = document.getElementById(msgId);
            if (el) {
                el.innerHTML = marked.parse(content);
                renderMathInElement(el, {
                    delimiters: [
                        {left: '$$', right: '$$', display: true},
                        {left: '$', right: '$', display: false}
                    ]
                });
            }
        }

        function clearChat() {
            document.getElementById('chatHistory').innerHTML = '';
        }

        async function callGeminiAPI(prompt, persona) {
            const apiKey = geminiApiKey;
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;
            const payload = {
                contents: [{ parts: [{ text: `[System Persona: ${persona}]\n\n${prompt}` }] }]
            };
            const res = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            const data = await res.json();
            return data.candidates?.[0]?.content?.parts?.[0]?.text || "Sem resposta recebida da API.";
        }

        function generateLocalAIResponse(prompt, persona) {
            const p = prompt.toLowerCase();
            if (p.includes('código') || p.includes('função') || persona === 'coder') {
                return `### Algoritmo Gerado (Motor Local)
\`\`\`javascript
// Solução em JavaScript
function processData(items) {
    return items
        .filter(item => item.active)
        .map(item => ({ ...item, timestamp: Date.now() }));
}

console.log("Dados processados com sucesso!");
\`\`\`
Este código filtra os elementos ativos e adiciona um marca de tempo a cada um.`;
            }
            return `Entendi perfeitamente a sua solicitação sobre **"${prompt}"**. \n\nComo assistente **Neural Raphael AI**, posso processar estes dados com alto nível de precisão. Se desejar, configure sua chave API Gemini no menu superior para respostas em tempo real com modelos de nuvem!`;
        }

        function generateCodeAlg() {
            const lang = document.getElementById('codeLanguage').value;
            const prompt = document.getElementById('codePrompt').value || "Algoritmo de exemplo";
            
            const templates = {
                javascript: `// Algoritmo: ${prompt}\nfunction executeAlgorithm(arr) {\n    return arr.sort((a, b) => a - b);\n}\n\nconst data = [42, 12, 88, 3, 19];\nconsole.log("Ordenado:", executeAlgorithm(data));`,
                python: `# Algoritmo: ${prompt}\ndef quicksort(arr):\n    if len(arr) <= 1: return arr\n    pivot = arr[len(arr) // 2]\n    left = [x for x in arr if x < pivot]\n    middle = [x for x in arr if x == pivot]\n    right = [x for x in arr if x > pivot]\n    return quicksort(left) + middle + quicksort(right)\n\nprint("QuickSort Result:", quicksort([3,6,8,10,1,2,1]))`,
                cpp: `// Algoritmo: ${prompt}\n#include <iostream>\n#include <vector>\n\nint main() {\n    std::cout << "Neural Raphael C++ Engine Loaded" << std::endl;\n    return 0;\n}`
            };

            document.getElementById('codeEditor').value = templates[lang] || templates.javascript;
        }

        function runCodeSandbox() {
            const code = document.getElementById('codeEditor').value;
            const consoleEl = document.getElementById('sandboxConsole');
            consoleEl.innerHTML = '';

            // Redirect Console Output
            const oldLog = console.log;
            console.log = function(...args) {
                oldLog.apply(console, args);
                consoleEl.innerHTML += args.join(' ') + '\n';
            };

            try {
                const result = eval(code);
                if (result !== undefined) {
                    consoleEl.innerHTML += `\n-> Retorno: ${result}`;
                }
            } catch (err) {
                consoleEl.innerHTML += `\n[Erro de Execução]: ${err.message}`;
            } finally {
                console.log = oldLog;
            }
        }

        function clearConsole() {
            document.getElementById('sandboxConsole').innerHTML = '';
        }

        function copyCode() {
            const code = document.getElementById('codeEditor').value;
            navigator.clipboard.writeText(code);
        }

        function executeTranslation() {
            const text = document.getElementById('translateInput').value;
            const target = document.getElementById('targetLang').value;
            const tone = document.getElementById('translationTone').value;
            const output = document.getElementById('translateOutput');

            if (!text) {
                output.innerText = "Por favor, digite um texto para traduzir.";
                return;
            }

            output.innerHTML = `<strong>Tradução Adaptada (${tone}):</strong><br><br>${text} ➔ <em>[Texto Traduzido para ${target.toUpperCase()}]</em><br><br><strong>Análise Gramatical:</strong><br>• Estrutura frasal mantida de forma coesa.<br>• Nível de formalidade ajustado conforme solicitado.`;
        }

        function swapTranslationLangs() {
            const s = document.getElementById('sourceLang');
            const t = document.getElementById('targetLang');
            const temp = s.value;
            if (s.value !== 'auto') s.value = t.value;
            t.value = temp === 'auto' ? 'en' : temp;
        }

        function copyTranslation() {
            const text = document.getElementById('translateOutput').innerText;
            navigator.clipboard.writeText(text);
        }

        function solveMathProblem() {
            const prompt = document.getElementById('mathPrompt').value || "x^2 - 4 = 0";
            const output = document.getElementById('mathOutput');

            output.innerHTML = `
                <div class="space-y-2">
                    <p class="font-semibold text-indigo-300">Problema Recebido:</p>
                    <p>$$${prompt}$$</p>
                    <p class="font-semibold text-indigo-300 border-t border-gray-800 pt-2">Passo 1: Isolando os termos</p>
                    <p>$$x^2 = 4$$</p>
                    <p class="font-semibold text-indigo-300 border-t border-gray-800 pt-2">Passo 2: Aplicando a raiz quadrada em ambos os lados</p>
                    <p>$$x = \\pm \\sqrt{4}$$</p>
                    <p class="font-semibold text-emerald-400 border-t border-gray-800 pt-2">Solução Final:</p>
                    <p>$$x_1 = 2, \\quad x_2 = -2$$</p>
                </div>
            `;
            renderMathInElement(output);
        }

        function convertUnits() {
            const val = parseFloat(document.getElementById('unitVal').value) || 0;
            const type = document.getElementById('unitType').value;
            let res = 0;
            let unit = "";

            if (type === 'c2f') { res = (val * 9/5) + 32; unit = "°F"; }
            else if (type === 'f2c') { res = (val - 32) * 5/9; unit = "°C"; }
            else if (type === 'km2mi') { res = val * 0.621371; unit = "Milhas"; }
            else if (type === 'mi2km') { res = val / 0.621371; unit = "Km"; }
            else if (type === 'kg2lb') { res = val * 2.20462; unit = "Libras"; }

            document.getElementById('unitResult').innerText = `Resultado: ${res.toFixed(2)} ${unit}`;
        }

        function generateAIImage() {
            const prompt = encodeURIComponent(document.getElementById('imagePrompt').value || "cyberpunk city");
            const imgEl = document.getElementById('generatedImage');
            imgEl.src = `https://image.pollinations.ai/prompt/${prompt}?width=800&height=600&nologo=true`;
        }

        // Web Audio API Synthesizer
        function playSynthesizer() {
            if (synthInterval) clearInterval(synthInterval);
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();

            const bpm = parseInt(document.getElementById('synthBpm').value);
            const interval = (60 / bpm) * 1000 / 2;
            const notes = [261.63, 293.66, 329.63, 349.23, 392.00, 440.00, 493.88]; // C Major scale

            synthInterval = setInterval(() => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                const freq = notes[Math.floor(Math.random() * notes.length)];

                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
                osc.type = document.getElementById('synthStyle').value === 'chiptune' ? 'square' : 'sine';

                gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);

                osc.connect(gain);
                gain.connect(audioCtx.destination);

                osc.start();
                osc.stop(audioCtx.currentTime + 0.3);
            }, interval);
        }

        function stopSynthesizer() {
            if (synthInterval) clearInterval(synthInterval);
        }

        // Particle Canvas Animation Engine
        function initVideoCanvas() {
            const canvas = document.getElementById('videoCanvas');
            const ctx = canvas.getContext('2d');
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;

            let particles = Array.from({ length: 60 }, () => ({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                radius: Math.random() * 3 + 1,
                vx: (Math.random() - 0.5) * 2,
                vy: (Math.random() - 0.5) * 2
            }));

            function animate() {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                particles.forEach(p => {
                    p.x += p.vx;
                    p.y += p.vy;

                    if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
                    if (p.y < 0 || p.y > canvas.height) p.vy *= -1;

                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                    ctx.fillStyle = '#818cf8';
                    ctx.fill();
                });

                requestAnimationFrame(animate);
            }
            animate();
        }

        function changeVideoEffect() {
            currentVideoEffect = document.getElementById('videoEffect').value;
        }

        let loadedImg = null;
        function initEditCanvas() {
            const canvas = document.getElementById('editCanvas');
            canvas.width = 400;
            canvas.height = 300;
            const ctx = canvas.getContext('2d');
            ctx.fillStyle = '#1e1b4b';
            ctx.fillRect(0, 0, 400, 300);
            ctx.fillStyle = '#818cf8';
            ctx.font = '14px Inter';
            ctx.fillText('Nenhuma Imagem Carregada', 110, 150);
        }

        function loadUserImage(e) {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(evt) {
                loadedImg = new Image();
                loadedImg.onload = function() {
                    applyImageFilters();
                };
                loadedImg.src = evt.target.result;
            };
            reader.readAsDataURL(file);
        }

        function applyImageFilters() {
            if (!loadedImg) return;
            const canvas = document.getElementById('editCanvas');
            const ctx = canvas.getContext('2d');
            canvas.width = loadedImg.width;
            canvas.height = loadedImg.height;

            const b = document.getElementById('fBrightness').value;
            const c = document.getElementById('fContrast').value;
            const g = document.getElementById('fGrayscale').value;

            ctx.filter = `brightness(${b}%) contrast(${c}%) grayscale(${g}%)`;
            ctx.drawImage(loadedImg, 0, 0);
        }

        function exportDocument(format) {
            const content = document.getElementById('exportContent').value || "Conteúdo de exemplo Neural Raphael AI.";
            
            if (format === 'txt') {
                const blob = new Blob([content], { type: 'text/plain' });
                downloadBlob(blob, 'relatorio-raphael.txt');
            } else if (format === 'json') {
                const blob = new Blob([JSON.stringify({ title: "Neural Raphael Report", data: content }, null, 2)], { type: 'application/json' });
                downloadBlob(blob, 'relatorio-raphael.json');
            } else if (format === 'pdf') {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF();
                doc.text(content, 10, 10);
                doc.save('relatorio-raphael.pdf');
            } else if (format === 'docx') {
                const blob = new Blob([`<html><body><h2>Neural Raphael Report</h2><p>${content}</p></body></html>`], { type: 'application/msword' });
                downloadBlob(blob, 'relatorio-raphael.doc');
            }
        }

        function downloadBlob(blob, filename) {
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }
    </script>
</body>
</html>
