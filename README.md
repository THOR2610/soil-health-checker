<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Soil Health Checker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      padding: 0;
      text-align: center;
    }
    h1 {
      color: #4CAF50;
    }
    .container {
      margin-top: 20px;
    }
    #satelliteImage {
      max-width: 100%;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Soil Health Checker</h1>
  <p>Click the button to check your soil's health based on your location.</p>

  <button onclick="getLocation()">Check Soil Health</button>

  <div class="container">
    <p id="location"></p>
    <p id="soilHealth"></p>
    <img id="satelliteImage" src="" alt="Satellite Image" />
  </div>

  <script>
    // NASA API Key (you need to use your own API key here)
    const NASA_API_KEY = 'YOUR_NASA_API_KEY';

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition, showError);
      } else {
        document.getElementById('location').innerText = "Geolocation is not supported by this browser.";
      }
    }

    function showPosition(position) {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;
      
      document.getElementById('location').innerText = `Latitude: ${latitude} \nLongitude: ${longitude}`;
      
      // Simulate soil health analysis (random result)
      const soilHealth = analyzeSoilHealth();
      document.getElementById('soilHealth').innerText = `Soil Health: ${soilHealth}`;

      // Fetch and display satellite image from NASA
      fetchSatelliteImage(latitude, longitude);
    }

    function showError(error) {
      switch (error.code) {
        case error.PERMISSION_DENIED:
          document.getElementById('location').innerText = "User denied the request for Geolocation.";
          break;
        case error.POSITION_UNAVAILABLE:
          document.getElementById('location').innerText = "Location information is unavailable.";
          break;
        case error.TIMEOUT:
          document.getElementById('location').innerText = "The request to get user location timed out.";
          break;
        case error.UNKNOWN_ERROR:
          document.getElementById('location').innerText = "An unknown error occurred.";
          break;
      }
    }

    function analyzeSoilHealth() {
      const randomValue = Math.random();
      if (randomValue < 0.3) return "Low Nutrition Levels";
      else if (randomValue < 0.7) return "Moderate Nutrition Levels";
      else return "High Nutrition Levels";
    }

    function fetchSatelliteImage(lat, lon) {
      const date = new Date().toISOString().split('T')[0]; // Get today's date
      const imageUrl = `https://api.nasa.gov/planetary/earth/imagery?lon=${lon}&lat=${lat}&date=${date}&dim=0.10&api_key=${NASA_API_KEY}`;

      // Set the image source to the NASA API result
      document.getElementById('satelliteImage').src = imageUrl;
    }
  </script>
</body>
</html>
