<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kepler's Third Law: Orbital Mechanics</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.6; background: #f9f9f9; }
    h1, h2 { color: #003366; }
    section { background: #fff; padding: 20px; margin: 20px 0; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    code { background: #eee; padding: 2px 4px; border-radius: 4px; }
    canvas { margin-top: 20px; }
  </style>
</head>
<body>
  <h1>Kepler's Third Law and Orbital Mechanics</h1>

  <section>
    <h2>Overview</h2>
    <p>Kepler's Third Law states that the square of the orbital period <code>(T^2)</code> is proportional to the cube of the orbital radius <code>(r^3)</code>. This fundamental principle connects Newtonian gravity with planetary motion and is pivotal for calculating satellite trajectories and understanding celestial mechanics.</p>
  </section>

  <section>
    <h2>Derivation</h2>
    <p>From Newton's laws:</p>
    <pre><code>F = G * M * m / r^2
F = m * v^2 / r</code></pre>
    <p>Equating the forces and substituting for <code>v = 2 * pi * r / T</code>:</p>
    <pre><code>T^2 = (4 * pi^2 * r^3) / (G * M)</code></pre>
    <p>This shows <strong><code>T^2 ∝ r^3</code></strong>.</p>
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
    <h2>Real-World Applications</h2>
    <ul>
      <li><strong>Moon-Earth System:</strong> Predicting the Moon's orbital period using its distance.</li>
      <li><strong>Satellite Launch:</strong> Designing orbital paths for satellites like GPS and weather stations.</li>
      <li><strong>Interplanetary Travel:</strong> Mission planning using transfer orbits guided by Kepler's laws.</li>
    </ul>
  </section>

  <section>
    <h2>Discussion</h2>
    <p>Kepler's Third Law not only simplifies the analysis of orbital systems but also lays the foundation for space travel and astrophysics. It can be extended to elliptical orbits by using the semi-major axis in place of radius.</p>
  </section>
</body>
</html>
