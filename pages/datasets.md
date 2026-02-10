---
layout: page
title: Datasets
subtitle: Talk is cheap. Show us the data
hero_image: "/img/headers/datasets.jpg"
hero_darken: true
show_sidebar: false
---

<!-- maplibre scripts -->
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl@5.15.0/dist/maplibre-gl.css" />
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl-components@0.6.0/dist/maplibre-gl-components.css" />
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl-layer-control/dist/maplibre-gl-layer-control.css" />

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
                "tileSize": 256,
                "attribution": 'Sources: Esri, HERE, Garmin, FAO, NOAA, USGS, © OpenStreetMap contributors, and the GIS User Community'
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
                'res_area', '#3388ff', // 
                'dataset',  '#ea930b', // 
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
        // filtering categories
        const targetCategory = 'dataset';
        const filterExpression = ['==', ['get', 'type'], targetCategory];
        // Hide the borders of other categories
        map.setFilter('areas_and_datasets_border', filterExpression);
        // Disable the click-area for other categories
        map.setFilter('areas_and_datasets_hit', filterExpression);
        // Disable the labels for other categories
        map.setFilter('areas_and_datasets_labels', filterExpression);



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










<!-- page div -->



This page has direct links to large datasets (point clouds, RPA images). If you are looking for the scripts and codes that go alogn with our articles, they are in GitHub, both in the [SPAMLab Repo](https://github.com/SPAMLab/data_sharing) and in [Prof. Grohmann's Repo](https://github.com/CarlosGrohmann/scripts_papers).  
&nbsp;

The map below shows the distribution of the datasets. Click on any rectangle to see more information about it. 
&nbsp;

<!-- Map div -->
<div id='map' style='width: 800px; height: 600px;'></div>
&nbsp;
&nbsp;


#### Datasets lists

Coelho, R.D., Frugis, G.L., Viana, C.D., Grohmann, C.H. (2025). High-resolution Digital Terrain Model and Land-surface Parameters of São Sebastião, southeastern Brazil (1.0). 
_Zenodo_  
Data provided by FAPESP grant [2023/11197-1](https://bv.fapesp.br/57077).  
[https://doi.org/10.5281/zenodo.16903678 (version 1)](https://doi.org/10.5281/zenodo.16903678)  


Frugis, G.L., Coelho, R.D., Viana, C.D., Grohmann, C.H.(2025). High-resolution Digital Terrain Model and Land-surface Parameters of Ilhabela, southeastern Brazil (1.0) 
_Zenodo_  
Data provided by FAPESP grant [2023/11197-1](https://bv.fapesp.br/57077).  
[https://doi.org/10.5281/zenodo.16903853 (version 1)](https://doi.org/10.5281/zenodo.16903853)  


Grohmann, C. H. (2025). Caverna do Diabo (Cav. da Tapagem / Devil's Cave) - Point Clouds  
_Zenodo_  
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264)  
[https://doi.org/10.5281/zenodo.14860981](https://doi.org/10.5281/zenodo.14860981)  


Coelho, R. D., Grohmann, C. H., Viana, C. D., Dias, V. (2024). Landslides of the 2023 summer event of São Sebastião, southeastern Brazil. Vector dataset (polygons and points). 
_Zenodo_  
Data provided by FAPESP grant [2023/11197-1](https://bv.fapesp.br/57077).  
[https://doi.org/10.5281/zenodo.11120078 (version 2)](https://doi.org/10.5281/zenodo.11120078)  
<!-- [https://doi.org/10.5281/zenodo.10558182 (version 1)](https://doi.org/10.5281/zenodo.10558182)   -->


Grohmann, C.H., 2023. Viana Beach, Ilhabela, Brazil. UAV Image Collection.  
_Zenodo_    
Data provided by FAPESP grant [2019/26568-0](https://bv.fapesp.br/52552).  
[https://doi.org/10.5281/zenodo.18418466](https://doi.org/10.5281/zenodo.18418466)  


Grohmann, C.H., Viana, C.D., Garcia, G.P.B., Albuquerque, R.W., Barale, F., Ferretti, F.A., 2022. Jardim Garcia quarry. RPA images, 3D mesh and point coud.  
_Zenodo_  
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://doi.org/10.5281/zenodo.7220755](https://doi.org/10.5281/zenodo.7220755)  


Grohmann, C.H. and Gomes, F., 2022. Digital Terrain and Surface Models of São Paulo.  
_Kaggle_  
[https://doi.org/10.34740/KAGGLE/DS/1915612](https://doi.org/10.34740/KAGGLE/DS/1915612)  


Grohmann, C.H., 2022. CEU Paz, São Paulo State, Brazil. UAV Image Collection.  
_GeoNadir.com_    
Data provided by FAPESP grant [2019/26568-0](https://bv.fapesp.br/52552).  
[https://data.geonadir.com/project-details/339](https://data.geonadir.com/project-details/797)  


Grohmann, C.H., 2022. Santa Madalena I (Jardim Elba), São Paulo State, Brazil. UAV Image Collection.  
_GeoNadir.com_    
Data provided by FAPESP grant [2019/26568-0](https://bv.fapesp.br/52552).  
[https://data.geonadir.com/project-details/339](https://data.geonadir.com/project-details/798)  


Lana, J.C., Jesus, D., Antonelli, T., 2021. Guia de procedimentos técnicos do Departamento de Gestão Territorial: setorização de áreas de risco geológico.  
_Repositório Institucional de Geociências - Serviço Geológico do Brasil_  
[https://rigeo.cprm.gov.br/jspui/handle/doc/22262](https://rigeo.cprm.gov.br/jspui/handle/doc/22262)  


Grohmann, C.H., 2021. Lagoinha Landslide, São Paulo State, Brazil. UAV Image Collection.  
_GeoNadir.com_    
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://data.geonadir.com/project-details/339](https://data.geonadir.com/project-details/339)  


Grohmann, C.H., 2021. Toque-Toque Grande Landslide, São Paulo State, Brazil. UAV Image Collection.  
_GeoNadir.com_    
Data provided by FAPESP grant [2019/26568-0](https://bv.fapesp.br/52552).  
[https://data.geonadir.com/project-details/339](https://data.geonadir.com/project-details/537)  


Grohmann, C.H., 2020. Garopaba Dune Field, Santa Catarina State, Brazil. UAV Image Collection.  
_GeoNadir.com_    
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://data.geonadir.com/project-details/339](https://data.geonadir.com/project-details/334)  


Grohmann, C.H., Garcia, G.P.B., Affonso, A.A., Albuquerque, R.W., 2019. Garopaba Dune Field, Santa Catarina State, Brazil. TLS point cloud.  
_OpenTopography.org_ opentopoID: OTDS.102019.32722.1  
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://doi.org/10.5069/G9CN7228](https://doi.org/10.5069/G9CN7228)  


Grohmann, C.H., 2019. Garopaba Dune Field, Santa Catarina State, Brazil. SfM-MVS point cloud.  
_OpenTopography.org_ opentopoID: OTDS.072019.32722.1  
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://doi.org/10.5069/G9DV1H19](https://doi.org/10.5069/G9DV1H19)  


Grohmann, C.H., 2019. Lagoinha Landslide, São Paulo State, Brazil. TLS point cloud.  
_OpenTopography.org_ opentopoID: OTDS.082019.32723.2   
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://doi.org/10.5069/G90P0X57](https://doi.org/10.5069/G90P0X57)  


Grohmann, C.H., 2019. Lagoinha Landslide, São Paulo State, Brazil. SfM-MVS point cloud.  
_OpenTopography.org_ opentopoID: OTDS.082019.32723.1  
Data provided by FAPESP grant [2016/06628-0](https://bv.fapesp.br/44264).  
[https://doi.org/10.5069/G94F1NWJ](https://doi.org/10.5069/G94F1NWJ)  


Grohmann, C.H., 2010. Coastal Dune Fields of Garopaba and Vila Nova, Santa Catarina State, Brazil.  
_OpenTopography.org_ opentopoID: OTLAS.032013.32722.1  
Data provided by FAPESP grant [2009/17675-5](https://bv.fapesp.br/7151).  
[https://doi.org/10.5069/G9DN430Z](https://doi.org/10.5069/G9DN430Z)  





















