<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>🌊 Wave Interference Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 960px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
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
      margin: 10px 5px;
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
    }
  </style>
</head>
<body>

  <h1>🌊 Interference Patterns from Multiple Wave Sources</h1>

  <h2>🔍 Overview</h2>
  <p>
    Wave interference is a fundamental phenomenon that arises when two or more waves overlap in space.
    Their amplitudes combine according to the <strong>superposition principle</strong>, producing complex patterns of crests and troughs.
    This behavior is seen across physics: in water ripples, sound fields, light fringes, and quantum wavefunctions.
  </p>

  <p>
    In this simulation, we visualize circular waves emitted by multiple sources positioned around a ring.
    The waves interact at every point in space, with the net wave height at each location determined by the sum:
  </p>

  <p>
    \[
    \eta(x, y, t) = \frac{A}{\sqrt{r}} \cos(k r - \omega t)
    \]
  </p>

  <p>
    Where:
    <ul>
      <li><strong>A</strong>: amplitude of each source</li>
      <li><strong>r</strong>: distance from the wave source to the point \((x, y)\)</li>
      <li><strong>k</strong>: wave number, \( k = \frac{2\pi}{\lambda} \)</li>
      <li><strong>\omega</strong>: angular frequency, \( \omega = 2\pi f \)</li>
    </ul>
  </p>

  <p>
    The interference can be:
    <ul>
      <li><strong>Constructive</strong>: waves amplify when in phase</li>
      <li><strong>Destructive</strong>: waves cancel when out of phase</li>
    </ul>
  </p>

  <h3>🌍 Real-World Applications</h3>
  <ul>
    <li>Water wave studies and tank experiments</li>
    <li>Acoustic speaker design and noise cancellation</li>
    <li>Optics: Young's double-slit, holography, and diffraction</li>
    <li>Quantum probability amplitudes and tunneling</li>
  </ul>

  <h2>🛠️ Controls</h2>

  <label for="numSources">Number of Sources:</label>
  <input type="range" id="numSources" min="2" max="12" value="6" step="1" oninput="updateSettings()">
  <span id="numSourcesValue">6</span><br>

  <label for="amplitude">Amplitude:</label>
  <input type="range" id="amplitude" min="0.5" max="2.0" value="1.0" step="0.1" oninput="updateSettings()">
  <span id="amplitudeValue">1.0</span><br>

  <button onclick="toggleAnimation()">⏯️ Pause/Resume</button>
  <button onclick="exportImage()">📸 Export as PNG</button>

  <div id="plot"></div>

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
