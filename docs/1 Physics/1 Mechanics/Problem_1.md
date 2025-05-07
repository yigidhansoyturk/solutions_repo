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

    /* New CSS to style formulas like in the image */
    .formula {
      font-size: 1.6em;
      font-family: 'Times New Roman', Times, serif;
      font-weight: bold;
      text-align: center;
      margin: 20px 0;
      color: #e74c3c;
    }

    .formula code {
      font-size: 1.3em;
      color: #e74c3c;
    }
  </style>
</head>
<body>

  <h1>🚀 Investigating Range vs Angle in Projectile Motion</h1>

  <section>
    <p>
      A projectile launched at an angle <strong>θ</strong> and initial speed <strong>v₀</strong> follows a parabolic path under gravity.
      Assuming flat terrain and no air resistance, the horizontal range is given by the equation:
    </p>
    
    <div class="formula">
      <code>Range: R = (v₀² * sin(2θ)) / g</code>
    </div>
    <p>This is derived from the motion's horizontal and vertical components:</p>
    <ul>
      <li><code>vx = v₀ * cos(θ)</code> (horizontal component)</li>
      <li><code>vy = v₀ * sin(θ)</code> (vertical component)</li>
      <li><code>Time of flight = 2 * vy / g</code></li>
      <li><code>Range = vx * time</code></li>
    </ul>
    <p>At an ideal launch angle of 45°, the range reaches its maximum value. We will explore how the range varies with different angles and initial velocities.</p>
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
      These effects require more complex models, typically solved numerically or via simulations.
    </p>
  </section>

  <script>
    // Function to compute range based on initial velocity, gravity, launch angle, and height
    function computeRange(v0, g, angleDeg, h0) {
      const theta = angleDeg * Math.PI / 180; // Convert angle to radians
      const v0x = v0 * Math.cos(theta);  // Horizontal velocity
      const v0y = v0 * Math.sin(theta);  // Vertical velocity
      // Time of flight with height: solve the quadratic equation for time
      const t_flight = (v0y + Math.sqrt(v0y * v0y + 2 * g * h0)) / g;
      return v0x * t_flight;  // Range = horizontal velocity * time of flight
    }

    // Function to update the plot based on user input
    function updateAll() {
      const v0 = parseFloat(document.getElementById('v0').value);  // Get velocity from input
      const g = parseFloat(document.getElementById('gravity').value);  // Get gravity from input
      const h0 = parseFloat(document.getElementById('height').value);  // Get height from input
      document.getElementById('v0Val').textContent = v0;  // Update displayed value of v₀

      const angles = [];  // Array for angles
      const ranges = [];  // Array for corresponding ranges

      // Calculate the range for angles from 0° to 90° (in increments of 0.5°)
      for (let theta = 0; theta <= 90; theta += 0.5) {
        angles.push(theta);
        ranges.push(computeRange(v0, g, theta, h0));
      }

      // Plot the range vs angle
      const trace = {
        x: angles,
        y: ranges,
        type: 'scatter',
        mode: 'lines',
        line: { color: 'royalblue', width: 3 },
        name: `v₀ = ${v0} m/s`
      };

      // Plot layout
      const layout = {
        title: 'Range vs Angle of Projection',
        xaxis: { title: 'Angle (degrees)' },
        yaxis: { title: 'Range (meters)' }
      };

      Plotly.newPlot('plot', [trace], layout);  // Plot using Plotly
    }

    // Initialize the plot with default values
    updateAll();
  </script>

</body>
</html>
