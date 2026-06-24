<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>makeup-me - Maskne Control</title>
    <!-- Font Awesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* --- RESET & BASIC STYLES --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: #f9f9f9;
            color: #333;
        }

        a { text-decoration: none; color: inherit; }
        ul { list-style: none; display: flex; gap: 20px; }

        /* --- HALAMAN LOGIN (Popup) --- */
        #login-page {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100vh;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
            z-index: 9999;
            align-items: center;
            justify-content: center;
        }
        #login-page.active { display: flex; }

        .login-box {
            width: 100%; max-width: 400px;
            background: #ffffff; padding: 40px 30px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            text-align: center;
            position: relative;
        }
        .login-box h2 { color: #000; margin-bottom: 20px; font-family: serif; letter-spacing: 1px; }
        .login-input {
            width: 100%; padding: 12px 15px; margin-bottom: 15px;
            border: 1px solid #ddd; border-radius: 4px; outline: none;
        }
        .login-btn {
            width: 100%; padding: 12px; background: #000;
            color: #fff; border: none; border-radius: 4px; font-size: 16px; cursor: pointer; font-weight: 600;
            text-transform: uppercase;
        }
        .login-btn:hover { background: #333; }
        .close-login {
            position: absolute; top: 15px; right: 20px;
            font-size: 24px; cursor: pointer; color: #888;
        }

        /* --- HEADER SECTION --- */
        .top-bar {
            background: #ffdbdb; /* Warna pink muda header atas */
            padding: 8px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 11px;
            font-weight: 600;
            color: #444;
            flex-wrap: wrap;
            gap: 5px;
        }
        .promo-code { background: #3f51b5; color: #fff; padding: 2px 8px; border-radius: 2px; margin-left: 5px;}

        .nav-top-links {
            display: flex;
            gap: 15px;
            align-items: center;
        }
        .nav-top-links i { margin-right: 5px; }

        /* Main Header */
        .main-header {
            background: #fff;
            padding: 20px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
            flex-wrap: wrap;
            gap: 15px;
        }

        .brand-area {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .brand-name { font-size: 28px; font-weight: 700; color: #000; font-family: serif; }
        .badge-circle {
            background: #d32f2f; color: #fff; font-size: 10px;
            border-radius: 50%; width: 40px; height: 40px;
            display: flex; justify-content: center; align-items: center; text-align: center;
            font-weight: bold; line-height: 1.1;
        }

        .search-area {
            flex: 1;
            max-width: 500px;
            min-width: 200px;
            display: flex;
        }
        .search-area input {
            width: 100%; padding: 10px 15px;
            border: 1px solid #ddd; border-right: none; outline: none;
        }
        .search-area button {
            padding: 10px 15px; background: #fff; border: 1px solid #ddd;
            cursor: pointer;
        }

        .header-actions {
            display: flex; gap: 20px; font-size: 13px; font-weight: 600; color: #333;
        }
        .header-actions i { margin-right: 5px; }
        .login-trigger { cursor: pointer; }
        .login-trigger:hover { color: #000; }

        /* Sub Nav */
        .sub-nav {
            background: #fff;
            padding: 12px 5%;
            display: flex;
            justify-content: space-between;
            font-size: 13px;
            color: #555;
            border-bottom: 1px solid #eee;
        }
        .sub-nav ul li { cursor: pointer; }
        .sub-nav ul li:hover { color: #000; font-weight: 500; }

        /* --- HERO BANNER (Maskne Control) --- */
        .hero-section {
            background-color: #ffc0cb; /* Pink cerah */
            padding: 40px 5%;
            margin: 20px 5% 40px 5%;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: relative;
            overflow: hidden;
            min-height: 300px;
        }

        /* Mockup Produk (Simulasi Gambar Kiri) */
        .hero-products {
            display: flex;
            gap: 10px;
            flex: 1;
            align-items: flex-end;
            position: relative;
            z-index: 2;
        }
        
        .product-mockup {
            background: #fff;
            padding: 10px;
            border-radius: 4px;
            width: 80px;
            height: 140px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            transform: rotate(-5deg);
        }
        .product-mockup:nth-child(2) { height: 160px; transform: rotate(3deg); margin-top: -20px;}
        .product-mockup:nth-child(3) { height: 120px; transform: rotate(-2deg); }
        .product-mockup i { font-size: 40px; color: #555; align-self: center; margin-top: 20px;}
        .product-mockup span { font-size: 8px; font-weight: bold; text-align: center; }

        /* Teks Kanan */
        .hero-text {
            flex: 1;
            text-align: right;
            position: relative;
            z-index: 2;
            padding-left: 20px;
        }
        .hero-title {
            font-family: 'Times New Roman', Times, serif;
            font-size: 42px;
            color: #111;
            margin-bottom: 10px;
        }
        .hero-desc {
            font-size: 14px;
            color: #333;
            margin-bottom: 25px;
            line-height: 1.5;
        }
        .btn-black {
            background: #000; color: #fff; border: none;
            padding: 12px 30px; font-size: 14px; font-weight: 700; letter-spacing: 1px;
            cursor: pointer; border-radius: 2px; text-transform: uppercase;
        }
        .btn-black:hover { background: #333; }

        /* Navigasi Panah & Dot */
        .hero-arrow {
            position: absolute; right: -15px; top: 50%; transform: translateY(-50%);
            background: #fff; width: 40px; height: 40px; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1); cursor: pointer;
        }
        .hero-dots {
            position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
            display: flex; gap: 8px;
        }
        .hero-dots span { width: 8px; height: 8px; background: rgba(255,255,255,0.5); border-radius: 50%; display: block; }
        .hero-dots span.active { background: #fff; width: 10px; height: 10px; }

        /* --- FOOTER KONTAK --- */
        .contact-footer {
            background: #fff; max-width: 1200px; margin: 0 auto 40px auto; padding: 30px;
            border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.03); border-top: 1px solid #eee;
        }
        .contact-title { font-size: 14px; font-weight: 700; color: #333; margin-bottom: 15px; }
        .contact-items { display: flex; flex-wrap: wrap; gap: 30px; }
        .contact-item { display: flex; align-items: center; gap: 10px; font-size: 14px; color: #555; }
        .contact-item i { width: 20px; color: #d32f2f; text-align: center; }
        .contact-item a { color: #555; transition: 0.3s; }
        .contact-item a:hover { color: #000; }

        /* --- RESPONSIVE (HP) --- */
        @media (max-width: 768px) {
            .top-bar { justify-content: center; font-size: 10px; text-align: center; }
            .nav-top-links { display: none; } /* Sembunyikan menu atas di HP */
            .main-header { flex-direction: column; align-items: stretch; padding: 15px; }
            .brand-area { justify-content: center; }
            .search-area { width: 100%; max-width: none; }
            .sub-nav ul { display: none; } /* Sembunyikan menu navigasi di HP */
            .sub-nav { justify-content: center; padding: 10px; }
            
            .hero-section { flex-direction: column; padding: 30px 15px; text-align: center; margin: 10px; }
            .hero-products { justify-content: center; margin-bottom: 20px; }
            .hero-text { text-align: center; padding-left: 0; }
            .hero-title { font-size: 28px; }
            .hero-arrow, .hero-dots { display: none; }
            
            .contact-items { flex-direction: column; gap: 10px; }
        }
    </style>
</head>
<body>

    <!-- POPUP LOGIN -->
    <div id="login-page">
        <div class="login-box">
            <div class="close-login" onclick="toggleLogin()"><i class="fas fa-times"></i></div>
            <h2>makeup-me</h2>
            <input type="text" class="login-input" placeholder="Alamat Email">
            <input type="password" class="login-input" placeholder="Kata Sandi">
            <button class="login-btn" onclick="alert('Selamat datang di makeup-me!'); toggleLogin();">Login</button>
            <p style="margin-top:15px; font-size:13px; color:#777;">Belum punya akun? <a href="#" style="color:#000; font-weight:600;">Daftar</a></p>
        </div>
    </div>

    <!-- TOP BAR (Pink) -->
    <div class="top-bar">
        <div>
            <i class="fas fa-tag"></i> UP TO 70% OFF + VOUCHER 50% OFF 
            <span class="promo-code">CODE: MAKEUP820</span>
        </div>
        <div class="nav-top-links">
            <span>Customer service : <i class="far fa-envelope"></i> cs@makeup-me.com</span>
            <span><i class="fab fa-instagram"></i> @makeup_me</span>
        </div>
    </div>

    <!-- MAIN HEADER -->
    <header class="main-header">
        <div class="brand-area">
            <div class="brand-name">makeup-me</div>
            <div class="badge-circle">100% <br> BPOM</div>
        </div>
        
        <div class="search-area">
            <input type="text" placeholder="Cari produk skincare & makeup favoritmu! 🥰">
            <button><i class="fas fa-search"></i></button>
        </div>
        
        <div class="header-actions">
            <div class="login-trigger" onclick="toggleLogin()"><i class="far fa-user"></i> LOGIN</div>
            <div><i class="fas fa-shopping-bag"></i> MY BAG (0)</div>
        </div>
    </header>

    <!-- SUB NAVIGATION -->
    <nav class="sub-nav">
        <ul>
            <li>Categories</li>
            <li>Brands</li>
            <li>Promotions</li>
            <li>New Arrival <span style="color: #d32f2f; font-size: 8px; margin-left:2px;">NEW</span></li>
        </ul>
        <div style="font-weight: 500;">MAKEUP PROMO UP TO 50% OFF! 🎉</div>
    </nav>

    <!-- HERO BANNER (Maskne Control) -->
    <section class="hero-section">
        <!-- Simulasi Gambar Produk Kiri -->
        <div class="hero-products">
            <div class="product-mockup" style="background: #fce4ec;">
                <span>PREVENT<br>MASKNE</span>
                <i class="fas fa-flask"></i>
                <span style="background:#f8bbd0; color:#fff; padding:2px;">Cleanser</span>
            </div>
            <div class="product-mockup" style="background: #fff8e1; height: 170px; transform: rotate(5deg);">
                <i class="fas fa-tube" style="font-size:30px; margin-top:0; color:#fbc02d;"></i>
                <span>Neutrogena</span>
            </div>
            <div class="product-mockup" style="background: #e3f2fd; transform: rotate(-5deg);">
                <span>CURE MASKNE</span>
                <i class="fas fa-heart" style="color: #d32f2f; font-size:20px;"></i>
                <span style="font-size:7px;">Derma Angel</span>
            </div>
        </div>

        <!-- Teks Kanan -->
        <div class="hero-text">
            <h1 class="hero-title">MASKNE<br>Control</h1>
            <p class="hero-desc">Cegah jerawat saat memakai masker wajah<br>dengan rekomendasi skincare di sini!</p>
            <button class="btn-black">Shop Maskne Control</button>
        </div>

        <div class="hero-arrow"><i class="fas fa-chevron-right"></i></div>
        
        <div class="hero-dots">
            <span></span>
            <span></span>
            <span class="active"></span>
            <span></span>
            <span></span>
        </div>
    </section>

    <!-- FOOTER KONTAK (Data Anda) -->
    <footer class="contact-footer">
        <div class="contact-title">Butuh Bantuan? Hubungi makeup-me</div>
        <div class="contact-items">
            <div class="contact-item">
                <i class="fas fa-phone-alt"></i>
                <span>0806042009</span>
            </div>
            <div class="contact-item">
                <i class="fas fa-envelope"></i>
                <a href="mailto:infokanmakeup@gmail.com">infokanmakeup@gmail.com</a>
            </div>
            <div class="contact-item">
                <i class="fas fa-map-marker-alt"></i>
                <span>Jl. Pelangi Indah</span>
            </div>
        </div>
    </footer>

    <!-- SCRIPT LOGIN TOGGLE -->
    <script>
        function toggleLogin() {
            const loginPage = document.getElementById('login-page');
            if (loginPage.classList.contains('active')) {
                loginPage.classList.remove('active');
                document.body.style.overflow = 'auto';
            } else {
                loginPage.classList.add('active');
                document.body.style.overflow = 'hidden';
            }
        }
    </script>

</body>
</html>
