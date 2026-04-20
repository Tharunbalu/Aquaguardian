<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>AquaGuardian Dashboard</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: white;
      text-align: center;
    }

    h1 {
      margin: 20px;
      color: #38bdf8;
    }

    .container {
      display: flex;
      justify-content: center;
      gap: 20px;
      flex-wrap: wrap;
    }

    .card {
      background: #1e293b;
      padding: 20px;
      border-radius: 12px;
      width: 200px;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }

    .value {
      font-size: 28px;
      margin-top: 10px;
    }

    .safe { color: #22c55e; }
    .warning { color: #facc15; }
    .danger { color: #ef4444; }

    canvas {
      margin-top: 30px;
      max-width: 600px;
    }
  </style>
</head>

<body>

<h1>💧 AquaGuardian Dashboard</h1>

<div class="container">
  <div class="card">
    <h3>pH Level</h3>
    <div id="ph" class="value">--</div>
  </div>

  <div class="card">
    <h3>Turbidity</h3>
    <div id="turbidity" class="value">--</div>
  </div>

  <div class="card">
    <h3>Temperature</h3>
    <div id="temp" class="value">--</div>
  </div>

  <div class="card">
    <h3>Status</h3>
    <div id="status" class="value">--</div>
  </div>
</div>

<canvas id="chart"></canvas>

<script>
  const phEl = document.getElementById("ph");
  const turbidityEl = document.getElementById("turbidity");
  const tempEl = document.getElementById("temp");
  const statusEl = document.getElementById("status");

  const ctx = document.getElementById('chart').getContext('2d');

  let chart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: [],
      datasets: [
        { label: 'pH', data: [] },
        { label: 'Turbidity', data: [] },
        { label: 'Temp', data: [] }
      ]
    }
  });

  function updateStatus(ph, turbidity) {
    if (ph >= 6.5 && ph <= 8.5 && turbidity < 300) {
      statusEl.innerHTML = "SAFE";
      statusEl.className = "value safe";
    } else if (ph < 6 || ph > 9) {
      statusEl.innerHTML = "DANGER";
      statusEl.className = "value danger";
    } else {
      statusEl.innerHTML = "WARNING";
      statusEl.className = "value warning";
    }
  }

  function updateData() {
    // 🔁 Replace this with real API later
    let ph = (6 + Math.random() * 3).toFixed(2);
    let turbidity = Math.floor(Math.random() * 500);
    let temp = (25 + Math.random() * 5).toFixed(1);

    phEl.innerHTML = ph;
    turbidityEl.innerHTML = turbidity + " NTU";
    tempEl.innerHTML = temp + " °C";

    updateStatus(ph, turbidity);

    let time = new Date().toLocaleTimeString();

    chart.data.labels.push(time);
    chart.data.datasets[0].data.push(ph);
    chart.data.datasets[1].data.push(turbidity);
    chart.data.datasets[2].data.push(temp);

    if (chart.data.labels.length > 10) {
      chart.data.labels.shift();
      chart.data.datasets.forEach(d => d.data.shift());
    }

    chart.update();
  }

  setInterval(updateData, 3000);
</script>

</body>
</html>
