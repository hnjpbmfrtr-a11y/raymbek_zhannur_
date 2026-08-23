<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Райымбек & Жаннұр — Үйлену тойына шақыру</title>
    
    <!-- Open Graph for WhatsApp/Social Media preview -->
    <meta property="og:title" content="Райымбек & Жаннұр — Үйлену тойына шақыру">
    <meta property="og:description" content="12.09.2026 | Сізді қуанышымызды бірге бөлісуге шақырамыз!">
    <meta property="og:type" content="website">
    <meta property="og:image" content="https://images.unsplash.com/photo-1519741497674-611481863552?q=80&w=1200&auto=format&fit=crop">

    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400&family=Montserrat:wght@300;400;500;600&family=Great+Vibes&display=swap" rel="stylesheet">
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <!-- Canvas Confetti for RSVP -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        cream: '#FAF7F2',
                        ivory: '#FFFDF9',
                        champagne: '#F4EBE1',
                        gold: {
                            DEFAULT: '#C5A880',
                            light: '#E6D5BC',
                            dark: '#A3845B'
                        },
                        charcoal: '#2C2A29'
                    },
                    fontFamily: {
                        serif: ['"Cormorant Garamond"', 'serif'],
                        sans: ['"Montserrat"', 'sans-serif'],
                        script: ['"Great Vibes"', 'cursive']
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #FAF7F2;
            color: #2C2A29;
            font-family: 'Montserrat', sans-serif;
            overflow-x: hidden;
            background-image: radial-gradient(rgba(197, 168, 128, 0.08) 1px, transparent 0);
            background-size: 24px 24px;
        }

        .gold-border {
            border: 1px solid rgba(197, 168, 128, 0.3);
        }

        .gold-gradient-text {
            background: linear-gradient(135deg, #B8966C 0%, #E6D5BC 50%, #A3845B 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .gold-gradient-bg {
            background: linear-gradient(135deg, #C5A880 0%, #D4BC9A 50%, #A3845B 100%);
        }

        /* Floating particles */
        .particle {
            position: absolute;
            pointer-events: none;
            background: radial-gradient(circle, rgba(212,188,154,0.6) 0%, rgba(255,255,255,0) 70%);
            border-radius: 50%;
            animation: float 12s infinite linear;
        }

        @keyframes float {
            0% { transform: translateY(0) rotate(0deg); opacity: 0; }
            20% { opacity: 0.6; }
            80% { opacity: 0.6; }
            100% { transform: translateY(-100vh) rotate(360deg); opacity: 0; }
        }

        /* Calendar animation */
        .calendar-card {
            transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
            perspective: 1000px;
        }
        .calendar-card.flipped {
            transform: rotateY(180deg);
        }

        .glass-card {
            background: rgba(255, 253, 249, 0.75);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(197, 168, 128, 0.25);
            box-shadow: 0 10px 30px -10px rgba(197, 168, 128, 0.15);
        }

        /* Fade in classes */
        .fade-in-up {
            opacity: 0;
            transform: translateY(30px);
            transition: opacity 0.8s ease-out, transform 0.8s ease-out;
        }
        .fade-in-up.visible {
            opacity: 1;
            transform: translateY(0);
        }
    </style>

    <!-- Firebase SDK Imports -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global Firebase state & handlers
        window.db = null;
        window.auth = null;
        window.user = null;
        window.appId = typeof __app_id !== 'undefined' ? __app_id : 'wedding-invitation-default';

        const firebaseConfig = typeof __firebase_config !== 'undefined' 
            ? JSON.parse(__firebase_config) 
            : {
                apiKey: "demo-key",
                authDomain: "demo.firebaseapp.com",
                projectId: "demo-project",
                storageBucket: "demo.appspot.com",
                messagingSenderId: "123456789",
                appId: "1:123456789:web:123456"
            };

        try {
            const app = initializeApp(firebaseConfig);
            window.auth = getAuth(app);
            window.db = getFirestore(app);

            async function initAuth() {
                try {
                    if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                        await signInWithCustomToken(window.auth, __initial_auth_token);
                    } else {
                        await signInAnonymously(window.auth);
                    }
                } catch (e) {
                    console.warn("Auth initialization fallback:", e);
                }
            }

            initAuth();

            onAuthStateChanged(window.auth, (currentUser) => {
                window.user = currentUser;
                if (currentUser && window.loadRsvpData) {
                    window.loadRsvpData();
                }
            });
        } catch (err) {
            console.error("Firebase init error:", err);
        }
    </script>
</head>
<body class="relative min-h-screen text-charcoal selection:bg-gold-light">

    <!-- Ambient Particles -->
    <div id="particles-container" class="fixed inset-0 pointer-events-none z-0 overflow-hidden"></div>

    <!-- Music Toggle Floating Button -->
    <div class="fixed top-5 right-5 z-50">
        <button id="musicToggleBtn" onclick="toggleAudio()" class="p-3 bg-ivory/90 border border-gold/40 rounded-full shadow-lg text-gold-dark hover:bg-gold/10 transition-all duration-300 flex items-center gap-2 group" aria-label="Музыка">
            <i data-lucide="music" class="w-5 h-5 text-gold-dark group-hover:scale-110 transition-transform"></i>
            <span id="musicText" class="text-xs font-medium pr-1 hidden sm:inline text-charcoal">Музыка</span>
        </button>
        <audio id="weddingMusic" loop preload="none">
            <source src="https://cdn.pixabay.com/download/audio/2022/05/27/audio_1808fbf07a.mp3?filename=gentle-piano-wedding-love-115312.mp3" type="audio/mpeg">
        </audio>
    </div>

    <!-- MAIN CONTAINER -->
    <div class="relative z-10 max-w-xl mx-auto px-4 sm:px-6 py-6 pb-20 space-y-16">

        <!-- HERO SECTION -->
        <section class="text-center pt-8 fade-in-up">
            <div class="inline-block tracking-widest text-xs uppercase text-gold-dark border-b border-gold/40 pb-1 mb-6 font-medium">
                Үйлену тойға шақыру
            </div>

            <!-- Main Couple Typography -->
            <h1 class="font-serif text-4xl sm:text-6xl font-light tracking-wide text-charcoal my-2 uppercase">
                Райымбек
            </h1>
            <div class="font-script text-3xl sm:text-4xl text-gold-dark my-1">&</div>
            <h1 class="font-serif text-4xl sm:text-6xl font-light tracking-wide text-charcoal my-2 uppercase">
                Жаннұр
            </h1>

            <!-- Subtitle Text -->
            <p class="font-serif italic text-lg sm:text-xl text-stone-600 mt-6 leading-relaxed max-w-md mx-auto px-4">
                «Бақытты болашаққа бірге қадам басатын ерекше күнімізде Сіздерді қуанышымызды бірге бөлісуге шақырамыз!»
            </p>

            <!-- Main Portrait Photo -->
            <div class="mt-8 relative max-w-sm mx-auto">
                <div class="absolute -inset-2 rounded-2xl border border-gold/30 rotate-1 pointer-events-none"></div>
                <div class="relative overflow-hidden rounded-xl shadow-xl glass-card p-2">
                    <img 
                        src="https://images.unsplash.com/photo-1519741497674-611481863552?q=80&w=1000&auto=format&fit=crop" 
                        alt="Райымбек мен Жаннұр" 
                        class="w-full h-[380px] sm:h-[450px] object-cover rounded-lg transform hover:scale-105 transition-transform duration-700"
                        loading="lazy"
                    />
                </div>
            </div>
        </section>


        <!-- DATE & INTERACTIVE CALENDAR SECTION -->
        <section class="text-center fade-in-up">
            <div class="glass-card rounded-2xl p-6 sm:p-8 relative">
                <p class="text-xs tracking-widest uppercase text-gold-dark font-semibold mb-2">Ерекше күн</p>
                <h2 class="font-serif text-3xl sm:text-4xl text-charcoal font-normal tracking-wide">
                    12 ҚЫРКҮЙЕК 2026
                </h2>

                <!-- Interactive 3D Minimal Calendar -->
                <div class="mt-8 max-w-xs mx-auto">
                    <div id="calendarCard" class="calendar-card bg-ivory border border-gold/30 rounded-xl p-5 shadow-sm cursor-pointer hover:shadow-md transition-all group" onclick="toggleCalendarDetails()">
                        
                        <!-- Month Header -->
                        <div class="flex justify-between items-center border-b border-gold/20 pb-3 mb-4">
                            <span class="font-serif text-lg font-semibold text-charcoal">ҚЫРКҮЙЕК 2026</span>
                            <i data-lucide="calendar-heart" class="w-5 h-5 text-gold-dark group-hover:scale-110 transition-transform"></i>
                        </div>

                        <!-- Mini Calendar Grid -->
                        <div class="grid grid-cols-7 gap-1 text-center text-xs font-medium text-stone-500 mb-2">
                            <span>Дс</span><span>Сс</span><span>Ср</span><span>Бс</span><span>Жм</span><span>Сб</span><span>Жс</span>
                        </div>
                        <div class="grid grid-cols-7 gap-1 text-center text-xs font-serif text-stone-700">
                            <!-- Empty offset -->
                            <span></span><span>1</span><span>2</span><span>3</span><span>4</span><span>5</span><span>6</span>
                            <span>7</span><span>8</span><span>9</span><span>10</span><span>11</span>
                            
                            <!-- Highlighted Wedding Day 12 -->
                            <div class="relative flex items-center justify-center">
                                <span class="w-7 h-7 rounded-full bg-gold text-ivory font-bold flex items-center justify-center shadow-md animate-pulse">
                                    12
                                </span>
                            </div>

                            <span>13</span><span>14</span><span>15</span><span>16</span><span>17</span><span>18</span><span>19</span>
                            <span>20</span><span>21</span><span>22</span><span>23</span><span>24</span><span>25</span><span>26</span>
                            <span>27</span><span>28</span><span>29</span><span>30</span>
                        </div>

                        <div class="mt-4 pt-3 border-t border-gold/20 text-xs text-gold-dark font-medium flex items-center justify-center gap-1">
                            <span>Толығырақ көру үшін басыңыз</span>
                            <i data-lucide="chevron-down" class="w-3.5 h-3.5"></i>
                        </div>
                    </div>

                    <!-- Calendar Pop-up Detail Modal -->
                    <div id="calendarModal" class="hidden mt-4 p-4 bg-champagne/60 border border-gold/40 rounded-xl text-center fade-in-up">
                        <div class="flex justify-center items-center gap-2 text-gold-dark font-semibold text-lg">
                            <i data-lucide="clock" class="w-5 h-5"></i>
                            <span>12 Қыркүйек 2026 жыл</span>
                        </div>
                        <p class="text-stone-700 text-sm mt-1 font-serif">Басталу уақыты: <strong class="text-charcoal">18:00</strong></p>
                    </div>
                </div>

                <!-- REAL-TIME COUNTDOWN -->
                <div class="mt-10 pt-6 border-t border-gold/20">
                    <p class="text-xs uppercase tracking-widest text-gold-dark font-medium mb-4">Тойымызға дейін қалды:</p>
                    <div class="grid grid-cols-4 gap-2 text-center max-w-sm mx-auto">
                        <div class="bg-ivory/80 border border-gold/30 rounded-lg p-2.5 shadow-sm">
                            <span id="days" class="font-serif text-2xl sm:text-3xl font-bold text-charcoal">00</span>
                            <p class="text-[10px] sm:text-xs text-stone-500 uppercase mt-0.5">Күн</p>
                        </div>
                        <div class="bg-ivory/80 border border-gold/30 rounded-lg p-2.5 shadow-sm">
                            <span id="hours" class="font-serif text-2xl sm:text-3xl font-bold text-charcoal">00</span>
                            <p class="text-[10px] sm:text-xs text-stone-500 uppercase mt-0.5">Сағат</p>
                        </div>
                        <div class="bg-ivory/80 border border-gold/30 rounded-lg p-2.5 shadow-sm">
                            <span id="minutes" class="font-serif text-2xl sm:text-3xl font-bold text-charcoal">00</span>
                            <p class="text-[10px] sm:text-xs text-stone-500 uppercase mt-0.5">Минут</p>
                        </div>
                        <div class="bg-ivory/80 border border-gold/30 rounded-lg p-2.5 shadow-sm">
                            <span id="seconds" class="font-serif text-2xl sm:text-3xl font-bold text-gold-dark">00</span>
                            <p class="text-[10px] sm:text-xs text-stone-500 uppercase mt-0.5">Секунд</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>


        <!-- INVITATION TEXT SECTION -->
        <section class="text-center fade-in-up">
            <div class="glass-card rounded-2xl p-8 sm:p-10 relative overflow-hidden">
                <div class="w-12 h-12 mx-auto mb-4 text-gold-dark flex items-center justify-center">
                    <i data-lucide="heart-handshake" class="w-8 h-8 stroke-1"></i>
                </div>
                
                <h3 class="font-serif text-2xl sm:text-3xl text-charcoal font-semibold mb-6">
                    Құрметті ағайын-туыс,<br>қадірлі дос-жаран!
                </h3>

                <p class="font-serif text-lg sm:text-xl text-stone-700 leading-relaxed font-light">
                    Ұлымыз <strong class="font-semibold text-charcoal">Райымбек</strong> пен келініміз <strong class="font-semibold text-charcoal">Жаннұрдың</strong> ақ босаға аттап, шаңырақ көтеру қуанышына арналған ақ дастарханымыздың қадірлі қонағы болуға шақырамыз!
                </p>
            </div>
        </section>


        <!-- HOSTS SECTION -->
        <section class="text-center fade-in-up">
            <div class="glass-card rounded-2xl p-8 relative">
                <span class="text-xs uppercase tracking-widest text-gold-dark font-semibold">Ақ тілекпен</span>
                <h2 class="font-serif text-3xl text-charcoal mt-1 mb-8">ТОЙ ИЕЛЕРІ</h2>

                <div class="space-y-6">
                    <div class="p-4 rounded-xl bg-ivory/60 border border-gold/20">
                        <p class="text-xs uppercase tracking-wider text-stone-500 font-medium">Ата-әжесі:</p>
                        <p class="font-serif text-2xl text-charcoal font-medium mt-1">Тілекқабыл (Роза)</p>
                    </div>

                    <div class="p-4 rounded-xl bg-ivory/60 border border-gold/20">
                        <p class="text-xs uppercase tracking-wider text-stone-500 font-medium">Әке-анасы:</p>
                        <p class="font-serif text-2xl text-charcoal font-medium mt-1">Асылбек – Айсұлу</p>
                    </div>
                </div>
            </div>
        </section>


        <!-- EVENT CARDS SECTION -->
        <section class="space-y-4 fade-in-up">
            <h2 class="text-center font-serif text-2xl text-gold-dark tracking-wide uppercase mb-6">Той туралы ақпарат</h2>

            <!-- Card 1: Date -->
            <div class="glass-card rounded-xl p-5 flex items-center gap-4 transition-transform hover:-translate-y-1">
                <div class="w-12 h-12 rounded-full bg-champagne border border-gold/30 flex items-center justify-center shrink-0 text-gold-dark">
                    <i data-lucide="calendar" class="w-6 h-6"></i>
                </div>
                <div>
                    <span class="text-[10px] uppercase tracking-widest text-stone-500 font-semibold">📅 КҮНІ</span>
                    <h4 class="font-serif text-xl font-semibold text-charcoal">12 қыркүйек 2026 жыл</h4>
                </div>
            </div>

            <!-- Card 2: Time -->
            <div class="glass-card rounded-xl p-5 flex items-center gap-4 transition-transform hover:-translate-y-1">
                <div class="w-12 h-12 rounded-full bg-champagne border border-gold/30 flex items-center justify-center shrink-0 text-gold-dark">
                    <i data-lucide="clock" class="w-6 h-6"></i>
                </div>
                <div>
                    <span class="text-[10px] uppercase tracking-widest text-stone-500 font-semibold">🕕 УАҚЫТЫ</span>
                    <h4 class="font-serif text-xl font-semibold text-charcoal">Сағат 18:00</h4>
                </div>
            </div>

            <!-- Card 3: Address -->
            <div class="glass-card rounded-xl p-5 flex items-center gap-4 transition-transform hover:-translate-y-1">
                <div class="w-12 h-12 rounded-full bg-champagne border border-gold/30 flex items-center justify-center shrink-0 text-gold-dark">
                    <i data-lucide="map-pin" class="w-6 h-6"></i>
                </div>
                <div>
                    <span class="text-[10px] uppercase tracking-widest text-stone-500 font-semibold">📍 МЕКЕНЖАЙЫ</span>
                    <h4 class="font-serif text-xl font-semibold text-charcoal">Құлсары қаласы</h4>
                    <p class="text-stone-600 text-sm">«Нұрасыл» мейрамханасы</p>
                </div>
            </div>
        </section>


        <!-- MAP SECTION -->
        <section class="fade-in-up text-center">
            <div class="glass-card rounded-2xl p-6">
                <h3 class="font-serif text-2xl text-charcoal mb-2">Мейрамхана картасы</h3>
                <p class="text-xs text-stone-500 mb-6">«Нұрасыл» мейрамханасына бағыт алу үшін батырманы басыңыз</p>

                <!-- Map Frame Graphic Fallback -->
                <div class="w-full h-48 bg-stone-200/60 rounded-xl overflow-hidden relative mb-6 border border-gold/30 flex items-center justify-center">
                    <iframe 
                        title="Ресторан Нурасыл Кульсары"
                        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2728.2!2d54.01!3d46.98!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNDbCsDU4JzQ4LjAiTiA1NMKwMDAnMzYuMCJF!5e0!3m2!1skk!2skz!4v1600000000000!5m2!1skk!2skz" 
                        class="w-full h-full border-0 filter grayscale contrast-125 opacity-80" 
                        allowfullscreen="" 
                        loading="lazy">
                    </iframe>
                </div>

                <div class="flex flex-col sm:flex-row gap-3 justify-center">
                    <a href="https://maps.google.com/?q=Құлсары+Нұрасыл+мейрамханасы" target="_blank" rel="noopener noreferrer" class="flex-1 inline-flex items-center justify-center gap-2 px-5 py-3 rounded-xl bg-ivory border border-gold text-gold-dark font-medium text-sm hover:bg-gold hover:text-white transition-all shadow-sm">
                        <i data-lucide="map" class="w-4 h-4"></i>
                        <span>Google Maps арқылы ашу</span>
                    </a>
                    <a href="https://2gis.kz/search/Құлсары%20Нұрасыл" target="_blank" rel="noopener noreferrer" class="flex-1 inline-flex items-center justify-center gap-2 px-5 py-3 rounded-xl bg-emerald-700 text-white font-medium text-sm hover:bg-emerald-800 transition-all shadow-sm">
                        <i data-lucide="navigation" class="w-4 h-4"></i>
                        <span>2GIS арқылы ашу</span>
                    </a>
                </div>
            </div>
        </section>


        <!-- RSVP INTERACTIVE SECTION -->
        <section id="rsvpSection" class="fade-in-up text-center">
            <div class="glass-card rounded-2xl p-6 sm:p-8 border-2 border-gold/40 relative">
                <span class="text-2xl mb-2 block">❤️</span>
                <h2 class="font-serif text-2xl sm:text-3xl text-charcoal font-semibold mb-2">
                    СІЗДІҢ КЕЛУІҢІЗ БІЗ ҮШІН МАҢЫЗДЫ
                </h2>
                <p class="text-stone-600 text-sm mb-6">Тойымызға келуіңізді растайсыз ба?</p>

                <!-- Initial Buttons -->
                <div id="rsvpInitialButtons" class="flex flex-col sm:flex-row gap-4 justify-center">
                    <button onclick="showRsvpForm('YES')" class="flex-1 py-4 px-6 rounded-xl gold-gradient-bg text-white font-medium shadow-md hover:opacity-95 transition-all flex items-center justify-center gap-2 group">
                        <i data-lucide="heart" class="w-5 h-5 fill-current group-hover:scale-110 transition-transform"></i>
                        <span>КЕЛЕМІН</span>
                    </button>
                    <button onclick="submitNoRsvp()" class="flex-1 py-4 px-6 rounded-xl bg-stone-100 text-stone-600 border border-stone-300 font-medium hover:bg-stone-200 transition-all flex items-center justify-center gap-2">
                        <i data-lucide="x-circle" class="w-5 h-5"></i>
                        <span>КЕЛЕ АЛМАЙМЫН</span>
                    </button>
                </div>

                <!-- Form for "КЕЛЕМІН" -->
                <form id="rsvpForm" onsubmit="handleFormSubmit(event)" class="hidden mt-6 text-left space-y-4 max-w-md mx-auto bg-ivory/80 p-5 rounded-xl border border-gold/30">
                    <div>
                        <label class="block text-xs font-semibold text-stone-600 uppercase mb-1">Аты-жөніңіз *</label>
                        <input type="text" id="guestName" required placeholder="Мысалы: Арман Қайратұлы" class="w-full px-4 py-3 rounded-lg border border-gold/30 focus:outline-none focus:ring-2 focus:ring-gold bg-white text-charcoal text-sm">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-stone-600 uppercase mb-2">Қанша адам болып келесіз?</label>
                        <div class="grid grid-cols-4 gap-2">
                            <button type="button" onclick="selectCount(1)" class="count-btn py-2.5 rounded-lg border border-gold/30 text-center font-medium hover:bg-gold/10 text-charcoal active" data-count="1">1</button>
                            <button type="button" onclick="selectCount(2)" class="count-btn py-2.5 rounded-lg border border-gold/30 text-center font-medium hover:bg-gold/10 text-charcoal" data-count="2">2</button>
                            <button type="button" onclick="selectCount(3)" class="count-btn py-2.5 rounded-lg border border-gold/30 text-center font-medium hover:bg-gold/10 text-charcoal" data-count="3">3</button>
                            <button type="button" onclick="selectCount(4)" class="count-btn py-2.5 rounded-lg border border-gold/30 text-center font-medium hover:bg-gold/10 text-charcoal" data-count="4">4</button>
                        </div>
                    </div>

                    <div class="flex gap-2 pt-2">
                        <button type="button" onclick="cancelRsvpForm()" class="w-1/3 py-3 rounded-lg bg-stone-200 text-stone-700 font-medium text-sm hover:bg-stone-300">Кейін</button>
                        <button type="submit" id="submitRsvpBtn" class="w-2/3 py-3 rounded-lg gold-gradient-bg text-white font-medium text-sm shadow-md hover:opacity-90 flex items-center justify-center gap-2">
                            <span>Растау</span>
                            <i data-lucide="send" class="w-4 h-4"></i>
                        </button>
                    </div>
                </form>

                <!-- Confirmation Animated Result -->
                <div id="rsvpConfirmation" class="hidden mt-4 p-6 bg-champagne/80 rounded-xl text-center fade-in-up">
                    <div class="w-12 h-12 bg-gold/20 text-gold-dark rounded-full flex items-center justify-center mx-auto mb-3">
                        <i data-lucide="check" class="w-6 h-6 stroke-[3]"></i>
                    </div>
                    <h3 id="confTitle" class="font-serif text-2xl text-charcoal font-bold">Жауабыңыз қабылданды ❤️</h3>
                    <p id="confSub" class="text-stone-600 text-sm mt-1">12 қыркүйекте кездескенше!</p>
                </div>
            </div>
        </section>


        <!-- FINAL SECTION -->
        <section class="text-center space-y-8 fade-in-up">
            <div class="glass-card rounded-2xl p-8">
                <p class="font-serif text-2xl sm:text-3xl text-charcoal leading-relaxed">
                    «Қуанышымызды бірге бөлісіп,<br>ақ тілектеріңізбен төрімізден табылыңыздар!»
                </p>

                <div class="my-8 w-24 h-px bg-gold/40 mx-auto"></div>

                <!-- Important Note -->
                <div class="bg-amber-50 border border-amber-200/80 p-4 rounded-xl max-w-md mx-auto">
                    <p class="text-amber-900 text-sm font-medium leading-normal">
                        Тойға кешікпей, балаларсыз келуіңізді сұраймыз!
                    </p>
                </div>

                <div class="mt-8 font-serif text-3xl text-gold-dark font-light">
                    Райымбек & Жаннұр 🤍
                </div>
            </div>

            <!-- Admin Link Trigger -->
            <div class="text-center pt-4">
                <button onclick="toggleAdminModal()" class="text-xs text-stone-400 hover:text-stone-600 underline">
                    Админ панельге өту
                </button>
            </div>
        </section>

    </div>


    <!-- ADMIN PANEL MODAL -->
    <div id="adminModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-ivory rounded-2xl w-full max-w-2xl max-h-[90vh] overflow-hidden flex flex-col shadow-2xl border border-gold/40">
            
            <!-- Modal Header -->
            <div class="p-5 bg-champagne border-b border-gold/30 flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i data-lucide="shield-check" class="w-5 h-5 text-gold-dark"></i>
                    <h3 class="font-serif text-xl font-semibold text-charcoal">Қонақтар тізімі (Админ Панель)</h3>
                </div>
                <button onclick="toggleAdminModal()" class="text-stone-500 hover:text-stone-800">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>

            <!-- Password Auth Screen -->
            <div id="adminAuthScreen" class="p-8 text-center space-y-4">
                <p class="text-sm text-stone-600">Қорғалған бөлім. Кіру үшін құпия сөзді енгізіңіз:</p>
                <div class="max-w-xs mx-auto space-y-3">
                    <input type="password" id="adminPasswordInput" placeholder="Пароль (әдепкі: 12092026)" class="w-full px-4 py-2 border rounded-lg text-sm text-center focus:ring-2 focus:ring-gold outline-none">
                    <button onclick="checkAdminPassword()" class="w-full py-2.5 gold-gradient-bg text-white font-medium text-sm rounded-lg shadow">Кіру</button>
                    <p id="adminAuthErr" class="text-xs text-red-500 hidden">Қате пароль!</p>
                </div>
            </div>

            <!-- Admin Dashboard Content -->
            <div id="adminContentScreen" class="hidden p-6 overflow-y-auto flex-1 space-y-6">
                
                <!-- Stats Grid -->
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                    <div class="bg-white p-3 rounded-xl border border-stone-200 text-center shadow-sm">
                        <span class="text-[10px] text-stone-500 uppercase font-bold">Барлық жауап</span>
                        <p id="statTotal" class="text-2xl font-serif font-bold text-charcoal">0</p>
                    </div>
                    <div class="bg-emerald-50 p-3 rounded-xl border border-emerald-200 text-center shadow-sm">
                        <span class="text-[10px] text-emerald-700 uppercase font-bold">Келеді</span>
                        <p id="statYes" class="text-2xl font-serif font-bold text-emerald-700">0</p>
                    </div>
                    <div class="bg-rose-50 p-3 rounded-xl border border-rose-200 text-center shadow-sm">
                        <span class="text-[10px] text-rose-700 uppercase font-bold">Келе алмайды</span>
                        <p id="statNo" class="text-2xl font-serif font-bold text-rose-700">0</p>
                    </div>
                    <div class="bg-amber-50 p-3 rounded-xl border border-amber-200 text-center shadow-sm">
                        <span class="text-[10px] text-amber-800 uppercase font-bold">Жалпы қонақ</span>
                        <p id="statGuestCount" class="text-2xl font-serif font-bold text-amber-800">0</p>
                    </div>
                </div>

                <!-- Export CSV Button -->
                <div class="flex justify-between items-center">
                    <h4 class="font-serif font-semibold text-lg text-charcoal">Қонақтар тізімі</h4>
                    <button onclick="exportToCSV()" class="px-3 py-1.5 bg-stone-800 text-white text-xs rounded-lg flex items-center gap-1.5 hover:bg-stone-900 transition-colors">
                        <i data-lucide="download" class="w-3.5 h-3.5"></i>
                        <span>Excel / CSV жүктеу</span>
                    </button>
                </div>

                <!-- Guests Table -->
                <div class="overflow-x-auto border border-stone-200 rounded-xl bg-white">
                    <table class="w-full text-left text-xs text-stone-700">
                        <thead class="bg-stone-100 text-stone-600 font-semibold uppercase border-b">
                            <tr>
                                <th class="p-3">Аты-жөні</th>
                                <th class="p-3">Жауабы</th>
                                <th class="p-3">Саны</th>
                                <th class="p-3">Уақыты</th>
                            </tr>
                        </thead>
                        <tbody id="adminGuestTableBody" class="divide-y divide-stone-100">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>


    <!-- JAVASCRIPT LOGIC -->
    <script>
        // Init Lucide Icons
        document.addEventListener("DOMContentLoaded", () => {
            lucide.createIcons();
            initParticles();
            initCountdown();
            initScrollAnimations();
        });

        // 1. Floating Gold Particles
        function initParticles() {
            const container = document.getElementById('particles-container');
            const particleCount = 12;
            for (let i = 0; i < particleCount; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                const size = Math.random() * 8 + 4;
                particle.style.width = `${size}px`;
                particle.style.height = `${size}px`;
                particle.style.left = `${Math.random() * 100}%`;
                particle.style.animationDelay = `${Math.random() * 10}s`;
                particle.style.animationDuration = `${10 + Math.random() * 10}s`;
                container.appendChild(particle);
            }
        }

        // 2. Audio Control
        let isPlaying = false;
        function toggleAudio() {
            const audio = document.getElementById('weddingMusic');
            const text = document.getElementById('musicText');
            if (isPlaying) {
                audio.pause();
                text.innerText = "Музыка";
            } else {
                audio.play().catch(() => {});
                text.innerText = "Тоқтату";
            }
            isPlaying = !isPlaying;
        }

        // 3. Interactive Calendar Toggle
        function toggleCalendarDetails() {
            const card = document.getElementById('calendarCard');
            const modal = document.getElementById('calendarModal');
            modal.classList.toggle('hidden');
        }

        // 4. Countdown Timer logic (Target: 12.09.2026 18:00)
        function initCountdown() {
            const targetDate = new Date("2026-09-12T18:00:00").getTime();

            function update() {
                const now = new Date().getTime();
                const diff = targetDate - now;

                if (diff <= 0) {
                    document.getElementById('days').innerText = "00";
                    document.getElementById('hours').innerText = "00";
                    document.getElementById('minutes').innerText = "00";
                    document.getElementById('seconds').innerText = "00";
                    return;
                }

                const days = Math.floor(diff / (1000 * 60 * 60 * 24));
                const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((diff % (1000 * 60)) / 1000);

                document.getElementById('days').innerText = String(days).padStart(2, '0');
                document.getElementById('hours').innerText = String(hours).padStart(2, '0');
                document.getElementById('minutes').innerText = String(minutes).padStart(2, '0');
                document.getElementById('seconds').innerText = String(seconds).padStart(2, '0');
            }

            update();
            setInterval(update, 1000);
        }

        // 5. Scroll Fade-In Animations
        function initScrollAnimations() {
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('visible');
                    }
                });
            }, { threshold: 0.1 });

            document.querySelectorAll('.fade-in-up').forEach(el => observer.observe(el));
        }

        // 6. RSVP Logic
        let selectedGuestCount = 1;

        function showRsvpForm(type) {
            document.getElementById('rsvpInitialButtons').classList.add('hidden');
            document.getElementById('rsvpForm').classList.remove('hidden');
        }

        function cancelRsvpForm() {
            document.getElementById('rsvpForm').classList.add('hidden');
            document.getElementById('rsvpInitialButtons').classList.remove('hidden');
        }

        function selectCount(count) {
            selectedGuestCount = count;
            document.querySelectorAll('.count-btn').forEach(btn => {
                if (parseInt(btn.getAttribute('data-count')) === count) {
                    btn.classList.add('bg-gold', 'text-white');
                    btn.classList.remove('bg-transparent', 'text-charcoal');
                } else {
                    btn.classList.remove('bg-gold', 'text-white');
                    btn.classList.add('bg-transparent', 'text-charcoal');
                }
            });
        }

        // Handle "КЕЛЕМІН" submit
        async function handleFormSubmit(event) {
            event.preventDefault();
            const nameInput = document.getElementById('guestName').value.trim();
            const submitBtn = document.getElementById('submitRsvpBtn');
            
            if (!nameInput) return;

            submitBtn.disabled = true;
            submitBtn.innerText = "Сақталуда...";

            await saveRsvpToFirestore({
                name: nameInput,
                attending: true,
                count: selectedGuestCount,
                timestamp: new Date().toISOString()
            });

            document.getElementById('rsvpForm').classList.add('hidden');
            showConfirmationModal("Жауабыңыз қабылданды ❤️", "12 қыркүйекте кездескенше!");

            // Trigger confetti
            if (window.confetti) {
                confetti({
                    particleCount: 80,
                    spread: 60,
                    origin: { y: 0.8 },
                    colors: ['#C5A880', '#E6D5BC', '#A3845B']
                });
            }
        }

        // Handle "КЕЛЕ АЛМАЙМЫН"
        async function submitNoRsvp() {
            const name = prompt("Аты-жөніңізді енгізіңіз:");
            if (!name) return;

            await saveRsvpToFirestore({
                name: name,
                attending: false,
                count: 0,
                timestamp: new Date().toISOString()
            });

            document.getElementById('rsvpInitialButtons').classList.add('hidden');
            showConfirmationModal("Рахмет!", "Өкінішке қарай, тойға қатыса алмайтыныңыз тіркелді.");
        }

        function showConfirmationModal(title, sub) {
            const conf = document.getElementById('rsvpConfirmation');
            document.getElementById('confTitle').innerText = title;
            document.getElementById('confSub').innerText = sub;
            conf.classList.remove('hidden');
        }

        // Local Memory & Firestore Fallback Storage
        let guestResponses = [];

        async function saveRsvpToFirestore(rsvpObj) {
            // Save locally
            guestResponses.push(rsvpObj);

            // Save to Firebase Firestore if initialized
            if (window.db && window.user) {
                try {
                    const colRef = collection(window.db, 'artifacts', window.appId, 'public', 'data', 'rsvp_responses');
                    await addDoc(colRef, rsvpObj);
                } catch (e) {
                    console.error("Firestore save error:", e);
                }
            }
            updateAdminStats();
        }

        // 7. Admin Panel Logic
        function toggleAdminModal() {
            const modal = document.getElementById('adminModal');
            modal.classList.toggle('hidden');
        }

        function checkAdminPassword() {
            const pwd = document.getElementById('adminPasswordInput').value;
            if (pwd === "12092026" || pwd === "admin") {
                document.getElementById('adminAuthScreen').classList.add('hidden');
                document.getElementById('adminContentScreen').classList.remove('hidden');
                document.getElementById('adminAuthErr').classList.add('hidden');
                updateAdminStats();
            } else {
                document.getElementById('adminAuthErr').classList.remove('hidden');
            }
        }

        // Sync Firestore data in real-time for Admin
        window.loadRsvpData = function() {
            if (!window.db || !window.user) return;

            try {
                const colRef = collection(window.db, 'artifacts', window.appId, 'public', 'data', 'rsvp_responses');
                onSnapshot(colRef, (snapshot) => {
                    guestResponses = [];
                    snapshot.forEach((doc) => {
                        guestResponses.push(doc.data());
                    });
                    updateAdminStats();
                }, (error) => {
                    console.warn("Firestore snapshot error:", error);
                });
            } catch (err) {
                console.warn("Realtime listener error:", err);
            }
        };

        function updateAdminStats() {
            const total = guestResponses.length;
            const yesList = guestResponses.filter(r => r.attending);
            const noList = guestResponses.filter(r => !r.attending);
            const totalGuests = yesList.reduce((acc, curr) => acc + (parseInt(curr.count) || 1), 0);

            document.getElementById('statTotal').innerText = total;
            document.getElementById('statYes').innerText = yesList.length;
            document.getElementById('statNo').innerText = noList.length;
            document.getElementById('statGuestCount').innerText = totalGuests;

            // Render Table
            const tbody = document.getElementById('adminGuestTableBody');
            tbody.innerHTML = '';

            if (guestResponses.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" class="p-4 text-center text-stone-400">Әлі жауаптар тіркелмеген</td></tr>`;
                return;
            }

            guestResponses.forEach(item => {
                const tr = document.createElement('tr');
                const dateStr = item.timestamp ? new Date(item.timestamp).toLocaleString('kk-KZ') : '-';
                tr.innerHTML = `
                    <td class="p-3 font-medium text-charcoal">${escapeHtml(item.name || 'Белгісіз')}</td>
                    <td class="p-3">
                        ${item.attending 
                            ? `<span class="px-2 py-0.5 rounded-full bg-emerald-100 text-emerald-800 text-[10px] font-bold">Келеді</span>`
                            : `<span class="px-2 py-0.5 rounded-full bg-rose-100 text-rose-800 text-[10px] font-bold">Келе алмайды</span>`}
                    </td>
                    <td class="p-3 font-semibold">${item.attending ? item.count : 0}</td>
                    <td class="p-3 text-stone-400 text-[10px]">${dateStr}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        }

        // Export data to CSV
        function exportToCSV() {
            if (guestResponses.length === 0) {
                alert("Жүктеп алатын деректер жоқ!");
                return;
            }

            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += "Аты-жөні,Жауабы,Қонақ саны,Уақыты\n";

            guestResponses.forEach(r => {
                const status = r.attending ? "Келеді" : "Келе алмайды";
                const count = r.attending ? r.count : 0;
                const time = r.timestamp ? new Date(r.timestamp).toLocaleString('kk-KZ') : '';
                csvContent += `"${r.name || ''}","${status}",${count},"${time}"\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `konaktar_tizimi_12092026.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>
</body>
</html>
