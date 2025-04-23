<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Payload Trajectories Near Earth</title>
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
    h1, h2, h3 {
      color: #2c3e50;
    }
    #plot {
      margin-top: 30px;
    }
    label {
      font-weight: bold;
    }
    input {
      margin-bottom: 10px;
    }
  </style>
</head>
<body>

  <h1>🚁 Trajectories of a Freely Released Payload Near Earth</h1>

  <h2>🔍 Background & Motivation</h2>
  <p>
    When a payload is released from a moving spacecraft near Earth, it doesn't just fall downward. Instead, its motion is influenced by the velocity of release and Earth’s gravitational pull. This results in a variety of paths:
  </p>
  <ul>
    <li><strong>Sub-orbital Trajectories:</strong> Payloads that fall back to Earth after a short arc.</li>
    <li><strong>Orbital Trajectories:</strong> Payloads that are released with the right speed and direction to enter stable orbits around Earth.</li>
    <li><strong>Escape Trajectories:</strong> Payloads moving fast enough to break free from Earth’s gravity, following a parabolic or hyperbolic path.</li>
  </ul>
  <p>
    Understanding these trajectories is essential in satellite deployment, designing reentry capsules, or planning interplanetary missions. These paths depend on physical laws such as Newton's law of gravitation:
  </p>
  <pre><code>F = G * M * m / r²</code></pre>
  <p>
    And Newton’s second law:
  </p>
  <pre><code>F = m * a</code></pre>
  <p>
    Combined, they determine how the object accelerates due to Earth’s gravity. Simulating these numerically with simple time steps helps us visualize realistic space mechanics.
  </p>

  <h2>🌍 Simulation Settings</h2>
  <label for="velocity">Initial Speed (m/s):</label>
  <input type="range" id="velocity" min="5000" max="12000" step="100" value="8000" oninput="updatePlot()" />
  <span id="velocityValue">8000</span> m/s<br>

  <label for="angle">Launch Angle (°):</label>
  <input type="range" id="angle" min="0" max="90" step="1" value="45" oninput="updatePlot()" />
  <span id="angleValue">45</span>°<br>

  <div id="plot"></div>

  <h2>📘 What You're Seeing</h2>
  <p>
    This simulation models the 2D trajectory of a payload released from Earth, influenced only by gravity.
    Depending on the velocity and launch angle, the payload might:
  </p>
  <ul>
    <li>Re-enter Earth's surface (sub-orbital)</li>
    <li>Enter a closed elliptical orbit</li>
    <li>Escape Earth on a hyperbolic trajectory</li>
  </ul>

  <script>
    function updatePlot() {
      const G = 6.67430e-11;
      const M = 5.972e24;
      const R = 6371000;
      const dt = 1;
      const N = 5000;

      const v0 = parseFloat(document.getElementById('velocity').value);
      const angleDeg = parseFloat(document.getElementById('angle').value);
      document.getElementById('velocityValue').textContent = v0;
      document.getElementById('angleValue').textContent = angleDeg;

      const angleRad = angleDeg * Math.PI / 180;
      let r = [R, 0];
      let v = [v0 * Math.cos(angleRad), v0 * Math.sin(angleRad)];

      const x = [], y = [];

      for (let i = 0; i < N; i++) {
        const dist = Math.sqrt(r[0]**2 + r[1]**2);
        if (dist < R) break; // Hit Earth

        x.push(r[0] / 1000); // km
        y.push(r[1] / 1000);

        const aMag = -G * M / (dist ** 3);
        const a = [aMag * r[0], aMag * r[1]];

        v = [v[0] + a[0] * dt, v[1] + a[1] * dt];
        r = [r[0] + v[0] * dt, r[1] + v[1] * dt];
      }

      Plotly.newPlot('plot', [{
        x: x,
        y: y,
        type: 'scatter',
        mode: 'lines',
        name: 'Payload Trajectory',
        line: { color: 'royalblue' }
      }, {
        x: [0],
        y: [0],
        mode: 'markers',
        marker: { size: 10, color: 'green' },
        name: 'Earth Center'
      }, {
        type: 'scatter',
        mode: 'lines',
        x: Array.from({ length: 361 }, (_, i) => R * Math.cos(i * Math.PI / 180) / 1000),
        y: Array.from({ length: 361 }, (_, i) => R * Math.sin(i * Math.PI / 180) / 1000),
        line: { color: 'gray', dash: 'dot' },
        name: 'Earth Surface'
      }], {
        title: 'Trajectory of Released Payload',
        xaxis: { title: 'X (km)', scaleanchor: 'y' },
        yaxis: { title: 'Y (km)' },
        showlegend: true
      });
    }

    updatePlot();
  </script>

</body>
</html>
