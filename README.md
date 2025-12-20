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
        /* ALL YOUR ORIGINAL CSS - WITH ADDITIONS FOR NEW FEATURES */
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

        /* UPDATED: User Profile in Header */
        .user-profile-header {
            position: absolute;
            top: 20px;
            right: 15px;
            z-index: 100;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .user-avatar-header {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: rgba(255, 87, 34, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            cursor: pointer;
            overflow: hidden;
        }

        .user-avatar-header img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .user-name-header {
            display: none; /* Hide name in header to prevent overlap */
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

        /* Header with logo - UPDATED FOR BETTER SPACING */
        .header {
            padding: 20px 15px 10px;
            text-align: center;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            position: relative;
            z-index: 10;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 70px;
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            font-size: 24px; /* Slightly smaller for better fit */
            font-weight: 900;
            flex: 1;
            text-align: center;
        }

        .logo-icon {
            color: #ff5722;
            font-size: 28px; /* Slightly smaller */
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
            padding: 20px 15px 100px; /* Increased bottom padding for larger dock */
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

        /* iOS-like Dock - UPDATED WITH 4TH ITEM */
        .dock {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255, 255, 255, 0.2);
            padding: 12px 15px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            z-index: 100;
            border-radius: 20px 20px 0 0;
            height: 80px; /* Slightly taller for 4 items */
        }

        .dock-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 5px;
            color: rgba(255, 255, 255, 0.7);
            text-decoration: none;
            padding: 8px 10px;
            border-radius: 12px;
            transition: all 0.2s ease;
            -webkit-touch-callout: none;
            flex: 1;
            min-width: 0; /* Allows items to shrink properly */
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
            font-size: 10px; /* Slightly smaller for 4 items */
            font-weight: 600;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 100%;
        }

        /* NEW: Account Tab */
        .account-content {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 25px;
            margin-top: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .account-header {
            text-align: center;
            margin-bottom: 25px;
        }

        .account-avatar-large {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            margin: 0 auto 15px;
            background: rgba(255, 87, 34, 0.2);
            border: 3px solid rgba(255, 255, 255, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            overflow: hidden;
            cursor: pointer;
            position: relative;
        }

        .account-avatar-large img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .account-avatar-large .change-photo {
            position: absolute;
            bottom: 0;
            width: 100%;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            font-size: 12px;
            padding: 5px;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .account-avatar-large:hover .change-photo {
            opacity: 1;
        }

        .account-name {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .account-email {
            color: rgba(255, 255, 255, 0.7);
            font-size: 14px;
            margin-bottom: 20px;
        }

        .account-section {
            margin-bottom: 25px;
        }

        .section-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            color: #ff5722;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .account-actions {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .account-btn {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.08);
            border-radius: 12px;
            text-decoration: none;
            color: white;
            transition: all 0.3s ease;
            border: 1px solid transparent;
        }

        .account-btn:hover {
            background: rgba(255, 255, 255, 0.15);
            border-color: rgba(255, 87, 34, 0.3);
            transform: translateX(5px);
        }

        .account-btn i {
            font-size: 20px;
            color: #ff5722;
            width: 24px;
        }

        .btn-text {
            flex: 1;
        }

        .btn-title {
            font-weight: 600;
            margin-bottom: 3px;
        }

        .btn-desc {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.7);
        }

        .btn-arrow {
            color: rgba(255, 255, 255, 0.5);
        }

        /* NEW: Modal for editing profile */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1002;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .modal-overlay.show {
            opacity: 1;
            pointer-events: all;
        }

        .modal {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(30px);
            border-radius: 20px;
            padding: 25px;
            width: 90%;
            max-width: 400px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
            transform: translateY(20px);
            transition: transform 0.3s ease;
        }

        .modal-overlay.show .modal {
            transform: translateY(0);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 20px;
            font-weight: 700;
        }

        .modal-close {
            background: none;
            border: none;
            color: rgba(255, 255, 255, 0.7);
            font-size: 24px;
            cursor: pointer;
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: all 0.3s ease;
        }

        .modal-close:hover {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .modal-body {
            margin-bottom: 25px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            font-size: 14px;
        }

        .form-input {
            width: 100%;
            padding: 14px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 12px;
            color: white;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .form-input:focus {
            outline: none;
            border-color: #ff5722;
            background: rgba(255, 255, 255, 0.12);
        }

        .modal-footer {
            display: flex;
            gap: 15px;
        }

        .modal-btn {
            flex: 1;
            padding: 14px;
            border-radius: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
            font-size: 16px;
        }

        .modal-btn-primary {
            background: #ff5722;
            color: white;
        }

        .modal-btn-primary:hover {
            background: #ff7043;
            transform: translateY(-2px);
        }

        .modal-btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .modal-btn-secondary:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        /* NEW: Purchases Section */
        .purchases-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .purchase-item {
            background: rgba(255, 255, 255, 0.08);
            border-radius: 12px;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            border: 1px solid transparent;
            transition: all 0.3s ease;
        }

        .purchase-item:hover {
            background: rgba(255, 255, 255, 0.12);
            border-color: rgba(255, 87, 34, 0.3);
        }

        .purchase-icon {
            width: 40px;
            height: 40px;
            border-radius: 10px;
            background: rgba(255, 87, 34, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            color: #ff5722;
            flex-shrink: 0;
        }

        .purchase-info {
            flex: 1;
        }

        .purchase-title {
            font-weight: 600;
            margin-bottom: 3px;
        }

        .purchase-date {
            font-size: 12px;
            color: rgba(255, 255, 255, 0.7);
            margin-bottom: 2px;
        }

        .purchase-price {
            font-size: 14px;
            color: #4CAF50;
            font-weight: 600;
        }

        /* NEW: Empty State */
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: rgba(255, 255, 255, 0.5);
        }

        .empty-icon {
            font-size: 50px;
            margin-bottom: 15px;
            opacity: 0.5;
        }

        .empty-text {
            font-size: 16px;
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
                padding: 8px 5px;
            }
            
            .dock-label {
                font-size: 9px;
            }
            
            .login-container {
                padding: 30px 20px;
            }
            
            .user-profile-header {
                top: 15px;
                right: 10px;
            }
            
            .logo {
                font-size: 22px;
            }
            
            .logo-icon {
                font-size: 26px;
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
            
            .dock-label {
                font-size: 11px;
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
    <!-- UPDATED: User Profile in Header (Small avatar only) -->
    <div class="user-profile-header" id="userProfileHeader" style="display: none;">
        <div class="user-avatar-header" id="userAvatarHeader">
            <i class="fas fa-user"></i>
        </div>
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

    <!-- NEW: Modal for editing profile -->
    <div class="modal-overlay" id="editProfileModal">
        <div class="modal">
            <div class="modal-header">
                <div class="modal-title">Edit Profile</div>
                <button class="modal-close" id="closeEditProfile">&times;</button>
            </div>
            <div class="modal-body">
                <div class="form-group">
                    <label class="form-label">Display Name</label>
                    <input type="text" class="form-input" id="editDisplayName" placeholder="Enter your name">
                </div>
                <div class="form-group">
                    <label class="form-label">Profile Photo URL</label>
                    <input type="text" class="form-input" id="editPhotoURL" placeholder="https://example.com/photo.jpg">
                    <small style="color: rgba(255,255,255,0.5); font-size: 12px; display: block; margin-top: 5px;">
                        Enter a URL to your profile picture (optional)
                    </small>
                </div>
            </div>
            <div class="modal-footer">
                <button class="modal-btn modal-btn-secondary" id="cancelEditProfile">Cancel</button>
                <button class="modal-btn modal-btn-primary" id="saveProfile">Save Changes</button>
            </div>
        </div>
    </div>

    <!-- Background with blur effect -->
    <div class="background"></div>
    
    <!-- Main container -->
    <div class="container" id="mainContainer" style="display: none;">
        <!-- Header with logo - UPDATED: No overlap -->
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
            
            <!-- NEW: Account Tab -->
            <div class="tab-pane" id="account">
                <div class="account-content">
                    <div class="account-header">
                        <div class="account-avatar-large" id="accountAvatarLarge">
                            <i class="fas fa-user" id="accountAvatarIcon"></i>
                            <div class="change-photo">Change Photo</div>
                        </div>
                        <div class="account-name" id="accountName">User Name</div>
                        <div class="account-email" id="accountEmail">user@example.com</div>
                    </div>
                    
                    <div class="account-section">
                        <div class="section-title">
                            <i class="fas fa-user-cog"></i>
                            <span>Account Settings</span>
                        </div>
                        <div class="account-actions">
                            <a href="#" class="account-btn" id="editProfileBtn">
                                <i class="fas fa-edit"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Edit Profile</div>
                                    <div class="btn-desc">Change your name and profile picture</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                            
                            <a href="#" class="account-btn" id="changePasswordBtn">
                                <i class="fas fa-key"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Change Password</div>
                                    <div class="btn-desc">Update your account password</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="account-section">
                        <div class="section-title">
                            <i class="fas fa-shopping-bag"></i>
                            <span>My Purchases</span>
                        </div>
                        <div class="purchases-list" id="purchasesList">
                            <!-- Purchases will be loaded here -->
                        </div>
                        <div class="empty-state" id="emptyPurchases" style="display: none;">
                            <div class="empty-icon">
                                <i class="fas fa-shopping-bag"></i>
                            </div>
                            <div class="empty-text">No purchases yet</div>
                        </div>
                    </div>
                    
                    <div class="account-section">
                        <div class="section-title">
                            <i class="fas fa-cog"></i>
                            <span>Preferences</span>
                        </div>
                        <div class="account-actions">
                            <a href="#" class="account-btn" id="notificationsBtn">
                                <i class="fas fa-bell"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Notifications</div>
                                    <div class="btn-desc">Manage your notification preferences</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                            
                            <a href="#" class="account-btn" id="privacyBtn">
                                <i class="fas fa-shield-alt"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Privacy & Security</div>
                                    <div class="btn-desc">Control your privacy settings</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="account-section">
                        <div class="section-title">
                            <i class="fas fa-info-circle"></i>
                            <span>Support</span>
                        </div>
                        <div class="account-actions">
                            <a href="#" class="account-btn" id="helpBtn">
                                <i class="fas fa-question-circle"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Help & Support</div>
                                    <div class="btn-desc">Get help with your account</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                            
                            <a href="#" class="account-btn" id="logoutAccountBtn">
                                <i class="fas fa-sign-out-alt"></i>
                                <div class="btn-text">
                                    <div class="btn-title">Logout</div>
                                    <div class="btn-desc">Sign out from your account</div>
                                </div>
                                <i class="fas fa-chevron-right btn-arrow"></i>
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- UPDATED: iOS-like Dock with 4 items (including Account) -->
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
            <a href="#" class="dock-item" data-tab="account">
                <i class="fas fa-user dock-icon"></i>
                <span class="dock-label">Account</span>
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
        const userProfileHeader = document.getElementById('userProfileHeader');
        const userAvatarHeader = document.getElementById('userAvatarHeader');
        const successMessage = document.getElementById('successMessage');
        const googleLogin = document.getElementById('googleLogin');
        const microsoftLogin = document.getElementById('microsoftLogin');
        const appleLogin = document.getElementById('appleLogin');
        const continueBtn = document.getElementById('continueBtn');
        const loginError = document.getElementById('loginError');
        const errorText = document.getElementById('errorText');
        const mainContainer = document.getElementById('mainContainer');
        
        // NEW: Account elements
        const accountAvatarLarge = document.getElementById('accountAvatarLarge');
        const accountAvatarIcon = document.getElementById('accountAvatarIcon');
        const accountName = document.getElementById('accountName');
        const accountEmail = document.getElementById('accountEmail');
        const editProfileBtn = document.getElementById('editProfileBtn');
        const logoutAccountBtn = document.getElementById('logoutAccountBtn');
        const purchasesList = document.getElementById('purchasesList');
        const emptyPurchases = document.getElementById('emptyPurchases');
        
        // NEW: Modal elements
        const editProfileModal = document.getElementById('editProfileModal');
        const closeEditProfile = document.getElementById('closeEditProfile');
        const cancelEditProfile = document.getElementById('cancelEditProfile');
        const saveProfile = document.getElementById('saveProfile');
        const editDisplayName = document.getElementById('editDisplayName');
        const editPhotoURL = document.getElementById('editPhotoURL');
        
        // ==================== USER DATA MANAGEMENT ====================
        // Get user data from localStorage
        function getUserData() {
            const userData = localStorage.getItem('danaUserData');
            return userData ? JSON.parse(userData) : {};
        }
        
        // Save user data to localStorage
        function saveUserData(data) {
            localStorage.setItem('danaUserData', JSON.stringify(data));
        }
        
        // Get user purchases
        function getUserPurchases() {
            const purchases = localStorage.getItem('danaUserPurchases');
            return purchases ? JSON.parse(purchases) : [];
        }
        
        // Save user purchases
        function saveUserPurchases(purchases) {
            localStorage.setItem('danaUserPurchases', JSON.stringify(purchases));
        }
        
        // Add a sample purchase (for testing)
        function addSamplePurchase() {
            const purchases = getUserPurchases();
            if (purchases.length === 0) {
                const samplePurchases = [
                    {
                        id: 1,
                        title: "Dhana yellow pappa logo",
                        date: "2024-03-15",
                        price: "$9.99"
                    },
                    {
                        id: 2,
                        title: "farad Dialog Data Card",
                        date: "2024-03-10",
                        price: "$4.99"
                    }
                ];
                saveUserPurchases(samplePurchases);
                return samplePurchases;
            }
            return purchases;
        }
        
        // Update purchases display
        function updatePurchasesDisplay() {
            const purchases = getUserPurchases();
            
            if (purchases.length === 0) {
                purchasesList.style.display = 'none';
                emptyPurchases.style.display = 'block';
            } else {
                purchasesList.style.display = 'flex';
                emptyPurchases.style.display = 'none';
                
                // Clear current list
                purchasesList.innerHTML = '';
                
                // Add purchases to list
                purchases.forEach(purchase => {
                    const purchaseItem = document.createElement('div');
                    purchaseItem.className = 'purchase-item';
                    purchaseItem.innerHTML = `
                        <div class="purchase-icon">
                            <i class="fas fa-shopping-bag"></i>
                        </div>
                        <div class="purchase-info">
                            <div class="purchase-title">${purchase.title}</div>
                            <div class="purchase-date">Purchased on ${purchase.date}</div>
                            <div class="purchase-price">${purchase.price}</div>
                        </div>
                    `;
                    purchasesList.appendChild(purchaseItem);
                });
            }
        }
        
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
        
        // Update all user UI elements
        function updateUserUI(user) {
            const userData = getUserData();
            const displayName = user.displayName || userData.displayName || user.email || 'User';
            const photoURL = user.photoURL || userData.photoURL || '';
            const email = user.email || '';
            
            // Update header avatar
            if (photoURL) {
                userAvatarHeader.innerHTML = `<img src="${photoURL}" alt="${displayName}">`;
            } else {
                const initial = displayName.charAt(0).toUpperCase();
                userAvatarHeader.innerHTML = initial;
                userAvatarHeader.style.background = getRandomColor();
            }
            
            // Update account tab
            accountName.textContent = displayName;
            accountEmail.textContent = email;
            
            if (photoURL) {
                accountAvatarLarge.innerHTML = `<img src="${photoURL}" alt="${displayName}"><div class="change-photo">Change Photo</div>`;
                accountAvatarIcon.style.display = 'none';
            } else {
                const initial = displayName.charAt(0).toUpperCase();
                accountAvatarLarge.innerHTML = `<div style="font-size: 40px;">${initial}</div><div class="change-photo">Change Photo</div>`;
                accountAvatarIcon.style.display = 'none';
                accountAvatarLarge.style.background = getRandomColor();
            }
            
            // Update purchases
            updatePurchasesDisplay();
        }
        
        // Update UI based on auth state
        function updateUI(user) {
            if (user) {
                // User is signed in
                loginOverlay.classList.add('hidden');
                userProfileHeader.style.display = 'flex';
                mainContainer.style.display = 'flex';
                
                // Initialize sample purchases if needed
                addSamplePurchase();
                
                // Update user info
                updateUserUI(user);
                
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
                userProfileHeader.style.display = 'none';
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
                
                // Save user data
                const user = result.user;
                const userData = {
                    displayName: user.displayName,
                    photoURL: user.photoURL,
                    email: user.email
                };
                saveUserData(userData);
                
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
        
        // ==================== PROFILE EDITING ====================
        // Open edit profile modal
        editProfileBtn.addEventListener('click', function(e) {
            e.preventDefault();
            const user = auth.currentUser;
            const userData = getUserData();
            
            if (user) {
                editDisplayName.value = user.displayName || userData.displayName || '';
                editPhotoURL.value = user.photoURL || userData.photoURL || '';
                editProfileModal.classList.add('show');
            }
        });
        
        // Close modal functions
        function closeModal() {
            editProfileModal.classList.remove('show');
        }
        
        closeEditProfile.addEventListener('click', closeModal);
        cancelEditProfile.addEventListener('click', closeModal);
        
        // Save profile changes
        saveProfile.addEventListener('click', async function() {
            const newName = editDisplayName.value.trim();
            const newPhotoURL = editPhotoURL.value.trim();
            
            if (!newName) {
                showError('Please enter a display name');
                return;
            }
            
            try {
                const user = auth.currentUser;
                const userData = getUserData();
                
                // Update user data in localStorage
                userData.displayName = newName;
                if (newPhotoURL) {
                    userData.photoURL = newPhotoURL;
                }
                saveUserData(userData);
                
                // Update Firebase user profile (if user is logged in with email/password)
                // Note: For Google sign-in, you can't update profile directly
                // But we can store the data locally
                
                // Update UI
                updateUserUI({
                    ...user,
                    displayName: newName,
                    photoURL: newPhotoURL || user.photoURL,
                    email: user.email
                });
                
                // Close modal
                closeModal();
                
                // Show success message
                showError('Profile updated successfully!');
                
            } catch (error) {
                console.error('Error updating profile:', error);
                showError('Failed to update profile: ' + error.message);
            }
        });
        
        // ==================== SIGN OUT ====================
        function handleLogout() {
            try {
                auth.signOut();
                localStorage.removeItem('danaHasSeenWelcome');
                showError("You have been signed out successfully.");
            } catch (error) {
                console.error('Sign out error:', error);
                showError('Failed to sign out. Please try again.');
            }
        }
        
        // Logout from header
        logoutAccountBtn.addEventListener('click', function(e) {
            e.preventDefault();
            handleLogout();
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
                    else if (currentTab === 'account') nextTab = 'about';
                } else {
                    // Swipe left - go to next tab
                    showGestureHint("→ Swiped left");
                    if (currentTab === 'home') nextTab = 'projects';
                    else if (currentTab === 'projects') nextTab = 'about';
                    else if (currentTab === 'about') nextTab = 'account';
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
            
            // Initialize other account buttons (placeholder functionality)
            document.getElementById('changePasswordBtn').addEventListener('click', function(e) {
                e.preventDefault();
                showError('Password change functionality will be available soon!');
            });
            
            document.getElementById('notificationsBtn').addEventListener('click', function(e) {
                e.preventDefault();
                showError('Notification settings will be available soon!');
            });
            
            document.getElementById('privacyBtn').addEventListener('click', function(e) {
                e.preventDefault();
                showError('Privacy settings will be available soon!');
            });
            
            document.getElementById('helpBtn').addEventListener('click', function(e) {
                e.preventDefault();
                showError('Help center will be available soon!');
            });
            
            // Account avatar click to edit profile
            accountAvatarLarge.addEventListener('click', function() {
                editProfileBtn.click();
            });
            
            // Header avatar click to go to account tab
            userAvatarHeader.addEventListener('click', function() {
                switchTab('account');
            });
        });
    </script>
</body>
</html>
