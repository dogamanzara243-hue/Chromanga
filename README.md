<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
    <title>Chromanga | HD Manga Oku</title>
    
    <meta name="description" content="Chromanga - En yeni renklendirilmiş mangaları ve Belalı serisini ücretsiz oku.">
    
    <style>
        :root { --accent: #ff4757; --bg: #0a0a0c; --card: #16161a; }
        body { background: var(--bg); color: white; font-family: 'Segoe UI', sans-serif; margin: 0; overflow-x: hidden; }
        
        header { padding: 30px; text-align: center; border-bottom: 2px solid var(--accent); background: #000; }
        .logo { font-size: 28px; font-weight: bold; letter-spacing: 2px; }
        .logo span { color: var(--accent); }

        .container { padding: 20px; max-width: 1000px; margin: 0 auto; display: flex; justify-content: center; }
        
        /* MANGA KARTI */
        .manga-card { 
            background: var(--card); 
            border-radius: 15px; 
            overflow: hidden; 
            border: 1px solid #333; 
            cursor: pointer; 
            width: 300px; 
            transition: 0.3s;
        }
        .manga-card:hover { transform: translateY(-5px); border-color: var(--accent); }
        .manga-card img { width: 100%; height: auto; display: block; }
        .manga-info { padding: 15px; text-align: center; font-weight: bold; }

        /* OKUYUCU EKRANI */
        #reader { 
            display: none; 
            position: fixed; 
            top: 0; left: 0; 
            width: 100%; height: 100%; 
            background: #000; 
            z-index: 1000; 
            overflow-y: auto; 
        }
        
        .nav-bar { 
            position: sticky; 
            top: 0; 
            background: rgba(0,0,0,0.95); 
            padding: 10px 15px; 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            backdrop-filter: blur(10px);
            border-bottom: 1px solid #222;
        }

        #chapter-select { 
            background: #222; 
            color: white; 
            padding: 8px 15px; 
            border-radius: 8px; 
            border: 1px solid var(--accent);
            font-size: 14px;
        }

        .view-area { display: flex; flex-direction: column; align-items: center; width: 100%; }
        .view-area img { 
            width: 100%; 
            max-width: 800px; 
            height: auto; 
            display: block; 
            image-rendering: -webkit-optimize-contrast;
        }

        .footer-nav { padding: 60px 20px; text-align: center; background: #000; }
        
        /* BUTONLAR */
        .btn { 
            background: var(--accent); 
            color: white; 
            border: none; 
            padding: 10px 15px; 
            border-radius: 5px; 
            cursor: pointer; 
            font-weight: bold; 
        }
        .btn-next { 
            background: var(--accent); 
            color: white; 
            padding: 18px 40px; 
            border-radius: 10px; 
            font-size: 18px; 
            border: none; 
            cursor: pointer; 
            font-weight: bold;
            box-shadow: 0 5px 15px rgba(255, 71, 87, 0.3);
        }
    </style>
</head>
<body>

<div id="home">
    <header><div class="logo">CHRO<span>MANGA</span></div></header>
    <div class="container">
        <div class="manga-card" onclick="openChapter(0)">
            <img src="https://i.ibb.co/MkW5pHKG/Screenshot-2026-03-09-19-57-38-392-com-miui-gallery.jpg" alt="Belalı">
            <div class="manga-info">Belalı - Bölüm 1</div>
        </div>
    </div>
</div>

<div id="reader">
    <div class="nav-bar">
        <button class="btn" onclick="closeReader()">← Kapat</button>
        <select id="chapter-select" onchange="openChapter(this.value)"></select>
        <button class="btn" onclick="nextStep()">İleri →</button>
    </div>

    <div class="view-area" id="manga-images">
        </div>

    <div class="footer-nav">
        <button class="btn-next" onclick="nextStep()">SONRAKİ BÖLÜM</button>
    </div>
</div>

<script>
    // VERİ MERKEZİ: Yeni bölüm gelince buraya ekleme yapabilirsin
    const mangaData = [
        {
            label: "1. Bölüm",
            images: [
                "https://i.ibb.co/MkW5pHKG/Screenshot-2026-03-09-19-57-38-392-com-miui-gallery.jpg",
                "https://i.ibb.co/vxJwY8Wz/Screenshot-2026-03-09-19-57-41-883-com-miui-gallery.jpg",
                "https://i.ibb.co/G37CYMVH/Screenshot-2026-03-09-19-57-46-627-com-miui-gallery.jpg",
                "https://i.ibb.co/RT7jRbxm/Screenshot-2026-03-09-19-57-50-074-com-miui-gallery.jpg",
                "https://i.ibb.co/ZzZLjMJb/Screenshot-2026-03-09-19-57-52-958-com-miui-gallery.jpg",
                "https://i.ibb.co/v4ZCp47r/Screenshot-2026-03-09-19-57-56-217-com-miui-gallery.jpg",
                "https://i.ibb.co/qLd4FWLq/Screenshot-2026-03-09-19-57-59-360-com-miui-gallery.jpg",
                "https://i.ibb.co/zVdHrYZf/Screenshot-2026-03-09-19-58-02-648-com-miui-gallery.jpg",
                "https://i.ibb.co/3m4b4P6d/Screenshot-2026-03-09-19-58-05-965-com-miui-gallery.jpg"
            ]
        },
        {
            label: "2. Bölüm",
            images: ["https://via.placeholder.com/800x1200?text=Bölüm+2+Hazırlanıyor..."]
        }
    ];

    let activeChapter = 0;

    function openChapter(index) {
        activeChapter = parseInt(index);
        const chapter = mangaData[activeChapter];
        
        // Menü doldurma
        const select = document.getElementById('chapter-select');
        select.innerHTML = mangaData.map((ch, i) => 
            `<option value="${i}" ${i === activeChapter ? 'selected' : ''}>${ch.label}</option>`
        ).join('');

        // Resimleri basma
        const box = document.getElementById('manga-images');
        box.innerHTML = chapter.images.map(url => `<img src="${url}" loading="lazy">`).join('');
        
        document.getElementById('reader').style.display = 'block';
        document.getElementById('reader').scrollTo(0, 0);
    }

    function nextStep() {
        if (activeChapter + 1 < mangaData.length) {
            openChapter(activeChapter + 1);
        } else {
            alert("Tebrikler! Son bölüme geldiniz. Yeni bölümler için takipte kalın.");
        }
    }

    function closeReader() {
        document.getElementById('reader').style.display = 'none';
    }
</script>

</body>
</html>
