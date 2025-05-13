
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <title>Perhitungan OEE</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 40px auto;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 12px;
      background-color: #f9f9f9;
    }
    input[type=number] {
      width: 100%;
      padding: 10px;
      margin: 6px 0 16px;
      box-sizing: border-box;
    }
    button {
      padding: 12px 20px;
      font-size: 16px;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    .result {
      margin-top: 20px;
      padding: 12px;
      background-color: #fff;
      border-radius: 6px;
      border: 1px solid #ddd;
      font-weight: bold;
    }
    .language-buttons {
      text-align: right;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="language-buttons">
    <button onclick="changeLanguage('id')">Bahasa Indonesia</button>
    <button onclick="changeLanguage('zh')">中文</button>
  </div>

  <h2 id="form-title">Form Perhitungan OEE</h2>

  <label id="plannedTime-label">Planned Production Time (menit):</label>
  <input type="number" id="plannedTime" required />

  <label id="downtime-label">Downtime (menit):</label>
  <input type="number" id="downtime" required />

  <label id="totalUnits-label">Total Units Produced:</label>
  <input type="number" id="totalUnits" required />

  <label id="goodUnits-label">Good Units:</label>
  <input type="number" id="goodUnits" required />

  <label id="idealCycleTime-label">Ideal Cycle Time (menit/unit):</label>
  <input type="number" id="idealCycleTime" value="1" required />

  <button onclick="hitungOEE()" id="calculate-button">Hitung OEE</button>

  <div class="result" id="hasil"></div>

  <script>
    const languageData = {
      id: {
        title: "Form Perhitungan OEE",
        labels: {
          plannedTime: "Planned Production Time (menit):",
          downtime: "Downtime (menit):",
          totalUnits: "Total Units Produced:",
          goodUnits: "Good Units:",
          idealCycleTime: "Ideal Cycle Time (menit/unit):",
        },
        buttonText: "Hitung OEE",
        resultTitle: "OEE Total: ",
      },
      zh: {
        title: "OEE计算表格",
        labels: {
          plannedTime: "计划生产时间（分钟）:",
          downtime: "停机时间（分钟）:",
          totalUnits: "总生产单位数:",
          goodUnits: "合格单位数:",
          idealCycleTime: "理想循环时间（分钟/单位）:",
        },
        buttonText: "计算OEE",
        resultTitle: "OEE总计：",
      },
    };

    function changeLanguage(lang) {
      document.getElementById('form-title').innerText = languageData[lang].title;
      document.getElementById('plannedTime-label').innerText = languageData[lang].labels.plannedTime;
      document.getElementById('downtime-label').innerText = languageData[lang].labels.downtime;
      document.getElementById('totalUnits-label').innerText = languageData[lang].labels.totalUnits;
      document.getElementById('goodUnits-label').innerText = languageData[lang].labels.goodUnits;
      document.getElementById('idealCycleTime-label').innerText = languageData[lang].labels.idealCycleTime;
      document.getElementById('calculate-button').innerText = languageData[lang].buttonText;
      document.getElementById('hasil').innerHTML = '';
    }

    function hitungOEE() {
      const plannedTime = parseFloat(document.getElementById('plannedTime').value);
      const downtime = parseFloat(document.getElementById('downtime').value);
      const totalUnits = parseFloat(document.getElementById('totalUnits').value);
      const goodUnits = parseFloat(document.getElementById('goodUnits').value);
      const idealCycleTime = parseFloat(document.getElementById('idealCycleTime').value);

      const operatingTime = plannedTime - downtime;

      const availability = (plannedTime > 0) ? operatingTime / plannedTime : 0;
      const performance = (operatingTime > 0) ? (totalUnits * idealCycleTime) / operatingTime : 0;
      const quality = (totalUnits > 0) ? goodUnits / totalUnits : 0;

      const oee = availability * performance * quality;

      const language = document.getElementById('calculate-button').innerText === "Hitung OEE" ? 'id' : 'zh';

      document.getElementById('hasil').innerHTML = `
        <div>Availability: ${(availability * 100).toFixed(2)}%</div>
        <div>Performance: ${(performance * 100).toFixed(2)}%</div>
        <div>Quality: ${(quality * 100).toFixed(2)}%</div>
        <hr/>
        <div style="font-size: 20px;">${languageData[language].resultTitle} ${(oee * 100).toFixed(2)}%</div>
      `;
    }
  </script>
</body>
</html>
