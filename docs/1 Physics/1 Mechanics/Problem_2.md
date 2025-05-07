<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Forced Damped Pendulum Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 900px;
      padding: 0 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2 {
      color: #2c3e50;
    }
    input[type="range"], input[type="number"] {
      width: 200px;
      margin: 5px 0;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 10px;
    }
    #plot {
      margin-top: 30px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      overflow-x: auto;
    }
  </style>
</head>
<body>

  <h1>🌀 Forced Damped Pendulum Simulation</h1>

  <section>
    <h2>📘 Theoretical Foundation</h2>
    <p>The motion is governed by the nonlinear second-order differential equation:</p>
    <pre>
d²θ/dt² + b·dθ/dt + (g/L)·sin(θ) = A·cos(ωt)
    </pre>
    <p>Where:</p>
    <ul>
      <li><strong>θ(t)</strong> is the angular displacement</li>
      <li><strong>b</strong> is the damping coefficient</li>
      <li><strong>g</strong> is gravitational acceleration</li>
      <li><strong>L</strong> is pendulum length</li>
      <li><strong>A</strong> is the forcing amplitude</li>
      <li><strong>ω</strong> is the forcing frequency</li>
    </ul>
  </section>

  <section>
    <h2>🎛️ Simulation Controls</h2>
    <label>Damping Coefficient (b): <input type="number" id="b" value="0.5" step="0.1"></label>
    <label>Amplitude (A): <input type="number" id="A" value="1.2" step="0.1"></label>
    <label>Frequency (ω): <input type="number" id="omega" value="2/3" step="0.01"></label>
    <label>Pendulum Length (L): <input type="number" id="L" value="9.8" step="0.1"></label>
    <button onclick="simulate()">Run Simulation</button>
  </section>

  <section id="plot"></section>

  <script>
    function simulate() {
      const b = parseFloat(document.getElementById('b').value);
      const A = parseFloat(document.getElementById('A').value);
      const omega = parseFloat(document.getElementById('omega').value);
      const L = parseFloat(document.getElementById('L').value);
      const g = 9.8;

      let theta = 0.2;  // initial angle
      let omega_theta = 0; // initial angular velocity
      const dt = 0.04;
      const time = [], angles = [];

      for (let t = 0; t < 100; t += dt) {
        const domega_dt = -b * omega_theta - (g / L) * Math.sin(theta) + A * Math.cos(omega * t);
        omega_theta += domega_dt * dt;
        theta += omega_theta * dt;

        time.push(t);
        angles.push(theta);
      }

      const trace = {
        x: time,
        y: angles,
        type: 'scatter',
        mode: 'lines',
        line: { color: 'darkred' },
        name: 'θ(t)'
      };

      const layout = {
        title: 'Forced Damped Pendulum Motion',
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'Angle θ (rad)' }
      };

      Plotly.newPlot('plot', [trace], layout);
    }

    simulate();
  </script>

</body>
</html>
