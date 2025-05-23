<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kepler's Third Law: Orbital Mechanics</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.150.1/build/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.150.1/examples/js/controls/OrbitControls.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      line-height: 1.6;
      background: #f9f9f9;
      color: #222;
      transition: background 0.4s, color 0.4s;
    }
    .dark-mode {
      background: #121212;
      color: #f0f0f0;
    }
    h1, h2 {
      color: #003366;
    }
    .dark-mode h1, .dark-mode h2 {
      color: #66ccff;
    }
    section {
      background: #fff;
      padding: 20px;
      margin: 20px 0;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      transition: background 0.4s, color 0.4s;
    }
    .dark-mode section {
      background: #1e1e1e;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.05);
    }
    canvas { margin-top: 20px; }
    button {
      margin-bottom: 20px;
      padding: 10px 20px;
      font-size: 14px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
      background-color: #003366;
      color: white;
      transition: background 0.3s;
    }
    .dark-mode button {
      background-color: #66ccff;
      color: #000;
    }
    #orbit3D {
      width: 100%;
      height: 400px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
  <h1>Kepler's Third Law and Orbital Mechanics</h1>

  <section>
    <h2>Overview</h2>
    <p>Kepler's Third Law is one of the foundational principles of celestial mechanics. It states that the square of the orbital period \( T^2 \) is proportional to the cube of the orbital radius \( r^3 \). This elegant relationship reveals the connection between time and space in orbital systems and allows astronomers to calculate planetary motions, design satellite orbits, and explore gravitational dynamics. It provides the framework for understanding how bodies move under the influence of gravity, from moons orbiting planets to entire planetary systems orbiting stars.</p>
  </section>

  <section>
    <h2>Derivation</h2>
    <p>From Newton's laws:</p>
    <p>\[ F = \frac{G M m}{r^2} \quad \text{and} \quad F = \frac{m v^2}{r} \]</p>
    <p>Equating the forces and substituting for \( v = \frac{2 \pi r}{T} \):</p>
    <p>\[ T^2 = \frac{4 \pi^2 r^3}{G M} \]</p>
    <p>This shows \( T^2 \propto r^3 \).</p>
  </section>

  <section>
    <h2>Python Simulation (Code Explanation)</h2>
    <p>The following Python code computes orbital periods for various radii and verifies Kepler's law:</p>
    <pre><code>import numpy as np
import matplotlib.pyplot as plt

G = 6.67430e-11  # Gravitational constant
M = 5.972e24     # Earth's mass
radii = np.linspace(7e6, 4.2e7, 100)
periods = 2 * np.pi * np.sqrt(radii**3 / (G * M))

plt.plot(radii**3, periods**2)
plt.title("Kepler's Law: T^2 vs r^3")
plt.xlabel("r^3 (m^3)")
plt.ylabel("T^2 (s^2)")
plt.grid(True)
plt.show()</code></pre>
  </section>

  <section>
    <h2>Visualization</h2>
    <canvas id="keplerChart" width="800" height="400"></canvas>
    <script>
      const G = 6.67430e-11;
      const M = 5.972e24;
      const radii = Array.from({length: 100}, (_, i) => 7e6 + i * (4.2e7 - 7e6) / 99);
      const r3 = radii.map(r => r ** 3);
      const T2 = radii.map(r => Math.pow(2 * Math.PI * Math.sqrt(r ** 3 / (G * M)), 2));

      const ctx = document.getElementById('keplerChart').getContext('2d');
      new Chart(ctx, {
        type: 'line',
        data: {
          labels: r3,
          datasets: [{
            label: 'T² vs r³',
            data: T2,
            borderColor: 'blue',
            borderWidth: 2,
            fill: false
          }]
        },
        options: {
          responsive: true,
          animation: {
            duration: 1500,
            easing: 'easeOutBounce'
          },
          scales: {
            x: {
              type: 'linear',
              title: { display: true, text: 'r³ (m³)' }
            },
            y: {
              title: { display: true, text: 'T² (s²)' }
            }
          }
        }
      });
    </script>
  </section>

  <section>
  <h2>Orbital Concepts Illustration</h2>
  <p>While 3D animation can be insightful, here we illustrate orbital concepts through simpler and more stable methods:</p>
  <ul>
    <li><strong>Proportionality of \( T^2 \) and \( r^3 \):</strong> Verified through mathematical derivation and simulation.</li>
    <li><strong>Circular vs. Elliptical Orbits:</strong> Circular orbits simplify calculations, but Kepler's law holds for elliptical orbits using the semi-major axis.</li>
    <li><strong>Real-World Relevance:</strong> Used in space mission planning, satellite launches, and estimating planetary masses.</li>
  </ul>
</section>

  <section>
    <h2>Discussion</h2>
    <p>Kepler's Third Law not only simplifies the analysis of orbital systems but also lays the foundation for space travel and astrophysics. It can be extended to elliptical orbits by using the semi-major axis in place of radius.</p>
  </section>

  <script>
    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }
  </script>
</body>
</html>
