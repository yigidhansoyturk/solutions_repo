<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>⚡ Lorentz Force Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      margin: 0;
      padding: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: start;
      background-color: #f4f4f4;
      font-family: Arial, sans-serif;
    }
    .container {
      width: 95%;
      max-width: 960px;
      margin: 40px 0;
      padding: 20px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }
    h1, h2 {
      color: #2c3e50;
    }
    .controls {
      margin: 20px 0;
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      align-items: center;
    }
    label {
      font-weight: bold;
    }
    input[type="range"] {
      width: 200px;
    }
    #plot {
      margin-top: 30px;
    }
    pre {
      background: #eee;
      padding: 10px;
      border-radius: 6px;
      overflow-x: auto;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>⚡ Lorentz Force: Particle Motion in a Magnetic Field</h1>

    <h2>📘 What’s Happening?</h2>
    <p>
      When a charged particle moves through a magnetic field, it experiences a force perpendicular to its motion. This Lorentz Force causes the particle to follow circular or helical paths.
      <br>
      The force is given by: <code>F = q (v × B)</code>
    </p>
    <ul>
      <li>Charged particles spiral in a uniform magnetic field.</li>
      <li>The radius and direction of motion depend on the charge, velocity, and field strength.</li>
    </ul>

    <h2>⚙️ Simulation Controls</h2>
    <div class="controls">
      <label for="vx">Initial vx:</label>
      <input type="range" id="vx" min="-10" max="10" value="5" step="0.1" oninput="update()">
      <span id="vxValue">5</span>

      <label for="vy">Initial vy:</label>
      <input type="range" id="vy" min="-10" max="10" value="0" step="0.1" oninput="update()">
      <span id="vyValue">0</span>

      <label for="vz">Initial vz:</label>
      <input type="range" id="vz" min="-10" max="10" value="5" step="0.1" oninput="update()">
      <span id="vzValue">5</span>

      <label for="Bz">Magnetic field Bz:</label>
      <input type="range" id="Bz" min="-5" max="5" value="1" step="0.1" oninput="update()">
      <span id="BzValue">1</span>
    </div>

    <div id="plot"></div>
  </div>

  <script>
    function update() {
      let vx = parseFloat(document.getElementById("vx").value);
      let vy = parseFloat(document.getElementById("vy").value);
      let vz = parseFloat(document.getElementById("vz").value);
      let Bz = parseFloat(document.getElementById("Bz").value);

      document.getElementById("vxValue").textContent = vx;
      document.getElementById("vyValue").textContent = vy;
      document.getElementById("vzValue").textContent = vz;
      document.getElementById("BzValue").textContent = Bz;

      const q = 1.0; // charge
      const m = 1.0; // mass
      const dt = 0.01;
      const N = 2000;

      let x = [0], y = [0], z = [0];
      let vx0 = vx, vy0 = vy, vz0 = vz;

      for (let i = 0; i < N; i++) {
        // Velocity update from Lorentz force (only magnetic field Bz)
        let ax = (q / m) * (vy0 * Bz);
        let ay = (q / m) * (-vx0 * Bz);
        let az = 0;

        vx0 += ax * dt;
        vy0 += ay * dt;
        vz0 += az * dt;

        // Position update
        x.push(x[i] + vx0 * dt);
        y.push(y[i] + vy0 * dt);
        z.push(z[i] + vz0 * dt);
      }

      Plotly.newPlot('plot', [{
        type: 'scatter3d',
        mode: 'lines',
        x: x,
        y: y,
        z: z,
        line: { color: 'royalblue' },
        name: 'Particle Path'
      }], {
        title: 'Trajectory of Charged Particle in Magnetic Field',
        scene: {
          xaxis: { title: 'X' },
          yaxis: { title: 'Y' },
          zaxis: { title: 'Z' }
        },
        margin: { t: 30 }
      });
    }

    update();
  </script>
</body>
</html>
