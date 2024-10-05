const MapView = ({ data }) => {
  return (
    <MapContainer center={[20, 0]} zoom={2} style={{ height: "600px", width: "100%" }}>
      <TileLayer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        attribution="&copy; OpenStreetMap contributors"
      />
      {data.map((region, idx) => (
        <Circle
          key={idx}
          center={[region.latitude, region.longitude]}
          radius={region.co2_emissions * 100} // Adjust scaling as needed
          color="red"
        >
          <Popup>
            <strong>{region.name}</strong><br />
            CO₂ Emissions: {region.co2_emissions} Mt<br />
            Temperature: {region.temperature}°C
          </Popup>
        </Circle>
      ))}
    </MapContainer>
  );
};

export default MapView;
