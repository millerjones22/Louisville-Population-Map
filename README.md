# Louisville-Population-Map
Interactive choropleth map showing population density (people per square mile) across census tracts in Jefferson County, Kentucky, using Leaflet and GeoJSON.

<!DOCTYPE html>
<html>
<head>
    <title>Jefferson County, KY Population Density</title>

    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <link rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
        }

        h2 {
            text-align: center;
            padding: 10px;
        }

        #map {
            width: 100%;
            height: 800px;
        }
    </style>
</head>

<body>

    <h2>Jefferson County, KY Population Density</h2>

    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

    <script src="jefftracts.js"></script>

    <script>

        // Set up the map
        var map = L.map('map').setView([38.19, -85.68], 10);

        // Esri basemap
        L.tileLayer(
            'https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}',
            {
                attribution: 'Tiles © Esri'
            }
        ).addTo(map);

        // Population density color ranges
        function getColor(v) {
            return v > 7184.33 ? '#800026' :
                   v > 4935.72 ? '#BD0026' :
                   v > 3311.67 ? '#E31A1C' :
                   v > 1721.32 ? '#FC4E2A' :
                   v > 114.10  ? '#FD8D3C' :
                                 '#FEB24C';
        }

        // Style census tracts
        function style(feature) {
            return {
                fillColor: getColor(feature.properties.PDENSITY),
                weight: 2,
                opacity: 1,
                color: 'white',
                dashArray: '3',
                fillOpacity: 0.7
            };
        }

        // Add census tract data
        var layer = L.geoJSON(tractData, {
            style: style,

            onEachFeature: function(feature, layer) {
                layer.bindPopup(
                    '<h3>Tract ID: ' +
                    feature.properties.GEO_ID +
                    '</h3><p>Population Density (per sq mi): ' +
                    feature.properties.PDENSITY +
                    '</p>'
                );
            }

        }).addTo(map);

    </script>

</body>
</html>
