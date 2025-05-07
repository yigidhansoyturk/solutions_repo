<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Projectile Motion: Range vs Angle</title>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 40px auto;
      max-width: 1000px;
      padding: 0 20px;
      background-color: #f8f9fa;
      color: #333;
      line-height: 1.6;
    }
    h1, h2, h3 {
      color: #2c3e50;
      margin-top: 30px;
    }
    h1 {
      border-bottom: 2px solid #3498db;
      padding-bottom: 10px;
    }
    .control-panel {
      background-color: #e8f4fc;
      padding: 20px;
      border-radius: 8px;
      margin: 20px 0;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .control-group {
      margin-bottom: 15px;
      display: flex;
      align-items: center;
    }
    label {
      font-weight: bold;
      min-width: 150px;
      margin-right: 15px;
    }
    input[type="range"] {
      width: 300px;
      margin-right: 15px;
    }
    input[type="number"] {
      width: 100px;
      padding: 5px;
      border-radius: 4px;
      border: 1px solid #ccc;
    }
    .value-display {
      min-width: 50px;
      display: inline-block;
      text-align: right;
    }
    #plot-container {
      margin: 30px 0;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 10px;
      background-color: white;
    }
    .formula {
      font-size: 1.4em;
      font-family: 'Times New Roman', Times, serif;
      text-align: center;
      margin: 25px 0;
      padding: 15px;
      background-color: #f0f7ff;
      border-left: 4px solid #3498db;
      color: #2c3e50;
    }
    .small-formula {
      font-size: 1.1em;
      font-family: 'Times New Roman', Times, serif;
      color: #2980b9;
    }
    .info-box {
      background-color: #e8f8f5;
      padding: 15px;
      border-radius: 8px;
      margin: 20px 0;
      border-left: 4px solid #1abc9c;
    }
    .limitations {
      background-color: #fdedec;
      padding: 15px;
      border-radius: 8px;
      margin: 20px 0;
      border-left: 4px solid #e74c3c;
    }
    ul {
      padding-left: 20px;
    }
    li {
      margin-bottom: 8px;
    }
    .key-point {
      font-weight: bold;
      color: #16a085;
    }
    .comparison-table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
    }
    .comparison-table th, .comparison-table td {
      border: 1px solid #ddd;
      padding: 8px;
      text-align: left;
    }
    .comparison-table th {
      background-color: #f2f2f2;
    }
    .comparison-table tr:nth-child(even) {
      background-color: #f9f9f9;
    }
  </style>
