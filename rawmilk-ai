import React, { useState } from "react";

const CITIES = [
  { name: "Los Angeles", lat: 34.0522, lng: -118.2437, radius: 40 },
  { name: "San Francisco", lat: 37.7749, lng: -122.4194, radius: 30 },
  { name: "San Diego", lat: 32.7157, lng: -117.1611, radius: 30 },
  { name: "Austin", lat: 30.2672, lng: -97.7431, radius: 30 },
  { name: "Houston", lat: 29.7604, lng: -95.3698, radius: 40 },
  { name: "Dallas", lat: 32.7767, lng: -96.7970, radius: 30 },
  { name: "Chicago", lat: 41.8781, lng: -87.6298, radius: 30 },
  { name: "New York City", lat: 40.7128, lng: -74.0060, radius: 25 },
  { name: "Miami", lat: 25.7617, lng: -80.1918, radius: 30 },
  { name: "Boston", lat: 42.3601, lng: -71.0589, radius: 25 },
  { name: "Philadelphia", lat: 39.9526, lng: -75.1652, radius: 30 },
  { name: "Denver", lat: 39.7392, lng: -104.9903, radius: 25 },
];

// Simulated function for demo (replace with real geocode API)
async function getLatLng(address) {
  // For actual prod, connect to Google Maps or Mapbox Geocoding
  // Here, simulate based on city keyword match
  const addressLC = address.toLowerCase();
  for (let city of CITIES) {
    if (addressLC.includes(city.name.toLowerCase())) {
      return { lat: city.lat, lng: city.lng };
    }
  }
  return null;
}

function haversine(lat1, lng1, lat2, lng2) {
  const toRad = deg => (deg * Math.PI) / 180;
  const R = 6371; // km
  const dLat = toRad(lat2 - lat1);
  const dLng = toRad(lng2 - lng1);
  const a =
    Math.sin(dLat / 2) * Math.sin(dLat / 2) +
    Math.cos(toRad(lat1)) *
      Math.cos(toRad(lat2)) *
      Math.sin(dLng / 2) *
      Math.sin(dLng / 2);
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  return R * c;
}

export default function Home() {
  const [address, setAddress] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleSubmit = async e => {
    e.preventDefault();
    setLoading(true);
    setResult(null);
    const loc = await getLatLng(address);
    if (!loc) {
      setResult("out of service area");
      setLoading(false);
      return;
    }
    let found = false;
    for (let city of CITIES) {
      const dist = haversine(loc.lat, loc.lng, city.lat, city.lng);
      const rad = city.radius || 30;
      if (dist <= rad) {
        found = true;
        setResult(
          <>
            <div>raw milk: <b>$18/gallon</b></div>
            <div>delivery: <b>$6</b></div>
          </>
        );
        break;
      }
    }
    if (!found) setResult("out of service area");
    setLoading(false);
  };

  return (
    <div style={{
      minHeight: "100vh",
      background: "#fff",
      color: "#000",
      display: "flex",
      alignItems: "center",
      justifyContent: "center",
      fontFamily: "monospace"
    }}>
      <div style={{ width: "100%", maxWidth: 420, padding: 24 }}>
        <h1 style={{
          fontSize: 32,
          fontWeight: "bold",
          marginBottom: 36,
          textAlign: "center"
        }}>rawmilk.ai</h1>
        <form onSubmit={handleSubmit} style={{ display: "flex", flexDirection: "column", gap: 12 }}>
          <input
            type="text"
            style={{
              border: "none",
              borderBottom: "1px solid #000",
              outline: "none",
              background: "transparent",
              fontSize: 18,
              padding: 12,
              color: "#000",
              marginBottom: 12
            }}
            placeholder="enter address for raw milk."
            value={address}
            onChange={e => setAddress(e.target.value)}
            autoFocus
          />
          <button
            type="submit"
            style={{
              fontWeight: "bold",
              color: "#000",
              border: "1px solid #000",
              background: "#fff",
              borderRadius: 16,
              padding: "10px 0",
              cursor: "pointer",
              fontSize: 16
            }}
            disabled={loading || !address.trim()}
          >
            {loading ? "Checking..." : "Check"}
          </button>
        </form>
        <div style={{
          marginTop: 40,
          fontSize: 20,
          textAlign: "center",
          minHeight: 32
        }}>
          {result}
        </div>
      </div>
    </div>
  );
}
