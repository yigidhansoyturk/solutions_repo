<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Projectile Motion Explorer</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 40px;
            line-height: 1.6;
            background-color: #f9f9f9;
            color: #333;
        }
        h1, h2 {
            color: #2c3e50;
        }
        #plot {
            width: 100%;
            max-width: 800px;
            margin: auto;
        }
        .section {
            margin-bottom: 50px;
        }
    </style>
</head>
<body>

    <h1>Projectile Range vs. Angle</h1>

    <div class="section">
        <h2>1. Theoretical Foundation</h2>
        <p>
            Projectile motion describes the curved path an object follows when launched at an angle.
            Ignoring air resistance, the range of a projectile is given by the formula:
        </p>
        <pre><code>R = (v₀² * sin(2θ)) / g</code></pre>
        <p>
            Where:
            <ul>
                <li><strong>v₀</strong>: Initial velocity</li>
                <li><strong>θ</strong>: Launch angle</li>
                <li><strong>g</strong>: Gravitational acceleration (9.81 m/s²)</li>
            </ul>
        </p>
    </div>

    <div class="section">
        <h2>2. Simulation</h2>
        <label for="velocity">Initial Velocity (m/s):</label>
        <input type="range" id="velocity" min="5" max="100" value="30" oninput="updatePlot()">
        <span id="velocityValue">30</span> m/s

        <div id="plot"></div>
    </div>

    <div class="section">
        <h2>3. Practical Insights</h2>
        <p>
            The range is maximum at a 45° angle for flat ground and no air resistance.
            You can explore how different velocities affect the shape and peak of the curve.
        </p>
        <p>
            Real-world adjustments may include:
            <ul>
                <li>Initial height offset</li>
                <li>Air resistance (drag)</li>
                <li>Uneven terrain</li>
            </ul>
        </p>
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

        updatePlot();  // initial plot
    </script>

</body>
</html>
