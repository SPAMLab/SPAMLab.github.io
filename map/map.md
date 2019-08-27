---
layout: page
title: Map
subtitle: 
published: true
---
Map created with <a href="http://leafletjs.com" target="_blank">Leaflet</a>, using base layer data from <a href="https://www.mapbox.com" target="_blank">MapBox</a>.
There's a <a href="https://carlosgrohmann.com/blog/making-a-where-ive-been-map-with-leaflet/" target="_blank">blog post</a> explaining how to make this map.

For a more immersive experience, there's a full-screen version <a href="/map_full.html" target="_blank">here</a> (opens in a new tab/window).
&nbsp;
&nbsp;
<div>
<!-- <h4>Where we go to work while you're at the beach</h4> -->

<!-- JQuery -->
<script src="http://code.jquery.com/jquery-1.10.2.min.js"></script>

<!-- Leaflet stuff -->
<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css" />
<script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"></script>

<!-- Leaflet Label plugin -->
<script src='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox.js/plugins/leaflet-label/v0.2.1/leaflet.label.css' rel='stylesheet' />

<!-- /* LeafLet map props*/ -->
<style type="text/css">
#map { height: 450px; width: 650px;}
</style>

<!-- LeafLet map  - relative link -->
<div id="map"></div>
<!-- places.geojson -->
<link rel="points" type="application/json" href='{{site.baseurl}}/map/spam_places.geojson'>

<script>
    // create map
    var map = L.map('map').setView([-15, -55], 4);

    // MapBox Terrain - zooms 5-18
    MBTerrain = L.tileLayer('http://{s}.tiles.mapbox.com/v3/carlosgrohmann.ibb4756i/{z}/{x}/{y}.png', {
            maxZoom: 18,
            minZoom: 0,
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