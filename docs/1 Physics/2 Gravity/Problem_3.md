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
    h3 {
      font-size: 1.5rem;
      margin-top: 1.5rem;
      color: #2c5282;
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
      margin: 2rem 0;
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
      When a payload is released from a rocket in Earth's vicinity, it doesn't just fall straight down. Instead, depending on its speed, direction, and altitude, it might return to Earth, enter orbit, or even escape into space. Studying such trajectories enhances mission planning and highlights the richness of Newtonian gravity in space mechanics.
    </p>

    <h2>Theoretical Foundation</h2>
    <h3>Newtonian Gravitation</h3>
    <p>
      According to Newton’s Law of Universal Gravitation, the gravitational force experienced by the payload is:
    </p>
    <p>$$ \vec{F} = -\frac{GMm}{r^2} \hat{r} $$</p>
    <p>
      Dividing both sides by mass \(m\), we get the acceleration:
    </p>
    <p>$$ \vec{a} = -\frac{GM}{r^2} \hat{r} $$</p>

    <h3>Trajectory Classification</h3>
    <p>
      Based on the total mechanical energy, the motion falls into one of three categories:
    </p>
    <ul>
      <li><strong>Elliptic Orbit:</strong> If total energy is negative.</li>
      <li><strong>Parabolic Escape:</strong> If total energy is exactly zero.</li>
      <li><strong>Hyperbolic Escape:</strong> If total energy is positive.</li>
    </ul>

    <h2>Python Simulation</h2>
    <p>
      We simulate the trajectory using Newtonian mechanics and visualize it using <code>matplotlib</code>.
    </p>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11  # Gravitational constant
M = 5.972e24     # Mass of Earth (kg)
R = 6.371e6      # Radius of Earth (m)

# Initial conditions
v0 = 7800                       # Initial speed (m/s)
th = np.radians(45)             # Launch angle
r0 = np.array([R, 0])
v0_vec = v0 * np.array([np.cos(th), np.sin(th)])

# Time steps
dt = 1
N = 5000
r = np.zeros((N, 2))
v = np.zeros((N, 2))
r[0], v[0] = r0, v0_vec

# Simulation loop
for i in range(N - 1):
    d = np.linalg.norm(r[i])
    a = -G * M * r[i] / d**3
    v[i + 1] = v[i] + a * dt
    r[i + 1] = r[i] + v[i + 1] * dt

# Plot results
plt.figure(figsize=(8, 8))
plt.plot(r[:,0]/1000, r[:,1]/1000, label='Payload Path')
plt.plot(0, 0, 'ro', label='Earth')
plt.title('Payload Trajectory Near Earth')
plt.xlabel('X (km)')
plt.ylabel('Y (km)')
plt.axis('equal')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.savefig('payload_trajectory.png')
plt.show()</code></pre>

    <h2>Interactive Graph</h2>
    <div id="trajectoryPlot" class="plot"></div>
    <script>
      Plotly.newPlot('trajectoryPlot', [{
        x: [],
        y: [],
        mode: 'lines',
        name: 'Trajectory',
        line: {color: '#2b6cb0'}
      }, {
        x: [0],
        y: [0],
        mode: 'markers',
        name: 'Earth',
        marker: {size: 10, color: 'red'}
      }], {
        title: 'Simulated Payload Trajectory (Interactive)',
        xaxis: {title: 'X (km)'},
        yaxis: {title: 'Y (km)'},
        margin: {t: 50},
        showlegend: true
      });
    </script>

    <h2>Implications and Future Work</h2>
    <ul>
      <li>Used in satellite mission design and orbital deployment logistics.</li>
      <li>Serves as foundation for controlled descent and re-entry modeling.</li>
      <li>Base case for interplanetary mission trajectories with adjusted parameters.</li>
    </ul>

    <h3>Limitations</h3>
    <ul>
      <li>Does not account for atmospheric drag or Earth’s oblateness.</li>
      <li>No modeling of multi-body gravitational interactions.</li>
    </ul>

    <footer>
      &copy; 2025 Physics Simulations | Trajectory Analysis Enhanced
    </footer>
  </div>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</body>
</html>
