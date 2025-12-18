<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Dana on Fire | Creative Projects</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
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
    <!-- Background with blur effect -->
    <div class="background"></div>
    
    <!-- Main container -->
    <div class="container" id="mainContainer">
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
                        <div class="contact-value">+1 (555) 123-4567</div>
                        <a href="tel:+15551234567" class="contact-action">Call Now</a>
                    </div>
                    
                    <!-- WhatsApp card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fab fa-whatsapp"></i>
                        </div>
                        <div class="contact-title">WhatsApp</div>
                        <div class="contact-value">+1 (555) 123-4567</div>
                        <a href="https://wa.me/15551234567" class="contact-action" target="_blank">Chat on WhatsApp</a>
                    </div>
                    
                    <!-- Address card -->
                    <div class="contact-card">
                        <div class="contact-icon">
                            <i class="fas fa-map-marker-alt"></i>
                        </div>
                        <div class="contact-title">Location</div>
                        <div class="contact-value">San Francisco, CA</div>
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
                        Our team of passionate developers, designers, and creators work together to transform ideas into reality. With a focus on user experience and modern aesthetics, we craft solutions that are not only functional but also visually stunning.
                    </p>
                    <p class="about-text">
                        Founded in 2020, we've successfully delivered over 50 projects to clients worldwide, ranging from startups to established enterprises. Our commitment to quality, innovation, and client satisfaction drives everything we do.
                    </p>
                    <p class="about-text">
                        Explore our projects to see examples of our work, or get in touch to discuss how we can help bring your vision to life!
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
            { title: "E-commerce Website", desc: "Full-stack e-commerce platform with payment integration", img: "https://raw.githubusercontent.com/jackblasterfuze-commits/new/refs/heads/main/6.jpeg", link: "https://drive.google.com/file/d/1-Cfa79dpSzu32l46Y8aEIqWpdOtt_uPS/view?usp=drivesdk" },
            { title: "Brand Identity", desc: "Complete branding package for a tech startup", img: "https://images.unsplash.com/photo-1545235617-9465d2a55698?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example3" },
            { title: "Dashboard Design", desc: "Analytics dashboard with real-time data visualization", img: "https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example4" },
            { title: "Game Concept", desc: "Concept art and design for a mobile adventure game", img: "https://images.unsplash.com/photo-1550745165-9bc0b252726f?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example5" },
            { title: "AR Experience", desc: "Augmented reality app for interior design visualization", img: "https://images.unsplash.com/photo-1542751110-97427bbecf20?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example6" },
            { title: "Social Media Kit", desc: "Template pack for consistent social media branding", img: "https://images.unsplash.com/photo-1611224923853-80b023f02d71?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example7" },
            { title: "Mobile Game UI", desc: "User interface design for a puzzle mobile game", img: "https://images.unsplash.com/photo-1511512578047-dfb367046420?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example8" },
            { title: "Web Application", desc: "Responsive web app for project management", img: "https://images.unsplash.com/photo-1460925895917-afdab827c52f?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example9" },
            { title: "Logo Collection", desc: "Set of 50 modern logo designs for various industries", img: "https://images.unsplash.com/photo-1545235617-9465d2a55698?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example10" },
            { title: "Animation Pack", desc: "Collection of Lottie animations for mobile apps", img: "https://images.unsplash.com/photo-1522869635100-9f4c5e86aa37?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example11" },
            { title: "Icon Set", desc: "500+ pixel-perfect icons in multiple styles", img: "https://images.unsplash.com/photo-1561070791-2526d30994b5?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80", link: "https://drive.google.com/file/d/example12" }
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
        
        // Initialize the page
        document.addEventListener('DOMContentLoaded', function() {
            initializeProjects();
            
            // Show initial gesture hint on first load
            setTimeout(() => {
                showGestureHint("← Swipe left/right to switch tabs →");
            }, 1000);
            
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
