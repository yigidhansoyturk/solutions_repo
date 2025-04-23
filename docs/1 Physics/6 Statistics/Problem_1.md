<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>📊 Central Limit Theorem Simulation</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.0.0/math.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f2f5;
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }
    .container {
      max-width: 1000px;
      background: #fff;
      padding: 30px;
      box-shadow: 0 5px 25px rgba(0, 0, 0, 0.1);
      border-radius: 10px;
    }
    h1, h2, p {
      color: #2c3e50;
    }
    button {
      margin: 10px 0;
      padding: 10px 20px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .chart {
      margin-top: 30px;
    }
  </style>
</head>
<body>
<div class="container">
  <h1>📊 Exploring the Central Limit Theorem</h1>

  <h2>🧠 Motivation</h2>
  <p>The Central Limit Theorem (CLT) explains how the distribution of sample means becomes normal as sample size increases, regardless of the population distribution. This simulation helps visualize that concept.</p>

  <h2>🎯 Task</h2>
  <ol>
    <li>Select a population distribution (Uniform, Exponential, or Binomial).</li>
    <li>Generate a population, sample from it, compute sample means, and plot the histogram.</li>
    <li>Observe the convergence of the sample mean distribution to a normal distribution.</li>
  </ol>

  <label for="dist">Choose Distribution:</label>
  <select id="dist">
    <option value="uniform">Uniform</option>
    <option value="exponential">Exponential</option>
    <option value="binomial">Binomial</option>
  </select>

  <br>
  <label for="size">Sample Size:</label>
  <input id="size" type="number" value="30" min="1">

  <br>
  <button onclick="simulateCLT()">Run Simulation</button>
  <div id="plot" class="chart"></div>
</div>

<script>
function simulateCLT() {
  const dist = document.getElementById('dist').value;
  const sampleSize = parseInt(document.getElementById('size').value);
  const repetitions = 1000;
  const populationSize = 10000;
  let population = [];

  if (dist === 'uniform') {
    population = Array.from({ length: populationSize }, () => Math.random());
  } else if (dist === 'exponential') {
    population = Array.from({ length: populationSize }, () => -Math.log(Math.random()));
  } else if (dist === 'binomial') {
    population = Array.from({ length: populationSize }, () => Math.floor(Math.random() + Math.random()));
  }

  const means = [];
  for (let i = 0; i < repetitions; i++) {
    const sample = [];
    for (let j = 0; j < sampleSize; j++) {
      const index = Math.floor(Math.random() * populationSize);
      sample.push(population[index]);
    }
    const mean = sample.reduce((a, b) => a + b, 0) / sample.length;
    means.push(mean);
  }

  const trace = {
    x: means,
    type: 'histogram',
    marker: { color: '#3498db' },
    opacity: 0.7,
  };

  const layout = {
    title: `Sampling Distribution of Mean (n = ${sampleSize}, ${dist} distribution)`,
    xaxis: { title: 'Sample Mean' },
    yaxis: { title: 'Frequency' },
    bargap: 0.05,
  };

  Plotly.newPlot('plot', [trace], layout);
}
</script>
</body>
</html>