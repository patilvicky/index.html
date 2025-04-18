<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>TLS Certificate Expiry Status</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f8f9fa;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .tile {
      background: white;
      border-radius: 8px;
      padding: 20px;
      margin: 10px auto;
      width: 300px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    .status-ok {
      color: green;
      font-weight: bold;
    }
    .status-notok {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>PROD TLS Certificate Expiry</h1>

  <div id="tiles"></div>

  <script>
    const certData = [
      { title: "MDP REST SPE2 SPE7", date: "22/09/2025" },
      { title: "SYSLOG SPE2", date: "10/06/2026" },
      { title: "SYSLOG SPE7", date: "10/06/2026" },
      { title: "XA GUI", date: "30/05/2025" },
      { title: "SPE2 XMS GUI", date: "30/05/2025" },
      { title: "SPE7 XMS GUI", date: "30/05/2025" }
    ];

    function getDaysLeft(expiryDate) {
      const [day, month, year] = expiryDate.split("/");
      const certDate = new Date(`${year}-${month}-${day}`);
      const now = new Date();
      const diffTime = certDate - now;
      return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    }

    const tilesDiv = document.getElementById("tiles");

    certData.forEach(cert => {
      const daysLeft = getDaysLeft(cert.date);
      const status = daysLeft <= 50 ? "NOT OK" : "OK";
      const statusClass = daysLeft <= 50 ? "status-notok" : "status-ok";

      const tile = document.createElement("div");
      tile.className = "tile";
      tile.innerHTML = `
        <h3>${cert.title}</h3>
        <p>Expiry Date: <strong>${cert.date}</strong></p>
        <p>Days Left: <strong>${daysLeft}</strong></p>
        <p>Status: <span class="${statusClass}">${status}</span></p>
      `;
      tilesDiv.appendChild(tile);
    });
  </script>
</body>
</html>
