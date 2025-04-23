<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Physics Simulations</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdn.jsdelivr.net/pyodide/v0.23.4/full/pyodide.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to right, #eef2f7, #d9e2ec);
      color: #2d3748;
    }
    .container {
      max-width: 960px;
      margin: 40px auto;
      padding: 40px;
      background-color: #ffffff;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    }
    h1 {
      font-size: 3rem;
      color: #1a202c;
      text-align: center;
      margin-bottom: 1rem;
    }
    .plot {
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Payload Trajectory Simulation</h1>
    <div id="cosmicVelocitiesPlot" class="plot"></div>
  </div>

  <script type="text/python">
import js
import math
import numpy as np

# Constants
G = 6.67430e-11
M = 5.972e24  # Mass of Earth
R = 6.371e6   # Radius of Earth
v0 = 7800     # Initial velocity (m/s)
angle = np.radians(45)

# Initial conditions
r0 = np.array([R, 0])
v0_vec = v0 * np.array([np.cos(angle), np.sin(angle)])

# Time setup
dt = 1
N = 5000
r = np.zeros((N, 2))
v = np.zeros((N, 2))
r[0], v[0] = r0, v0_vec

# Integrate trajectory
for i in range(N - 1):
    d = np.linalg.norm(r[i])
    a = -G * M * r[i] / d**3
    v[i + 1] = v[i] + a * dt
    r[i + 1] = r[i] + v[i + 1] * dt

# Convert to km
x_km = r[:, 0] / 1000
y_km = r[:, 1] / 1000

# Plot
js.Plotly.newPlot("cosmicVelocitiesPlot", [
  {
    "x": x_km.tolist(),
    "y": y_km.tolist(),
    "type": "scatter",
    "mode": "lines",
    "name": "Payload Trajectory",
    "line": {"color": "#3182ce", "width": 2}
  },
  {
    "x": [0],
    "y": [0],
    "type": "scatter",
    "mode": "markers",
    "name": "Earth Center",
    "marker": {"size": 8, "color": "red"}
  }
], {
  "title": "Payload Trajectory Around Earth",
  "xaxis": {"title": "X (km)"},
  "yaxis": {"title": "Y (km)"},
  "showlegend": True
})
  </script>

  <script>
    async function main() {
      const pyodide = await loadPyodide();
      await pyodide.loadPackage("numpy");
      await pyodide.runPythonAsync(document.querySelector('script[type="text/python"]').textContent);
    }
    main();
  </script>
</body>
</html>
