

<!-- page div -->
<div>

<!-- map and pop-up style -->
<style>
    body { margin: 0; padding: 0; }
    /* Ensure layer control is clickable */
    .maplibregl-ctrl-top-left {
    z-index: 1;
    }
    .layer-control-container {
    pointer-events: auto;
    }
    .maplibregl-popup {
    max-width: 400px;
    font: 12px/20px 'Helvetica Neue', Arial, Helvetica, sans-serif;
    z-index: 2;
    }
    .filter-ctrl {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 10;
    background: white;
    padding: 10px;
    border-radius: 4px;
    font-family: sans-serif;
    }
</style>

<!-- Map div -->
<div id='map' style='width: 800px; height: 600px;'>

<div class="filter-ctrl">
    <label for="category-select">Show Category:</label>
    <select id="category-select">
        <option value="all">Show All</option>
        <option value="res_area">Research area</option>
        <option value="dataset">Dataset</option>
    </select>
</div>
</div><!-- End page div -->







<script type="module">
    import maplibregl from 'https://esm.sh/maplibre-gl@5.15.0';
    import { BasemapControl } from 'https://esm.sh/maplibre-gl-components@0.6.0';
    import { LayerControl } from 'https://esm.sh/maplibre-gl-layer-control@0.11.0';


    // -----------------------------------------------------------
    // -----------------------------------------------------------
    // -----------------------------------------------------------
    // Initialize map
    const map = new maplibregl.Map({
        container: 'map',
        //style: 'https://tiles.openfreemap.org/styles/bright',
        center: [-55,-17],
        zoom: 3.5,
        style: {
        "version": 8,
        "sources": {
            "world_basemap": {
                "type": "raster",
                "tiles": [
                    "https://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/{z}/{y}/{x}"
                ],
                "tileSize": 256
            }
        },
        "layers": [{
            "id": "world_basemap",
            "type": "raster",
            "source": "world_basemap"
        }]
    }
    });


    // -----------------------------------------------------------
    // -----------------------------------------------------------
    // -----------------------------------------------------------
    map.on('load', () => {
        map.addSource('areas_and_datasets_json', {
            'type': 'geojson',
            'data': '/assets/js/research_areas_and_datasets.geojson',  
            });
        // polygon labels
        map.addLayer({
            id: 'areas_and_datasets_labels',
            type: 'symbol',
            source: 'areas_and_datasets_json',
            layout: {
                'text-field': '{short_title}',
                'text-font': ['Open Sans Semibold'],
                'text-offset': [0, 1],
                'text-anchor': 'top'
            },
            paint: {
                'text-color': '#000000',
                'text-halo-color': '#ffffff',
                'text-halo-width': 2
            }
        });
        // polygon borders
        map.addLayer({
            id: 'areas_and_datasets_border',
            type: 'line',
            source: 'areas_and_datasets_json',
            paint: {
                // Use the 'match' expression
                'line-color': [
                'match',
                ['get', 'type'], // 1. Get the property to check
                // Define your categories and their colors
                'res_area', '#3388ff', // If 'land_use' === 'residential', use Blue
                'dataset',  '#ea930b', // If 'land_use' === 'commercial', use Red
                '#aaaaaa' // If it doesn't match any above, use Grey
                ],
                'line-opacity': 0.8,
               'line-width': [
                'interpolate',
                ['linear'],
                ['zoom'],
                2, 2,
                6, 5,
                10, 8
              ]
          }
        });
        // polygon Hit Area (Fill Layer)
        // This is what the user CLICKS.
        map.addLayer({
            id: 'areas_and_datasets_hit',
            type: 'fill',
            source: 'areas_and_datasets_json',
            paint: {
                'fill-color': '#000000', // Color doesn't matter, it's invisible
                'fill-opacity': 0        // 0 opacity = invisible but interactive
          }
        });




        // -----------------------------------------------------------
        // -----------------------------------------------------------
        // -----------------------------------------------------------
        // Add navigation control
        map.addControl(new maplibregl.NavigationControl(), 'top-right');
        map.addControl(new maplibregl.GlobeControl(), 'top-right');

        // Add basemap control - fetches from xyzservices by default
        const basemapControl = new BasemapControl({
          showSearch: true,
          collapsible: true,
          displayMode: 'dropdown',
          filterGroups: ['OpenStreetMap', 'Esri', 'Google', 'OpenTopoMap'],
          excludeBroken: true,
          maxHeight: 400
        });
        map.addControl(basemapControl, 'top-right');

        // Keep places layer on top when basemap changes
        basemapControl.on('basemapchange', () => {
          if (map.getLayer('areas_and_datasets_border')) {
            map.moveLayer('areas_and_datasets_border');
          }
          if (map.getLayer('areas_and_datasets_labels')) {
            map.moveLayer('areas_and_datasets_labels');
          }
        });


        // Handle the Dropdown Filter Logic
        document.getElementById('category-select').addEventListener('change', (e) => {
            const category = e.target.value;
            // Define the filter expression
                let filterExpression = null; // null removes the filter (shows all)
                if (category !== 'all') {
                    filterExpression = ['==', 'type', category];
                }
                // Apply the same filter to BOTH layers
                // Hides the visible border lines
                map.setFilter('areas_and_datasets_border', filterExpression);
                // Disables the invisible click area
                map.setFilter('areas_and_datasets_hit', filterExpression);
        });


        // Popup Click
        map.on('click', (e) => {
            const features = map.queryRenderedFeatures(e.point, {
                layers: ['areas_and_datasets_hit']
            });

            if (!features.length) return;

            // const feature = features[0];
            
            // 2. Build the HTML string by looping
            let popupHTML = '<div class="popup-list">';
            features.forEach((feature, index) => {
                // Optional: Add a separator line between items (except the first one)
                if (index > 0) {
                    popupHTML += '<hr style="margin: 8px 0; border: 0; border-top: 1px solid #ccc;">';
                }
                popupHTML += `
                    <div class="popup-item">
                        <strong>Name:</strong> ${feature.properties.Title}<br>
                        <small>Type: ${feature.properties.type}</small>
                    </div>
                `;
            });
            popupHTML += '</div>';

            new maplibregl.Popup()
                .setLngLat(e.lngLat)
                .setHTML(popupHTML)
                .addTo(map);
        });

        // Change cursor on hover (also respects filter automatically)
        map.on('mouseenter', 'areas_and_datasets_hit', () => {
            map.getCanvas().style.cursor = 'pointer';
        });

        map.on('mouseleave', 'areas_and_datasets_hit', () => {
            map.getCanvas().style.cursor = '';
        });
});

</script>

