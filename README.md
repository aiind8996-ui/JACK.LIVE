 # pdf upload Website 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>PDF Upload Form (Locked)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      user-select: none;
    }
    #videoButton {
      background-color: black;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    #videoButton:hover { background-color: #333; }

    .whatsapp-btn {
      width: 180px;
      height: 45px;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      margin: 5px;
      transition: 0.3s;
    }

    #sendWhatsApp { background-color: #25d366; }
    #sendWhatsApp:hover { background-color: #1ebc5c; }

    #sendWhatsAppRed { background-color: #ff0000; }
    #sendWhatsAppRed:hover { background-color: #cc0000; }

    /* 🎥 YouTube button style */
    #youtubeButton {
      background-color: #ff0000;
      color: white;
      border: none;
      padding: 10px 25px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      margin-top: 20px;
      transition: 0.3s;
    }
    #youtubeButton:hover {
      background-color: #cc0000;
    }

    .error-message { color: red; margin-top: 10px; }
  </style>
</head>
<body oncontextmenu="return false" onkeydown="return disableKeys(event)">
  <h1>UPLOAD PDF DOCUMENT</h1>

  <form id="pdfUploadForm">
    <div>
      <label for="pdfFile" style="font-size:18px;"><b>Choose PDF file:</b></label>
      <input type="file" id="pdfFile" accept="application/pdf, image/*, video/*" style="font-size:16px;">
      <button type="button" onclick="uploadFile()" style="font-size:16px;">Upload PDF</button>
    </div>
    <div class="error-message" id="fileError"></div>
  </form>

  <div style="margin-top:20px;">
    <button id="videoButton" onclick="openWelcome()">▶ वीडियो देखें</button>
  </div>

  <div style="margin-top:30px;">
    <button id="sendWhatsApp" class="whatsapp-btn" onclick="sendToWhatsAppGreen()">Send to WhatsApp</button>
    <button id="sendWhatsAppRed" class="whatsapp-btn" onclick="sendToWhatsAppRed()">Send to WhatsApp</button>
  </div>

  <script>
    let uploadedFiles = [];
    let lastPage = '';

    function getSavedFiles() {
      const files = localStorage.getItem('savedFiles');
      const backup = localStorage.getItem('backupFiles');
      if (files) return JSON.parse(files);
      if (backup) {
        localStorage.setItem('savedFiles', backup);
        return JSON.parse(backup);
      }
      return [];
    }

    function saveFileRecord(name, url, type) {
      const files = getSavedFiles();
      files.push({ name, url, type, date: new Date().toLocaleString() });
      localStorage.setItem('savedFiles', JSON.stringify(files));
      localStorage.setItem('backupFiles', JSON.stringify(files));
    }

    function uploadFile() {
      const fileInput = document.getElementById('pdfFile');
      const file = fileInput.files[0];
      const error = document.getElementById('fileError');
      error.textContent = "";

      if (!file) {
        error.textContent = "⚠ कृपया एक फ़ाइल चुनें।";
        return;
      }

      uploadedFiles.push(file);
      const fileURL = URL.createObjectURL(file);
      saveFileRecord(file.name, fileURL, file.type);
      alert("✅ " + file.name + " upload complete! (History me add ho gaya)");
    }

    function openWelcome() {
      if (uploadedFiles.length === 0) {
        alert("⚠ पहले कोई फ़ाइल अपलोड करें!");
        return;
      }

      const file = uploadedFiles[uploadedFiles.length - 1];
      const url = URL.createObjectURL(file);

      let fileDisplay = "";
      if (file.type.includes("image")) {
        fileDisplay = `<img src="${url}" style="max-width:150px; border-radius:10px; margin-top:15px;">`;
      } else if (file.type.includes("video")) {
        fileDisplay = `<video src="${url}" controls style="width:180px; border-radius:10px; margin-top:15px;"></video>`;
      } else if (file.type.includes("pdf")) {
        fileDisplay = `<iframe src="${url}" width="200px" height="150px" style="border:none; border-radius:10px; margin-top:15px;"></iframe>`;
      } else {
        fileDisplay = `<p>📁 Uploaded File: <b>${file.name}</b></p>`;
      }

      lastPage = document.body.innerHTML;

      document.body.innerHTML = `
        <style>
          body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: black;
            color: white;
            font-family: Arial, sans-serif;
            min-height: 100vh;
            margin: 0;
          }
          .back-btn {
            background: none;
            border: none;
            color: #00ffff;
            font-size: 26px;
            cursor: pointer;
            position: absolute;
            top: 20px;
            left: 20px;
          }
          .back-btn:hover { color: white; }
          .red-btn {
            background: none;
            border: none;
            color: #ff4444;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
            position: absolute;
            top: 20px;
            right: 20px;
          }
          h1 {
            font-size: 50px;
            text-align: center;
            text-shadow: 0 0 15px #00ffff;
            margin-top: 80px;
          }
          .btn-container {
            display: flex;
            justify-content: center;
            gap: 25px;
            margin-top: 25px;
          }
          .action-btn {
            background: none;
            border: none;
            color: #00ffff;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
          }
          #youtubeButton {
            background-color: #ff0000;
            color: white;
            border: none;
            padding: 10px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 18px;
            margin-top: 25px;
          }
          #youtubeButton:hover { background-color: #cc0000; }
        </style>

        <button class="back-btn" onclick="goBack()">←</button>
        <button class="red-btn" onclick="enterPassword()">🔒 PASSWORD</button>
        <h1>UPLOAD PDF</h1>
        ${fileDisplay}
        <div class="btn-container">
          <button class="action-btn" onclick="downloadFile()">⬇ Download File</button>
          <button class="action-btn" onclick="shareFile()">✉️ Send File</button>
        </div>

        <!-- 🎥 YouTube Button -->
        <button id="youtubeButton" onclick="openYouTube()">▶ Open YouTube</button>
      `;

      window.downloadFile = function () {
        const blob = new Blob([file], { type: file.type });
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = file.name;
        link.click();
      }

      window.shareFile = function () {
        const number = "7007576493";
        const message = "नमस्ते! मैंने एक फ़ाइल शेयर की है।";
        window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
      }

      window.openYouTube = function () {
        window.open("https://www.youtube.com/", "_blank");
      }

      window.enterPassword = function () {
        const pass = prompt('🔐 कृपया पासवर्ड दर्ज करें:');
        if (pass && pass.toLowerCase() === 'jack7005') {
          openHistoryPage();
        } else if (pass) {
          alert('❌ गलत पासवर्ड!');
        }
      }
    }

    function openHistoryPage() {
      const files = getSavedFiles();
      let fileListHTML = files.map(f => {
        if (f.type.includes("pdf")) {
          return `<iframe src="${f.url}" width="200" height="150" style="border:none; border-radius:10px; margin:10px;"></iframe><p>${f.name}<br><small>${f.date}</small></p>`;
        } else if (f.type.includes("image")) {
          return `<img src="${f.url}" style="max-width:150px; border-radius:10px; margin:10px;"><p>${f.name}<br><small>${f.date}</small></p>`;
        } else if (f.type.includes("video")) {
          return `<video src="${f.url}" controls style="width:180px; border-radius:10px; margin:10px;"></video><p>${f.name}<br><small>${f.date}</small></p>`;
        } else {
          return `<p>📁 ${f.name}<br><small>${f.date}</small></p>`;
        }
      }).join('');

      document.body.innerHTML = `
        <body oncontextmenu="return false" onkeydown="return disableKeys(event)">
        <style>
          body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
          }
          h1 {
            margin-top: 40px;
            font-size: 48px;
            text-shadow: 0 0 15px #00ffff;
          }
          .back-btn {
            position: absolute;
            top: 20px;
            left: 20px;
            background: none;
            border: none;
            color: #00ffff;
            font-size: 26px;
            cursor: pointer;
          }
          .file-container {
            margin-top: 30px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
          }
          .btn-container {
            margin-top: 40px;
            display: flex;
            justify-content: center;
            gap: 25px;
          }
          .action-btn {
            background: none;
            border: 1px solid #00ffff;
            color: #00ffff;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
            border-radius: 8px;
            padding: 8px 20px;
          }
        </style>
        <button class="back-btn" onclick="goBack()">←</button>
        <h1>HISTORY</h1>
        <div class="file-container">${fileListHTML}</div>
        <div class="btn-container">
          <button class="action-btn" onclick="shareHistory()">✉️ Send</button>
          <button class="action-btn" onclick="downloadHistory()">⬇ Download</button>
        </div>
        </body>
      `;

      window.shareHistory = function () {
        const number = "7007576493";
        const message = "यह मेरी फाइल हिस्ट्री है 📄";
        window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
      }

      window.downloadHistory = function () {
        const blob = new Blob([JSON.stringify(files, null, 2)], { type: "application/json" });
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = "history.json";
        link.click();
      }
    }

    window.goBack = function() {
      document.body.innerHTML = lastPage;
    }

    function sendToWhatsAppGreen() {
      const number = "7007576493";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
    }

    function sendToWhatsAppRed() {
      const number = "6392908732";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
    }

    function disableKeys(e) {
      if (
        e.keyCode === 123 || 
        (e.ctrlKey && e.shiftKey && e.keyCode === 73) || 
        (e.ctrlKey && e.shiftKey && e.keyCode === 74) || 
        (e.ctrlKey && e.keyCode === 85) || 
        (e.ctrlKey && e.keyCode === 83)
      ) {
        alert("🔒 Page is locked!");
        e.preventDefault();
        return false;
      }
    }
  </script>
</body>
</html>
