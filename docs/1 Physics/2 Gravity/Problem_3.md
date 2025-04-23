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
    <h1>Trajectories of a Freely Released Payload Near Earth</h1>

    <h2>Motivation</h2>
    <p>
      When an object is released from a moving rocket near Earth, its trajectory depends on initial conditions and gravitational forces. This scenario presents a rich problem, blending principles of orbital mechanics and numerical methods. Understanding the potential trajectories is vital for space missions, such as deploying payloads or returning objects to Earth.
    </p>

    <h2>Theoretical Foundation</h2>
    <p>
      Using Newton's Law of Gravitation and second law of motion, the motion of a payload under gravity can be modeled by:
      \[ \vec{F} = -\frac{GMm}{r^2} \hat{r} \quad \Rightarrow \quad \vec{a} = -\frac{GM}{r^2} \hat{r} \]
    </p>
    <p>
      This equation leads to different conic section trajectories (elliptical, parabolic, or hyperbolic), depending on the initial velocity and direction.
    </p>

    <h2>Python Simulation</h2>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11  # m^3 kg^-1 s^-2
earth_mass = 5.972e24  # kg
earth_radius = 6.371e6  # m

# Initial conditions
v0 = 7800  # m/s (example velocity)
th = np.radians(45)  # launch angle
r0 = np.array([earth_radius, 0])
v0_vec = v0 * np.array([np.cos(th), np.sin(th)])

# Time settings
dt = 1
steps = 5000

# Arrays to store data
r = np.zeros((steps, 2))
v = np.zeros((steps, 2))
r[0] = r0
v[0] = v0_vec

# Simulation loop
for i in range(steps - 1):
    dist = np.linalg.norm(r[i])
    acc = -G * earth_mass * r[i] / dist**3
    v[i + 1] = v[i] + acc * dt
    r[i + 1] = r[i] + v[i + 1] * dt

# Plot
plt.figure(figsize=(8, 8))
plt.plot(r[:, 0] / 1e3, r[:, 1] / 1e3, label='Payload Trajectory')
plt.plot(0, 0, 'ro', label='Earth')
plt.xlabel('X (km)')
plt.ylabel('Y (km)')
plt.title('Trajectory of a Freely Released Payload')
plt.axis('equal')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.savefig('payload_trajectory.png')
plt.show()</code></pre>

    <h2>Applications</h2>
    <ul>
      <li>Satellite deployment trajectories</li>
      <li>Payload reentry analysis</li>
      <li>Space debris dynamics</li>
    </ul>

    <h2>Discussion</h2>
    <ul>
      <li><strong>Limitations:</strong> Assumes point mass Earth and ignores atmosphere</li>
      <li><strong>Extensions:</strong> Add atmospheric drag, Earth's oblateness, and 3D motion</li>
    </ul>

    <footer>
      &copy; 2025 Physics Simulations | Payload Dynamics and Trajectories
    </footer>
  </div>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</body>
</html>
