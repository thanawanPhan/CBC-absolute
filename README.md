<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WBC Differential Calculator (Dog & Cat)</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background-color: #f4f4f9; }
    h2 { text-align: center; color: #333; }
    input, button { padding: 10px; font-size: 14px; margin: 5px 0; }
    input[type="number"] { width: 120px; }
    label { display: block; margin-top: 10px; color: #333; }

    button { 
      width: 150px;
      background-color: #4CAF50; 
      color: white; 
      border: none; 
      cursor: pointer;
    }
    button:hover { background-color: #45a049; }
    
    button.cat-btn { 
      background-color: #FF7F50;
    }
    button.cat-btn:hover {
      background-color: #FF6347;
    }

    table { 
      border-collapse: collapse; 
      margin-top: 20px; 
      width: 100%;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    th, td {
      padding: 12px;
      text-align: center;
      border: 1px solid #ccc;
    }
    th {
      background-color: #f2f2f2;
    }
    .abnormal { 
      color: red; 
      font-weight: bold;
    }
    .result-container { 
      margin-top: 20px;
      text-align: center;
    }
  </style>
</head>
<body>
  <h2>คำนวณค่า Absolute WBC Differential (หมา / แมว)</h2>

  <div style="max-width: 500px; margin: 0 auto;">
    <label>WBC (x1,000): <input type="number" id="wbc" step="0.01" required></label><br><br>
    <label>Neutrophils (%): <input type="number" id="neu" step="0.1" required></label><br>
    <label>Band neutrophils (%): <input type="number" id="band" step="0.1" required></label><br>
    <label>Eosinophils (%): <input type="number" id="eos" step="0.1" required></label><br>
    <label>Basophils (%): <input type="number" id="baso" step="0.1" required></label><br>
    <label>Lymphocytes (%): <input type="number" id="lymph" step="0.1" required></label><br>
    <label>Monocytes (%): <input type="number" id="mono" step="0.1" required></label><br><br>

    <div style="text-align: center;">
      <button onclick="calculate('dog')">คำนวณสำหรับหมา</button>
      <button class="cat-btn" onclick="calculate('cat')">คำนวณสำหรับแมว</button>
    </div>
  </div>

  <div class="result-container" id="result"></div>

  <script>
    function calculate(animal) {
      const wbcInput = parseFloat(document.getElementById("wbc").value) * 1000;
      const neu = parseFloat(document.getElementById("neu").value);
      const band = parseFloat(document.getElementById("band").value);
      const eos = parseFloat(document.getElementById("eos").value);
      const baso = parseFloat(document.getElementById("baso").value);
      const lymph = parseFloat(document.getElementById("lymph").value);
      const mono = parseFloat(document.getElementById("mono").value);

      if (isNaN(wbcInput) || isNaN(neu) || isNaN(band) || isNaN(eos) || isNaN(baso) || isNaN(lymph) || isNaN(mono)) {
        alert("กรุณากรอกข้อมูลให้ครบถ้วน");
        return;
      }

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
</body>
</html>
