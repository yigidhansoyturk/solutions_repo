<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Physics Simulations</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
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
    h2 {
      font-size: 2rem;
      margin-top: 2rem;
      color: #2b6cb0;
      border-bottom: 2px solid #cbd5e0;
      padding-bottom: 0.3rem;
    }
    p, ul {
      font-size: 1.125rem;
      line-height: 1.75;
    }
    ul {
      margin-left: 1.5rem;
    }
    pre {
      background-color: #2d3748;
      color: #f7fafc;
      padding: 20px;
      border-radius: 10px;
      overflow-x: auto;
      font-size: 0.95rem;
    }
    code {
      font-family: 'Courier New', monospace;
    }
    .plot {
      margin-top: 2rem;
    }
    footer {
      text-align: center;
      margin-top: 3rem;
      font-size: 0.9rem;
      color: #718096;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Orbital Period and Orbital Radius</h1>

    <h2>Motivation</h2>
    <p>
      Kepler's Third Law reveals the elegant relationship between the square of the orbital period and the cube of the orbital radius for celestial bodies in circular orbits. This fundamental principle of astronomy allows us to understand planetary motion, calculate orbital characteristics, and grasp gravitational interactions at astronomical scales.
    </p>

    <h2>Theoretical Foundation</h2>
    <p>
      For a body in circular orbit, the gravitational force provides the necessary centripetal force:
    </p>
    <pre><code>F = G \cdot \frac{Mm}{r^2} = \frac{mv^2}{r}</code></pre>
    <p>Substituting the orbital velocity \( v = \frac{2\pi r}{T} \) into the equation and solving for \( T \) gives:</p>
    <pre><code>T^2 = \frac{4\pi^2}{GM} \cdot r^3</code></pre>
    <p>This derivation highlights how orbital period depends on the radius and the mass of the central body.</p>

    <h2>Python Simulation</h2>
    <pre><code>import numpy as np
import plotly.graph_objs as go
from plotly.offline import plot

G = 6.67430e-11  # gravitational constant
M = 5.972e24     # mass of Earth in kg
radii = np.linspace(6.4e6, 4.2e7, 100)
periods = 2 * np.pi * np.sqrt(radii**3 / (G * M))

fig = go.Figure()
fig.add_trace(go.Scatter(x=radii / 1e6, y=periods / 3600, mode='lines', name='Period'))
fig.update_layout(title='Orbital Period vs Orbital Radius',
                  xaxis_title='Orbital Radius (10⁶ m)',
                  yaxis_title='Orbital Period (hours)',
                  template='plotly_white')
plot(fig, filename='orbital_period_radius.html')</code></pre>
    <div class="plot" id="graph"></div>

    <h2>Applications</h2>
    <ul>
      <li>Used in calculating distances to planets and satellites</li>
      <li>Essential in designing satellite orbits and interplanetary missions</li>
      <li>Connects Newton’s laws with astronomical observations</li>
    </ul>

    <h2>Beyond Circular Orbits</h2>
    <p>
      Kepler's Third Law can be extended to elliptical orbits using the semi-major axis in place of radius. This extension enables broader application to real-world planetary motion.
    </p>

    <h2>Discussion</h2>
    <ul>
      <li><strong>Limitations:</strong> Assumes circular orbits and neglects perturbations, atmospheric drag, or relativistic effects</li>
      <li><strong>Extensions:</strong> Elliptical orbit dynamics, non-Keplerian forces (e.g., solar radiation pressure)</li>
    </ul>

    <footer>
      &copy; 2025 Physics Simulations | Orbital Period and Radius Exploration
    </footer>
  </div>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</body>
</html>
