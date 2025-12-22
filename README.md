<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Dana on Fire | Creative Projects</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    <style>
        :root {
            --ios-bg: #000000;
            --ios-accent: #ff5722;
            --ios-card: rgba(28, 28, 30, 0.7);
            --ios-blur: blur(20px);
            --safe-area-bottom: env(safe-area-inset-bottom);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", "Helvetica Neue", Arial, sans-serif;
            background: var(--ios-bg);
            color: #fff;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
        }

        /* Background */
        .background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a0000 0%, #000 100%);
            z-index: -1;
        }

        /* Login Overlay */
        .login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            backdrop-filter: var(--ios-blur);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 2000;
            transition: opacity 0.5s ease;
        }

        .login-overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .login-container {
            width: 85%;
            max-width: 350px;
            text-align: center;
            animation: iosSpring 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        @keyframes iosSpring {
            from { transform: scale(0.8); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }

        .login-icon {
            font-size: 60px;
            color: var(--ios-accent);
            margin-bottom: 20px;
            filter: drop-shadow(0 0 15px rgba(255,87,34,0.4));
        }

        .login-title { font-size: 32px; font-weight: 800; margin-bottom: 8px; letter-spacing: -0.5px; }
        .login-subtitle { color: #8e8e93; margin-bottom: 40px; font-size: 16px; }

        .login-btn {
            width: 100%;
            padding: 16px;
            border-radius: 14px;
            border: none;
            font-size: 17px;
            font-weight: 600;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            cursor: pointer;
            transition: transform 0.1s;
        }

        .login-btn:active { transform: scale(0.96); }
        .btn-google { background: #fff; color: #000; }

        /* Main App Interface */
        .app-shell {
            display: flex;
            flex-direction: column;
            height: 100vh;
            padding-top: env(safe-area-inset-top);
        }

        .app-header {
            padding: 20px;
            background: rgba(0,0,0,0.5);
            backdrop-filter: var(--ios-blur);
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .app-header h1 {
            font-size: 34px;
            font-weight: 800;
            letter-spacing: -1px;
        }

        .user-pill {
            display: flex;
            align-items: center;
            gap: 10px;
            background: var(--ios-card);
            padding: 6px 14px;
            border-radius: 20px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .user-avatar {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: var(--ios-accent);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
        }

        /* Tabs Content */
        .scroll-container {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            padding-bottom: calc(100px + var(--safe-area-bottom));
        }

        .tab-pane { display: none; }
        .tab-pane.active { display: block; animation: fadeIn 0.4s ease; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } }

        /* iPhone Style Grid */
        .app-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
        }

        .app-card {
            background: var(--ios-card);
            border-radius: 24px;
            padding: 16px;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .app-icon-img {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #333, #111);
            border-radius: 18px;
            margin-bottom: 12px;
            box-shadow: 0 8px 15px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            color: var(--ios-accent);
        }

        .app-name { font-weight: 600; font-size: 15px; margin-bottom: 4px; }
        .app-meta { font-size: 12px; color: #8e8e93; }

        /* iOS Dock */
        .ios-dock-container {
            position: fixed;
            bottom: 20px;
            left: 20px;
            right: 20px;
            z-index: 1000;
            padding-bottom: var(--safe-area-bottom);
        }

        .ios-dock {
            background: rgba(44, 44, 46, 0.7);
            backdrop-filter: blur(30px);
            -webkit-backdrop-filter: blur(30px);
            border-radius: 30px;
            display: flex;
            justify-content: space-around;
            padding: 10px;
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .dock-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #8e8e93;
            text-decoration: none;
            width: 60px;
            transition: 0.2s;
        }

        .dock-btn.active { color: var(--ios-accent); }
        .dock-btn i { font-size: 22px; margin-bottom: 4px; }
        .dock-btn span { font-size: 10px; font-weight: 500; }

        /* About & Contact Styles */
        .ios-list {
            background: var(--ios-card);
            border-radius: 14px;
            overflow: hidden;
            margin-bottom: 20px;
        }

        .ios-list-item {
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            text-decoration: none;
            color: white;
        }

        .ios-list-item:last-child { border-bottom: none; }
        .ios-list-item i { width: 30px; height: 30px; background: #3a3a3c; border-radius: 7px; display: flex; align-items: center; justify-content: center; font-size: 14px; color: var(--ios-accent); }

        .about-box { line-height: 1.6; color: #d1d1d6; font-size: 16px; }
        .about-box h2 { color: white; margin-bottom: 15px; font-size: 24px; }
    </style>
</head>
<body>

    <div class="background"></div>

    <div class="login-overlay" id="loginOverlay">
        <div class="login-container">
            <i class="fas fa-fire-flame-curved login-icon"></i>
            <h1 class="login-title">Dana on Fire</h1>
            <p class="login-subtitle">Creative projects at your fingertips</p>
            
            <div id="loginError" style="color: #ff453a; margin-bottom: 15px; display: none; font-size: 14px;"></div>
            
            <button class="login-btn btn-google" id="googleLogin">
                <i class="fab fa-google"></i> Continue with Google
            </button>
            <p style="font-size: 12px; color: #8e8e93; margin-top: 20px;">Secure biometric login enabled</p>
        </div>
    </div>

    <div class="app-shell" id="appShell" style="display: none;">
        <header class="app-header">
            <h1 id="headerTitle">Home</h1>
            <div class="user-pill">
                <div class="user-avatar" id="userInitial">U</div>
                <span id="logoutBtn" style="font-size: 12px; font-weight: 600; color: #ff453a;">Logout</span>
            </div>
        </header>

        <div class="scroll-container">
            <div class="tab-pane active" id="home">
                <div class="ios-list">
                    <a href="mailto:contact@danaonfire.com" class="ios-list-item">
                        <i class="fas fa-envelope"></i>
                        <div>
                            <div style="font-size: 15px; font-weight: 500;">Email</div>
                            <div style="font-size: 13px; color: #8e8e93;">contact@danaonfire.com</div>
                        </div>
                    </a>
                    <a href="tel:+94741048179" class="ios-list-item">
                        <i class="fas fa-phone"></i>
                        <div>
                            <div style="font-size: 15px; font-weight: 500;">Phone</div>
                            <div style="font-size: 13px; color: #8e8e93;">0741048179</div>
                        </div>
                    </a>
                    <a href="https://wa.me/94741048179" class="ios-list-item">
                        <i class="fab fa-whatsapp"></i>
                        <div>
                            <div style="font-size: 15px; font-weight: 500;">WhatsApp</div>
                            <div style="font-size: 13px; color: #8e8e93;">Chat with us</div>
                        </div>
                    </a>
                    <a href="#" class="ios-list-item">
                        <i class="fas fa-location-arrow"></i>
                        <div>
                            <div style="font-size: 15px; font-weight: 500;">Location</div>
                            <div style="font-size: 13px; color: #8e8e93;">Sri Lanka</div>
                        </div>
                    </a>
                </div>
            </div>

            <div class="tab-pane" id="projects">
                <div class="app-grid" id="projectsGrid">
                    </div>
            </div>

            <div class="tab-pane" id="about">
                <div class="about-box">
                    <h2>Creative Studio</h2>
                    <p>Welcome to <b>Dana on Fire</b>. We are a creative studio dedicated to bringing innovative digital projects to life with a focus on speed and impact.</p>
                    <br>
                    <div class="ios-list" style="padding: 15px; font-style: italic; font-size: 14px;">
                        පූරුවේ කරපු අකුසල් ගෙවන්නට...<br>
                        පාරුවේ ඇවිත් විය දුක් විඳින්න්නට...<br>
                        මාරුවේ අවල අදිමින් විටින් විට...<br>
                        සීරුවේ බසිමු ගොස් තුන් මෝදරට...
                    </div>
                </div>
            </div>
        </div>

        <div class="ios-dock-container">
            <nav class="ios-dock">
                <a href="#" class="dock-btn active" data-tab="home" data-title="Home">
                    <i class="fas fa-house"></i>
                    <span>Home</span>
                </a>
                <a href="#" class="dock-btn" data-tab="projects" data-title="Projects">
                    <i class="fas fa-th-large"></i>
                    <span>Projects</span>
                </a>
                <a href="#" class="dock-btn" data-tab="about" data-title="About">
                    <i class="fas fa-info-circle"></i>
                    <span>About</span>
                </a>
            </nav>
        </div>
    </div>

    <script>
        // Firebase Config (Keep your original config)
        const firebaseConfig = {
            apiKey: "AIzaSyAOGZTqdkmcwo-wcpDOduMYua6eyjN6f6A",
            authDomain: "dana-on-fire.firebaseapp.com",
            projectId: "dana-on-fire",
            storageBucket: "dana-on-fire.firebasestorage.app",
            messagingSenderId: "136143488788",
            appId: "1:136143488788:web:263ff21f7ca852d75e6401"
        };

        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();

        // Auth Logic
        const googleLogin = document.getElementById('googleLogin');
        googleLogin.onclick = () => {
            const provider = new firebase.auth.GoogleAuthProvider();
            auth.signInWithPopup(provider).catch(err => {
                document.getElementById('loginError').innerText = err.message;
                document.getElementById('loginError').style.display = 'block';
            });
        };

        auth.onAuthStateChanged(user => {
            if (user) {
                document.getElementById('loginOverlay').classList.add('hidden');
                document.getElementById('appShell').style.display = 'flex';
                document.getElementById('userInitial').innerText = user.displayName ? user.displayName[0] : 'U';
                loadProjects();
            } else {
                document.getElementById('loginOverlay').classList.remove('hidden');
                document.getElementById('appShell').style.display = 'none';
            }
        });

        document.getElementById('logoutBtn').onclick = () => auth.signOut();

        // Tab Navigation
        const dockBtns = document.querySelectorAll('.dock-btn');
        dockBtns.forEach(btn => {
            btn.onclick = (e) => {
                e.preventDefault();
                const tabId = btn.getAttribute('data-tab');
                const title = btn.getAttribute('data-title');
                
                document.querySelectorAll('.tab-pane').forEach(p => p.classList.remove('active'));
                document.getElementById(tabId).classList.add('active');
                
                dockBtns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                
                document.getElementById('headerTitle').innerText = title;
            };
        });

        // Project Generator
        function loadProjects() {
            const grid = document.getElementById('projectsGrid');
            grid.innerHTML = '';
            for (let i = 1; i <= 12; i++) {
                grid.innerHTML += `
                    <div class="app-card">
                        <div class="app-icon-img">
                            <i class="fas fa-fire"></i>
                        </div>
                        <div class="app-name">Project ${i}</div>
                        <div class="app-meta">v1.0.4</div>
                        <div style="margin-top:10px; background:var(--ios-accent); color:white; font-size:12px; padding:4px 12px; border-radius:10px; font-weight:bold;">GET</div>
                    </div>
                `;
            }
        }
    </script>
</body>
</html>
