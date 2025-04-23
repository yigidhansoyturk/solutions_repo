<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Water Surface Interference Patterns</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 1000px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2 {
      color: #2c3e50;
    }
    #plot {
      margin-top: 30px;
    }
    label {
      font-weight: bold;
    }
    input {
      margin-bottom: 10px;
    }
    button {
      margin-top: 10px;
      padding: 6px 12px;
      font-weight: bold;
      background: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #2980b9;
    }
  </style>
</head>
<body>

  <h1>🌊 Animated Water Surface Interference Patterns</h1>

  <h2>⚙️ Controls</h2>
  <label for="numSources">Number of Sources (Polygon Vertices):</label>
  <input type="range" id="numSources" min="2" max="8" value="4" oninput="updateParams()">
  <span id="numSourcesValue">4</span>

  <br>

  <label for="amplitude">Amplitude (A):</label>
  <input type="range" id="amplitude" min="0.5" max="5" step="0.1" value="1" oninput="updateParams()">
  <span id="amplitudeValue">1</span>

  <br>

  <button onclick="downloadImage()">📸 Download Image</button>
  <button onclick="downloadPDF()">📝 Export to PDF</button>

  <div id="plot"></div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <script>
    const gridSize = 100;
    const spacing = 10;
    const k = 1.0;
    const omega = 2.0;
    const phi = 0;
    let t = 0;
    let interval;

    const xRange = Array.from({ length: gridSize }, (_, i) => (i - gridSize / 2) * spacing);
    const yRange = Array.from({ length: gridSize }, (_, j) => (j - gridSize / 2) * spacing);

    function wave(x, y, sx, sy, amp) {
      const r = Math.sqrt((x - sx) ** 2 + (y - sy) ** 2);
      return amp * Math.cos(k * r - omega * t + phi) / Math.sqrt(r + 1e-3);
    }

    function generateSources(N, radius = 150) {
      return Array.from({ length: N }, (_, i) => {
        const angle = (2 * Math.PI * i) / N;
        return [radius * Math.cos(angle), radius * Math.sin(angle)];
      });
    }

    function plotPattern() {
      const N = parseInt(document.getElementById("numSources").value);
      const amp = parseFloat(document.getElementById("amplitude").value);
      const sources = generateSources(N);

      const z = yRange.map(y =>
        xRange.map(x =>
          sources.reduce((sum, [sx, sy]) => sum + wave(x, y, sx, sy, amp), 0)
        )
      );

      Plotly.react('plot', [{
        z: z,
        x: xRange,
        y: yRange,
        type: 'contour',
        colorscale: 'RdBu',
        contours: { coloring: 'heatmap' }
      }], {
        title: `Interference Pattern (N = ${N}, A = ${amp.toFixed(1)}, t = ${t.toFixed(2)})`,
        xaxis: { title: 'x (cm)' },
        yaxis: { title: 'y (cm)', scaleanchor: 'x' }
      });
    }

    function updateParams() {
      document.getElementById("numSourcesValue").textContent = document.getElementById("numSources").value;
      document.getElementById("amplitudeValue").textContent = document.getElementById("amplitude").value;
    }

    function animate() {
      interval = setInterval(() => {
        t += 0.2;
        plotPattern();
      }, 100);
    }

    function downloadImage() {
      Plotly.downloadImage('plot', {format: 'png', width: 800, height: 600, filename: 'interference_pattern'});
    }

    async function downloadPDF() {
      const { jsPDF } = window.jspdf;
      const pdf = new jsPDF();
      const imgData = await Plotly.toImage('plot', {format: 'png', width: 800, height: 600});
      pdf.text("Water Surface Interference Pattern", 10, 10);
      pdf.addImage(imgData, 'PNG', 10, 20, 180, 135);
      pdf.save('interference_pattern.pdf');
    }

    updateParams();
    animate();
  </script>

</body>
</html>
