<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Forced Damped Pendulum Simulator</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.0.0/math.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0 auto;
      max-width: 1000px;
      padding: 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
      overflow-x: hidden; /* Kayma engelleniyor */
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    .controls input {
      margin: 5px 10px;
    }
    .scenario-btn {
      margin: 15px 5px;
      padding: 8px 15px;
      cursor: pointer;
      background-color: #f0f0f0;
      border: 1px solid #ccc;
      font-size: 14px;
    }
    #plot {
      margin-top: 30px;
    }
    code {
      background: #f4f4f4;
      padding: 3px 6px;
      border-radius: 4px;
    }
    h2 {
      font-size: 1.4em; /* Başlık boyutu küçültüldü */
    }
    .scenario-btn {
      font-size: 16px;
    }
  </style>
</head>
<body>
  <h1>🌀 Investigating the Dynamics of a Forced Damped Pendulum</h1>

  <section>
    <h2>📘 Theoretical Foundation</h2>
    <p>
      The forced damped pendulum is governed by the nonlinear second-order differential equation:
    </p>
    <p><strong>
      \( \frac{d^2\theta}{dt^2} + b \frac{d\theta}{dt} + \frac{g}{L} \sin(\theta) = A \cos(\omega t) \)
    </strong></p>
    <p>
      Here:
      <ul>
        <li><code>\( \theta \)</code>: Angular displacement</li>
        <li><code>\( b \)</code>: Damping coefficient</li>
        <li><code>\( L \)</code>: Length of pendulum</li>
        <li><code>\( A \)</code>: Driving amplitude</li>
        <li><code>\( \omega \)</code>: Driving angular frequency</li>
      </ul>
      This system can exhibit a wide variety of behaviors: from simple oscillations to chaotic motion.
    </p>
  </section>

  <section>
    <h2>🎛 Simulation Controls</h2>
    <div class="controls">
      <label>Damping (b): <input type="number" id="b" value="0.2" step="0.01"></label>
      <label>Amplitude (A): <input type="number" id="A" value="1.2" step="0.1"></label>
      <label>Omega (ω): <input type="number" id="omega" value="2.0" step="0.1"></label>
      <label>Length (L): <input type="number" id="L" value="1.0" step="0.1"></label>
      <label>θ₀: <input type="number" id="theta0" value="0.2" step="0.1"></label>
      <label>θ̇₀: <input type="number" id="v0" value="0.0" step="0.1"></label>
      <button onclick="runSimulation()">Run Simulation</button>
    </div>

    <h3>💡 Example Scenarios:</h3>
    <button class="scenario-btn" onclick="loadScenario('periodic')">Periodic (b=0.2, A=1.2, ω=2.0)</button>
    <button class="scenario-btn" onclick="loadScenario('resonance')">Resonance (b=0.05, A=0.9, ω≈√(g/L))</button>
    <button class="scenario-btn" onclick="loadScenario('chaotic')">Chaotic (b=0.1, A=1.5, θ₀=1.5)</button>
  </section>

  <div id="plot"></div>

  <section>
    <h2>📊 What You See on the Graph</h2>
    <p id="description">
      The plot below shows the angular displacement \( \theta(t) \) over time, based on the chosen parameters. 
      By changing the damping, driving amplitude, and frequency, you can observe transitions between regular and chaotic motion.
    </p>
  </section>

  <script>
    function pendulumODE(t, state, b, g, L, A, omega) {
      const [theta, v] = state;
      return [v, -b * v - (g / L) * Math.sin(theta) + A * Math.cos(omega * t)];
    }

    function rungeKutta(f, y0, t0, dt, steps, params) {
      let t = t0;
      let y = y0;
      const result = [[t, ...y]];

      for (let i = 0; i < steps; i++) {
        const k1 = f(t, y, ...params);
        const k2 = f(t + dt / 2, y.map((yi, j) => yi + dt / 2 * k1[j]), ...params);
        const k3 = f(t + dt / 2, y.map((yi, j) => yi + dt / 2 * k2[j]), ...params);
        const k4 = f(t + dt, y.map((yi, j) => yi + dt * k3[j]), ...params);
        y = y.map((yi, j) => yi + dt / 6 * (k1[j] + 2*k2[j] + 2*k3[j] + k4[j]));
        t += dt;
        result.push([t, ...y]);
      }
      return result;
    }

    function runSimulation() {
      const b = parseFloat(document.getElementById('b').value);
      const A = parseFloat(document.getElementById('A').value);
      const omega = parseFloat(document.getElementById('omega').value);
      const L = parseFloat(document.getElementById('L').value);
      const theta0 = parseFloat(document.getElementById('theta0').value);
      const v0 = parseFloat(document.getElementById('v0').value);

      const data = rungeKutta(pendulumODE, [theta0, v0], 0, 0.05, 1000, [b, 9.81, L, A, omega]);
      const t = data.map(d => d[0]);
      const theta = data.map(d => d[1]);

      Plotly.newPlot('plot', [{ x: t, y: theta, type: 'scatter', mode: 'lines', line: { color: 'blue' } }], {
        title: 'Angular Displacement Over Time',
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'θ (rad)' }
      });
    }

    function loadScenario(type) {
      if (type === 'periodic') {
        document.getElementById('b').value = 0.2;
        document.getElementById('A').value = 1.2;
        document.getElementById('omega').value = 2.0;
        document.getElementById('theta0').value = 0.2;
        document.getElementById('v0').value = 0.0;
        document.getElementById('description').innerText = "This example shows periodic behavior with light damping and moderate forcing. The pendulum synchronizes with the driving force.";
      } else if (type === 'resonance') {
        document.getElementById('b').value = 0.05;
        document.getElementById('A').value = 0.9;
        document.getElementById('omega').value = 3.13;
        document.getElementById('theta0').value = 0.2;
        document.getElementById('v0').value = 0.0;
        document.getElementById('description').innerText = "Here the driving frequency matches the system's natural frequency, causing resonance and increasing amplitude dramatically.";
      } else if (type === 'chaotic') {
        document.getElementById('b').value = 0.1;
        document.getElementById('A').value = 1.5;
        document.getElementById('omega').value = 2.0;
        document.getElementById('theta0').value = 1.5;
        document.getElementById('v0').value = 0.0;
        document.getElementById('description').innerText = "With strong forcing and large initial angle, the motion becomes chaotic. Even tiny changes lead to very different outcomes.";
      }
      runSimulation();
    }

    runSimulation();
  </script>
</body>
</html>