</head>
<body>

  <h1>🚀 Projectile Motion: Range vs Launch Angle</h1>

  <section>
    <p>This interactive simulation explores how the range of a projectile varies with its launch angle under different conditions. Adjust the parameters below to see how they affect the projectile's trajectory.</p>
    
    <div class="formula">
      \( R = \frac{v_0^2 \cdot \sin(2\theta)}{g} \) (for h₀ = 0)
    </div>
    
    <p>For non-zero launch heights, the range equation becomes:</p>
    
    <div class="small-formula">
      \( R = \frac{v_0 \cos\theta}{g} \left( v_0 \sin\theta + \sqrt{v_0^2 \sin^2\theta + 2gh_0} \right) \)
    </div>
  </section>

  <section class="control-panel">
    <h2>🧪 Simulation Controls</h2>
    
    <div class="control-group">
      <label for="v0">Initial Velocity (v₀):</label>
      <input type="range" id="v0" min="5" max="100" value="30" step="1" oninput="updateAll()" />
      <span id="v0Val" class="value-display">30</span> m/s
    </div>
    
    <div class="control-group">
      <label for="gravity">Gravity (g):</label>
      <input type="number" id="gravity" min="1" max="25" value="9.81" step="0.1" oninput="updateAll()" /> m/s²
    </div>
    
    <div class="control-group">
      <label for="height">Launch Height (h₀):</label>
      <input type="number" id="height" min="0" max="100" value="0" step="1" oninput="updateAll()" /> meters
    </div>
  </section>

  <div id="plot-container">
    <div id="plot"></div>
  </div>

  <section class="info-box">
    <h2>📊 Key Observations</h2>
    <ul>
      <li><span class="key-point">Maximum range occurs at 45°</span> when launched from ground level (h₀ = 0)</li>
      <li>For non-zero launch heights, the optimal angle <span class="key-point">decreases below 45°</span></li>
      <li>Range is <span class="key-point">proportional to v₀²</span> - doubling velocity quadruples the range</li>
      <li>Range is <span class="key-point">inversely proportional to g</span> - lower gravity increases range</li>
    </ul>
  </section>

  <section>
    <h2>🔍 Detailed Analysis</h2>
    <p>The motion can be broken into horizontal and vertical components:</p>
    
    <table class="comparison-table">
      <tr>
        <th>Component</th>
        <th>Equation</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Horizontal</td>
        <td class="small-formula">\( x(t) = v_0 \cos\theta \cdot t \)</td>
        <td>Constant velocity (no acceleration)</td>
      </tr>
      <tr>
        <td>Vertical</td>
        <td class="small-formula">\( y(t) = h_0 + v_0 \sin\theta \cdot t - \frac{1}{2}gt^2 \)</td>
        <td>Accelerated motion under gravity</td>
      </tr>
    </table>
    
    <p>The time of flight is determined by solving for when the projectile returns to ground level (y = 0).</p>
  </section>

  <section class="limitations">
    <h2>⚠️ Model Limitations</h2>
    <p>This simulation makes several simplifying assumptions:</p>
    <ul>
      <li><strong>No air resistance:</strong> In reality, drag forces significantly affect high-velocity projectiles</li>
      <li><strong>Constant gravity:</strong> For very high trajectories, g decreases with altitude</li>
      <li><strong>Flat Earth:</strong> Neglects Earth's curvature for long-range projectiles</li>
      <li><strong>No spin effects:</strong> Real projectiles often rotate, creating Magnus effects</li>
      <li><strong>Point mass:</strong> Ignores the projectile's size and shape</li>
    </ul>
  </section>

  <section>
    <h2>🌍 Real-World Applications</h2>
    <p>Understanding projectile motion is crucial for:</p>
    <ul>
      <li><strong>Sports:</strong> Optimizing throws, kicks, and shots in basketball, football, golf, etc.</li>
      <li><strong>Military:</strong> Artillery and missile trajectory calculations</li>
      <li><strong>Engineering:</strong> Designing water fountains, fireworks, and amusement park rides</li>
      <li><strong>Spaceflight:</strong> Calculating suborbital trajectories and re-entry paths</li>
    </ul>
  </section>

  <script>
    // Function to compute range with optional height
    function computeRange(v0, g, angleDeg, h0) {
      const theta = angleDeg * Math.PI / 180;
      const v0x = v0 * Math.cos(theta);
      const v0y = v0 * Math.sin(theta);
      
      // Solve quadratic equation for time of flight
      const discriminant = v0y * v0y + 2 * g * h0;
      if (discriminant < 0) return 0; // No real solution (projectile never reaches ground)
      
      const t_flight = (v0y + Math.sqrt(discriminant)) / g;
      return v0x * t_flight;
    }

    // Function to update the plot
    function updateAll() {
      const v0 = parseFloat(document.getElementById('v0').value);
      const g = parseFloat(document.getElementById('gravity').value);
      const h0 = parseFloat(document.getElementById('height').value);
      document.getElementById('v0Val').textContent = v0;

      // Calculate data points
      const angles = [];
      const ranges = [];
      const maxAngle = 90;
      const step = 0.5;
      
      for (let theta = 0; theta <= maxAngle; theta += step) {
        angles.push(theta);
        ranges.push(computeRange(v0, g, theta, h0));
      }

      // Find maximum range and optimal angle
      let maxRange = 0;
      let optimalAngle = 0;
      ranges.forEach((range, index) => {
        if (range > maxRange) {
          maxRange = range;
          optimalAngle = angles[index];
        }
      });

      // Create plot data
      const trace = {
        x: angles,
        y: ranges,
        type: 'scatter',
        mode: 'lines',
        line: { color: '#3498db', width: 3 },
        name: `Range (max: ${maxRange.toFixed(1)}m at ${optimalAngle.toFixed(1)}°)`
      };

      // Highlight optimal angle
      const optimalTrace = {
        x: [optimalAngle],
        y: [maxRange],
        mode: 'markers',
        marker: {
          color: '#e74c3c',
          size: 10
        },
        name: 'Optimal Angle'
      };

      // Plot layout
      const layout = {
        title: `Projectile Range vs Launch Angle (v₀ = ${v0}m/s, g = ${g}m/s², h₀ = ${h0}m)`,
        xaxis: { 
          title: 'Launch Angle (degrees)',
          range: [0, maxAngle]
        },
        yaxis: { 
          title: 'Range (meters)',
          range: [0, maxRange * 1.1]
        },
        showlegend: true,
        legend: { x: 0.7, y: 0.1 },
        annotations: [{
          x: optimalAngle,
          y: maxRange,
          text: `Max: ${maxRange.toFixed(1)}m`,
          showarrow: true,
          arrowhead: 5,
          ax: 0,
          ay: -40
        }]
      };

      Plotly.newPlot('plot', [trace, optimalTrace], layout);
    }

    // Initialize plot
    updateAll();
  </script>

</body>
</html>