<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WBC Differential Calculator (Dog & Cat)</title>
  <link href="https://fonts.googleapis.com/css2?family=Mali&display=swap" rel="stylesheet">
  <style>
    body { 
      font-family: 'Mali', cursive; 
      background: linear-gradient(to right, #FFE4E1, #FFE4E1); 
      min-height: 100vh; 
      padding: 20px; 
      color: #8B4513; 
    }
    h2 {
      text-align: center; 
      margin-bottom: 20px;
      color: #000000;
      }
    .form-container {
      background: #FFFACD;
      padding: 16px;
      border-radius: 12px;
      max-width: 500px;
      margin: auto;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    input, button {
      padding: 10px;
      font-size: 16px;
      margin: 5px 0;
      width: 100%;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
    label {
      display: block;
      margin-top: 10px;
    }
    button {
      margin-top: 15px;
      border: none;
      font-weight: bold;
    }
    button.dog-btn {
      background-color: #4CAF50;
      color: white;
    }
    button.dog-btn:hover {
      background-color: #45a049;
    }
    button.cat-btn {
      background-color: #FF7F50;
      color: white;
    }
    button.cat-btn:hover {
      background-color: #FF6347;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      table-layout: fixed;
      background: #fff;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    th, td {
      padding: 12px;
      border: 1px solid #ddd;
      text-align: center;
    }
    th {
      background-color: #f0f8ff;
    }
    .abnormal {
      color: red;
      font-weight: bold;
    }
    .result-container {
      margin-top: 30px;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }
  </style>
</head>
<body>

<h2>คำนวณ WBC Absolute (หมา / แมว)</h2>

<div class="form-container">
  <label>WBC (x1,000): <input type="number" id="wbc" step="0.01"></label>
  <label>Neutrophils (%): <input type="number" id="neu" step="0.1"></label>
  <label>Band neutrophils (%): <input type="number" id="band" step="0.1"></label>
  <label>Eosinophils (%): <input type="number" id="eos" step="0.1"></label>
  <label>Basophils (%): <input type="number" id="baso" step="0.1"></label>
  <label>Lymphocytes (%): <input type="number" id="lymph" step="0.1"></label>
  <label>Monocytes (%): <input type="number" id="mono" step="0.1"></label>

  <button class="dog-btn" onclick="calculate('dog')">คำนวณสำหรับหมา</button>
  <button class="cat-btn" onclick="calculate('cat')">คำนวณสำหรับแมว</button>
</div>

<div id="result" class="result-container"></div>

<script>
function calculate(animal) {
  const wbcInput = parseFloat(document.getElementById("wbc").value) * 1000 || 0;
  const neu = parseFloat(document.getElementById("neu").value) || 0;
  const band = parseFloat(document.getElementById("band").value) || 0;
  const eos = parseFloat(document.getElementById("eos").value) || 0;
  const baso = parseFloat(document.getElementById("baso").value) || 0;
  const lymph = parseFloat(document.getElementById("lymph").value) || 0;
  const mono = parseFloat(document.getElementById("mono").value) || 0;

  let normalPercent = {};

  if (animal === "dog") {
    normalPercent = {
      neu: [51, 84],
      band: [0, 1],
      eos: [0, 1],
      baso: [0, 1],
      lymph: [8, 38],
      mono: [1, 9]
    };
  } else if (animal === "cat") {
    normalPercent = {
      neu: [34, 84],
      band: [0, 1],
      eos: [0, 12],
      baso: [0, 1],
      lymph: [7, 60],
      mono: [0, 5]
    };
  }

  function formatCell(value, low, high) {
    if (value < low || value > high) {
      return '<span class="abnormal">' + value + ' cell/µL</span>';
    } else {
      return value + ' cell/µL';
    }
  }

  const resultHTML = `
    <h3>ผลลัพธ์สำหรับ ${animal === "dog" ? "หมา" : "แมว"}</h3>
    <table>
      <tr><th>เซลล์</th><th>Absolute (cell/µL)</th><th>ช่วงค่าปกติ (cell/µL)</th></tr>
      <tr><td>Neutrophils (${neu}%)</td><td>${formatCell(Math.round(wbcInput * neu / 100), wbcInput * normalPercent.neu[0] / 100, wbcInput * normalPercent.neu[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.neu[0] / 100)} - ${Math.round(wbcInput * normalPercent.neu[1] / 100)}</td></tr>
      <tr><td>Band neutrophils (${band}%)</td><td>${formatCell(Math.round(wbcInput * band / 100), wbcInput * normalPercent.band[0] / 100, wbcInput * normalPercent.band[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.band[0] / 100)} - ${Math.round(wbcInput * normalPercent.band[1] / 100)}</td></tr>
      <tr><td>Eosinophils (${eos}%)</td><td>${formatCell(Math.round(wbcInput * eos / 100), wbcInput * normalPercent.eos[0] / 100, wbcInput * normalPercent.eos[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.eos[0] / 100)} - ${Math.round(wbcInput * normalPercent.eos[1] / 100)}</td></tr>
      <tr><td>Basophils (${baso}%)</td><td>${formatCell(Math.round(wbcInput * baso / 100), wbcInput * normalPercent.baso[0] / 100, wbcInput * normalPercent.baso[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.baso[0] / 100)} - ${Math.round(wbcInput * normalPercent.baso[1] / 100)}</td></tr>
      <tr><td>Lymphocytes (${lymph}%)</td><td>${formatCell(Math.round(wbcInput * lymph / 100), wbcInput * normalPercent.lymph[0] / 100, wbcInput * normalPercent.lymph[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.lymph[0] / 100)} - ${Math.round(wbcInput * normalPercent.lymph[1] / 100)}</td></tr>
      <tr><td>Monocytes (${mono}%)</td><td>${formatCell(Math.round(wbcInput * mono / 100), wbcInput * normalPercent.mono[0] / 100, wbcInput * normalPercent.mono[1] / 100)}</td><td>${Math.round(wbcInput * normalPercent.mono[0] / 100)} - ${Math.round(wbcInput * normalPercent.mono[1] / 100)}</td></tr>
    </table>
  `;
  document.getElementById("result").innerHTML = resultHTML;
}
</script>
<footer style="text-align:center; margin-top: 30px; font-size: 14px; color: gray;">
  Created by Cream Vet84
</footer>
</body>

</html>
