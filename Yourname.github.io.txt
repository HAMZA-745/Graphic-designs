<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Designer Portfolio | Full Control</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        :root {
            --primary: #6366f1;
            --secondary: #ec4899;
            --bg: #0a0a0a;
            --surface: #1a1a1a;
            --text: #ffffff;
            --muted: #a1a1aa;
            --accent: #22d3ee;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            overflow-x: hidden;
        }
        
        h1, h2, h3, .brand {
            font-family: 'Space Grotesk', sans-serif;
        }
        
        .gradient-text {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .glass {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .control-panel {
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
        }
        
        .control-panel.open {
            transform: translateX(0);
        }
        
        .project-card {
            transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
        }
        
        .project-card:hover {
            transform: translateY(-8px) scale(1.02);
        }
        
        .cursor-glow {
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, var(--primary) 0%, transparent 70%);
            opacity: 0.15;
            position: fixed;
            pointer-events: none;
            z-index: 0;
            transform: translate(-50%, -50%);
            transition: opacity 0.3s;
        }
        
        .reveal {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
        }
        
        .reveal.active {
            opacity: 1;
            transform: translateY(0);
        }
        
        input[type="color"] {
            -webkit-appearance: none;
            border: none;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            cursor: pointer;
            overflow: hidden;
        }
        
        input[type="color"]::-webkit-color-swatch-wrapper {
            padding: 0;
        }
        
        input[type="color"]::-webkit-color-swatch {
            border: none;
            border-radius: 50%;
            border: 2px solid rgba(255,255,255,0.2);
        }
        
        .range-slider {
            -webkit-appearance: none;
            width: 100%;
            height: 4px;
            border-radius: 2px;
            background: rgba(255,255,255,0.1);
            outline: none;
        }
        
        .range-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: var(--primary);
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .range-slider::-webkit-slider-thumb:hover {
            transform: scale(1.2);
        }
        
        .switch {
            position: relative;
            display: inline-block;
            width: 48px;
            height: 24px;
        }
        
        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(255,255,255,0.1);
            transition: .4s;
            border-radius: 24px;
        }
        
        .slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            left: 3px;
            bottom: 3px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .slider {
            background-color: var(--primary);
        }
        
        input:checked + .slider:before {
            transform: translateX(24px);
        }
        
        .grid-masonry {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 2rem;
        }
        
        @media (max-width: 768px) {
            .grid-masonry {
                grid-template-columns: 1fr;
            }
        }
        
        .noise {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 9999;
            opacity: 0.03;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
        }
    </style>
