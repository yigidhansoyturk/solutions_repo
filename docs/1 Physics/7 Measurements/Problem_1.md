<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Pendulum Gravity Estimation</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f4f4f4;
      padding: 20px;
      color: #222;
      transition: all 0.3s ease;
    }
    section {
      background: #fff;
      padding: 25px;
      border-radius: 10px;
      margin-bottom: 40px;
      box-shadow: 0 0 15px rgba(0,0,0,0.05);
    }
    h1, h2, h3 {
      color: #003366;
    }
    textarea {
      width: 100%;
      font-family: monospace;
      height: 70px;
    }
    label {
      display: inline-block;
      width: 220px;
      margin-bottom: 8px;
    }
    button {
      margin: 10px 10px 0 0;
      padding: 8px 14px;
      background-color: #003366;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #00509e;
    }

    .dark-mode {
      background: #121212;
      color: #eee;
    }
    .dark-mode section {
      background: #1e1e1e;
      box-shadow: none;
    }
    .dark-mode input,
    .dark-mode textarea {
      background: #333;
      color: #fff;
      border: 1px solid #666;
    }
    .dark-mode canvas {
      background: #fff;
    }
    .dark-mode button {
      background-color: #444;
      color: #fff;
    }
  </style>
</head>
<body>

  <button onclick="toggleDarkMode()">🌙 Toggle Dark Mode</button>

  <h1>Measuring Gravity with a Simple Pendulum</h1>

  <section>
    <h2>Overview</h2>
    <p>
      The acceleration due to gravity on Earth can be estimated using a simple pendulum — a mass hanging from a string of fixed length.
      When displaced and released, the pendulum swings in a regular arc. The period of these oscillations is related to gravity through:
    </p>
    <p>\[
      g = \frac{4\pi^2 L}{T^2}
    \]</p>
    <p>
      Here, \( L \) is the pendulum length, and \( T \) is the period of a single oscillation. Since timing a single swing can be error-prone,
      a better method is to measure the time for 10 full oscillations (denoted \( T_{10} \)), average multiple trials, and compute \( T = \frac{T_{10}}{10} \).
    </p>
    <p>
      By accounting for uncertainty in the length measurement and timing variability, we also estimate the error in \( g \):
    </p>
    <p>\[
      \Delta g = g \cdot \sqrt{\left( \frac{\Delta L}{L} \right)^2 + \left( \frac{2 \Delta T}{T} \right)^2 }
    \]</p>
  </section>

  <section>
    <h2>Enter Your Data</h2>
    <label>Pendulum Length \( L \) (m):</label>
    <input type="number" id="L" value="1.5" step="0.01"><br>
    <label>Ruler Resolution (m):</label>
    <input type="number" id="ruler" value="0.01" step="0.001"><br><br>

    <label>10 Measurements of \( T_{10} \) (seconds):</label><br>
    <textarea id="T10_input">15.32, 15.28, 15.36, 15.30, 15.33, 15.29, 15.31, 15.34, 15.27, 15.35</textarea><br>

    <button onclick="analyzePendulum()">Calculate</button>
    <button onclick="exportMarkdown()">Export as Markdown</button>
    <button onclick="exportCSV()">Export as CSV</button>

    <h3>Results:</h3>
    <div id="pendulumOutput" style="font-family: monospace; white-space: pre-line;"></div>
    <canvas id="pendulumChart" width="500" height="300"></canvas>
  </section>

  <script>
    let pendulumChartInstance = null;

    function toggleDarkMode() {
      document.body.classList.toggle("dark-mode");
    }

    function analyzePendulum() {
      const L = parseFloat(document.getElementById('L').value);
      const ruler = parseFloat(document.getElementById('ruler').value);
      const deltaL = ruler / 2;

      const values = document.getElementById('T10_input').value.split(',').map(v => parseFloat(v.trim())).filter(v => !isNaN(v));
      if (values.length !== 10) {
        alert("Please enter exactly 10 timing values.");
        return;
      }

      const meanT10 = values.reduce((a, b) => a + b, 0) / values.length;
      const sigma = Math.sqrt(values.reduce((sum, v) => sum + (v - meanT10) ** 2, 0) / (values.length - 1));
      const deltaT10 = sigma / Math.sqrt(values.length);

      const T = meanT10 / 10;
      const deltaT = deltaT10 / 10;
      const g = (4 * Math.PI ** 2 * L) / (T ** 2);
      const deltaG = g * Math.sqrt((deltaL / L) ** 2 + (2 * deltaT / T) ** 2);

      document.getElementById('pendulumOutput').innerText =
`Mean T₁₀: ${meanT10.toFixed(3)} s
Std Dev σ(T₁₀): ${sigma.toFixed(3)} s
ΔT₁₀: ${deltaT10.toFixed(3)} s
Period T: ${T.toFixed(3)} s
ΔT: ${deltaT.toFixed(3)} s

Estimated g: ${g.toFixed(3)} m/s²
Uncertainty Δg: ±${deltaG.toFixed(3)} m/s²`;

      if (pendulumChartInstance) {
        pendulumChartInstance.destroy();
      }

      pendulumChartInstance = new Chart(document.getElementById('pendulumChart'), {
        type: 'bar',
        data: {
          labels: values.map((_, i) => `Trial ${i + 1}`),
          datasets: [{
            label: 'T₁₀ (s)',
            data: values,
            backgroundColor: 'skyblue',
            borderColor: '#003366',
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: { beginAtZero: false }
          },
          plugins: {
            legend: { display: false },
            title: {
              display: true,
              text: 'Timing for 10 Oscillations'
            }
          }
        }
      });
    }

    function exportMarkdown() {
      const output = document.getElementById("pendulumOutput").innerText;
      const markdown = `### Pendulum Measurement Results\n\n${output}`;
      const blob = new Blob([markdown], { type: 'text/markdown' });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "pendulum_results.md";
      link.click();
    }

    function exportCSV() {
      const values = document.getElementById('T10_input').value.split(',').map(v => parseFloat(v.trim())).filter(v => !isNaN(v));
      const rows = [
        ["Trial", "T10 (s)"],
        ...values.map((val, i) => [i + 1, val]),
        [],
        ["Label", "Value"],
        ["Mean T10", document.getElementById('pendulumOutput').innerText.match(/Mean T₁₀: ([\d.]+)/)?.[1] || ""],
        ["Std Dev", document.getElementById('pendulumOutput').innerText.match(/σ\(T₁₀\): ([\d.]+)/)?.[1] || ""],
        ["Delta T10", document.getElementById('pendulumOutput').innerText.match(/ΔT₁₀: ([\d.]+)/)?.[1] || ""],
        ["Period T", document.getElementById('pendulumOutput').innerText.match(/Period T: ([\d.]+)/)?.[1] || ""],
        ["Delta T", document.getElementById('pendulumOutput').innerText.match(/ΔT: ([\d.]+)/)?.[1] || ""],
        ["g", document.getElementById('pendulumOutput').innerText.match(/Estimated g: ([\d.]+)/)?.[1] || ""],
        ["Delta g", document.getElementById('pendulumOutput').innerText.match(/±([\d.]+)/)?.[1] || ""]
      ];
      const csv = rows.map(row => row.join(",")).join("\n");
      const blob = new Blob([csv], { type: 'text/csv' });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "pendulum_results.csv";
      link.click();
    }
  </script>
</body>
</html>
