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
    <h1>Escape Velocities and Cosmic Velocities</h1>

    <h2>Motivation</h2>
    <p>
      The concept of escape velocity is crucial for understanding the conditions required to leave a celestial body's gravitational influence. Extending this concept, the first, second, and third cosmic velocities define the thresholds for orbiting, escaping, and leaving a star system. These principles underpin modern space exploration, from launching satellites to interplanetary missions.
    </p>

    <h2>Theoretical Foundation</h2>
    <p><strong>First Cosmic Velocity (Orbital Velocity):</strong></p>
    <p>
      \[ v_1 = \sqrt{\frac{GM}{r}} \]
    </p>
    <p><strong>Second Cosmic Velocity (Escape Velocity):</strong></p>
    <p>
      \[ v_2 = \sqrt{\frac{2GM}{r}} \]
    </p>
    <p><strong>Third Cosmic Velocity (Interstellar Escape):</strong></p>
    <p>
      It is the minimum velocity required to escape the gravitational field of the entire solar system.
    </p>

    <h2>Python Simulation</h2>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11  # gravitational constant
celestial_bodies = {
    'Earth': {'M': 5.972e24, 'r': 6.371e6},
    'Mars': {'M': 6.417e23, 'r': 3.3895e6},
    'Jupiter': {'M': 1.898e27, 'r': 6.9911e7},
}

bodies = list(celestial_bodies.keys())
v1 = []
v2 = []

for body in bodies:
    M = celestial_bodies[body]['M']
    r = celestial_bodies[body]['r']
    v1.append(np.sqrt(G * M / r))
    v2.append(np.sqrt(2 * G * M / r))

x = np.arange(len(bodies))
width = 0.35

fig, ax = plt.subplots()
ax.bar(x - width/2, v1, width, label='First Cosmic Velocity')
ax.bar(x + width/2, v2, width, label='Second Cosmic Velocity')
ax.set_xticks(x)
ax.set_xticklabels(bodies)
ax.set_ylabel('Velocity (m/s)')
ax.set_title('Cosmic Velocities for Celestial Bodies')
ax.legend()
plt.tight_layout()
plt.savefig('cosmic_velocities.png')
plt.show()</code></pre>

    <h2>Applications</h2>
    <ul>
      <li>Designing satellite orbits and interplanetary missions</li>
      <li>Determining launch velocities for space travel</li>
      <li>Understanding gravitational escape in different celestial environments</li>
    </ul>

    <h2>Discussion</h2>
    <ul>
      <li><strong>Limitations:</strong> Assumes spherical symmetry and vacuum; neglects atmospheric drag and relativistic effects</li>
      <li><strong>Extensions:</strong> Include effects of rotation, atmosphere, and gravity assists</li>
    </ul>

    <footer>
      &copy; 2025 Physics Simulations | Escape Velocities and Cosmic Dynamics
    </footer>
  </div>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</body>
</html>
