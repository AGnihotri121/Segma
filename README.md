<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>File Converter</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        .container { max-width: 500px; margin: auto; padding: 20px; border: 1px solid #ddd; border-radius: 10px; }
        input, button { margin: 10px; padding: 10px; width: 100%; }
    </style>
</head>
<body>
    <h2>Simple File Converter</h2>
    <div class="container">
        <h3>JPG to PNG</h3>
        <input type="file" id="jpgInput" accept="image/jpeg">
        <button onclick="convertImage()">Convert to PNG</button>
        
        <h3>PDF to Excel</h3>
        <input type="file" id="pdfExcelInput" accept="application/pdf">
        <button onclick="convertPDF('xls')">Convert to Excel</button>
        
        <h3>PDF to PowerPoint</h3>
        <input type="file" id="pdfPptInput" accept="application/pdf">
        <button onclick="convertPDF('pptx')">Convert to PowerPoint</button>
    </div>
    
    <script>
        const API_KEY = 'YOUR_CLOUDCONVERT_API_KEY';
        
        function convertImage() {
            let file = document.getElementById('jpgInput').files[0];
            if (!file) return alert('Please select a JPG file.');
            let link = document.createElement('a');
            link.href = URL.createObjectURL(file);
            link.download = file.name.replace('.jpg', '.png');
            link.click();
        }

        async function convertPDF(format) {
            let fileInput = format === 'xls' ? 'pdfExcelInput' : 'pdfPptInput';
            let file = document.getElementById(fileInput).files[0];
            if (!file) return alert('Please select a PDF file.');

            let formData = new FormData();
            formData.append('file', file);
            formData.append('apikey', API_KEY);
            formData.append('input', 'upload');
            formData.append('outputformat', format);

            let response = await fetch('https://api.cloudconvert.com/v2/convert', {
                method: 'POST',
                body: formData
            });

            let result = await response.json();
            if (result.data && result.data.url) {
                window.location.href = result.data.url;
            } else {
                alert('Conversion failed!');
            }
        }
    </script>
</body>
</html>
