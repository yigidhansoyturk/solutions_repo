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
    <h1>Investigating the Dynamics of a Forced Damped Pendulum</h1>

    <h2>Motivation</h2>
    <p>
      The forced damped pendulum is a captivating example of a physical system with intricate behavior resulting from the interplay of damping, restoring forces, and external driving forces. By introducing both damping and external periodic forcing, the system demonstrates a transition from simple harmonic motion to a rich spectrum of dynamics, including resonance, chaos, and quasiperiodic behavior.
    </p>
    <p>
      These phenomena are a foundation for understanding complex real-world systems, such as driven oscillators, climate systems, and mechanical structures under periodic stress. By varying the parameters of the external force—amplitude and frequency—a diverse class of solutions can be observed, highlighting synchronized oscillations, chaotic motion, and resonance phenomena.
    </p>

    <h2>Theoretical Foundation</h2>
    <p>The motion of a forced damped pendulum is governed by the differential equation:</p>
    <pre><code>d²θ/dt² + b*dθ/dt + ω₀²*sin(θ) = A*cos(ω*t)</code></pre>
    <ul>
      <li><strong>b</strong>: damping coefficient</li>
      <li><strong>ω₀</strong>: natural angular frequency</li>
      <li><strong>A</strong>: amplitude of the external periodic force</li>
      <li><strong>ω</strong>: driving frequency</li>
    </ul>
    <p>For small angles, <code>sin(θ) ≈ θ</code>, leading to a linearized form suitable for analytical solutions. However, full dynamics require numerical exploration due to nonlinearity and external forcing.</p>

    <h2>Analysis of Dynamics</h2>
    <ul>
      <li>Effect of damping coefficient, driving amplitude, and frequency on the pendulum's behavior</li>
      <li>Transition between regular oscillations and chaotic motion</li>
    </ul>

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

t_span = [0, 40]
t_eval = np.linspace(*t_span, 1000)

sol = solve_ivp(pendulum, t_span, [0.1, 0], args=(b, omega0, A, omega), t_eval=t_eval)

plt.plot(sol.t, sol.y[0])
plt.title("Forced Damped Pendulum")
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.grid(True)
plt.show()</code></pre>

    <h2>Practical Applications</h2>
    <ul>
      <li>Energy harvesting in mechanical and piezoelectric systems</li>
      <li>Bridge and building resonance analysis</li>
      <li>Driven RLC circuits in electronics</li>
    </ul>

    <h2>Limitations and Extensions</h2>
    <p><strong>Limitations:</strong></p>
    <ul>
      <li>Linear damping approximation</li>
      <li>Sinusoidal driving force only</li>
      <li>Does not account for external noise or stochastic effects</li>
    </ul>
    <p><strong>Possible Extensions:</strong></p>
    <ul>
      <li>Nonlinear damping (e.g., quadratic or Coulomb friction)</li>
      <li>Non-periodic or real-world data-driven forcing</li>
      <li>Visualization using bifurcation diagrams and Poincaré sections</li>
    </ul>

    <h2>Advanced Visualizations</h2>
    <p>
      Phase portraits and Poincaré maps reveal the complex landscape of motion—transitions from order to chaos and back. Bifurcation diagrams highlight how varying a single parameter leads to dramatically different dynamics.
    </p>

    <footer>
      &copy; 2025 Physics Simulations | Investigating Forced Damped Pendulum Dynamics
    </footer>
  </div>
</body>
</html>
