<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>USA VIN Auto Report - Vehicle History Check</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, Helvetica, sans-serif;
    }

    body {
      background: #f4f6fb;
      color: #222;
    }

    header {
      background: #0b1f3a;
      padding: 18px 50px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: white;
    }

    header h1 {
      font-size: 22px;
      font-weight: bold;
    }

    nav a {
      color: white;
      text-decoration: none;
      margin-left: 20px;
      font-size: 15px;
      font-weight: 500;
    }

    nav a:hover {
      text-decoration: underline;
    }

    .hero {
      background: linear-gradient(to right, #0b1f3a, #133d75);
      color: white;
      padding: 80px 50px;
      text-align: center;
    }

    .hero h2 {
      font-size: 42px;
      margin-bottom: 12px;
    }

    .hero p {
      font-size: 17px;
      margin-bottom: 30px;
      opacity: 0.9;
    }

    .vin-box {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .vin-box input {
      padding: 14px;
      width: 330px;
      border-radius: 8px;
      border: none;
      font-size: 16px;
      outline: none;
    }

    .vin-box button {
      padding: 14px 22px;
      background: #ff3b3b;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      font-weight: bold;
    }

    .vin-box button:hover {
      background: #d92c2c;
    }

    .container {
      max-width: 1100px;
      margin: auto;
      padding: 50px 20px;
    }

    .section-title {
      text-align: center;
      margin-bottom: 35px;
    }

    .section-title h3 {
      font-size: 30px;
      margin-bottom: 10px;
      color: #0b1f3a;
    }

    .section-title p {
      font-size: 15px;
      opacity: 0.8;
    }

    .features {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 20px;
    }

    .card {
      background: white;
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0px 4px 15px rgba(0,0,0,0.08);
      text-align: center;
    }

    .card h4 {
      margin-bottom: 10px;
      color: #133d75;
    }

    .card p {
      font-size: 14px;
      opacity: 0.8;
    }

    .report-section {
      margin-top: 50px;
      background: white;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0px 4px 15px rgba(0,0,0,0.08);
      display: none;
    }

    .report-section h3 {
      margin-bottom: 15px;
      color: #0b1f3a;
    }

    .report-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 15px;
      margin-top: 15px;
    }

    .report-item {
      background: #f4f6fb;
      padding: 15px;
      border-radius: 10px;
    }

    .report-item strong {
      color: #133d75;
    }

    .loading {
      text-align: center;
      margin-top: 20px;
      font-size: 16px;
      font-weight: bold;
      color: #133d75;
      display: none;
    }

    footer {
      background: #0b1f3a;
      color: white;
      text-align: center;
      padding: 20px;
      margin-top: 50px;
      font-size: 14px;
    }
  </style>
</head>

<body>

  <!-- Header -->
  <header>
    <h1>USA VIN Auto Report</h1>
    <nav>
      <a href="#home">Home</a>
      <a href="#features">Features</a>
      <a href="#reportSection">Report</a>
    </nav>
  </header>

  <!-- Hero -->
  <section class="hero" id="home">
    <h2>Get Your Vehicle History Report Instantly</h2>
    <p>Decode your VIN with real USA NHTSA data (Make, Model, Year, Engine, Manufacturer & more).</p>

    <div class="vin-box">
      <input type="text" id="vinInput" placeholder="Enter VIN Number (17 characters)" maxlength="17">
      <button onclick="getVinReport()">Get Report</button>
    </div>

    <p style="margin-top:15px;font-size:14px;opacity:0.8;">
      🔒 Secure VIN Lookup | Powered by USA NHTSA Data
    </p>
  </section>

  <!-- Features -->
  <section class="container" id="features">
    <div class="section-title">
      <h3>Why Choose Our VIN Reports?</h3>
      <p>Fast and reliable VIN decoding directly from official USA vehicle database.</p>
    </div>

    <div class="features">
      <div class="card">
        <h4>🚘 Vehicle Overview</h4>
        <p>Get Make, Model, Year, Body Type, Engine, Trim & more.</p>
      </div>

      <div class="card">
        <h4>🏭 Manufacturer Info</h4>
        <p>See where your vehicle was built and who manufactured it.</p>
      </div>

      <div class="card">
        <h4>⚙ Engine Details</h4>
        <p>Check engine cylinders, fuel type, displacement and performance data.</p>
      </div>

      <div class="card">
        <h4>📌 USA Official Data</h4>
        <p>Reports are decoded using the NHTSA VIN Decoder system.</p>
      </div>
    </div>

    <!-- Loading -->
    <div class="loading" id="loadingText">🔄 Fetching VIN report, please wait...</div>

    <!-- Report Output -->
    <div class="report-section" id="reportSection">
      <h3>📌 VIN Report Result</h3>
      <p><strong>VIN:</strong> <span id="vinResult"></span></p>

      <div class="report-grid">
        <div class="report-item"><strong>Make:</strong> <span id="make"></span></div>
        <div class="report-item"><strong>Model:</strong> <span id="model"></span></div>
        <div class="report-item"><strong>Year:</strong> <span id="year"></span></div>
        <div class="report-item"><strong>Trim:</strong> <span id="trim"></span></div>
        <div class="report-item"><strong>Body Class:</strong> <span id="bodyClass"></span></div>
        <div class="report-item"><strong>Vehicle Type:</strong> <span id="vehicleType"></span></div>
        <div class="report-item"><strong>Engine Cylinders:</strong> <span id="cylinders"></span></div>
        <div class="report-item"><strong>Fuel Type:</strong> <span id="fuel"></span></div>
        <div class="report-item"><strong>Manufacturer:</strong> <span id="manufacturer"></span></div>
        <div class="report-item"><strong>Plant Country:</strong> <span id="country"></span></div>
      </div>
    </div>
  </section>

  <footer>
    <p>© 2026 USA VIN Auto Report. Powered by NHTSA VIN Decoder API.</p>
  </footer>

  <!-- JavaScript API Integration -->
  <script>
    async function getVinReport() {
      let vin = document.getElementById("vinInput").value.trim();

      if (vin.length !== 17) {
        alert("Please enter a valid 17-character VIN number.");
        return;
      }

      document.getElementById("loadingText").style.display = "block";
      document.getElementById("reportSection").style.display = "none";

      try {
        // NHTSA VIN Decoder API
        let apiUrl = `https://vpic.nhtsa.dot.gov/api/vehicles/decodevinvalues/${vin}?format=json`;

        let response = await fetch(apiUrl);
        let data = await response.json();

        document.getElementById("loadingText").style.display = "none";
        document.getElementById("reportSection").style.display = "block";

        let result = data.Results[0];

        // Fill Results
        document.getElementById("vinResult").innerText = vin;
        document.getElementById("make").innerText = result.Make || "N/A";
        document.getElementById("model").innerText = result.Model || "N/A";
        document.getElementById("year").innerText = result.ModelYear || "N/A";
        document.getElementById("trim").innerText = result.Trim || "N/A";
        document.getElementById("bodyClass").innerText = result.BodyClass || "N/A";
        document.getElementById("vehicleType").innerText = result.VehicleType || "N/A";
        document.getElementById("cylinders").innerText = result.EngineCylinders || "N/A";
        document.getElementById("fuel").innerText = result.FuelTypePrimary || "N/A";
        document.getElementById("manufacturer").innerText = result.Manufacturer || "N/A";
        document.getElementById("country").innerText = result.PlantCountry || "N/A";

        // Scroll to report section
        window.location.href = "#reportSection";

      } catch (error) {
        document.getElementById("loadingText").style.display = "none";
        alert("Error fetching VIN data. Please try again later.");
        console.log(error);
      }
    }
  </script>

</body>
</html>
