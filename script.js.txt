// ================================
// WAIT FOR DOM TO LOAD
// ================================

document.addEventListener('DOMContentLoaded', function() {
    
    // ================================
    // NAVIGATION FUNCTIONALITY
    // ================================
    
    const hamburger = document.getElementById('hamburger');
    const navLinks = document.getElementById('navLinks');
    const navbar = document.getElementById('navbar');
    const navLinkElements = document.querySelectorAll('.nav-link');
    
    // Toggle mobile menu
    if (hamburger && navLinks) {
        hamburger.addEventListener('click', function() {
            hamburger.classList.toggle('active');
            navLinks.classList.toggle('active');
            document.body.style.overflow = navLinks.classList.contains('active') ? 'hidden' : '';
        });
    }
    
    // Close mobile menu when clicking a link
    navLinkElements.forEach(link => {
        link.addEventListener('click', function() {
            if (hamburger && navLinks) {
                hamburger.classList.remove('active');
                navLinks.classList.remove('active');
                document.body.style.overflow = '';
            }
        });
    });
    
    // Close mobile menu when clicking outside
    document.addEventListener('click', function(e) {
        if (navLinks && hamburger) {
            if (navLinks.classList.contains('active') && 
                !navLinks.contains(e.target) && 
                !hamburger.contains(e.target)) {
                hamburger.classList.remove('active');
                navLinks.classList.remove('active');
                document.body.style.overflow = '';
            }
        }
    });
    
    // Add scrolled class to navbar
    let lastScroll = 0;
    window.addEventListener('scroll', function() {
        const currentScroll = window.pageYOffset;
        
        if (currentScroll > 50) {
            navbar.classList.add('scrolled');
        } else {
            navbar.classList.remove('scrolled');
        }
        
        lastScroll = currentScroll;
    });
    
    // ================================
    // ACTIVE NAVIGATION LINK
    // ================================
    
    function updateActiveNavLink() {
        const sections = document.querySelectorAll('section[id]');
        const scrollPosition = window.scrollY + 120;
        
        sections.forEach(section => {
            const sectionTop = section.offsetTop;
            const sectionHeight = section.offsetHeight;
            const sectionId = section.getAttribute('id');
            
            if (scrollPosition >= sectionTop && scrollPosition < sectionTop + sectionHeight) {
                navLinkElements.forEach(link => {
                    link.classList.remove('active');
                    if (link.getAttribute('href') === `#${sectionId}`) {
                        link.classList.add('active');
                    }
                });
            }
        });
        
        // Special case for home section at the top
        if (window.scrollY < 100) {
            navLinkElements.forEach(link => {
                link.classList.remove('active');
                if (link.getAttribute('href') === '#home') {
                    link.classList.add('active');
                }
            });
        }
    }
    
    window.addEventListener('scroll', updateActiveNavLink);
    updateActiveNavLink();
    
    // ================================
    // SMOOTH SCROLLING
    // ================================
    
    navLinkElements.forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            const targetId = this.getAttribute('href');
            const targetSection = document.querySelector(targetId);
            
            if (targetSection) {
                const offsetTop = targetSection.offsetTop - 80;
                window.scrollTo({
                    top: offsetTop,
                    behavior: 'smooth'
                });
            }
        });
    });
    
    // ================================
    // HERO CTA BUTTONS
    // ================================
    
    const projectsCTA = document.getElementById('projectsCTA');
    
    if (projectsCTA) {
        projectsCTA.addEventListener('click', function() {
            const projectsSection = document.getElementById('projects');
            if (projectsSection) {
                const offsetTop = projectsSection.offsetTop - 80;
                window.scrollTo({
                    top: offsetTop,
                    behavior: 'smooth'
                });
            }
        });
    }
    
    // ================================
    // TYPEWRITER EFFECT FOR HERO
    // ================================
    
    const heroTagline = document.getElementById('heroTagline');
    const heroButtons = document.getElementById('heroButtons');
    const fullText = 'Building Systems That Solve Real Problems Through Clear Thinking';
    let charIndex = 0;
    
    function typeWriter() {
        if (charIndex < fullText.length) {
            heroTagline.textContent += fullText.charAt(charIndex);
            charIndex++;
            // Vary speed slightly for more natural feel
            const speed = Math.random() * 30 + 40;
            setTimeout(typeWriter, speed);
        } else {
            // Show buttons after typing is complete
            setTimeout(() => {
                if (heroButtons) {
                    heroButtons.classList.add('show');
                }
            }, 400);
        }
    }
    
    // Start typewriter effect after a delay
    setTimeout(typeWriter, 1500);
    
    // ================================
    // INTERSECTION OBSERVER FOR FADE-IN
    // ================================
    
    const observerOptions = {
        threshold: 0.15,
        rootMargin: '0px 0px -80px 0px'
    };
    
    const observer = new IntersectionObserver(function(entries) {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('fade-in-visible');
            }
        });
    }, observerOptions);
    
    // Observe all fade-in elements
    const fadeElements = document.querySelectorAll('.fade-in');
    fadeElements.forEach(element => {
        observer.observe(element);
    });
    
    // ================================
    // PARALLAX EFFECT FOR GALAXY
    // ================================
    
    let ticking = false;
    
    function updateParallax() {
        const scrolled = window.pageYOffset;
        const stars1 = document.querySelector('.stars');
        const stars2 = document.querySelector('.stars2');
        const stars3 = document.querySelector('.stars3');
        const nebula = document.querySelector('.nebula');
        
        if (stars1) stars1.style.transform = `translateY(${scrolled * 0.4}px)`;
        if (stars2) stars2.style.transform = `translateY(${scrolled * 0.25}px)`;
        if (stars3) stars3.style.transform = `translateY(${scrolled * 0.15}px)`;
        if (nebula) nebula.style.transform = `translate(-20%, ${-50 + scrolled * 0.1}%)`;
        
        ticking = false;
    }
    
    window.addEventListener('scroll', function() {
        if (!ticking) {
            window.requestAnimationFrame(updateParallax);
            ticking = true;
        }
    });
    
    // ================================
    // CARD GLOW EFFECT
    // ================================
    
    const cards = document.querySelectorAll('.about-card, .philosophy-card, .skill-card, .project-card');
    
    cards.forEach(card => {
        card.addEventListener('mousemove', function(e) {
            const rect = card.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            
            const glow = card.querySelector('.card-glow');
            if (glow) {
                glow.style.background = `radial-gradient(circle at ${x}px ${y}px, rgba(123, 183, 255, 0.12) 0%, transparent 60%)`;
            }
        });
        
        card.addEventListener('mouseleave', function() {
            const glow = card.querySelector('.card-glow');
            if (glow) {
                glow.style.background = '';
            }
        });
    });
    
    // ================================
    // SCROLL TO TOP BUTTON
    // ================================
    
    const scrollTopBtn = document.createElement('button');
    scrollTopBtn.innerHTML = '↑';
    scrollTopBtn.className = 'scroll-top-btn';
    scrollTopBtn.setAttribute('aria-label', 'Scroll to top');
    scrollTopBtn.style.cssText = `
        position: fixed;
        bottom: 40px;
        right: 40px;
        width: 56px;
        height: 56px;
        border-radius: 50%;
        background: linear-gradient(135deg, #7bb7ff 0%, #c7b8ff 100%);
        color: white;
        border: none;
        font-size: 1.8rem;
        cursor: pointer;
        opacity: 0;
        visibility: hidden;
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        z-index: 999;
        box-shadow: 0 10px 30px rgba(123, 183, 255, 0.4);
        font-weight: bold;
    `;
    
    document.body.appendChild(scrollTopBtn);
    
    window.addEventListener('scroll', function() {
        if (window.scrollY > 600) {
            scrollTopBtn.style.opacity = '1';
            scrollTopBtn.style.visibility = 'visible';
        } else {
            scrollTopBtn.style.opacity = '0';
            scrollTopBtn.style.visibility = 'hidden';
        }
    });
    
    scrollTopBtn.addEventListener('click', function() {
        window.scrollTo({
            top: 0,
            behavior: 'smooth'
        });
    });
    
    scrollTopBtn.addEventListener('mouseenter', function() {
        this.style.transform = 'translateY(-6px) scale(1.05)';
        this.style.boxShadow = '0 15px 40px rgba(123, 183, 255, 0.6)';
    });
    
    scrollTopBtn.addEventListener('mouseleave', function() {
        this.style.transform = 'translateY(0) scale(1)';
        this.style.boxShadow = '0 10px 30px rgba(123, 183, 255, 0.4)';
    });
    
    // ================================
    // PROJECT SCREENSHOT LIGHTBOX
    // ================================
    
    const projectScreenshots = document.querySelectorAll('.project-screenshot');
    
    projectScreenshots.forEach(screenshot => {
        screenshot.addEventListener('click', function() {
            const img = this.querySelector('img');
            if (img) {
                createLightbox(img.src, img.alt);
            }
        });
    });
    
    function createLightbox(imageSrc, imageAlt) {
        const lightbox = document.createElement('div');
        lightbox.className = 'lightbox';
        lightbox.style.cssText = `
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 10000;
            cursor: pointer;
            animation: fadeIn 0.3s ease;
            padding: 2rem;
        `;
        
        const img = document.createElement('img');
        img.src = imageSrc;
        img.alt = imageAlt;
        img.style.cssText = `
            max-width: 90%;
            max-height: 90%;
            border-radius: 12px;
            box-shadow: 0 20px 80px rgba(0, 0, 0, 0.8);
            animation: scaleIn 0.3s ease;
        `;
        
        const closeBtn = document.createElement('button');
        closeBtn.innerHTML = '×';
        closeBtn.style.cssText = `
            position: absolute;
            top: 2rem;
            right: 2rem;
            width: 50px;
            height: 50px;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 2.5rem;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
        `;
        
        closeBtn.addEventListener('mouseenter', function() {
            this.style.background = 'rgba(255, 255, 255, 0.2)';
            this.style.transform = 'rotate(90deg)';
        });
        
        closeBtn.addEventListener('mouseleave', function() {
            this.style.background = 'rgba(255, 255, 255, 0.1)';
            this.style.transform = 'rotate(0deg)';
        });
        
        lightbox.appendChild(img);
        lightbox.appendChild(closeBtn);
        document.body.appendChild(lightbox);
        
        // Prevent scrolling
        document.body.style.overflow = 'hidden';
        
        // Close lightbox
        function closeLightbox() {
            lightbox.style.animation = 'fadeOut 0.3s ease';
            setTimeout(() => {
                lightbox.remove();
                document.body.style.overflow = '';
            }, 300);
        }
        
        lightbox.addEventListener('click', function(e) {
            if (e.target === lightbox) {
                closeLightbox();
            }
        });
        
        closeBtn.addEventListener('click', closeLightbox);
        
        // Close on Escape key
        document.addEventListener('keydown', function escapeHandler(e) {
            if (e.key === 'Escape') {
                closeLightbox();
                document.removeEventListener('keydown', escapeHandler);
            }
        });
    }
    
    // Add lightbox animations
    const style = document.createElement('style');
    style.textContent = `
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }
        
        @keyframes scaleIn {
            from { 
                opacity: 0;
                transform: scale(0.9);
            }
            to { 
                opacity: 1;
                transform: scale(1);
            }
        }
    `;
    document.head.appendChild(style);
    
    // ================================
    // SKILL ITEMS HOVER EFFECT
    // ================================
    
    const skillItems = document.querySelectorAll('.skill-item');
    
    skillItems.forEach(item => {
        item.addEventListener('mouseenter', function() {
            this.style.transition = 'all 0.3s cubic-bezier(0.4, 0, 0.2, 1)';
        });
    });
    
    // ================================
    // SMOOTH REVEAL FOR SECTIONS
    // ================================
    
    const sections = document.querySelectorAll('section');
    
    const sectionObserver = new IntersectionObserver(function(entries) {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.style.opacity = '1';
                entry.target.style.transform = 'translateY(0)';
            }
        });
    }, {
        threshold: 0.1
    });
    
    sections.forEach(section => {
        section.style.opacity = '0';
        section.style.transform = 'translateY(30px)';
        section.style.transition = 'opacity 0.8s ease, transform 0.8s ease';
        sectionObserver.observe(section);
    });
    
    // ================================
    // PERFORMANCE OPTIMIZATION
    // ================================
    
    // Debounce scroll events
    let scrollTimeout;
    
    function debounce(func, wait) {
        return function executedFunction(...args) {
            const later = () => {
                clearTimeout(scrollTimeout);
                func(...args);
            };
            clearTimeout(scrollTimeout);
            scrollTimeout = setTimeout(later, wait);
        };
    }
    
    const optimizedScrollHandler = debounce(updateActiveNavLink, 100);
    window.addEventListener('scroll', optimizedScrollHandler);
    
    // ================================
    // PRELOAD IMAGES
    // ================================
    
    const images = document.querySelectorAll('img[src]');
    images.forEach(img => {
        const src = img.getAttribute('src');
        if (src && !src.startsWith('data:')) {
            const preloadLink = document.createElement('link');
            preloadLink.rel = 'preload';
            preloadLink.as = 'image';
            preloadLink.href = src;
            document.head.appendChild(preloadLink);
        }
    });
    
    // ================================
    // CONSOLE MESSAGE
    // ================================
    
    const consoleStyles = [
        'color: #7bb7ff',
        'font-size: 20px',
        'font-weight: bold',
        'text-shadow: 0 0 10px rgba(123, 183, 255, 0.5)'
    ].join(';');
    
    console.log('%c👋 Welcome to my portfolio!', consoleStyles);
    console.log('%cBuilt with focus, precision, and deliberate practice', 'color: #c7b8ff; font-size: 14px;');
    console.log('%c- Siddhartha Rawal', 'color: #ffd6c9; font-size: 12px; font-style: italic;');
    console.log('%cInterested in the code? Check it out on GitHub!', 'color: #8a8e9b; font-size: 12px;');
    
    // ================================
    // ACCESSIBILITY IMPROVEMENTS
    // ================================
    
    // Add focus visible states for keyboard navigation
    document.addEventListener('keydown', function(e) {
        if (e.key === 'Tab') {
            document.body.classList.add('keyboard-nav');
        }
    });
    
    document.addEventListener('mousedown', function() {
        document.body.classList.remove('keyboard-nav');
    });
    
    // Add focus styles
    const focusStyle = document.createElement('style');
    focusStyle.textContent = `
        body.keyboard-nav *:focus {
            outline: 2px solid #7bb7ff;
            outline-offset: 4px;
        }
        
        body:not(.keyboard-nav) *:focus {
            outline: none;
        }
    `;
    document.head.appendChild(focusStyle);
    
    // ================================
    // PAGE LOAD ANIMATION
    // ================================
    
    window.addEventListener('load', function() {
        document.body.style.opacity = '1';
        document.body.style.transition = 'opacity 0.5s ease';
    });
    
    // Set initial opacity
    document.body.style.opacity = '0';
    
    // ================================
    // REDUCE MOTION FOR ACCESSIBILITY
    // ================================
    
    const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)');
    
    if (prefersReducedMotion.matches) {
        // Disable animations for users who prefer reduced motion
        const animationStyle = document.createElement('style');
        animationStyle.textContent = `
            *, *::before, *::after {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }
        `;
        document.head.appendChild(animationStyle);
    }
    
    // ================================
    // INITIALIZE
    // ================================
    
    console.log('%c✅ All systems operational', 'color: #baf2e9; font-size: 12px; font-weight: bold;');
    
    // Trigger initial updates
    updateActiveNavLink();
    updateParallax();
    
});

// ================================
// UTILITY FUNCTIONS
// ================================

// Smooth scroll polyfill for older browsers
if (!('scrollBehavior' in document.documentElement.style)) {
    const smoothScroll = function(targetPosition, duration) {
        const startPosition = window.pageYOffset;
        const distance = targetPosition - startPosition;
        let startTime = null;
        
        const animation = function(currentTime) {
            if (startTime === null) startTime = currentTime;
            const timeElapsed = currentTime - startTime;
            const run = ease(timeElapsed, startPosition, distance, duration);
            window.scrollTo(0, run);
            if (timeElapsed < duration) requestAnimationFrame(animation);
        };
        
        const ease = function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * t * t + b;
            t--;
            return -c / 2 * (t * (t - 2) - 1) + b;
        };
        
        requestAnimationFrame(animation);
    };
    
    // Override scroll behavior
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function(e) {
            e.preventDefault();
            const target = document.querySelector(this.getAttribute('href'));
            if (target) {
                smoothScroll(target.offsetTop - 80, 1000);
            }
        });
    });
}