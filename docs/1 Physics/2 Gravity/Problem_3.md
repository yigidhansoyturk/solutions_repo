<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Trajectories of a Freely Released Payload Near Earth</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background: #f9f9f9;
      color: #222;
      transition: background 0.3s, color 0.3s;
    }
    .dark-mode {
      background-color: #121212;
      color: #eee;
    }
    section {
      background: #fff;
      margin: 20px 0;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      transition: background 0.3s, color 0.3s;
    }
    .dark-mode section {
      background: #1e1e1e;
    }
    h1, h2 { color: #003366; }
    .dark-mode h1, .dark-mode h2 { color: #66ccff; }
    button, select {
      margin: 10px 10px 20px 0;
      padding: 10px;
      border: none;
      border-radius: 5px;
      background-color: #003366;
      color: white;
      font-size: 14px;
      cursor: pointer;
    }
    .dark-mode button, .dark-mode select {
      background-color: #66ccff;
      color: #000;
    }
  </style>
</head>
<body>
  <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
  <select id="velocitySelect" onchange="updateTrajectory()">
    <option value="7000">7000 m/s (Suborbital)</option>
    <option value="7700" selected>7700 m/s (Orbital)</option>
    <option value="11200">11200 m/s (Escape)</option>
  </select>

  <h1>Trajectories of a Freely Released Payload Near Earth</h1>

  <section>
  <h2>Overview</h2>
  <p>
    When a payload is released near Earth from a moving object such as a rocket, its trajectory is governed by the principles of orbital mechanics and gravitational physics. The motion depends critically on the initial conditions — namely, the payload's velocity, direction, and altitude.
  </p>
  <p>
    Depending on these initial parameters, the payload can follow a range of paths: it may re-enter the atmosphere (suborbital), achieve a stable circular or elliptical orbit (orbital), or escape Earth's gravity completely (hyperbolic escape trajectory). Each of these outcomes is associated with a specific energy regime and is predictable using Newtonian physics and numerical integration methods.
  </p>
  <p>
    This simulation framework allows us to explore these possibilities by adjusting the initial launch speed. Suborbital speeds result in a curved path back to Earth, orbital velocities allow continuous revolution, and escape velocities let the payload move away indefinitely. This mirrors real mission scenarios such as deploying satellites (orbital), sounding rockets (suborbital), and interplanetary probes like Voyager (escape trajectory).
  </p>
</section>

  <section>
    <h2>Key Equations and Models</h2>
    <p><strong>Gravitational Force:</strong></p>
    <p>\[ F = \frac{G M m}{r^2} \]</p>
    <p><strong>Equations of Motion (simplified 2D):</strong></p>
    <p>
      \[ \frac{d^2x}{dt^2} = -\frac{GMx}{(x^2 + y^2)^{3/2}}, \quad \frac{d^2y}{dt^2} = -\frac{GMy}{(x^2 + y^2)^{3/2}} \]
    </p>
  </section>

  <section>
    <h2>Python Simulation (Code Overview)</h2>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11
M = 5.972e24  # Earth mass
r0 = 6.371e6 + 400000  # 400 km altitude
v0 = 7700  # m/s

x, y = r0, 0
vx, vy = 0, v0

dt = 1
trajectory = []

for _ in range(10000):
    r = np.sqrt(x**2 + y**2)
    ax = -G * M * x / r**3
    ay = -G * M * y / r**3
    vx += ax * dt
    vy += ay * dt
    x += vx * dt
    y += vy * dt
    trajectory.append((x, y))

trajectory = np.array(trajectory)
plt.plot(trajectory[:,0], trajectory[:,1])
plt.axis('equal')
plt.show()</code></pre>
  </section>

  <section>
    <h2>Interactive Visualization</h2>
    <div id="trajectoryPlot" style="height: 500px;"></div>
    <script>
      const G = 6.67430e-11;
      const M = 5.972e24;
      const r0 = 6.371e6 + 4e5;

      function updateTrajectory() {
        const v0 = parseFloat(document.getElementById("velocitySelect").value);
        let x = r0, y = 0;
        let vx = 0, vy = v0;
        const dt = 1;
        const steps = 1000;
        const X = [], Y = [];

        for (let i = 0; i < steps; i++) {
          const r = Math.sqrt(x*x + y*y);
          const ax = -G * M * x / Math.pow(r, 3);
          const ay = -G * M * y / Math.pow(r, 3);
          vx += ax * dt;
          vy += ay * dt;
          x += vx * dt;
          y += vy * dt;
          X.push(x);
          Y.push(y);
        }

        Plotly.newPlot("trajectoryPlot", [{
          x: X,
          y: Y,
          mode: "lines",
          line: { color: "green" },
          name: "Trajectory"
        }], {
          title: "Simulated Payload Trajectory",
          xaxis: { title: "x (m)" },
          yaxis: { title: "y (m)" },
          margin: { t: 50 }
        });
      }

      window.onload = updateTrajectory;
    </script>
  </section>

  <section>
    <h2>Conclusion</h2>
    <p>
      Depending on its speed and release angle, the payload may fall back to Earth, enter stable orbit, or escape into space. Understanding this behavior is vital in launch vehicle design and planetary exploration.
    </p>
  </section>

  <script>
    function toggleDarkMode() {
      document.body.classList.toggle("dark-mode");
    }
  </script>
</body>
</html>
