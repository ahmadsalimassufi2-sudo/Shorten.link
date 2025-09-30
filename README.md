<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>SafeLinkU Bulk Shortener</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f4f9; text-align: center; padding: 40px; }
    textarea, button { padding: 10px; margin: 5px; width: 80%; max-width: 600px; }
    textarea { height: 150px; }
    button { cursor: pointer; background: #007BFF; color: white; border: none; border-radius: 5px; }
    button:hover { background: #0056b3; }
    .result { margin-top: 20px; padding: 15px; background: #fff; border-radius: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.2); display: inline-block; width: 90%; max-width: 600px; text-align: left; }
    .link-item { display: flex; justify-content: space-between; align-items: center; margin: 5px 0; padding: 5px; background: #f9f9f9; border-radius: 5px; }
    .link-text { flex: 1; margin-right: 10px; }
  </style>
</head>
<body>
  <h2>🔗 SafeLinkU Bulk Link Shortener</h2>
  <p>Masukkan banyak link (pisahkan dengan baris baru):</p>
  <textarea id="longUrls" placeholder="https://link1.com&#10;https://link2.com&#10;https://link3.com"></textarea>
  <br>
  <button onclick="shortenLinks()">Shorten Semua</button>
  <button onclick="copyAll()">Copy Semua Hasil</button>

  <div id="results" class="result" style="display:none;">
    <h3>✅ Hasil Shortened Links:</h3>
    <div id="linksList"></div>
  </div>

  <script>
    async function shortenLinks() {
      const urls = document.getElementById("longUrls").value.trim().split("\n");
      const resultsBox = document.getElementById("results");
      const linksList = document.getElementById("linksList");
      linksList.innerHTML = "";

      if (urls.length === 0) {
        alert("Masukkan minimal satu link!");
        return;
      }

      resultsBox.style.display = "block";

      for (let url of urls) {
        url = url.trim();
        if (!url) continue;

        let listItem = document.createElement("div");
        listItem.className = "link-item";
        listItem.innerHTML = `<span class="link-text">⏳ Memproses: ${url}</span>`;
        linksList.appendChild(listItem);

        try {
          let response = await fetch("https://safelinku.com/api/v1/links", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
              "Authorization": "Bearer d09c8d5f826f0aa1700b58196eb1b43ebb00818b"
            },
            body: JSON.stringify({ url })
          });

          let data = await response.json();
          console.log("Response:", data);

          // cari field url hasil shorten
          let shortUrl = data?.data?.shortenedUrl || data?.shortenedUrl || data?.short_url;

          if (shortUrl) {
            listItem.innerHTML = `
              <input type="text" class="link-text" value="${shortUrl}" readonly>
              <button onclick="copyOne(this)">Copy</button>
            `;
          } else {
            listItem.innerHTML = `<span class="link-text">❌ Gagal memendekkan: ${url}</span>`;
          }
        } catch (error) {
          listItem.innerHTML = `<span class="link-text">⚠️ Error koneksi: ${url}</span>`;
          console.error("Fetch error:", error);
        }
      }
    }

    function copyOne(btn) {
      const input = btn.parentElement.querySelector(".link-text");
      navigator.clipboard.writeText(input.value || input.innerText).then(() => {
        alert("Disalin: " + (input.value || input.innerText));
      });
    }

    function copyAll() {
      const inputs = document.querySelectorAll("#linksList .link-text");
      let allLinks = [];
      inputs.forEach(el => {
        allLinks.push(el.value || el.innerText);
      });
      navigator.clipboard.writeText(allLinks.join("\n")).then(() => {
        alert("Semua link berhasil disalin!");
      });
    }
  </script>
</body>
</html>
