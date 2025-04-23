<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Payload Trajectories Near Earth</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 960px;
      margin: auto;
      background: white;
      padding: 40px;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    }
    h1, h2, h3 {
      color: #2c5282;
    }
    .plot {
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Trajectories of a Freely Released Payload Near Earth</h1>

    <h2>Motivation</h2>
    <p>
      When an object is released from a moving rocket near Earth, its trajectory depends on initial conditions and gravitational forces.
      This scenario presents a rich problem, blending principles of orbital mechanics and numerical methods. Understanding the potential
      trajectories is vital for space missions, such as deploying payloads or returning objects to Earth.
    </p>

    <h2>Simulation: Payload Motion Under Gravity</h2>
    <p>This simulation computes and displays the trajectory of a payload released at orbital altitude with a chosen speed and direction.</p>
    <div id="trajectoryPlot" class="plot"></div>

    <h2>Discussion</h2>
    <p>The shape of the trajectory (ellipse, parabola, or hyperbola) is determined by the payload's total mechanical energy.</p>
  </div>

  <script type="text/python" id="py-script">
import js
import numpy as np

# Constants
G = 6.67430e-11
M = 5.972e24
R = 6.371e6

# Initial conditions
altitude = 400000  # 400 km above Earth's surface
initial_speed = 7500  # in m/s
angle_deg = 45
angle_rad = np.radians(angle_deg)

r0 = np.array([R + altitude, 0])
v0 = initial_speed * np.array([np.cos(angle_rad), np.sin(angle_rad)])

# Time setup
dt = 1  # seconds
N = 5000
r = np.zeros((N, 2))
v = np.zeros((N, 2))

r[0] = r0
v[0] = v0

for i in range(N - 1):
    d = np.linalg.norm(r[i])
    a = -G * M * r[i] / d**3
    v[i + 1] = v[i] + a * dt
    r[i + 1] = r[i] + v[i + 1] * dt

x_vals = r[:, 0] / 1000  # Convert to km
y_vals = r[:, 1] / 1000

js.Plotly.newPlot("trajectoryPlot", [
  {
    "x": x_vals.tolist(),
    "y": y_vals.tolist(),
    "type": "scatter",
    "mode": "lines",
    "name": "Trajectory",
    "line": {"color": "#3182ce"}
  }
], {
  "title": "Payload Trajectory Released Near Earth",
  "xaxis": {"title": "X (km)"},
  "yaxis": {"title": "Y (km)"},
  "showlegend": true
})
  </script>

  <script>
    async function main() {
      const pyodide = await loadPyodide();
      await pyodide.loadPackage("numpy");
      await pyodide.runPythonAsync(document.getElementById("py-script").textContent);
    }
    main();
  </script>
</body>
</html>
