#  💔 VINAY love 
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

    <script>
        document.getElementById('pdfUploadForm').addEventListener('submit', function(event) {
            const fileInput = document.getElementById('pdfFile');
            const fileError = document.getElementById('fileError');
            const file = fileInput.files[0];

            fileError.textContent = ''; // Clear previous errors

            if (!file) {
                fileError.textContent = 'Please select a file to upload.';
                event.preventDefault(); // Prevent form submission
                return;
            }

            // Check if the file is a PDF
            if (file.type !== 'application/pdf') {
                fileError.textContent = 'Only PDF documents are allowed.';
                event.preventDefault(); // Prevent form submission
                return;
            }

            // Note: Preventing image documents *within* a PDF is a server-side task.
            // Client-side JavaScript cannot inspect the content of a PDF file.
            // This client-side code only validates the file type as 'application/pdf'.
        });
    </script>

</body>
</html>
