<!DOCTYPE html>
<html>
  <head>
    <title>Concentric Circles on Google Maps</title>
    <!-- Replace YOUR_API_KEY with your actual Google Maps API Key -->
    <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBHDOvAc5LGCRbHOazW0JuxnHt3PvwYiGk&callback=initMap" async defer></script>
    <style>
      /* Make sure the body and html fill the page */
      html, body {
        height: 100%;
        margin: 0;
      }
      
      /* Style the map container */
      #map {
        height: 100%;
        width: 100%;
      }

      /* Styling for the map label in top-left */
      .map-label {
        position: absolute;
        top: 10px;
        left: 10px;
        background-color: white;
        padding: 10px;
        font-size: 18px;
        font-weight: bold;
        color: black;
        border: 1px solid #000;
        font-family: Arial, sans-serif; /* Set the font to Arial */
        z-index: 1000; /* Ensure it appears above the map */
      }

      /* Responsive styling for smaller devices */
      @media (max-width: 768px) {
        .map-label {
          font-size: 16px; /* Adjust font size for smaller screens */
          padding: 8px;
        }
      }
    </style>
    <!-- Viewport meta tag for mobile responsiveness -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  </head>
  <body>
    <div id="map"></div>
    <div class="map-label">50, 75, 100, 125, and 150 mile radius from AMPI</div>
    <script>
      var map;
      var center = { lat: 44.9441, lng: -92.2819 }; // Centering the map on S1075 Wasson Drive, Spring Valley, WI

      function initMap() {
        map = new google.maps.Map(document.getElementById("map"), {
          center: center,
          zoom: 8,  // Adjust zoom level to show the desired area
        });

        // Define concentric circles with radii in miles and convert to meters
        var radiiInMiles = [50, 75, 100, 125, 150];
        var radiiInMeters = [
          80467,    // 50 miles
          120700.5, // 75 miles
          160934,   // 100 miles
          201167.5, // 125 miles
          241401    // 150 miles
        ];

        // Loop through radii and draw circles
        for (var i = 0; i < radiiInMeters.length; i++) {
          var radiusInMiles = radiiInMiles[i];
          var radiusInMeters = radiiInMeters[i];

          // Draw the circle with yellow color
          new google.maps.Circle({
            map: map,
            center: center,
            radius: radiusInMeters, // Radius in meters
            strokeColor: '#FF0000', // Red stroke
            strokeOpacity: 0.8,
            strokeWeight: 2,
            fillColor: '#FFFF00', // Yellow fill
            fillOpacity: 0.02, // Transparent fill to see the map below
          });

          // Calculate eastward label position based on radius using the specified multiplier
          var offsetLng = radiusInMeters / 111320; // Convert meters to degrees longitude
          var labelPosition = {
            lat: center.lat, // Same latitude as the center
            lng: center.lng + offsetLng * 1.41255 // Adjust longitude to move label further east
          };

          // Place a label (marker) at the calculated position
          new google.maps.Marker({
            position: labelPosition,
            map: map,
            label: {
              text: radiusInMiles + " miles", // Label text with radius in miles
              color: "black",
              fontSize: "16px",
              fontWeight: "bold"
            },
          });
        }
      }
    </script>
  </body>
</html>
