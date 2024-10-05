<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Greenhouse Gas Impact Visualizer</title>
  
  <!-- Leaflet CSS -->
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.3/dist/leaflet.css"
    integrity="sha256-sA+4QwYv0T9Xo+Q+RyN6LTTISUHKXJZhS5hmGLlM9P0="
    crossorigin=""
  />
  
  <!-- Custom CSS -->
  /* styles.css */

body {
  margin: 0;
  font-family: Arial, sans-serif;
}

header {
  background-color: #2E8B57;
  color: white;
  padding: 20px;
  text-align: center;
}

main {
  display: flex;
  flex-direction: column;
}

#map {
  height: 70vh;
  width: 100%;
}

#info-panel {
  padding: 20px;
  background-color: #f4f4f4;
}

footer {
  background-color: #2E8B57;
  color: white;
  text-align: center;
  padding: 10px;
}

/* Responsive Design */
@media (min-width: 768px) {
  main {
    flex-direction: row;
  }
  
  #map {
    width: 70%;
    height: 100vh;
  }
  
  #info-panel {
    width: 30%;
    height: 100vh;
    overflow-y: auto;
  }
}
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  
  <header>
    <h1>Greenhouse Gas Impact Visualizer</h1>
    <p>Explore the effects of GHGs across different regions</p>
  </header>
  
  <main>
    <div id="map"></div>
    <aside id="info-panel">
      <h2>Region Information</h2>
      <div id="region-data">
        <p>Select a region on the map to see details.</p>
      </div>
    </aside>
  </main>
  
  <footer>
    <p>&copy; 2024 GHG Visualizer</p>
  </footer>
  
  <!-- Leaflet JS -->
  // script.js

// Initialize the map
const map = L.map('map').setView([20, 0], 2);

// Add OpenStreetMap tiles
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '&copy; OpenStreetMap contributors'
}).addTo(map);

// Sample Data (Replace with real data or fetch from APIs)
const regions = [
  {
    name: "Amazon Rainforest",
    coords: [ -3.4653, -62.2159 ],
    humanEmissions: { CO2: 500, CH4: 150 },
    naturalEmissions: { CO2Absorption: 1000, CH4Emissions: 200 },
    temperature: 27.5,
    largeEvents: [ "Methane Leak - 2023" ]
  },
  {
    name: "Sahara Desert",
    coords: [ 23.4162, 25.6628 ],
    humanEmissions: { CO2: 300, CH4: 80 },
    naturalEmissions: { CO2Absorption: 500, CH4Emissions: 100 },
    temperature: 35.0,
    largeEvents: [ "Dust Storm - 2022" ]
  },
  // Add more regions as needed
];

// Create Layers
const humanEmissionsLayer = L.layerGroup();
const naturalSourcesLayer = L.layerGroup();
const largeEventsLayer = L.layerGroup();
const temperatureLayer = L.layerGroup();

// Function to determine color based on temperature
function getTemperatureColor(temp) {
  return temp > 35 ? '#FF0000' :
         temp > 30 ? '#FF4500' :
         temp > 25 ? '#FFA500' :
         temp > 20 ? '#FFFF00' :
         '#32CD32';
}

// Add markers to humanEmissionsLayer
regions.forEach(region => {
  const marker = L.circleMarker(region.coords, {
    radius: 8,
    fillColor: "#FF5733",
    color: "#FF5733",
    weight: 1,
    opacity: 1,
    fillOpacity: 0.8
  }).bindPopup(`<strong>${region.name}</strong><br>
              <strong>Human-Caused Emissions:</strong><br>
              CO₂: ${region.humanEmissions.CO2} Mt<br>
              CH₄: ${region.humanEmissions.CH4} Mt`);
  humanEmissionsLayer.addLayer(marker);
});

// Add markers to naturalSourcesLayer
regions.forEach(region => {
  const marker = L.circleMarker(region.coords, {
    radius: 8,
    fillColor: "#33C1FF",
    color: "#33C1FF",
    weight: 1,
    opacity: 1,
    fillOpacity: 0.8
  }).bindPopup(`<strong>${region.name}</strong><br>
              <strong>Natural Sources & Sinks:</strong><br>
              CO₂ Absorption: ${region.naturalEmissions.CO2Absorption} Mt<br>
              CH₄ Emissions: ${region.naturalEmissions.CH4Emissions} Mt`);
  naturalSourcesLayer.addLayer(marker);
});

// Add markers to largeEventsLayer
regions.forEach(region => {
  if (region.largeEvents.length > 0) {
    const marker = L.circleMarker(region.coords, {
      radius: 8,
      fillColor: "#FFC300",
      color: "#FFC300",
      weight: 1,
      opacity: 1,
      fillOpacity: 0.8
    }).bindPopup(`<strong>${region.name}</strong><br>
              <strong>Large Emission Events:</strong><br>
              ${region.largeEvents.join('<br>')}`);
    largeEventsLayer.addLayer(marker);
  }
});

