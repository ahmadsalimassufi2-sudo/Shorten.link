<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Shortener Link - Savelinku</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
    form { margin-bottom: 20px; }
    input[type="url"] { padding: 8px; width: 300px; }
    button { padding: 8px 16px; }
    .result { margin-top: 20px; font-size: 18px; }
    .error { color: red; }
  </style>
</head>
<body>
  <h1>Shortener Link (via Savelinku)</h1>
  <form id="shortenForm">
    <input type="url" id="longUrl" placeholder="Masukkan link panjang..." required>
    <button type="submit">Shorten</button>
  </form>

  <div id="output"></div>

  <script>
    // API Key kamu
    const apiKey = "d09c8d5f826f0aa1700b58196eb1b43ebb00818b";
    // endpoint Savelinku (contoh umum, bisa berbeda)
    const apiUrl = "https://safelinku.com/api";

    document.getElementById("shortenForm").addEventListener("submit", async function(e) {
      e.preventDefault();
      const longUrl = document.getElementById("longUrl").value.trim();
      const output = document.getElementById("output");

      if (!longUrl) {
        output.innerHTML = "<p class='error'>URL tidak boleh kosong!</p>";
        return;
      }

      try {
        const requestUrl = `${apiUrl}?api=${encodeURIComponent(apiKey)}&url=${encodeURIComponent(longUrl)}`;
        const response = await fetch(requestUrl);
        const data = await response.json();

        if (data.shortenedUrl) {
          output.innerHTML = `
            <div class="result">
              Link pendek: <a href="${data.shortenedUrl}" target="_blank">${data.shortenedUrl}</a>
            </div>
          `;
        } else {
          output.innerHTML = "<p class='error'>API tidak mengembalikan link pendek.</p>";
        }
      } catch (err) {
        output.innerHTML = "<p class='error'>Gagal terhubung ke API Savelinku.</p>";
      }
    });
  </script>
</body>
</html>