</head>
<body class="antialiased">
    <div class="noise"></div>
    <div class="cursor-glow" id="cursorGlow"></div>
    
    <!-- Control Panel Toggle -->
    <button onclick="toggleControlPanel()" class="fixed top-6 right-6 z-50 glass p-3 rounded-full hover:scale-110 transition-transform duration-300 group">
        <i data-lucide="settings" class="w-6 h-6 text-white group-hover:rotate-90 transition-transform duration-500"></i>
    </button>

    <!-- Control Panel -->
    <div id="controlPanel" class="control-panel fixed top-0 right-0 h-full w-96 glass z-40 overflow-y-auto border-l border-white/10">
        <div class="p-8 space-y-8">
            <div class="flex justify-between items-center border-b border-white/10 pb-4">
                <h3 class="text-xl font-bold">Control Center</h3>
                <button onclick="toggleControlPanel()" class="hover:text-red-400 transition-colors">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>

            <!-- Brand Control -->
            <div class="space-y-4">
                <h4 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Brand Identity</h4>
                <div>
                    <label class="block text-sm mb-2">Designer Name</label>
                    <input type="text" id="designerName" value="Alex Chen" class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-2 text-white focus:outline-none focus:border-indigo-500 transition-colors" oninput="updateContent()">
                </div>
                <div>
                    <label class="block text-sm mb-2">Tagline</label>
                    <input type="text" id="tagline" value="Visual Storyteller & Digital Artist" class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-2 text-white focus:outline-none focus:border-indigo-500 transition-colors" oninput="updateContent()">
                </div>
            </div>

            <!-- Color Control -->
            <div class="space-y-4">
                <h4 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Color System</h4>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm mb-2">Primary</label>
                        <div class="flex items-center gap-3">
                            <input type="color" id="primaryColor" value="#6366f1" onchange="updateColors()">
                            <span class="text-xs font-mono" id="primaryHex">#6366f1</span>
                        </div>
                    </div>
                    <div>
                        <label class="block text-sm mb-2">Secondary</label>
                        <div class="flex items-center gap-3">
                            <input type="color" id="secondaryColor" value="#ec4899" onchange="updateColors()">
                            <span class="text-xs font-mono" id="secondaryHex">#ec4899</span>
                        </div>
                    </div>
                    <div>
                        <label class="block text-sm mb-2">Accent</label>
                        <div class="flex items-center gap-3">
                            <input type="color" id="accentColor" value="#22d3ee" onchange="updateColors()">
                            <span class="text-xs font-mono" id="accentHex">#22d3ee</span>
                        </div>
                    </div>
                    <div>
                        <label class="block text-sm mb-2">Background</label>
                        <div class="flex items-center gap-3">
                            <input type="color" id="bgColor" value="#0a0a0a" onchange="updateColors()">
                            <span class="text-xs font-mono" id="bgHex">#0a0a0a</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Layout Control -->
            <div class="space-y-4">
                <h4 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Layout Settings</h4>
                <div>
                    <label class="block text-sm mb-2">Border Radius</label>
                    <input type="range" class="range-slider" id="borderRadius" min="0" max="32" value="16" oninput="updateLayout()">
                </div>
                <div>
                    <label class="block text-sm mb-2">Animation Speed</label>
                    <input type="range" class="range-slider" id="animSpeed" min="0.1" max="2" step="0.1" value="1" oninput="updateLayout()">
                </div>
                <div class="flex items-center justify-between">
                    <span class="text-sm">Show Grid Lines</span>
                    <label class="switch">
                        <input type="checkbox" id="showGrid" onchange="toggleGrid()">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="flex items-center justify-between">
                    <span class="text-sm">Cursor Glow</span>
                    <label class="switch">
                        <input type="checkbox" id="cursorGlowToggle" checked onchange="toggleCursorGlow()">
                        <span class="slider"></span>
                    </label>
                </div>
            </div>

            <!-- Content Control -->
            <div class="space-y-4">
                <h4 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Content Sections</h4>
                <div class="space-y-2">
                    <div class="flex items-center justify-between">
                        <span class="text-sm">Hero Section</span>
                        <label class="switch">
                            <input type="checkbox" checked onchange="toggleSection('hero')">
                            <span class="slider"></span>
                        </label>
                    </div>
                    <div class="flex items-center justify-between">
                        <span class="text-sm">About Section</span>
                        <label class="switch">
                            <input type="checkbox" checked onchange="toggleSection('about')">
                            <span class="slider"></span>
                        </label>
                    </div>
                    <div class="flex items-center justify-between">
                        <span class="text-sm">Portfolio Grid</span>
                        <label class="switch">
                            <input type="checkbox" checked onchange="toggleSection('portfolio')">
                            <span class="slider"></span>
                        </label>
                    </div>
                    <div class="flex items-center justify-between">
                        <span class="text-sm">Services</span>
                        <label class="switch">
                            <input type="checkbox" checked onchange="toggleSection('services')">
                            <span class="slider"></span>
                        </label>
                    </div>
                    <div class="flex items-center justify-between">
                        <span class="text-sm">Contact</span>
                        <label class="switch">
                            <input type="checkbox" checked onchange="toggleSection('contact')">
                            <span class="slider"></span>
                        </label>
                    </div>
                </div>
            </div>

            <!-- Typography -->
            <div class="space-y-4">
                <h4 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Typography</h4>
                <div>
                    <label class="block text-sm mb-2">Heading Font</label>
                    <select id="headingFont" class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-2 text-white focus:outline-none focus:border-indigo-500" onchange="updateFonts()">
                        <option value="'Space Grotesk', sans-serif">Space Grotesk</option>
                        <option value="'Playfair Display', serif">Playfair Display</option>
                        <option value="'Archivo Black', sans-serif">Archivo Black</option>
                        <option value="'Syne', sans-serif">Syne</option>
                    </select>
                </div>
                <div>
                    <label class="block text-sm mb-2">Body Font</label>
                    <select id="bodyFont" class="w-full bg-white/5 border border-white/10 rounded-lg px-4 py-2 text-white focus:outline-none focus:border-indigo-500" onchange="updateFonts()">
                        <option value="'Inter', sans-serif">Inter</option>
                        <option value="'DM Sans', sans-serif">DM Sans</option>
                        <option value="'Work Sans', sans-serif">Work Sans</option>
                        <option value="'Manrope', sans-serif">Manrope</option>
                    </select>
                </div>
            </div>

            <div class="pt-4 border-t border-white/10">
                <button onclick="resetDefaults()" class="w-full py-3 border border-white/20 rounded-lg hover:bg-white/5 transition-colors text-sm font-medium">
                    Reset to Defaults
                </button>
                <button onclick="exportConfig()" class="w-full mt-3 py-3 bg-gradient-to-r from-indigo-500 to-purple-500 rounded-lg hover:opacity-90 transition-opacity text-sm font-medium">
                    Export Configuration
                </button>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="fixed top-0 w-full z-30 glass border-b border-white/5">
        <div class="max-w-7xl mx-auto px-6 py-4 flex justify-between items-center">
            <div class="text-2xl font-bold tracking-tighter brand" id="navBrand">AC.</div>
            <div class="hidden md:flex gap-8 text-sm font-medium text-gray-300">
                <a href="#work" class="hover:text-white transition-colors">Work</a>
                <a href="#about" class="hover:text-white transition-colors">About</a>
                <a href="#services" class="hover:text-white transition-colors">Services</a>
                <a href="#contact" class="hover:text-white transition-colors">Contact</a>
            </div>
            <button class="md:hidden">
                <i data-lucide="menu" class="w-6 h-6"></i>
            </button>
        </div>
    </nav>

    <!-- Hero Section -->
    <section id="hero" class="min-h-screen flex items-center justify-center relative pt-20">
        <div class="max-w-7xl mx-auto px-6 grid md:grid-cols-2 gap-12 items-center">
            <div class="space-y-8 reveal">
                <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full border border-white/10 bg-white/5 text-sm">
                    <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
                    Available for projects
                </div>
                <h1 class="text-6xl md:text-8xl font-bold leading-none" id="heroTitle">
                    <span class="block">Creative</span>
                    <span class="gradient-text" id="heroName">Alex Chen</span>
                </h1>
                <p class="text-xl text-gray-400 max-w-lg leading-relaxed" id="heroTagline">
                    Visual Storyteller & Digital Artist crafting immersive brand experiences and digital products that captivate and convert.
                </p>
                <div class="flex gap-4">
                    <a href="#work" class="px-8 py-4 bg-white text-black rounded-full font-semibold hover:scale-105 transition-transform flex items-center gap-2">
                        View Work <i data-lucide="arrow-right" class="w-4 h-4"></i>
                    </a>
                    <a href="#contact" class="px-8 py-4 border border-white/20 rounded-full font-semibold hover:bg-white/5 transition-colors">
                        Get in Touch
                    </a>
                </div>
                <div class="flex gap-6 pt-4 text-gray-400">
                    <a href="#" class="hover:text-white transition-colors"><i data-lucide="instagram" class="w-5 h-5"></i></a>
                    <a href="#" class="hover:text-white transition-colors"><i data-lucide="twitter" class="w-5 h-5"></i></a>
                    <a href="#" class="hover:text-white transition-colors"><i data-lucide="linkedin" class="w-5 h-5"></i></a>
                    <a href="#" class="hover:text-white transition-colors"><i data-lucide="dribbble" class="w-5 h-5"></i></a>
                </div>
            </div>
            <div class="relative reveal delay-200">
                <div class="aspect-square rounded-2xl overflow-hidden relative group">
                    <img src="https://images.unsplash.com/photo-1561070791-2526d30994b5?w=800&auto=format&fit=crop&q=80" alt="Design Work" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
                </div>
                <div class="absolute -bottom-6 -left-6 glass p-6 rounded-2xl max-w-xs hidden md:block">
                    <div class="flex items-center gap-4">
                        <div class="w-12 h-12 rounded-full bg-gradient-to-br from-indigo-500 to-purple-500 flex items-center justify-center text-xl font-bold">8+</div>
                        <div>
                            <div class="font-semibold">Years Experience</div>
                            <div class="text-sm text-gray-400">Digital Design</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-32 relative">
        <div class="max-w-7xl mx-auto px-6">
            <div class="grid md:grid-cols-2 gap-16 items-center">
                <div class="reveal">
                    <h2 class="text-4xl md:text-6xl font-bold mb-6">Design that <span class="gradient-text">speaks</span></h2>
                    <div class="space-y-6 text-gray-400 text-lg leading-relaxed">
                        <p>I believe in the power of visual storytelling. Every pixel, every color choice, and every interaction is an opportunity to create meaningful connections between brands and their audiences.</p>
                        <p>With over 8 years of experience in digital design, I specialize in creating cohesive brand identities, immersive web experiences, and compelling visual narratives that drive results.</p>
                    </div>
                    <div class="grid grid-cols-3 gap-8 mt-12">
                        <div>
                            <div class="text-3xl font-bold gradient-text mb-1">150+</div>
                            <div class="text-sm text-gray-400">Projects Completed</div>
                        </div>
                        <div>
                            <div class="text-3xl font-bold gradient-text mb-1">50+</div>
                            <div class="text-sm text-gray-400">Happy Clients</div>
                        </div>
                        <div>
                            <div class="text-3xl font-bold gradient-text mb-1">12</div>
                            <div class="text-sm text-gray-400">Awards Won</div>
                        </div>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4 reveal delay-200">
                    <div class="space-y-4 mt-8">
                        <div class="glass p-6 rounded-2xl aspect-square flex items-center justify-center">
                            <i data-lucide="palette" class="w-12 h-12 text-indigo-400"></i>
                        </div>
                        <div class="glass p-6 rounded-2xl aspect-[4/5] overflow-hidden">
                            <img src="https://images.unsplash.com/photo-1558655146-d09347e92766?w=400&auto=format&fit=crop&q=80" class="w-full h-full object-cover rounded-lg" alt="Design Process">
                        </div>
                    </div>
                    <div class="space-y-4">
                        <div class="glass p-6 rounded-2xl aspect-[4/5] overflow-hidden">
                            <img src="https://images.unsplash.com/photo-1542744094-3a31f272c490?w=400&auto=format&fit=crop&q=80" class="w-full h-full object-cover rounded-lg" alt="Creative Work">
                        </div>
                        <div class="glass p-6 rounded-2xl aspect-square flex items-center justify-center">
                            <i data-lucide="pen-tool" class="w-12 h-12 text-pink-400"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Portfolio Grid -->
    <section id="portfolio" class="py-32 relative">
        <div class="max-w-7xl mx-auto px-6">
            <div class="flex justify-between items-end mb-16 reveal">
                <div>
                    <h2 class="text-4xl md:text-6xl font-bold mb-4">Selected <span class="gradient-text">Works</span></h2>
                    <p class="text-gray-400">A curation of my finest projects</p>
                </div>
                <div class="hidden md:flex gap-2">
                    <button onclick="filterProjects('all')" class="px-4 py-2 rounded-full border border-white/20 hover:bg-white/5 transition-colors text-sm active-filter" data-filter="all">All</button>
                    <button onclick="filterProjects('branding')" class="px-4 py-2 rounded-full border border-white/20 hover:bg-white/5 transition-colors text-sm" data-filter="branding">Branding</button>
                    <button onclick="filterProjects('web')" class="px-4 py-2 rounded-full border border-white/20 hover:bg-white/5 transition-colors text-sm" data-filter="web">Web</button>
                    <button onclick="filterProjects('motion')" class="px-4 py-2 rounded-full border border-white/20 hover:bg-white/5 transition-colors text-sm" data-filter="motion">Motion</button>
                </div>
            </div>
            
            <div class="grid-masonry" id="portfolioGrid">
                <!-- Project 1 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="branding">
                    <div class="aspect-[4/5] overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1634942537034-2531766767d1?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 1">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-indigo-400 text-sm font-medium mb-2">Brand Identity</span>
                        <h3 class="text-2xl font-bold mb-2">Lumina Studio</h3>
                        <p class="text-gray-300 text-sm mb-4">Complete visual identity for a photography studio</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>

                <!-- Project 2 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="web">
                    <div class="aspect-square overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 2">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-pink-400 text-sm font-medium mb-2">Web Design</span>
                        <h3 class="text-2xl font-bold mb-2">TechFlow Dashboard</h3>
                        <p class="text-gray-300 text-sm mb-4">SaaS analytics platform interface</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>

                <!-- Project 3 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="motion">
                    <div class="aspect-[3/4] overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1550745165-9bc0b252726f?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 3">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-cyan-400 text-sm font-medium mb-2">Motion Design</span>
                        <h3 class="text-2xl font-bold mb-2">Neon Dreams</h3>
                        <p class="text-gray-300 text-sm mb-4">Animated brand campaign</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>

                <!-- Project 4 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="branding">
                    <div class="aspect-square overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1600607686527-6fb886090705?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 4">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-indigo-400 text-sm font-medium mb-2">Brand Identity</span>
                        <h3 class="text-2xl font-bold mb-2">Minimal Co</h3>
                        <p class="text-gray-300 text-sm mb-4">Luxury lifestyle brand identity</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>

                <!-- Project 5 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="web">
                    <div class="aspect-[4/5] overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1551650975-87deedd944c3?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 5">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-pink-400 text-sm font-medium mb-2">Web Design</span>
                        <h3 class="text-2xl font-bold mb-2">App Showcase</h3>
                        <p class="text-gray-300 text-sm mb-4">Mobile app landing page</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>

                <!-- Project 6 -->
                <div class="project-card group relative overflow-hidden rounded-2xl reveal" data-category="motion">
                    <div class="aspect-square overflow-hidden">
                        <img src="https://images.unsplash.com/photo-1558655146-9f40138edfeb?w=600&auto=format&fit=crop&q=80" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" alt="Project 6">
                    </div>
                    <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-all duration-300 flex flex-col justify-end p-8">
                        <span class="text-cyan-400 text-sm font-medium mb-2">Motion Design</span>
                        <h3 class="text-2xl font-bold mb-2">Fluid Transitions</h3>
                        <p class="text-gray-300 text-sm mb-4">UI animation system</p>
                        <button class="w-12 h-12 rounded-full bg-white text-black flex items-center justify-center hover:scale-110 transition-transform">
                            <i data-lucide="arrow-up-right" class="w-5 h-5"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Services -->
    <section id="services" class="py-32 relative bg-white/5">
        <div class="max-w-7xl mx-auto px-6">
            <div class="text-center mb-16 reveal">
                <h2 class="text-4xl md:text-6xl font-bold mb-4">What I <span class="gradient-text">Do</span></h2>
                <p class="text-gray-400 max-w-2xl mx-auto">Comprehensive design solutions tailored to elevate your brand and digital presence</p>
            </div>
            
            <div class="grid md:grid-cols-3 gap-8">
                <div class="glass p-8 rounded-3xl hover:bg-white/10 transition-colors group reveal">
                    <div class="w-14 h-14 rounded-2xl bg-indigo-500/20 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                        <i data-lucide="compass" class="w-7 h-7 text-indigo-400"></i>
                    </div>
                    <h3 class="text-2xl font-bold mb-4">Brand Strategy</h3>
                    <p class="text-gray-400 leading-relaxed">Developing cohesive brand identities that resonate with your target audience and stand the test of time.</p>
                    <ul class="mt-6 space-y-2 text-sm text-gray-500">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-indigo-400"></i> Logo Design</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-indigo-400"></i> Visual Systems</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-indigo-400"></i> Brand Guidelines</li>
                    </ul>
                </div>

                <div class="glass p-8 rounded-3xl hover:bg-white/10 transition-colors group reveal delay-100">
                    <div class="w-14 h-14 rounded-2xl bg-pink-500/20 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                        <i data-lucide="monitor" class="w-7 h-7 text-pink-400"></i>
                    </div>
                    <h3 class="text-2xl font-bold mb-4">Digital Design</h3>
                    <p class="text-gray-400 leading-relaxed">Creating immersive digital experiences that engage users and drive conversions across all platforms.</p>
                    <ul class="mt-6 space-y-2 text-sm text-gray-500">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-pink-400"></i> Web Design</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-pink-400"></i> UI/UX Design</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-pink-400"></i> Mobile Apps</li>
                    </ul>
                </div>

                <div class="glass p-8 rounded-3xl hover:bg-white/10 transition-colors group reveal delay-200">
                    <div class="w-14 h-14 rounded-2xl bg-cyan-500/20 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform">
                        <i data-lucide="play-circle" class="w-7 h-7 text-cyan-400"></i>
                    </div>
                    <h3 class="text-2xl font-bold mb-4">Motion & Video</h3>
                    <p class="text-gray-400 leading-relaxed">Bringing brands to life through captivating animations and video content that tells your story.</p>
                    <ul class="mt-6 space-y-2 text-sm text-gray-500">
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-cyan-400"></i> Animation</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-cyan-400"></i> Video Editing</li>
                        <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-cyan-400"></i> 3D Motion</li>
                    </ul>
                </div>
            </div>
        </div>
    </section>

    <!-- Contact -->
    <section id="contact" class="py-32 relative">
        <div class="max-w-4xl mx-auto px-6 text-center">
            <div class="reveal">
                <h2 class="text-4xl md:text-6xl font-bold mb-6">Let's Create <span class="gradient-text">Together</span></h2>
                <p class="text-gray-400 text-lg mb-12">Have a project in mind? I'd love to hear about it. Let's discuss how we can bring your vision to life.</p>
                
                <form class="max-w-md mx-auto space-y-4 text-left" onsubmit="event.preventDefault(); alert('Message sent! (Demo only)')">
                    <div>
                        <input type="text" placeholder="Your Name" class="w-full bg-white/5 border border-white/10 rounded-xl px-6 py-4 text-white placeholder-gray-500 focus:outline-none focus:border-indigo-500 transition-colors">
                    </div>
                    <div>
                        <input type="email" placeholder="Your Email" class="w-full bg-white/5 border border-white/10 rounded-xl px-6 py-4 text-white placeholder-gray-500 focus:outline-none focus:border-indigo-500 transition-colors">
                    </div>
                    <div>
                        <textarea rows="4" placeholder="Tell me about your project" class="w-full bg-white/5 border border-white/10 rounded-xl px-6 py-4 text-white placeholder-gray-500 focus:outline-none focus:border-indigo-500 transition-colors resize-none"></textarea>
                    </div>
                    <button type="submit" class="w-full py-4 bg-gradient-to-r from-indigo-500 to-purple-500 rounded-xl font-semibold hover:opacity-90 transition-opacity flex items-center justify-center gap-2">
                        Send Message <i data-lucide="send" class="w-4 h-4"></i>
                    </button>
                </form>
                
                <div class="mt-16 pt-16 border-t border-white/10 flex flex-col md:flex-row justify-between items-center gap-4 text-gray-500 text-sm">
                    <div id="footerBrand">© 2024 Alex Chen. All rights reserved.</div>
                    <div class="flex gap-6">
                        <a href="#" class="hover:text-white transition-colors">Privacy</a>
                        <a href="#" class="hover:text-white transition-colors">Terms</a>
                        <a href="#" class="hover:text-white transition-colors">Twitter</a>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <script>
        // Initialize Lucide icons
        lucide.createIcons();

        // Control Panel Toggle
        function toggleControlPanel() {
            const panel = document.getElementById('controlPanel');
            panel.classList.toggle('open');
        }

        // Update Colors
        function updateColors() {
            const primary = document.getElementById('primaryColor').value;
            const secondary = document.getElementById('secondaryColor').value;
            const accent = document.getElementById('accentColor').value;
            const bg = document.getElementById('bgColor').value;
            
            document.documentElement.style.setProperty('--primary', primary);
            document.documentElement.style.setProperty('--secondary', secondary);
            document.documentElement.style.setProperty('--accent', accent);
            document.documentElement.style.setProperty('--bg', bg);
            
            document.getElementById('primaryHex').textContent = primary;
            document.getElementById('secondaryHex').textContent = secondary;
            document.getElementById('accentHex').textContent = accent;
            document.getElementById('bgHex').textContent = bg;
            
            document.body.style.backgroundColor = bg;
        }

        // Update Content
        function updateContent() {
            const name = document.getElementById('designerName').value;
            const tagline = document.getElementById('tagline').value;
            
            document.getElementById('heroName').textContent = name;
            document.getElementById('heroTagline').textContent = tagline;
            document.getElementById('navBrand').textContent = name.split(' ').map(n => n[0]).join('.') + '.';
            document.getElementById('footerBrand').textContent = `© 2024 ${name}. All rights reserved.`;
        }

        // Update Layout
        function updateLayout() {
            const radius = document.getElementById('borderRadius').value;
            const speed = document.getElementById('animSpeed').value;
            
            document.querySelectorAll('.rounded-2xl, .rounded-3xl, .rounded-full').forEach(el => {
                if (el.classList.contains('rounded-full')) return;
                el.style.borderRadius = radius + 'px';
            });
            
            document.documentElement.style.setProperty('--anim-speed', speed + 's');
        }

        // Toggle Sections
        function toggleSection(sectionId) {
            const section = document.getElementById(sectionId);
            if (section) {
                section.style.display = section.style.display === 'none' ? 'block' : 'none';
            }
        }

        // Toggle Grid
        function toggleGrid() {
            const grid = document.getElementById('showGrid').checked;
            if (grid) {
                document.body.style.backgroundImage = 'linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px)';
                document.body.style.backgroundSize = '50px 50px';
            } else {
                document.body.style.backgroundImage = 'none';
            }
        }

        // Toggle Cursor Glow
        function toggleCursorGlow() {
            const enabled = document.getElementById('cursorGlowToggle').checked;
            const glow = document.getElementById('cursorGlow');
            glow.style.opacity = enabled ? '0.15' : '0';
        }

        // Update Fonts
        function updateFonts() {
            const heading = document.getElementById('headingFont').value;
            const body = document.getElementById('bodyFont').value;
            
            document.querySelectorAll('h1, h2, h3, .brand').forEach(el => {
                el.style.fontFamily = heading;
            });
            document.body.style.fontFamily = body;
        }

        // Filter Projects
        function filterProjects(category) {
            const projects = document.querySelectorAll('.project-card');
            const buttons = document.querySelectorAll('[data-filter]');
            
            buttons.forEach(btn => {
                btn.classList.remove('bg-white', 'text-black');
                btn.classList.add('border-white/20');
                if (btn.dataset.filter === category) {
                    btn.classList.add('bg-white', 'text-black');
                    btn.classList.remove('border-white/20');
                }
            });
            
            projects.forEach(project => {
                if (category === 'all' || project.dataset.category === category) {
                    project.style.display = 'block';
                    setTimeout(() => project.style.opacity = '1', 10);
                } else {
                    project.style.opacity = '0';
                    setTimeout(() => project.style.display = 'none', 300);
                }
            });
        }

        // Reset Defaults
        function resetDefaults() {
            document.getElementById('primaryColor').value = '#6366f1';
            document.getElementById('secondaryColor').value = '#ec4899';
            document.getElementById('accentColor').value = '#22d3ee';
            document.getElementById('bgColor').value = '#0a0a0a';
            document.getElementById('designerName').value = 'Alex Chen';
            document.getElementById('tagline').value = 'Visual Storyteller & Digital Artist';
            document.getElementById('borderRadius').value = '16';
            document.getElementById('animSpeed').value = '1';
            
            updateColors();
            updateContent();
            updateLayout();
            
            // Show all sections
            ['hero', 'about', 'portfolio', 'services', 'contact'].forEach(id => {
                document.getElementById(id).style.display = 'block';
            });
        }

        // Export Config
        function exportConfig() {
            const config = {
                colors: {
                    primary: document.getElementById('primaryColor').value,
                    secondary: document.getElementById('secondaryColor').value,
                    accent: document.getElementById('accentColor').value,
                    background: document.getElementById('bgColor').value
                },
                content: {
                    name: document.getElementById('designerName').value,
                    tagline: document.getElementById('tagline').value
                },
                layout: {
                    borderRadius: document.getElementById('borderRadius').value,
                    animationSpeed: document.getElementById('animSpeed').value
                }
            };
            
            const dataStr = JSON.stringify(config, null, 2);
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            
            const exportFileDefaultName = 'designer-config.json';
            
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
        }

        // Cursor Glow Effect
        document.addEventListener('mousemove', (e) => {
            const glow = document.getElementById('cursorGlow');
            glow.style.left = e.clientX + 'px';
            glow.style.top = e.clientY + 'px';
        });

        // Scroll Reveal Animation
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('active');
                }
            });
        }, observerOptions);

        document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

        // Smooth Scroll
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }
            });
        });

        // Initialize
        updateColors();
    </script>
</body>
</html>
