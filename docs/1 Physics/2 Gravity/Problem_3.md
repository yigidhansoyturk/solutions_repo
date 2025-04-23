<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>🌊 Wave Interference Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      margin: 0;
      padding: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: start;
      background-color: #f9f9f9;
      font-family: Arial, sans-serif;
      color: #333;
    }
    .container {
      margin: 40px 0;
      width: 95%;
      max-width: 960px;
      padding: 20px;
      background-color: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }
    h1, h2, h3 {
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
      margin: 10px 5px 20px 0;
      padding: 6px 12px;
      font-size: 14px;
      border: none;
      background-color: #3498db;
      color: white;
      border-radius: 6px;
      cursor: pointer;
    }
    button:hover {
      background-color: #2980b9;
    }
    pre {
      background-color: #eee;
      padding: 10px;
      overflow-x: auto;
      border-radius: 6px;
    }
    .controls {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 12px;
      margin-top: 10px;
    }
    .controls label {
      margin-right: 6px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>🌊 Interference Patterns from Multiple Wave Sources</h1>

    <h2>📖 What’s Going On Here?</h2>
    <p>
      Multiple circular wave sources on a flat surface create fascinating interference patterns. When these waves overlap,
      they interact based on the <strong>superposition principle</strong> — adding their amplitudes at every point in space.
    </p>

    <pre><code>η(x, y, t) = (A / √r) * cos(k * r - ω * t)</code></pre>

    <p>
      This formula means waves decrease in amplitude as they travel (1/√r), oscillate with distance and time,
      and interact with each other. You'll see areas of:
    </p>
    <ul>
      <li><strong>Constructive interference</strong> (bright spots)</li>
      <li><strong>Destructive interference</strong> (dark spots)</li>
    </ul>

    <h2>🛠️ Controls</h2>
    <div class="controls">
      <label for="numSources">Number of Sources:</label>
      <input type="range" id="numSources" min="2" max="12" value="6" step="1" oninput="updateSettings()">
      <span id="numSourcesValue">6</span>

      <label for="amplitude">Amplitude:</label>
      <input type="range" id="amplitude" min="0.5" max="2.0" value="1.0" step="0.1" oninput="updateSettings()">
      <span id="amplitudeValue">1.0</span>

      <button onclick="toggleAnimation()">⏯️ Pause/Resume</button>
      <button onclick="exportImage()">📸 Export as PNG</button>
    </div>

    <div id="plot"></div>
  </div>

  <script>
    const size = 100;
    const res = 100;
    const λ = 10;
    const k = 2 * Math.PI / λ;
    const f = 1;
    const ω = 2 * Math.PI * f;
    const x = Array.from({ length: res }, (_, i) => -size / 2 + i * size / res);
    const y = Array.from({ length: res }, (_, j) => -size / 2 + j * size / res);
    let t = 0;
    let interval = null;
    let isPaused = false;

    function generateSources(n) {
      const R = 20;
      const sources = [];
      for (let i = 0; i < n; i++) {
        const θ = 2 * Math.PI * i / n;
        sources.push([R * Math.cos(θ), R * Math.sin(θ)]);
      }
      return sources;
    }

    function calculateFrame(A, sources, t) {
      const z = [];
      for (let j = 0; j < y.length; j++) {
        const row = [];
        for (let i = 0; i < x.length; i++) {
          let sum = 0;
          for (const [sx, sy] of sources) {
            const dx = x[i] - sx;
            const dy = y[j] - sy;
            const r = Math.sqrt(dx * dx + dy * dy) + 0.001;
            sum += (A / Math.sqrt(r)) * Math.cos(k * r - ω * t);
          }
          row.push(sum);
        }
        z.push(row);
      }
      return z;
    }

    function updateSettings() {
      const N = parseInt(document.getElementById("numSources").value);
      const A = parseFloat(document.getElementById("amplitude").value);
      document.getElementById("numSourcesValue").textContent = N;
      document.getElementById("amplitudeValue").textContent = A.toFixed(1);
      startAnimation(N, A);
    }

    function startAnimation(N, A) {
      const sources = generateSources(N);
      if (interval) clearInterval(interval);
      interval = setInterval(() => {
        if (isPaused) return;
        const z = calculateFrame(A, sources, t);
        Plotly.newPlot('plot', [{
          z: z,
          x: x,
          y: y,
          type: 'contour',
          contours: { coloring: 'heatmap' },
          colorbar: { title: 'Amplitude' }
        }], {
          title: `Wave Interference with ${N} Sources`,
          xaxis: { title: 'X', scaleanchor: 'y' },
          yaxis: { title: 'Y' }
        });
        t += 0.1;
      }, 100);
    }

    function toggleAnimation() {
      isPaused = !isPaused;
    }

    function exportImage() {
      Plotly.toImage(document.getElementById('plot'), { format: 'png', height: 600, width: 700 })
        .then(dataUrl => {
          const a = document.createElement('a');
          a.href = dataUrl;
          a.download = 'wave_interference.png';
          document.body.appendChild(a);
          a.click();
          document.body.removeChild(a);
        });
    }

    updateSettings();
  </script>

</body>
</html>
