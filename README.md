            <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>PDF Upload Form</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
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

    .photo-section {
      margin-top: 30px;
      padding: 15px;
      border: 2px dashed #aaa;
      border-radius: 10px;
      max-width: 400px;
      text-align: center;
    }

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

    .error-message { color: red; margin-top: 10px; }
  </style>
</head>
<body>

  <h1>UPLOAD PDF DOCUMENT</h1>

  <form id="pdfUploadForm">
    <div>
      <label for="pdfFile" style="font-size:18px;"><b>Choose PDF file:</b></label>
      <input type="file" id="pdfFile" name="pdfFile" accept="application/pdf, image/*, video/*" style="font-size:16px;">
      <button type="button" onclick="uploadFile()" style="font-size:16px;">Upload PDF</button>
    </div>
    <div class="error-message" id="fileError"></div>
  </form>

  <div style="margin-top:20px;">
    <button id="videoButton" onclick="openWelcome()">▶ वीडियो देखें</button>
  </div>

  <div class="photo-section">
    <button id="sendWhatsApp" class="whatsapp-btn" onclick="sendToWhatsAppGreen()">Send to WhatsApp</button>
    <button id="sendWhatsAppRed" class="whatsapp-btn" onclick="sendToWhatsAppRed()">Send to WhatsApp</button>
  </div>

  <script>
    let uploadedFile = null;
    let uploadedURL = "";

    function uploadFile() {
      const fileInput = document.getElementById('pdfFile');
      const file = fileInput.files[0];
      const error = document.getElementById('fileError');
      error.textContent = "";

      if (!file) {
        error.textContent = "⚠ कृपया एक फ़ाइल चुनें।";
        return;
      }

      uploadedFile = file;
      uploadedURL = URL.createObjectURL(file);
      alert("✅ " + file.name + " upload complete!");
    }

    function openWelcome() {
      if (!uploadedFile) {
        alert("⚠ पहले कोई फ़ाइल अपलोड करें!");
        return;
      }

      const fileType = uploadedFile.type;
      let fileDisplay = "";

      if (fileType.includes("image")) {
        fileDisplay = `<img src="${uploadedURL}" alt="Uploaded Image" style="max-width:80%; border-radius:10px; margin-top:20px;">`;
      } else if (fileType.includes("video")) {
        fileDisplay = `<video src="${uploadedURL}" controls style="max-width:80%; border-radius:10px; margin-top:20px;"></video>`;
      } else if (fileType.includes("pdf")) {
        fileDisplay = `<iframe src="${uploadedURL}" width="80%" height="500px" style="border:2px solid #00ffff; border-radius:10px; margin-top:20px;"></iframe>`;
      } else {
        fileDisplay = `<p>📁 Uploaded File: <b>${uploadedFile.name}</b></p>`;
      }

      const newWindow = window.open('', '_blank');
      newWindow.uploadedFileBlob = uploadedFile;

      newWindow.document.write(`
        <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8"/>
          <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
          <title>Welcome</title>
          <style>
            body {
              display: flex;
              flex-direction: column;
              justify-content: center;
              align-items: center;
              min-height: 100vh;
              background: radial-gradient(circle, #000000 60%, #1a1a1a);
              color: white;
              font-family: Arial, sans-serif;
              position: relative;
            }
            .red-btn {
              background-color: red;
              color: white;
              border: none;
              padding: 12px 24px;
              border-radius: 8px;
              font-size: 18px;
              cursor: pointer;
              font-weight: bold;
              position: absolute;
              top: 20px;
              right: 20px;
              box-shadow: 0 0 10px #ff0000;
            }
            .red-btn:hover { background-color: darkred; }

            h1 {
              font-size: 50px;
              text-align: center;
              text-shadow: 0 0 15px #00ffff, 0 0 25px #00ffff;
              letter-spacing: 3px;
              animation: glow 2s ease-in-out infinite alternate;
              margin-bottom: 20px;
              margin-top: 80px;
            }
            @keyframes glow {
              from { text-shadow: 0 0 10px #00ffff, 0 0 20px #00ffff; }
              to { text-shadow: 0 0 25px #00ffff, 0 0 40px #00ffff; }
            }

            .btn-container {
              display: flex;
              justify-content: center;
              gap: 20px;
              margin-top: 20px;
            }

            .action-btn {
              background-color: #00ffff;
              color: black;
              padding: 12px 24px;
              border-radius: 8px;
              border: none;
              font-size: 18px;
              cursor: pointer;
              width: 180px;
              font-weight: bold;
              transition: 0.3s;
            }
            .action-btn:hover {
              background-color: black;
              color: #00ffff;
              border: 2px solid #00ffff;
              box-shadow: 0 0 15px #00ffff;
            }

            button.close-btn {
              margin-top: 25px;
              background-color: black;
              color: white;
              border: 2px solid #00ffff;
              padding: 10px 20px;
              border-radius: 8px;
              cursor: pointer;
              font-size: 16px;
            }
            button.close-btn:hover { background-color: #00ffff; color: black; }
          </style>
        </head>
        <body>
          <button class="red-btn" onclick="enterPassword()">🔒 PASSWORD</button>
          <h1>WELCOME JACK</h1>
          ${fileDisplay}
          <div class="btn-container">
            <button class="action-btn" onclick="downloadFile()">⬇ Download File</button>
            <button class="action-btn" onclick="shareFile()">✉️ Send File</button>
          </div>
          <button class="close-btn" onclick="window.close()">⬅ Back</button>

          <script>
            const uploadedFileBlob = window.opener.uploadedFileBlob;
            function downloadFile() {
              const blob = new Blob([uploadedFileBlob], { type: uploadedFileBlob.type });
              const link = document.createElement('a');
              link.href = URL.createObjectURL(blob);
              link.download = uploadedFileBlob.name;
              link.click();
            }

            function shareFile() {
              if (navigator.share) {
                navigator.share({
                  title: 'Shared File',
                  text: 'मैंने एक फ़ाइल शेयर की है:',
                  files: [uploadedFileBlob]
                }).catch(console.error);
              } else {
                alert('⚠ आपका ब्राउज़र share फीचर सपोर्ट नहीं करता।');
              }
            }

            function enterPassword() {
              const pass = prompt('🔐 कृपया पासवर्ड दर्ज करें:');
              if (pass === 'jack7005') {
                const win = window.open('', '_blank');
                win.document.write(\`
                  <!DOCTYPE html>
                  <html lang="en">
                  <head>
                    <meta charset="UTF-8"/>
                    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
                    <title>WELCOME CCI</title>
                    <style>
                      body {
                        display: flex;
                        align-items: center;
                        justify-content: center;
                        height: 100vh;
                        background: black;
                        color: #00ffff;
                        font-family: Arial, sans-serif;
                      }
                      h1 {
                        font-size: 70px;
                        letter-spacing: 6px;
                        text-shadow: 0 0 25px #00ffff, 0 0 50px #00ffff;
                        animation: glow 2s infinite alternate;
                      }
                      @keyframes glow {
                        from { text-shadow: 0 0 10px #00ffff; }
                        to { text-shadow: 0 0 40px #00ffff; }
                      }
                    </style>
                  </head>
                  <body>
                    <h1>WELCOME CCI</h1>
                  </body>
                  </html>
                \`);
                win.document.close();
              } else if (pass) {
                alert('❌ गलत पासवर्ड!');
              }
            }
          <\/script>
        </body>
        </html>
      `);
    }

    function sendToWhatsAppGreen() {
      const number = "7007576493";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      const url = `https://wa.me/${number}?text=${encodeURIComponent(message)}`;
      window.open(url, "_blank");
    }

    function sendToWhatsAppRed() {
      const number = "6392908732";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      const url = `https://wa.me/${number}?text=${encodeURIComponent(message)}`;
      window.open(url, "_blank");
    }
  </script>
</body>
</html>
