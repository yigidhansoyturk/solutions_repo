<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Estimating π Using Monte Carlo Methods</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f8f8f8;
      padding: 20px;
      color: #222;
    }
    h1, h2 {
      color: #003366;
    }
    canvas {
      border: 1px solid #ccc;
      margin: 10px 0;
    }
    section {
      margin-bottom: 40px;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
    }
    .controls {
      margin-top: 10px;
    }
    label {
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>Estimating π Using Monte Carlo Methods</h1>

  <section>
    <h2>1. Circle-Based Simulation</h2>
    <p>This method uses the ratio of points falling inside a circle to estimate π. The formula is:</p>
    <p>\[\pi \approx 4 \cdot \frac{\text{points inside circle}}{\text{total points}}\]</p>

    <div class="controls">
      <label for="circlePoints">Number of Points:</label>
      <input type="range" id="circlePoints" min="100" max="10000" step="100" value="1000">
      <span id="circleCount">1000</span>
      <button onclick="simulateCircle()">Simulate</button>
    </div>
    <p id="circleResult"><strong>Estimated π: —</strong></p>
    <canvas id="circleCanvas" width="400" height="400"></canvas>
  </section>

  <section>
    <h2>2. Buffon's Needle Simulation</h2>
    <p>This method estimates π based on the probability of a needle crossing parallel lines:</p>
    <p>\[\pi \approx \frac{2 \cdot L \cdot N}{d \cdot C}\]</p>

    <div class="controls">
      <label for="needleThrows">Number of Throws:</label>
      <input type="range" id="needleThrows" min="100" max="10000" step="100" value="1000">
      <span id="needleCount">1000</span><br>
      <label>Needle Length \( L = \)</label> <input type="number" id="needleLength" value="50">
      <label>Line Spacing \( d = \)</label> <input type="number" id="lineSpacing" value="100">
      <button onclick="simulateNeedle()">Simulate</button>
    </div>
    <p id="needleResult"><strong>Estimated π: —</strong></p>
    <canvas id="needleCanvas" width="400" height="400"></canvas>
  </section>

  <script>
    // Circle simulation
    const circleCanvas = document.getElementById("circleCanvas");
    const ctx1 = circleCanvas.getContext("2d");

    document.getElementById("circlePoints").oninput = function () {
      document.getElementById("circleCount").textContent = this.value;
    };

    function simulateCircle() {
      const points = +document.getElementById("circlePoints").value;
      let inside = 0;
      ctx1.clearRect(0, 0, 400, 400);
      ctx1.beginPath();
      ctx1.arc(200, 200, 200, 0, 2 * Math.PI);
      ctx1.stroke();

      for (let i = 0; i < points; i++) {
        const x = Math.random();
        const y = Math.random();
        const dx = x - 0.5, dy = y - 0.5;
        const d2 = dx * dx + dy * dy;
        const cx = x * 400, cy = y * 400;
        if (d2 <= 0.25) {
          inside++;
          ctx1.fillStyle = "blue";
        } else {
          ctx1.fillStyle = "red";
        }
        ctx1.fillRect(cx, cy, 1.5, 1.5);
      }
      const pi = 4 * inside / points;
      document.getElementById("circleResult").innerHTML = `<strong>Estimated π: ${pi.toFixed(5)}</strong>`;
    }

    // Buffon's Needle simulation
    const needleCanvas = document.getElementById("needleCanvas");
    const ctx2 = needleCanvas.getContext("2d");

    document.getElementById("needleThrows").oninput = function () {
      document.getElementById("needleCount").textContent = this.value;
    };

    function simulateNeedle() {
      const throws = +document.getElementById("needleThrows").value;
      const L = +document.getElementById("needleLength").value;
      const d = +document.getElementById("lineSpacing").value;
      let crosses = 0;
      ctx2.clearRect(0, 0, 400, 400);

      // draw lines
      ctx2.strokeStyle = "#999";
      for (let x = d; x < 400; x += d) {
        ctx2.beginPath();
        ctx2.moveTo(x, 0);
        ctx2.lineTo(x, 400);
        ctx2.stroke();
      }

      ctx2.strokeStyle = "#000";
      for (let i = 0; i < throws; i++) {
        const x = Math.random() * 400;
        const y = Math.random() * 400;
        const angle = Math.random() * Math.PI;
        const x2 = x + (L / 2) * Math.cos(angle);
        const x1 = x - (L / 2) * Math.cos(angle);
        const y2 = y + (L / 2) * Math.sin(angle);
        const y1 = y - (L / 2) * Math.sin(angle);

        // draw needle
        ctx2.beginPath();
        ctx2.moveTo(x1, y1);
        ctx2.lineTo(x2, y2);
        ctx2.stroke();

        // check if it crosses any vertical line
        const xMin = Math.min(x1, x2);
        const xMax = Math.max(x1, x2);
        for (let lineX = d; lineX < 400; lineX += d) {
          if (xMin < lineX && xMax > lineX) {
            crosses++;
            break;
          }
        }
      }

      let pi = '—';
      if (crosses > 0) {
        pi = (2 * L * throws) / (d * crosses);
        pi = pi.toFixed(5);
      }

      document.getElementById("needleResult").innerHTML =
        `<strong>Estimated π: ${pi} (${crosses} crossings)</strong>`;
    }

    // Run both initially
    simulateCircle();
    simulateNeedle();
  </script>
</body>
</html>
