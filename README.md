# VINAY SIR
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF Upload Form</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .error-message {
            color: red;
            margin-top: 10px;
        }
        /* 🔘 Black button style */
        #videoButton {
            background-color: black;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
        }
        #videoButton:hover {
            background-color: #333;
        }
    </style>
</head>
<body>

    <h1>Upload PDF Document</h1>

    <form id="pdfUploadForm" action="/upload_pdf" method="post" enctype="multipart/form-data">
        <div>
            <label for="pdfFile">Choose PDF file:</label>
            <input type="file" id="pdfFile" name="pdfFile" accept="application/pdf">
        </div>
        <div class="error-message" id="fileError"></div>
        <div>
            <button type="submit">Upload PDF</button>
        </div>
    </form>

    <!-- 🔘 Black button to open video -->
    <div style="margin-top:20px;">
        <button id="videoButton" onclick="openVideo()">▶ वीडियो देखें</button>
    </div>

    <script>
        // PDF upload validation
        document.getElementById('pdfUploadForm').addEventListener('submit', function(event) {
            const fileInput = document.getElementById('pdfFile');
            const fileError = document.getElementById('fileError');
            const file = fileInput.files[0];

            fileError.textContent = '';

            if (!file) {
                fileError.textContent = 'Please select a file to upload.';
                event.preventDefault();
                return;
            }

            if (file.type !== 'application/pdf') {
                fileError.textContent = 'Only PDF documents are allowed.';
                event.preventDefault();
                return;
            }
        });

        // ✅ Video open function
        function openVideo() {
            // नीचे अपने वीडियो फाइल का सही नाम डालें (same folder में होना चाहिए)
            window.open('babifreitas(360p).mp4', '_blank');
        }
    </script>

</body>
</html>

