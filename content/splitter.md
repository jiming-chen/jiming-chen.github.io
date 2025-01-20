---
title: 'Splitter'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
{{< rawhtml >}}
  <h1>Instagram Image Splitter</h1>
  <input type="file" id="imageInput" accept="image/*">
  <div id="canvas-container"></div>
  <button id="saveAllBtn" style="display: none; margin: 20px 0;">Save All Images</button>

  <script>
      document.getElementById('imageInput').addEventListener('change', function(event) {
          const file = event.target.files[0];
          if (file) {
              const img = new Image();
              img.onload = function() {
                  processImage(img);
              };
              img.src = URL.createObjectURL(file);
          }
      });

      function processImage(img) {
          const canvasContainer = document.getElementById('canvas-container');
          canvasContainer.innerHTML = '';
          const originalWidth = img.width;
          const originalHeight = img.height;
          const squareSize = Math.min(originalWidth, originalHeight / Math.ceil(originalHeight / originalWidth));
          
          let y = 0;
          let partNumber = 1;
          
          const images = [];
          while (y + squareSize <= originalHeight) {
              const canvas = document.createElement('canvas');
              canvas.width = squareSize;
              canvas.height = squareSize;
              const ctx = canvas.getContext('2d');
              ctx.drawImage(img, 0, y, squareSize, squareSize, 0, 0, squareSize, squareSize);
              
              const link = document.createElement('a');
              link.href = canvas.toDataURL("image/png");
              link.download = `split_part_${partNumber}.png`;
              link.innerHTML = `<br>Download Part ${partNumber}`;
              
              canvasContainer.appendChild(canvas);
              canvasContainer.appendChild(link);
              images.push({ url: link.href, filename: link.download });
              
              y += squareSize;
              partNumber++;
          }

          // Show and setup save all button
          const saveAllBtn = document.getElementById('saveAllBtn');
          saveAllBtn.style.display = 'block';
          saveAllBtn.onclick = () => {
              images.forEach(img => {
                  const link = document.createElement('a');
                  link.href = img.url;
                  link.download = img.filename;
                  link.click();
              });
          };
      }
  </script>
{{< /rawhtml >}}