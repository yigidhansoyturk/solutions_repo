<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>🌊 Interference Patterns on a Water Surface</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 900px;
      background-color: #f9f9f9;
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
    input, select {
      margin-bottom: 10px;
    }
    pre {
      background-color: #eee;
      padding: 10px;
      overflow-x: auto;
    }
  </style>
</head>
<body>

  <h1>🌊 Interference Patterns on a Water Surface</h1>

  <h2>📖 What Is This Simulation About?</h2>

  <p>
    When multiple circular water waves from different sources overlap, they create interference patterns.
    These patterns arise due to the <strong>principle of superposition</strong>: the total wave at any point is
    the sum of all the individual waves reaching it. Depending on how these waves align in space and time, they can
    either reinforce each other (constructive interference) or cancel each other out (destructive interference).
  </p>

  <p>
    In this simulation, <strong>N point wave sources</strong> are placed symmetrically around a circle (the vertices
    of a regular polygon). Each source emits a wave described by:
  </p>

  <pre><code>η(x, y, t) = (A / √r) * cos(k * r - ω * t + φ)</code></pre>

  <ul>
    <li><strong>A</strong>: Wave amplitude (controllable)</li>
    <li><strong>r</strong>: Distance from the source to the point (x, y)</li>
    <li><strong>k</strong>: Wave number, related to wavelength</li>
    <li><strong>ω</strong>: Angular frequency, determines wave speed</li>
    <li><strong>φ</strong>: Initial phase (set to zero here)</li>
  </ul>

  <p>
    The result is the sum of all these waves at every point on a 2D grid. We visualize this as a dynamic
    <strong>contour plot</strong>, which shows high and low points of the water surface over time. Bright areas
    show constructive interference; dark areas show destructive cancellation.
  </p>

  <h3>🔍 Why Is This Interesting?</h3>
  <ul>
    <li>Models real-world physics like ripples in a pond, sound wave interactions, and antenna radiation.</li>
    <li>Reveals beautiful and sometimes surprising symmetric patterns depending on the number of sources.</li>
    <li>Lets you experiment interactively to build intuition about wave phenomena.</li>
  </ul>

  <h2>🛠️ Simulation Settings</h2>
  <label for="numSources">Number of Wave Sources:</label>
  <input type="range" id="numSources" min="2" max="12" step="1" value="6" oninput="updatePlot()" />
  <span id="sourceValue">6</span><br>

  <label for="amplitude">Wave Amplitude:</label>
  <input type="range" id="amplitude" min="0.5" max="2" step="0.1" value="1" oninput="updatePlot()" />
  <span id="amplitudeValue">1.0</span><br>

  <div id="plot"></div>

  <script>
    function updatePlot() {
      const N = parseInt(document.getElementById("numSources").value);
      const A = parseFloat(document.getElementById("amplitude").value);
      document.getElementById("sourceValue").textContent = N;
      document.getElementById("amplitudeValue").textContent = A.toFixed(1);

      const size = 100;
      const resolution = 100;
      const λ = 10;
      const k = 2 * Math.PI / λ;
      const f = 1;
      const ω = 2 * Math.PI * f;
      const t = 0;

      const x = Array.from({length: resolution}, (_, i) => -size/2 + i * size / resolution);
      const y = Array.from({length: resolution}, (_, j) => -size/2 + j * size / resolution);
      const z = [];

      const R = 20; // radius of the polygon
      const sources = [];
      for (let i = 0; i < N; i++) {
        const angle = 2 * Math.PI *