// Add temperature markers to temperatureLayer
regions.forEach(region => {
  const marker = L.circleMarker(region.coords, {
    radius: 10,
    fillColor: getTemperatureColor(region.temperature),
    color: '#000',
    weight: 1,
    opacity: 1,
    fillOpacity: 0.7
  }).bindPopup(`<strong>${region.name}</strong><br>
              Temperature: ${region.temperature}°C`);
  temperatureLayer.addLayer(marker);
});

// Add Layers to Map
humanEmissionsLayer.addTo(map);
naturalSourcesLayer.addTo(map);
largeEventsLayer.addTo(map);
temperatureLayer.addTo(map);

// Layer Control
const overlayMaps = {
  "Human-Caused Emissions": humanEmissionsLayer,
  "Natural Sources & Sinks": naturalSourcesLayer,
  "Large Emission Events": largeEventsLayer,
  "Temperature Heatmap": temperatureLayer
};

L.control.layers(null, overlayMaps, { collapsed: false }).addTo(map);

// Handling Region Selection
function onMapClick(e) {
  const { lat, lng } = e.latlng;
  // Find the nearest region (simple nearest logic)
  let nearest = null;
    let minDist = Infinity;
    regions.forEach(region => {
      const dist = Math.sqrt(Math.pow(region.coords[0] - lat, 2) + Math.pow(region.coords[1] - lng, 2));
      if (dist < minDist) {
        minDist = dist;
        nearest = region;
      }
    });
  
  if (nearest && minDist < 10) { // Threshold for selection
    displayRegionInfo(nearest);
  }
}

map.on('click', onMapClick);

// Display Region Information in Info Panel
function displayRegionInfo(region) {
  const infoPanel = document.getElementById('region-data');
  infoPanel.innerHTML = `
    <h3>${region.name}</h3>
    <p><strong>Temperature:</strong> ${region.temperature}°C</p>
    <h4>Human-Caused Emissions</h4>
    <ul>
      <li>CO₂: ${region.humanEmissions.CO2} Mt</li>
      <li>CH₄: ${region.humanEmissions.CH4} Mt</li>
    </ul>
    <h4>Natural Sources & Sinks</h4>
    <ul>
      <li>CO₂ Absorption: ${region.naturalEmissions.CO2Absorption} Mt</li>
      <li>CH₄ Emissions: ${region.naturalEmissions.CH4Emissions} Mt</li>
    </ul>
    <h4>Large Emission Events</h4>
    <ul>
      ${region.largeEvents.map(event => `<li>${event}</li>`).join('')}
    </ul>
    <h4>Policy and Scientific Challenges</h4>
    <p>Addressing GHG emissions involves complex policy-making and scientific research. Challenges include:</p>
    <ul>
      <li>Implementing effective emission reduction policies</li>
      <li>Balancing economic growth with environmental sustainability</li>
      <li>Enhancing natural sinks to absorb more GHGs</li>
      <li>Monitoring and mitigating large emission events</li>
    </ul>
    <h4>Emissions Chart</h4>
    <canvas id="emissions-chart" width="300" height="200"></canvas>
  `;
  
  // Create Bar Chart
  const ctx = document.getElementById('emissions-chart').getContext('2d');
  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: ['Human CO₂', 'Human CH₄', 'Natural CO₂ Absorption', 'Natural CH₄ Emissions'],
      datasets: [{
        label: 'Emissions (Mt)',
        data: [
          region.humanEmissions.CO2,
          region.humanEmissions.CH4,
          region.naturalEmissions.CO2Absorption,
          region.naturalEmissions.CH4Emissions
        ],
        backgroundColor: [
          'rgba(255, 87, 51, 0.6)',
          'rgba(51, 153, 255, 0.6)',
          'rgba(75, 192, 192, 0.6)',
          'rgba(255, 206, 86, 0.6)'
        ],
        borderColor: [
          'rgba(255, 87, 51, 1)',
          'rgba(51, 153, 255, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(255, 206, 86, 1)'
        ],
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: { beginAtZero: true }
      }
    }
  });
}
  <script
    src="https://unpkg.com/leaflet@1.9.3/dist/leaflet.js"
    integrity="sha256-nI4vGdr5mXYc0jFhXDvYUbtNlOfrHyJ1bQx1FZGt5k8="
    crossorigin=""
  ></script>
  
  <!-- Chart.js (Optional) -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  
  <!-- Custom JS -->
  <script src="script.js"></script>
</body>
</html>
