# CBC-absolute in DOG🐶
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WBC Differential Calculator</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    input { width: 80px; margin-bottom: 10px; }
    button { padding: 10px; background-color: #4CAF50; color: white; border: none; cursor: pointer; }
    button:hover { background-color: #45a049; }
    table { border-collapse: collapse; margin-top: 20px; width: 100%; }
    td, th { border: 1px solid #ccc; padding: 8px; text-align: center; }
  </style>
</head>
<body>
  <h2>คำนวณค่า Absolute WBC Differential พร้อมช่วงค่าปกติ</h2>

  <label>WBC (x1,000): <input type="number" id="wbc" step="0.01"></label><br><br>
  <label>Neutrophils (%): <input type="number" id="neu" step="0.1"></label><br>
  <label>Eosinophils (%): <input type="number" id="eos" step="0.1"></label><br>
  <label>Basophils (%): <input type="number" id="baso" step="0.1"></label><br>
  <label>Lymphocytes (%): <input type="number" id="lymph" step="0.1"></label><br>
  <label>Monocytes (%): <input type="number" id="mono" step="0.1"></label><br><br>

  <button onclick="calculate()">คำนวณ</button>

  <div id="result"></div>

  <script>
    function calculate() {
      const wbcInput = parseFloat(document.getElementById("wbc").value) * 1000; // คูณ 1,000
      const neu = parseFloat(document.getElementById("neu").value);
      const eos = parseFloat(document.getElementById("eos").value);
      const baso = parseFloat(document.getElementById("baso").value);
      const lymph = parseFloat(document.getElementById("lymph").value);
      const mono = parseFloat(document.getElementById("mono").value);

      // ช่วงค่าปกติเป็นเปอร์เซ็นต์
      const normalPercent = {
        neu: [51, 84],
        eos: [0, 1],
        baso: [0, 1],
        lymph: [8, 38],
        mono: [1, 9]
      };

      const resultHTML = `
        <table>
          <tr><th>เซลล์</th><th>Absolute (เซลล์/µL)</th><th>ช่วงค่าปกติ (เซลล์/µL)</th></tr>
          <tr><td>Neutrophils (${neu}%)</td><td>${Math.round(wbcInput * neu / 100)}</td><td>${Math.round(wbcInput * normalPercent.neu[0]/100)} - ${Math.round(wbcInput * normalPercent.neu[1]/100)}</td></tr>
          <tr><td>Eosinophils (${eos}%)</td><td>${Math.round(wbcInput * eos / 100)}</td><td>${Math.round(wbcInput * normalPercent.eos[0]/100)} - ${Math.round(wbcInput * normalPercent.eos[1]/100)}</td></tr>
          <tr><td>Basophils (${baso}%)</td><td>${Math.round(wbcInput * baso / 100)}</td><td>${Math.round(wbcInput * normalPercent.baso[0]/100)} - ${Math.round(wbcInput * normalPercent.baso[1]/100)}</td></tr>
          <tr><td>Lymphocytes (${lymph}%)</td><td>${Math.round(wbcInput * lymph / 100)}</td><td>${Math.round(wbcInput * normalPercent.lymph[0]/100)} - ${Math.round(wbcInput * normalPercent.lymph[1]/100)}</td></tr>
          <tr><td>Monocytes (${mono}%)</td><td>${Math.round(wbcInput * mono / 100)}</td><td>${Math.round(wbcInput * normalPercent.mono[0]/100)} - ${Math.round(wbcInput * normalPercent.mono[1]/100)}</td></tr>
        </table>
      `;
      document.getElementById("result").innerHTML = resultHTML;
    }
  </script>
</body>
</html>
