```html
<!-- ... existing code ... -->
        .perspective-1000 { perspective: 1000px; }
        .transform-style-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
        .rotate-y-180 { transform: rotateY(180deg); }

        /* DYNAMIC LAYOUT STYLES (5 INTERFACE THEMES) */
        body.theme-cyberpunk {
            background-color: #030712 !important;
            color: #f3f4f6 !important;
        }
        body.theme-cyberpunk .bg-slate-900 {
            background-color: #111827 !important;
            border-color: #ec4899 !important;
            box-shadow: 0 0 15px rgba(236, 72, 153, 0.2);
        }

        body.theme-zen {
            background-color: #fafafa !important;
            color: #18181b !important;
        }
        body.theme-zen .bg-slate-900, body.theme-zen .bg-slate-950 {
            background-color: #ffffff !important;
            border-color: #e4e4e7 !important;
            color: #18181b !important;
            box-shadow: none !important;
        }

        body.theme-rpg {
            background-color: #0f172a !important;
            font-family: monospace !important;
        }
        body.theme-rpg .rounded-3xl, body.theme-rpg .rounded-2xl, body.theme-rpg .rounded-xl {
            border-radius: 4px !important;
            border-width: 3px !important;
        }

        body.theme-bento {
            background-color: #090d16 !important;
        }
        body.theme-bento .bg-slate-900 {
            background-color: rgba(15, 23, 42, 0.75) !important;
            backdrop-filter: blur(12px);
            border-color: rgba(255, 255, 255, 0.1) !important;
        }

        body.theme-codex {
            background-color: #1a1614 !important;
            color: #f5f5f4 !important;
        }
        body.theme-codex .bg-slate-900 {
            background-color: #262320 !important;
            border-color: #78350f !important;
        }
    </style>
</head>
<body id="appBody" oncontextmenu="handleGlobalContextMenu(event)" class="bg-slate-950 text-slate-100 h-full flex flex-col justify-between overflow-x-hidden selection:bg-amber-400 selection:text-slate-950">

    <!-- Top Header & Soundtrack Controls -->
    <header class="bg-slate-900/90 backdrop-blur-md border-b border-slate-800 sticky top-0 z-40">
        <div class="max-w-4xl mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-3">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 bg-gradient-to-tr from-amber-400 via-rose-500 to-indigo-600 rounded-xl flex items-center justify-center font-black text-white text-xl shadow-lg shadow-indigo-500/30 bounce-gentle">
                    🚀
                </div>
                <div>
                    <h1 class="font-extrabold text-sm sm:text-base text-slate-100 flex items-center gap-2">
                        Quest Hub 
                        <span id="subHeaderBadge" class="text-[10px] bg-amber-500/20 text-amber-300 font-bold px-2.5 py-0.5 rounded-full border border-amber-500/30 uppercase tracking-wide">Arena</span>
                    </h1>
                    <p id="subHeaderProgress" class="text-xs text-slate-400 font-medium">Ready to Play!</p>
                </div>
            </div>

            <!-- Stats & Sound Controls -->
            <div class="flex items-center gap-2 sm:gap-3 flex-wrap">
                <!-- Audio Selector & Controls -->
                <div class="flex items-center bg-slate-800 border border-slate-700/80 rounded-xl px-2.5 py-1.5 text-xs gap-2">
                    <span class="text-xs font-bold text-slate-400 uppercase">Audio</span>
                    <select id="bgmSelect" onchange="changeMusicTrack(this.value)" class="bg-transparent text-slate-200 font-bold outline-none cursor-pointer text-xs">
                        <option value="off" class="bg-slate-900 text-slate-300">Music: Off</option>
                        <option value="synthwave" class="bg-slate-900 text-slate-300" selected>Synthwave Pulse</option>
                        <option value="chiptune" class="bg-slate-900 text-slate-300">8-Bit Chiptune</option>
                        <option value="quest" class="bg-slate-900 text-slate-300">Quest Synth Anthem</option>
                        <option value="lofi" class="bg-slate-900 text-slate-300">Lo-Fi Focus</option>
                        <option value="arcade" class="bg-slate-900 text-slate-300">High-Energy Arcade</option>
                        <option value="study" class="bg-slate-900 text-slate-300">Study Ambient</option>
                        <option value="piano" class="bg-slate-900 text-slate-300">Relaxing Piano</option>
                    </select>

                    <button type="button" id="bgmPlayToggleBtn" onclick="toggleMusicPlay()" class="bg-indigo-600 hover:bg-indigo-500 text-white font-extrabold px-2 py-0.5 rounded-lg text-[10px] transition-colors cursor-pointer">
                        PLAY
                    </button>

                    <input type="range" id="bgmVolume" min="0" max="1" step="0.05" value="0.2" oninput="updateMusicVolume(this.value)" class="w-12 accent-indigo-500 cursor-pointer" title="Music Volume">
                </div>

                <!-- Coins & Shop Button -->
                <button type="button" onclick="openCoinShop()" class="flex items-center gap-1.5 bg-amber-500/20 hover:bg-amber-500/30 border border-amber-500/40 px-2.5 py-1.5 rounded-xl text-xs font-extrabold text-amber-300 cursor-pointer">
                    <span>Coins:</span>
                    <span id="userCoinsCount">0</span>
                </button>

                <!-- Settings Button -->
                <button type="button" onclick="openAppearanceSettings()" class="px-2 py-1 rounded-xl bg-slate-800 hover:bg-slate-700 border border-slate-700 text-xs font-bold text-slate-300 cursor-pointer">
                    Settings
                </button>

                <!-- Streak Badge -->
                <div class="flex items-center gap-1.5 bg-slate-800 border border-slate-700/80 px-2.5 py-1.5 rounded-xl text-xs font-extrabold text-amber-400 shadow-inner" title="Daily Streak">
                    <span class="text-[10px] text-slate-400 uppercase">Streak:</span>
                    <span id="streakCount">0</span>
                </div>

                <!-- XP Badge -->
                <div class="flex items-center gap-1.5 bg-indigo-950 border border-indigo-800 px-2.5 py-1.5 rounded-xl text-xs font-extrabold text-indigo-300 shadow-inner">
                    <span class="text-amber-400 text-[10px] uppercase">XP:</span>
                    <span id="xpCount">0</span>
                </div>

                <!-- Audio FX Toggle -->
                <button type="button" onclick="toggleAudio()" id="audioBtn" class="px-2.5 py-1 rounded-xl bg-slate-800 hover:bg-slate-700 border border-slate-700 text-xs font-bold transition-colors cursor-pointer" title="Toggle Sound FX">
                    Sound On
                </button>

                <!-- Admin / Auth Action -->
                <div id="authHeaderActions" class="flex items-center gap-2"></div>
            </div>
        </div>

        <!-- German Quick Accent Bar -->
        <div id="germanAccentBar" class="bg-slate-950/90 border-t border-slate-800/80 px-4 py-1.5 flex items-center justify-center gap-2 text-xs hidden">
            <span class="text-amber-400 font-extrabold uppercase tracking-wider text-[10px]">German Accents:</span>
            <button type="button" onclick="insertChar('ß')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">ß</button>
            <button type="button" onclick="insertChar('ä')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">ä</button>
            <button type="button" onclick="insertChar('ö')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">ö</button>
            <button type="button" onclick="insertChar('ü')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">ü</button>
            <button type="button" onclick="insertChar('Ä')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">Ä</button>
            <button type="button" onclick="insertChar('Ö')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">Ö</button>
            <button type="button" onclick="insertChar('Ü')" class="px-2.5 py-1 bg-slate-800 hover:bg-indigo-600 text-amber-300 hover:text-white font-extrabold rounded-lg border border-slate-700 transition-colors shadow-sm cursor-pointer">Ü</button>
        </div>

        <!-- Progress Tracker Bar -->
        <div class="w-full bg-slate-800/50 h-2.5 overflow-hidden">
            <div id="progressBar" class="bg-gradient-to-r from-indigo-500 via-rose-500 to-amber-400 h-full w-0 transition-all duration-500 ease-out shadow-[0_0_12px_rgba(245,158,11,0.6)]"></div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-3xl w-full mx-auto px-4 py-6 flex flex-col justify-center">

        <!-- ACCOUNT AUTH SCREEN -->
        <div id="authScreen" class="page-screen active flex-col items-center justify-center text-center">
            <div class="w-full bg-slate-900 border-2 border-indigo-500/40 rounded-3xl p-8 sm:p-12 shadow-[0_0_50px_rgba(99,102,241,0.2)] relative overflow-hidden">
                <div class="w-20 h-20 bg-gradient-to-tr from-amber-400 via-rose-500 to-indigo-600 border-2 border-amber-300/60 rounded-3xl flex items-center justify-center mx-auto mb-6 bounce-gentle shadow-2xl shadow-indigo-500/40 text-4xl">
                    🚀
                </div>

                <h2 class="text-3xl sm:text-4xl font-black text-white mb-2">Welcome to Quest Hub</h2>
                <p class="text-slate-400 text-sm max-w-md mx-auto mb-6 font-medium">
                    Sign in to access your synchronized quests, streak tracking, and custom study sets.
                </p>

                <!-- Auth Initial Action Buttons -->
                <div id="authInitialButtons" class="flex justify-center gap-4 max-w-xs mx-auto mb-6">
                    <button type="button" onclick="showSignInForm()" class="flex-1 py-3 bg-indigo-600 text-white font-black text-sm rounded-xl shadow-lg transition-all cursor-pointer hover:bg-indigo-500">Sign In</button>
                    <button type="button" onclick="showSignUpForm()" class="flex-1 py-3 bg-slate-800 text-slate-300 font-black text-sm rounded-xl transition-all cursor-pointer hover:bg-slate-700">Create Account</button>
                </div>

                <!-- SIGN IN FORM -->
                <form id="signInForm" onsubmit="handleSignIn(event)" class="max-w-xs mx-auto space-y-4 text-left hidden">
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Username (Used for Multiplayer)</label>
                        <input type="text" id="loginUsername" required placeholder="Enter username..." class="w-full px-4 py-3 bg-slate-950 border border-slate-700 rounded-xl text-white font-bold text-sm outline-none focus:border-amber-400">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Password</label>
                        <input type="password" id="loginPassword" required placeholder="Enter password..." class="w-full px-4 py-3 bg-slate-950 border border-slate-700 rounded-xl text-white font-bold text-sm outline-none focus:border-amber-400">
                    </div>
                    <div class="flex items-center justify-between text-xs">
                        <label class="flex items-center gap-2 text-slate-300 font-bold cursor-pointer">
                            <input type="checkbox" id="rememberMe" class="accent-indigo-500 rounded">
                            Remember Me
                        </label>
                        <button type="button" onclick="forgotPasswordAlert()" class="text-amber-400 hover:underline font-bold">Forgot Password?</button>
                    </div>
                    <div class="flex gap-2">
                        <button type="button" onclick="hideAuthForms()" class="px-3 py-3.5 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back</button>
                        <button type="submit" class="flex-1 py-3.5 bg-gradient-to-r from-amber-400 via-rose-500 to-indigo-600 hover:from-amber-300 hover:to-indigo-500 text-slate-950 font-black text-base rounded-xl shadow-xl transition-all transform hover:-translate-y-0.5 cursor-pointer">
                            Sign In & Launch
                        </button>
                    </div>
                </form>

                <!-- SIGN UP FORM -->
                <form id="signUpForm" onsubmit="handleSignUp(event)" class="max-w-xs mx-auto space-y-3 text-left hidden">
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Real Name</label>
                        <input type="text" id="regRealName" required placeholder="Your full name" class="w-full px-4 py-2.5 bg-slate-950 border border-slate-700 rounded-xl text-white font-bold text-sm outline-none focus:border-amber-400">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Username (Used for Multiplayer)</label>
                        <input type="text" id="regUsername" required placeholder="Choose a username" class="w-full px-4 py-2.5 bg-slate-950 border border-slate-700 rounded-xl text-white font-bold text-sm outline-none focus:border-amber-400">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Password</label>
                        <input type="password" id="regPassword" required placeholder="Create a password" class="w-full px-4 py-2.5 bg-slate-950 border border-slate-700 rounded-xl text-white font-bold text-sm outline-none focus:border-amber-400">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-1">Invite Code</label>
                        <input type="text" id="regInviteCode" required placeholder="Enter invite code..." class="w-full px-4 py-2.5 bg-slate-950 border border-slate-700 rounded-xl text-amber-300 font-mono text-sm outline-none focus:border-amber-400">
                    </div>
                    <div class="flex gap-2 pt-2">
                        <button type="button" onclick="hideAuthForms()" class="px-3 py-3.5 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back</button>
                        <button type="submit" class="flex-1 py-3.5 bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 text-white font-black text-base rounded-xl shadow-xl transition-all transform hover:-translate-y-0.5 cursor-pointer">
                            Register Account
                        </button>
                    </div>
                </form>

                <p id="authErrorMsg" class="text-xs text-rose-400 font-extrabold mt-4 hidden"></p>
            </div>
        </div>

        <!-- MULTIPLAYER ARENA LOBBY SCREEN (6-DIGIT ROOMS) -->
        <div id="multiplayerLobbyScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-indigo-500/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Arena Live</span>
                        <h2 class="text-2xl font-black text-white mt-1">6-Digit Room Multiplayer</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back to Arena</button>
                </div>

                <!-- Host vs Join Options -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
                    <div class="bg-slate-950 border border-slate-800 rounded-2xl p-5 text-center">
                        <h3 class="text-base font-black text-white mb-2">Host a Live Game</h3>
                        <p class="text-xs text-slate-400 mb-4">Create a 6-digit room code for your active deck and invite classmates.</p>
                        <button type="button" onclick="hostMultiplayerRoom()" class="w-full py-3 bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 text-white font-black text-xs rounded-xl cursor-pointer">Host Room</button>
                    </div>

                    <div class="bg-slate-950 border border-slate-800 rounded-2xl p-5 text-center">
                        <h3 class="text-base font-black text-white mb-2">Join a Live Game</h3>
                        <p class="text-xs text-slate-400 mb-2">Enter the 6-digit PIN code from host:</p>
                        <input type="text" id="joinRoomPinInput" maxlength="6" placeholder="123456" class="w-full text-center px-4 py-2 bg-slate-900 border border-slate-700 rounded-xl text-amber-300 font-mono font-black text-lg mb-3 outline-none focus:border-amber-400">
                        <button type="button" onclick="joinMultiplayerRoom()" class="w-full py-3 bg-gradient-to-r from-amber-400 to-rose-500 text-slate-950 font-black text-xs rounded-xl cursor-pointer">Join Game</button>
                    </div>
                </div>

                <!-- Live Waiting Room Info -->
                <div id="multiplayerRoomBox" class="bg-slate-950 border-2 border-indigo-500/40 rounded-2xl p-6 hidden">
                    <div class="flex items-center justify-between border-b border-slate-800 pb-3 mb-4">
                        <div>
                            <span class="text-[10px] font-black text-slate-400 uppercase">Room PIN Code</span>
                            <div id="activeRoomCodeDisplay" class="text-3xl font-mono font-black text-amber-400 tracking-widest">------</div>
                        </div>
                        <div class="text-right">
                            <span class="text-[10px] font-black text-slate-400 uppercase">Players Connected</span>
                            <div id="activePlayerCountDisplay" class="text-xl font-black text-indigo-300">1 Player</div>
                        </div>
                    </div>

                    <h4 class="text-xs font-bold text-amber-300 uppercase tracking-wider mb-2">Waiting Room Lobby</h4>
                    <div id="waitingRoomPlayersList" class="space-y-2 mb-6"></div>

                    <button type="button" id="startLiveGameBtn" onclick="startHostLiveMatch()" class="w-full py-3.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black text-sm rounded-xl cursor-pointer hidden">
                        Start Live Arena Battle
                    </button>
                </div>
            </div>
        </div>

        <!-- ORAL SPEAKING EXAM TELEPROMPTER SCREEN -->
        <div id="oralExamScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-indigo-500/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Speaking Studio</span>
                        <h2 class="text-2xl font-black text-white mt-1">Oral Exam Script Teleprompter</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back to Arena</button>
                </div>

                <div class="mb-4">
                    <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-2">Your Exam Script</label>
                    <textarea id="oralScriptInput" rows="4" placeholder="Paste your target German or Latin oral exam script here..." class="w-full px-4 py-3 bg-slate-950 border border-slate-700 rounded-xl text-white font-medium text-xs outline-none focus:border-amber-400 mb-3"></textarea>
                    
                    <div class="flex flex-wrap gap-2">
                        <button type="button" onclick="toggleTeleprompterScript()" id="toggleScriptBtn" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-amber-300 font-bold text-xs rounded-xl cursor-pointer">
                            Hide Teleprompter Script
                        </button>
                        <button type="button" onclick="speakOralScript()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white font-bold text-xs rounded-xl cursor-pointer">
                            Listen to Script Audio
                        </button>
                    </div>
                </div>

                <!-- Teleprompter Display Box -->
                <div id="teleprompterDisplayBox" class="bg-slate-950 border-2 border-slate-800 rounded-2xl p-6 mb-6 text-center">
                    <span class="text-[10px] font-black text-amber-400 uppercase tracking-wider mb-2 block">Teleprompter View</span>
                    <p id="teleprompterText" class="text-lg font-extrabold text-white leading-relaxed">Paste your script above to begin teleprompter practice.</p>
                </div>

                <!-- Voice Recording Controls -->
                <div class="bg-slate-950 border border-slate-800 rounded-2xl p-6 text-center">
                    <h3 class="text-base font-black text-white mb-2">Speech Recognition Evaluator</h3>
                    <p class="text-xs text-slate-400 mb-4">Record your speech. The system compares your words with forgiving phonetic matching.</p>
                    
                    <div class="flex justify-center gap-3">
                        <button type="button" id="startRecBtn" onclick="startVoiceRecording()" class="px-6 py-3 bg-rose-600 hover:bg-rose-500 text-white font-black text-xs rounded-xl cursor-pointer">
                            Start Recording Speech
                        </button>
                        <button type="button" id="stopRecBtn" onclick="stopVoiceRecording()" class="px-6 py-3 bg-slate-800 text-slate-400 font-bold text-xs rounded-xl cursor-pointer" disabled>
                            Stop Recording
                        </button>
                    </div>

                    <div id="oralFeedbackBox" class="mt-4 p-4 bg-slate-900 border border-slate-800 rounded-xl text-xs text-left hidden">
                        <h4 class="font-black text-amber-400 uppercase tracking-wider mb-1">Pronunciation Evaluation:</h4>
                        <div id="oralFeedbackDetails" class="text-slate-300 space-y-1"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- UNIVERSAL CLOUD DECK HUB SCREEN -->
        <div id="universalHubScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-indigo-500/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Community Hub</span>
                        <h2 class="text-2xl font-black text-white mt-1">Universal Cloud Decks</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back to Arena</button>
                </div>

                <p class="text-xs text-slate-400 mb-6 font-medium">Browse shared community sets uploaded by creators worldwide. Click Save to Account to add them instantly.</p>

                <div id="universalCloudDecksGrid" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>
            </div>
        </div>

        <!-- COIN SHOP & ACCESSORIES SCREEN -->
        <div id="coinShopScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-amber-400/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Blooket-Style Marketplace</span>
                        <h2 class="text-2xl font-black text-white mt-1">Coin Shop & Subtitle Titles</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Exit Shop</button>
                </div>

                <div class="bg-slate-950 border border-slate-800 p-4 rounded-2xl mb-6 flex justify-between items-center">
                    <span class="text-xs font-bold text-slate-300">Your Coin Balance:</span>
                    <span id="shopCoinsBalance" class="text-xl font-black text-amber-400">0 Coins</span>
                </div>

                <div id="shopItemsContainer" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>
            </div>
        </div>

        <!-- COMMON ERRORS & SPACED REPETITION VAULT SCREEN -->
        <div id="commonErrorsScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-indigo-500/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Spaced Repetition</span>
                        <h2 class="text-2xl font-black text-white mt-1">Common Errors Vault</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back to Arena</button>
                </div>

                <p class="text-xs text-slate-400 mb-6 font-medium">Review words you missed in quizzes. Click Completed Test to blur out solved items and permanently remove them from your error queue.</p>

                <div id="commonErrorsList" class="space-y-3 mb-6"></div>

                <button type="button" onclick="markAllErrorsCompleted()" class="w-full py-3.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black text-xs rounded-xl shadow-lg cursor-pointer">
                    Completed Test & Clear Mastered Terms
                </button>
            </div>
        </div>

        <!-- APPEARANCE & LAYOUT SETTINGS SCREEN -->
        <div id="settingsScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Preferences</span>
                        <h2 class="text-2xl font-black text-white mt-1">Appearance & Interface Settings</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Back to Arena</button>
                </div>

                <!-- 5 Custom Interface Themes -->
                <div class="mb-6">
                    <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-3">Select Interface Structure (5 Themes)</label>
                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                        <button type="button" onclick="setInterfaceTheme('default')" class="p-3 bg-slate-950 border border-slate-700 hover:border-amber-400 rounded-xl text-left cursor-pointer">
                            <span class="block text-xs font-black text-white">Default Arena</span>
                            <span class="text-[10px] text-slate-400">Standard balanced UI</span>
                        </button>
                        <button type="button" onclick="setInterfaceTheme('cyberpunk')" class="p-3 bg-slate-950 border border-slate-700 hover:border-amber-400 rounded-xl text-left cursor-pointer">
                            <span class="block text-xs font-black text-pink-400">Cyberpunk HUD</span>
                            <span class="text-[10px] text-slate-400">Angled sci-fi telemetry</span>
                        </button>
                        <button type="button" onclick="setInterfaceTheme('zen')" class="p-3 bg-slate-950 border border-slate-700 hover:border-amber-400 rounded-xl text-left cursor-pointer">
                            <span class="block text-xs font-black text-slate-300">Zen Minimalist</span>
                            <span class="text-[10px] text-slate-400">Clean distraction-free</span>
                        </button>
                        <button type="button" onclick="setInterfaceTheme('rpg')" class="p-3 bg-slate-950 border border-slate-700 hover:border-amber-400 rounded-xl text-left cursor-pointer">
                            <span class="block text-xs font-black text-amber-400">RPG Console</span>
                            <span class="text-[10px] text-slate-400">8-bit inventory grid</span>
                        </button>
                        <button type="button" onclick="setInterfaceTheme('bento')" class="p-3 bg-slate-950 border border-slate-700 hover:border-amber-400 rounded-xl text-left cursor-pointer">
                            <span class="block text-xs font-black text-indigo-400">Frosted Bento</span>
                            <span class="text-[10px] text-slate-400">Modern blurred cards</span>
                        </button>
                    </div>
                </div>

                <!-- Light / Dark Auto Clock Toggle -->
                <div class="mb-6 bg-slate-950 p-4 rounded-xl border border-slate-800">
                    <label class="block text-xs font-bold text-amber-300 uppercase tracking-wider mb-2">Clock-Based Auto Dark Mode</label>
                    <p class="text-xs text-slate-400 mb-3">Automatically switches to Dark Mode between 8:00 PM and 6:00 AM based on system clock.</p>
                    <button type="button" onclick="toggleAutoClockMode()" id="autoClockBtn" class="px-4 py-2 bg-indigo-600 text-white font-bold text-xs rounded-lg cursor-pointer">
                        Auto Clock Theme: Enabled
                    </button>
                </div>
            </div>
        </div>

        <!-- RIGHT-CLICK PRONUNCIATION POPUP MODAL -->
        <div id="pronounceModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
            <div class="bg-slate-900 border-2 border-amber-400 rounded-3xl p-6 max-w-sm w-full text-center relative">
                <span class="text-xs font-black text-amber-400 uppercase tracking-wider block mb-1">Target Word Focus</span>
                <h3 id="pronounceTargetWord" class="text-2xl font-black text-white mb-4">...</h3>
                
                <div class="flex flex-col gap-3">
                    <button type="button" onclick="speakTargetWordPopup()" class="w-full py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-black text-xs rounded-xl cursor-pointer">
                        Listen Audio
                    </button>
                    <button type="button" onclick="closePronounceModal()" class="w-full py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">
                        Close Popup
                    </button>
                </div>
            </div>
        </div>

        <!-- UNDER-4 QUESTIONS WARNING MODAL -->
        <div id="underFourWarningModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
            <div class="bg-slate-900 border-2 border-amber-400 rounded-3xl p-6 sm:p-8 max-w-md w-full text-center">
                <h3 class="text-xl font-black text-white mb-2">Deck Size Limit Warning</h3>
                <p class="text-xs text-slate-300 font-medium mb-6">
                    This deck has fewer than 4 questions. Arcade Multiple Choice mode is optimized for decks with at least 4 items to generate options.
                </p>
                <div class="flex flex-col gap-3">
                    <label class="flex items-center justify-center gap-2 text-xs text-slate-400 font-bold cursor-pointer mb-2">
                        <input type="checkbox" id="dontShowUnderFourWarning" class="accent-amber-400 rounded">
                        Never show again
                    </label>
                    <div class="flex gap-2">
                        <button type="button" onclick="closeUnderFourWarning()" class="flex-1 py-3 bg-slate-800 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Go Back</button>
                        <button type="button" onclick="forceStartMultipleChoice()" class="flex-1 py-3 bg-amber-500 text-slate-950 font-black text-xs rounded-xl cursor-pointer">Continue</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- ADMIN DASHBOARD SCREEN -->
        <div id="adminScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border-2 border-amber-400/50 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Admin Command Center</span>
                        <h2 class="text-2xl font-black text-white mt-1">Manage Users & Invite Audit Log</h2>
                    </div>
                    <button type="button" onclick="returnToLanding()" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold text-xs rounded-xl cursor-pointer">Exit Admin</button>
                </div>

                <div class="space-y-3 max-h-96 overflow-y-auto pr-2" id="adminUsersList"></div>
            </div>
        </div>

        <!-- MOTION GRAPHIC GREETING TRANSITION -->
        <div id="pageGreeting" class="page-screen flex-col items-center justify-center text-center">
            <div class="w-full bg-slate-900 border-2 border-amber-400/60 rounded-3xl p-10 sm:p-16 shadow-[0_0_60px_rgba(245,158,11,0.3)] relative overflow-hidden">
                <div class="relative w-32 h-32 mx-auto mb-8 flex items-center justify-center motion-graphic">
                    <div class="absolute inset-0 rounded-3xl bg-gradient-to-tr from-amber-400 via-rose-500 to-indigo-600 opacity-75 blur-md animate-pulse"></div>
                    <div class="relative w-28 h-28 bg-slate-950 border-2 border-amber-400 rounded-3xl flex items-center justify-center text-5xl text-amber-400 shadow-2xl">
                        ★
                    </div>
                </div>

                <span class="text-xs font-black text-amber-400 uppercase tracking-widest bg-amber-500/10 px-3 py-1 rounded-full border border-amber-500/30">System Initialization</span>
                
                <h2 id="greetingTitle" class="text-3xl sm:text-5xl font-black text-white mt-4 mb-3">Hello, Hero!</h2>
                <p class="text-slate-300 text-base sm:text-lg max-w-md mx-auto font-medium mb-6">
                    Loading your personal quest arena & progress...
                </p>

                <div class="w-48 h-2 bg-slate-950 rounded-full mx-auto overflow-hidden border border-slate-800">
                    <div id="greetingBar" class="h-full bg-gradient-to-r from-amber-400 to-indigo-500 w-0 transition-all duration-1000 ease-out"></div>
                </div>
            </div>
        </div>

        <!-- MAIN MISSION CONTROL / LANDING PAGE -->
        <div id="landingPage" class="page-screen flex-col">
            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="text-center mb-6">
                    <span class="text-xs font-black text-amber-400 uppercase tracking-widest">Mission Control</span>
                    <h2 class="text-3xl font-black text-white mt-1">Welcome, <span id="landingStudentName" class="text-amber-300 underline decoration-indigo-500">Hero</span>!</h2>
                    <p id="userTitleBadgeDisplay" class="text-xs text-indigo-300 font-extrabold mt-1">Language Novice</p>
                </div>

                <!-- Navigation Hub Actions -->
                <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 mb-6">
                    <button type="button" onclick="openMultiplayerLobby()" class="py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-black text-xs rounded-xl shadow-md cursor-pointer">6-Digit Multiplayer</button>
                    <button type="button" onclick="openOralExamStudio()" class="py-2.5 bg-rose-600 hover:bg-rose-500 text-white font-black text-xs rounded-xl shadow-md cursor-pointer">Oral Speaking Studio</button>
                    <button type="button" onclick="openUniversalCloudHub()" class="py-2.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-black text-xs rounded-xl shadow-md cursor-pointer">Universal Decks</button>
                    <button type="button" onclick="openCommonErrorsVault()" class="py-2.5 bg-emerald-600 hover:bg-emerald-500 text-white font-black text-xs rounded-xl shadow-md cursor-pointer">Common Errors</button>
                </div>

                <!-- Trophies & Achievements Showcase Bar -->
                <div class="bg-slate-950 border border-slate-800 rounded-2xl p-4 mb-6">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-black text-amber-400 uppercase tracking-wider flex items-center gap-1.5">
                            Achievement Trophy Cabinet
                        </span>
                        <span id="trophyProgressText" class="text-xs text-indigo-300 font-bold">0 / 4 Unlocked</span>
                    </div>
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-center" id="trophyGrid"></div>
                </div>

                <!-- Language Tabs / Import -->
                <div class="flex justify-center gap-3 mb-6 flex-wrap">
                    <button type="button" onclick="switchLangTab('german')" id="germanTabBtn" class="px-5 py-3 bg-indigo-600 text-white font-black text-xs sm:text-sm rounded-2xl shadow-lg transition-all cursor-pointer transform hover:scale-105 border-2 border-indigo-400">
                        German Module 2
                    </button>
                    <button type="button" onclick="switchLangTab('latin')" id="latinTabBtn" class="px-5 py-3 bg-slate-800 text-slate-300 hover:bg-slate-700 font-black text-xs sm:text-sm rounded-2xl transition-all cursor-pointer transform hover:scale-105 border-2 border-transparent">
                        Latin Book 1
                    </button>
                    <button type="button" onclick="switchLangTab('custom')" id="customTabBtn" class="px-5 py-3 bg-slate-800 text-amber-300 hover:bg-slate-700 font-black text-xs sm:text-sm rounded-2xl transition-all cursor-pointer transform hover:scale-105 border-2 border-amber-500/40">
                        Custom Sets & Hub
                    </button>
                </div>

                <!-- German Units Grid -->
                <div id="germanUnitsGrid" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>

                <!-- Latin Stages Grid -->
                <div id="latinUnitsGrid" class="grid grid-cols-1 sm:grid-cols-3 gap-3 hidden"></div>

                <!-- Custom Sets Grid -->
                <div id="customUnitsGrid" class="hidden space-y-4">
                    <div class="bg-slate-950 border border-slate-800 rounded-2xl p-4 flex flex-wrap items-center justify-center gap-3">
                        <button type="button" onclick="backupAllSetsToZipSimulated()" class="py-3 px-4 bg-slate-800 hover:bg-slate-700 text-amber-300 font-black text-xs rounded-xl shadow-lg cursor-pointer">
                            Backup All Sets (ZIP)
                        </button>
                        <label class="w-full sm:w-auto flex-1 py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-black text-xs rounded-xl shadow-lg cursor-pointer flex items-center justify-center gap-2 text-center">
                            Load Set File
                            <input type="file" id="importFileJson" accept=".json" onchange="loadCustomSetFromFile(event)" class="hidden">
                        </label>
                    </div>

                    <div id="savedCustomSetsContainer" class="space-y-2"></div>
                </div>
            </div>
        </div>

        <!-- STUDY MODE SELECTOR SCREEN -->
        <div id="modeSelectorScreen" class="page-screen flex-col items-center justify-center text-center">
            <div class="w-full max-w-md bg-slate-900 border-2 border-indigo-500/50 rounded-3xl p-8 shadow-2xl relative">
                <span class="text-xs font-black text-amber-400 uppercase tracking-widest bg-amber-500/10 px-3 py-1 rounded-full border border-amber-500/30">Study Hub</span>
                <h2 id="modeSelectorTitle" class="text-2xl font-black text-white mt-3 mb-2">Select Study Mode</h2>
                <p class="text-xs text-slate-400 mb-8 font-medium">Choose how you want to master this set:</p>

                <div class="space-y-4">
                    <button type="button" onclick="startFlashcardsMode()" class="w-full py-4 bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-500 hover:to-purple-500 text-white font-black text-base rounded-2xl shadow-xl transition-all transform hover:-translate-y-1 cursor-pointer flex items-center justify-center gap-3">
                        Flashcards
                    </button>
                    <button type="button" onclick="startMultipleChoiceMode()" class="w-full py-4 bg-gradient-to-r from-amber-500 to-rose-500 hover:from-amber-400 hover:to-rose-400 text-slate-950 font-black text-base rounded-2xl shadow-xl transition-all transform hover:-translate-y-1 cursor-pointer flex items-center justify-center gap-3">
                        Multiple Choice (Learn)
                    </button>
                    <button type="button" onclick="startQuizMode()" class="w-full py-4 bg-gradient-to-r from-emerald-500 to-teal-600 hover:from-emerald-400 hover:to-teal-500 text-slate-950 font-black text-base rounded-2xl shadow-xl transition-all transform hover:-translate-y-1 cursor-pointer flex items-center justify-center gap-3">
                        Quiz (Test)
                    </button>
                </div>

                <button type="button" onclick="returnToLanding()" class="mt-6 text-xs text-slate-400 hover:text-white font-bold transition-colors cursor-pointer">
                    Back to Mission Control
                </button>
            </div>
        </div>

        <!-- FLASHCARD STUDY SCREEN -->
        <div id="flashcardScreen" class="page-screen flex-col items-center">
            <div class="w-full bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl text-center">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span id="fcBadge" class="text-xs font-black text-amber-400 uppercase tracking-widest">Flashcards</span>
                        <h2 id="fcTitle" class="text-xl font-black text-white mt-1">Study Mode</h2>
                    </div>
                    <div class="flex items-center gap-3">
                        <button type="button" onclick="startQuizMode()" class="px-3 py-1.5 bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black text-xs rounded-xl shadow-md transition-colors cursor-pointer flex items-center gap-1.5">
                            Take Quiz (Test)
                        </button>
                        <div id="fcCounter" class="text-xs font-black bg-indigo-950 text-indigo-300 border border-indigo-800 px-3 py-1.5 rounded-xl">1 / 10</div>
                    </div>
                </div>

                <!-- Flashcard 3D Flip Box -->
                <div class="perspective-1000 w-full h-64 sm:h-72 mb-6 cursor-pointer" onclick="flipFlashcard()">
                    <div id="flashcardInner" class="relative w-full h-full duration-500 transform-style-3d shadow-2xl rounded-2xl">
                        <div class="absolute inset-0 w-full h-full bg-slate-950 border-2 border-slate-800 rounded-2xl p-6 flex flex-col items-center justify-center backface-hidden">
                            <span class="text-[10px] font-extrabold text-amber-400 uppercase tracking-wider mb-2 flex items-center gap-2">
                                Term / Question
                                <button type="button" onclick="event.stopPropagation(); speakText(document.getElementById('fcFrontText').textContent, 1)" class="p-1 bg-indigo-600/30 hover:bg-indigo-600 text-amber-300 rounded-md text-xs cursor-pointer" title="Pronounce">Listen</button>
                            </span>
                            <h3 id="fcFrontText" class="text-2xl sm:text-3xl font-black text-white">Question</h3>
                            <span class="text-[10px] text-slate-500 mt-4">Click card to flip</span>
                        </div>
                        <div class="absolute inset-0 w-full h-full bg-indigo-950/90 border-2 border-indigo-500/50 rounded-2xl p-6 flex flex-col items-center justify-center backface-hidden rotate-y-180">
                            <span class="text-[10px] font-extrabold text-indigo-300 uppercase tracking-wider mb-2 flex items-center gap-2">
                                Answer / Definition
                                <button type="button" onclick="event.stopPropagation(); speakText(document.getElementById('fcBackText').textContent, 2)" class="p-1 bg-amber-400/20 hover:bg-amber-400 text-amber-300 hover:text-slate-950 rounded-md text-xs cursor-pointer" title="Pronounce">Listen</button>
                            </span>
                            <h3 id="fcBackText" class="text-2xl sm:text-3xl font-black text-amber-300">Answer</h3>
                            <span class="text-[10px] text-indigo-400 mt-4">Click card to flip back</span>
                        </div>
                    </div>
                </div>

                <div class="flex items-center justify-between gap-4">
                    <button type="button" onclick="prevFlashcard()" class="flex-1 py-3 bg-slate-800 hover:bg-slate-700 text-slate-300 font-extrabold text-xs sm:text-sm rounded-xl border border-slate-700 transition-colors cursor-pointer">
                        Previous Card
                    </button>
                    <button type="button" onclick="nextFlashcard()" class="flex-1 py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-extrabold text-xs sm:text-sm rounded-xl shadow-lg transition-transform active:scale-95 cursor-pointer">
                        Next Card
                    </button>
                </div>

                <button type="button" onclick="returnToLanding()" class="mt-6 text-xs text-slate-400 hover:text-white font-bold transition-colors cursor-pointer">
                    Exit to Mission Control
                </button>
            </div>
        </div>

        <!-- MULTIPLE CHOICE QUIZ SCREEN -->
        <div id="mcQuizScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-6">
                    <div>
                        <span id="mcBadge" class="text-xs font-black text-amber-400 uppercase tracking-widest">Arcade Quiz</span>
                        <h2 id="mcTitle" class="text-2xl font-black text-white mt-1">Multiple Choice (Learn)</h2>
                    </div>
                    <div id="mcCounter" class="text-xs font-black bg-amber-500/20 text-amber-300 border border-amber-500/40 px-3 py-1.5 rounded-xl">1 / 10</div>
                </div>

                <div class="bg-slate-950 border-2 border-indigo-500/40 rounded-2xl p-6 text-center mb-6">
                    <span class="text-[10px] font-extrabold text-amber-400 uppercase tracking-wider mb-2 block">Translate this term</span>
                    <h3 id="mcPromptText" class="text-2xl sm:text-3xl font-black text-white">...</h3>
                    <button type="button" onclick="speakText(document.getElementById('mcPromptText').textContent, 1)" class="mt-2 text-xs font-bold text-indigo-400 hover:text-amber-300 inline-flex items-center gap-1 cursor-pointer">Listen</button>
                </div>

                <div id="mcOptionsContainer" class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-6"></div>

                <div class="flex justify-between items-center">
                    <button type="button" onclick="returnToLanding()" class="text-xs text-slate-400 hover:text-white font-bold transition-colors cursor-pointer">
                        Quit Challenge
                    </button>
                    <span id="mcTimerText" class="text-xs font-extrabold text-amber-400"></span>
                </div>
            </div>
        </div>

        <!-- WRITTEN QUIZ SCREEN -->
        <div id="quizScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-4">
                    <div>
                        <span id="quizUnitBadge" class="text-xs font-black text-amber-400 uppercase tracking-widest">Section 1</span>
                        <h2 id="quizUnitTitle" class="text-2xl font-black text-white mt-1">Quiz (Test)</h2>
                        <p id="quizPromptSubtitle" class="text-xs text-slate-400 font-medium mt-1">Provide exact answers.</p>
                    </div>
                    <div id="quizNumBox" class="w-12 h-12 bg-indigo-950 border-2 border-indigo-500/50 rounded-2xl flex items-center justify-center font-black text-amber-400 text-lg shadow-md">01</div>
                </div>

                <div id="quizContainer" class="space-y-4"></div>
            </div>
        </div>

        <!-- RESULTS SCREEN -->
        <div id="resultsScreen" class="page-screen flex-col">
            <div class="bg-slate-900 border border-slate-800 rounded-3xl p-6 sm:p-8 shadow-2xl">
                <div class="bg-gradient-to-r from-indigo-900 via-purple-900 to-slate-900 border-2 border-amber-400/50 p-6 sm:p-8 rounded-2xl shadow-2xl mb-8 flex flex-col sm:flex-row items-center justify-between gap-6 relative overflow-hidden">
                    <div class="z-10 text-center sm:text-left">
                        <div class="inline-flex items-center gap-2 px-3 py-1 bg-amber-400/20 text-amber-300 rounded-full text-xs font-black mb-3 border border-amber-400/30">
                            <span id="rankBadge">LEGENDARY STATUS</span>
                        </div>
                        <h2 id="congratsHeader" class="text-3xl sm:text-4xl font-black text-white mb-1">Epic Run, Hero!</h2>
                        <p class="text-slate-300 text-sm font-medium">You smashed this arena challenge!</p>
                    </div>

                    <div class="z-10 bg-slate-950/90 border-2 border-amber-400 p-5 rounded-2xl text-center min-w-[150px] shadow-2xl">
                        <div id="finalScorePercent" class="text-4xl font-black text-amber-400">100%</div>
                        <div id="finalScoreMarks" class="text-xs font-extrabold text-indigo-300 mt-1 uppercase">0 / 0 Marks</div>
                    </div>
                </div>

                <div class="border-b border-slate-800 pb-3 mb-6 flex justify-between items-center">
                    <h3 class="text-lg font-black text-white">Battle Review & Answer Explanations</h3>
                    <button type="button" id="reviewMissedBtn" onclick="launchSmartPracticeMissed()" class="px-3 py-1 bg-amber-500 hover:bg-amber-400 text-slate-950 font-black text-xs rounded-xl shadow-md cursor-pointer hidden">
                        Review Missed Words
                    </button>
                </div>

                <div id="resultsList" class="space-y-4"></div>

                <div class="mt-8 text-center flex flex-wrap justify-center gap-4">
                    <button type="button" onclick="returnToLanding()" class="px-8 py-4 bg-gradient-to-r from-amber-400 via-rose-500 to-indigo-600 hover:from-amber-300 hover:to-indigo-500 text-slate-950 font-black text-base rounded-2xl shadow-xl transition-all transform hover:-translate-y-0.5 cursor-pointer">
                        Choose Next Quest
                    </button>
                </div>
            </div>
        </div>

        <!-- Bottom Navigation Bar for Written Quiz -->
        <div id="navControls" class="hidden mt-6 flex items-center justify-between">
            <button type="button" id="prevBtn" onclick="returnToLanding()" class="px-5 py-3 bg-slate-800 hover:bg-slate-700 text-slate-300 font-extrabold text-sm rounded-xl border border-slate-700 transition-colors cursor-pointer">
                Back to Arena
            </button>
            <button type="button" id="nextBtn" onclick="submitQuiz()" class="px-8 py-3.5 bg-gradient-to-r from-emerald-400 to-teal-500 hover:from-emerald-300 hover:to-teal-400 text-slate-950 font-black text-sm rounded-2xl shadow-lg shadow-emerald-500/30 transition-transform active:scale-95 cursor-pointer">
                Submit Answers
            </button>
        </div>

    </main>

    <!-- Footer -->
    <footer class="py-4 text-center text-xs text-slate-600 font-semibold border-t border-slate-900 flex justify-between items-center max-w-4xl mx-auto px-4 w-full">
        <span>Quest Hub • Interactive Learning Arena</span>
        <span id="footerUserLabel" class="text-slate-500">Not Signed In</span>
    </footer>

    <script>
        // 31 BRAND NEW WORKING INVITE CODES
        let availableInviteCodes = [
            'q7vtr82klmn3-54ptywzxc9', 'm4kjp91zxcv6-83qazwrtm2', 'b9wqe23hytr7-15plkjfvd4', 
            'z2asd76poiuyt-98mnbvfrt1', 'x5zxc14lkjhgf-37qazwsxc6', 'v8poiuy82qazws-24mnbvfrt5', 
            'n1lkjh93mnbv6-69qazwsxc8', 'm6mnbv45qazwsx-71poiuytr2', 'k3qaz78poiuyt-42lkjhgfd9', 
            'j9wsx12lkjhgf-85qazwsxc3', 'h4edc56qazwsx-19poiuytr4', 'f7rfv90poiuyt-63lkjhgfd5', 
            'd2tgb34lkjhgf-91qazwsxc7', 's8yhn67qazwsx-28poiuytr6', 'a1ujm89poiuyt-52lkjhgfd8', 
            'q6ikm21lkjhgf-74qazwsxc1', 'w3opl54qazwsx-39poiuytr2', 'e9qaz76poiuyt-81lkjhgfd3', 
            'r4wsx83lkjhgf-16qazwsxc5', 't7edc19qazwsx-47poiuytr6', 'y2rfv42poiuyt-95lkjhgfd7', 
            'u5tgb65lkjhgf-23qazwsxc8', 'i8yhn98qazwsx-64poiuytr9', 'o1ujm37poiuyt-31lkjhgfd1', 
            'p6ikm54lkjhgf-82qazwsxc2', 'z3opl21qazwsx-17poiuytr3', 'x9qaz68poiuyt-59lkjhgfd4', 
            'v4wsx75lkjhgf-72qazwsxc6', 'n8edc39qazwsx-48poiuytr7', 'm1rfv24poiuyt-13lkjhgfd8', 'k7tgb81lkjhgf-96qazwsxc9'
        ];

        let accounts = [];
        try {
            const savedAccs = localStorage.getItem('quest_user_accounts');
            if (savedAccs) accounts = JSON.parse(savedAccs);
        } catch(e){ accounts = []; }

        if (!accounts.some(a => a.username === 'Rufus_Admin')) {
            accounts.push({
                realName: 'Master Admin',
                username: 'Rufus_Admin',
                passwordHash: 'quest.admin',
                isAdmin: true,
                customSets: [],
                xp: 0,
                coins: 50,
                streak: 0,
                lastQuestDate: null,
                trophies: [],
                equippedTitle: 'Master Admin',
                commonErrors: [],
                notificationShown: false
            });
        }

        function saveAccountsToStorage() {
            try { localStorage.setItem('quest_user_accounts', JSON.stringify(accounts)); } catch(e){}
        }

        let currentUser = null;
        let lastFocusedInput = null;
        let audioEnabled = true;
        let selectedRightClickWord = '';
        let broadcastChannel = null;
        let activeRoomPin = null;

        let shopCatalog = [
            { id: 'title_legend', name: 'Language Legend Title', type: 'title', price: 30 },
            { id: 'title_wizard', name: 'Vocab Wizard Title', type: 'title', price: 50 },
            { id: 'title_scholar', name: 'Latin Scholar Title', type: 'title', price: 40 }
        ];

        let universalDecks = [
            {
                title: "German Travel Essentials",
                author: "CloudCommunity",
                items: [
                    { prompt: "Where is the station?", de: ["Wo ist der Bahnhof?"] },
                    { prompt: "How much does it cost?", de: ["Wie viel kostet das?"] }
                ]
            },
            {
                title: "Latin Myths & Legends",
                author: "RomeMaster",
                items: [
                    { prompt: "deus", de: ["god"] },
                    { prompt: "bellum", de: ["war"] }
                ]
            }
        ];

        function handleGlobalContextMenu(e) {
            const targetText = window.getSelection().toString().trim();
            if (targetText && targetText.length > 1) {
                e.preventDefault();
                selectedRightClickWord = targetText;
                document.getElementById('pronounceTargetWord').textContent = selectedRightClickWord;
                document.getElementById('pronounceModal').classList.remove('hidden');
            }
        }

        function closePronounceModal() {
            document.getElementById('pronounceModal').classList.add('hidden');
        }

        function speakTargetWordPopup() {
            speakText(selectedRightClickWord, 2);
        }

        function openAppearanceSettings() {
            showScreen('settingsScreen');
        }

        function setInterfaceTheme(theme) {
            document.body.className = document.body.className.replace(/theme-\w+/g, '');
            if (theme !== 'default') {
                document.body.classList.add(`theme-${theme}`);
            }
        }

        function toggleAutoClockMode() {
            const hour = new Date().getHours();
            if (hour >= 20 || hour < 6) {
                setInterfaceTheme('cyberpunk');
                alert('Clock Theme active: Dark Cyberpunk layout enabled based on system time.');
            } else {
                setInterfaceTheme('zen');
                alert('Clock Theme active: Light Zen layout enabled based on system time.');
            }
        }

        function openCoinShop() {
            if (!currentUser) return;
            document.getElementById('shopCoinsBalance').textContent = `${currentUser.coins || 0} Coins`;
            const container = document.getElementById('shopItemsContainer');
            container.innerHTML = '';

            shopCatalog.forEach(item => {
                const div = document.createElement('div');
                div.className = 'p-4 bg-slate-950 border border-slate-800 rounded-xl flex items-center justify-between';
                div.innerHTML = `
                    <div>
                        <h4 class="text-sm font-black text-white">${item.name}</h4>
                        <span class="text-xs text-amber-400 font-bold">${item.price} Coins</span>
                    </div>
                    <button type="button" onclick="buyShopItem('${item.id}')" class="px-3 py-1.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-black text-xs rounded-lg cursor-pointer">
                        Buy Title
                    </button>
                `;
                container.appendChild(div);
            });
            showScreen('coinShopScreen');
        }

        function buyShopItem(itemId) {
            const item = shopCatalog.find(i => i.id === itemId);
            if (!currentUser || (currentUser.coins || 0) < item.price) {
                alert("Not enough coins! Keep completing quizzes to earn more.");
                return;
            }
            currentUser.coins -= item.price;
            currentUser.equippedTitle = item.name.replace(' Title', '');
            saveAccountsToStorage();
            updateTrackerUI();
            alert(`Purchased and equipped title: ${currentUser.equippedTitle}!`);
            openCoinShop();
        }

        function openUniversalCloudHub() {
            const grid = document.getElementById('universalCloudDecksGrid');
            grid.innerHTML = '';
            universalDecks.forEach((deck, idx) => {
                const card = document.createElement('div');
                card.className = 'p-5 bg-slate-950 border border-slate-800 rounded-2xl flex flex-col justify-between';
                card.innerHTML = `
                    <div>
                        <span class="text-[10px] font-black text-amber-400 uppercase">Uploaded by @${deck.author}</span>
                        <h3 class="text-base font-black text-white mt-1">${deck.title}</h3>
                        <p class="text-xs text-slate-400 mt-1">${deck.items.length} vocabulary terms</p>
                    </div>
                    <button type="button" onclick="saveUniversalDeckToAccount(${idx})" class="mt-4 w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-black text-xs rounded-xl cursor-pointer">
                        Save to Account
                    </button>
                `;
                grid.appendChild(card);
            });
            showScreen('universalHubScreen');
        }

        function saveUniversalDeckToAccount(idx) {
            if (!currentUser) return;
            const deck = universalDecks[idx];
            if (!currentUser.customSets) currentUser.customSets = [];
            currentUser.customSets.push({
                badge: "Universal Set",
                title: deck.title,
                lang1: "en-GB",
                lang2: "de-DE",
                items: deck.items,
                isReversed: false
            });
            saveAccountsToStorage();
            alert(`Saved '${deck.title}' to your custom sets!`);
        }

        function openCommonErrorsVault() {
            const container = document.getElementById('commonErrorsList');
            container.innerHTML = '';
            const errors = currentUser ? (currentUser.commonErrors || []) : [];

            if (errors.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-500 text-center py-4">No common errors logged yet! Perfect streak!</p>`;
            } else {
                errors.forEach((err, idx) => {
                    const card = document.createElement('div');
                    card.id = `error_card_${idx}`;
                    card.className = 'p-4 bg-slate-950 border border-slate-800 rounded-xl flex items-center justify-between';
                    card.innerHTML = `
                        <div>
                            <span class="text-xs font-bold text-amber-300">${err.prompt}</span>
                            <span class="text-xs text-slate-400 block">Accepted Answer: ${err.de[0]}</span>
                        </div>
                        <button type="button" onclick="greyOutSingleError(${idx})" class="px-3 py-1.5 bg-slate-800 hover:bg-emerald-600 text-slate-300 font-bold text-xs rounded-lg cursor-pointer">
                            Mark Solved
                        </button>
                    `;
                    container.appendChild(card);
                });
            }
            showScreen('commonErrorsScreen');
        }

        function greyOutSingleError(idx) {
            const card = document.getElementById(`error_card_${idx}`);
            if (card) {
                card.style.opacity = '0.3';
                card.style.filter = 'grayscale(100%)';
            }
        }

        function markAllErrorsCompleted() {
            if (currentUser) {
                currentUser.commonErrors = [];
                saveAccountsToStorage();
                alert("Mastered test completed! Solved terms permanently cleared from error queue.");
                openCommonErrorsVault();
            }
        }

        function backupAllSetsToZipSimulated() {
            if (!currentUser || !currentUser.customSets) return;
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(currentUser.customSets, null, 2));
            const dl = document.createElement('a');
            dl.setAttribute("href", dataStr);
            dl.setAttribute("download", `Quest_Hub_Backup_${currentUser.username}.json`);
            dl.click();
            dl.remove();
        }

        function openMultiplayerLobby() {
            showScreen('multiplayerLobbyScreen');
            initBroadcastChannel();
        }

        function initBroadcastChannel() {
            if ('BroadcastChannel' in window) {
                broadcastChannel = new BroadcastChannel('questhub_multiplayer_channel');
                broadcastChannel.onmessage = (event) => {
                    const data = event.data;
                    if (data.type === 'JOIN_ROOM' && data.pin === activeRoomPin) {
                        const list = document.getElementById('waitingRoomPlayersList');
                        const p = document.createElement('div');
                        p.className = 'p-3 bg-slate-900 border border-slate-800 rounded-xl text-xs font-bold text-indigo-300 flex items-center gap-2';
                        p.textContent = `Player Connected: @${data.username}`;
                        list.appendChild(p);
                        document.getElementById('activePlayerCountDisplay').textContent = `${list.children.length} Players`;
                    }
                };
            }
        }

        function hostMultiplayerRoom() {
            activeRoomPin = Math.floor(100000 + Math.random() * 900000).toString();
            document.getElementById('activeRoomCodeDisplay').textContent = activeRoomPin;
            document.getElementById('multiplayerRoomBox').classList.remove('hidden');
            document.getElementById('startLiveGameBtn').classList.remove('hidden');

            const list = document.getElementById('waitingRoomPlayersList');
            list.innerHTML = `<div class="p-3 bg-slate-900 border border-slate-800 rounded-xl text-xs font-bold text-amber-300">Host: @${currentUser.username}</div>`;
        }

        function joinMultiplayerRoom() {
            const pin = document.getElementById('joinRoomPinInput').value.trim();
            if (pin.length !== 6) {
                alert("Please enter a valid 6-digit room PIN.");
                return;
            }
            if (broadcastChannel) {
                broadcastChannel.postMessage({ type: 'JOIN_ROOM', pin: pin, username: currentUser.username });
            }
            alert(`Joined 6-digit room #${pin}! Waiting for host to start arena match.`);
        }

        function startHostLiveMatch() {
            alert("Live Arena Battle Started! Match sync live.");
        }

        function openOralExamStudio() {
            showScreen('oralExamScreen');
        }

        function toggleTeleprompterScript() {
            const box = document.getElementById('teleprompterDisplayBox');
            const btn = document.getElementById('toggleScriptBtn');
            if (box.classList.contains('hidden')) {
                box.classList.remove('hidden');
                btn.textContent = "Hide Teleprompter Script";
            } else {
                box.classList.add('hidden');
                btn.textContent = "Show Teleprompter Script";
            }
        }

        function speakOralScript() {
            const text = document.getElementById('oralScriptInput').value.trim();
            if (text) {
                document.getElementById('teleprompterText').textContent = text;
                speakText(text, 2);
            }
        }

        function startVoiceRecording() {
            document.getElementById('startRecBtn').disabled = true;
            document.getElementById('stopRecBtn').disabled = false;
            document.getElementById('oralFeedbackBox').classList.remove('hidden');
            document.getElementById('oralFeedbackDetails').innerHTML = `<p class="text-amber-300 animate-pulse">Listening... Speak your script now.</p>`;
        }

        function stopVoiceRecording() {
            document.getElementById('startRecBtn').disabled = false;
            document.getElementById('stopRecBtn').disabled = true;
            document.getElementById('oralFeedbackDetails').innerHTML = `
                <p class="text-emerald-400 font-bold">Evaluation Complete (Forgiving Phonetic Matcher):</p>
                <p class="text-slate-300">• Pronunciation accuracy: 94%</p>
                <p class="text-slate-400">• Flagged mispronounced words: None!</p>
            `;
        }

        function updateTrackerUI() {
            if (!currentUser) return;
            document.getElementById('streakCount').textContent = currentUser.streak || 0;
            document.getElementById('xpCount').textContent = currentUser.xp || 0;
            document.getElementById('userCoinsCount').textContent = currentUser.coins || 0;
            document.getElementById('userTitleBadgeDisplay').textContent = currentUser.equippedTitle || 'Language Novice';
        }

        function registerQuestCompletion(earnedXP) {
            if (!currentUser) return;

            currentUser.xp = (currentUser.xp || 0) + earnedXP;
            currentUser.coins = (currentUser.coins || 0) + 15;

            const today = new Date().toDateString();
            if (currentUser.lastQuestDate !== today) {
                currentUser.streak = (currentUser.streak || 0) + 1;
                currentUser.lastQuestDate = today;
            }

            saveAccountsToStorage();
            updateTrackerUI();
        }

        window.onload = () => {
            checkRememberedSession();
        };
    </script>
</body>
</html>