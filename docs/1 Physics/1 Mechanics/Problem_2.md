<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Forced Damped Pendulum Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async
          src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
  </script>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 960px;
      margin: 20px auto;
      padding: 20px;
      background-color: #fefefe;
      line-height: 1.6;
      color: #333;
    }
    h1, h2 {
      color: #2c3e50;
    }
    input[type="range"], input[type="number"] {
      width: 150px;
      margin-right: 10px;
    }
    label {
      font-weight: bold;
      margin-right: 5px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      overflow-x: auto;
    }
    #plot {
      margin-top: 30px;
    }
  </style>
</head>
<body>

  <h1>🌀 Forced Damped Pendulum Simulation</h1>

  <h2>📘 Theoretical Model</h2>
  <p>The motion is governed by the nonlinear second-order differential equation:</p>
  <p>
    $$
    \frac{d^2\theta}{dt^2} + b\frac{d\theta}{dt} + \frac{g}{L}\sin(\theta) = A\cos(\omega t)
    $$
  </p>
  <p>This equation models a pendulum subject to gravity, damping, and an external periodic force.</p>

  <h2>⚙️ Simulation Controls</h2>
  <div>
    <label>Damping (b):</label>
    <input type="number" id="b" value="0.5" step="0.1" oninput="simulate()" /><br><br>

    <label>Amplitude (A):</label>
    <input type="number" id="A" value="1.2" step="0.1" oninput="simulate()" /><br><br>

    <label>Frequency (ω):</label>
    <input type="number" id="omega" value="2.0" step="0.1" oninput="simulate()" /><br><br>

    <label>Length (L):</label>
    <input type="number" id="L" value="1.0" step="0.1" oninput="simulate()" /><br><br>
  </div>

  <div id="plot"></div>

  <h2>🧪 Method: Euler Integration</h2>
  <p>
    We approximate the motion using the Euler method by discretizing time and updating both angle \( \theta \) and angular velocity \( \omega \) step-by-step:
  </p>
  <pre><code>
theta' = omega
omega' = -b * omega - (g/L) * sin(theta) + A * cos(w * t)

theta += dt * omega
omega += dt * omega'
  </code></pre>

  <script>
    function simulate() {
      const b = parseFloat(document.getElementById("b").value);
      const A = parseFloat(document.getElementById("A").value);
      const omega_force = parseFloat(document.getElementById("omega").value);
      const L = parseFloat(document.getElementById("L").value);
      const g = 9.81;

      const dt = 0.02;
      const T = 20; // total simulation time
      const steps = Math.floor(T / dt);

      let theta = 0.2; // initial angle
      let omega = 0;   // initial angular velocity
      let t = 0;

      const time = [];
      const angle = [];

      for (let i = 0; i < steps; i++) {
        // Save current values
        time.push(t);
        angle.push(theta);

        // Calculate derivatives
        let domega = -b * omega - (g / L) * Math.sin(theta) + A * Math.cos(omega_force * t);

        // Euler update
        theta += dt * omega;
        omega += dt * domega;
        t += dt;
      }

      const trace = {
        x: time,
        y: angle,
        type: 'scatter',
        mode: 'lines',
        line: { color: 'tomato', width: 2 },
        name: 'θ(t)'
      };

      const layout = {
        title: 'Pendulum Angle θ(t) Over Time',
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'Angle θ (rad)' }
      };

      Plotly.newPlot('plot', [trace], layout);
    }

    simulate();
  </script>

</body>
</html>
