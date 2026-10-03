<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Netflix Clone</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        netred: '#E50914',
                        netblack: '#141414',
                    },
                    fontFamily: {
                        sans: ['Helvetica Neue', 'Helvetica', 'Arial', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #141414;
            color: white;
            overflow-x: hidden;
        }

        /* Hide scrollbar for Chrome, Safari and Opera */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        /* Hide scrollbar for IE, Edge and Firefox */
        .no-scrollbar {
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }

        /* Smooth scaling for movie cards */
        .movie-card {
            transition: transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            transform-origin: center center;
            will-change: transform;
        }
        
        .movie-card:hover {
            transform: scale(1.08);
            z-index: 50;
        }

        /* Hero gradient overlay */
        .hero-vignette {
            background: linear-gradient(to top, rgba(20, 20, 20, 1) 0%, rgba(20, 20, 20, 0) 40%, rgba(20, 20, 20, 0.8) 100%);
        }
        
        .hero-vignette-bottom {
            background: linear-gradient(to top, rgba(20, 20, 20, 1) 0%, rgba(20, 20, 20, 0) 100%);
        }

        /* Prevent hover clipping by adding padding */
        .row-container {
            padding-top: 2rem;
            padding-bottom: 2rem;
            margin-top: -2rem;
            margin-bottom: -2rem;
        }
    </style>
</head>
<body class="antialiased selection:bg-netred selection:text-white">

    <nav id="navbar" class="fixed w-full z-50 transition-colors duration-500 bg-gradient-to-b from-black/80 to-transparent flex items-center justify-between px-4 md:px-12 py-4 text-sm">
        <div class="flex items-center gap-8">
            <!-- Netflix Logo SVG -->
            <svg class="w-24 h-auto text-netred fill-current" viewBox="0 0 111 30" xmlns="http://www.w3.org/2000/svg">
                <path d="M105.062 14.28L111 30c-1.75-.25-3.499-.563-5.28-.845l-3.345-8.686-3.437 7.969c-1.687-.282-3.344-.376-5.031-.595l6.031-13.75L94.469 0h5.063l3.062 7.874L105.875 0h5.124l-5.937 14.28zM90.47 0h-4.594v27.25c1.5.094 3.062.156 4.594.343V0zm-8.563 26.937c-4.187-.281-8.375-.53-12.656-.625V0h4.687v21.875c2.688.062 5.375.28 7.969.405v4.657zM64.25 10.657v4.687h-6.406V26H53.22V0h13.125v4.687h-8.5v5.97h6.406zm-18.906-5.97V26.25c-1.563 0-3.156 0-4.688.062V4.687h-4.844V0h14.406v4.687h-4.874zM30.75 15.593c-2.062 0-4.5 0-6.25.095v6.968c2.75-.188 5.5-.406 8.281-.5v4.5l-12.968.967V0h12.75v4.687H24.5v5.376c2.313-.094 4.375-.094 6.25-.094v5.624zM4.78 12.968v16.22C3.188 29.375 1.594 29.5 0 29.688V0h4.469l6.093 17.032V0h4.688v28.062c-1.656.125-3.344.25-4.969.437L4.78 12.968z" />
            </svg>
            <!-- Desktop Nav Links -->
            <ul class="hidden md:flex gap-5 text-gray-300 font-medium">
                <li class="text-white cursor-pointer font-bold">Home</li>
                <li class="cursor-pointer hover:text-gray-400 transition">TV Shows</li>
                <li class="cursor-pointer hover:text-gray-400 transition">Movies</li>
                <li class="cursor-pointer hover:text-gray-400 transition">New & Popular</li>
                <li class="cursor-pointer hover:text-gray-400 transition">My List</li>
            </ul>
        </div>
        <!-- Right side icons -->
        <div class="flex items-center gap-5">
            <svg class="w-6 h-6 cursor-pointer" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            <span class="hidden md:block cursor-pointer">Kids</span>
            <svg class="w-6 h-6 cursor-pointer" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"></path></svg>
            <div class="w-8 h-8 rounded bg-blue-600 flex items-center justify-center cursor-pointer">
                <img src="https://upload.wikimedia.org/wikipedia/commons/0/0b/Netflix-avatar.png" alt="Profile" class="w-full h-full rounded">
            </div>
        </div>
    </nav>

    <header class="relative h-[85vh] w-full bg-cover bg-center flex items-center" style="background-image: url('https://images.unsplash.com/photo-1536440136628-849c177e76a1?auto=format&fit=crop&w=1925&q=80');">
        <!-- Vignette overlays -->
        <div class="absolute inset-0 hero-vignette"></div>
        <div class="absolute inset-0 top-auto h-32 hero-vignette-bottom"></div>

        <!-- Hero Content -->
        <div class="relative z-10 pl-4 md:pl-12 w-full max-w-2xl mt-24">
            <h1 class="text-5xl md:text-7xl font-bold mb-4 drop-shadow-lg leading-tight">INTERSTELLAR<br><span class="text-2xl md:text-4xl text-gray-300 font-light tracking-wide">ODYSSEY</span></h1>
            
            <div class="flex items-center gap-4 text-sm font-semibold mb-4 drop-shadow-md">
                <span class="text-green-500">98% Match</span>
                <span>2024</span>
                <span class="border border-gray-400 px-1 rounded text-xs">PG-13</span>
                <span>2h 45m</span>
                <span class="border border-gray-400 px-1 rounded text-xs">HD</span>
            </div>

            <p class="text-base md:text-lg text-gray-200 mb-8 drop-shadow-md line-clamp-3">
                When humanity's survival hangs in the balance, a team of explorers undertakes the most important mission in human history; traveling beyond this galaxy to discover whether mankind has a future among the stars.
            </p>

            <div class="flex gap-4">
                <button class="bg-white text-black px-6 md:px-8 py-2 md:py-3 rounded font-bold flex items-center gap-2 hover:bg-white/80 transition shadow-lg">
                    <svg class="w-6 h-6" fill="black" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
                    Play
                </button>
                <button class="bg-gray-500/70 text-white px-6 md:px-8 py-2 md:py-3 rounded font-bold flex items-center gap-2 hover:bg-gray-500/50 transition shadow-lg backdrop-blur-sm">
                    <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                    More Info
                </button>
            </div>
        </div>
    </header>

    <main class="relative z-20 pb-20 -mt-24 md:-mt-32">
        
        <section class="mb-8 px-4 md:px-12">
            <h2 class="text-xl md:text-2xl font-bold mb-2 text-gray-200">Trending Now</h2>
            <div class="row-container overflow-x-scroll no-scrollbar flex gap-4 snap-x snap-mandatory cursor-pointer">
                <!-- Generate cards -->
                <script>
                    const trendingImages = [
                        'https://images.unsplash.com/photo-1626814026160-2237a95fc5a0?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1534447677768-be436bb09401?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1596727147705-61a532a659bd?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1574375927938-d5a98e8ffe85?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1533613220915-609f661a6fe1?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1608889175123-8ee362201f81?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1485846234645-a62644f84728?auto=format&fit=crop&w=400&q=80'
                    ];
                    trendingImages.forEach(img => {
                        document.write(`
                            <div class="snap-start flex-none w-32 md:w-48 lg:w-56 h-[12rem] md:h-[18rem] lg:h-[21rem] relative movie-card rounded-md overflow-hidden group">
                                <img src="${img}" alt="Movie Poster" class="w-full h-full object-cover">
                                <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end p-3">
                                    <div class="flex gap-2 text-xs">
                                        <button class="bg-white text-black p-1 rounded-full"><svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg></button>
                                        <button class="border border-white text-white p-1 rounded-full"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path></svg></button>
                                    </div>
                                </div>
                            </div>
                        `);
                    });
                </script>
            </div>
        </section>

        <section class="mb-8 px-4 md:px-12">
            <h2 class="text-xl md:text-2xl font-bold mb-2 text-gray-200">Top Picks for You</h2>
            <div class="row-container overflow-x-scroll no-scrollbar flex gap-4 snap-x snap-mandatory">
                <script>
                    const topPicksImages = [
                        'https://images.unsplash.com/photo-1605810230434-7631ac76ec81?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1542204165-65bf26472b9b?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1552693892-0fbc49196b02?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1560109947-543149eceb16?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1614729939124-03290b270737?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1478720568477-152d9b164e26?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1505686994434-e3cc5abf1330?auto=format&fit=crop&w=400&q=80'
                    ];
                    topPicksImages.forEach(img => {
                        document.write(`
                            <div class="snap-start flex-none w-32 md:w-48 lg:w-56 h-[12rem] md:h-[18rem] lg:h-[21rem] relative movie-card rounded-md overflow-hidden group">
                                <img src="${img}" alt="Movie Poster" class="w-full h-full object-cover">
                                <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end p-3">
                                </div>
                            </div>
                        `);
                    });
                </script>
            </div>
        </section>

        <section class="mb-8 px-4 md:px-12">
            <h2 class="text-xl md:text-2xl font-bold mb-2 text-gray-200">Action Movies</h2>
            <div class="row-container overflow-x-scroll no-scrollbar flex gap-4 snap-x snap-mandatory">
                <script>
                    const actionImages = [
                        'https://images.unsplash.com/photo-1509281373149-e957c6296406?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1440404653325-ab127d49abc1?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1533105079780-92b9be482077?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1627856013091-fed6e4e053c5?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1470229722913-7c0e2dbbafd3?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1616530940355-351fabd9524b?auto=format&fit=crop&w=400&q=80',
                        'https://images.unsplash.com/photo-1585647347384-2593bc35786b?auto=format&fit=crop&w=400&q=80'
                    ];
                    actionImages.forEach(img => {
                        document.write(`
                            <div class="snap-start flex-none w-32 md:w-48 lg:w-56 h-[12rem] md:h-[18rem] lg:h-[21rem] relative movie-card rounded-md overflow-hidden group">
                                <img src="${img}" alt="Movie Poster" class="w-full h-full object-cover">
                            </div>
                        `);
                    });
                </script>
            </div>
        </section>
        
        <footer class="mt-16 px-4 md:px-12 py-8 text-gray-500 text-sm max-w-4xl mx-auto">
            <div class="flex gap-4 mb-4">
                <svg class="w-6 h-6 hover:text-white cursor-pointer" fill="currentColor" viewBox="0 0 24 24"><path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"/></svg>
                <svg class="w-6 h-6 hover:text-white cursor-pointer" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/></svg>
                <svg class="w-6 h-6 hover:text-white cursor-pointer" fill="currentColor" viewBox="0 0 24 24"><path d="M19.615 3.184c-3.604-.246-11.631-.245-15.23 0-3.897.266-4.356 2.62-4.385 8.816.029 6.185.484 8.549 4.385 8.816 3.6.245 11.626.246 15.23 0 3.897-.266 4.356-2.62 4.385-8.816-.029-6.185-.484-8.549-4.385-8.816zm-10.615 12.816v-8l8 3.993-8 4.007z"/></svg>
            </div>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-4">
                <a href="#" class="hover:underline">Audio Description</a>
                <a href="#" class="hover:underline">Help Center</a>
                <a href="#" class="hover:underline">Gift Cards</a>
                <a href="#" class="hover:underline">Media Center</a>
                <a href="#" class="hover:underline">Investor Relations</a>
                <a href="#" class="hover:underline">Jobs</a>
                <a href="#" class="hover:underline">Terms of Use</a>
                <a href="#" class="hover:underline">Privacy</a>
            </div>
            <button class="border border-gray-500 px-4 py-2 hover:text-white mb-4">Service Code</button>
            <p>&copy; 1997-2026 Netflix Clone, Inc.</p>
        </footer>

    </main>

    <script>
        // Change navbar background on scroll
        const navbar = document.getElementById('navbar');
        
        window.addEventListener('scroll', () => {
            if (window.scrollY > 50) {
                // Scrolled down: Solid black
                navbar.classList.remove('bg-gradient-to-b', 'from-black/80', 'to-transparent');
                navbar.classList.add('bg-netblack');
            } else {
                // Top of page: Gradient transparent
                navbar.classList.add('bg-gradient-to-b', 'from-black/80', 'to-transparent');
                navbar.classList.remove('bg-netblack');
            }
        });

        // Add mouse wheel horizontal scrolling to row containers
        const rowContainers = document.querySelectorAll('.row-container');
        rowContainers.forEach(container => {
            container.addEventListener('wheel', (e) => {
                // Prevent vertical scroll if mostly scrolling horizontally
                if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
                    e.preventDefault();
                    container.scrollLeft += e.deltaX;
                }
            });
        });
    </script>
</body>
</html>
