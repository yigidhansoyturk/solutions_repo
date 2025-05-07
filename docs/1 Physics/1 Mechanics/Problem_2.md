<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Forced Damped Pendulum Simulation</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 40px auto;
            max-width: 960px;
            background-color: #fefefe;
            color: #2d3436;
            line-height: 1.6;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            overflow-x: auto;
        }
        code {
            font-family: Consolas, monospace;
        }
        .control-group {
            margin-bottom: 10px;
        }
        label {
            font-weight: bold;
        }
        #plot {
            margin-top: 20px;
        }
    </style>
</head>
<body>

<h1>🌀 Investigating the Dynamics of a Forced Damped Pendulum</h1>

<h2>📘 Theoretical Foundation</h2>
<p>The equation governing a forced damped pendulum is:</p>
<p><span id="equation"></span></p>

<script>
  MathJax.Hub.Queue(["Typeset", MathJax.Hub, "equation"]);
  document.getElementById("equation").innerHTML = '\\( \\frac{d^2\\theta}{dt^2} + \\gamma \\frac{d\\theta}{dt} + \\omega_0^2 \\sin(\\theta) = A \\cos(\\omega t) \\)';
</script>

<p>Where:</p>
<ul>
  <li><strong>\\( \\gamma \\)</strong>: damping coefficient</li>
  <li><strong>\\( \\omega_0 \\)</strong>: natural frequency</li>
  <li><strong>\\( A \\)</strong>: driving amplitude</li>
  <li><strong>\\( \\omega \\)</strong>: driving frequency</li>
</ul>
<p>This second-order nonlinear differential equation exhibits behaviors ranging from periodic motion to chaos.</p>

<h2>🧪 Simulation Controls</h2>
<div class="control-group">
    <label for="gamma">Damping Coefficient (\\( \\gamma \\)):</label>
    <input type="range" id="gamma" min="0" max="1" step="0.01" value="0.2" oninput="simulate()">
    <span id="gammaVal">0.2</span>
</div>
<div class="control-group">
    <label for="A">Driving Amplitude (\\( A \\)):</label>
    <input type="range" id="A" min="0" max="2" step="0.1" value="1" oninput="simulate()">
    <span id="AVal">1</span>
</div>
<div class="control-group">
    <label for="omega">Driving Frequency (\\( \\omega \\)):</label>
    <input type="range" id="omega" min="0.1" max="5" step="0.1" value="1.5" oninput="simulate()">
    <span id="omegaVal">1.5</span>
</div>

<div id="plot"></div>

<h2>📈 Dynamics & Visualizations</h2>
<p>The graph above illustrates the angular displacement of a forced damped pendulum over time. Depending on your parameter choices, you may observe:</p>
<ul>
  <li>Simple harmonic motion</li>
  <li>Resonant amplification</li>
  <li>Chaotic dynamics</li>
</ul>

<script>
function simulate() {
    const gamma = parseFloat(document.getElementById('gamma').value);
    const A = parseFloat(document.getElementById('A').value);
    const omega = parseFloat(document.getElementById('omega').value);

    document.getElementById('gammaVal').textContent = gamma;
    document.getElementById('AVal').textContent = A;
    document.getElementById('omegaVal').textContent = omega;

    let t = [], theta = [], w = 0, angle = 0, dt = 0.05;
    let maxT = 50;

    for (let time = 0; time <= maxT; time += dt) {
        let dw = -gamma * w - Math.sin(angle) + A * Math.cos(omega * time);
        w += dw * dt;
        angle += w * dt;
        t.push(time);
        theta.push(angle);
    }

    const trace = {
        x: t,
        y: theta,
        mode: 'lines',
        line: { color: 'crimson' },
        name: 'Theta(t)'
    };

    const layout = {
        title: 'Forced Damped Pendulum Motion',
        xaxis: { title: 'Time (s)' },
        yaxis: { title: 'Angular Displacement (rad)' }
    };

    Plotly.newPlot('plot', [trace], layout);
}

simulate();
</script>

</body>
</html>
