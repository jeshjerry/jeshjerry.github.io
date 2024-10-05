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
