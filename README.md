[solu_o_eletr_nica_de_votos_tjpb (13).html](https://github.com/user-attachments/files/31981118/solu_o_eletr_nica_de_votos_tjpb.13.html)
<!DOCTYPE html>
<html lang="pt-BR" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Solução Eletrônica de Votos (SEV-Relatoria) - TJPB</title>
    
    <!-- Script Anti-FOUC para carregar o tema antes do render inicial -->
    <script>
        (function() {
            try {
                const savedTheme = localStorage.getItem('tjpb_sev_theme');
                const systemDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
                if (savedTheme === 'dark' || (!savedTheme && systemDark)) {
                    document.documentElement.setAttribute('data-theme', 'dark');
                    document.documentElement.classList.add('dark');
                } else {
                    document.documentElement.setAttribute('data-theme', 'light');
                    document.documentElement.classList.remove('dark');
                }
            } catch(e) {}
        })();
    </script>

    <!-- Google Fonts & Tailwind CDN -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>

    <script>
        tailwind.config = {
            darkMode: ['class', '[data-theme="dark"]'],
            theme: {
                extend: {
                    colors: {
                        bg: 'var(--bg)',
                        surface: 'var(--surface)',
                        'surface-soft': 'var(--surface-soft)',
                        'surface-hover': 'var(--surface-hover)',
                        primary: 'var(--primary)',
                        'primary-hover': 'var(--primary-hover)',
                        'primary-soft': 'var(--primary-soft)',
                        secondary: 'var(--secondary)',
                        'secondary-soft': 'var(--secondary-soft)',
                        accent: 'var(--accent)',
                        'accent-soft': 'var(--accent-soft)',
                        'text-primary': 'var(--text-primary)',
                        'text-secondary': 'var(--text-secondary)',
                        'text-muted': 'var(--text-muted)',
                        border: 'var(--border)',
                        divider: 'var(--divider)',
                        success: 'var(--success)',
                        warning: 'var(--warning)',
                        danger: 'var(--danger)',
                        info: 'var(--info)'
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace']
                    },
                    boxShadow: {
                        'paper': '0 4px 20px rgba(0, 0, 0, 0.08)',
                        'paper-dark': '0 4px 25px rgba(0, 0, 0, 0.45)',
                        'glow-primary': '0 0 15px rgba(40, 94, 110, 0.35)',
                        'glow-accent': '0 0 15px rgba(196, 154, 72, 0.4)'
                    }
                }
            }
        };
    </script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mammoth/1.6.0/mammoth.browser.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.min.js"></script>
    <script>
        if (typeof pdfjsLib !== 'undefined') {
            pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js';
        }
    </script>
    <!-- Lucide UMD Bundle com fallback seguro -->
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <script>
        if (typeof lucide === 'undefined') {
            document.write('<script src="https://cdn.jsdelivr.net/npm/lucide@latest/dist/umd/lucide.js"><\/script>');
        }
    </script>

    <style>
        :root {
            /* Equilibrium Judicial - Light Mode */
            --bg: #F5F7F8;
            --surface: #FFFFFF;
            --surface-soft: #EDF2F3;
            --surface-hover: #E5ECEE;
            --primary: #285E6E;
            --primary-hover: #204E5C;
            --primary-soft: #DDEBED;
            --secondary: #3F806D;
            --secondary-soft: #E1EFEA;
            --accent: #C49A48;
            --accent-soft: #F5EDD9;
            --text-primary: #17272D;
            --text-secondary: #52656C;
            --text-muted: #78898F;
            --border: #D8E1E3;
            --divider: #E6EBEC;
            --success: #2D7D5F;
            --warning: #B7791F;
            --danger: #B94A48;
            --info: #34789A;
        }

        [data-theme="dark"] {
            /* Equilibrium Judicial - Dark Mode Acessível & Elegante */
            --bg: #131E23;
            --surface: #1B2A31;
            --surface-soft: #23353D;
            --surface-hover: #2B414B;
            --primary: #67A7B5;
            --primary-hover: #80BAC5;
            --primary-soft: #1D3B43;
            --secondary: #72AE96;
            --secondary-soft: #1E3F35;
            --accent: #D6B467;
            --accent-soft: #423820;
            --text-primary: #F4F8F9;
            --text-secondary: #CBD8DB;
            --text-muted: #93A8AF;
            --border: #354F5A;
            --divider: #263C44;
            --success: #65B991;
            --warning: #E0AE59;
            --danger: #DF7773;
            --info: #6DAFD0;
        }

        body {
            background-color: var(--bg);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            min-height: 100vh;
            min-height: 100dvh;
        }

        /* Scrollbars Customizadas */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background-color: var(--border); border-radius: 9999px; }
        ::-webkit-scrollbar-thumb:hover { background-color: var(--text-muted); }
        .scrollbar-custom::-webkit-scrollbar { width: 5px; }

        /* Animação em Alto-Relevo (5 ciclos lentos para atrair atenção) */
        @keyframes highReliefBlink {
            0%, 100% {
                transform: scale(1) translateY(0);
                box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
            }
            50% {
                transform: scale(1.025) translateY(-2px);
                box-shadow: 0 8px 18px -2px rgba(196, 154, 72, 0.45), 0 0 0 2px var(--accent);
            }
        }
        .attention-blink {
            animation: highReliefBlink 1.4s ease-in-out 5;
        }

        /* Card de Processo com Destaques Luminosos */
        .process-card {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            border-left: 4px solid var(--border);
            background: var(--surface);
            position: relative;
        }
        .process-card:hover {
            transform: translateY(-1px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.06);
            border-color: var(--primary);
        }
        .process-card.is-active {
            border-left-width: 7px !important;
            border-left-color: var(--primary) !important;
            background-color: var(--surface-soft);
            box-shadow: 0 0 0 2px var(--primary-soft), 0 8px 24px -2px rgba(40, 94, 110, 0.28);
            transform: translateX(2px);
            z-index: 10;
        }
        .process-card.has-prioridade {
            border-left-color: var(--accent) !important;
            background: linear-gradient(to right, var(--accent-soft), var(--surface));
        }
        .process-card.has-sustentacao {
            border-left-color: var(--info) !important;
            background: linear-gradient(to right, var(--primary-soft), var(--surface));
        }
        .process-card.has-prioridade.has-sustentacao {
            border-left-color: var(--accent) !important;
            background: linear-gradient(to right, var(--accent-soft), var(--surface-soft));
        }

        /* Botões Rápidos de Votação */
        .btn-vote {
            transition: all 0.15s ease;
            background-color: var(--surface);
            border: 1px solid var(--border);
            color: var(--text-secondary);
            font-size: 9.5px;
            font-weight: 700;
            padding: 5px 3px;
            border-radius: 6px;
            cursor: pointer;
            outline: none;
            text-align: center;
            user-select: none;
        }
        .btn-vote:hover {
            background-color: var(--surface-hover);
            color: var(--text-primary);
        }
        .btn-vote.active-provido, .btn-vote.active-procedente, .btn-vote.active-concedida {
            background-color: var(--success) !important;
            border-color: var(--success) !important;
            color: #ffffff !important;
            box-shadow: 0 1px 3px rgba(0,0,0,0.15);
        }
        .btn-vote.active-improvido, .btn-vote.active-improcedente, .btn-vote.active-denegada {
            background-color: var(--danger) !important;
            border-color: var(--danger) !important;
            color: #ffffff !important;
            box-shadow: 0 1px 3px rgba(0,0,0,0.15);
        }
        .btn-vote.active-parcial, .btn-vote.active-pendente {
            background-color: var(--warning) !important;
            border-color: var(--warning) !important;
            color: #ffffff !important;
            box-shadow: 0 1px 3px rgba(0,0,0,0.15);
        }
        .btn-vote.active-nconhecido, .btn-vote.active-prejudicado {
            background-color: #7c3aed !important;
            border-color: #6d28d9 !important;
            color: #ffffff !important;
            box-shadow: 0 1px 3px rgba(0,0,0,0.15);
        }
        .btn-vote.active-extincao {
            background-color: var(--text-secondary) !important;
            border-color: var(--text-primary) !important;
            color: #ffffff !important;
        }
        .btn-vote.active-adiado {
            background-color: #0f172a !important;
            border-color: #000000 !important;
            color: #ffffff !important;
        }

        /* Editor Forense A4 Padronizado (Eternamente folha branca física) */
        .editor-voto {
            font-family: Arial, Helvetica, sans-serif;
            font-size: 12pt;
            line-height: 1.5;
            text-align: justify;
            color: #1e293b;
            min-height: 860px;
            outline: none;
            padding: 2cm 1.5cm;
            background: #ffffff !important;
            box-shadow: 0 10px 25px -5px rgba(0,0,0,0.1), 0 0 1px rgba(0,0,0,0.1);
            border-radius: 4px;
        }
        @media (min-width: 768px) {
            .editor-voto {
                padding: 3cm 2cm 2cm 3cm;
            }
        }
        .editor-voto p {
            margin-bottom: 1em;
            text-indent: 1.25cm;
            margin-left: 0cm;
        }
        .editor-voto blockquote, .editor-voto .citacao-longa {
            margin: 1.2em 0 1.2em 4cm !important;
            font-size: 10.5pt !important;
            line-height: 1.2 !important;
            text-align: justify !important;
            text-indent: 0cm !important;
        }
        .edit-red {
            color: #dc2626 !important;
            font-weight: 500;
        }

        /* Régua Horizontal Forense */
        .regua-container {
            background: var(--surface-soft);
            border-bottom: 1px solid var(--border);
            height: 22px;
            position: relative;
            user-select: none;
            width: 100%;
            border-top-left-radius: 8px;
            border-top-right-radius: 8px;
        }
        .marcador-primeira-linha {
            position: absolute; width: 10px; margin-left: -5px; cursor: ew-resize; top: 0;
            display: flex; flex-direction: column; align-items: center; z-index: 30; height: 9px;
        }
        .marcador-paragrafo-inteiro {
            position: absolute; width: 10px; margin-left: -5px; cursor: ew-resize; bottom: 0;
            display: flex; flex-direction: column; align-items: center; z-index: 30; height: 9px;
        }
        .marcador-primeira-linha .icone-triangulo-baixo {
            width: 0; height: 0; border-left: 5px solid transparent; border-right: 5px solid transparent; border-top: 7px solid var(--primary);
        }
        .marcador-paragrafo-inteiro .icone-triangulo-cima {
            width: 0; height: 0; border-left: 5px solid transparent; border-right: 5px solid transparent; border-bottom: 6px solid var(--text-primary);
        }
        .marcador-paragrafo-inteiro .corpo-base-quadrada {
            width: 6px; height: 4px; background: var(--text-primary); border-radius: 1px; margin: 0 auto;
        }

        .pill-filter {
            padding: 4px 12px; border-radius: 9999px; font-size: 10.5px; font-weight: 700; cursor: pointer;
            transition: all 0.2s; border: 1px solid var(--border); background: var(--surface); color: var(--text-secondary); white-space: nowrap;
        }
        .pill-filter:hover { background: var(--surface-hover); color: var(--text-primary); }
        .pill-filter.active { background: var(--primary); color: #ffffff; border-color: var(--primary); }

        .seg-btn { padding: 4px 10px; font-size: 10.5px; font-weight: 700; color: var(--text-muted); transition: all 0.2s; border-radius: 6px; }
        .seg-btn.active { background: var(--surface); color: var(--text-primary); box-shadow: 0 1px 3px rgba(0,0,0,0.1); }

        /* Impressão Limpa em A4 */
        @media print {
            body { background: white !important; color: black !important; }
            header, aside, #barra-atalhos-adaptativos, #editor-toolbar, .regua-container, #ai-drawer, #toast-container, .no-print {
                display: none !important;
            }
            main, #main-leitor, #painel-leitura-ativo {
                overflow: visible !important; width: 100% !important; height: auto !important; margin: 0 !important; padding: 0 !important;
            }
            .editor-voto {
                box-shadow: none !important; border: none !important; padding: 0 !important; min-height: auto !important;
            }
        }
    </style>
