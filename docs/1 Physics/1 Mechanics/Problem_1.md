<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Projectile Motion: Range vs Angle</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px auto;
      max-width: 1000px;
      padding: 0 20px;
      background-color: #fefefe;
      color: #333;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    input[type="range"], input[type="number"] {
      width: 200px;
      margin-right: 10px;
    }
    #controls label {
      font-weight: bold;
      margin-right: 10px;
    }
    #plot {
      margin-top: 20px;
    }
    pre {
      background: #f4f4f4;
      padding: 10px;
      overflow-x: auto;
    }
    ul {
      margin-top: 0;
    }
  </style>
</head>
<body>

  <h1>🚀 Investigating Range vs Angle in Projectile Motion</h1>

  <section>
    <h2>📘 Theoretical Foundation</h2>
    <p>
      A projectile launched at an angle <strong>θ</strong> and initial speed <strong>v₀</strong> follows a parabolic path under gravity.
      Assuming flat terrain and no air resistance:
    </p>
    <pre><code>Range: R = (v₀² * sin(2θ)) / g</code></pre>
    <p>This comes from decomposing motion into components:</p>
    <ul>
      <li><code>vx = v₀ * cos(θ)</code></li>
      <li><code>vy = v₀ * sin(θ)</code></li>
      <li><code>Time of flight = 2 * vy / g</code></li>
      <li><code>Range = vx * time</code></li>
    </ul>
    <p>
      The shape of the curve depends on angle, speed, and gravity. Maximum range occurs at 45°.
    </p>
  </section>

  <section>
    <h2>🧪 Simulation Controls</h2>
    <div id="controls">
      <label>Initial Velocity (v₀):</label>
      <input type="range" id="v0" min="5" max="100" value="30" step="1" oninput="updateAll()" />
      <span id="v0Val">30</span> m/s<br><br>

      <label>Gravity (g):</label>
      <input type="number" id="gravity" min="1" max="25" value="9.81" step="0.1" oninput="updateAll()" /> m/s²<br><br>

      <label>Launch Height (h₀):</label>
      <input type="number" id="height" min="0" max="100" value="0" step="1" oninput="updateAll()" /> meters<br>
    </div>
    <div id="plot"></div>
  </section>

  <section>
    <h2>📊 Graph Interpretation</h2>
    <p>
      The plot shows how the horizontal range varies with angle for a given set of conditions.
      When height is zero, the range is maximized at 45°. If you increase height, this symmetry is broken.
      Higher velocities stretch the curve upwards.
    </p>
  </section>

  <section>
    <h2>🌍 Real-World Considerations</h2>
    <p>
      The model assumes:
      <ul>
        <li>Flat launch/landing height</li>
        <li>No air resistance</li>
        <li>Still air and no spin</li>
      </ul>
    </p>
    <p>
      In real scenarios, these factors matter:
      <ul>
        <li><strong>Air drag:</strong> slows the projectile and reduces range</li>
        <li><strong>Launch height:</strong> longer flight time, increases range</li>
        <li><strong>Wind:</strong> adds or subtracts from velocity</li>
        <li><strong>Magnus effect:</strong> from spin, curves the path</li>
      </ul>
      Such effects require numerical methods to simulate realistically.
    </p>
  </section>

  <script>
    function computeRange(v0, g, angleDeg, h0) {
      const theta = angleDeg * Math.PI / 180;
      const v0x = v0 * Math.cos(theta);
      const v0y = v0 * Math.sin(theta);
      // Time of flight with height: solve quadratic
      const t_flight = (v0y + Math.sqrt(v0y * v0y + 2 * g * h0)) / g;
      return v0x * t_flight;
    }

    function updateAll() {
      const v0 = parseFloat(document.getElementById('v0').value);
      const g = parseFloat(document.getElementById('gravity').value);
      const h0 = parseFloat(document.getElementById('height').value);
      document.getElementById('v0Val').textContent = v0;

      const angles = [];
      const ranges = [];
      for (let theta = 0; theta <= 90; theta += 0.5) {
        angles.push(theta);
        ranges.push(computeRange(v0, g, theta, h0));
      }

      const trace = {
        x: angles,
        y: ranges,
        type: 'scatter',
        mode: 'lines',
        line: { color: 'royalblue', width: 3 },
        name: `v₀ = ${v0} m/s`
      };

      const layout = {
        title: 'Range vs Angle of Projection',
        xaxis: { title: 'Angle (degrees)' },
        yaxis: { title: 'Range (meters)' }
      };

      Plotly.newPlot('plot', [trace], layout);
    }

    updateAll();
  </script>

</body>
</html>
