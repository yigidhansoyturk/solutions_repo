<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Escape Velocities and Cosmic Velocities</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background-color: #f9f9f9;
      color: #222;
      transition: background 0.4s, color 0.4s;
    }
    .dark-mode {
      background-color: #121212;
      color: #f0f0f0;
    }
    button {
      padding: 10px 20px;
      margin-bottom: 20px;
      background-color: #003366;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .dark-mode button {
      background-color: #66ccff;
      color: #000;
    }
    section {
      background: #fff;
      padding: 20px;
      margin: 20px 0;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .dark-mode section {
      background: #1e1e1e;
      box-shadow: 0 0 10px rgba(255,255,255,0.05);
    }
  </style>
</head>
<body>
  <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
  <h1>Escape Velocities and Cosmic Velocities</h1>

  <section>
    <h2>Overview</h2>
    <p>The concept of escape velocity is crucial in astrodynamics. It defines the minimum speed needed for an object to escape the gravitational field of a body without further propulsion. Extending from this are the first, second, and third cosmic velocities:</p>
    <ul>
      <li><strong>First Cosmic Velocity</strong> – orbital velocity to maintain a stable circular orbit.</li>
      <li><strong>Second Cosmic Velocity</strong> – escape velocity from a planet's gravitational field.</li>
      <li><strong>Third Cosmic Velocity</strong> – velocity required to leave the solar system.</li>
    </ul>
    <p>These concepts form the basis for understanding satellite launches, planetary exploration, and interstellar mission design.</p>
  </section>

  <section>
    <h2>Mathematical Derivations</h2>
    <p>Starting from energy conservation, escape velocity \( v_e \) is derived by setting kinetic energy equal to gravitational potential energy:</p>
    <p>\[ \frac{1}{2}mv^2 = \frac{GMm}{r} \Rightarrow v_e = \sqrt{\frac{2GM}{r}} \]</p>
    <p>Similarly, orbital velocity \( v_o \):</p>
    <p>\[ v_o = \sqrt{\frac{GM}{r}} \]</p>
    <p>The third cosmic velocity depends on both planetary and solar gravitational fields.</p>
  </section>

  <section>
    <h2>Python Code: Calculate and Compare</h2>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11
celestial_bodies = {
    "Earth": {"mass": 5.972e24, "radius": 6.371e6},
    "Mars": {"mass": 6.39e23, "radius": 3.389e6},
    "Jupiter": {"mass": 1.898e27, "radius": 6.9911e7},
}

for name, data in celestial_bodies.items():
    M = data["mass"]
    R = data["radius"]
    v1 = np.sqrt(G * M / R)
    v2 = np.sqrt(2 * G * M / R)
    print(f"{name}: First = {v1:.2f} m/s, Second = {v2:.2f} m/s")</code></pre>
  </section>

  <section>
    <h2>Visualization</h2>
    <canvas id="velocityChart" width="800" height="400"></canvas>
    <script>
      const labels = ['Earth', 'Mars', 'Jupiter'];
      const firstCosmic = [7900, 3500, 42000];
      const secondCosmic = [11200, 5000, 60000];

      new Chart(document.getElementById('velocityChart').getContext('2d'), {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [
            {
              label: 'First Cosmic Velocity (m/s)',
              data: firstCosmic,
              backgroundColor: 'blue'
            },
            {
              label: 'Second Cosmic Velocity (m/s)',
              data: secondCosmic,
              backgroundColor: 'red'
            }
          ]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true,
              title: { display: true, text: 'Velocity (m/s)' }
            }
          }
        }
      });
    </script>
  </section>

  <section>
    <h2>Discussion and Application</h2>
    <p>These velocities have direct implications in:</p>
    <ul>
      <li>Satellite deployment and orbital stability.</li>
      <li>Designing missions to escape Earth's gravity and travel to other planets.</li>
      <li>Planning interstellar trajectories that may one day leave our solar system.</li>
    </ul>
  </section>

  <script>
    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }
  </script>
</body>
</html>