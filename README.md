<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Baby ShortLink</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: background-image 1s ease-in-out;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    .container {
      position: relative;
      background: rgba(255, 255, 255, 0.9);
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 8px 25px rgba(0,0,0,0.2);
      width: 90%;
      max-width: 500px;
      text-align: center;
      z-index: 1;
    }

    h1 {
      color: #0078ff;
      margin-bottom: 15px;
    }

    textarea {
      width: 100%;
      height: 120px;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 15px;
      margin-bottom: 10px;
      resize: none;
      background: rgba(255, 255, 255, 0.8);
    }

    button {
      background: #0078ff;
      color: white;
      border: none;
      padding: 10px 16px;
      border-radius: 8px;
      font-size: 15px;
      cursor: pointer;
      transition: background 0.3s;
      margin: 3px;
    }

    button:hover {
      background: #005fcc;
    }

    .result {
      margin-top: 15px;
      font-size: 14px;
      text-align: left;
      word-wrap: break-word;
    }

    .short-link {
      background: rgba(240, 248, 255, 0.9);
      padding: 8px;
      border-radius: 8px;
      margin-bottom: 6px;
    }

    .copy-btn {
      background: #00c851;
      border: none;
      padding: 4px 8px;
      color: white;
      border-radius: 8px;
      cursor: pointer;
      font-size: 13px;
      margin-left: 5px;
    }

    .clear-btn {
      background: #ff4444;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>🔗ShortLink</h1>
    <textarea id="longUrls" placeholder="Tempel banyak URL di sini (pisahkan dengan baris baru)..."></textarea>
    <div>
      <button id="shortenBtn">Pendekkan Semua</button>
      <button id="copyAllBtn">📋 Copy Semua</button>
      <button id="clearBtn" class="clear-btn">🧹 Hapus Semua</button>
    </div>
    <div class="result" id="result"></div>
  </div>

  <script>
    // API key TinyURL
    const API_KEY = "YhSIrCTfLiVXGGYEtXYMHjW2RgH3xugP6g1uUB0b6ieTo5BAavcrN0o9JNlb";

    // -------------------------
    // Ganti background otomatis
    // -------------------------
    const backgrounds = [
      "https://images.unsplash.com/photo-1507525428034-b723cf961d3e",
      "https://images.unsplash.com/photo-1529333166437-7750a6dd5a70",
      "https://images.unsplash.com/photo-1506744038136-46273834b3fb",
      "https://images.unsplash.com/photo-1491553895911-0055eca6402d",
      "https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05",
      "https://images.unsplash.com/photo-1486308510493-aa64833634ef"
    ];

    let index = 0;
    function changeBackground() {
      document.body.style.backgroundImage = `url('${backgrounds[index]}?auto=format&fit=crop&w=1920&q=90')`;
      index = (index + 1) % backgrounds.length;
    }
    changeBackground();
    setInterval(changeBackground, 2000);

    // -------------------------
    // Logika Shortlink
    // -------------------------
    document.getElementById('shortenBtn').addEventListener('click', async () => {
      const input = document.getElementById('longUrls');
      const result = document.getElementById('result');
      const urls = input.value.trim().split(/\n+/).filter(u => u);

      if (urls.length === 0) {
        result.innerHTML = "⚠️ Masukkan minimal satu URL ya Baby 💕";
        return;
      }

      result.innerHTML = "⏳ Sedang memproses semua link...";

      let allShortened = [];

      for (const longUrl of urls) {
        try {
          const response = await fetch('https://api.tinyurl.com/create', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + API_KEY
            },
            body: JSON.stringify({
              url: longUrl,
              domain: 'tinyurl.com'
            })
          });

          const data = await response.json();

          if (data.data && data.data.tiny_url) {
            const shortUrl = data.data.tiny_url;
            allShortened.push(shortUrl);
          } else {
            allShortened.push(`❌ Gagal: ${longUrl}`);
          }
        } catch (error) {
          allShortened.push(`⚠️ Error: ${longUrl}`);
          console.error(error);
        }
      }

      result.innerHTML = "<b>✅ Hasil Link Pendek:</b><br><br>" + 
        allShortened.map(link => `
          <div class="short-link">
            <a href="${link}" target="_blank">${link}</a>
            <button class="copy-btn" onclick="navigator.clipboard.writeText('${link}')">Copy</button>
          </div>
        `).join('');
    });

    // Copy semua link
    document.getElementById('copyAllBtn').addEventListener('click', () => {
      const links = [...document.querySelectorAll('.short-link a')].map(a => a.href).join('\n');
      if (links) {
        navigator.clipboard.writeText(links);
        alert('📋 Semua link berhasil disalin!');
      } else {
        alert('Belum ada link yang dipendekkan.');
      }
    });

    // Hapus input & hasil
    document.getElementById('clearBtn').addEventListener('click', () => {
      document.getElementById('longUrls').value = '';
      document.getElementById('result').innerHTML = '';
    });
  </script>

</body>
</html>
