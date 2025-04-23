<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Investigating the Range as a Function of the Angle of Projection</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      line-height: 1.6;
      max-width: 960px;
      margin: 0 auto;
      padding: 2rem;
      background-color: #f9f9f9;
      color: #333;
    }
    h1, h2, h3 {
      color: #2c3e50;
    }
    pre {
      background: #272822;
      color: #f8f8f2;
      padding: 1em;
      overflow-x: auto;
    }
    code {
      font-family: Consolas, monospace;
    }
    .equation {
      background: #e3e3e3;
      padding: 1em;
      font-family: 'Courier New', Courier, monospace;
      margin: 1rem 0;
    }
    img {
      max-width: 100%;
      display: block;
      margin: 1rem 0;
    }
  </style>
</head>
<body>
  <h1>Investigating the Range as a Function of the Angle of Projection</h1>

  <h2>Motivation</h2>
  <p>
    Projectile motion, while seemingly simple, offers a rich playground for exploring fundamental principles of physics.
    The task is to analyze how the range of a projectile depends on its angle of projection. This system involves parameters
    like initial velocity, gravitational acceleration, and launch height — all contributing to a diverse family of solutions.
  </p>

  <h2>1. Theoretical Foundation</h2>
  <p>
    The equations of projectile motion are derived from Newton's laws. Assume a projectile is launched from ground level
    with initial velocity <em>v<sub>0</sub></em> at an angle <em>&theta;</em>.
  </p>

  <div class="equation">
    Horizontal motion: x(t) = v<sub>0</sub> cos(&theta;) * t<br>
    Vertical motion: y(t) = v<sub>0</sub> sin(&theta;) * t - 0.5 * g * t<sup>2</sup>
  </div>

  <p>
    To find the range, we determine the time <em>t</em> at which <em>y(t) = 0</em> and plug it into <em>x(t)</em>:
  </p>
  <div class="equation">
    Range R = (v<sub>0</sub><sup>2</sup> * sin(2&theta;)) / g
  </div>

  <h2>2. Analysis of the Range</h2>
  <p>
    This range is maximum when sin(2&theta;) = 1, i.e., when &theta; = 45&deg;. Increasing the initial velocity or reducing
    gravitational acceleration increases the range.
  </p>
  <img src="range_vs_angle.png" alt="Graph showing range as a function of angle">

  <h3>Effects of Parameters:</h3>
  <ul>
    <li>Initial velocity <strong>v<sub>0</sub></strong>: Quadratic increase in range.</li>
    <li>Gravitational acceleration <strong>g</strong>: Inversely proportional to range.</li>
    <li>Launch angle <strong>&theta;</strong>: Optimal range at 45&deg; under ideal conditions.</li>
  </ul>

  <h2>3. Practical Applications</h2>
  <p>
    This model applies to various real-world problems:
  </p>
  <ul>
    <li>Ballistics and sports (e.g., soccer, golf, basketball)</li>
    <li>Engineering (e.g., water fountains, structural launches)</li>
    <li>Astrophysics (e.g., satellite launch trajectories)</li>
  </ul>
  <p>
    In reality, factors like air resistance and wind must be considered for accuracy.
  </p>

  <h2>4. Implementation</h2>
  <p>
    A Python simulation can visualize the dependency of range on angle:
  </p>

  <pre><code>import numpy as np
import matplotlib.pyplot as plt

g = 9.81  # m/s^2
v0 = 20   # m/s
angles = np.linspace(0, 90, 500)
ranges = (v0 ** 2) * np.sin(2 * np.radians(angles)) / g

plt.plot(angles, ranges)
plt.title('Range vs Angle of Projection')
plt.xlabel('Angle (degrees)')
plt.ylabel('Range (meters)')
plt.grid(True)
plt.savefig('range_vs_angle.png')
plt.show()</code></pre>

  <h2>Discussion</h2>
  <p>
    The idealized model assumes no air resistance and flat terrain. For more realistic predictions:
  </p>
  <ul>
    <li>Incorporate drag force: proportional to velocity squared.</li>
    <li>Model uneven terrain using elevation maps.</li>
    <li>Account for wind forces or rotational effects (e.g., Magnus effect).</li>
  </ul>

  <h2>Conclusion</h2>
  <p>
    Understanding projectile motion's range dependency on launch angle provides fundamental insights into classical mechanics
    and has broad real-world applications. Through simulation and theoretical analysis, we grasp both its elegance and limitations.
  </p>
</body>
</html>
