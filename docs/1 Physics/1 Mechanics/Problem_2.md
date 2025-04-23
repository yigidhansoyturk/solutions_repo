<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Forced Damped Pendulum</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 900px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2 {
      color: #2c3e50;
    }
    pre {
      background-color: #f0f0f0;
      padding: 10px;
      overflow-x: auto;
    }
    code {
      font-family: Consolas, monospace;
    }
    .slider {
      width: 100%;
    }
  </style>
</head>
<body>

  <h1>Dynamics of a Forced Damped Pendulum</h1>

  <h2>1. Theoretical Foundation</h2>
  <p>The equation of motion for a forced damped pendulum is:</p>
  <pre><code>d²θ/dt² + b*dθ/dt + ω₀²*sin(θ) = A*cos(ω*t)</code></pre>
  <p>
    Where:
    <ul>
      <li><strong>b</strong>: Damping coefficient</li>
      <li><strong>ω₀</strong>: Natural frequency</li>
      <li><strong>A</strong>: Driving force amplitude</li>
      <li><strong>ω</strong>: Driving force frequency</li>
    </ul>
  </p>
  <p>
    For small θ, we can approximate sin(θ) ≈ θ and analyze resonance conditions where ω ≈ ω₀.
  </p>

  <h2>2. Python Simulation (Notebook Ready)</h2>
  <pre><code>import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

def pendulum(t, y, b, omega0, A, omega):
    theta, omega_ = y
    dydt = [omega_, -b * omega_ - omega0**2 * np.sin(theta) + A * np.cos(omega * t)]
    return dydt

b = 0.5
omega0 = 1.5
A = 1.2
omega = 2.0

sol = solve_ivp(pendulum, [0, 40], [0.1, 0], args=(b, omega0, A, omega), t_eval=np.linspace(0, 40, 1000))

plt.plot(sol.t, sol.y[0])
plt.title("Forced Damped Pendulum")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.grid(True)
plt.show()</code></pre>

  <h2>3. Practical Applications</h2>
  <p>
    This system models real-world systems like:
    <ul>
      <li>Energy harvesters</li>
      <li>Suspension bridges under wind load</li>
      <li>Driven RLC electrical circuits</li>
    </ul>
  </p>

  <h2>4. Discussion and Extensions</h2>
  <p>
    Limitations include:
    <ul>
      <li>Neglecting nonlinear damping</li>
      <li>Assuming a purely sinusoidal force</li>
      <li>Numerical stiffness for chaotic regimes</li>
    </ul>
    Extensions:
    <ul>
      <li>Add drag proportional to ω²</li>
      <li>Use a non-sinusoidal driving function</li>
      <li>Explore bifurcation diagrams and chaos</li>
    </ul>
  </p>

  <h2>5. Visual Analysis (To be added via notebook)</h2>
  <p>Visualizations such as phase portraits, Poincaré sections, and bifurcation diagrams can be generated with Python for deeper insight into chaotic behavior.</p>

</body>
</html>
