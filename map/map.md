---
layout: page
title: Map
subtitle: 
published: true
---
Map created with <a href="http://leafletjs.com" target="_blank">Leaflet</a>, using base layer data from <a href="https://www.mapbox.com" target="_blank">MapBox</a>.
There's a <a href="https://carlosgrohmann.com/blog/making-a-where-ive-been-map-with-leaflet/" target="_blank">blog post</a> explaining how to make this map.

For a more immersive experience, there's a full-screen version <a href="{{site.baseurl}}/map/map_full.html" target="_blank">here</a> (opens in a new tab/window).
&nbsp;
&nbsp;
<div>
<!-- <h4>Where we go to work while you're at the beach</h4> -->

<!-- JQuery -->
<script src="{{site.baseurl}}/map/jquery-3.4.1.min.js"></script>

<!-- Leaflet stuff -->
<link rel="stylesheet" href="{{site.baseurl}}/map/leaflet.css" />
<script src="{{site.baseurl}}/map/leaflet.js"></script>
<script src="{{site.baseurl}}/map/leaflet-providers.js"></script>

<!-- Leaflet Label plugin -->
<!-- <script src='{{site.baseurl}}/map/leaflet.label.js'></script> -->
<!-- <link href='{{site.baseurl}}/map/leaflet.label.css' rel='stylesheet' /> -->

<!-- /* LeafLet map props*/ -->
<style type="text/css">
#map { height: 450px; width: 650px;}
</style>

<!-- LeafLet map  - relative link -->
<div id="map"></div>
<!-- places.geojson -->
<link rel="points" type="application/json" href='{{site.baseurl}}/map/spam_places.geojson'>

<script>
    //  free tile providers, from lafletf.providers plugin
    var OpenTopoMap = L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
        maxZoom: 17,
        attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)'
    });

    var OpenStreetMap_Mapnik = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    });

    var Esri_WorldStreetMap = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Source: Esri, DeLorme, NAVTEQ, USGS, Intermap, iPC, NRCAN, Esri Japan, METI, Esri China (Hong Kong), Esri (Thailand), TomTom, 2012'
    });

    var Esri_WorldTopoMap = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Esri, DeLorme, NAVTEQ, TomTom, Intermap, iPC, USGS, FAO, NPS, NRCAN, GeoBase, Kadaster NL, Ordnance Survey, Esri Japan, METI, Esri China (Hong Kong), and the GIS User Community'
    });

    var Esri_WorldImagery = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
    });

    var Esri_WorldShadedRelief = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Shaded_Relief/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; Source: Esri',
        maxZoom: 13
    });


    // MapBox Terrain - zooms 5-18
    MBTerrain = L.tileLayer('http://{s}.tiles.mapbox.com/v3/carlosgrohmann.ibb4756i/{z}/{x}/{y}.png', {
            maxZoom: 18,
            minZoom: 0,
            attribution: '&copy; Tiles Courtesy of <a href="https://www.mapbox.com" title="MapBox" target="_blank">MapBox</a>',
    });  

    // color itens acording to properties
    function getColor(category) {
        return category == "meeting"  ?   '#002E63' : 
               category == "research" ?   '#FF7E00' :
                                          '#000';
    }

    var places = L.layerGroup();

    // Attaching a GeoJSON file with relative link: (from: http://lyzidiamond.com/posts/osgeo-august-meeting/)
      $.getJSON($('link[rel="points"]').attr("href"), function(data) {
        var geoJsonLayer = L.geoJson(data, {
            onEachFeature: function (feature, layer) {
                //https://plnkr.co/edit/TwAuI90IIJzyEJU8iGSu?p=preview
                var div_element = document.createElement("div");
                var p_element = document.createElement("p");
                // p_element.innerHTML = "Theme: " + feature.properties.Theme +"<br>"
                p_element.innerHTML = "Project: " + feature.properties.Proj_title +"<br>"
                p_element.innerHTML += "Spammer: " + feature.properties.Proj_person +"<br>"
                var a_element = document.createElement("a");
                a_element.href = feature.properties.Proj_link
                a_element.target = "_blank"
                a_element.innerHTML = "More info"
                p_element.appendChild(a_element)
                div_element.appendChild(p_element)
                // var content = feature.properties.Proj_title + '<br>' + feature.properties.Proj_person + '<br>' + p_element
                // var desc = feature.properties.Title
                // layer.bindLabel(content)
                layer.bindPopup(div_element)
            },
            pointToLayer: function (feature, latlng) {
                return L.circleMarker(latlng, {
                radius: 5,
                Label: getColor(feature.properties.Title), 
                fillColor: getColor(feature.properties.category), 
                color: "#000",
                weight: 0.5,
                opacity: 0.8,
                fillOpacity: 0.7,})
            },
        });
        geoJsonLayer.addTo(places);
      });

      //vars for the layer control
    var otopo = OpenTopoMap,
        osm  =  OpenStreetMap_Mapnik,
        ewsm = Esri_WorldStreetMap,
        ewtm = Esri_WorldTopoMap,
        ewim = Esri_WorldImagery,
        ewsr = Esri_WorldShadedRelief,
        gmbt = MBTerrain;


    var map = L.map('map', {
        center: [-15, -55],
        zoom: 4,
        layers: [gmbt, places]
    });

    var baseLayers = {
        "MapBox Terrain": gmbt,
        "OpenTopo Map": otopo, 
        "OpenStreetMap Mapnik": osm, 
        "Esri WorldStreetMap": ewsm,
        "Esri WorldTopoMap": ewtm,
        "Esri WorldImagery": ewim,
        "Esri WorldShadedRelief": ewsr
    };

    var overlays = {
        "Study areas": places
    };

    L.control.layers(baseLayers, overlays).addTo(map);


    // create map
    // var map = L.map('map').setView([-15, -55], 4);


    // L.tileLayer.provider('OpenTopoMap').addTo(map);




</script>

&nbsp;
&nbsp;
&nbsp;

<!-- 
<script>
    // create map
    var map = L.map('map').setView([-15, -55], 4);
    // MapBox Terrain - zooms 5-18
    MBTerrain = L.tileLayer('http://{s}.tiles.mapbox.com/v3/carlosgrohmann.ibb4756i/{z}/{x}/{y}.png', {
            maxZoom: 18,
            minZoom: 2,
            attribution: '&copy; Tiles Courtesy of <a href="https://www.mapbox.com" title="MapBox" target="_blank">MapBox</a>',
            }).addTo(map);                
    // color itens coording to properties
    function getColor(category) {
        return category == "meeting"  ?   '#002E63' : 
               category == "research" ?   '#FF7E00' :
                                          '#000';
    }
    // Attaching a GeoJSON file with relative link: (from: http://lyzidiamond.com/posts/osgeo-august-meeting/)
      $.getJSON($('link[rel="points"]').attr("href"), function(data) {
        var geoJsonLayer = L.geoJson(data, {
            onEachFeature: function (feature, layer) {
                var content = feature.properties.Area + '<br>' + feature.properties.Title + '<br>'
                // var desc = feature.properties.Title
                layer.bindLabel(content)
            },
            pointToLayer: function (feature, latlng) {
                return L.circleMarker(latlng, {
                radius: 3,
                Label: getColor(feature.properties.Title), 
                fillColor: getColor(feature.properties.category), 
                color: "#000",
                weight: 0.5,
                opacity: 0.8,
                fillOpacity: 0.8,})
            },
        });
        geoJsonLayer.addTo(map);
      });
</script>
 -->