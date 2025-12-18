<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Dana on Fire | Creative Projects</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    <style>
        /* ALL YOUR ORIGINAL CSS - KEEPING EVERYTHING EXACTLY THE SAME */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: #000;
            color: #fff;
            overflow-x: hidden;
            height: 100vh;
            touch-action: pan-y;
        }

        /* Background with blur effect */
        .background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://wallpapers.com/images/hd/red-black-background-h18bb4lw1nyraagm.jpg');
            background-size: cover;
            background-position: center;
            filter: blur(8px);
            transform: scale(1.1);
            z-index: -1;
        }

        /* Login Overlay */
        .login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            backdrop-filter: blur(20px);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            padding: 20px;
            opacity: 1;
            transition: opacity 0.5s ease;
        }

        .login-overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .login-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(30px);
            border-radius: 24px;
            padding: 40px 30px;
            width: 100%;
            max-width: 400px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
            text-align: center;
            animation: fadeInUp 0.5s ease;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .login-logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            font-size: 32px;
            font-weight: 900;
            margin-bottom: 30px;
        }

        .login-icon {
            color: #ff5722;
            font-size: 40px;
            animation: flicker 2s infinite alternate;
        }

        .login-title {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .login-subtitle {
            color: rgba(255, 255, 255, 0.7);
            margin-bottom: 30px;
            font-size: 16px;
        }

        .login-options {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 25px;
        }

        .login-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            padding: 16px;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
            color: white;
        }

        .login-btn:hover {
            transform: translateY(-3px);
            opacity: 0.9;
        }

        .btn-google { background: #4285F4; }
        .btn-microsoft { background: #00BCF2; }
        .btn-apple { background: #000; }

        .login-btn i {
            font-size: 20px;
        }

        .login-error {
            color: #ff6b6b;
            background: rgba(255, 107, 107, 0.1);
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 15px;
            font-size: 14px;
            display: none;
        }

        .login-error.show {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        /* User Profile */
        .user-profile {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: rgba(255, 87, 34, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            cursor: pointer;
        }

        .user-name {
            background: rgba(0, 0, 0, 0.7);
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 600;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logout-btn {
            background: rgba(255, 87, 34, 0.2);
            color: #ff5722;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .logout-btn:hover {
            background: rgba(255, 87, 34, 0.3);
        }

        /* Success Message */
        .success-message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(30px);
            border-radius: 20px;
            padding: 30px;
            max-width: 350px;
            width: 90%;
            text-align: center;
            z-index: 1001;
            display: none;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
            animation: fadeInUp 0.5s ease;
        }

        .success-message.show {
            display: block;
        }

        .success-icon {
            font-size: 60px;
            color: #4CAF50;
            margin-bottom: 20px;
            animation: successPulse 1s ease;
        }

        @keyframes successPulse {
            0% { transform: scale(0.8); opacity: 0; }
            70% { transform: scale(1.1); }
            100% { transform: scale(1); opacity: 1; }
        }

        .success-title {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .success-text {
            color: rgba(255, 255, 255, 0.8);
            margin-bottom: 25px;
            line-height: 1.5;
        }

        .success-btn {
            display: inline-block;
            padding: 12px 30px;
            background: #4CAF50;
            color: white;
            border-radius: 50px;
            font-weight: 600;
            text-decoration: none;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .success-btn:hover {
            background: #66BB6A;
            transform: scale(1.05);
        }

        /* Main container */
        .container {
            max-width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        /* Header with logo */
        .header {
            padding: 20px 15px 10px;
            text-align: center;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            position: relative;
            z-index: 10;
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            font-size: 28px;
            font-weight: 900;
        }

        .logo-icon {
            color: #ff5722;
            font-size: 34px;
            animation: flicker 2s infinite alternate;
        }

        @keyframes flicker {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.8; }
        }

        /* Tab content area */
        .tab-content {
            flex: 1;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            padding: 20px 15px 80px;
            position: relative;
        }

        /* Hide all tabs by default */
        .tab-pane {
            display: none;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Active tab */
        .tab-pane.active {
            display: block;
        }

        /* iOS-like Dock */
        .dock {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255, 255, 255, 0.2);
            padding: 10px 15px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            z-index: 100;
            border-radius: 20px 20px 0 0;
        }

        .dock-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 5px;
            color: rgba(255, 255, 255, 0.7);
            text-decoration: none;
            padding: 8px 12px;
            border-radius: 12px;
            transition: all 0.2s ease;
            -webkit-touch-callout: none;
        }

        .dock-item.active, .dock-item:hover {
            color: #fff;
            background: rgba(255, 255, 255, 0.15);
            transform: translateY(-5px);
        }

        .dock-icon {
            font-size: 20px;
        }

        .dock-label {
            font-size: 11px;
            font-weight: 600;
        }

        /* Home Tab - Contact Cards */
        .contact-cards {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .contact-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: transform 0.3s ease;
        }

        .contact-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.15);
        }

        .contact-icon {
            font-size: 28px;
            margin-bottom: 15px;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            background: rgba(255, 87, 34, 0.2);
            color: #ff5722;
        }

        .contact-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .contact-value {
            font-size: 16px;
            opacity: 0.9;
            margin-bottom: 15px;
        }

        .contact-action {
            display: inline-block;
            padding: 8px 20px;
            background: #ff5722;
            color: white;
            border-radius: 50px;
            font-weight: 600;
            text-decoration: none;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .contact-action:hover {
            background: #ff7043;
            transform: scale(1.05);
        }

        /* Projects Tab - File Grid */
        .projects-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-top: 20px;
        }

        .project-item {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            transition: all 0.3s ease;
        }

        .project-item:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.15);
        }

        .project-image {
            width: 100%;
            height: 120px;
            border-radius: 12px;
            object-fit: cover;
            margin-bottom: 12px;
            background-color: #333;
        }

        .project-title {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .project-desc {
            font-size: 13px;
            opacity: 0.8;
            margin-bottom: 12px;
            line-height: 1.4;
        }

        .download-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            padding: 8px 15px;
            background: rgba(255, 87, 34, 0.2);
            color: #ff5722;
            border-radius: 50px;
            font-weight: 600;
            text-decoration: none;
            font-size: 14px;
            transition: all 0.3s ease;
            width: 100%;
        }

        .download-btn:hover {
            background: rgba(255, 87, 34, 0.3);
        }

        /* About Tab */
        .about-content {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 25px;
            margin-top: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .about-title {
            font-size: 24px;
            font-weight: 800;
            margin-bottom: 15px;
            color: #ff5722;
        }

        .about-text {
            font-size: 16px;
            line-height: 1.6;
            opacity: 0.9;
            margin-bottom: 20px;
        }

        .about-highlight {
            color: #ff5722;
            font-weight: 700;
        }

        /* Gesture indicator */
        .gesture-hint {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 15px 25px;
            border-radius: 15px;
            font-size: 14px;
            display: none;
            z-index: 1000;
            text-align: center;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        /* Responsive adjustments */
        @media (max-width: 480px) {
            .contact-cards {
                grid-template-columns: 1fr;
            }
            
            .projects-grid {
                grid-template-columns: 1fr;
            }
            
            .dock-item {
                padding: 8px 8px;
            }
            
            .login-container {
                padding: 30px 20px;
            }
            
            .user-profile {
                top: 10px;
                right: 10px;
            }
            
            .user-name {
                display: none;
            }
        }

        @media (min-width: 768px) {
            .contact-cards {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .projects-grid {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .container {
                max-width: 768px;
                margin: 0 auto;
            }
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 5px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(255, 87, 34, 0.5);
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <!-- User Profile (Shown when logged in) -->
    <div class="user-profile" id="userProfile" style="display: none;">
        <div class="user-avatar" id="userAvatar">
            <i class="fas fa-user"></i>
        </div>
        <div class="user-name" id="userName"></div>
        <button class="logout-btn" id="logoutBtn">Logout</button>
    </div>

    <!-- Login Overlay (Shows when not logged in) -->
    <div class="login-overlay" id="loginOverlay">
        <div class="login-container">
            <div class="login-logo">
                <i class="fas fa-fire-flame-curved login-icon"></i>
                <span>Dana on Fire</span>
            </div>
            
            <h2 class="login-title">Welcome Back</h2>
            <p class="login-subtitle">Sign in to access your creative projects</p>
            
            <div class="login-error" id="loginError">
                <i class="fas fa-exclamation-circle"></i> <span id="errorText"></span>
            </div>
            
            <div class="login-options">
                <button class="login-btn btn-google" id="googleLogin">
                    <i class="fab fa-google"></i>
                    <span>Continue with Google</span>
                </button>
                
                <button class="login-btn btn-microsoft" id="microsoftLogin" style="display: none;">
                    <i class="fab fa-microsoft"></i>
                    <span>Continue with Microsoft</span>
                </button>
                
                <button class="login-btn btn-apple" id="appleLogin" style="display: none;">
                    <i class="fab fa-apple"></i>
                    <span>Continue with Apple</span>
                </button>
            </div>
            
            <p style="color: rgba(255,255,255,0.5); font-size: 12px; margin-top: 20px;">
                By signing in, you agree to our Terms of Service and Privacy Policy
            </p>
        </div>
    </div>

    <!-- Success Message -->
    <div class="success-message" id="successMessage">
        <i class="fas fa-check-circle success-icon"></i>
        <h3 class="success-title">Successfully Signed In!</h3>
        <p class="success-text">Welcome to Dana on Fire. You now have full access to all creative projects and features.</p>
        <button class="success-btn" id="continueBtn">Continue to Dashboard</button>
    </div>

    <!-- Background with blur effect -->
    <div class="background"></div>
    
    <!-- Main container -->
    <div class="container" id="mainContainer" style="display: none;">
        <!-- Header with logo -->
        <header class="header">
            <div class="logo">
                <i class="fas fa-fire-flame-curved logo-icon"></i>
                <span>Dana on Fire</span>
            </div>
        </header>
        
        <!-- Tab content area -->
        <div class="tab-content">
            <!-- Home Tab -->
            <div class="tab-pane active" id="home">
                <!-- HOME TAB: Contact information section -->
                <div class="contact-cards">
                    <!-- Email card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fas fa-envelope"></i>
                        </div>
                        <div class="contact-title">Email</div>
                        <div class="contact-value">contact@danaonfire.com</div>
                        <a href="mailto:contact@danaonfire.com" class="contact-action">Send Email</a>
                    </div>
                    
                    <!-- Phone card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fas fa-phone"></i>
                        </div>
                        <div class="contact-title">Phone</div>
                        <div class="contact-value">0741048179</div>
                        <a href="tel:+94741048179" class="contact-action">Call Now</a>
                    </div>
                    
                    <!-- WhatsApp card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fab fa-whatsapp"></i>
                        </div>
                        <div class="contact-title">WhatsApp</div>
                        <div class="contact-value">+94741048179</div>
                        <a href="https://wa.me/15551234567" class="contact-action" target="_blank">Chat on WhatsApp</a>
                    </div>
                    
                    <!-- Address card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fas fa-map-marker-alt"></i>
                        </div>
                        <div class="contact-title">Location</div>
                        <div class="contact-value">srilanka</div>
                        <a href="https://maps.google.com/?q=San+Francisco" class="contact-action" target="_blank">View on Map</a>
                    </div>
                </div>
            </div>
            
            <!-- Projects Tab -->
            <div class="tab-pane" id="projects">
                <!-- PROJECTS TAB: 12 file slots for projects -->
                <div class="projects-grid" id="projectsGrid">
                    <!-- Project items will be dynamically added here -->
                </div>
            </div>
            
            <!-- About Tab -->
            <div class="tab-pane" id="about">
                <!-- ABOUT TAB: Description about the company -->
                <div class="about-content">
                    <h2 class="about-title">About Dana on Fire</h2>
                    <p class="about-text">
                        Welcome to <span class="about-highlight">Dana on Fire</span>, a creative studio dedicated to bringing innovative digital projects.
                    </p>
                    <p class="about-text">
                        පූරුවේ කරපු අකුසල්                                    ගෙවන්නට
පාරුවේ ඇවිත් විය දුක්                                     විඳින්නට
මාරුවේ අවල අදිමින් විටින්                                      විට
සීරුවේ බසිමු ගොස් තුන්                                  මෝදරට
                    </p>
                    <p class="about-text">
                        සූර්යයා යනු අපගේ සෞරග්‍රහ මණ්ඩලයේ මධ්‍යයේ ඇති විශාල තාරකාවකි. එය ප්‍රධාන වශයෙන් හයිඩ්‍රජන් හා හීලියම් වලින් සෑදුණු අතිශයින් රත් වූ වායු ගෝලයකි (ප්ලාස්මා). එහි ඇති ගුරුත්වාකර්ෂණ බලය හේතුවෙන් ග්‍රහලෝක, ග්‍රහක හා අනෙකුත් වස්තූන් එය වටා කක්ෂගත වී ඇත.
                    </p>
                    <p class="about-text">
                       Free Fire තරගකාරී ක්‍රීඩාවක් වුවද, එය මිතුරන් සමග සම්බන්ධ වීමට, එකට කණ්ඩායම් වශයෙන් ක්‍රියා කිරීමට හා සහයෝගයෙන් කටයුතු කිරීමට හොඳ වේදියක් සපයයි.
                    </p>
                </div>
            </div>
        </div>
        
        <!-- iOS-like Dock -->
        <nav class="dock">
            <a href="#" class="dock-item active" data-tab="home">
                <i class="fas fa-home dock-icon"></i>
                <span class="dock-label">Home</span>
            </a>
            <a href="#" class="dock-item" data-tab="projects">
                <i class="fas fa-folder dock-icon"></i>
                <span class="dock-label">Projects</span>
            </a>
            <a href="#" class="dock-item" data-tab="about">
                <i class="fas fa-info-circle dock-icon"></i>
                <span class="dock-label">About</span>
            </a>
        </nav>
        
        <!-- Gesture hint (shown on swipe) -->
        <div class="gesture-hint" id="gestureHint"></div>
    </div>

    <script>
        // ==================== FIREBASE CONFIGURATION ====================
        // Using YOUR EXACT Firebase config from the console
        const firebaseConfig = {
            apiKey: "AIzaSyAOGZTqdkmcwo-wcpDOduMYua6eyjN6f6A",
            authDomain: "dana-on-fire.firebaseapp.com",
            projectId: "dana-on-fire",
            storageBucket: "dana-on-fire.firebasestorage.app",
            messagingSenderId: "136143488788",
            appId: "1:136143488788:web:263ff21f7ca852d75e6401",
            measurementId: "G-NJDPB16ZLF"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();

        // ==================== DOM ELEMENTS ====================
        const loginOverlay = document.getElementById('loginOverlay');
        const userProfile = document.getElementById('userProfile');
        const userAvatar = document.getElementById('userAvatar');
        const userName = document.getElementById('userName');
        const logoutBtn = document.getElementById('logoutBtn');
        const successMessage = document.getElementById('successMessage');
        const googleLogin = document.getElementById('googleLogin');
        const microsoftLogin = document.getElementById('microsoftLogin');
        const appleLogin = document.getElementById('appleLogin');
        const continueBtn = document.getElementById('continueBtn');
        const loginError = document.getElementById('loginError');
        const errorText = document.getElementById('errorText');
        const mainContainer = document.getElementById('mainContainer');
        
        // ==================== AUTH FUNCTIONS ====================
        
        // Show loading state
        function showLoading(button, text = "Signing in...") {
            const originalHTML = button.innerHTML;
            button.innerHTML = `<i class="fas fa-spinner fa-spin"></i><span>${text}</span>`;
            button.disabled = true;
            return originalHTML;
        }
        
        // Reset button
        function resetButton(button, originalHTML) {
            setTimeout(() => {
                button.innerHTML = originalHTML;
                button.disabled = false;
            }, 1000);
        }
        
        // Show error message
        function showError(message) {
            errorText.textContent = message;
            loginError.classList.add('show');
            setTimeout(() => {
                loginError.classList.remove('show');
            }, 5000);
        }
        
        // Generate random color for avatar
        function getRandomColor() {
            const colors = [
                'rgba(255, 87, 34, 0.3)',
                'rgba(33, 150, 243, 0.3)',
                'rgba(76, 175, 80, 0.3)',
                'rgba(156, 39, 176, 0.3)',
                'rgba(244, 67, 54, 0.3)'
            ];
            return colors[Math.floor(Math.random() * colors.length)];
        }
        
        // Update UI based on auth state
        function updateUI(user) {
            if (user) {
                // User is signed in
                loginOverlay.classList.add('hidden');
                userProfile.style.display = 'flex';
                mainContainer.style.display = 'flex';
                
                // Update user info
                const displayName = user.displayName || user.email || 'User';
                userName.textContent = displayName;
                
                // Set avatar
                if (user.photoURL) {
                    userAvatar.innerHTML = `<img src="${user.photoURL}" alt="${displayName}" style="width:100%;height:100%;border-radius:50%;">`;
                } else {
                    const initial = displayName.charAt(0).toUpperCase();
                    userAvatar.innerHTML = initial;
                    userAvatar.style.background = getRandomColor();
                }
                
                // Show success message on first login
                const hasSeenMessage = localStorage.getItem('danaHasSeenWelcome');
                if (!hasSeenMessage) {
                    setTimeout(() => {
                        successMessage.classList.add('show');
                        localStorage.setItem('danaHasSeenWelcome', 'true');
                    }, 500);
                }
                
                console.log('User signed in:', user);
            } else {
                // User is signed out
                loginOverlay.classList.remove('hidden');
                userProfile.style.display = 'none';
                mainContainer.style.display = 'none';
            }
        }
        
        // ==================== REAL GOOGLE SIGN-IN ====================
        googleLogin.addEventListener('click', async function() {
            const originalHTML = showLoading(this, "Opening Google Sign-in...");
            
            try {
                // Create Google Auth Provider
                const provider = new firebase.auth.GoogleAuthProvider();
                
                // Add scopes for profile and email
                provider.addScope('profile');
                provider.addScope('email');
                
                // Optional: Force account selection every time
                provider.setCustomParameters({
                    prompt: 'select_account'
                });
                
                // Sign in with popup
                const result = await auth.signInWithPopup(provider);
                
                // This will trigger the auth state listener
                // which will call updateUI()
                
            } catch (error) {
                console.error('Google Sign-in Error:', error);
                
                // Handle specific errors
                if (error.code === 'auth/popup-blocked') {
                    showError('Popup blocked by browser. Please allow popups for this site and try again.');
                } else if (error.code === 'auth/popup-closed-by-user') {
                    showError('Sign-in cancelled. Please try again.');
                } else if (error.code === 'auth/unauthorized-domain') {
                    showError('This domain is not authorized. Please add "localhost" to Firebase Authorized Domains.');
                } else if (error.code === 'auth/operation-not-allowed') {
                    showError('Google sign-in is not enabled. Please enable it in Firebase Console.');
                } else {
                    showError('Sign-in failed: ' + error.message);
                }
                
                resetButton(this, originalHTML);
            }
        });
        
        // ==================== SIGN OUT ====================
        logoutBtn.addEventListener('click', async function() {
            try {
                await auth.signOut();
                localStorage.removeItem('danaHasSeenWelcome');
                showError("You have been signed out successfully.");
            } catch (error) {
                console.error('Sign out error:', error);
                showError('Failed to sign out. Please try again.');
            }
        });
        
        // ==================== CONTINUE BUTTON ====================
        continueBtn.addEventListener('click', function() {
            successMessage.classList.remove('show');
        });
        
        // ==================== AUTH STATE LISTENER ====================
        // This listens for sign-in/sign-out events
        auth.onAuthStateChanged((user) => {
            updateUI(user);
            
            // Show gesture hint if user is logged in
            if (user) {
                setTimeout(() => {
                    showGestureHint("← Swipe left/right to switch tabs →");
                }, 1000);
            }
        });
        
        // ==================== YOUR ORIGINAL SITE CODE ====================
        // Gesture detection variables
        let touchStartX = 0;
        let touchStartY = 0;
        let touchEndX = 0;
        let touchEndY = 0;
        const gestureZone = document.getElementById('mainContainer');
        const gestureHint = document.getElementById('gestureHint');
        
        // Tab switching functionality
        const dockItems = document.querySelectorAll('.dock-item');
        const tabPanes = document.querySelectorAll('.tab-pane');
        
        // Initialize projects grid with 12 items
        const projectsGrid = document.getElementById('projectsGrid');
        const projectData = [
            { title: "Dhana yellow pappa logo", desc: "❌රුපියල් 100", img: "https://raw.githubusercontent.com/jackblasterfuze-commits/new/refs/heads/main/saiko%201.jpeg", link: "https://drive.google.com/file/d/10BXV0rRV47V0VV1_U-yOIw3dUSXaTIVb/view?usp=drivesdk" },
            { title: "farad", desc: "⭕dialog data card", img: "https://raw.githubusercontent.com/jackblasterfuze-commits/new/refs/heads/main/6.jpeg", link: "https://drive.google.com/file/d/1-Cfa79dpSzu32l46Y8aEIqWpdOtt_uPS/view?usp=drivesdk" },
            { title: "tollak", desc: "*❌රුපියල් 100*", img: "https://raw.githubusercontent.com/jackblasterfuze-commits/new/refs/heads/main/1.jpeg", link: "https://drive.google.com/file/d/10J1LDCVuIwo74r7HV0-glAkAEO7UnIVh/view?usp=drivesdk" },
            { title: "kolla", desc: "⭕dialog data card", img: "https://github.com/user-attachments/assets/4b20c798-1e6a-400d-ac47-7409011a7ba2", link: "https://drive.google.com/file/d/example4" },
            { title: "201", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example5" },
            { title: "301", desc: "Content not found", img: "https://images.unsplash.com/photo-1542751110-97427bbecf20?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example6" },
            { title: "475", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example7" },
            { title: "532", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example8" },
            { title: "412", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example9" },
            { title: "865", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example10" },
            { title: "4725", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example11" },
            { title: "4785", desc: "Content not found", img: "https://upload.wikimedia.org/wikipedia/commons/d/d1/Image_not_available.png?20210219185637", link: "https://drive.google.com/file/d/example12" }
        ];
        
        // Function to switch tabs
        function switchTab(tabId) {
            // Update dock items
            dockItems.forEach(item => {
                if (item.dataset.tab === tabId) {
                    item.classList.add('active');
                } else {
                    item.classList.remove('active');
                }
            });
            
            // Update tab panes
            tabPanes.forEach(pane => {
                if (pane.id === tabId) {
                    pane.classList.add('active');
                } else {
                    pane.classList.remove('active');
                }
            });
        }
        
        // Initialize the projects grid with 12 items
        function initializeProjects() {
            projectsGrid.innerHTML = '';
            
            projectData.forEach((project, index) => {
                const projectItem = document.createElement('div');
                projectItem.className = 'project-item';
                projectItem.innerHTML = `
                    <img src="${project.img}" alt="${project.title}" class="project-image">
                    <div class="project-title">${project.title}</div>
                    <div class="project-desc">${project.desc}</div>
                    <a href="${project.link}" class="download-btn" target="_blank">
                        <i class="fas fa-download"></i>
                        Download
                    </a>
                `;
                projectsGrid.appendChild(projectItem);
            });
        }
        
        // Show gesture hint temporarily
        function showGestureHint(message) {
            gestureHint.textContent = message;
            gestureHint.style.display = 'block';
            
            setTimeout(() => {
                gestureHint.style.display = 'none';
            }, 1500);
        }
        
        // Handle touch start for gesture detection
        if (gestureZone) {
            gestureZone.addEventListener('touchstart', function(event) {
                touchStartX = event.changedTouches[0].screenX;
                touchStartY = event.changedTouches[0].screenY;
            }, false);
            
            // Handle touch end for gesture detection
            gestureZone.addEventListener('touchend', function(event) {
                touchEndX = event.changedTouches[0].screenX;
                touchEndY = event.changedTouches[0].screenY;
                handleSwipe();
            }, false);
        }
        
        // Process swipe gesture
        function handleSwipe() {
            const deltaX = touchEndX - touchStartX;
            const deltaY = touchEndY - touchStartY;
            
            // Minimum swipe distance (in pixels)
            const minSwipeDistance = 50;
            
            // Check if it's a horizontal swipe (more horizontal than vertical)
            if (Math.abs(deltaX) > Math.abs(deltaY) && Math.abs(deltaX) > minSwipeDistance) {
                // Get current active tab
                const activeDockItem = document.querySelector('.dock-item.active');
                const currentTab = activeDockItem.dataset.tab;
                let nextTab = '';
                
                // Determine next tab based on swipe direction
                if (deltaX > 0) {
                    // Swipe right - go to previous tab
                    showGestureHint("← Swiped right");
                    if (currentTab === 'projects') nextTab = 'home';
                    else if (currentTab === 'about') nextTab = 'projects';
                } else {
                    // Swipe left - go to next tab
                    showGestureHint("→ Swiped left");
                    if (currentTab === 'home') nextTab = 'projects';
                    else if (currentTab === 'projects') nextTab = 'about';
                }
                
                // Switch tab if nextTab is determined
                if (nextTab) {
                    switchTab(nextTab);
                }
            }
            
            // Reset touch coordinates
            touchStartX = 0;
            touchStartY = 0;
            touchEndX = 0;
            touchEndY = 0;
        }
        
        // Event listeners for dock items (tab switching)
        dockItems.forEach(item => {
            item.addEventListener('click', function(e) {
                e.preventDefault();
                const tabId = this.dataset.tab;
                switchTab(tabId);
            });
        });
        
        // ==================== INITIALIZE PAGE ====================
        document.addEventListener('DOMContentLoaded', function() {
            initializeProjects();
            
            // Add some interactivity to contact cards
            const contactCards = document.querySelectorAll('.contact-card');
            contactCards.forEach(card => {
                card.addEventListener('click', function() {
                    this.style.transform = 'translateY(-8px) scale(1.02)';
                    setTimeout(() => {
                        this.style.transform = 'translateY(-5px)';
                    }, 150);
                });
            });
        });
    </script>
</body>
</html>
