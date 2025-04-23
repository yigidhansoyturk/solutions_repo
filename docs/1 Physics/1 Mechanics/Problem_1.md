<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Projectile Motion: Range vs Angle</title>
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
        pre {
            background-color: #f0f0f0;
            padding: 10px;
            overflow-x: auto;
        }
        code {
            font-family: Consolas, monospace;
            color: #2d3436;
        }
        #plot {
            margin-top: 20px;
        }
        label {
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>Investigating the Range as a Function of the Angle of Projection</h1>

    <div>
        <h2>📘 Theoretical Foundation</h2>
        <p>
            In ideal projectile motion, an object launched at angle <strong>θ</strong> with speed <strong>v₀</strong> follows a parabolic path.
            The horizontal range R is derived from Newton’s laws as:
        </p>
        <pre><code>R = (v₀² * sin(2θ)) / g</code></pre>
        <p>This equation emerges by solving the motion components:</p>
        <ul>
            <li><code>vx = v₀ * cos(θ)</code></li>
            <li><code>vy = v₀ * sin(θ)</code></li>
            <li>Time of flight: <code>T = 2 * v₀ * sin(θ) / g</code></li>
            <li>Range: <code>R = vx * T</code></li>
        </ul>
        <p>
            This results in a <strong>family of parabolas</strong> based on initial velocity and angle.
            All trajectories share a common shape but scale differently with <code>v₀</code> and <code>θ</code>.
        </p>
    </div>

    <div>
        <h2>🧪 Simulation: Range vs Angle</h2>
        <label for="velocity">Adjust Initial Velocity (m/s):</label>
        <input type="range" id="velocity" min="5" max="100" value="30" oninput="updatePlot()">
        <span id="velocityValue">30</span> m/s

        <div id="plot"></div>
    </div>

    <div>
        <h2>📈 Graphical Representations</h2>
        <p>
            The graph above shows how the range changes with projection angle.
            Maximum range is achieved at 45° when launched from flat ground.
            You can observe how higher initial velocities scale the range.
        </p>
    </div>

    <div>
        <h2>🧠 Limitations and Real-World Factors</h2>
        <p>
            This ideal model assumes:
            <ul>
                <li>No air resistance</li>
                <li>Flat launch and landing heights</li>
                <li>No wind or drag</li>
            </ul>
        </p>
        <p>
            Realistic modeling can include:
            <ul>
                <li>Drag force (air resistance)</li>
                <li>Uneven terrain or launch height</li>
                <li>Wind vectors</li>
                <li>Spinning effects (like Magnus force)</li>
            </ul>
            These can be simulated using numerical methods like Euler or Runge-Kutta solvers.
        </p>
    </div>

    <div>
        <h2>📜 Python Code (for reference or notebook submission)</h2>
        <p>You can copy this into a Python file or Jupyter Notebook.</p>
        <pre><code>import numpy as np
import matplotlib.pyplot as plt

def compute_range(v0, g, angle_deg):
    angle_rad = np.radians(angle_deg)
    return (v0**2 * np.sin(2 * angle_rad)) / g

v0 = 30.0
g = 9.81
angles = np.linspace(0, 90, 500)
ranges = compute_range(v0, g, angles)

plt.plot(angles, ranges)
plt.title(\"Range vs Angle of Projection\")
plt.xlabel(\"Angle (degrees)\")
plt.ylabel(\"Range (meters)\")
plt.grid(True)
plt.show()</code></pre>
    </div>

    <script>
        function computeRange(v0, g, angleDeg) {
            const angleRad = angleDeg * Math.PI / 180;
            return (Math.pow(v0, 2) * Math.sin(2 * angleRad)) / g;
        }

        function updatePlot() {
            const g = 9.81;
            const v0 = parseFloat(document.getElementById('velocity').value);
            document.getElementById('velocityValue').textContent = v0;

            const angles = [];
            const ranges = [];
            for (let theta = 0; theta <= 90; theta += 0.5) {
                angles.push(theta);
                ranges.push(computeRange(v0, g, theta));
            }

            const trace = {
                x: angles,
                y: ranges,
                type: 'scatter',
                mode: 'lines',
                line: { color: 'royalblue' },
                name: `v₀ = ${v0} m/s`
            };

            const layout = {
                title: 'Range vs Angle of Projection',
                xaxis: { title: 'Angle (degrees)' },
                yaxis: { title: 'Range (meters)' }
            };

            Plotly.newPlot('plot', [trace], layout);
        }

        updatePlot();  // initial render
    </script>

</body>
</html>