</head>
<body id="app-body" class="flex flex-col h-screen overflow-hidden antialiased select-text">

    <header class="bg-surface text-text-primary border-b border-border flex flex-col shrink-0 z-30 shadow-xs">
        
        <!-- LINHA 1: Título Institucional + Central de Acessos + Importações + Menu de Ações -->
        <div class="flex flex-wrap items-center justify-between px-3 md:px-5 py-2 border-b border-divider gap-2">
            
            <div class="flex items-center gap-2.5 shrink-0">
                <button onclick="toggleSidebar()" class="text-text-secondary hover:text-text-primary p-1.5 rounded-lg hover:bg-surface-hover transition" aria-label="Abrir/Fechar Barra Lateral">
                    <i data-lucide="menu" class="w-4 h-4"></i>
                </button>
                <div class="flex items-center gap-2">
                    <div class="bg-primary text-white p-2 rounded-xl font-black flex items-center justify-center h-8 w-8 shadow-sm shrink-0">
                        <i data-lucide="scale" class="w-4 h-4"></i>
                    </div>
                    <div class="flex flex-col leading-tight">
                        <span class="text-sm md:text-base font-black tracking-wide uppercase text-text-primary">SEV-Relatoria</span>
                        <span class="text-[9px] md:text-[10px] font-bold text-accent uppercase tracking-wider">Tribunal de Justiça da Paraíba</span>
                    </div>
                </div>
            </div>

            <!-- Links Rápidos e Ferramentas Estendidas -->
            <div class="flex items-center gap-1.5 flex-wrap shrink-0">
                
                <!-- Central de Sistemas e Atalhos Principais -->
                <button onclick="abrirModalCentralLinks()" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-primary border border-border px-2.5 py-1 rounded-md text-[11px] font-bold transition-all shadow-xs" title="Central de Sistemas TJPB, Google Workspace e IAs">
                    <i data-lucide="layout-grid" class="w-3.5 h-3.5 text-accent"></i>
                    <span>Sistemas & IAs</span>
                </button>

                <!-- Links Rápidos Diretos (Desktop e Telas Médias/Grandes) -->
                <div class="hidden lg:flex items-center gap-1 border-r border-border pr-2">
                    <a href="https://pjesg.tjpb.jus.br/pje2g/ng2/dev.seam#/painel-usuario-interno" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-info px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="PJe 2º Grau">
                        <i data-lucide="external-link" class="w-3 h-3"></i> PJe 2G
                    </a>
                    <a href="https://minutaia.tjpb.jus.br/login" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-primary px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Minuta IA TJPB">
                        <i data-lucide="file-text" class="w-3 h-3 text-primary"></i> Minuta IA
                    </a>
                    <a href="https://gemini.google.com/app" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-secondary px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Google Gemini">
                        <i data-lucide="sparkles" class="w-3 h-3 text-secondary"></i> Gemini
                    </a>
                    <a href="https://www.jusbrasil.com.br/jurisprudencia/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-text-primary px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Jusbrasil Jurisprudência">
                        <i data-lucide="scale" class="w-3 h-3 text-primary"></i> Jusbrasil
                    </a>
                    <a href="https://ia.jusbrasil.com.br/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-accent px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Jus IA">
                        <i data-lucide="bot" class="w-3 h-3 text-accent"></i> Jus IA
                    </a>
                    <a href="https://sei.tjpb.jus.br/sip/login.php?sigla_orgao_sistema=TJPB&sigla_sistema=SEI&infra_url=L3NlaS8=" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-success px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Sistema SEI TJPB">
                        <i data-lucide="file-check" class="w-3 h-3 text-success"></i> SEI
                    </a>
                    <a href="https://www.cnj.jus.br/sistemas/datajud/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-accent px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="DataJud CNJ">
                        <i data-lucide="landmark" class="w-3 h-3 text-accent"></i> DataJud
                    </a>
                    <a href="https://qlik-sense6.tjpb.jus.br/qap/single/?appid=2e9f5df1-ba98-46d5-a764-8c3986bd2e1e&sheet=7f4f4bd4-2486-4369-a768-ec8b032177ee&lang=pt-BR&opt=currsel%2Cctxmenu&select=IDOJ,132" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-primary px-2 py-1 rounded-md border border-border font-semibold text-[10.5px]" title="Painel PJE + 2º G (Qlik Sense)">
                        <i data-lucide="pie-chart" class="w-3 h-3 text-primary"></i> PJE + 2G
                    </a>

                    <!-- Google Workspace Dropdown -->
                    <div class="relative group">
                        <button class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-text-primary px-2 py-1 rounded-md border border-border font-semibold text-[10.5px] transition-all" title="Google Workspace Institucional">
                            <i data-lucide="cloud" class="w-3 h-3 text-info"></i>
                            <span>Google</span>
                            <i data-lucide="chevron-down" class="w-2.5 h-2.5 opacity-60"></i>
                        </button>
                        <div class="absolute right-0 top-full mt-1 w-44 bg-surface rounded-xl shadow-xl border border-border opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all z-50 p-1 space-y-0.5 text-xs">
                            <a href="https://docs.google.com/document/u/0/?pli=1&tgif=d" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="file-text" class="w-3.5 h-3.5 text-info"></i> Docs
                            </a>
                            <a href="https://mail.google.com/mail/u/0/?service=mail&flowName=GlifWebSignIn&flowEntry=AccountChooser&ec=asw-gmail-globalnav-signin#inbox" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="mail" class="w-3.5 h-3.5 text-danger"></i> Gmail
                            </a>
                            <a href="https://chat.google.com/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="message-circle" class="w-3.5 h-3.5 text-success"></i> Chat
                            </a>
                            <a href="https://drive.google.com/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="hard-drive" class="w-3.5 h-3.5 text-accent"></i> Drive
                            </a>
                            <a href="https://meet.google.com/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="video" class="w-3.5 h-3.5 text-primary"></i> Meet
                            </a>
                        </div>
                    </div>

                    <!-- Suporte DITEC Dropdown -->
                    <div class="relative group">
                        <button class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-success px-2 py-1 rounded-md border border-border font-semibold text-[10.5px] transition-all" title="Suporte Técnico DITEC">
                            <i data-lucide="headset" class="w-3 h-3 text-success"></i>
                            <span>DITEC</span>
                            <i data-lucide="chevron-down" class="w-2.5 h-2.5 opacity-60"></i>
                        </button>
                        <div class="absolute right-0 top-full mt-1 w-48 bg-surface rounded-xl shadow-xl border border-border opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all z-50 p-1 space-y-0.5 text-xs">
                            <a href="https://portaldousuario.tjpb.jus.br/" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-text-primary transition">
                                <i data-lucide="headset" class="w-3.5 h-3.5 text-secondary"></i> Chamados TI
                            </a>
                            <a href="https://api.whatsapp.com/send?phone=5583993819106" target="_blank" rel="noopener noreferrer" class="flex items-center gap-2 p-1.5 hover:bg-surface-soft rounded-lg text-success transition">
                                <i data-lucide="message-square" class="w-3.5 h-3.5 text-success"></i> WhatsApp DITEC
                            </a>
                        </div>
                    </div>
                </div>

                <!-- Botão de Aprendizado Adaptativo -->
                <button onclick="abrirModalAprendizado()" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-accent border border-border px-2 py-1 rounded-md text-[10.5px] font-bold transition-all" title="Motor de Aprendizado Adaptativo">
                    <i data-lucide="brain-circuit" class="w-3.5 h-3.5"></i>
                    <span class="hidden sm:inline">Adaptativo</span>
                </button>

                <!-- Botões Separados de Importação -->
                <div class="flex items-center gap-1 bg-surface-soft p-0.5 rounded-lg border border-border">
                    <label class="flex items-center gap-1 bg-surface hover:bg-surface-hover text-text-primary px-2 py-1 rounded-md text-[10.5px] font-bold cursor-pointer transition border border-border" title="Importar voto avulso">
                        <i data-lucide="file-plus" class="w-3.5 h-3.5 text-primary"></i>
                        <span class="hidden sm:inline">Importar Voto</span>
                        <input type="file" accept=".pdf,.docx,.doc,.docm,.dot,.dotx,.odt,.txt,.html" onchange="processarArquivosImportados(this, false)" class="hidden">
                    </label>
                    <label class="flex items-center gap-1 bg-primary hover:bg-primary-hover text-white px-2.5 py-1 rounded-md text-[10.5px] font-black cursor-pointer transition shadow-xs" title="Importar múltiplos votos da pauta">
                        <i data-lucide="files" class="w-3.5 h-3.5 text-accent"></i>
                        <span>Importar Lote</span>
                        <input type="file" accept=".pdf,.docx,.doc,.docm,.dot,.dotx,.odt,.txt,.html" multiple onchange="processarArquivosImportados(this, true)" class="hidden">
                    </label>
                </div>

                <!-- Alternador de Tema Claro/Escuro -->
                <button onclick="alternarTemaManual()" class="p-1.5 rounded-lg border border-border bg-surface-soft hover:bg-surface-hover text-text-secondary hover:text-text-primary transition shadow-xs" title="Alternar Modo Claro / Escuro" aria-label="Alternar Tema">
                    <i data-lucide="sun" class="w-4 h-4 hidden dark:block text-accent"></i>
                    <i data-lucide="moon" class="w-4 h-4 block dark:hidden text-primary"></i>
                </button>

                <!-- Assistente Local Gemini -->
                <button onclick="toggleAiDrawer()" class="flex items-center gap-1 bg-surface-soft hover:bg-surface-hover text-accent border border-border px-2 py-1 rounded-md text-[10.5px] font-bold transition-all" title="Assistente Local IA">
                    <i data-lucide="bot" class="w-3.5 h-3.5 text-accent"></i>
                    <span class="hidden md:inline">Assistente</span>
                </button>

                <!-- Menu Extra / Sessão -->
                <div class="relative group">
                    <button class="flex items-center justify-center w-7 h-7 bg-surface-soft hover:bg-surface-hover text-text-secondary rounded-md transition border border-border" aria-label="Menu de Sessão">
                        <i data-lucide="more-vertical" class="w-3.5 h-3.5"></i>
                    </button>
                    <div class="absolute right-0 top-full mt-1 w-56 bg-surface rounded-xl shadow-xl border border-border opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all z-50 overflow-hidden text-text-primary">
                        <div class="p-1.5 text-xs font-medium space-y-0.5">
                            <label class="flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg cursor-pointer transition">
                                <i data-lucide="folder-input" class="w-4 h-4 text-secondary"></i> Carregar Sessão (.pir)
                                <input type="file" accept=".pir,.json" onchange="carregarSessaoPirPackage(this)" class="hidden">
                            </label>
                            <button onclick="exportarSessaoPirPackage()" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left transition">
                                <i data-lucide="share-2" class="w-4 h-4 text-accent"></i> Exportar Sessão (.pir)
                            </button>
                            <div class="h-px bg-divider my-1"></div>
                            <button onclick="abrirModalNovaPauta()" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left transition">
                                <i data-lucide="calendar-plus" class="w-4 h-4 text-info"></i> Nova Pauta
                            </button>
                            <button onclick="gerarAtaJulgamento()" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left transition">
                                <i data-lucide="file-signature" class="w-4 h-4 text-primary"></i> Gerar Ata de Julgamento
                            </button>
                            <div class="h-px bg-divider my-1"></div>
                            <button onclick="limparCacheMemoria()" class="w-full flex items-center gap-2 p-2 hover:bg-danger/10 text-danger rounded-lg text-left transition">
                                <i data-lucide="trash-2" class="w-4 h-4"></i> Limpar Pauta Atual
                            </button>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <!-- LINHA 2: Seletores de Câmara, Relator com Realce de Alto-Relevo, Data da Sessão e Contadores -->
        <div class="bg-surface-soft px-3 md:px-5 py-1.5 flex flex-wrap items-center justify-between gap-2 border-b border-border text-xs">
            
            <div class="flex items-center gap-1.5 flex-wrap">
                <!-- Órgão / Câmara -->
                <select id="header-select-unidade" onchange="aoMudarUnidadeHeader(this.value)" class="bg-surface text-text-primary font-bold text-[11px] px-2.5 py-1 rounded-md border border-border outline-none cursor-pointer hover:bg-surface-hover transition shadow-xs">
                    <option value="Tribunal Pleno">Tribunal Pleno</option>
                    <option value="Órgão Especial">Órgão Especial</option>
                    <option value="Seção Especializada Cível">Seção Especializada Cível</option>
                    <option value="1ª Câmara Especializada Cível">1ª Câmara Cível</option>
                    <option value="2ª Câmara Especializada Cível">2ª Câmara Cível</option>
                    <option value="3ª Câmara Especializada Cível" selected>3ª Câmara Cível</option>
                    <option value="4ª Câmara Especializada Cível">4ª Câmara Cível</option>
                    <option value="Câmara Especializada Criminal">Câmara Criminal</option>
                </select>

                <!-- Relator com Animação em Alto-Relevo (5 Ciclos) -->
                <div class="relative attention-blink rounded-md">
                    <select id="header-select-relator" onchange="aoMudarRelatorHeader(this.value)" class="bg-surface text-accent font-black text-[11px] px-2.5 py-1 rounded-md border border-accent/40 outline-none cursor-pointer max-w-[220px] sm:max-w-[270px] truncate hover:bg-surface-hover transition shadow-xs" title="Relator(a) Titular da Sessão">
                        <!-- Injetado dinamicamente -->
                    </select>
                </div>

                <!-- Data da Pauta / Sessão com Efeito de Alto-Relevo -->
                <div class="flex items-center gap-1.5 bg-surface px-2.5 py-1 rounded-md border border-accent/40 attention-blink text-[11px] font-bold text-text-primary relative cursor-pointer shadow-xs" title="Clique para alterar a data da pauta">
                    <i data-lucide="calendar" class="w-3.5 h-3.5 text-accent"></i>
                    <span>Sessão:</span>
                    <span id="lbl-data-pauta" class="text-accent font-black">--/--/----</span>
                    <input type="date" id="input-data-pauta" onchange="aoMudarDataPauta(this.value)" class="absolute inset-0 opacity-0 w-full cursor-pointer">
                </div>

                <!-- Hora e Modalidade -->
                <div class="hidden sm:flex items-center gap-1 bg-surface px-2 py-0.5 rounded-md border border-border text-[10px] text-text-muted font-mono">
                    <i data-lucide="clock" class="w-3 h-3 text-secondary"></i>
                    <span id="lbl-hora-hoje">--:--:--</span>
                </div>

                <div class="flex items-center gap-1 text-[11px] font-bold text-secondary relative cursor-pointer bg-surface px-2 py-1 rounded-md border border-border">
                    <i data-lucide="video" class="w-3 h-3"></i>
                    <span id="lbl-modalidade">Videoconferência</span>
                    <select id="select-modalidade-sessao" onchange="mudarModalidade(this.value)" class="absolute inset-0 opacity-0 w-full cursor-pointer">
                        <option value="Videoconferência">Videoconferência</option>
                        <option value="Presencial">Presencial</option>
                        <option value="Híbrida">Híbrida</option>
                    </select>
                </div>

                <!-- Campo de Escolha da Ordem dos Processos (# Início da Sessão - até 500 processos) -->
                <div class="flex items-center gap-1.5 bg-surface px-2 py-1 rounded-md border border-accent/40 text-[11px] font-bold text-text-primary shadow-xs" title="Número inicial da pauta na sessão (suporta até 500 processos)">
                    <i data-lucide="hash" class="w-3 h-3 text-accent"></i>
                    <span class="text-text-secondary"># Início:</span>
                    <input type="number" id="input-ordem-inicio-header" min="1" max="500" value="35" onchange="aoMudarNumInicioDireto(this.value)" class="w-12 bg-surface-soft text-accent font-black text-center rounded border border-border outline-none focus:border-accent text-[11px] py-0.5" title="Digite o número inicial da sessão (1 a 500)">
                    <button type="button" onclick="abrirModalConfigOrdem()" class="text-text-muted hover:text-text-primary p-0.5 rounded transition" title="Configurar sequência detalhada da pauta">
                        <i data-lucide="sliders-horizontal" class="w-3 h-3"></i>
                    </button>
                </div>
            </div>

            <!-- Contadores Rápidos da Sessão -->
            <div class="flex items-center gap-2.5 text-[10.5px] font-bold text-text-secondary ml-auto">
                <span>TOTAL: <strong id="cnt-gabinete" class="text-text-primary">0</strong></span>
                <span>JULGADOS: <strong id="cnt-julgados" class="text-success">0</strong></span>
                <span>PENDENTES: <strong id="cnt-pendentes" class="text-warning">0</strong></span>
                <span class="hidden md:inline">SUSTENTAÇÃO: <strong id="cnt-sustentacao" class="text-info">0</strong></span>
                
                <div class="hidden lg:flex items-center gap-1.5 ml-1">
                    <div class="w-16 bg-surface h-2 rounded-full overflow-hidden border border-border">
                        <div id="bar-progresso" class="bg-success h-full w-0 transition-all duration-300"></div>
                    </div>
                    <span id="lbl-progresso-pct" class="text-success font-mono text-[10px] w-7 text-right">0%</span>
                </div>
            </div>

        </div>
    </header>

    <div class="flex-1 flex overflow-hidden relative">

        <!-- BARRA LATERAL DA PAUTA (500px desktop, responsiva para tablet/celular) -->
        <aside id="aside-pauta" class="w-full md:w-[480px] lg:w-[500px] border-r border-border bg-surface flex flex-col shrink-0 z-20 shadow-sm transition-all duration-300 absolute md:relative inset-y-0 left-0">
            
            <!-- Barra de Atalhos Adaptativos (Motor de Aprendizado) -->
            <div id="barra-atalhos-adaptativos" class="bg-surface-soft px-3 py-1.5 border-b border-border flex items-center justify-between gap-1 text-[10.5px]">
                <div class="flex items-center gap-1 text-text-muted font-bold">
                    <i data-lucide="sparkles" class="w-3.5 h-3.5 text-accent"></i>
                    <span>Atalhos Inteligentes:</span>
                </div>
                <div id="container-atalhos-inteligentes" class="flex items-center gap-1 overflow-x-auto scrollbar-custom">
                    <!-- Gerados pelo AdaptiveEngine -->
                    <button onclick="filtrarPauta('Pendentes')" class="px-2 py-0.5 rounded bg-surface border border-border text-text-secondary hover:text-text-primary font-bold transition text-[10px]">Pendentes</button>
                    <button onclick="document.getElementById('input-busca-pauta').focus()" class="px-2 py-0.5 rounded bg-surface border border-border text-text-secondary hover:text-text-primary font-bold transition text-[10px]">Buscar</button>
                </div>
            </div>

            <!-- Busca e Filtros da Pauta -->
            <div class="p-3 border-b border-border space-y-2 shrink-0 bg-surface">
                
                <div class="relative flex items-center">
                    <i data-lucide="search" class="w-4 h-4 text-text-muted absolute left-3"></i>
                    <input type="text" id="input-busca-pauta" oninput="renderizarListaPauta()" placeholder="Pesquisar por CNJ, partes, assessor, classe..." class="w-full text-xs pl-9 pr-3 py-2 rounded-xl border border-border bg-surface-soft focus:bg-surface focus:outline-none focus:border-primary transition text-text-primary font-medium placeholder:text-text-muted shadow-inner">
                </div>

                <!-- Pílulas de Filtro -->
                <div class="flex items-center gap-1.5 overflow-x-auto pb-0.5 scrollbar-custom">
                    <button onclick="filtrarPauta('Todos')" class="pill-filter active" id="filter-Todos">Todos</button>
                    <button onclick="filtrarPauta('Pendentes')" class="pill-filter" id="filter-Pendentes">Pendentes</button>
                    <button onclick="filtrarPauta('Julgados')" class="pill-filter" id="filter-Julgados">Julgados</button>
                    <button onclick="filtrarPauta('Prioridades')" class="pill-filter" id="filter-Prioridades">Prioridades</button>
                </div>

                <div class="flex items-center justify-between text-xs pt-0.5">
                    <label class="flex items-center gap-1.5 font-semibold cursor-pointer text-text-secondary hover:text-text-primary text-[11px]">
                        <input type="checkbox" id="chk-selecionar-todos" onchange="alternarSelecionarTodos(this.checked)" class="w-3.5 h-3.5 rounded border-border text-primary focus:ring-0">
                        <span>Sel. Lote</span>
                    </label>

                    <div class="flex items-center bg-surface-soft rounded-lg p-0.5 border border-border font-bold">
                        <button onclick="alternarModoOrdem('original')" id="btn-ordem-orig" class="seg-btn">Gabinete</button>
                        <button onclick="alternarModoOrdem('sessao')" id="btn-ordem-sess" class="seg-btn active">Sessão</button>
                    </div>
                </div>
            </div>

            <!-- LISTA DE PROCESSOS -->
            <div id="container-lista-pauta" class="lista-processos flex-1 p-2.5 space-y-2 overflow-y-auto scrollbar-custom bg-bg">
                <!-- Preenchido via JavaScript -->
            </div>

            <!-- BARRA DE AÇÕES EM LOTE -->
            <div id="barra-acoes-em-lote" class="p-2.5 border-t border-primary/30 bg-primary-soft flex flex-col gap-1.5 shrink-0 hidden shadow-md">
                <div class="flex items-center justify-between font-bold text-primary text-xs">
                    <span id="lbl-total-selecionados">0 selecionado(s)</span>
                    <button onclick="desmarcarTodosProcessos()" class="text-primary hover:underline text-[11px]">Desmarcar</button>
                </div>
                <div class="flex items-center gap-1">
                    <button onclick="aplicarStatusEmLote('Adiado', 'Adiado')" class="flex-1 py-1 bg-surface hover:bg-surface-hover text-text-primary text-[11px] font-bold rounded-lg border border-border shadow-xs transition">
                        Adiar Lote
                    </button>
                    <button onclick="excluirProcessosEmLote()" class="p-1.5 bg-danger/10 hover:bg-danger/20 text-danger font-bold rounded-lg border border-danger/20 transition" title="Excluir Lote">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                    </button>
                </div>
            </div>

        </aside>

        <!-- ÁREA PRINCIPAL: LEITOR E EDITOR DE VOTOS -->
        <main id="main-leitor" class="flex-1 flex flex-col bg-bg overflow-hidden relative min-w-0">

            <!-- FAIXA DE SUGESTÃO DE PRÓXIMA AÇÃO (Motor Adaptativo) -->
            <div id="faixa-sugestao-adaptativa" class="bg-accent-soft border-b border-accent/30 px-4 py-1.5 flex items-center justify-between text-xs text-text-primary hidden z-20">
                <div class="flex items-center gap-2">
                    <i data-lucide="sparkles" class="w-4 h-4 text-accent shrink-0"></i>
                    <span id="txt-sugestao-adaptativa" class="font-medium">Sugestão contextual calculada</span>
                </div>
                <div class="flex items-center gap-2">
                    <button id="btn-acao-sugestao" class="px-2.5 py-0.5 bg-accent text-white font-bold rounded-md hover:opacity-90 transition text-[11px]">Executar</button>
                    <button onclick="dispensarSugestaoAtual()" class="p-1 text-text-muted hover:text-text-primary rounded" title="Dispensar"><i data-lucide="x" class="w-3.5 h-3.5"></i></button>
                </div>
            </div>

            <!-- ESTACA ZERO / ESTADO VAZIO COM MANUAL E SISTEMAS INTEGRADOS -->
            <div id="estaca-zero-view" class="flex-1 overflow-y-auto p-4 md:p-8 flex flex-col items-center justify-start">
                <div class="bg-surface rounded-3xl shadow-sm max-w-4xl w-full p-6 md:p-8 border border-border my-auto space-y-6">
                    
                    <div class="text-center flex flex-col items-center">
                        <div class="w-14 h-14 bg-primary-soft text-primary rounded-2xl flex items-center justify-center mb-3 shadow-inner">
                            <i data-lucide="scale" class="w-7 h-7 stroke-[2.2px]"></i>
                        </div>
                        <h2 class="text-lg md:text-xl font-black text-text-primary tracking-tight">Solução Eletrônica de Votos (SEV-Relatoria)</h2>
                        <p class="text-xs text-text-secondary font-medium mt-1">Tribunal de Justiça da Paraíba • Gabinete 24</p>
                        
                        <div class="flex items-center gap-2 mt-4">
                            <label class="inline-flex items-center gap-2 bg-surface hover:bg-surface-hover text-text-primary border border-border px-4 py-2.5 rounded-xl text-xs font-bold cursor-pointer transition shadow-xs">
                                <i data-lucide="file-plus" class="w-4 h-4 text-primary"></i>
                                <span>Importar Voto Único</span>
                                <input type="file" accept=".pdf,.docx,.doc,.docm,.dot,.dotx,.odt,.txt,.html" onchange="processarArquivosImportados(this, false)" class="hidden">
                            </label>
                            <label class="inline-flex items-center gap-2 bg-primary hover:bg-primary-hover text-white px-5 py-2.5 rounded-xl text-xs font-bold cursor-pointer transition shadow-md">
                                <i data-lucide="files" class="w-4 h-4 text-accent"></i>
                                <span>Importar Votos em Lote</span>
                                <input type="file" accept=".pdf,.docx,.doc,.docm,.dot,.dotx,.odt,.txt,.html" multiple onchange="processarArquivosImportados(this, true)" class="hidden">
                            </label>
                        </div>
                    </div>

                    <!-- SEÇÃO: GOOGLE WORKSPACE INSTITUCIONAL -->
                    <div class="space-y-2">
                        <div class="flex items-center gap-2 text-text-secondary text-[11px] font-bold uppercase tracking-wider">
                            <i data-lucide="cloud" class="w-4 h-4 text-info"></i>
                            <span>Google Workspace Institucional</span>
                        </div>
                        <div class="grid grid-cols-2 sm:grid-cols-5 gap-2.5">
                            <a href="https://chat.google.com/" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3 rounded-2xl border border-border hover:border-primary transition flex flex-col items-center justify-center text-center shadow-xs">
                                <div class="w-10 h-10 rounded-full bg-[#131E23] dark:bg-[#23353D] text-[#65B991] flex items-center justify-center mb-1.5 shadow-sm group-hover:scale-105 transition-transform">
                                    <i data-lucide="message-circle" class="w-5 h-5"></i>
                                </div>
                                <span class="text-xs font-bold text-text-primary">Chat</span>
                            </a>
                            <a href="https://docs.google.com/document/u/0/?pli=1&tgif=d" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3 rounded-2xl border border-border hover:border-primary transition flex flex-col items-center justify-center text-center shadow-xs">
                                <div class="w-10 h-10 rounded-full bg-[#131E23] dark:bg-[#23353D] text-[#6DAFD0] flex items-center justify-center mb-1.5 shadow-sm group-hover:scale-105 transition-transform">
                                    <i data-lucide="file-text" class="w-5 h-5"></i>
                                </div>
                                <span class="text-xs font-bold text-text-primary">Docs</span>
                            </a>
                            <a href="https://drive.google.com/" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3 rounded-2xl border border-border hover:border-primary transition flex flex-col items-center justify-center text-center shadow-xs">
                                <div class="w-10 h-10 rounded-full bg-[#131E23] dark:bg-[#23353D] text-[#E0AE59] flex items-center justify-center mb-1.5 shadow-sm group-hover:scale-105 transition-transform">
                                    <i data-lucide="hard-drive" class="w-5 h-5"></i>
                                </div>
                                <span class="text-xs font-bold text-text-primary">Drive</span>
                            </a>
                            <a href="https://mail.google.com/mail/u/0/?service=mail&flowName=GlifWebSignIn&flowEntry=AccountChooser&ec=asw-gmail-globalnav-signin#inbox" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3 rounded-2xl border border-border hover:border-primary transition flex flex-col items-center justify-center text-center shadow-xs">
                                <div class="w-10 h-10 rounded-full bg-[#131E23] dark:bg-[#23353D] text-[#DF7773] flex items-center justify-center mb-1.5 shadow-sm group-hover:scale-105 transition-transform">
                                    <i data-lucide="mail" class="w-5 h-5"></i>
                                </div>
                                <span class="text-xs font-bold text-text-primary">Gmail</span>
                            </a>
                            <a href="https://meet.google.com/" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3 rounded-2xl border border-border hover:border-primary transition flex flex-col items-center justify-center text-center shadow-xs col-span-2 sm:col-span-1">
                                <div class="w-10 h-10 rounded-full bg-[#131E23] dark:bg-[#23353D] text-[#67A7B5] flex items-center justify-center mb-1.5 shadow-sm group-hover:scale-105 transition-transform">
                                    <i data-lucide="video" class="w-5 h-5"></i>
                                </div>
                                <span class="text-xs font-bold text-text-primary">Meet</span>
                            </a>
                        </div>
                    </div>

                    <!-- SEÇÃO: INTELIGÊNCIA ARTIFICIAL & PESQUISA -->
                    <div class="space-y-2">
                        <div class="flex items-center gap-2 text-text-secondary text-[11px] font-bold uppercase tracking-wider">
                            <i data-lucide="cpu" class="w-4 h-4 text-accent"></i>
                            <span>Inteligência Artificial & Pesquisa</span>
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2.5">
                            <a href="https://gemini.google.com/app" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3.5 rounded-2xl border border-border hover:border-primary transition flex flex-col justify-between shadow-xs">
                                <div>
                                    <div class="flex items-center justify-between mb-1">
                                        <h4 class="font-extrabold text-xs text-[#6366f1] group-hover:text-primary transition-colors">Google Gemini</h4>
                                        <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted opacity-50 group-hover:opacity-100"></i>
                                    </div>
                                    <p class="text-[10px] text-text-secondary leading-snug">Minutas, sínteses e análises processuais.</p>
                                </div>
                            </a>
                            <a href="https://www.jusbrasil.com.br/jurisprudencia/" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3.5 rounded-2xl border border-border hover:border-primary transition flex flex-col justify-between shadow-xs">
                                <div>
                                    <div class="flex items-center justify-between mb-1">
                                        <h4 class="font-extrabold text-xs text-primary group-hover:text-primary-hover transition-colors">Jusbrasil</h4>
                                        <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted opacity-50 group-hover:opacity-100"></i>
                                    </div>
                                    <p class="text-[10px] text-text-secondary leading-snug">Precedentes e jurisprudência vinculante.</p>
                                </div>
                            </a>
                            <a href="https://ia.jusbrasil.com.br/" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3.5 rounded-2xl border border-border hover:border-primary transition flex flex-col justify-between shadow-xs">
                                <div>
                                    <div class="flex items-center justify-between mb-1">
                                        <h4 class="font-extrabold text-xs text-accent transition-colors">Jus IA</h4>
                                        <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted opacity-50 group-hover:opacity-100"></i>
                                    </div>
                                    <p class="text-[10px] text-text-secondary leading-snug">Pesquisa automatizada de teses.</p>
                                </div>
                            </a>
                            <a href="https://minutaia.tjpb.jus.br/login" target="_blank" rel="noopener noreferrer" class="group bg-surface hover:bg-surface-hover p-3.5 rounded-2xl border border-border hover:border-primary transition flex flex-col justify-between shadow-xs">
                                <div>
                                    <div class="flex items-center justify-between mb-1">
                                        <h4 class="font-extrabold text-xs text-text-primary group-hover:text-primary transition-colors">Minuta IA TJPB</h4>
                                        <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted opacity-50 group-hover:opacity-100"></i>
                                    </div>
                                    <p class="text-[10px] text-text-secondary leading-snug">Plataforma oficial de IA do Tribunal.</p>
                                </div>
                            </a>
                        </div>
                    </div>

                    <!-- SEÇÃO: SISTEMAS INSTITUCIONAIS & GESTÃO TJPB -->
                    <div class="space-y-2">
                        <div class="flex items-center gap-2 text-text-secondary text-[11px] font-bold uppercase tracking-wider">
                            <i data-lucide="building" class="w-4 h-4 text-primary"></i>
                            <span>Sistemas Institucionais & Gestão TJPB</span>
                        </div>
                        <div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-4 gap-2">
                            <a href="https://www.tjpb.jus.br/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="globe" class="w-4 h-4 text-primary shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">Portal TJPB</span>
                            </a>
                            <a href="https://pjesg.tjpb.jus.br/pje2g/ng2/dev.seam#/painel-usuario-interno" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="external-link" class="w-4 h-4 text-info shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">PJe 2º Grau</span>
                            </a>
                            <a href="https://sei.tjpb.jus.br/sip/login.php?sigla_orgao_sistema=TJPB&sigla_sistema=SEI&infra_url=L3NlaS8=" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="file-check" class="w-4 h-4 text-success shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">Sistema SEI</span>
                            </a>
                            <a href="https://reports.tjpb.jus.br/saopje/login.jsf" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="database" class="w-4 h-4 text-accent shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">SAO TJPB</span>
                            </a>
                            <a href="https://intranet.tjpb.jus.br/paineis-bi" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="bar-chart-3" class="w-4 h-4 text-info shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">Painel BI</span>
                            </a>
                            <a href="https://www.cnj.jus.br/sistemas/datajud/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="landmark" class="w-4 h-4 text-accent shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">DataJud CNJ</span>
                            </a>
                            <a href="https://qlik-sense6.tjpb.jus.br/qap/single/?appid=2e9f5df1-ba98-46d5-a764-8c3986bd2e1e&sheet=7f4f4bd4-2486-4369-a768-ec8b032177ee&lang=pt-BR&opt=currsel%2Cctxmenu&select=IDOJ,132" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="pie-chart" class="w-4 h-4 text-primary shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">PJE + 2º G</span>
                            </a>
                            <a href="https://portaldousuario.tjpb.jus.br/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary">
                                <i data-lucide="headset" class="w-4 h-4 text-secondary shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">Chamados TI</span>
                            </a>
                            <a href="https://api.whatsapp.com/send?phone=5583993819106" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border transition flex items-center gap-2 text-text-primary col-span-2 sm:col-span-4 lg:col-span-4 justify-center">
                                <i data-lucide="message-square" class="w-4 h-4 text-success shrink-0"></i>
                                <span class="text-[10.5px] font-bold truncate">Suporte WhatsApp DITEC: (83) 99381-9106</span>
                            </a>
                        </div>
                    </div>

                    <!-- MANUAL SINTETIZADO E PRÉ-REQUISITOS GABINETE 24 -->
                    <div class="bg-surface-soft p-4 md:p-5 rounded-2xl border border-border text-xs text-text-secondary space-y-3">
                        <div class="flex items-center gap-2 text-text-primary font-bold">
                            <i data-lucide="book-open" class="w-4 h-4 text-accent"></i>
                            <span>Manual de Operação & Padrões do Gabinete 24</span>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                            <div class="bg-surface p-3 rounded-xl border border-border">
                                <span class="font-bold text-primary block mb-1">1. Formato Arquivo</span>
                                <p class="text-[11px] leading-relaxed">Salvar as minutas em formato <strong>.odt</strong> ou <strong>.docx</strong> (Word XML nativo).</p>
                            </div>
                            <div class="bg-surface p-3 rounded-xl border border-border">
                                <span class="font-bold text-primary block mb-1">2. Nomenclatura Recomendada</span>
                                <p class="text-[11px] leading-relaxed">Sigla do recurso (AC, AI, ED, MS) + CNJ padrão PJe + Sigla do Assessor (ADL, APP, CSA...) + Resumo.</p>
                            </div>
                            <div class="bg-surface p-3 rounded-xl border border-border">
                                <span class="font-bold text-primary block mb-1">3. Estrutura Padrão</span>
                                <p class="text-[11px] leading-relaxed">Cabeçalho (Poder Judiciário, TJPB, Gabinete 24) &rarr; Polos &rarr; Ementa &rarr; Relatório &rarr; Voto e Dispositivo.</p>
                            </div>
                        </div>
                    </div>

                </div>
            </div>

            <!-- PAINEL DE LEITURA E EDIÇÃO ATIVO -->
            <div id="painel-leitura-ativo" class="flex-1 flex flex-col h-full overflow-hidden hidden bg-surface shadow-xs z-10">
                
                <!-- Cabeçalho do Processo em Edição -->
                <div class="bg-surface border-b border-border px-4 py-2 flex items-center justify-between shrink-0 gap-3 relative z-30">
                    
                    <div class="flex items-center gap-3 overflow-hidden min-w-0 flex-1">
                        <!-- Navegador Anterior/Próximo -->
                        <div class="flex items-center gap-0.5 border border-border bg-surface-soft rounded-lg p-0.5 shrink-0">
                            <button onclick="navegarProcessoRelativo(-1)" class="p-1 hover:bg-surface rounded text-text-secondary transition" title="Processo Anterior">
                                <i data-lucide="chevron-left" class="w-4 h-4"></i>
                            </button>
                            <button onclick="navegarProcessoRelativo(1)" class="p-1 hover:bg-surface rounded text-text-secondary transition" title="Próximo Processo">
                                <i data-lucide="chevron-right" class="w-4 h-4"></i>
                            </button>
                        </div>

                        <!-- Botão de Voltar para Mobile -->
                        <button onclick="voltarParaListaMobile()" class="md:hidden p-1.5 rounded-lg bg-surface-soft border border-border text-primary font-bold text-xs flex items-center gap-1">
                            <i data-lucide="arrow-left" class="w-4 h-4"></i>
                        </button>

                        <span id="badge-seq-ativo" class="font-black text-xs bg-surface-soft text-text-primary px-2 py-0.5 rounded-md border border-border">#01</span>

                        <div class="overflow-hidden min-w-0 leading-tight">
                            <div class="flex items-center gap-2 flex-wrap">
                                <h2 onclick="copiarNumeroCNJAtivo()" class="text-xs font-black text-text-primary truncate cursor-pointer hover:text-primary transition-colors flex items-center gap-1 font-mono" title="Clique para copiar o número CNJ">
                                    <span id="title-processo-cnj">0800000-00.2026.8.15.0000</span>
                                    <i data-lucide="copy" class="w-3 h-3 opacity-40 hover:opacity-100"></i>
                                </h2>

                                <!-- Link Direto ao PJe 2º Grau -->
                                <a href="https://pjesg.tjpb.jus.br/pje2g/ng2/dev.seam#/painel-usuario-interno" target="_blank" rel="noopener noreferrer" class="flex items-center gap-1 bg-info/10 hover:bg-info/20 text-info border border-info/30 px-2 py-0.5 rounded text-[10.5px] font-bold transition shadow-2xs" title="Abrir no PJe 2º Grau">
                                    <i data-lucide="external-link" class="w-3 h-3"></i> PJe 2G
                                </a>

                                <span id="badge-classe-ativo" class="text-[10px] font-bold bg-surface-soft text-text-secondary px-1.5 py-0.5 rounded border border-border">Classe</span>
                                
                                <!-- Seletor de Assessor Ativo -->
                                <div class="flex items-center gap-1 bg-surface-soft border border-border px-1.5 py-0.5 rounded text-[10.5px]">
                                    <span class="text-text-muted font-bold">Ass:</span>
                                    <select id="select-assessor-ativo" onchange="aoMudarAssessorAtivo(this.value)" class="bg-transparent font-black text-primary outline-none cursor-pointer">
                                        <!-- Preenchido dinamicamente -->
                                    </select>
                                </div>

                                <span id="badge-status-ativo" class="px-2 py-0.5 text-[9.5px] font-bold rounded uppercase tracking-wider border">PENDENTE</span>
                            </div>
                            <p id="subtitle-processo-partes" class="text-[11px] font-medium text-text-secondary truncate mt-0.5">Relator: Desembargador | Partes</p>
                        </div>
                    </div>

                    <!-- Botões de Ação do Voto -->
                    <div class="flex items-center gap-2 shrink-0">
                        <button onclick="restaurarVotoOriginalAtivo()" class="px-2.5 py-1.5 bg-surface-soft hover:bg-surface-hover text-text-secondary font-semibold rounded-lg text-xs flex items-center gap-1.5 transition border border-border" title="Restaurar arquivo original">
                            <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i>
                            <span class="hidden md:inline">Restaurar</span>
                        </button>

                        <!-- Dropdown de Exportação -->
                        <div class="relative group">
                            <button class="px-3 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg text-xs flex items-center gap-1.5 transition shadow-xs">
                                <i data-lucide="download" class="w-3.5 h-3.5"></i>
                                <span>Exportar</span>
                                <i data-lucide="chevron-down" class="w-3 h-3 opacity-60"></i>
                            </button>
                            <div class="absolute right-0 top-full mt-1 w-60 bg-surface rounded-xl shadow-xl border border-border opacity-0 invisible group-hover:opacity-100 group-hover:visible transition-all z-50 overflow-hidden">
                                <div class="p-1.5 text-xs font-medium space-y-0.5">
                                    <button onclick="exportarVotoCorrigidoAtivo('docx')" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left text-text-primary transition">
                                        <i data-lucide="file-text" class="w-4 h-4 text-primary"></i>
                                        <div><div class="font-bold">Word (.docx)</div><div class="text-[9.5px] text-text-muted">Office Open XML</div></div>
                                    </button>
                                    <button onclick="exportarVotoCorrigidoAtivo('odt')" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left text-text-primary transition">
                                        <i data-lucide="file-code" class="w-4 h-4 text-secondary"></i>
                                        <div><div class="font-bold">LibreOffice (.odt)</div><div class="text-[9.5px] text-text-muted">OpenDocument Text</div></div>
                                    </button>
                                    <button onclick="exportarVotoCorrigidoAtivo('doc')" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left text-text-primary transition">
                                        <i data-lucide="file-edit" class="w-4 h-4 text-accent"></i>
                                        <div><div class="font-bold">Word 97-2003 (.doc)</div><div class="text-[9.5px] text-text-muted">Compatível legado</div></div>
                                    </button>
                                    <button onclick="exportarVotoCorrigidoAtivo('pdf')" class="w-full flex items-center gap-2 p-2 hover:bg-surface-soft rounded-lg text-left text-text-primary transition border-t border-divider">
                                        <i data-lucide="printer" class="w-4 h-4 text-danger"></i>
                                        <div><div class="font-bold">Imprimir PDF A4</div><div class="text-[9.5px] text-text-muted">Direto do navegador</div></div>
                                    </button>
                                </div>
                            </div>
                        </div>

                    </div>
                </div>

                <!-- Barra de Ferramentas WYSIWYG do Editor -->
                <div id="editor-toolbar" class="bg-surface border-b border-border px-4 py-1.5 flex flex-wrap items-center gap-1.5 shrink-0 z-20 text-xs overflow-x-auto scrollbar-custom">
                    
                    <div class="flex items-center gap-0.5 bg-surface-soft border border-border rounded-lg p-0.5">
                        <button onclick="executarDesfazer()" class="p-1 hover:bg-surface rounded text-text-secondary" title="Desfazer (Ctrl+Z)"><i data-lucide="undo" class="w-3.5 h-3.5"></i></button>
                        <button onclick="executarRefazer()" class="p-1 hover:bg-surface rounded text-text-secondary" title="Refazer (Ctrl+Y)"><i data-lucide="redo" class="w-3.5 h-3.5"></i></button>
                    </div>

                    <div class="w-px h-5 bg-divider mx-0.5"></div>

                    <select onchange="executarComandoEditorFocado('fontName', this.value)" class="bg-surface-soft border border-border text-xs p-1 rounded-lg font-medium text-text-primary outline-none">
                        <option value="Arial" selected>Arial</option>
                        <option value="Times New Roman">Times New Roman</option>
                    </select>
                    <select onchange="executarComandoEditorFocado('fontSize', this.value)" class="bg-surface-soft border border-border text-xs p-1 rounded-lg font-medium text-text-primary outline-none">
                        <option value="3" selected>12pt</option>
                        <option value="4">14pt</option>
                    </select>

                    <div class="w-px h-5 bg-divider mx-0.5"></div>

                    <div class="flex items-center gap-0.5 bg-surface-soft border border-border rounded-lg p-0.5">
                        <button onclick="executarComandoEditor('bold')" class="w-6 h-6 flex items-center justify-center hover:bg-surface rounded font-black text-xs text-text-primary" title="Negrito (Ctrl+B)">B</button>
                        <button onclick="executarComandoEditor('italic')" class="w-6 h-6 flex items-center justify-center hover:bg-surface rounded italic font-serif text-xs text-text-primary" title="Itálico (Ctrl+I)">I</button>
                        <button onclick="executarComandoEditor('underline')" class="w-6 h-6 flex items-center justify-center hover:bg-surface rounded underline text-xs text-text-primary" title="Sublinhado (Ctrl+U)">U</button>
                    </div>

                    <div class="w-px h-5 bg-divider mx-0.5"></div>

                    <div class="flex items-center gap-0.5 bg-surface-soft border border-border rounded-lg p-0.5">
                        <button onclick="executarComandoEditor('justifyLeft')" class="p-1 hover:bg-surface rounded text-text-secondary" title="Alinhar à Esquerda"><i data-lucide="align-left" class="w-3.5 h-3.5"></i></button>
                        <button onclick="executarComandoEditor('justifyCenter')" class="p-1 hover:bg-surface rounded text-text-secondary" title="Centralizar"><i data-lucide="align-center" class="w-3.5 h-3.5"></i></button>
                        <button onclick="executarComandoEditor('justifyFull')" class="p-1 hover:bg-surface rounded text-text-secondary" title="Justificar"><i data-lucide="align-justify" class="w-3.5 h-3.5"></i></button>
                    </div>

                    <div class="flex items-center gap-1 ml-0.5">
                        <select onchange="aplicarEstiloBlocoFocado('lineHeight', this.value)" class="bg-surface-soft border border-border text-xs p-1 rounded-lg font-medium text-text-primary outline-none" title="Espaçamento Entrelinhas">
                            <option value="1.0">1.0</option>
                            <option value="1.5" selected>1.5</option>
                            <option value="2.0">2.0</option>
                        </select>
                    </div>

                    <div class="w-px h-5 bg-divider mx-0.5"></div>

                    <!-- Botão Citação 4cm -->
                    <button onclick="toggleCitacaoLonga()" class="px-2.5 py-1 bg-surface-soft hover:bg-surface-hover text-primary border border-border rounded-lg text-xs font-bold flex items-center gap-1 transition" title="Formatar Citação Longa (4cm, 10.5pt, entrelinhas 1.2)">
                        <i data-lucide="quote" class="w-3 h-3 text-primary"></i> Citação (4cm)
                    </button>

                    <!-- Modo Correção (Letras Vermelhas) -->
                    <button id="btn-modo-revisao" onclick="toggleModoRevisao()" class="px-2.5 py-1 bg-surface-soft text-text-secondary hover:bg-surface-hover rounded-lg text-xs font-bold flex items-center gap-1.5 transition border border-border" title="Ativar Letras Vermelhas de Correção">
                        <span class="w-2 h-2 rounded-full bg-danger"></span> Correção
                    </button>

                    <div class="ml-auto flex items-center gap-2">
                        <button onclick="salvarConteudoEditadoDireto()" class="px-3.5 py-1 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg text-xs flex items-center gap-1.5 shadow-xs transition">
                            <i data-lucide="save" class="w-3.5 h-3.5 text-accent"></i> Salvar Voto
                        </button>
                    </div>
                </div>

                <!-- Folha A4 do Editor com Régua Forense -->
                <div class="flex-1 overflow-y-auto p-4 md:p-8 flex flex-col items-center bg-bg scrollbar-custom relative">
                    <div class="max-w-4xl w-full flex flex-col items-stretch">
                        
                        <!-- Régua Horizontal -->
                        <div class="sticky top-0 z-20 regua-container rounded-t-lg shadow-2xs hidden sm:flex border-x border-t border-border">
                            <div id="regua-escala-principal" class="relative w-full h-full">
                                <div id="marcador-recuo-1a-principal" class="marcador-primeira-linha" style="left: 7.81%;" title="Recuo 1ª Linha">
                                    <div class="icone-triangulo-baixo"></div>
                                </div>
                                <div id="marcador-recuo-paragrafo-principal" class="marcador-paragrafo-inteiro" style="left: 0%;" title="Margem do Parágrafo">
                                    <div class="icone-triangulo-cima"></div>
                                    <div class="corpo-base-quadrada"></div>
                                </div>
                            </div>
                        </div>

                        <!-- Corpo Editável do Voto (Fundo Branco Imutável A4) -->
                        <div id="editor-conteudo-voto" contenteditable="true" spellcheck="true"
                             class="editor-voto transition-all"
                             onbeforeinput="capturarAntesDeDigitar(event)"
                             oninput="lidarComEdicaoDeTexto(this)"
                             onpaste="lidarComColar(event)"
                             onkeydown="interceptarTeclasEditor(event)"
                             onclick="sincronizarRecuoAoClicar(this)">
                        </div>

                    </div>
                </div>

            </div>

        </main>

        <!-- GAVETA LATERAL DO ASSISTENTE IA -->
        <div id="ai-drawer" class="absolute right-0 top-0 bottom-0 w-full sm:w-[420px] bg-surface shadow-2xl z-[60] transform translate-x-full transition-transform duration-300 flex flex-col border-l border-border">
            <div class="p-4 border-b border-border flex items-center justify-between bg-surface-soft">
                <div class="flex items-center gap-2">
                    <div class="bg-surface p-1.5 rounded-lg border border-border shadow-xs">
                        <i data-lucide="bot" class="w-5 h-5 text-accent"></i>
                    </div>
                    <div>
                        <h3 class="font-black text-text-primary text-sm flex items-center gap-1">Assistente Local IA</h3>
                        <p class="text-[10px] text-text-muted">Conferência & Análise Processual</p>
                    </div>
                </div>
                <button onclick="toggleAiDrawer()" class="p-1.5 hover:bg-surface-hover rounded-md text-text-muted transition">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div id="ai-chat-history" class="flex-1 overflow-y-auto p-4 space-y-3 bg-surface scrollbar-custom text-xs">
                <div class="bg-surface-soft p-3 rounded-xl border border-border leading-relaxed text-text-primary">
                    Olá! Sou seu assistente de sessão. Posso resumir a minuta na tela, checar a jurisprudência citada ou tabular os resultados dos recursos. Como posso ajudar?
                </div>
            </div>
            <div class="p-3 border-t border-border bg-surface-soft flex items-end gap-2 shrink-0">
                <textarea id="ai-user-input" rows="1" class="flex-1 bg-surface border border-border rounded-xl px-3 py-2 text-xs text-text-primary focus:outline-none focus:border-primary resize-none max-h-32 scrollbar-custom" placeholder="Faça uma pergunta sobre o voto..."></textarea>
                <button onclick="enviarMensagemGemini()" class="bg-primary hover:bg-primary-hover text-white p-2 rounded-xl shadow-xs transition shrink-0">
                    <i data-lucide="send" class="w-4 h-4"></i>
                </button>
            </div>
        </div>

    </div>

    <!-- MODAL CENTRAL DE SISTEMAS, WORKSPACE & IAS -->
    <div id="modal-central-links" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-3xl p-6 max-w-2xl w-full shadow-2xl border border-border flex flex-col max-h-[90vh] text-xs text-text-primary space-y-4">
            
            <div class="flex items-center justify-between border-b border-border pb-3">
                <div class="flex items-center gap-2.5">
                    <div class="w-8 h-8 rounded-xl bg-primary-soft text-primary flex items-center justify-center shadow-xs">
                        <i data-lucide="layout-grid" class="w-4 h-4 text-accent"></i>
                    </div>
                    <div>
                        <h3 class="font-black text-text-primary text-sm">Central de Sistemas & Acessos Institucionais</h3>
                        <p class="text-[10.5px] text-text-muted">Acesso rápido aos sistemas do TJPB, Google Workspace e IAs jurídicas</p>
                    </div>
                </div>
                <button onclick="fecharModal('modal-central-links')" class="p-1.5 hover:bg-surface-hover rounded-lg text-text-muted transition">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>

            <div class="flex-1 overflow-y-auto space-y-4 pr-1 scrollbar-custom">
                <!-- Google Workspace -->
                <div class="space-y-2">
                    <h4 class="font-bold text-[11px] uppercase tracking-wider text-text-secondary flex items-center gap-1.5">
                        <i data-lucide="cloud" class="w-3.5 h-3.5 text-info"></i> Google Workspace Institucional
                    </h4>
                    <div class="grid grid-cols-2 sm:grid-cols-5 gap-2">
                        <a href="https://chat.google.com/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex flex-col items-center gap-1.5 text-center transition">
                            <i data-lucide="message-circle" class="w-5 h-5 text-success"></i>
                            <span class="font-bold text-[11px]">Chat</span>
                        </a>
                        <a href="https://docs.google.com/document/u/0/?pli=1&tgif=d" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex flex-col items-center gap-1.5 text-center transition">
                            <i data-lucide="file-text" class="w-5 h-5 text-info"></i>
                            <span class="font-bold text-[11px]">Docs</span>
                        </a>
                        <a href="https://drive.google.com/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex flex-col items-center gap-1.5 text-center transition">
                            <i data-lucide="hard-drive" class="w-5 h-5 text-accent"></i>
                            <span class="font-bold text-[11px]">Drive</span>
                        </a>
                        <a href="https://mail.google.com/mail/u/0/?service=mail&flowName=GlifWebSignIn&flowEntry=AccountChooser&ec=asw-gmail-globalnav-signin#inbox" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex flex-col items-center gap-1.5 text-center transition">
                            <i data-lucide="mail" class="w-5 h-5 text-danger"></i>
                            <span class="font-bold text-[11px]">Gmail</span>
                        </a>
                        <a href="https://meet.google.com/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex flex-col items-center gap-1.5 text-center transition col-span-2 sm:col-span-1">
                            <i data-lucide="video" class="w-5 h-5 text-primary"></i>
                            <span class="font-bold text-[11px]">Meet</span>
                        </a>
                    </div>
                </div>

                <!-- Inteligência Artificial & Pesquisa -->
                <div class="space-y-2">
                    <h4 class="font-bold text-[11px] uppercase tracking-wider text-text-secondary flex items-center gap-1.5">
                        <i data-lucide="cpu" class="w-3.5 h-3.5 text-accent"></i> Inteligência Artificial & Pesquisa
                    </h4>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                        <a href="https://gemini.google.com/app" target="_blank" rel="noopener noreferrer" class="p-3 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center justify-between transition group">
                            <div>
                                <div class="font-bold text-xs text-[#6366f1]">Google Gemini</div>
                                <div class="text-[10px] text-text-muted">Minutas, sínteses e análises processuais</div>
                            </div>
                            <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted group-hover:text-primary"></i>
                        </a>
                        <a href="https://www.jusbrasil.com.br/jurisprudencia/" target="_blank" rel="noopener noreferrer" class="p-3 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center justify-between transition group">
                            <div>
                                <div class="font-bold text-xs text-primary">Jusbrasil Jurisprudência</div>
                                <div class="text-[10px] text-text-muted">Precedentes e jurisprudência vinculante</div>
                            </div>
                            <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted group-hover:text-primary"></i>
                        </a>
                        <a href="https://ia.jusbrasil.com.br/" target="_blank" rel="noopener noreferrer" class="p-3 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center justify-between transition group">
                            <div>
                                <div class="font-bold text-xs text-accent">Jus IA</div>
                                <div class="text-[10px] text-text-muted">Pesquisa automatizada de teses jurídicas</div>
                            </div>
                            <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted group-hover:text-primary"></i>
                        </a>
                        <a href="https://minutaia.tjpb.jus.br/login" target="_blank" rel="noopener noreferrer" class="p-3 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center justify-between transition group">
                            <div>
                                <div class="font-bold text-xs text-text-primary">Minuta IA TJPB</div>
                                <div class="text-[10px] text-text-muted">Plataforma oficial de IA do Tribunal</div>
                            </div>
                            <i data-lucide="external-link" class="w-3.5 h-3.5 text-text-muted group-hover:text-primary"></i>
                        </a>
                    </div>
                </div>

                <!-- Sistemas do TJPB -->
                <div class="space-y-2">
                    <h4 class="font-bold text-[11px] uppercase tracking-wider text-text-secondary flex items-center gap-1.5">
                        <i data-lucide="building" class="w-3.5 h-3.5 text-primary"></i> Sistemas do TJPB, CNJ & Suporte
                    </h4>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2">
                        <a href="https://www.tjpb.jus.br/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="globe" class="w-4 h-4 text-primary shrink-0"></i>
                            <div><div class="font-bold text-xs">Portal TJPB</div><div class="text-[9.5px] text-text-muted">Site oficial</div></div>
                        </a>
                        <a href="https://pjesg.tjpb.jus.br/pje2g/ng2/dev.seam#/painel-usuario-interno" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="external-link" class="w-4 h-4 text-info shrink-0"></i>
                            <div><div class="font-bold text-xs">PJe 2º Grau</div><div class="text-[9.5px] text-text-muted">Painel interno</div></div>
                        </a>
                        <a href="https://sei.tjpb.jus.br/sip/login.php?sigla_orgao_sistema=TJPB&sigla_sistema=SEI&infra_url=L3NlaS8=" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="file-check" class="w-4 h-4 text-success shrink-0"></i>
                            <div><div class="font-bold text-xs">Sistema SEI</div><div class="text-[9.5px] text-text-muted">Processo Adm.</div></div>
                        </a>
                        <a href="https://reports.tjpb.jus.br/saopje/login.jsf" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="database" class="w-4 h-4 text-accent shrink-0"></i>
                            <div><div class="font-bold text-xs">Sistema SAO</div><div class="text-[9.5px] text-text-muted">Relatórios e dados</div></div>
                        </a>
                        <a href="https://intranet.tjpb.jus.br/paineis-bi" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="bar-chart-3" class="w-4 h-4 text-info shrink-0"></i>
                            <div><div class="font-bold text-xs">Painel BI</div><div class="text-[9.5px] text-text-muted">BI Institucional</div></div>
                        </a>
                        <a href="https://www.cnj.jus.br/sistemas/datajud/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="landmark" class="w-4 h-4 text-accent shrink-0"></i>
                            <div><div class="font-bold text-xs">DataJud</div><div class="text-[9.5px] text-text-muted">CNJ Nacional</div></div>
                        </a>
                        <a href="https://qlik-sense6.tjpb.jus.br/qap/single/?appid=2e9f5df1-ba98-46d5-a764-8c3986bd2e1e&sheet=7f4f4bd4-2486-4369-a768-ec8b032177ee&lang=pt-BR&opt=currsel%2Cctxmenu&select=IDOJ,132" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="pie-chart" class="w-4 h-4 text-primary shrink-0"></i>
                            <div><div class="font-bold text-xs">PJE + 2º G</div><div class="text-[9.5px] text-text-muted">Painel Qlik Sense</div></div>
                        </a>
                        <a href="https://portaldousuario.tjpb.jus.br/" target="_blank" rel="noopener noreferrer" class="p-2.5 bg-surface-soft hover:bg-surface-hover rounded-xl border border-border flex items-center gap-2 transition">
                            <i data-lucide="headset" class="w-4 h-4 text-secondary shrink-0"></i>
                            <div><div class="font-bold text-xs">Chamados TI</div><div class="text-[9.5px] text-text-muted">Portal do Usuário</div></div>
                        </a>
                    </div>
                </div>

            </div>

            <div class="flex items-center justify-between pt-3 border-t border-border">
                <a href="https://api.whatsapp.com/send?phone=5583993819106" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-1.5 text-success font-bold text-xs hover:underline">
                    <i data-lucide="message-square" class="w-3.5 h-3.5"></i> Suporte WhatsApp DITEC: (83) 99381-9106
                </a>
                <button onclick="fecharModal('modal-central-links')" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg transition">
                    Fechar
                </button>
            </div>
        </div>
    </div>

    <!-- MODAL DO MOTOR DE APRENDIZADO ADAPTATIVO -->
    <div id="modal-aprendizado-adaptativo" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-3xl p-6 max-w-xl w-full shadow-2xl border border-border space-y-4 text-xs text-text-primary max-h-[85vh] flex flex-col">
            <div class="flex items-center justify-between border-b border-border pb-3">
                <div class="flex items-center gap-2">
                    <div class="p-1.5 bg-accent-soft text-accent rounded-lg">
                        <i data-lucide="brain-circuit" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="font-black text-sm text-text-primary">Motor de Aprendizado Adaptativo</h3>
                        <p class="text-[10px] text-text-muted">Comportamento, Frequência e Previsão Contextual</p>
                    </div>
                </div>
                <button onclick="fecharModal('modal-aprendizado-adaptativo')" class="text-text-muted hover:text-text-primary p-1">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>

            <div class="flex-1 overflow-y-auto space-y-4 scrollbar-custom pr-1">
                <!-- Seção: O Que o Sistema Aprendeu -->
                <div class="bg-surface-soft p-3.5 rounded-2xl border border-border space-y-2">
                    <span class="font-bold text-[11px] text-accent uppercase tracking-wider flex items-center gap-1">
                        <i data-lucide="sparkles" class="w-3.5 h-3.5"></i> O Que o Sistema Aprendeu
                    </span>
                    <div id="status-aprendizado-conteudo" class="space-y-1 text-[11px] text-text-secondary leading-relaxed">
                        <!-- Renderizado via JS -->
                    </div>
                </div>

                <!-- Controles do Usuário -->
                <div class="space-y-2.5">
                    <span class="font-bold text-[11px] uppercase tracking-wider text-text-secondary">Configurações & Controles</span>
                    <div class="space-y-2">
                        <label class="flex items-center justify-between p-2.5 rounded-xl border border-border bg-surface-soft cursor-pointer">
                            <div>
                                <div class="font-bold text-xs text-text-primary">Aprendizado Adaptativo Ativo</div>
                                <div class="text-[10px] text-text-muted">Observar interações e calcular scores com decaimento temporal</div>
                            </div>
                            <input type="checkbox" id="chk-adaptativo-ativo" onchange="AdaptiveEngine.toggleAtivo(this.checked)" class="w-4 h-4 rounded text-primary">
                        </label>
                        <label class="flex items-center justify-between p-2.5 rounded-xl border border-border bg-surface-soft cursor-pointer">
                            <div>
                                <div class="font-bold text-xs text-text-primary">Exibir Atalhos Inteligentes</div>
                                <div class="text-[10px] text-text-muted">Priorizar funções de maior recorrência na barra lateral</div>
                            </div>
                            <input type="checkbox" id="chk-adaptativo-atalhos" onchange="AdaptiveEngine.toggleAtalhos(this.checked)" class="w-4 h-4 rounded text-primary">
                        </label>
                    </div>
                </div>
            </div>

            <div class="flex items-center justify-between pt-3 border-t border-border mt-2">
                <button onclick="AdaptiveEngine.resetarAprendizado()" class="text-danger hover:underline text-[11px] font-bold">
                    Redefinir Dados de Aprendizado
                </button>
                <div class="flex items-center gap-2">
                    <button onclick="AdaptiveEngine.exportarPerfilJson()" class="px-3 py-1.5 bg-surface-soft hover:bg-surface-hover text-text-primary font-bold rounded-lg border border-border text-xs">
                        Exportar JSON
                    </button>
                    <button onclick="fecharModal('modal-aprendizado-adaptativo')" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg text-xs">
                        Concluído
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL EDITAR PROCESSO & POLOS -->
    <div id="modal-editar-processo" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-2xl p-5 max-w-md w-full shadow-2xl border border-border space-y-3 text-xs text-text-primary">
            <div class="flex items-center justify-between border-b border-border pb-2">
                <h3 class="font-black text-text-primary flex items-center gap-1.5">
                    <i data-lucide="edit" class="w-4 h-4 text-primary"></i> Editar Processo & Assessor
                </h3>
                <button onclick="fecharModal('modal-editar-processo')" class="text-text-muted hover:text-text-primary">
                    <i data-lucide="x" class="w-4 h-4"></i>
                </button>
            </div>
            <div class="space-y-2">
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Número CNJ:</label>
                        <input type="text" id="edit-cnj" class="w-full p-1.5 rounded-lg border border-border font-bold bg-surface-soft font-mono">
                    </div>
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Classe:</label>
                        <input type="text" id="edit-classe" class="w-full p-1.5 rounded-lg border border-border font-medium bg-surface">
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Polo Ativo:</label>
                        <input type="text" id="edit-tipo-ativo" class="w-full p-1 rounded-md border border-border font-bold text-primary bg-primary-soft mb-1">
                        <textarea id="edit-recorrente" rows="2" class="w-full p-1.5 rounded-lg border border-border text-xs resize-none bg-surface" placeholder="Nome da Parte Ativa"></textarea>
                    </div>
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Polo Passivo:</label>
                        <input type="text" id="edit-tipo-passivo" class="w-full p-1 rounded-md border border-border font-bold text-text-secondary bg-surface-soft mb-1">
                        <textarea id="edit-recorrido" rows="2" class="w-full p-1.5 rounded-lg border border-border text-xs resize-none bg-surface" placeholder="Nome da Parte Passiva"></textarea>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Relator(a):</label>
                        <select id="edit-relator" class="w-full p-1.5 rounded-lg border border-border font-bold bg-surface"></select>
                    </div>
                    <div>
                        <label class="block font-bold text-text-secondary mb-0.5">Assessor(a):</label>
                        <select id="edit-assessor" class="w-full p-1.5 rounded-lg border border-border font-bold bg-surface"></select>
                    </div>
                </div>
            </div>
            <div class="flex items-center justify-end gap-2 pt-2 border-t border-border mt-2">
                <button onclick="fecharModal('modal-editar-processo')" class="px-3 py-1.5 bg-surface-soft text-text-secondary font-bold rounded-lg">Cancelar</button>
                <button onclick="salvarModalEditarProcesso()" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg shadow-xs">Salvar</button>
            </div>
        </div>
    </div>

    <!-- MODAL CONFIG ORDEM (# INÍCIO) -->
    <div id="modal-config-ordem" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-2xl p-5 max-w-sm w-full shadow-2xl border border-border space-y-3 text-xs text-text-primary">
            <div class="flex items-center justify-between border-b border-border pb-2">
                <h3 class="font-black text-text-primary flex items-center gap-1.5"><i data-lucide="hash" class="w-4 h-4 text-accent"></i> Sequência da Sessão</h3>
                <button onclick="fecharModal('modal-config-ordem')" class="text-text-muted hover:text-text-primary"><i data-lucide="x" class="w-4 h-4"></i></button>
            </div>
            <div class="space-y-2">
                <label class="block font-bold text-text-secondary">Número Inicial da Pauta (# Início):</label>
                <input type="number" id="input-num-inicio-sessao" value="35" min="1" max="500" class="w-full text-sm p-2 rounded-lg border border-border font-bold bg-surface-soft">
                <p class="text-[10px] text-text-muted">A numeração no modo Sessão começará a partir deste número. A pauta comporta até 500 processos.</p>
            </div>
            <div class="flex items-center justify-end gap-2 pt-2 border-t border-border">
                <button onclick="fecharModal('modal-config-ordem')" class="px-3 py-1.5 bg-surface-soft text-text-secondary font-bold rounded-lg">Cancelar</button>
                <button onclick="salvarModalConfigOrdem()" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg shadow-xs">Aplicar</button>
            </div>
        </div>
    </div>

    <!-- MODAL NOVA PAUTA -->
    <div id="modal-nova-pauta" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-2xl p-5 max-w-sm w-full shadow-2xl border border-border space-y-3 text-xs text-text-primary">
            <div class="flex items-center justify-between border-b border-border pb-2">
                <h3 class="font-black text-text-primary flex items-center gap-1.5"><i data-lucide="calendar-plus" class="w-4 h-4 text-primary"></i> Nova Pauta</h3>
                <button onclick="fecharModal('modal-nova-pauta')" class="text-text-muted hover:text-text-primary"><i data-lucide="x" class="w-4 h-4"></i></button>
            </div>
            <div>
                <label class="block font-bold text-text-secondary mb-0.5">Data da Nova Pauta:</label>
                <input type="date" id="input-nova-pauta-data" class="w-full p-2 rounded-lg border border-border font-bold bg-surface">
            </div>
            <div class="flex items-center justify-end gap-2 pt-2 border-t border-border">
                <button onclick="fecharModal('modal-nova-pauta')" class="px-3 py-1.5 bg-surface-soft text-text-secondary font-bold rounded-lg">Cancelar</button>
                <button onclick="confirmarCriarNovaPauta()" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg shadow-xs">Criar</button>
            </div>
        </div>
    </div>

    <!-- MODAL ATA DE JULGAMENTO -->
    <div id="modal-ata-julgamento" class="fixed inset-0 bg-black/60 backdrop-blur-xs z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-surface rounded-2xl p-5 max-w-2xl w-full shadow-2xl border border-border flex flex-col max-h-[85vh] text-xs text-text-primary">
            <div class="flex items-center justify-between border-b border-border pb-2 mb-3">
                <h3 class="font-black text-text-primary flex items-center gap-1.5"><i data-lucide="file-signature" class="w-4 h-4 text-primary"></i> Ata da Sessão</h3>
                <button onclick="fecharModal('modal-ata-julgamento')" class="text-text-muted hover:text-text-primary"><i data-lucide="x" class="w-4 h-4"></i></button>
            </div>
            <div id="conteudo-ata-texto" class="flex-1 overflow-y-auto font-mono text-[11px] p-3 bg-surface-soft border border-border rounded-xl whitespace-pre-wrap select-text leading-relaxed"></div>
            <div class="flex items-center justify-between pt-3 border-t border-border mt-3">
                <span class="text-[10px] text-text-muted">Formatado para colar no Sistema SEI</span>
                <button onclick="copiarAtaParaClipboard()" class="px-4 py-1.5 bg-primary hover:bg-primary-hover text-white font-bold rounded-lg flex items-center gap-1">
                    <i data-lucide="copy" class="w-3.5 h-3.5"></i> Copiar Ata
                </button>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICAÇÕES -->
    <div id="toast-container" class="fixed bottom-6 right-6 z-[110] flex flex-col gap-2 pointer-events-none"></div>

    <script>
        /* ==========================================================================
           1. ESTADO GLOBAL & CONSTANTES FORENSES DO TJPB
           ========================================================================== */
        const STORAGE_KEY = 'appState_TJPB_SEV_2026';
        const GEMINI_KEY_STORAGE = 'tjpb_gemini_api_key';

        const MAGISTRADOS_TJPB = {
            "Tribunal Pleno": ["Todos os Desembargadores do TJPB"],
            "Órgão Especial": ["Des. Presidente do TJPB", "Des. Vice-Presidente", "Des. Corregedor-Geral"],
            "Seção Especializada Cível": ["Todos os Desembargadores da Seção Cível"],
            "1ª Câmara Especializada Cível": ["Des. José Ricardo Porto", "Des. Maria de Fátima M. B. C. Maranhão", "Des. Onaldo Rocha de Queiroga", "Des. Francisco Seraphico F. Nóbrega Filho - Presidente", "Dr. Vandemberg de Freitas Rocha"],
            "2ª Câmara Especializada Cível": ["Des. Agamenilde Dias Arruda Vieira Dantas", "Des. Aluízio Bezerra Filho", "Des. Carlos Eduardo Leite Lisboa - Presidente", "Des. José Guedes Cavalcanti Neto", "Des. Lilian Frassinetti Correia Cananéa"],
            "3ª Câmara Especializada Cível": ["Des. Túlia Gomes de Souza Neves", "Des. Wolfram da Cunha Ramos - Presidente", "Dr. Inácio Jario Queiroz de Albuquerque - Juiz Subst.", "Dr. Manuel Maria Antunes de Melo - Juiz Convocado", "Des. Miguel de Britto Lyra Filho"],
            "4ª Câmara Especializada Cível": ["Des. Oswaldo Trigueiro do Valle Filho", "Des. Abraham Lincoln da Cunha Ramos", "Des. Anna Carla Lopes C. L. Freitas - Presidente", "Des. Horácio Ferreira de Melo Júnior", "Dr. Carlos Antônio Sarmento"],
            "Câmara Especializada Criminal": ["Des. Ricardo Vital de Almeida", "Des. Joás de Brito Pereira Filho", "Des. Márcio Murilo da Cunha Ramos", "Des. Saulo Henriques de Sá e Benevides", "Des. João Benedito da Silva", "Des. Carlos Martins Beltrão Filho - Presidente"]
        };

        const ASSESSORES_GABINETE = [
            { sigla: "ADL", nome: "Antônio" },
            { sigla: "APP", nome: "Altamir" },
            { sigla: "CSA", nome: "Cleberson" },
            { sigla: "JJRJ", nome: "Júnior" },
            { sigla: "RLC", nome: "Rodrigo" },
            { sigla: "RAFM", nome: "Robson" },
            { sigla: "GAC", nome: "George" },
            { sigla: "MPF", nome: "Marcelo" },
            { sigla: "WAL", nome: "Waldir" },
            { sigla: "SEL", nome: "Selene" }
        ];

        let appState = {
            unidadeAtiva: "3ª Câmara Especializada Cível",
            relatorAtivo: "Des. Túlia Gomes de Souza Neves",
            sessaoData: new Date().toISOString().split('T')[0],
            sessaoModalidade: "Videoconferência",
            numInicioSessao: 35,
            processos: [],
            processoSelecionadoId: null,
            filtroPauta: 'Todos',
            modoOrdem: 'sessao',
            recuo1aLinhaCm: 1.25,
            recuoParagrafoCm: 0.00,
            assessoresCustom: []
        };

        let debounceTimeout = null;
        let editorUndoStack = [];
        let editorRedoStack = [];
        let editorSavedRange = null;
        let isModoRevisao = false;

        /* ==========================================================================
           2. MOTOR DE APRENDIZADO ADAPTATIVO (Adaptive Learning Engine)
           ========================================================================== */
        const AdaptiveEngine = {
            storageKey: 'tjpb_sev_adaptive_profile',
            profile: {
                ativo: true,
                atalhosAtivos: true,
                eventos: [],
                frequencias: {},
                sequencias: {},
                rejeicoes: {},
                correcoes: [],
                preferencias: {}
            },

            init() {
                try {
                    const salvo = localStorage.getItem(this.storageKey);
                    if (salvo) {
                        const parsed = JSON.parse(salvo);
                        Object.assign(this.profile, parsed);
                    }
                } catch(e) {}
                this.renderSmartShortcuts();
            },

            salvar() {
                try {
                    if (this.profile.eventos.length > 300) {
                        this.profile.eventos = this.profile.eventos.slice(-200);
                    }
                    localStorage.setItem(this.storageKey, JSON.stringify(this.profile));
                } catch(e) {}
            },

            recordAction(actionName, context = {}) {
                if (!this.profile.ativo) return;
                const now = Date.now();
                this.profile.eventos.push({ action: actionName, timestamp: now, context });
                this.profile.frequencias[actionName] = (this.profile.frequencias[actionName] || 0) + 1;

                const ultimosEventos = this.profile.eventos.slice(-2);
                if (ultimosEventos.length === 2) {
                    const prevAction = ultimosEventos[0].action;
                    if (!this.profile.sequencias[prevAction]) this.profile.sequencias[prevAction] = {};
                    this.profile.sequencias[prevAction][actionName] = (this.profile.sequencias[prevAction][actionName] || 0) + 1;
                }

                this.salvar();
                this.renderSmartShortcuts();
                this.evaluateNextAction(actionName);
            },

            recordCorrection(processoId, sugestaoOriginal, valorCorrigido) {
                if (!this.profile.ativo) return;
                this.profile.correcoes.push({
                    processoId,
                    sugestaoOriginal,
                    valorCorrigido,
                    timestamp: Date.now()
                });
                this.salvar();
            },

            evaluateNextAction(lastAction) {
                if (!this.profile.ativo) return;
                const seq = this.profile.sequencias[lastAction];
                if (!seq) {
                    document.getElementById('faixa-sugestao-adaptativa')?.classList.add('hidden');
                    return;
                }

                let bestNext = null;
                let maxCount = 0;
                let total = 0;
                for (let k in seq) {
                    total += seq[k];
                    if (seq[k] > maxCount) {
                        maxCount = seq[k];
                        bestNext = k;
                    }
                }

                const confianca = total > 0 ? (maxCount / total) : 0;
                if (maxCount >= 3 && confianca >= 0.6) {
                    const rejeitado = (this.profile.rejeicoes[bestNext] || 0) >= 3;
                    if (!rejeitado) {
                        this.showNextActionSuggestion(bestNext, Math.round(confianca * 100));
                    }
                } else {
                    document.getElementById('faixa-sugestao-adaptativa')?.classList.add('hidden');
                }
            },

            showNextActionSuggestion(actionName, pct) {
                const faixa = document.getElementById('faixa-sugestao-adaptativa');
                const txt = document.getElementById('txt-sugestao-adaptativa');
                const btn = document.getElementById('btn-acao-sugestao');
                if (!faixa || !txt || !btn) return;

                const labelMap = {
                    'salvar_voto': 'Salvar alterações da minuta',
                    'exportar_docx': 'Exportar para Word (.docx)',
                    'exportar_odt': 'Exportar para LibreOffice (.odt)',
                    'exportar_pdf': 'Gerar Impressão A4 em PDF',
                    'filtrar_pendentes': 'Filtrar processos pendentes',
                    'abrir_pje': 'Abrir PJe 2º Grau'
                };

                const rotulo = labelMap[actionName] || actionName;
                txt.innerHTML = `<strong>Próxima ação provável:</strong> ${rotulo} <span class="opacity-70 text-[10px]">(${pct}% das sessões)</span>`;
                btn.onclick = () => {
                    this.executeSuggestedAction(actionName);
                    faixa.classList.add('hidden');
                    this.recordAction('aceitou_sugestao', { action: actionName });
                };
                faixa.classList.remove('hidden');
            },

            executeSuggestedAction(actionName) {
                switch(actionName) {
                    case 'salvar_voto': salvarConteudoEditadoDireto(); break;
                    case 'exportar_docx': exportarVotoCorrigidoAtivo('docx'); break;
                    case 'exportar_odt': exportarVotoCorrigidoAtivo('odt'); break;
                    case 'exportar_pdf': exportarVotoCorrigidoAtivo('pdf'); break;
                    case 'filtrar_pendentes': filtrarPauta('Pendentes'); break;
                    case 'abrir_pje': window.open('https://pjesg.tjpb.jus.br/pje2g/ng2/dev.seam#/painel-usuario-interno', '_blank'); break;
                }
            },

            dismissCurrentSuggestion() {
                const faixa = document.getElementById('faixa-sugestao-adaptativa');
                if (faixa) faixa.classList.add('hidden');
            },

            renderSmartShortcuts() {
                const container = document.getElementById('container-atalhos-inteligentes');
                if (!container || !this.profile.atalhosAtivos) return;

                const freq = this.profile.frequencias;
                const sorted = Object.keys(freq).sort((a, b) => freq[b] - freq[a]).slice(0, 4);

                if (sorted.length === 0) {
                    container.innerHTML = `
                        <button onclick="filtrarPauta('Pendentes')" class="px-2 py-0.5 rounded bg-surface border border-border text-text-secondary hover:text-text-primary font-bold transition text-[10px]">Pendentes</button>
                        <button onclick="document.getElementById('input-busca-pauta').focus()" class="px-2 py-0.5 rounded bg-surface border border-border text-text-secondary hover:text-text-primary font-bold transition text-[10px]">Buscar</button>
                    `;
                    return;
                }

                const mapaBotoes = {
                    'salvar_voto': { label: 'Salvar Minuta', fn: 'salvarConteudoEditadoDireto()' },
                    'exportar_docx': { label: 'Word (.docx)', fn: "exportarVotoCorrigidoAtivo('docx')" },
                    'exportar_odt': { label: 'LibreOffice (.odt)', fn: "exportarVotoCorrigidoAtivo('odt')" },
                    'filtrar_pauta': { label: 'Filtrar Pendentes', fn: "filtrarPauta('Pendentes')" },
                    'abrir_central_links': { label: 'Sistemas', fn: 'abrirModalCentralLinks()' }
                };

                let html = '';
                sorted.forEach(k => {
                    const item = mapaBotoes[k];
                    if (item) {
                        html += `<button onclick="${item.fn}" class="px-2 py-0.5 rounded bg-surface border border-border text-text-secondary hover:text-text-primary font-bold transition text-[10px]">${item.label}</button>`;
                    }
                });
                container.innerHTML = html || `<span class="text-[10px] text-text-muted">Aprendendo com seu fluxo...</span>`;
            },

            renderModalContent() {
                const el = document.getElementById('status-aprendizado-conteudo');
                if (!el) return;
                const totalEventos = this.profile.eventos.length;
                const freq = this.profile.frequencias;
                const topAcoes = Object.keys(freq).sort((a, b) => freq[b] - freq[a]).slice(0, 3);

                el.innerHTML = `
                    <div>&bull; <strong>Total de ações observadas:</strong> ${totalEventos} interações processuais</div>
                    <div>&bull; <strong>Ações mais frequentes:</strong> ${topAcoes.length ? topAcoes.join(', ') : 'Padrão inicial em observação'}</div>
                    <div>&bull; <strong>Transições aprendidas:</strong> ${Object.keys(this.profile.sequencias).length} fluxos sequenciais registrados</div>
                    <div>&bull; <strong>Correções manuais respeitadas:</strong> ${this.profile.correcoes.length} ajustes do magistrado/assessor</div>
                `;

                document.getElementById('chk-adaptativo-ativo').checked = this.profile.ativo;
                document.getElementById('chk-adaptativo-atalhos').checked = this.profile.atalhosAtivos;
            },

            toggleAtivo(v) { this.profile.ativo = v; this.salvar(); },
            toggleAtalhos(v) { this.profile.atalhosAtivos = v; this.salvar(); this.renderSmartShortcuts(); },

            resetarAprendizado() {
                this.profile.eventos = [];
                this.profile.frequencias = {};
                this.profile.sequencias = {};
                this.profile.rejeicoes = {};
                this.profile.correcoes = [];
                this.salvar();
                this.renderModalContent();
                this.renderSmartShortcuts();
                exibirToast('Perfil de aprendizado redefinido com sucesso.');
            },

            exportarPerfilJson() {
                const blob = new Blob([JSON.stringify(this.profile, null, 2)], { type: 'application/json' });
                const a = document.createElement('a');
                a.href = URL.createObjectURL(blob);
                a.download = `PERFIL_ADAPTATIVO_TJPB_${new Date().toISOString().split('T')[0]}.json`;
                a.click();
            }
        };

        /* ==========================================================================
           3. MOTOR SEMÂNTICO DE JULGAMENTO (Conclusão, Dispositivo, Ementa)
           ========================================================================== */
        function analisarSemanticaJulgamento(textoCompleto, classeProcessual = "") {
            if (!textoCompleto) return { resultado: null, confianca: 'BAIXA', origem: 'Indefinido', trecho: '' };

            const sanitizado = textoCompleto
                .replace(/&nbsp;/g, ' ')
                .replace(/\s+/g, ' ')
                .replace(/["'“”«»]/g, '"');

            // 1ª PRIORIDADE: DISPOSITIVO / CONCLUSÃO FINAL
            const regexDispositivo = /(?:DISPOSITIVO|CONCLUSÃO|ANTE\s+O\s+EXPOSTO|PELO\s+EXPOSTO|ISSO\s+POSTO|POSTO\s+ISSO)[\s:.\-_]+([\s\S]{30,2200})/i;
            const matchDispositivo = sanitizado.match(regexDispositivo);
            const textoDispositivo = matchDispositivo ? matchDispositivo[1] : sanitizado.slice(-2800);

            let resDispositivo = extrairResultadoEspecifico(textoDispositivo, classeProcessual);
            if (resDispositivo) {
                return {
                    resultado: resDispositivo.categoria,
                    confianca: 'ALTA',
                    origem: matchDispositivo ? 'Dispositivo' : 'Conclusão Final',
                    trecho: resDispositivo.trecho
                };
            }

            // 2ª PRIORIDADE: EMENTA
            const regexEmenta = /(?:EMENTA|ACÓRDÃO)[\s:.\-_]+([\s\S]{30,2000})/i;
            const matchEmenta = sanitizado.match(regexEmenta);
            if (matchEmenta) {
                let resEmenta = extrairResultadoEspecifico(matchEmenta[1], classeProcessual);
                if (resEmenta) {
                    return {
                        resultado: resEmenta.categoria,
                        confianca: 'MÉDIA',
                        origem: 'Ementa',
                        trecho: resEmenta.trecho
                    };
                }
            }

            // 3ª PRIORIDADE SUBSIDIÁRIA: CORPO DO VOTO
            let resCorpo = extrairResultadoEspecifico(sanitizado.slice(-4000), classeProcessual);
            if (resCorpo) {
                return {
                    resultado: resCorpo.categoria,
                    confianca: 'BAIXA',
                    origem: 'Corpo do Voto',
                    trecho: resCorpo.trecho
                };
            }

            return { resultado: null, confianca: 'BAIXA', origem: 'Não Identificado', trecho: '' };
        }

        function limparFalsosPositivos(trecho) {
            return trecho
                .replace(/(?:o\s+apelante|o\s+recorrente|o\s+autor|a\s+autora|a\s+parte)\s+(?:pugna|requer|pleiteia|busca|postula)\s+(?:pelo|o)\s+(?:provimento|desprovimento)/gi, '')
                .replace(/(?:sentença\s+que\s+julgou|decisão\s+recorrida\s+que)\s+(?:procedente|improcedente|extinto)/gi, '')
                .replace(/parecer\s+ministerial\s+(?:pelo|no\s+sentido\s+do)\s+(?:provimento|desprovimento)/gi, '');
        }

        function extrairResultadoEspecifico(texto, classe) {
            const t = limparFalsosPositivos(texto.toLowerCase());
            const isMS = /mandado\s+de\s+segurança|ms\b/i.test(classe);
            const isOriginaria = /ação\s+rescisória|conflito\s+de\s+competência|reclamação/i.test(classe);

            // Regra 1: Extinção sem resolução de mérito
            if (t.match(/(?:extinção\s+sem\s+(?:resolução|julgamento)|extingo\s+o\s+processo\s+sem|extinto\s+sem\s+resolução|art(?:igo)?\.?\s*485)/i)) {
                return { categoria: 'Extinção', trecho: 'Extinção sem resolução do mérito' };
            }

            // Regra 2: Não Conhecido / Prejudicado
            if (t.match(/(?:não\s+conheço|não\s+se\s+conhece|recurso\s+não\s+conhecido|inadmissibilidade\s+recursal|dele\s+não\s+conheço)/i)) {
                return { categoria: 'Não Conhecido', trecho: 'Não conhecimento do recurso' };
            }
            if (t.match(/(?:julgo\s+prejudicado|resta\s+prejudicado|recurso\s+prejudicado|perda\s+do\s+objeto)/i) && !t.includes('prejudicial de mérito')) {
                return { categoria: 'Prejudicado', trecho: 'Recurso declarado prejudicado' };
            }

            // Regra 3: Mandado de Segurança
            if (isMS) {
                if (t.match(/(?:concedo\s+a\s+segurança|segurança\s+concedida|conceder\s+a\s+segurança|concedida\s+a\s+ordem)/i)) {
                    return { categoria: 'Concedida', trecho: 'Segurança concedida' };
                }
                if (t.match(/(?:denego\s+a\s+segurança|segurança\s+denegada|denegar\s+a\s+segurança|denegada\s+a\s+ordem)/i)) {
                    return { categoria: 'Denegada', trecho: 'Segurança denegada' };
                }
            }

            // Regra 4: Ações Originárias
            if (isOriginaria) {
                if (t.match(/(?:julgo\s+procedente|pedido\s+procedente|ação\s+rescisória\s+procedente)/i)) {
                    return { categoria: 'Procedente', trecho: 'Pedido julgado procedente' };
                }
                if (t.match(/(?:julgo\s+improcedente|pedido\s+improcedente|ação\s+rescisória\s+improcedente)/i)) {
                    return { categoria: 'Improcedente', trecho: 'Pedido julgado improcedente' };
                }
            }

            // Regra 5: Especificidade Recursal (Parcial antes do Integral)
            if (t.match(/(?:parcialmente\s+provido|provido\s+em\s+parte|parcial\s+provimento|provimento\s+parcial|dou\s+parcial\s+provimento|dar\s+parcial\s+provimento)/i)) {
                return { categoria: 'Prov. Parcial', trecho: 'Provimento parcial' };
            }
            if (t.match(/(?:nego\s+provimento|negar\s+provimento|negado\s+provimento|desprovimento|recurso\s+desprovido|recurso\s+improvido|improvido|desprovido)/i)) {
                return { categoria: 'Improvido', trecho: 'Recurso improvido/desprovido' };
            }
            if (t.match(/(?:dou\s+provimento|dar\s+provimento|provimento\s+do\s+recurso|recurso\s+provido|apelo\s+provido|acolho\s+os\s+embargos)/i)) {
                return { categoria: 'Provido', trecho: 'Recurso provido' };
            }

            if (t.match(/(?:julgo\s+procedente|procedência\s+do\s+pedido)/i)) return { categoria: 'Procedente', trecho: 'Procedente' };
            if (t.match(/(?:julgo\s+improcedente|improcedência\s+do\s+pedido)/i)) return { categoria: 'Improcedente', trecho: 'Improcedente' };

            return null;
        }

        /* ==========================================================================
           4. EXTRAÇÃO DE METADADOS & IMPORTAÇÃO DE ARQUIVOS (SEM PERDAS EM LOTE)
           ========================================================================== */
        function normalizarNumeroCNJ(texto) {
            if (!texto) return "";
            const matchFormatado = texto.match(/\b\d{7}-\d{2}\.\d{4}\.\d\.\d{2}\.\d{4}\b/);
            if (matchFormatado) return matchFormatado[0];

            const match20Digitos = texto.match(/\b\d{20}\b/);
            if (match20Digitos) {
                const s = match20Digitos[0];
                return `${s.slice(0,7)}-${s.slice(7,9)}.${s.slice(9,13)}.${s.slice(13,14)}.${s.slice(14,16)}.${s.slice(16,20)}`;
            }

            const matchParcial = texto.match(/\b\d{7}-\d{2}\.\d{4}\b/);
            if (matchParcial) {
                return `${matchParcial[0]}.8.15.0000`;
            }
            return "";
        }

        function extrairSiglaAssessor(nomeArquivo, textoDocumento) {
            const pool = [...ASSESSORES_GABINETE, ...(appState.assessoresCustom || [])];
            const cabecalho = (textoDocumento || "").slice(0, 1000);
            
            for (let a of pool) {
                const regex = new RegExp(`(?:[_\-\\s\\[\\(]|^)${a.sigla}(?:[_\-\\s\\]\\)]|$)`, 'i');
                if (regex.test(nomeArquivo) || regex.test(cabecalho)) {
                    return a.sigla;
                }
            }
            return "ADL";
        }

        function extrairMetadadosProcessuaisPermanente(nomeArquivo, textoDocumento, indexLote = 0) {
            const cabecalho = (textoDocumento || "").slice(0, 3500);
            let cnj = normalizarNumeroCNJ(cabecalho) || normalizarNumeroCNJ(nomeArquivo);

            if (!cnj) {
                const timestampId = String(Date.now()).slice(-4);
                cnj = `08${String(indexLote + 1).padStart(5, '0')}-${timestampId}.2026.8.15.0000`;
            }

            let classeProcessual = "";
            const matchClasse = cabecalho.match(/(?:CLASSE|PROCESSO|AÇÃO|RECURSO)[\s:.\-_]*([A-ZÁÉÍÓÚÂÊÔÃÕÇ\s]{3,40})(?:\s*N[º°]|\s*N\b|\s*\n)/i) ||
                                 cabecalho.match(/(Apelação(?:\s+Cível|\s+Criminal)?|Agravo\s+de\s+Instrumento|Agravo\s+Interno|Embargos\s+de\s+Declaração|Habeas\s+Corpus|Mandado\s+de\s+Segurança|Ação\s+Rescisória|Conflito\s+de\s+Competência|Reexame\s+Necessário|Recurso\s+Inominado)/i) ||
                                 nomeArquivo.match(/(Apelação|Agravo|Embargos|MS|HC|Rescisória|Conflito|AC|AI|ED)/i);

            if (matchClasse) classeProcessual = matchClasse[1].trim();

            let relator = appState.relatorAtivo;
            const assessor = extrairSiglaAssessor(nomeArquivo, cabecalho);

            // Identificação de Polos
            let recorrente = "";
            let recorrido = "";
            let tipoPoloAtivo = "Recorrente";
            let tipoPoloPassivo = "Recorrido(a)";

            const mAtivo = cabecalho.match(/(?:APELANTE|AGRAVANTE|EMBARGANTE|IMPETRANTE|AUTOR|RECORRENTE)[\s:.\-_]+([^\n\r;]{3,70})/i);
            const mPassivo = cabecalho.match(/(?:APELADO|AGRAVADO|EMBARGADO|IMPETRADO|RÉU|RECORRIDO)[\s:.\-_]+([^\n\r;]{3,70})/i);

            if (mAtivo) recorrente = mAtivo[1].trim();
            if (mPassivo) recorrido = mPassivo[1].trim();

            if (!recorrente || !recorrido) {
                const matchVersus = nomeArquivo.match(/(.+?)\s+[xX]\s+(.+)/i);
                if (matchVersus) {
                    if (!recorrente) recorrente = matchVersus[1].replace(/_/g, ' ').trim();
                    if (!recorrido) recorrido = matchVersus[2].replace(/_/g, ' ').replace(/\.[^.]+$/, '').trim();
                }
            }

            if (!recorrente) recorrente = "Polo Ativo";
            if (!recorrido) recorrido = "Polo Passivo";

            return {
                cnj,
                classeProcessual: classeProcessual || "Recurso Judicial",
                tipoPoloAtivo,
                tipoPoloPassivo,
                recorrente,
                recorrido,
                partes: `${recorrente} x ${recorrido}`,
                relator,
                assessor,
                prioridade: /idoso|prioridade|urgente/i.test(nomeArquivo + cabecalho) ? 'Legal' : null,
                sustentacaoOral: /sustenta/i.test(nomeArquivo + cabecalho)
            };
        }

        async function processarArquivosImportados(input, isLote = false) {
            const files = input.files;
            if (!files || !files.length) return;

            exibirToast(`Importando ${files.length} documento(s) na SEV...`);
            const procsDaSessao = getProcessosContextoAtual();
            let count = procsDaSessao.length;

            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                const ext = file.name.split('.').pop().toLowerCase();
                const docId = `doc-${Date.now()}-${i}`;
                const procId = `proc-${Date.now()}-${i}`;

                let htmlProcessado = "";
                let textoPuro = "";
                let formatoDetectado = ext.toUpperCase();

                try {
                    const arrayBuffer = await file.arrayBuffer();

                    if (ext === 'docx' || ext === 'dotx' || ext === 'docm') {
                        formatoDetectado = "DOCX (Word XML)";
                        const result = await mammoth.convertToHtml({ arrayBuffer: arrayBuffer });
                        htmlProcessado = result.value || "";
                    } else if (ext === 'odt') {
                        formatoDetectado = "ODT (LibreOffice)";
                        htmlProcessado = await processarOdtCompleto(file);
                    } else if (ext === 'pdf') {
                        formatoDetectado = "PDF";
                        const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
                        let textBlocks = [];
                        for (let p = 1; p <= pdf.numPages; p++) {
                            const page = await pdf.getPage(p);
                            const content = await page.getTextContent();
                            content.items.forEach(item => textBlocks.push(item.str));
                        }
                        htmlProcessado = textBlocks.map(t => `<p>${t}</p>`).join('');
                    } else {
                        const dec = new TextDecoder('utf-8').decode(arrayBuffer);
                        htmlProcessado = dec.split(/\r\n\r\n|\n\n/).map(p => `<p>${p.trim()}</p>`).join('');
                    }
                    textoPuro = htmlProcessado.replace(/<[^>]+>/g, ' ');
                } catch(e) {
                    console.error("Erro na leitura do arquivo", file.name, e);
                    htmlProcessado = `<p>[Conteúdo importado do arquivo ${file.name}]</p>`;
                    textoPuro = file.name;
                }

                const nomeLimpo = file.name.replace(/\.[^/.]+$/, "");
                const metadados = extrairMetadadosProcessuaisPermanente(nomeLimpo, textoPuro, count + i);
                const analise = analisarSemanticaJulgamento(textoPuro, metadados.classeProcessual);

                appState.processos.push({
                    id: procId,
                    sessaoData: appState.sessaoData,
                    ownerUnidade: appState.unidadeAtiva,
                    ownerRelator: appState.relatorAtivo,
                    seqOriginal: count + i + 1,
                    seqSessao: (appState.numInicioSessao || 35) + count + i,
                    cnj: metadados.cnj,
                    classeProcessual: metadados.classeProcessual,
                    tipoPoloAtivo: metadados.tipoPoloAtivo,
                    tipoPoloPassivo: metadados.tipoPoloPassivo,
                    recorrente: metadados.recorrente,
                    recorrido: metadados.recorrido,
                    partes: metadados.partes,
                    relator: metadados.relator,
                    assessor: metadados.assessor,
                    status: analise.resultado ? 'Julgado' : 'Pendente',
                    situacao: analise.resultado ? 'Julgado' : 'Pendente',
                    desfecho: analise.resultado || '',
                    resultado: analise.resultado || null,
                    votado: !!analise.resultado,
                    selecionado: false,
                    prioridade: metadados.prioridade,
                    sustentacaoOral: metadados.sustentacaoOral,
                    analiseSemantica: analise,
                    documentos: [{
                        id: docId,
                        nome: file.name,
                        extensao: ext,
                        formatoDetectado: formatoDetectado,
                        conteudoTexto: htmlProcessado,
                        conteudoTextoOriginal: htmlProcessado
                    }]
                });

                if (i === 0 && !appState.processoSelecionadoId) {
                    appState.processoSelecionadoId = procId;
                }
            }

            input.value = '';
            salvarEstadoLocal();
            renderizarTudo();
            AdaptiveEngine.recordAction(isLote ? 'importar_lote' : 'importar_voto', { total: files.length });
            exibirToast(`${files.length} voto(s) importado(s) com sucesso na SEV!`);
        }

        async function processarOdtCompleto(file) {
            try {
                const zip = await JSZip.loadAsync(file);
                const contentXml = await zip.file("content.xml").async("text");
                const parser = new DOMParser();
                const xmlDoc = parser.parseFromString(contentXml, "text/xml");
                let html = "";
                const body = xmlDoc.getElementsByTagName("office:body")[0];
                if (!body) return "<p></p>";
                const textNode = body.getElementsByTagName("office:text")[0];
                if (!textNode) return "<p></p>";
                for (let child of textNode.children) {
                    const tag = child.tagName.toLowerCase();
                    if (tag.includes('p') || tag.includes('h')) {
                        const txt = child.textContent.trim();
                        if (txt) html += `<p>${txt}</p>`;
                    }
                }
                return html || "<p></p>";
            } catch(e) {
                return "<p>[Documento ODT processado]</p>";
            }
        }

        /* ==========================================================================
           5. RENDERIZAÇÃO DA PAUTA & DESTAQUES LUMINOSOS
           ========================================================================== */
        function getProcessosContextoAtual() {
            return appState.processos.filter(p =>
                p.sessaoData === appState.sessaoData &&
                (!p.ownerUnidade || p.ownerUnidade === appState.unidadeAtiva) &&
                (!p.ownerRelator || p.ownerRelator === appState.relatorAtivo)
            );
        }

        function renderizarListaPauta() {
            const container = document.getElementById('container-lista-pauta');
            if (!container) return;
            container.innerHTML = '';

            let procs = getProcessosContextoAtual();
            const query = (document.getElementById('input-busca-pauta')?.value || '').toLowerCase();

            procs = procs.filter(p => {
                const atendeBusca = p.cnj.toLowerCase().includes(query) ||
                                    (p.partes && p.partes.toLowerCase().includes(query)) ||
                                    (p.classeProcessual && p.classeProcessual.toLowerCase().includes(query)) ||
                                    (p.assessor && p.assessor.toLowerCase().includes(query));
                if (!atendeBusca) return false;

                if (appState.filtroPauta === 'Pendentes') return (p.status === 'Pendente') && !p.votado;
                if (appState.filtroPauta === 'Julgados') return p.votado;
                if (appState.filtroPauta === 'Prioridades') return p.prioridade || p.sustentacaoOral;
                return true;
            });

            const sortFn = (a, b) => appState.modoOrdem === 'sessao' ? a.seqSessao - b.seqSessao : a.seqOriginal - b.seqOriginal;
            procs.sort(sortFn);

            if (procs.length === 0) {
                container.innerHTML = `
                    <div class="p-8 text-center text-text-muted flex flex-col items-center mt-6">
                        <i data-lucide="inbox" class="w-8 h-8 opacity-40 mb-2"></i>
                        <p class="text-xs font-semibold">Nenhum voto cadastrado nesta pauta</p>
                    </div>`;
                if (window.lucide) lucide.createIcons();
                atualizarContadores();
                return;
            }

            procs.forEach(proc => {
                const isSel = proc.id === appState.processoSelecionadoId;
                const numSeq = String(appState.modoOrdem === 'sessao' ? proc.seqSessao : proc.seqOriginal).padStart(2, '0');
                const isAdiado = proc.status === 'Adiado' || proc.desfecho === 'Adiado';

                // Classes de Destaque Luminoso
                let highlightClasses = "";
                if (isSel) highlightClasses += " is-active";
                if (proc.prioridade) highlightClasses += " has-prioridade";
                if (proc.sustentacaoOral) highlightClasses += " has-sustentacao";

                const isA = (val) => {
                    const cur = (proc.desfecho || proc.resultado || proc.status || '').toLowerCase().trim();
                    const target = val.toLowerCase().trim();
                    if (cur === target) {
                        if (cur.includes('não conhec') || cur === 'prejudicado') return 'active-nconhecido';
                        if (cur.includes('parcial')) return 'active-parcial';
                        if (cur.includes('extin')) return 'active-extincao';
                        if (['improvido', 'improcedente', 'denegada'].includes(cur)) return 'active-improvido';
                        if (['provido', 'procedente', 'concedida'].includes(cur)) return 'active-provido';
                        if (cur === 'adiado') return 'active-adiado';
                    }
                    return '';
                };

                // Montagem do Seletor de Assessores
                const poolAssessores = [...ASSESSORES_GABINETE, ...(appState.assessoresCustom || [])];
                let optsAssessores = poolAssessores.map(a => 
                    `<option value="${a.sigla}" ${a.sigla === proc.assessor ? 'selected' : ''}>${a.sigla} (${a.nome})</option>`
                ).join('');
                optsAssessores += `<option value="CUSTOM">+ Novo...</option>`;

                const card = document.createElement('div');
                card.className = `process-card p-3 rounded-2xl border mb-2 cursor-pointer flex flex-col gap-1.5 ${highlightClasses}`;
                card.onclick = () => selecionarProcesso(proc.id);

                card.innerHTML = `
                    <div class="flex items-center justify-between gap-1">
                        <div class="flex items-center gap-1.5 overflow-hidden">
                            <input type="checkbox" ${proc.selecionado ? 'checked' : ''} onclick="event.stopPropagation()" onchange="alternarSelecaoProcesso('${proc.id}', this.checked)" class="w-3.5 h-3.5 rounded text-primary border-border focus:ring-0">
                            <span class="font-black text-[11px] bg-surface-soft text-text-primary px-1.5 py-0.5 rounded-md border border-border shadow-2xs">#${numSeq}</span>
                            
                            ${isSel ? `<span class="bg-primary text-white text-[9px] font-black px-1.5 py-0.5 rounded flex items-center gap-0.5"><i data-lucide="edit-3" class="w-2.5 h-2.5"></i> EM EDIÇÃO</span>` : ''}
                            ${proc.prioridade ? `<span class="bg-accent text-white text-[9px] font-black px-1.5 py-0.5 rounded flex items-center gap-0.5"><i data-lucide="star" class="w-2.5 h-2.5 fill-current"></i> PRIORIDADE</span>` : ''}
                            ${proc.sustentacaoOral ? `<span class="bg-info text-white text-[9px] font-black px-1.5 py-0.5 rounded flex items-center gap-0.5"><i data-lucide="mic" class="w-2.5 h-2.5"></i> SUSTENTAÇÃO</span>` : ''}
                        </div>

                        <div class="flex items-center gap-1 shrink-0">
                            <button type="button" onclick="event.stopPropagation(); togglePrioridade('${proc.id}')" class="p-1 rounded hover:bg-surface-soft ${proc.prioridade ? 'text-accent' : 'text-text-muted'}" title="Prioridade Legal"><i data-lucide="star" class="w-3.5 h-3.5 ${proc.prioridade ? 'fill-current' : ''}"></i></button>
                            <button type="button" onclick="event.stopPropagation(); toggleSustentacao('${proc.id}')" class="p-1 rounded hover:bg-surface-soft ${proc.sustentacaoOral ? 'text-info' : 'text-text-muted'}" title="Sustentação Oral"><i data-lucide="mic" class="w-3.5 h-3.5"></i></button>
                            <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Adiado', 'Adiado');" class="p-1 rounded hover:bg-surface-soft ${isAdiado ? 'text-text-primary font-bold' : 'text-text-muted'}" title="Adiar"><i data-lucide="clock-4" class="w-3.5 h-3.5"></i></button>
                            <button onclick="event.stopPropagation(); abrirModalEditar('${proc.id}')" class="p-1 hover:bg-surface-soft rounded text-text-muted" title="Editar Metadados"><i data-lucide="edit-3" class="w-3.5 h-3.5"></i></button>
                            <button onclick="event.stopPropagation(); excluirProcesso('${proc.id}')" class="p-1 hover:bg-danger/10 rounded text-danger" title="Remover"><i data-lucide="trash" class="w-3.5 h-3.5"></i></button>
                        </div>
                    </div>

                    <div class="flex flex-col gap-0.5">
                        <div class="flex items-center justify-between gap-1">
                            <span class="font-black text-xs text-text-primary tracking-tight font-mono hover:text-primary">${proc.cnj}</span>
                            <div class="flex items-center gap-1">
                                <!-- Seletor Rápido de Assessor -->
                                <select onclick="event.stopPropagation()" onchange="aoMudarAssessorProcesso('${proc.id}', this.value)" class="text-[9.5px] font-bold bg-surface-soft border border-border rounded px-1 py-0.5 text-primary outline-none cursor-pointer">
                                    ${optsAssessores}
                                </select>
                                <span class="text-[9px] font-bold px-1.5 py-0.2 rounded bg-surface-soft text-text-secondary border border-border truncate max-w-[110px]">${proc.classeProcessual}</span>
                            </div>
                        </div>
                        <div class="text-[11px] text-text-secondary leading-tight mt-0.5">
                            <div class="truncate"><span class="font-bold text-text-primary">${proc.tipoPoloAtivo || 'Ativo'}:</span> ${proc.recorrente}</div>
                            <div class="truncate"><span class="font-bold text-text-primary">${proc.tipoPoloPassivo || 'Passivo'}:</span> ${proc.recorrido}</div>
                        </div>
                    </div>

                    <!-- Botões Rápidos de Julgamento -->
                    <div class="pt-1.5 border-t border-border grid grid-cols-3 gap-1 text-[9.5px] font-bold">
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Provido');" class="btn-vote ${isA('Provido')}">Provido</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Procedente');" class="btn-vote ${isA('Procedente')}">Procedente</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Concedida');" class="btn-vote ${isA('Concedida')}">Concedida</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Improvido');" class="btn-vote ${isA('Improvido')}">Improvido</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Improcedente');" class="btn-vote ${isA('Improcedente')}">Improcedente</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Denegada');" class="btn-vote ${isA('Denegada')}">Denegada</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Prov. Parcial');" class="btn-vote ${isA('Prov. Parcial')}">Parcial</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Não Conhecido');" class="btn-vote ${isA('Não Conhecido')}">N. Conhecido</button>
                        <button type="button" onclick="event.stopPropagation(); registrarStatusProcesso('${proc.id}', 'Julgado', 'Extinção');" class="btn-vote ${isA('Extinção')}">Extinção</button>
                    </div>
                `;

                container.appendChild(card);
            });

            if (window.lucide) lucide.createIcons();
            atualizarContadores();
        }

        /* ==========================================================================
           6. SINCRONIZAÇÃO DO LEITOR, EDITOR E ASSESSORES
           ========================================================================== */
        function selecionarProcesso(procId) {
            appState.processoSelecionadoId = procId;
            salvarEstadoLocal();
            renderizarListaPauta();
            renderizarPainelLeitura();
            AdaptiveEngine.recordAction('selecionar_processo', { procId });

            // Mobile off-canvas: ao clicar, recolhe a barra lateral
            if (window.innerWidth < 768) {
                document.getElementById('aside-pauta')?.classList.add('-translate-x-full');
            }
        }

        function voltarParaListaMobile() {
            document.getElementById('aside-pauta')?.classList.remove('-translate-x-full');
        }

        function renderizarPainelLeitura() {
            const proc = getProcessosContextoAtual().find(p => p.id === appState.processoSelecionadoId);
            if (!proc) {
                document.getElementById('estaca-zero-view').classList.remove('hidden');
                document.getElementById('painel-leitura-ativo').classList.add('hidden');
                return;
            }

            document.getElementById('estaca-zero-view').classList.add('hidden');
            document.getElementById('painel-leitura-ativo').classList.remove('hidden');

            const seq = appState.modoOrdem === 'sessao' ? proc.seqSessao : proc.seqOriginal;
            document.getElementById('badge-seq-ativo').innerText = `#${String(seq).padStart(2, '0')}`;
            document.getElementById('title-processo-cnj').innerText = proc.cnj;
            
            const badgeClasse = document.getElementById('badge-classe-ativo');
            if (badgeClasse) badgeClasse.innerText = proc.classeProcessual || 'Recurso Judicial';

            document.getElementById('subtitle-processo-partes').innerText = 
                `Relator(a): ${proc.relator} | ${proc.tipoPoloAtivo || 'Ativo'}: ${proc.recorrente} x ${proc.tipoPoloPassivo || 'Passivo'}: ${proc.recorrido}`;

            // Sincroniza Assessor no Cabeçalho do Leitor
            const selAssessor = document.getElementById('select-assessor-ativo');
            if (selAssessor) {
                const pool = [...ASSESSORES_GABINETE, ...(appState.assessoresCustom || [])];
                selAssessor.innerHTML = pool.map(a => 
                    `<option value="${a.sigla}" ${a.sigla === proc.assessor ? 'selected' : ''}>${a.sigla} - ${a.nome}</option>`
                ).join('') + `<option value="CUSTOM">+ Novo...</option>`;
            }

            // Badge de Status e Resultado
            const badgeStatus = document.getElementById('badge-status-ativo');
            const res = proc.resultado || proc.desfecho || proc.status || 'PENDENTE';
            badgeStatus.innerText = res.toUpperCase();

            let colorClass = 'bg-surface-soft text-text-secondary border-border';
            if (['Provido', 'Procedente', 'Concedida'].includes(res)) colorClass = 'bg-success/20 text-success border-success/40';
            else if (['Improvido', 'Improcedente', 'Denegada'].includes(res)) colorClass = 'bg-danger/20 text-danger border-danger/40';
            else if (res.includes('Parcial')) colorClass = 'bg-warning/20 text-warning border-warning/40';
            else if (res === 'Não Conhecido' || res === 'Prejudicado') colorClass = 'bg-purple-500/20 text-purple-600 border-purple-500/40';
            else if (res === 'Extinção') colorClass = 'bg-surface-hover text-text-primary border-border';
            badgeStatus.className = `px-2 py-0.5 text-[9.5px] font-bold rounded uppercase tracking-wider border ${colorClass}`;

            const editor = document.getElementById('editor-conteudo-voto');
            if (editor && proc.documentos && proc.documentos.length > 0) {
                editor.innerHTML = proc.documentos[0].conteudoTexto || `<p>Minuta do voto.</p>`;
                editorUndoStack = [editor.innerHTML];
                editorRedoStack = [];
            }

            if (window.lucide) lucide.createIcons();
        }

        function aoMudarAssessorProcesso(procId, sigla) {
            if (sigla === 'CUSTOM') {
                const novo = prompt('Informe a sigla e o nome do Assessor(a) (ex: JPB - João):');
                if (novo) {
                    const partes = novo.split('-');
                    const s = partes[0].trim().toUpperCase();
                    const n = partes[1] ? partes[1].trim() : s;
                    if (!appState.assessoresCustom) appState.assessoresCustom = [];
                    appState.assessoresCustom.push({ sigla: s, nome: n });
                    sigla = s;
                } else {
                    renderizarListaPauta();
                    return;
                }
            }
            const proc = appState.processos.find(p => p.id === procId);
            if (proc) {
                proc.assessor = sigla;
                salvarEstadoLocal();
                renderizarListaPauta();
                if (appState.processoSelecionadoId === procId) renderizarPainelLeitura();
            }
        }

        function aoMudarAssessorAtivo(sigla) {
            if (!appState.processoSelecionadoId) return;
            aoMudarAssessorProcesso(appState.processoSelecionadoId, sigla);
        }

        function registrarStatusProcesso(procId, status, desfecho) {
            const proc = appState.processos.find(p => p.id === procId);
            if (proc) {
                const curDesfecho = proc.desfecho || proc.resultado || proc.status || '';
                const antigoResultado = curDesfecho;

                if ((proc.votado || proc.status === 'Adiado') && curDesfecho.toLowerCase() === (desfecho || status).toLowerCase()) {
                    proc.status = 'Pendente';
                    proc.situacao = 'Pendente';
                    proc.resultado = null;
                    proc.desfecho = '';
                    proc.votado = false;
                    exibirToast('Status alterado para Pendente');
                } else {
                    proc.status = status;
                    proc.situacao = status;
                    proc.resultado = desfecho || status;
                    proc.desfecho = desfecho || status;
                    proc.votado = status === 'Julgado' || status === 'Adiado';
                    exibirToast(`Resultado: ${desfecho || status}`);

                    if (antigoResultado && antigoResultado !== (desfecho || status)) {
                        AdaptiveEngine.recordCorrection(procId, antigoResultado, desfecho || status);
                    }
                }
                salvarEstadoLocal();
                renderizarListaPauta();
                renderizarPainelLeitura();
                atualizarContadores();
                AdaptiveEngine.recordAction('votar_resultado', { status: desfecho || status });
            }
        }

        /* ==========================================================================
           7. COMANDOS WYSIWYG, RÉGUA E EXPORTAÇÃO (DOCX, ODT, PDF)
           ========================================================================== */
        function executarComandoEditorFocado(cmd, value = null) {
            if (editorSavedRange) {
                const sel = window.getSelection();
                sel.removeAllRanges();
                sel.addRange(editorSavedRange);
            }
            document.execCommand(cmd, false, value);
            salvarConteudoEditadoDireto(true);
        }

        function executarComandoEditor(cmd, value = null) {
            document.execCommand(cmd, false, value);
            salvarConteudoEditadoDireto(true);
        }

        function aplicarEstiloBlocoFocado(estilo, valor) {
            if (editorSavedRange) {
                const sel = window.getSelection();
                sel.removeAllRanges();
                sel.addRange(editorSavedRange);
            }
            const sel = window.getSelection();
            if (!sel.rangeCount) return;
            let node = sel.anchorNode;
            if (node.nodeType === 3) node = node.parentNode;
            const p = node.closest('p, blockquote');
            if (p) {
                p.style[estilo] = valor;
                salvarConteudoEditadoDireto(true);
            }
        }

        function toggleCitacaoLonga() {
            if (editorSavedRange) {
                const sel = window.getSelection();
                sel.removeAllRanges();
                sel.addRange(editorSavedRange);
            }
            const sel = window.getSelection();
            if (!sel.rangeCount) return;
            let node = sel.anchorNode;
            if (node.nodeType === 3) node = node.parentNode;
            const p = node.closest('p, blockquote');
            if (p) {
                if (p.style.marginLeft === '4cm' || p.classList.contains('citacao-longa')) {
                    p.classList.remove('citacao-longa');
                    p.style.marginLeft = '0cm';
                    p.style.fontSize = '';
                    p.style.textIndent = '1.25cm';
                    p.style.lineHeight = '1.5';
                    exibirToast("Citação longa desativada.");
                } else {
                    p.classList.add('citacao-longa');
                    p.style.marginLeft = '4cm';
                    p.style.fontSize = '10.5pt';
                    p.style.textIndent = '0cm';
                    p.style.lineHeight = '1.2';
                    exibirToast("Citação longa (4cm, 10.5pt) aplicada!");
                }
                salvarConteudoEditadoDireto(true);
            }
        }

        function toggleModoRevisao() {
            isModoRevisao = !isModoRevisao;
            const btn = document.getElementById('btn-modo-revisao');
            if (isModoRevisao) {
                btn.classList.add('bg-danger/20', 'text-danger', 'border-danger/40');
                document.execCommand('foreColor', false, '#dc2626');
                exibirToast("Modo Correção: LIGADO (Letras vermelhas)");
            } else {
                btn.classList.remove('bg-danger/20', 'text-danger', 'border-danger/40');
                document.execCommand('foreColor', false, '#1e293b');
                exibirToast("Modo Correção: DESLIGADO");
            }
        }

        function capturarAntesDeDigitar(event) {
            if (isModoRevisao) document.execCommand('foreColor', false, '#dc2626');
        }

        function lidarComColar(e) {
            e.preventDefault();
            const text = (e.originalEvent || e).clipboardData.getData('text/plain');
            document.execCommand('insertText', false, text);
            salvarConteudoEditadoDireto(true);
        }

        function interceptarTeclasEditor(e) {
            if ((e.ctrlKey || e.metaKey) && e.key === 'z') { e.preventDefault(); executarDesfazer(); }
            else if ((e.ctrlKey || e.metaKey) && e.key === 'y') { e.preventDefault(); executarRefazer(); }
        }

        function executarDesfazer() {
            const editor = document.getElementById('editor-conteudo-voto');
            if (editorUndoStack.length > 1) {
                const curr = editorUndoStack.pop();
                editorRedoStack.push(curr);
                editor.innerHTML = editorUndoStack[editorUndoStack.length - 1];
                salvarConteudoEditadoDireto(true);
            }
        }

        function executarRefazer() {
            const editor = document.getElementById('editor-conteudo-voto');
            if (editorRedoStack.length > 0) {
                const next = editorRedoStack.pop();
                editorUndoStack.push(next);
                editor.innerHTML = next;
                salvarConteudoEditadoDireto(true);
            }
        }

        function lidarComEdicaoDeTexto(el) {
            clearTimeout(debounceTimeout);
            debounceTimeout = setTimeout(() => {
                const currentHTML = el.innerHTML;
                if (editorUndoStack[editorUndoStack.length - 1] !== currentHTML) {
                    editorUndoStack.push(currentHTML);
                    if (editorUndoStack.length > 50) editorUndoStack.shift();
                    editorRedoStack = [];
                }
                salvarConteudoEditadoDireto(true);
            }, 400);
        }

        function salvarConteudoEditadoDireto(silent = false) {
            const proc = appState.processos.find(p => p.id === appState.processoSelecionadoId);
            if (proc && proc.documentos && proc.documentos.length) {
                proc.documentos[0].conteudoTexto = document.getElementById('editor-conteudo-voto').innerHTML;
                salvarEstadoLocal();
                if (!silent) {
                    exibirToast('Voto salvo com sucesso!');
                    AdaptiveEngine.recordAction('salvar_voto');
                }
            }
        }

        function restaurarVotoOriginalAtivo() {
            const proc = appState.processos.find(p => p.id === appState.processoSelecionadoId);
            if (!proc || !proc.documentos || !proc.documentos.length) return;
            proc.documentos[0].conteudoTexto = proc.documentos[0].conteudoTextoOriginal || proc.documentos[0].conteudoTexto;
            salvarEstadoLocal();
            renderizarPainelLeitura();
            exibirToast('Voto restaurado para a versão original do arquivo!');
        }

        function sincronizarRecuoAoClicar(editor) {
            const sel = window.getSelection();
            if (!sel.rangeCount) return;
            let node = sel.anchorNode;
            if (node.nodeType === 3) node = node.parentNode;
            const p = node.closest('p');
            if (p) {
                const recuo1a = p.style.textIndent ? parseFloat(p.style.textIndent.replace('cm', '')) : 1.25;
                const recuoP = p.style.marginLeft ? parseFloat(p.style.marginLeft.replace('cm', '')) : 0.00;
                appState.recuo1aLinhaCm = isNaN(recuo1a) ? 1.25 : recuo1a;
                appState.recuoParagrafoCm = isNaN(recuoP) ? 0.00 : recuoP;
                atualizarPosicaoMarcadoresRegua();
            }
        }

        function atualizarPosicaoMarcadoresRegua() {
            const pct1a = (appState.recuo1aLinhaCm / 16) * 100;
            const pctP = (appState.recuoParagrafoCm / 16) * 100;
            const m1aPrin = document.getElementById('marcador-recuo-1a-principal');
            const mPPrin = document.getElementById('marcador-recuo-paragrafo-principal');
            if (m1aPrin) m1aPrin.style.left = `${pct1a}%`;
            if (mPPrin) mPPrin.style.left = `${pctP}%`;
        }

        /* Exportadores Forenses com Preservação de Correções em Vermelho */
        async function exportarVotoCorrigidoAtivo(formato) {
            const proc = appState.processos.find(p => p.id === appState.processoSelecionadoId);
            if (!proc || !proc.documentos || !proc.documentos.length) return;

            let htmlConteudo = proc.documentos[0].conteudoTexto || "";
            const recuo1aCm = appState.recuo1aLinhaCm || 1.25;
            const recuoPCm = appState.recuoParagrafoCm || 0.00;

            // Padroniza as tags de correção para garantir que o estilo vermelho seja interpretado por todos os softwares
            htmlConteudo = htmlConteudo
                .replace(/class=["']?edit-red["']?/gi, 'style="color: #dc2626; font-weight: 500;" class="edit-red"')
                .replace(/<font color=["']?#dc2626["']?>/gi, '<span style="color: #dc2626; font-weight: 500;" class="edit-red">')
                .replace(/<\/font>/gi, '</span>');

            const cnjLimpo = proc.cnj.replace(/\D/g, '');

            // Helper para identificar nós com formatação de cor vermelha
            const verificarCorVermelha = (node, raiz) => {
                let cur = node;
                while (cur && cur !== raiz) {
                    if (cur.nodeType === 1) {
                        const style = cur.getAttribute('style') || '';
                        const color = cur.getAttribute('color') || '';
                        const className = cur.className || '';
                        if (
                            style.includes('#dc2626') || 
                            style.includes('rgb(220, 38, 38)') || 
                            style.includes('color: red') || 
                            color.includes('#dc2626') || 
                            color.toLowerCase() === 'red' || 
                            className.includes('edit-red')
                        ) {
                            return true;
                        }
                    }
                    cur = cur.parentNode;
                }
                return false;
            };

            if (formato === 'pdf') {
                const printWindow = window.open('', '_blank', 'width=900,height=700');
                printWindow.document.write(`
                    <!DOCTYPE html><html><head><meta charset="utf-8"><title>Voto ${proc.cnj}</title>
                    <style>
                        @page { size: A4 portrait; margin: 2.5cm; }
                        body { 
                            font-family: Arial, Helvetica, sans-serif; 
                            font-size: 12pt; 
                            line-height: 1.5; 
                            text-align: justify; 
                            color: #000; 
                            -webkit-print-color-adjust: exact !important; 
                            print-color-adjust: exact !important; 
                        }
                        p { margin: 0 0 1em 0; text-indent: ${recuo1aCm}cm; margin-left: ${recuoPCm}cm; }
                        blockquote, .citacao-longa { margin: 1.2em 0 1.2em 4.0cm !important; font-size: 10.5pt !important; line-height: 1.2 !important; text-indent: 0cm !important; }
                        /* Preservação explícita de correções em vermelho na impressão A4 */
                        .edit-red, [style*="#dc2626"], [style*="220, 38, 38"], [style*="color: red"], font[color="#dc2626"] { 
                            color: #dc2626 !important; 
                            font-weight: 500 !important; 
                            -webkit-print-color-adjust: exact !important; 
                            print-color-adjust: exact !important; 
                        }
                    </style></head><body>
                    <div style="text-align: center; font-weight: bold; margin-bottom: 2em;">
                        <div>PODER JUDICIÁRIO DO ESTADO DA PARAÍBA</div>
                        <div>${appState.unidadeAtiva.toUpperCase()}</div>
                        <div style="margin-top: 0.5em;">PROCESSO Nº ${proc.cnj}</div>
                        <div style="font-size: 10pt; font-weight: normal; margin-top: 0.25em;">Relator(a): ${proc.relator} | ${proc.tipoPoloAtivo}: ${proc.recorrente} x ${proc.tipoPoloPassivo}: ${proc.recorrido}</div>
                    </div>
                    <hr style="border: 0; border-top: 1px solid #000; margin-bottom: 1.5em;" />
                    <div>${htmlConteudo}</div>
                    </body></html>
                `);
                printWindow.document.close();
                printWindow.focus();
                setTimeout(() => printWindow.print(), 500);
                AdaptiveEngine.recordAction('exportar_pdf');
                exibirToast("Janela de impressão A4 gerada com correções preservadas!");
                return;
            }

            if (formato === 'docx') {
                const zip = new JSZip();
                zip.file("[Content_Types].xml", `<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types"><Default Extension="rels" ContentType="application/vnd.openxmlformats-package.relationships+xml"/><Default Extension="xml" ContentType="application/xml"/><Override PartName="/word/document.xml" ContentType="application/vnd.openxmlformats-officedocument.wordprocessingml.document.main+xml"/></Types>`);
                zip.file("_rels/.rels", `<?xml version="1.0" encoding="UTF-8" standalone="yes"?><Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships"><Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument" Target="word/document.xml"/></Relationships>`);
                
                const tempDiv = document.createElement('div');
                tempDiv.innerHTML = htmlConteudo;
                let wml = "";

                // Converte elementos e trechos mantendo a tag <w:color w:val="DC2626"/> nas correções
                const extrairRunsWord = (elementoPai) => {
                    let runsXml = "";
                    const walker = document.createTreeWalker(elementoPai, NodeFilter.SHOW_TEXT, null, false);
                    let textNode;
                    while ((textNode = walker.nextNode())) {
                        const texto = textNode.textContent.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
                        if (!texto) continue;

                        const isRed = verificarCorVermelha(textNode, tempDiv);
                        let rPr = '<w:rFonts w:ascii="Arial" w:hAnsi="Arial"/><w:sz w:val="24"/>';
                        if (isRed) {
                            rPr += '<w:color w:val="DC2626"/><w:b/>';
                        }
                        runsXml += `<w:r><w:rPr>${rPr}</w:rPr><w:t xml:space="preserve">${texto}</w:t></w:r>`;
                    }
                    return runsXml;
                };

                const paragrafos = tempDiv.querySelectorAll('p, blockquote, div');
                if (paragrafos.length > 0) {
                    paragrafos.forEach(p => {
                        const runs = extrairRunsWord(p);
                        if (runs) {
                            const isCitacao = p.tagName.toLowerCase() === 'blockquote' || p.classList.contains('citacao-longa') || p.style.marginLeft === '4cm';
                            const ind = isCitacao ? '<w:ind w:left="2268" w:firstLine="0"/>' : `<w:ind w:firstLine="${Math.round(recuo1aCm * 567)}" w:left="${Math.round(recuoPCm * 567)}"/>`;
                            const spacing = isCitacao ? '<w:spacing w:lineRule="auto" w:line="288" w:after="120"/>' : '<w:spacing w:lineRule="auto" w:line="360" w:after="140"/>';
                            wml += `<w:p><w:pPr><w:jc w:val="both"/>${ind}${spacing}</w:pPr>${runs}</w:p>`;
                        }
                    });
                } else {
                    const runs = extrairRunsWord(tempDiv);
                    wml += `<w:p><w:pPr><w:jc w:val="both"/><w:ind w:firstLine="709"/><w:spacing w:lineRule="auto" w:line="360" w:after="140"/></w:pPr>${runs}</w:p>`;
                }

                zip.file("word/document.xml", `<?xml version="1.0" encoding="UTF-8" standalone="yes"?><w:document xmlns:w="http://schemas.openxmlformats.org/wordprocessingml/2006/main"><w:body><w:p><w:pPr><w:jc w:val="center"/></w:pPr><w:r><w:rPr><w:b/><w:sz w:val="24"/></w:rPr><w:t>PODER JUDICIÁRIO DO ESTADO DA PARAÍBA</w:t></w:r></w:p><w:p><w:pPr><w:jc w:val="center"/></w:pPr><w:r><w:rPr><w:b/><w:sz w:val="24"/></w:rPr><w:t>${appState.unidadeAtiva.toUpperCase()}</w:t></w:r></w:p><w:p><w:pPr><w:jc w:val="center"/></w:pPr><w:r><w:rPr><w:sz w:val="20"/></w:rPr><w:t>PROCESSO Nº ${proc.cnj}</w:t></w:r></w:p>${wml}<w:sectPr><w:pgSz w:w="11906" w:h="16838"/><w:pgMar w:top="1417" w:right="1417" w:bottom="1417" w:left="1417"/></w:sectPr></w:body></w:document>`);
                
                const blob = await zip.generateAsync({ type: "blob" });
                const a = document.createElement('a');
                a.href = URL.createObjectURL(blob);
                a.download = `VOTO_${cnjLimpo}_CORRIGIDO.docx`;
                a.click();
                AdaptiveEngine.recordAction('exportar_docx');
                exibirToast("Exportado como Word (.docx) com correções em vermelho!");
                return;
            }

            if (formato === 'odt') {
                const zip = new JSZip();
                zip.file("mimetype", "application/vnd.oasis.opendocument.text", { compression: "STORE" });
                zip.file("META-INF/manifest.xml", `<?xml version="1.0" encoding="UTF-8"?><manifest:manifest xmlns:manifest="urn:oasis:names:tc:opendocument:xmlns:manifest:1.0" manifest:version="1.2"><manifest:file-entry manifest:full-path="/" manifest:media-type="application/vnd.oasis.opendocument.text"/><manifest:file-entry manifest:full-path="content.xml" manifest:media-type="text/xml"/></manifest:manifest>`);
                
                const tempDiv = document.createElement('div');
                tempDiv.innerHTML = htmlConteudo;
                let odtXml = "";

                const processarSpansODT = (el) => {
                    let spans = "";
                    const walker = document.createTreeWalker(el, NodeFilter.SHOW_TEXT, null, false);
                    let textNode;
                    while ((textNode = walker.nextNode())) {
                        const texto = textNode.textContent.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
                        if (!texto) continue;
                        const isRed = verificarCorVermelha(textNode, tempDiv);
                        if (isRed) {
                            spans += `<text:span text:style-name="T_Red">${texto}</text:span>`;
                        } else {
                            spans += texto;
                        }
                    }
                    return spans;
                };

                const paragrafos = tempDiv.querySelectorAll('p, blockquote, div');
                if (paragrafos.length > 0) {
                    paragrafos.forEach(p => {
                        const inner = processarSpansODT(p);
                        if (inner) odtXml += `<text:p>${inner}</text:p>`;
                    });
                } else {
                    odtXml += `<text:p>${processarSpansODT(tempDiv)}</text:p>`;
                }

                zip.file("content.xml", `<?xml version="1.0" encoding="UTF-8"?><office:document-content xmlns:office="urn:oasis:names:tc:opendocument:xmlns:office:1.0" xmlns:text="urn:oasis:names:tc:opendocument:xmlns:text:1.0" xmlns:style="urn:oasis:names:tc:opendocument:xmlns:style:1.0" xmlns:fo="urn:oasis:names:tc:opendocument:xmlns:xsl-fo-compatible:1.0"><office:automatic-styles><style:style style:name="T_Red" style:family="text"><style:text-properties fo:color="#dc2626" fo:font-weight="bold"/></style:style></office:automatic-styles><office:body><office:text><text:p text:style-name="P_Center">PODER JUDICIÁRIO DA PARAÍBA</text:p><text:p text:style-name="P_Center">PROCESSO Nº ${proc.cnj}</text:p>${odtXml}</office:text></office:body></office:document-content>`);

                const blob = await zip.generateAsync({ type: "blob" });
                const a = document.createElement('a');
                a.href = URL.createObjectURL(blob);
                a.download = `VOTO_${cnjLimpo}_CORRIGIDO.odt`;
                a.click();
                AdaptiveEngine.recordAction('exportar_odt');
                exibirToast("Exportado como LibreOffice (.odt) com correções em vermelho!");
                return;
            }

            if (formato === 'doc') {
                const docHtml = `<html xmlns:o='urn:schemas-microsoft-com:office:office' xmlns:w='urn:schemas-microsoft-com:office:word'><head><meta charset="utf-8"><style>.edit-red { color: #dc2626 !important; font-weight: bold !important; }</style></head><body><div style="text-align:center;font-weight:bold;">PODER JUDICIÁRIO DA PARAÍBA<br>PROCESSO Nº ${proc.cnj}</div><br>${htmlConteudo}</body></html>`;
                const blob = new Blob(['\ufeff', docHtml], { type: 'application/msword;charset=utf-8' });
                const a = document.createElement('a');
                a.href = URL.createObjectURL(blob);
                a.download = `VOTO_${cnjLimpo}_CORRIGIDO.doc`;
                a.click();
                exibirToast("Exportado como Word 97-2003 (.doc) com correções!");
                return;
            }
        }

        /* ==========================================================================
           8. UTILITÁRIOS, DATAS, TEMA E EVENTOS DE INICIALIZAÇÃO
           ========================================================================== */
        function inicializarDatas() {
            atualizarRelogio();
            setInterval(atualizarRelogio, 1000);
            const hoje = new Date().toISOString().split('T')[0];
            if (!appState.sessaoData) appState.sessaoData = hoje;
            const inP = document.getElementById('input-data-pauta');
            if (inP) inP.value = appState.sessaoData;
            atualizarLabelDataPauta();
        }

        function atualizarRelogio() {
            const agora = new Date();
            const h = String(agora.getHours()).padStart(2, '0');
            const m = String(agora.getMinutes()).padStart(2, '0');
            const s = String(agora.getSeconds()).padStart(2, '0');
            const lbl = document.getElementById('lbl-hora-hoje');
            if (lbl) lbl.innerText = `${h}:${m}:${s}`;
        }

        function atualizarLabelDataPauta() {
            const p = appState.sessaoData.split('-');
            const lbl = document.getElementById('lbl-data-pauta');
            if (lbl && p.length === 3) lbl.innerText = `${p[2]}/${p[1]}/${p[0]}`;
        }

        function alternarTemaManual() {
            const isDark = document.documentElement.classList.contains('dark');
            if (isDark) {
                document.documentElement.setAttribute('data-theme', 'light');
                document.documentElement.classList.remove('dark');
                localStorage.setItem('tjpb_sev_theme', 'light');
            } else {
                document.documentElement.setAttribute('data-theme', 'dark');
                document.documentElement.classList.add('dark');
                localStorage.setItem('tjpb_sev_theme', 'dark');
            }
            if (window.lucide) lucide.createIcons();
            AdaptiveEngine.recordAction('alternar_tema', { tema: isDark ? 'light' : 'dark' });
        }

        function atualizarOpcoesRelatorHeader() {
            const selectRelator = document.getElementById('header-select-relator');
            if (!selectRelator) return;
            selectRelator.innerHTML = '';
            const lista = MAGISTRADOS_TJPB[appState.unidadeAtiva] || [];
            lista.forEach(m => {
                const opt = document.createElement('option');
                opt.value = m;
                opt.innerText = m;
                if (m === appState.relatorAtivo) opt.selected = true;
                selectRelator.appendChild(opt);
            });
            if (!lista.includes(appState.relatorAtivo) && lista.length > 0) {
                appState.relatorAtivo = lista[0];
                selectRelator.value = lista[0];
            }
        }

        function aoMudarUnidadeHeader(u) {
            appState.unidadeAtiva = u;
            atualizarOpcoesRelatorHeader();
            salvarEstadoLocal();
            renderizarTudo();
        }

        function aoMudarRelatorHeader(r) {
            appState.relatorAtivo = r;
            salvarEstadoLocal();
            renderizarTudo();
        }

        function aoMudarDataPauta(d) {
            if (!d) return;
            appState.sessaoData = d;
            atualizarLabelDataPauta();
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast(`Pauta alterada para ${d.split('-').reverse().join('/')}`);
        }

        function mudarModalidade(m) {
            appState.sessaoModalidade = m;
            document.getElementById('lbl-modalidade').innerText = m;
            salvarEstadoLocal();
        }

        function toggleSidebar() {
            const el = document.getElementById('aside-pauta');
            if (el) el.classList.toggle('-translate-x-full');
        }

        function toggleAiDrawer() {
            const d = document.getElementById('ai-drawer');
            if (d) d.classList.toggle('translate-x-full');
        }

        function abrirModalCentralLinks() {
            document.getElementById('modal-central-links')?.classList.remove('hidden');
            if (window.lucide) lucide.createIcons();
            AdaptiveEngine.recordAction('abrir_central_links');
        }

        function abrirModalAprendizado() {
            AdaptiveEngine.renderModalContent();
            document.getElementById('modal-aprendizado-adaptativo')?.classList.remove('hidden');
            if (window.lucide) lucide.createIcons();
        }

        function fecharModal(id) {
            document.getElementById(id)?.classList.add('hidden');
        }

        function dispensarSugestaoAtual() {
            AdaptiveEngine.dismissCurrentSuggestion();
        }

        function togglePrioridade(procId) {
            const proc = appState.processos.find(p => p.id === procId);
            if (!proc) return;
            proc.prioridade = proc.prioridade ? null : 'Legal';
            salvarEstadoLocal();
            renderizarListaPauta();
            exibirToast(proc.prioridade ? 'Prioridade Legal Ativada' : 'Prioridade Removida');
        }

        function toggleSustentacao(procId) {
            const proc = appState.processos.find(p => p.id === procId);
            if (!proc) return;
            proc.sustentacaoOral = !proc.sustentacaoOral;
            salvarEstadoLocal();
            renderizarListaPauta();
            exibirToast(proc.sustentacaoOral ? 'Sustentação Oral Marcada' : 'Sustentação Desmarcada');
        }

        function navegarProcessoRelativo(dir) {
            const procs = getProcessosContextoAtual();
            if (!procs.length) return;
            const idx = procs.findIndex(p => p.id === appState.processoSelecionadoId);
            let n = idx + dir;
            if (n < 0) n = procs.length - 1;
            if (n >= procs.length) n = 0;
            selecionarProcesso(procs[n].id);
        }

        function filtrarPauta(filtro) {
            appState.filtroPauta = filtro;
            ['Todos', 'Pendentes', 'Julgados', 'Prioridades'].forEach(f => {
                const btn = document.getElementById(`filter-${f}`);
                if (btn) btn.className = (f === filtro) ? 'pill-filter active' : 'pill-filter';
            });
            renderizarListaPauta();
            AdaptiveEngine.recordAction('filtrar_pauta', { filtro });
        }

        function alternarModoOrdem(modo) {
            appState.modoOrdem = modo;
            document.getElementById('btn-ordem-orig').className = modo === 'original' ? "seg-btn active" : "seg-btn";
            document.getElementById('btn-ordem-sess').className = modo === 'sessao' ? "seg-btn active" : "seg-btn";
            renderizarListaPauta();
            if (appState.processoSelecionadoId) renderizarPainelLeitura();
        }

        function alternarSelecionarTodos(checked) {
            getProcessosContextoAtual().forEach(p => p.selecionado = checked);
            renderizarListaPauta();
            atualizarBarraAcoesEmLote();
        }

        function alternarSelecaoProcesso(procId, checked) {
            const proc = appState.processos.find(p => p.id === procId);
            if (proc) {
                proc.selecionado = checked;
                renderizarListaPauta();
                atualizarBarraAcoesEmLote();
            }
        }

        function desmarcarTodosProcessos() {
            getProcessosContextoAtual().forEach(p => p.selecionado = false);
            renderizarListaPauta();
            atualizarBarraAcoesEmLote();
        }

        function atualizarBarraAcoesEmLote() {
            const barra = document.getElementById('barra-acoes-em-lote');
            const lbl = document.getElementById('lbl-total-selecionados');
            if (!barra || !lbl) return;
            const sel = getProcessosContextoAtual().filter(p => p.selecionado);
            if (sel.length > 0) {
                lbl.innerText = `${sel.length} selecionado(s)`;
                barra.classList.remove('hidden');
            } else {
                barra.classList.add('hidden');
            }
        }

        function aplicarStatusEmLote(status, desfecho = '') {
            const sel = getProcessosContextoAtual().filter(p => p.selecionado);
            if (!sel.length) return;
            sel.forEach(p => {
                p.status = status;
                p.desfecho = desfecho || status;
                p.resultado = desfecho || status;
                p.votado = status === 'Julgado';
                p.selecionado = false;
            });
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast(`${sel.length} processo(s) atualizado(s) em lote.`);
        }

        function excluirProcessosEmLote() {
            const sel = getProcessosContextoAtual().filter(p => p.selecionado);
            if (!sel.length) return;
            const ids = new Set(sel.map(p => p.id));
            appState.processos = appState.processos.filter(p => !ids.has(p.id));
            if (ids.has(appState.processoSelecionadoId)) appState.processoSelecionadoId = null;
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast(`${sel.length} processo(s) removido(s).`);
        }

        function excluirProcesso(procId) {
            appState.processos = appState.processos.filter(p => p.id !== procId);
            if (appState.processoSelecionadoId === procId) appState.processoSelecionadoId = null;
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast('Processo excluído.');
        }

        function copiarNumeroCNJAtivo() {
            const txt = document.getElementById('title-processo-cnj').innerText;
            if (txt) {
                const temp = document.createElement('textarea');
                temp.value = txt;
                document.body.appendChild(temp);
                temp.select();
                document.execCommand('copy');
                document.body.removeChild(temp);
                exibirToast(`CNJ copiado: ${txt}`);
            }
        }

        function atualizarContadores() {
            const procs = getProcessosContextoAtual();
            const total = procs.length;
            const julgados = procs.filter(p => p.votado).length;
            const pendentes = procs.filter(p => !p.votado).length;
            const sustentacoes = procs.filter(p => p.sustentacaoOral).length;

            document.getElementById('cnt-gabinete').innerText = total;
            document.getElementById('cnt-julgados').innerText = julgados;
            document.getElementById('cnt-pendentes').innerText = pendentes;
            document.getElementById('cnt-sustentacao').innerText = sustentacoes;

            const pct = total > 0 ? Math.round((julgados / total) * 100) : 0;
            document.getElementById('bar-progresso').style.width = `${pct}%`;
            document.getElementById('lbl-progresso-pct').innerText = `${pct}%`;
        }

        function abrirModalEditar(procId) {
            appState.editProcessoIdModal = procId;
            const proc = appState.processos.find(p => p.id === procId);
            if (!proc) return;

            document.getElementById('edit-cnj').value = proc.cnj;
            document.getElementById('edit-classe').value = proc.classeProcessual || '';
            document.getElementById('edit-tipo-ativo').value = proc.tipoPoloAtivo || 'Ativo';
            document.getElementById('edit-tipo-passivo').value = proc.tipoPoloPassivo || 'Passivo';
            document.getElementById('edit-recorrente').value = proc.recorrente || '';
            document.getElementById('edit-recorrido').value = proc.recorrido || '';

            const selRel = document.getElementById('edit-relator');
            selRel.innerHTML = (MAGISTRADOS_TJPB[appState.unidadeAtiva] || []).map(m =>
                `<option value="${m}" ${m === proc.relator ? 'selected' : ''}>${m}</option>`
            ).join('');

            const selAss = document.getElementById('edit-assessor');
            const pool = [...ASSESSORES_GABINETE, ...(appState.assessoresCustom || [])];
            selAss.innerHTML = pool.map(a =>
                `<option value="${a.sigla}" ${a.sigla === proc.assessor ? 'selected' : ''}>${a.sigla} (${a.nome})</option>`
            ).join('');

            document.getElementById('modal-editar-processo').classList.remove('hidden');
        }

        function salvarModalEditarProcesso() {
            const proc = appState.processos.find(p => p.id === appState.editProcessoIdModal);
            if (proc) {
                proc.cnj = document.getElementById('edit-cnj').value;
                proc.classeProcessual = document.getElementById('edit-classe').value;
                proc.tipoPoloAtivo = document.getElementById('edit-tipo-ativo').value;
                proc.tipoPoloPassivo = document.getElementById('edit-tipo-passivo').value;
                proc.recorrente = document.getElementById('edit-recorrente').value;
                proc.recorrido = document.getElementById('edit-recorrido').value;
                proc.partes = `${proc.recorrente} x ${proc.recorrido}`;
                proc.relator = document.getElementById('edit-relator').value;
                proc.assessor = document.getElementById('edit-assessor').value;
                salvarEstadoLocal();
                renderizarTudo();
            }
            fecharModal('modal-editar-processo');
        }

        function abrirModalConfigOrdem() {
            document.getElementById('input-num-inicio-sessao').value = appState.numInicioSessao || 35;
            document.getElementById('modal-config-ordem').classList.remove('hidden');
        }

        function aoMudarNumInicioDireto(val) {
            let n = parseInt(val, 10);
            if (isNaN(n) || n < 1) n = 1;
            if (n > 500) n = 500;
            appState.numInicioSessao = n;

            const inputHeader = document.getElementById('input-ordem-inicio-header');
            if (inputHeader) inputHeader.value = n;
            const inputModal = document.getElementById('input-num-inicio-sessao');
            if (inputModal) inputModal.value = n;

            recalcularSequenciaSessao();
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast(`Ordem da sessão iniciada no nº ${n} (suporte a até 500 processos)`);
        }

        function salvarModalConfigOrdem() {
            let n = parseInt(document.getElementById('input-num-inicio-sessao').value, 10);
            if (isNaN(n) || n < 1) n = 1;
            if (n > 500) n = 500;
            appState.numInicioSessao = n;

            const inputHeader = document.getElementById('input-ordem-inicio-header');
            if (inputHeader) inputHeader.value = n;

            recalcularSequenciaSessao();
            salvarEstadoLocal();
            renderizarTudo();
            fecharModal('modal-config-ordem');
            exibirToast(`Sessão configurada a partir do nº ${appState.numInicioSessao}`);
        }

        function recalcularSequenciaSessao() {
            const n = appState.numInicioSessao || 35;
            const procs = getProcessosContextoAtual();
            procs.forEach((p, idx) => { 
                p.seqSessao = n + idx; 
            });
        }

        function salvarModalConfigOrdem() {
            appState.numInicioSessao = parseInt(document.getElementById('input-num-inicio-sessao').value) || 35;
            const procs = getProcessosContextoAtual();
            procs.forEach((p, idx) => { p.seqSessao = appState.numInicioSessao + idx; });
            salvarEstadoLocal();
            renderizarTudo();
            fecharModal('modal-config-ordem');
            exibirToast(`Sessão configurada a partir do nº ${appState.numInicioSessao}`);
        }

        function abrirModalNovaPauta() {
            document.getElementById('input-nova-pauta-data').value = appState.sessaoData;
            document.getElementById('modal-nova-pauta').classList.remove('hidden');
        }

        function confirmarCriarNovaPauta() {
            const d = document.getElementById('input-nova-pauta-data').value;
            if (!d) return;
            aoMudarDataPauta(d);
            fecharModal('modal-nova-pauta');
        }

        function gerarAtaJulgamento() {
            const procs = getProcessosContextoAtual();
            let text = `ATA DE SESSÃO DE JULGAMENTO\n${appState.unidadeAtiva.toUpperCase()}\nData: ${appState.sessaoData.split('-').reverse().join('/')}\nRelatoria: ${appState.relatorAtivo}\n\n`;
            procs.forEach((p, idx) => {
                text += `${idx+1}. PROCESSO CNJ: ${p.cnj} (${p.classeProcessual})\n   Assessor(a): ${p.assessor}\n   Partes: ${p.partes}\n   Resultado: ${p.desfecho || p.status}\n\n`;
            });
            document.getElementById('conteudo-ata-texto').innerText = text;
            document.getElementById('modal-ata-julgamento').classList.remove('hidden');
        }

        function copiarAtaParaClipboard() {
            const txt = document.getElementById('conteudo-ata-texto').innerText;
            const temp = document.createElement('textarea');
            temp.value = txt;
            document.body.appendChild(temp);
            temp.select();
            document.execCommand('copy');
            document.body.removeChild(temp);
            fecharModal('modal-ata-julgamento');
            exibirToast('Ata copiada para transferência!');
        }

        function salvarEstadoLocal() {
            try { localStorage.setItem(STORAGE_KEY, JSON.stringify(appState)); } catch(e) {}
        }

        function carregarEstadoLocal() {
            try {
                const s = localStorage.getItem(STORAGE_KEY);
                if (s) Object.assign(appState, JSON.parse(s));
                const inpHeader = document.getElementById('input-ordem-inicio-header');
                if (inpHeader) inpHeader.value = appState.numInicioSessao || 35;
                const inpModal = document.getElementById('input-num-inicio-sessao');
                if (inpModal) inpModal.value = appState.numInicioSessao || 35;
            } catch(e) {}
        }

        function exportarSessaoPirPackage() {
            const blob = new Blob([JSON.stringify({ versao: "SEV-Relatoria-2026", appState }, null, 2)], { type: 'application/json' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `SESSAO_${appState.sessaoData}.pir`;
            a.click();
            exibirToast('Backup da sessão exportado (.pir)!');
        }

        function carregarSessaoPirPackage(input) {
            const file = input.files[0];
            if (!file) return;
            const r = new FileReader();
            r.onload = (e) => {
                try {
                    const data = JSON.parse(e.target.result);
                    if (data.appState) Object.assign(appState, data.appState);
                    atualizarOpcoesRelatorHeader();
                    salvarEstadoLocal();
                    renderizarTudo();
                    exibirToast('Sessão restaurada com sucesso!');
                } catch(err) {
                    exibirToast('Erro ao carregar arquivo de sessão.');
                }
            };
            r.readAsText(file);
            input.value = '';
        }

        function limparCacheMemoria() {
            appState.processos = [];
            appState.processoSelecionadoId = null;
            salvarEstadoLocal();
            renderizarTudo();
            exibirToast('Pauta e processos limpos com sucesso.');
        }

        function exibirToast(msg) {
            const c = document.getElementById('toast-container');
            if (!c) return;
            const t = document.createElement('div');
            t.className = 'bg-surface text-text-primary border border-border px-4 py-3 rounded-2xl text-xs font-bold shadow-2xl flex items-center gap-2.5 pointer-events-auto transition-all transform translate-y-0';
            t.innerHTML = `<i data-lucide="check-circle" class="w-4 h-4 text-success"></i><span>${msg}</span>`;
            c.appendChild(t);
            if (window.lucide) lucide.createIcons();
            setTimeout(() => {
                t.style.opacity = '0';
                t.style.transform = 'translateY(10px)';
                setTimeout(() => t.remove(), 300);
            }, 3500);
        }

        async function enviarMensagemGemini() {
            const inputEl = document.getElementById('ai-user-input');
            const text = inputEl.value.trim();
            if (!text) return;
            inputEl.value = '';

            const chatBox = document.getElementById('ai-chat-history');
            chatBox.innerHTML += `<div class="bg-primary text-white p-2.5 rounded-2xl self-end text-xs max-w-[85%] ml-auto font-medium shadow-2xs">${text}</div>`;
            chatBox.innerHTML += `<div id="ai-typing" class="bg-surface-soft text-text-muted p-2.5 rounded-2xl text-xs max-w-[85%] border border-border animate-pulse">Consultando acervo e minutas...</div>`;
            chatBox.scrollTop = chatBox.scrollHeight;

            const pAtivo = appState.processos.find(p => p.id === appState.processoSelecionadoId);
            let context = "Você é o assistente judiciário oficial do Tribunal de Justiça da Paraíba (TJPB).\n";
            if (pAtivo && pAtivo.documentos[0]) {
                context += `Processo: ${pAtivo.cnj} (${pAtivo.classeProcessual})\nAssessor(a): ${pAtivo.assessor}\nPartes: ${pAtivo.partes}\nTexto: ${pAtivo.documentos[0].conteudoTexto.replace(/<[^>]+>/g, ' ').substring(0, 4000)}\n`;
            }

            try {
                const apiKey = localStorage.getItem(GEMINI_KEY_STORAGE) || "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;
                const payload = {
                    contents: [{ parts: [{ text: `${context}\nUsuário: ${text}` }] }]
                };
                const res = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const data = await res.json();
                document.getElementById('ai-typing')?.remove();
                const reply = data.candidates?.[0]?.content?.parts?.[0]?.text || "Não foi possível gerar a resposta. Verifique a chave de API.";
                chatBox.innerHTML += `<div class="bg-surface-soft text-text-primary p-2.5 rounded-2xl border border-border text-xs leading-relaxed max-w-[90%]">${reply.replace(/\n/g, '<br>')}</div>`;
            } catch(err) {
                document.getElementById('ai-typing')?.remove();
                chatBox.innerHTML += `<div class="bg-danger/10 text-danger p-2.5 rounded-2xl border border-danger/30 text-xs">Erro na conexão com o serviço de IA.</div>`;
            }
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function renderizarTudo() {
            renderizarListaPauta();
            renderizarPainelLeitura();
            atualizarContadores();
            atualizarBarraAcoesEmLote();
            if (window.lucide) lucide.createIcons();
        }

        /* Ciclo de Vida Inicial */
        window.addEventListener('DOMContentLoaded', () => {
            inicializarDatas();
            carregarEstadoLocal();
            atualizarOpcoesRelatorHeader();
            AdaptiveEngine.init();
            renderizarTudo();

            const aiInput = document.getElementById('ai-user-input');
            if (aiInput) {
                aiInput.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter' && !e.shiftKey) {
                        e.preventDefault();
                        enviarMensagemGemini();
                    }
                });
            }

            document.addEventListener('selectionchange', () => {
                const sel = window.getSelection();
                if (sel.rangeCount > 0) {
                    const node = sel.anchorNode;
                    const editor = document.getElementById('editor-conteudo-voto');
                    if (editor && editor.contains(node)) {
                        editorSavedRange = sel.getRangeAt(0);
                    }
                }
            });
        });
    </script>
</body>
</html>
