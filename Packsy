<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Verificación Packsy — Escaneo NFC</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Fuente Inter -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', 'sans-serif'],
          },
          boxShadow: {
            'soft-xl': '0 20px 45px -15px rgba(15, 23, 42, 0.12), 0 4px 16px -4px rgba(15, 23, 42, 0.06)',
            'card': '0 6px 20px -4px rgba(15, 23, 42, 0.06), 0 2px 6px -1px rgba(15, 23, 42, 0.04)',
            'card-hover': '0 14px 28px -6px rgba(15, 23, 42, 0.11), 0 4px 10px -2px rgba(15, 23, 42, 0.05)',
          }
        },
      },
    };
  </script>
  <style>
    /* Fondo dinámico con profundidad sutil */
    body {
      background-color: #f1f5f9;
      background-image:
        radial-gradient(at 0% 0%, rgba(14, 165, 233, 0.11) 0px, transparent 50%),
        radial-gradient(at 100% 0%, rgba(16, 185, 129, 0.10) 0px, transparent 50%),
        radial-gradient(at 50% 100%, rgba(99, 102, 241, 0.08) 0px, transparent 55%);
    }

    /* Páginas verdaderamente independientes con transición de entrada */
    .page-view {
      display: none;
    }
    .page-view.active {
      display: flex;
      animation: pageEnter 0.34s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    @keyframes pageEnter {
      from {
        opacity: 0;
        transform: translateY(12px) scale(0.985);
      }
      to {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
    }

    /* Onda de escaneo NFC */
    @keyframes nfcRipple {
      0% {
        transform: scale(0.85);
        opacity: 0.75;
      }
      100% {
        transform: scale(2.1);
        opacity: 0;
      }
    }
    .animate-nfc-ripple {
      animation: nfcRipple 2s cubic-bezier(0.22, 1, 0.36, 1) infinite;
    }

    /* Animación al completar verificación */
    @keyframes popIn {
      0% {
        opacity: 0;
        transform: scale(0.95) translateY(6px);
      }
      100% {
        opacity: 1;
        transform: scale(1) translateY(0);
      }
    }
    .animate-pop-in {
      animation: popIn 0.28s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    button, a, input {
      -webkit-tap-highlight-color: transparent;
    }
  </style>
</head>
<body class="text-slate-900 font-sans antialiased min-h-screen flex flex-col items-center justify-start sm:justify-center p-0 sm:p-6">

  <!-- Contenedor principal tipo dispositivo móvil con relieve premium -->
  <main class="w-full max-w-md bg-white/95 backdrop-blur-md sm:rounded-[2rem] sm:shadow-soft-xl sm:border sm:border-white min-h-screen sm:min-h-[710px] flex flex-col justify-between overflow-hidden relative">

    <!-- Barra superior fija con espacio para el logotipo de Packsy e indicador NFC -->
    <header class="px-6 pt-6 pb-4 border-b border-slate-200/70 bg-gradient-to-b from-slate-50/90 to-white/80 backdrop-blur-md sticky top-0 z-20">
      <div class="flex items-center justify-between gap-3">
        <!-- Espacio destacado y editable para el logotipo de Packsy -->
        <div
          class="inline-flex items-center gap-2.5 px-3.5 py-2 rounded-2xl bg-gradient-to-br from-slate-50 to-slate-100/80 border border-dashed border-slate-300/90 shadow-2xs select-none"
          title="Espacio reservado para el logotipo oficial de Packsy"
        >
          <div class="w-8 h-8 rounded-xl bg-gradient-to-br from-slate-800 to-slate-950 text-white flex items-center justify-center shadow-sm">
            <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M6 7h12l1 13H5L6 7z" />
              <path d="M9 7V5a3 3 0 0 1 6 0v2" />
            </svg>
          </div>
          <div class="flex flex-col leading-none">
            <span class="font-extrabold tracking-widest text-sm text-slate-900">PACKSY</span>
            <span class="text-[10px] font-medium text-slate-400 mt-0.5">Espacio para logo</span>
          </div>
        </div>

        <!-- Estado NFC dinámico -->
        <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full bg-emerald-50/90 text-emerald-700 border border-emerald-200/80 text-xs font-semibold shadow-2xs">
          <span class="relative flex h-2.5 w-2.5">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-emerald-500"></span>
          </span>
          <span>Chip NFC leído</span>
        </div>
      </div>
    </header>

    <!-- Contenedor de Páginas Independientes -->
    <div class="flex-1 flex flex-col relative">

      <!-- =================================================================== -->
      <!-- PÁGINA 1 (ENTRADA DIRECTA): VERIFICAR AUTENTICIDAD (#verificar)     -->
      <!-- =================================================================== -->
      <section id="page-verificar" class="page-view active flex-1 flex-col justify-between px-6 py-6" aria-labelledby="title-verificar">
        <div>
          <!-- Barra superior de navegación con botón Inicio visible -->
          <div class="flex items-center justify-between mb-5">
            <button
              type="button"
              onclick="navigateTo('inicio')"
              class="inline-flex items-center gap-2 px-4 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200/80 active:scale-[0.97] text-slate-800 font-semibold text-sm border border-slate-200/80 shadow-2xs transition-all"
            >
              <svg class="w-4 h-4 text-slate-700" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
                <polyline points="9 22 9 12 15 12 15 22" />
              </svg>
              <span>Inicio</span>
            </button>

            <span class="text-xs font-medium text-slate-400 bg-slate-50 px-2.5 py-1 rounded-lg border border-slate-200/60">
              Paso 1 • Autenticación
            </span>
          </div>

          <!-- Banner interactivo de Lectura NFC detectada -->
          <div class="mb-6 p-4 rounded-2xl bg-gradient-to-r from-slate-900 via-slate-800 to-slate-900 text-white shadow-card relative overflow-hidden">
            <div class="flex items-center gap-3.5 relative z-10">
              <!-- Icono con onda NFC -->
              <div class="relative w-11 h-11 shrink-0 rounded-2xl bg-white/10 border border-white/15 flex items-center justify-center">
                <span class="absolute inset-0 rounded-2xl border border-emerald-400/60 animate-nfc-ripple"></span>
                <svg class="w-5 h-5 text-emerald-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M6 8.32a7.43 7.43 0 0 1 0 7.36" />
                  <path d="M9.46 6.21a11.76 11.76 0 0 1 0 11.58" />
                  <path d="M12.91 4.1a15.91 15.91 0 0 1 .01 15.8" />
                  <path d="M16.37 2a20.16 20.16 0 0 1 0 20" />
                </svg>
              </div>
              <div class="flex-1 min-w-0">
                <div class="flex items-center gap-1.5">
                  <span class="text-xs font-bold uppercase tracking-wider text-emerald-400">Lectura NFC detectada</span>
                </div>
                <p id="nfc-banner-subtitle" class="text-xs text-slate-300 mt-0.5 leading-snug">
                  Código único extraído automáticamente del chip de tu bolso.
                </p>
              </div>
            </div>

            <!-- Acción discreta para simular otro escaneo NFC único -->
            <div class="mt-3 pt-2.5 border-t border-white/10 flex items-center justify-between gap-2 relative z-10">
              <span class="text-[11px] text-slate-400">¿Probar otra lectura?</span>
              <button
                type="button"
                onclick="simulateAnotherNfcScan()"
                class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-lg bg-white/10 hover:bg-white/20 active:scale-[0.97] text-white text-xs font-medium transition-all"
              >
                <svg class="w-3.5 h-3.5 text-sky-300" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M21 12a9 9 0 0 0-9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" />
                  <path d="M3 3v5h5" />
                  <path d="M3 12a9 9 0 0 0 9 9 9.75 9.75 0 0 0 6.74-2.74L21 16" />
                  <path d="M16 16h5v5" />
                </svg>
                <span>Simular otro escaneo NFC</span>
              </button>
            </div>
          </div>

          <h1 id="title-verificar" class="text-2xl font-extrabold tracking-tight text-slate-900">
            Verificar autenticidad
          </h1>
          <p class="mt-1 text-sm text-slate-600">
            El código único de tu bolso se cargó desde el chip NFC. También puedes editarlo manualmente.
          </p>

          <!-- Campo de código y botón Verificar -->
          <div class="mt-5 space-y-3.5">
            <div>
              <div class="flex items-center justify-between mb-1.5">
                <label for="code-input" class="text-xs font-bold uppercase tracking-wider text-slate-500">
                  Código único del bolso
                </label>
                <button
                  type="button"
                  onclick="restoreDefaultDemo()"
                  class="text-xs font-semibold text-sky-600 hover:text-sky-700 py-0.5 px-1.5 rounded-md hover:bg-sky-50 transition-colors"
                >
                  Cargar PACKSY-DEMO
                </button>
              </div>
              <div class="relative">
                <input
                  id="code-input"
                  type="text"
                  autocomplete="off"
                  autocapitalize="characters"
                  spellcheck="false"
                  placeholder="Ingresa el código único"
                  class="w-full pl-4 pr-11 py-3.5 rounded-2xl bg-slate-50/90 border-2 border-slate-200 text-slate-900 placeholder:text-slate-400 font-mono text-base font-bold tracking-wider uppercase focus:outline-none focus:ring-4 focus:ring-slate-900/10 focus:border-slate-900 focus:bg-white transition-all"
                />
                <button
                  type="button"
                  onclick="clearCodeInput()"
                  title="Borrar código"
                  aria-label="Borrar código"
                  class="absolute right-3 top-1/2 -translate-y-1/2 p-1.5 rounded-xl text-slate-400 hover:text-slate-700 hover:bg-slate-200/60 transition-colors"
                >
                  <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                    <line x1="18" y1="6" x2="6" y2="18" />
                    <line x1="6" y1="6" x2="18" y2="18" />
                  </svg>
                </button>
              </div>
            </div>

            <button
              type="button"
              onclick="verifyCode()"
              class="w-full py-3.5 px-5 rounded-2xl bg-gradient-to-r from-slate-900 via-slate-800 to-slate-900 hover:from-slate-800 hover:to-slate-950 active:scale-[0.985] text-white font-semibold text-base shadow-card hover:shadow-card-hover transition-all flex items-center justify-center gap-2.5"
            >
              <svg class="w-5 h-5 text-emerald-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
                <path d="m9 12 2 2 4-4" />
              </svg>
              <span>Verificar</span>
            </button>
          </div>

          <!-- Resultado de verificación -->
          <div id="verify-result" class="mt-5" role="status" aria-live="polite"></div>

          <!-- Accesos rápidos a las otras páginas independientes tras verificar -->
          <div class="mt-5 grid grid-cols-2 gap-3">
            <button
              type="button"
              onclick="navigateTo('detalles')"
              class="p-3.5 rounded-2xl bg-slate-50 hover:bg-slate-100/90 active:scale-[0.98] border border-slate-200/80 text-left transition-all flex items-center gap-2.5 group"
            >
              <div class="w-9 h-9 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center shrink-0">
                <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z" />
                  <polyline points="14 2 14 8 20 8" />
                </svg>
              </div>
              <div class="min-w-0">
                <span class="block text-xs font-bold text-slate-900 truncate">Ver ficha</span>
                <span class="block text-[11px] text-slate-500 truncate">Mi Packsy</span>
              </div>
            </button>

            <button
              type="button"
              onclick="navigateTo('funcionalidades')"
              class="p-3.5 rounded-2xl bg-slate-50 hover:bg-slate-100/90 active:scale-[0.98] border border-slate-200/80 text-left transition-all flex items-center gap-2.5 group"
            >
              <div class="w-9 h-9 rounded-xl bg-sky-100 text-sky-700 flex items-center justify-center shrink-0">
                <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <rect width="7" height="7" x="3" y="3" rx="1.5" />
                  <rect width="7" height="7" x="14" y="3" rx="1.5" />
                  <rect width="7" height="7" x="14" y="14" rx="1.5" />
                  <rect width="7" height="7" x="3" y="14" rx="1.5" />
                </svg>
              </div>
              <div class="min-w-0">
                <span class="block text-xs font-bold text-slate-900 truncate">Funciones</span>
                <span class="block text-[11px] text-slate-500 truncate">Descubrir</span>
              </div>
            </button>
          </div>
        </div>

        <!-- Aclaración discreta sobre base de datos oficial -->
        <div class="mt-6 p-3.5 rounded-2xl bg-slate-50/90 border border-slate-200/70">
          <p class="text-[11px] text-slate-500 leading-relaxed text-center">
            Prototipo interactivo con código único de prueba. La verificación en tiempo real quedará conectada a la base de datos oficial de Packsy.
          </p>
        </div>
      </section>

      <!-- =================================================================== -->
      <!-- PÁGINA 2: INICIO (#inicio)                                          -->
      <!-- =================================================================== -->
      <section id="page-inicio" class="page-view flex-1 flex-col justify-between px-6 py-6" aria-labelledby="title-inicio">
        <div>
          <!-- Encabezado principal con espacio destacado para logotipo -->
          <div class="p-6 rounded-3xl bg-gradient-to-br from-slate-900 via-slate-850 to-slate-800 text-white shadow-card mb-6 relative overflow-hidden">
            <div class="relative z-10">
              <div class="inline-flex items-center gap-2 px-3 py-1 rounded-xl bg-white/10 border border-dashed border-white/25 text-xs font-medium text-slate-200 mb-4">
                <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
                <span>Espacio destacado • Logotipo Packsy</span>
              </div>
              <h1 id="title-inicio" class="text-2xl sm:text-[26px] font-extrabold tracking-tight text-white">
                Verificación Packsy
              </h1>
              <p class="mt-1.5 text-sm text-slate-300 leading-relaxed">
                Verifica tu bolsa y descubre sus funciones
              </p>
            </div>
            <!-- Elemento decorativo sutil de profundidad -->
            <div class="w-36 h-36 rounded-full bg-sky-500/15 blur-2xl absolute -right-8 -bottom-10 pointer-events-none"></div>
          </div>

          <!-- Tres tarjetas grandes clicables hacia páginas independientes -->
          <div class="space-y-3.5">
            <!-- Tarjeta 1: Verificar autenticidad -->
            <button
              type="button"
              onclick="navigateTo('verificar')"
              class="w-full text-left p-5 rounded-2xl bg-white hover:bg-slate-50/90 active:scale-[0.985] border border-slate-200/90 shadow-card hover:shadow-card-hover transition-all duration-200 flex items-center gap-4 group"
            >
              <div class="w-13 h-13 shrink-0 rounded-2xl bg-gradient-to-br from-slate-800 to-slate-950 text-white flex items-center justify-center p-3.5 shadow-sm group-hover:scale-105 transition-transform">
                <svg class="w-6 h-6 text-emerald-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
                  <path d="m9 12 2 2 4-4" />
                </svg>
              </div>
              <div class="flex-1 min-w-0">
                <h2 class="text-base font-bold text-slate-900">
                  Verificar autenticidad
                </h2>
                <p class="text-xs text-slate-500 mt-0.5">
                  Valida el código NFC único de tu bolsa
                </p>
              </div>
              <div class="w-8 h-8 rounded-full bg-slate-100 group-hover:bg-slate-900 group-hover:text-white text-slate-500 flex items-center justify-center transition-colors shrink-0">
                <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                  <path d="m9 18 6-6-6-6" />
                </svg>
              </div>
            </button>

            <!-- Tarjeta 2: Funcionalidades -->
            <button
              type="button"
              onclick="navigateTo('funcionalidades')"
              class="w-full text-left p-5 rounded-2xl bg-white hover:bg-slate-50/90 active:scale-[0.985] border border-slate-200/90 shadow-card hover:shadow-card-hover transition-all duration-200 flex items-center gap-4 group"
            >
              <div class="w-13 h-13 shrink-0 rounded-2xl bg-gradient-to-br from-sky-500 to-blue-600 text-white flex items-center justify-center p-3.5 shadow-sm group-hover:scale-105 transition-transform">
                <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <rect width="7" height="7" x="3" y="3" rx="1.5" />
                  <rect width="7" height="7" x="14" y="3" rx="1.5" />
                  <rect width="7" height="7" x="14" y="14" rx="1.5" />
                  <rect width="7" height="7" x="3" y="14" rx="1.5" />
                </svg>
              </div>
              <div class="flex-1 min-w-0">
                <h2 class="text-base font-bold text-slate-900">
                  Funcionalidades
                </h2>
                <p class="text-xs text-slate-500 mt-0.5">
                  Conoce sus beneficios térmicos y smart
                </p>
              </div>
              <div class="w-8 h-8 rounded-full bg-slate-100 group-hover:bg-sky-600 group-hover:text-white text-slate-500 flex items-center justify-center transition-colors shrink-0">
                <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                  <path d="m9 18 6-6-6-6" />
                </svg>
              </div>
            </button>

            <!-- Tarjeta 3: Detalles de mi Packsy -->
            <button
              type="button"
              onclick="navigateTo('detalles')"
              class="w-full text-left p-5 rounded-2xl bg-white hover:bg-slate-50/90 active:scale-[0.985] border border-slate-200/90 shadow-card hover:shadow-card-hover transition-all duration-200 flex items-center gap-4 group"
            >
              <div class="w-13 h-13 shrink-0 rounded-2xl bg-gradient-to-br from-emerald-500 to-teal-600 text-white flex items-center justify-center p-3.5 shadow-sm group-hover:scale-105 transition-transform">
                <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z" />
                  <polyline points="14 2 14 8 20 8" />
                  <line x1="16" y1="13" x2="8" y2="13" />
                  <line x1="16" y1="17" x2="8" y2="17" />
                </svg>
              </div>
              <div class="flex-1 min-w-0">
                <h2 class="text-base font-bold text-slate-900">
                  Detalles de mi Packsy
                </h2>
                <p class="text-xs text-slate-500 mt-0.5">
                  Ficha técnica vinculada a tu bolsa
                </p>
              </div>
              <div class="w-8 h-8 rounded-full bg-slate-100 group-hover:bg-emerald-600 group-hover:text-white text-slate-500 flex items-center justify-center transition-colors shrink-0">
                <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                  <path d="m9 18 6-6-6-6" />
                </svg>
              </div>
            </button>
          </div>
        </div>

        <div class="mt-6 pt-4 border-t border-slate-100 text-center">
          <p class="text-xs text-slate-400">
            Experiencia interactiva Packsy NFC
          </p>
        </div>
      </section>

      <!-- =================================================================== -->
      <!-- PÁGINA 3: FUNCIONALIDADES (#funcionalidades)                        -->
      <!-- =================================================================== -->
      <section id="page-funcionalidades" class="page-view flex-1 flex-col justify-between px-6 py-6" aria-labelledby="title-funcionalidades">
        <div>
          <!-- Botón visible de regreso a Inicio -->
          <div class="flex items-center justify-between mb-5">
            <button
              type="button"
              onclick="navigateTo('inicio')"
              class="inline-flex items-center gap-2 px-4 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200/80 active:scale-[0.97] text-slate-800 font-semibold text-sm border border-slate-200/80 shadow-2xs transition-all"
            >
              <svg class="w-4 h-4 text-slate-700" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
                <polyline points="9 22 9 12 15 12 15 22" />
              </svg>
              <span>Inicio</span>
            </button>

            <span class="text-xs font-medium text-sky-700 bg-sky-50 px-2.5 py-1 rounded-lg border border-sky-200/70">
              Producto
            </span>
          </div>

          <h1 id="title-funcionalidades" class="text-2xl font-extrabold tracking-tight text-slate-900">
            Funcionalidades
          </h1>
          <p class="mt-1 text-sm text-slate-600">
            Todo lo que tu bolsa térmica multifuncional hace por ti.
          </p>

          <!-- Tres tarjetas visuales atractivas con icono claro y una línea breve -->
          <div class="mt-6 space-y-4">
            <!-- Función 1 -->
            <div class="p-5 rounded-2xl bg-gradient-to-br from-sky-50 to-blue-50/60 border border-sky-200/80 shadow-card flex items-center gap-4">
              <div class="w-14 h-14 shrink-0 rounded-2xl bg-gradient-to-br from-sky-500 to-blue-600 text-white flex items-center justify-center shadow-sm">
                <svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M14 4v10.54a4 4 0 1 1-4 0V4a2 2 0 0 1 4 0Z" />
                  <path d="M12 14v-4" />
                </svg>
              </div>
              <p class="text-base font-bold text-slate-900 leading-snug">
                Conservar la temperatura de tus alimentos
              </p>
            </div>

            <!-- Función 2 -->
            <div class="p-5 rounded-2xl bg-gradient-to-br from-amber-50 to-orange-50/60 border border-amber-200/80 shadow-card flex items-center gap-4">
              <div class="w-14 h-14 shrink-0 rounded-2xl bg-gradient-to-br from-amber-500 to-orange-600 text-white flex items-center justify-center shadow-sm">
                <svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M6 7h12l1 13H5L6 7z" />
                  <path d="M9 7V5a3 3 0 0 1 6 0v2" />
                  <line x1="9" y1="12" x2="15" y2="12" />
                </svg>
              </div>
              <p class="text-base font-bold text-slate-900 leading-snug">
                Transportar alimentos y objetos personales
              </p>
            </div>

            <!-- Función 3 -->
            <div class="p-5 rounded-2xl bg-gradient-to-br from-emerald-50 to-teal-50/60 border border-emerald-200/80 shadow-card flex items-center gap-4">
              <div class="w-14 h-14 shrink-0 rounded-2xl bg-gradient-to-br from-emerald-500 to-teal-600 text-white flex items-center justify-center shadow-sm">
                <svg class="w-7 h-7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M6 8.32a7.43 7.43 0 0 1 0 7.36" />
                  <path d="M9.46 6.21a11.76 11.76 0 0 1 0 11.58" />
                  <path d="M12.91 4.1a15.91 15.91 0 0 1 .01 15.8" />
                  <path d="M16.37 2a20.16 20.16 0 0 1 0 20" />
                </svg>
              </div>
              <p class="text-base font-bold text-slate-900 leading-snug">
                Acceder a la ficha de tu bolsa con NFC
              </p>
            </div>
          </div>
        </div>

        <div class="mt-6 pt-4 border-t border-slate-100 text-center">
          <p class="text-xs text-slate-400">
            Packsy • Diseño térmico inteligente
          </p>
        </div>
      </section>

      <!-- =================================================================== -->
      <!-- PÁGINA 4: DETALLES DE MI PACKSY (#detalles)                         -->
      <!-- =================================================================== -->
      <section id="page-detalles" class="page-view flex-1 flex-col justify-between px-6 py-6" aria-labelledby="title-detalles">
        <div>
          <!-- Botón visible de regreso a Inicio -->
          <div class="flex items-center justify-between mb-5">
            <button
              type="button"
              onclick="navigateTo('inicio')"
              class="inline-flex items-center gap-2 px-4 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200/80 active:scale-[0.97] text-slate-800 font-semibold text-sm border border-slate-200/80 shadow-2xs transition-all"
            >
              <svg class="w-4 h-4 text-slate-700" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
                <path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
                <polyline points="9 22 9 12 15 12 15 22" />
              </svg>
              <span>Inicio</span>
            </button>

            <span class="text-xs font-medium text-emerald-700 bg-emerald-50 px-2.5 py-1 rounded-lg border border-emerald-200/70">
              Ficha NFC
            </span>
          </div>

          <h1 id="title-detalles" class="text-2xl font-extrabold tracking-tight text-slate-900">
            Detalles de mi Packsy
          </h1>
          <p class="mt-1 text-sm text-slate-600">
            Información vinculada al chip de identificación de esta unidad.
          </p>

          <!-- Tarjeta compacta y elegante con espacio para logo de Packsy -->
          <div class="mt-5 rounded-3xl bg-white border border-slate-200/90 shadow-card overflow-hidden">
            <!-- Encabezado de la ficha con espacio para logotipo -->
            <div class="px-5 py-4 bg-gradient-to-r from-slate-900 to-slate-800 text-white flex items-center justify-between">
              <div class="flex items-center gap-2.5">
                <div class="w-8 h-8 rounded-xl bg-white/10 border border-dashed border-white/30 flex items-center justify-center text-xs font-bold">
                  P
                </div>
                <div>
                  <span class="block text-xs font-bold tracking-wider uppercase text-white">Ficha Oficial Packsy</span>
                  <span class="block text-[10px] text-slate-300">Espacio para logotipo de marca</span>
                </div>
              </div>
              <span class="px-2.5 py-1 rounded-full bg-emerald-500/20 border border-emerald-400/30 text-emerald-300 text-[11px] font-semibold">
                NFC Activo
              </span>
            </div>

            <!-- Campos de la ficha -->
            <div class="divide-y divide-slate-100">
              <!-- Nombre del producto -->
              <div class="p-4 flex flex-col gap-1">
                <div class="flex items-center justify-between gap-2">
                  <span class="text-[11px] font-bold uppercase tracking-wider text-slate-400">
                    Nombre del producto
                  </span>
                  <span class="px-2 py-0.5 rounded-md bg-amber-100/80 text-amber-800 text-[10px] font-semibold">
                    Dato de ejemplo
                  </span>
                </div>
                <span class="text-base font-bold text-slate-900">
                  Bolsa Térmica Multifuncional Packsy
                </span>
              </div>

              <!-- Modelo -->
              <div class="p-4 flex flex-col gap-1">
                <div class="flex items-center justify-between gap-2">
                  <span class="text-[11px] font-bold uppercase tracking-wider text-slate-400">
                    Modelo
                  </span>
                  <span class="px-2 py-0.5 rounded-md bg-amber-100/80 text-amber-800 text-[10px] font-semibold">
                    Dato de ejemplo
                  </span>
                </div>
                <span class="text-base font-bold text-slate-900">
                  Packsy Urban Thermal v1
                </span>
              </div>

              <!-- Color -->
              <div class="p-4 flex flex-col gap-1">
                <div class="flex items-center justify-between gap-2">
                  <span class="text-[11px] font-bold uppercase tracking-wider text-slate-400">
                    Color
                  </span>
                  <span class="px-2 py-0.5 rounded-md bg-amber-100/80 text-amber-800 text-[10px] font-semibold">
                    Dato de ejemplo
                  </span>
                </div>
                <div class="flex items-center gap-2 mt-0.5">
                  <span class="w-4 h-4 rounded-full bg-slate-800 ring-2 ring-slate-200 inline-block"></span>
                  <span class="text-base font-bold text-slate-900">
                    Gris Grafito / Negro Mate
                  </span>
                </div>
              </div>

              <!-- Código de identificación (sincronizado con el código único escaneado/verificado) -->
              <div class="p-4 flex flex-col gap-1 bg-slate-50/80">
                <div class="flex items-center justify-between gap-2">
                  <span class="text-[11px] font-bold uppercase tracking-wider text-slate-400">
                    Código de identificación
                  </span>
                  <span id="details-code-badge" class="px-2 py-0.5 rounded-md bg-emerald-100 text-emerald-800 text-[10px] font-semibold">
                    Código de prueba NFC
                  </span>
                </div>
                <span id="details-code-value" class="text-base font-mono font-extrabold text-slate-900 tracking-wider">
                  PACKSY-DEMO
                </span>
              </div>
            </div>
          </div>
        </div>

        <!-- Nota sobre datos de ejemplo -->
        <div class="mt-5 p-3.5 rounded-2xl bg-amber-50/80 border border-amber-200/80">
          <p class="text-[11px] text-amber-900 leading-relaxed">
            Los campos etiquetados como <strong>Dato de ejemplo</strong> muestran valores provisionales que se completarán con los datos reales de fabricación de cada bolso Packsy.
          </p>
        </div>
      </section>

    </div>
  </main>

  <script>
    // Estado global del código único escaneado/verificado en la sesión actual
    let currentBagCode = 'PACKSY-DEMO';
    // Lista de códigos únicos simulados al pulsar "Simular otro escaneo NFC"
    const simulatedNfcPool = [
      { code: 'PACKSY-DEMO', isDemoValid: true },
      { code: 'PACKSY-0048-MX', isDemoValid: false },
      { code: 'PACKSY-0192-NFC', isDemoValid: false },
      { code: 'PACKSY-DEMO', isDemoValid: true }
    ];
    let scanIndex = 0;

    // Mapa de rutas independientes y títulos de ventana
    const ROUTES = {
      verificar: {
        pageId: 'page-verificar',
        title: 'Verificar autenticidad — Packsy'
      },
      inicio: {
        pageId: 'page-inicio',
        title: 'Verificación Packsy — Inicio'
      },
      funcionalidades: {
        pageId: 'page-funcionalidades',
        title: 'Funcionalidades — Packsy'
      },
      detalles: {
        pageId: 'page-detalles',
        title: 'Detalles de mi Packsy'
      }
    };

    /**
     * Navega a una página independiente actualizando la vista activa y el hash de la URL
     * @param {string} routeName - 'verificar' | 'inicio' | 'funcionalidades' | 'detalles'
     * @param {boolean} updateHash - Indica si debe actualizar window.location.hash
     */
    function navigateTo(routeName, updateHash = true) {
      const targetRoute = ROUTES[routeName] ? routeName : 'verificar';

      // Ocultar por completo todas las páginas
      document.querySelectorAll('.page-view').forEach((section) => {
        section.classList.remove('active');
      });

      // Mostrar únicamente la página seleccionada
      const activeSection = document.getElementById(ROUTES[targetRoute].pageId);
      if (activeSection) {
        activeSection.classList.add('active');
      }

      // Actualizar título del documento
      document.title = ROUTES[targetRoute].title;

      // Sincronizar ficha de detalles si se abre esa página
      if (targetRoute === 'detalles') {
        syncDetailsPage();
      }

      // Actualizar hash de URL para navegación independiente y soporte del botón Atrás
      if (updateHash && window.location.hash !== `#${targetRoute}`) {
        window.location.hash = targetRoute;
      }

      // Desplazar suavemente hacia arriba
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    /**
     * Sincroniza el código único actual en la pantalla "Detalles de mi Packsy"
     */
    function syncDetailsPage() {
      const codeEl = document.getElementById('details-code-value');
      const badgeEl = document.getElementById('details-code-badge');
      const normalized = (currentBagCode || '').trim().toUpperCase();

      if (!normalized) {
        codeEl.textContent = 'SIN CÓDIGO INGRESADO';
        badgeEl.textContent = 'Dato pendiente';
        badgeEl.className = 'px-2 py-0.5 rounded-md bg-amber-100/80 text-amber-800 text-[10px] font-semibold';
        return;
      }

      codeEl.textContent = normalized;
      if (normalized === 'PACKSY-DEMO') {
        badgeEl.textContent = 'Código de prueba NFC';
        badgeEl.className = 'px-2 py-0.5 rounded-md bg-emerald-100 text-emerald-800 text-[10px] font-semibold';
      } else {
        badgeEl.textContent = 'Código escaneado (Ejemplo)';
        badgeEl.className = 'px-2 py-0.5 rounded-md bg-amber-100/80 text-amber-800 text-[10px] font-semibold';
      }
    }

    /**
     * Simula acercar el teléfono a otro bolso Packsy con un código NFC único
     */
    function simulateAnotherNfcScan() {
      scanIndex = (scanIndex + 1) % simulatedNfcPool.length;
      const nextScan = simulatedNfcPool[scanIndex];
      const input = document.getElementById('code-input');
      const subtitle = document.getElementById('nfc-banner-subtitle');

      input.value = nextScan.code;
      currentBagCode = nextScan.code;
      subtitle.textContent = `Nueva lectura NFC recibida: ${nextScan.code}`;

      verifyCode();
    }

    /**
     * Restaura el código PACKSY-DEMO y ejecuta la verificación
     */
    function restoreDefaultDemo() {
      const input = document.getElementById('code-input');
      const subtitle = document.getElementById('nfc-banner-subtitle');
      input.value = 'PACKSY-DEMO';
      currentBagCode = 'PACKSY-DEMO';
      subtitle.textContent = 'Código único extraído automáticamente del chip de tu bolso.';
      verifyCode();
    }

    /**
     * Limpia el campo de código para permitir ingresar uno nuevo manualmente
     */
    function clearCodeInput() {
      const input = document.getElementById('code-input');
      input.value = '';
      currentBagCode = '';
      input.focus();
    }

    /**
     * Valida el código ingresado o leído por NFC y muestra el resultado con animación
     */
    function verifyCode() {
      const input = document.getElementById('code-input');
      const resultContainer = document.getElementById('verify-result');
      const rawValue = input.value.trim();

      currentBagCode = rawValue.toUpperCase();
      syncDetailsPage();

      // Caso 1: Campo vacío
      if (!rawValue) {
        resultContainer.innerHTML = `
          <div class="animate-pop-in p-4 rounded-2xl bg-amber-50 border border-amber-200/90 text-amber-950 flex items-start gap-3 shadow-2xs">
            <div class="w-8 h-8 rounded-xl bg-amber-500 text-white flex items-center justify-center shrink-0 mt-0.5">
              <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="10" />
                <line x1="12" y1="8" x2="12" y2="12" />
                <line x1="12" y1="16" x2="12.01" y2="16" />
              </svg>
            </div>
            <div>
              <p class="text-sm font-bold text-amber-900">Por favor, ingresa un código</p>
              <p class="text-xs text-amber-800 mt-0.5 leading-relaxed">
                Escribe el código único de tu bolso o pulsa <strong>Cargar PACKSY-DEMO</strong> para probar.
              </p>
            </div>
          </div>
        `;
        input.focus();
        return;
      }

      const normalizedCode = rawValue.toUpperCase();

      // Caso 2: Código de demostración válido (PACKSY-DEMO)
      if (normalizedCode === 'PACKSY-DEMO') {
        resultContainer.innerHTML = `
          <div class="animate-pop-in p-4 rounded-2xl bg-gradient-to-br from-emerald-50 to-teal-50/70 border border-emerald-200/90 text-emerald-950 flex items-start gap-3.5 shadow-card">
            <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-emerald-500 to-teal-600 text-white flex items-center justify-center shrink-0 mt-0.5 shadow-xs">
              <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="20 6 9 17 4 12" />
              </svg>
            </div>
            <div class="flex-1 min-w-0">
              <div class="flex flex-wrap items-center gap-2">
                <p class="text-base font-extrabold text-emerald-900">Código de demostración válido</p>
                <span class="px-2 py-0.5 rounded-full bg-emerald-200/80 text-emerald-950 text-[11px] font-bold">
                  Código de prueba único
                </span>
              </div>
              <p class="text-xs text-emerald-800 mt-1 leading-relaxed">
                Identificador <span class="font-mono font-bold">${escapeHtml(normalizedCode)}</span> reconocido correctamente en el entorno de demostración de Packsy.
              </p>
            </div>
          </div>
        `;
        return;
      }

      // Caso 3: Cualquier otro código no registrado
      resultContainer.innerHTML = `
        <div class="animate-pop-in p-4 rounded-2xl bg-gradient-to-br from-rose-50 to-red-50/60 border border-rose-200/90 text-rose-950 flex items-start gap-3.5 shadow-card">
          <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-rose-500 to-red-600 text-white flex items-center justify-center shrink-0 mt-0.5 shadow-xs">
            <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round">
              <line x1="18" y1="6" x2="6" y2="18" />
              <line x1="6" y1="6" x2="18" y2="18" />
            </svg>
          </div>
          <div class="flex-1 min-w-0">
            <p class="text-base font-extrabold text-rose-900">Código no encontrado</p>
            <p class="text-xs text-rose-800 mt-1 leading-relaxed">
              El código <span class="font-mono font-bold break-all">${escapeHtml(normalizedCode)}</span> no figura en los registros de prueba. Verifica los caracteres o prueba con <span class="font-mono font-bold">PACKSY-DEMO</span>.
            </p>
          </div>
        </div>
      `;
    }

    /**
     * Escapa HTML para evitar inyección en mensajes dinámicos
     */
    function escapeHtml(text) {
      const div = document.createElement('div');
      div.textContent = text;
      return div.innerHTML;
    }

    // Soporte para tecla Enter en el input
    document.getElementById('code-input').addEventListener('keydown', function (event) {
      if (event.key === 'Enter') {
        event.preventDefault();
        verifyCode();
      }
    });

    // Soporte para navegación con botones Atrás / Adelante del navegador
    window.addEventListener('hashchange', function () {
      const hashRoute = window.location.hash.replace('#', '');
      navigateTo(hashRoute || 'verificar', false);
    });

    // Inicialización al cargar la página:
    // 1. Entra directamente a la pantalla "Verificar autenticidad"
    // 2. Simula la lectura automática del chip NFC único (PACKSY-DEMO o parámetro ?code= en URL)
    window.addEventListener('DOMContentLoaded', function () {
      const urlParams = new URLSearchParams(window.location.search);
      const scannedParam = urlParams.get('code') || urlParams.get('nfc');
      const initialCode = scannedParam ? scannedParam.trim().toUpperCase() : 'PACKSY-DEMO';

      const input = document.getElementById('code-input');
      input.value = initialCode;
      currentBagCode = initialCode;

      // Ejecutar verificación inicial automática tras simular escaneo NFC
      verifyCode();

      // Abrir pantalla solicitada por hash o entrar directo a "verificar"
      const initialHash = window.location.hash.replace('#', '');
      navigateTo(ROUTES[initialHash] ? initialHash : 'verificar', true);
    });
  </script>
</body>
</html>
