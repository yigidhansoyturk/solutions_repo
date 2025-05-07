<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Forced Damped Pendulum Simulation</title>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 1000px;
      padding: 0 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    #controls label {
      font-weight: bold;
      margin-right: 10px;
    }
    #plot {
      margin-top: 20px;
    }
    input {
      margin-bottom: 10px;
    }
  </style>
</head>
<body>

  <h1>🌀 Investigating the Dynamics of a Forced Damped Pendulum</h1>

  <section>
    <h2>📘 Theoretical Foundation</h2>
    <p>
      The motion of a forced damped pendulum is described by the nonlinear second-order differential equation:
    </p>
    <p>
      \[
      \frac{d^2\theta}{dt^2} + b\frac{d\theta}{dt} + \frac{g}{L}\sin(\theta) = A\cos(\omega t)
      \]
    </p>
    <p>
      Here, \( \theta \) is the angular displacement, \( b \) is the damping coefficient, \( g \) is gravitational acceleration, \( L \) is the length of the pendulum, \( A \) is the forcing amplitude, and \( \omega \) is the driving frequency.
    </p>
  </section>

  <section>
    <h2>🧪 Simulation Controls</h2>
    <div id="controls">
      <label>Damping (b):</label>
      <input type="number" id="b" value="0.5" step="0.1"><br>
      <label>Driving Amplitude (A):</label>
      <input type="number" id="A" value="1.2" step="0.1"><br>
      <label>Driving Frequency (ω):</label>
      <input type="number" id="omega" value="2/3" step="0.01"><br>
      <label>Length (L):</label>
      <input type="number" id="L" value="9.8" step="0.1"><br>
      <button onclick="simulate()">Run Simulation</button>
    </div>
    <div id="plot"></div>
  </section>

  <section>
    <h2>📊 Interpretation</h2>
    <p>
      The graph below shows the time evolution of the angle \( \theta(t) \) under the influence of damping and external periodic forcing.
      The solution is obtained numerically using the Euler method. As parameters change, the motion may become periodic, quasi-periodic, or chaotic.
    </p>
  </section>

  <script>
    function simulate() {
      const b = parseFloat(document.getElementById("b").value);
      const A = parseFloat(document.getElementById("A").value);
      const omega = eval(document.getElementById("omega").value);
      const L = parseFloat(document.getElementById("L").value);
      const g = 9.8;

      let t = 0, dt = 0.04;
      let theta = 0.2;
      let omega_theta = 0;
      const T = 100;
      const time = [], angles = [];

      for (let i = 0; i < T / dt; i++) {
        const d2theta = -b * omega_theta - (g / L) * Math.sin(theta) + A * Math.cos(omega * t);
        omega_theta += d2theta * dt;
        theta += omega_theta * dt;
        t += dt;
        time.push(t);
        angles.push(theta);
      }

      const trace = {
        x: time,
        y: angles,
        type: 'scatter',
        mode: 'lines',
        line: { color: 'tomato', width: 2 },
        name: 'θ(t)'
      };

      const layout = {
        title: 'Forced Damped Pendulum - Angular Displacement vs Time',
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'θ (radians)' }
      };

      Plotly.newPlot('plot', [trace], layout);
    }

    simulate();
  </script>

</body>
</html>
