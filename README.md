# RIFQY_portofolio
portofolio
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Rifqy Ridho Pratama - 3D Interactive Portfolio</title>
    <meta name="description" content="Portfolio Rifqy Ridho Pratama — Creative Developer & Designer. 3D interactive portfolio built with Three.js and modern UI/UX.">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&family=Montserrat:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* TYPOGRAPHY & SPACING VARIABLES */
        :root{
            --primary-dark: #0a0a14;
            --accent-primary: #a855f7;
            --accent-light: #d946ef;
            --text-primary: #e0e0e0;
            --text-secondary: #b0b0b0;
            --glow-color: rgba(168,85,247,0.5);

            --section-gap: 4rem;
            --heading-gap: 1.25rem;
            --content-gap: 1rem;
            --card-padding: 1.25rem;
            --max-width: 1400px;
        }

        *{box-sizing:border-box;margin:0;padding:0}
        html{scroll-behavior:smooth}
        body{
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, var(--primary-dark) 0%, #1a0033 50%, var(--primary-dark) 100%);
            color:var(--text-primary);
            -webkit-font-smoothing:antialiased;
            -moz-osx-font-smoothing:grayscale;
            overflow-x:hidden;
        }

        /* accessibility helpers */
        .sr-only{position:absolute!important;width:1px!important;height:1px!important;padding:0!important;margin:-1px!important;overflow:hidden!important;clip:rect(0,0,0,0)!important;white-space:nowrap!important;border:0!important;}
        .sr-only-focusable:active,.sr-only-focusable:focus{position:static!important;width:auto!important;height:auto!important;margin:0!important;overflow:visible!important;clip:auto!important;white-space:normal!important}

        /* GENERAL LAYOUT */
        .container{max-width:var(--max-width);margin:0 auto;padding:0 2rem}
        .section{padding:calc(var(--section-gap) + 2rem) 0;position:relative}
        .section-container{max-width:var(--max-width);margin:0 auto;padding:0 2rem}
        .section-title{font-size:clamp(34px,5vw,54px);font-weight:800;margin-bottom:var(--heading-gap);display:block;background:linear-gradient(135deg,#fff 0%,var(--accent-primary) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
        .heading-divider{width:120px;height:4px;border-radius:2px;background:linear-gradient(90deg,var(--accent-primary),var(--accent-light));box-shadow:0 0 20px var(--glow-color);margin:0.75rem 0 1.5rem}

        /* section intro (short paragraph under heading) */
        .section-intro{color:var(--text-secondary);max-width:900px;margin-bottom:1.5rem;font-size:16px;line-height:1.8}

        /* text blocks and columns */
        .text-block{margin-bottom:var(--content-gap);line-height:1.85;color:var(--text-secondary)}
        .text-columns{column-gap:2rem;column-rule:0px}
        @media(min-width:920px){
            .text-columns.two{column-count:2}
        }

        /* NAVBAR */
        .navbar{position:fixed;top:0;width:100%;background:rgba(10,10,20,0.8);backdrop-filter:blur(10px);border-bottom:1px solid rgba(168,85,247,0.12);z-index:1000;padding:1rem 0}
        .nav-container{max-width:var(--max-width);margin:0 auto;padding:0 2rem;display:flex;justify-content:space-between;align-items:center}
        .nav-logo{font-size:22px;font-weight:800;background:linear-gradient(135deg,#fff 0%,var(--accent-primary) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;display:flex;align-items:center;gap:8px;letter-spacing:1px}
        .nav-menu{display:flex;gap:2.5rem;list-style:none}
        .nav-link{color:var(--text-primary);text-decoration:none;font-weight:600;letter-spacing:0.8px;text-transform:uppercase;font-size:13px}
        .hamburger{display:none}
        @media(max-width:768px){.nav-menu{display:none}.nav-menu.active{display:flex;flex-direction:column;padding:2rem;background:rgba(10,10,20,0.95);gap:1.25rem}.hamburger{display:flex;gap:6px}}

        /* HERO */
        .hero{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:2rem;margin-top:60px;position:relative;overflow:hidden}
        .hero-content{max-width:var(--max-width);width:100%;display:grid;grid-template-columns:1fr 1fr;gap:3.5rem;align-items:center}
        @media(max-width:968px){.hero-content{grid-template-columns:1fr}.hero-3d{order:-1;height:350px}}
        .hero-title{font-size:clamp(48px,8vw,80px);font-weight:800;line-height:1.05;margin-bottom:0.5rem}
        .hero-subtitle{font-size:clamp(18px,3vw,24px);font-weight:600;color:var(--accent-primary);margin-bottom:1rem}
        .hero-description{font-size:16px;color:var(--text-secondary);max-width:520px;margin-bottom:1.25rem}
        .hero-buttons{display:flex;gap:1rem;flex-wrap:wrap}
        .btn{padding:12px 28px;border-radius:40px;border:2px solid var(--accent-primary);background:transparent;color:var(--text-primary);cursor:pointer;font-weight:700;letter-spacing:0.6px}
        .btn-primary{background:linear-gradient(90deg,var(--accent-primary),var(--accent-light));color:#07030a}
        .btn-secondary{border-color:rgba(168,85,247,0.35);color:var(--accent-primary);}

        /* 3D CANVAS & fallback text spacing */
        .hero-3d{position:relative;height:520px;display:flex;align-items:center;justify-content:center}
        #canvas3d{width:100%;height:100%;display:block}
        .no-webgl{display:none;color:var(--text-secondary);padding:1rem;text-align:center;max-width:640px;margin:0 auto}
        /* ensure descriptive text spacing under canvas */
        #canvas-desc{margin-top:0.75rem}

        /* PROFILE: grid + card spacing */
        .profile-grid{display:grid;grid-template-columns:1fr 1fr;gap:2.5rem;align-items:start}
        @media(max-width:920px){.profile-grid{grid-template-columns:1fr}}
        .profile-card{background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);padding:var(--card-padding);border-radius:12px;border:1px solid rgba(168,85,247,0.06);box-shadow:0 6px 30px rgba(0,0,0,0.45)}
        .profile-header{display:flex;gap:1rem;align-items:center;margin-bottom:0.75rem}
        .avatar{width:72px;height:72px;border-radius:12px;background:linear-gradient(90deg,var(--accent-primary),var(--accent-light));display:flex;align-items:center;justify-content:center;font-weight:800;color:#fff;font-size:20px}
        .profile-info h3{font-size:20px;margin-bottom:0.25rem}
        .profile-info p{margin-bottom:0.25rem;color:var(--text-secondary)}
        .skills-grid{display:grid;grid-template-columns:1fr 1fr;gap:0.8rem}
        @media(max-width:520px){.skills-grid{grid-template-columns:1fr}}

        /* skill card spacing */
        .skill-card{background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);padding:0.75rem;border-radius:10px;border:1px solid rgba(168,85,247,0.04)}
        .skill-header{display:flex;justify-content:space-between;font-weight:600;margin-bottom:0.5rem}
        .skill-bar{height:8px;background:rgba(255,255,255,0.04);border-radius:999px;overflow:hidden}
        .skill-progress{height:100%;width:0;background:linear-gradient(90deg,var(--accent-primary),var(--accent-light));transition:width 1s ease}

        /* RESUME timeline spacing */
        .resume-grid{display:grid;grid-template-columns:1fr 1fr;gap:2rem}
        @media(max-width:920px){.resume-grid{grid-template-columns:1fr}}
        .timeline{display:flex;flex-direction:column;gap:1rem}
        .timeline-item{display:flex;gap:0.75rem;padding:0.75rem;border-radius:10px;border:1px solid rgba(255,255,255,0.02);background:transparent}
        .timeline-meta{color:var(--text-secondary);font-size:13px;margin-bottom:0.35rem}
        .timeline-description{color:var(--text-secondary);font-size:15px;line-height:1.6}

        /* PROJECTS spacing & card layout */
        .projects-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.25rem}
        @media(max-width:1100px){.projects-grid{grid-template-columns:repeat(2,1fr)}}
        @media(max-width:680px){.projects-grid{grid-template-columns:1fr}}
        .project-card{padding:var(--card-padding);border-radius:12px;background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);border:1px solid rgba(168,85,247,0.04);display:flex;gap:1rem;align-items:flex-start}
        .project-content h3{margin-bottom:0.4rem}
        .project-description{color:var(--text-secondary);margin-bottom:0.6rem;line-height:1.6}
        .project-tech{display:flex;gap:0.6rem;font-size:13px;color:var(--text-secondary);margin-bottom:0.6rem}

        /* CONTACT: form spacing improvements */
        .contact-wrapper{display:grid;grid-template-columns:1fr 360px;gap:2rem;align-items:start}
        @media(max-width:920px){.contact-wrapper{grid-template-columns:1fr}}
        .contact-form-box{padding:var(--card-padding);border-radius:12px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);border:1px solid rgba(168,85,247,0.04)}
        .form-group{margin-bottom:0.8rem}
        label{display:block;margin-bottom:0.35rem;color:var(--text-secondary);font-weight:600}
        input,textarea{width:100%;padding:0.75rem;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text-primary);resize:vertical}
        .btn-submit{padding:12px 20px;border-radius:10px;background:linear-gradient(90deg,var(--accent-primary),var(--accent-light));border:none;color:#07030a;font-weight:700;cursor:pointer}

        /* FOOTER */
        .footer{padding:2rem 0;text-align:center;color:var(--text-secondary)}
    </style>
</head>
<body>
    <!-- NAV -->
    <nav class="navbar" role="navigation" aria-label="Main navigation">
        <div class="nav-container">
            <div class="nav-logo" aria-hidden="false"><span>RRP</span><span style="width:8px;height:8px;display:inline-block;background:var(--accent-light);border-radius:50%;margin-left:8px"></span></div>
            <ul class="nav-menu" id="navMenu">
                <li><a href="#home" class="nav-link">Beranda</a></li>
                <li><a href="#profile" class="nav-link">Profile</a></li>
                <li><a href="#resume" class="nav-link">Resume</a></li>
                <li><a href="#projects" class="nav-link">Project</a></li>
                <li><a href="#contact" class="nav-link">Kontak</a></li>
            </ul>
            <button class="hamburger" id="hamburger" aria-label="Toggle navigation" aria-expanded="false" aria-controls="navMenu"><span style="width:22px;height:3px;background:var(--accent-primary);display:block;border-radius:2px;margin:4px 0"></span><span style="width:18px;height:3px;background:var(--accent-primary);display:block;border-radius:2px;margin:4px 0"></span><span style="width:14px;height:3px;background:var(--accent-primary);display:block;border-radius:2px;margin:4px 0"></span></button>
        </div>
    </nav>

    <main>
        <!-- HERO -->
        <section id="home" class="hero" role="region" aria-labelledby="home-title">
            <div class="hero-content container">
                <div class="hero-text">
                    <h1 id="home-title" class="hero-title">Hi, saya <span style="color:var(--accent-light)">Rifqy</span></h1>
                    <p class="hero-subtitle">Creative Developer & Designer</p>
                    <p class="hero-description text-block">Mahasiswa Sistem Informasi dengan minat pada pengembangan web, desain UI/UX, dan konten visual. Saya membuat pengalaman web modern—menggabungkan estetika visual dengan interaksi yang halus dan fungsionalitas yang jelas.</p>
                    <div class="hero-buttons">
                        <button id="contactBtn" class="btn btn-primary">Hubungi Saya</button>
                        <button id="projectsBtn" class="btn btn-secondary">Lihat Project</button>
                    </div>
                </div>

                <div class="hero-3d" aria-hidden="true">
                    <canvas id="canvas3d" aria-hidden="true"></canvas>
                    <div class="no-webgl" id="canvasFallback">Konten 3D tidak tersedia pada perangkat ini. Anda tetap dapat menjelajahi portofolio.</div>
                    <div class="glow-ring" style="position:absolute;inset:0;border-radius:50%;pointer-events:none;border:2px solid rgba(168,85,247,0.18)"></div>
                    <p id="canvas-desc" class="sr-only">Robot 3D interaktif yang merespons gerakan kursor.</p>
                </div>
            </div>
        </section>

        <!-- PROFILE -->
        <section id="profile" class="section" role="region" aria-labelledby="profile-title">
            <div class="section-container container">
                <h2 id="profile-title" class="section-title">Tentang Saya</h2>
                <div class="heading-divider" aria-hidden="true"></div>
                <p class="section-intro">Saya seorang mahasiswa Sistem Informasi yang menggabungkan ketertarikan pada teknologi, desain, dan konten visual. Fokus saya adalah membangun antarmuka yang mudah digunakan, estetis, dan efisien secara teknis.</p>

                <div class="profile-grid">
                    <div class="profile-card">
                        <div class="profile-header">
                            <div class="avatar">RP</div>
                            <div class="profile-info">
                                <h3>Rifqy Ridho Pratama</h3>
                                <p class="profile-status">Mahasiswa • Sistem Informasi</p>
                                <p class="profile-location">📍 Cilacap, Indonesia</p>
                            </div>
                        </div>
                        <p class="profile-description text-block">Saya fokus pada pengembangan web, UI/UX, dan pembuatan konten visual yang rapi. Dalam tiap proyek, saya menjaga keseimbangan antara bentuk dan fungsi—mengutamakan pengalaman pengguna yang jelas dan estetika yang konsisten.</p>
                        <div style="display:flex;gap:0.75rem;flex-wrap:wrap">
                            <a class="btn btn-secondary" href="#projects">Lihat Proyek</a>
                        </div>
                    </div>

                    <div>
                        <h3 style="font-size:18px;margin-bottom:0.75rem;color:var(--accent-primary);text-transform:uppercase;letter-spacing:1px">Keahlian</h3>
                        <div class="skills-grid">
                            <div class="skill-card">
                                <div class="skill-header"><span>UI/UX Design</span><span>90%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="90"></div></div>
                            </div>
                            <div class="skill-card">
                                <div class="skill-header"><span>Web Development</span><span>85%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="85"></div></div>
                            </div>
                            <div class="skill-card">
                                <div class="skill-header"><span>Data Analysis</span><span>80%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="80"></div></div>
                            </div>
                            <div class="skill-card">
                                <div class="skill-header"><span>Video Editing</span><span>88%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="88"></div></div>
                            </div>
                            <div class="skill-card">
                                <div class="skill-header"><span>JavaScript</span><span>82%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="82"></div></div>
                            </div>
                            <div class="skill-card">
                                <div class="skill-header"><span>3D Web Tech</span><span>75%</span></div>
                                <div class="skill-bar"><div class="skill-progress" data-width="75"></div></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- RESUME -->
        <section id="resume" class="section" role="region" aria-labelledby="resume-title">
            <div class="section-container container">
                <h2 id="resume-title" class="section-title">Resume</h2>
                <div class="heading-divider" aria-hidden="true"></div>
                <p class="section-intro">Riwayat pendidikan dan pengalaman yang relevan. Saya menyajikan hal-hal penting dan ringkas agar mudah dibaca oleh pengunjung.</p>

                <div class="resume-grid">
                    <div class="timeline-group">
                        <h3 class="timeline-title">Pendidikan</h3>
                        <div class="timeline">
                            <div class="timeline-item">
                                <div>
                                    <h4>Universitas Komputama Majenang</h4>
                                    <p class="timeline-meta">2025 - Sekarang</p>
                                    <p class="timeline-description">Program Studi Sistem Informasi</p>
                                </div>
                            </div>
                            <div class="timeline-item">
                                <div>
                                    <h4>MA Minhajut Tholabah Purbalingga</h4>
                                    <p class="timeline-meta">2022 - 2025</p>
                                    <p class="timeline-description">Sekolah Menengah Atas</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="timeline-group">
                        <h3 class="timeline-title">Pengalaman</h3>
                        <div class="timeline">
                            <div class="timeline-item">
                                <div>
                                    <h4>Student Information System</h4>
                                    <p class="timeline-meta">Project</p>
                                    <p class="timeline-description">Sistem manajemen data mahasiswa berbasis web dengan MySQL dan desain database yang terstruktur.</p>
                                </div>
                            </div>
                            <div class="timeline-item">
                                <div>
                                    <h4>Edge Eye - YouTube Channel</h4>
                                    <p class="timeline-meta">Content Creator</p>
                                    <p class="timeline-description">Editing video lirik estetis hitam putih menggunakan teknik profesional.</p>
                                </div>
                            </div>
                            <div class="timeline-item">
                                <div>
                                    <h4>HIMSI Organization</h4>
                                    <p class="timeline-meta">Anggota Aktif</p>
                                    <p class="timeline-description">Berpartisipasi dalam kegiatan organisasi dan pengembangan keterampilan teknis.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- PROJECTS -->
        <section id="projects" class="section" role="region" aria-labelledby="projects-title">
            <div class="section-container container">
                <h2 id="projects-title" class="section-title">Project Saya</h2>
                <div class="heading-divider" aria-hidden="true"></div>
                <p class="section-intro">Beberapa proyek yang pernah dikerjakan—ringkas, jelas, dan fokus pada hasil.</p>

                <div class="projects-grid">
                    <div class="project-card" tabindex="0">
                        <div style="font-size:34px;margin-right:0.5rem">💻</div>
                        <div class="project-content">
                            <h3>Student Information System</h3>
                            <div class="project-tech"><span>Web Dev</span><span>MySQL</span><span>Database</span></div>
                            <p class="project-description">Sistem manajemen data mahasiswa berbasis web dengan fitur lengkap untuk pengelolaan akademik dan pelaporan otomatis.</p>
                            <div style="display:flex;gap:0.5rem"><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">Demo</a><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">GitHub</a></div>
                        </div>
                    </div>

                    <div class="project-card" tabindex="0">
                        <div style="font-size:34px;margin-right:0.5rem">🎬</div>
                        <div class="project-content">
                            <h3>Edge Eye - YouTube</h3>
                            <div class="project-tech"><span>Editing</span><span>CapCut</span><span>Content</span></div>
                            <p class="project-description">Channel YouTube yang menampilkan karya editing video lirik estetis dengan gaya hitam-putih.</p>
                            <div style="display:flex;gap:0.5rem"><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">Channel</a><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">Portfolio</a></div>
                        </div>
                    </div>

                    <div class="project-card" tabindex="0">
                        <div style="font-size:34px;margin-right:0.5rem">🎨</div>
                        <div class="project-content">
                            <h3>3D Interactive Portfolio</h3>
                            <div class="project-tech"><span>Three.js</span><span>WebGL</span><span>3D</span></div>
                            <p class="project-description">Portofolio pribadi dengan sentuhan 3D modern, efek glasmorphism, dan animasi halus untuk pengalaman yang menarik.</p>
                            <div style="display:flex;gap:0.5rem"><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">Live</a><a href="#" class="btn btn-secondary" style="padding:8px 12px;font-size:13px">GitHub</a></div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- CONTACT -->
        <section id="contact" class="section" role="region" aria-labelledby="contact-title">
            <div class="section-container container">
                <h2 id="contact-title" class="section-title">Hubungi Saya</h2>
                <div class="heading-divider" aria-hidden="true"></div>
                <p class="section-intro">Jika ingin bekerja sama atau menanyakan sesuatu, silakan kirim pesan melalui formulir berikut.</p>

                <div class="contact-wrapper">
                    <div class="contact-form-box">
                        <form class="contact-form" id="contactForm" aria-label="Contact form">
                            <div class="form-group">
                                <label for="name">Nama</label>
                                <input type="text" id="name" name="name" placeholder="Masukkan nama Anda" required aria-required="true">
                            </div>
                            <div class="form-group">
                                <label for="email">Email</label>
                                <input type="email" id="email" name="email" placeholder="Masukkan email Anda" required aria-required="true">
                            </div>
                            <div class="form-group">
                                <label for="message">Pesan</label>
                                <textarea id="message" name="message" placeholder="Tulis pesan Anda..." rows="6" required aria-required="true"></textarea>
                            </div>
                            <button type="submit" class="btn-submit">Kirim Pesan</button>
                        </form>
                    </div>

                    <aside class="contact-info-box" aria-label="Informasi kontak">
                        <div style="padding:var(--card-padding);border-radius:12px;border:1px solid rgba(168,85,247,0.04);background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent)">
                            <div style="margin-bottom:0.75rem">
                                <h4 style="margin-bottom:0.25rem">Email</h4>
                                <a href="mailto:rifqyridhopratama29@gmail.com" style="color:var(--accent-primary)">rifqyridhopratama29@gmail.com</a>
                            </div>
                            <div style="margin-bottom:0.75rem">
                                <h4 style="margin-bottom:0.25rem">Telepon</h4>
                                <a href="tel:+6285876184825" style="color:var(--text-primary)">+62-858-7618-4825</a>
                            </div>
                            <div style="margin-bottom:0.75rem">
                                <h4 style="margin-bottom:0.25rem">Lokasi</h4>
                                <p style="color:var(--text-secondary)">Cilacap, Central Java, Indonesia</p>
                            </div>
                            <div>
                                <h4 style="margin-bottom:0.5rem">Sosial Media</h4>
                                <div style="display:flex;gap:0.5rem">
                                    <a class="btn btn-secondary" href="#" aria-label="Instagram">IG</a>
                                    <a class="btn btn-secondary" href="#" aria-label="LinkedIn">IN</a>
                                    <a class="btn btn-secondary" href="#" aria-label="GitHub">GH</a>
                                </div>
                            </div>
                        </div>
                    </aside>
                </div>
            </div>
        </section>
    </main>

    <!-- FOOTER -->
    <footer class="footer">
        <div class="container">
            <p style="margin-bottom:6px">&copy; 2026 Rifqy Ridho Pratama. All rights reserved.</p>
            <p style="color:var(--text-secondary)">Crafted with <span aria-hidden="true">💜</span> and modern web technology</p>
        </div>
    </footer>

    <!-- Three.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js" crossorigin="anonymous"></script>

    <script>
        // minimal JS to preserve original interactions (kept as-is with small updates)
        // Cursor glow (defensive)
        (function(){
            const cursorGlow = document.querySelector('.cursor-glow');
            if(cursorGlow){
                document.addEventListener('mousemove', (e)=> {
                    cursorGlow.style.left = (e.clientX - 20) + 'px';
                    cursorGlow.style.top = (e.clientY - 20) + 'px';
                }, {passive:true});
            }
        })();

        // mobile menu
        (function(){
            const hamburger = document.getElementById('hamburger');
            const navMenu = document.getElementById('navMenu');
            if(hamburger && navMenu){
                hamburger.addEventListener('click', ()=> {
                    const expanded = hamburger.getAttribute('aria-expanded') === 'true';
                    hamburger.setAttribute('aria-expanded', String(!expanded));
                    navMenu.classList.toggle('active');
                });
                document.querySelectorAll('.nav-link').forEach(link => {
                    link.addEventListener('click', ()=> {
                        if(navMenu.classList.contains('active')){
                            navMenu.classList.remove('active');
                            hamburger.setAttribute('aria-expanded','false');
                        }
                    });
                });
            }
        })();

        // hero buttons scroll (replace inline)
        (function(){
            const contactBtn = document.getElementById('contactBtn');
            const projectsBtn = document.getElementById('projectsBtn');
            if(contactBtn) contactBtn.addEventListener('click', ()=> document.getElementById('contact').scrollIntoView({behavior:'smooth'}));
            if(projectsBtn) projectsBtn.addEventListener('click', ()=> document.getElementById('projects').scrollIntoView({behavior:'smooth'}));
        })();

        // skill bars animation (kept)
        (function animateSkillBars(){
            const skillCards = document.querySelectorAll('.skill-card');
            if(!('IntersectionObserver' in window)){
                document.querySelectorAll('.skill-progress').forEach(bar => {
                    const w = bar.getAttribute('data-width')||0; bar.style.width = w + '%';
                });
                return;
            }
            const obs = new IntersectionObserver((entries, o)=> {
                entries.forEach(entry => {
                    if(entry.isIntersecting){
                        entry.target.querySelectorAll('.skill-progress').forEach(bar => {
                            const w = bar.getAttribute('data-width')||0;
                            setTimeout(()=> bar.style.width = w + '%',100);
                        });
                        o.unobserve(entry.target);
                    }
                });
            },{threshold:0.15});
            skillCards.forEach(c => obs.observe(c));
        })();

        // simple contact form -> mailto
        (function contactHandler(){
            const form = document.getElementById('contactForm');
            if(!form) return;
            form.addEventListener('submit', (e)=> {
                e.preventDefault();
                const name = document.getElementById('name').value.trim();
                const email = document.getElementById('email').value.trim();
                const message = document.getElementById('message').value.trim();
                if(!name||!email||!message){ alert('Mohon isi semua kolom'); return; }
                const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
                if(!emailRegex.test(email)){ alert('Email tidak valid'); return; }
                const subject = `Pesan dari ${name}`;
                const body = `${message}\n\nDari: ${name} <${email}>`;
                window.location.href = 'mailto:rifqyridhopratama29@gmail.com?subject=' + encodeURIComponent(subject) + '&body=' + encodeURIComponent(body);
                form.reset();
            });
        })();

        // Three.js robot (kept - minimal resource saving: pause on tab hidden)
        (function initThreeJS(){
            const canvas = document.getElementById('canvas3d');
            const fallback = document.getElementById('canvasFallback');
            if(!canvas || typeof THREE === 'undefined'){ if(fallback) fallback.style.display='block'; return; }

            const rect = canvas.getBoundingClientRect();
            const w = Math.max(300, rect.width);
            const h = Math.max(150, rect.height);

            const scene = new THREE.Scene();
            const camera = new THREE.PerspectiveCamera(75, w/h, 0.1, 1000);
            const renderer = new THREE.WebGLRenderer({canvas, antialias:true, alpha:true});
            const DPR = Math.min(window.devicePixelRatio||1, 2);
            renderer.setPixelRatio(DPR);
            renderer.setSize(w,h,false);
            renderer.setClearColor(0x000000,0);
            camera.position.z = 5;

            const robotGroup = new THREE.Group(); scene.add(robotGroup);

            const body = new THREE.Mesh(new THREE.CylinderGeometry(0.8,0.8,2,32), new THREE.MeshStandardMaterial({color:0xffffff, roughness:0.4, metalness:0.3}));
            body.position.y = -0.5; robotGroup.add(body);

            const head = new THREE.Mesh(new THREE.SphereGeometry(0.6,32,32), new THREE.MeshStandardMaterial({color:0xffffff, roughness:0.4, metalness:0.3}));
            head.position.y = 1.8; robotGroup.add(head);

            const eyeMat = new THREE.MeshStandardMaterial({color:0xa855f7, emissive:0xa855f7, emissiveIntensity:0.6});
            const leftEye = new THREE.Mesh(new THREE.SphereGeometry(0.15,16,16), eyeMat); leftEye.position.set(-0.2,2.0,0.55); robotGroup.add(leftEye);
            const rightEye = leftEye.clone(); rightEye.position.x = 0.2; robotGroup.add(rightEye);

            const leftArm = new THREE.Mesh(new THREE.BoxGeometry(0.3,1.5,0.3), new THREE.MeshStandardMaterial({color:0xffffff}));
            leftArm.position.set(-1.2,0.5,0); robotGroup.add(leftArm);
            const rightArm = leftArm.clone(); rightArm.position.x = 1.2; robotGroup.add(rightArm);

            const leftLeg = new THREE.Mesh(new THREE.BoxGeometry(0.3,1.2,0.3), new THREE.MeshStandardMaterial({color:0xa855f7}));
            leftLeg.position.set(-0.4,-1.8,0); robotGroup.add(leftLeg);
            const rightLeg = leftLeg.clone(); rightLeg.position.x = 0.4; robotGroup.add(rightLeg);

            scene.add(new THREE.AmbientLight(0xffffff,0.6));
            const pl1 = new THREE.PointLight(0xa855f7,1,100); pl1.position.set(5,5,5); scene.add(pl1);
            const pl2 = new THREE.PointLight(0xd946ef,1,100); pl2.position.set(-5,-5,5); scene.add(pl2);

            let mouseX=0, mouseY=0, targetX=0, targetY=0;
            document.addEventListener('mousemove',(e)=>{ targetX = (e.clientX / window.innerWidth) * 2 - 1; targetY = -(e.clientY / window.innerHeight) * 2 + 1; },{passive:true});
            document.addEventListener('touchmove',(e)=>{ if(e.touches && e.touches[0]){ targetX = (e.touches[0].clientX / window.innerWidth) * 2 - 1; targetY = -(e.touches[0].clientY / window.innerHeight) * 2 + 1; } },{passive:true});

            let running = true;
            function animate(){
                if(!running) return;
                requestAnimationFrame(animate);
                mouseX += (targetX - mouseX) * 0.08;
                mouseY += (targetY - mouseY) * 0.08;
                robotGroup.rotation.y = mouseX * 0.5;
                robotGroup.rotation.x = mouseY * 0.3;

                const t = Date.now() * 0.001;
                leftArm.rotation.z = Math.sin(t) * 0.3;
                rightArm.rotation.z = -Math.sin(t) * 0.3;
                leftLeg.rotation.x = Math.sin(t * 1.5) * 0.15;
                rightLeg.rotation.x = -Math.sin(t * 1.5) * 0.15;

                const ei = 0.5 + Math.sin(t * 2) * 0.3;
                leftEye.material.emissiveIntensity = Math.max(0, ei);
                rightEye.material.emissiveIntensity = Math.max(0, ei);

                renderer.render(scene, camera);
            }
            animate();

            document.addEventListener('visibilitychange', ()=> {
                running = !document.hidden;
                if(running) animate();
            });
            const ro = new ResizeObserver(entries => {
                for(let entry of entries){
                    const cr = entry.contentRect;
                    const ww = Math.max(300, cr.width);
                    const hh = Math.max(150, cr.height);
                    camera.aspect = ww / hh; camera.updateProjectionMatrix();
                    renderer.setSize(ww, hh, false);
                }
            });
            ro.observe(canvas);
            window.addEventListener('resize', ()=> {
                const rect2 = canvas.getBoundingClientRect();
                const ww = Math.max(300, rect2.width);
                const hh = Math.max(150, rect2.height);
                camera.aspect = ww / hh; camera.updateProjectionMatrix();
                renderer.setSize(ww, hh, false);
            }, {passive:true});
        })();
    </script>
</body>
</html>
