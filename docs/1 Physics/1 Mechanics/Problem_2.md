<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Forced Damped Pendulum</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Roboto', sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to right, #f0f4f8, #d9e2ec);
      color: #333;
    }
    .container {
      max-width: 960px;
      margin: 0 auto;
      padding: 40px 20px;
      background-color: #ffffff;
      border-radius: 12px;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    }
    h1 {
      font-size: 2.8rem;
      color: #1a202c;
      text-align: center;
      margin-bottom: 1rem;
    }
    h2 {
      font-size: 1.8rem;
      margin-top: 2rem;
      color: #2b6cb0;
    }
    p, ul {
      font-size: 1.1rem;
      line-height: 1.7;
    }
    ul {
      margin-left: 1.5rem;
    }
    pre {
      background-color: #2d3748;
      color: #f7fafc;
      padding: 15px;
      border-radius: 8px;
      overflow-x: auto;
    }
    code {
      font-family: 'Courier New', monospace;
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
    <h1>Dynamics of a Forced Damped Pendulum</h1>

    <h2>Theoretical Foundation</h2>
    <p>The motion of a forced damped pendulum is described by the second-order differential equation:</p>
    <pre><code>d²θ/dt² + b*dθ/dt + ω₀²*sin(θ) = A*cos(ω*t)</code></pre>
    <p>
      Where:
      <ul>
        <li><strong>b</strong>: damping coefficient</li>
        <li><strong>ω₀</strong>: natural angular frequency</li>
        <li><strong>A</strong>: amplitude of the external periodic force</li>
        <li><strong>ω</strong>: frequency of the driving force</li>
      </ul>
    </p>
    <p>
      When the angle is small, the equation can be linearized using <code>sin(θ) ≈ θ</code>, simplifying the system into a driven harmonic oscillator. The most intriguing behavior, however, emerges when nonlinearity and forcing interact, leading to complex dynamics including resonance and chaos.
    </p>

    <h2>Python Simulation</h2>
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

    <h2>Practical Applications</h2>
    <p>This model is applicable in a range of systems, including:</p>
    <ul>
      <li>Energy harvesting from mechanical vibrations</li>
      <li>Vibration analysis of suspension bridges and buildings</li>
      <li>Driven electrical circuits like RLC systems</li>
    </ul>

    <h2>Model Limitations and Extensions</h2>
    <p><strong>Limitations:</strong></p>
    <ul>
      <li>Assumes linear damping</li>
      <li>Uses a purely sinusoidal driving force</li>
      <li>Does not account for structural or environmental nonlinearity</li>
    </ul>
    <p><strong>Possible Extensions:</strong></p>
    <ul>
      <li>Introduce nonlinear damping or noise</li>
      <li>Incorporate arbitrary or non-periodic driving functions</li>
      <li>Simulate chaotic behavior using bifurcation diagrams and Poincaré maps</li>
    </ul>

    <h2>Visual Dynamics Exploration</h2>
    <p>
      Complex dynamical behaviors can be visualized using phase portraits and Poincaré sections.
      These tools reveal the structure of chaotic and periodic motion, enhancing our understanding of transition behaviors.
    </p>

    <footer>
      &copy; 2025 Physics Simulations | Forced Damped Pendulum
    </footer>
  </div>
</body>
</html>
