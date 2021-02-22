<template>
  <div class="about">
    <h1>This is a map</h1>
    <div id="map"></div>
  </div>
</template>
<style>
      body { margin: 0; padding: 0; }
      #map { position: absolute; top: 0; bottom: 0; height: 1200px; width: 100%; }
 </style>
<script>

/* global mapboxgl */ 

export default {
  data() {
    return {
      value: null,
      places: [
        { lat: 55.7539, long: 37.6208, desc: "it's a square and it happens to be red" },
        { lat: 55.7520, long: 37.6175, desc: "its an onion building"},
        { lat: 55.7549, long: 37.5734, desc: "its a house that is white"},
      ] 
      
      };
    },
  mounted: function() {
    this.setupMap();
    },
   methods: {
     setupMap: function() {
     mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
            var map = new mapboxgl.Map({
            container: 'map', // container id
            style: 'mapbox://styles/mapbox/satellite-streets-v11', // style URL
            center: [37.6173, 55.7558], // starting position [lng, lat]
            zoom: 9 // starting zoom
            });
            map.on('load', function () {
            map.addSource('mapbox-dem', {
            'type': 'raster-dem',
            'url': 'mapbox://mapbox.mapbox-terrain-dem-v1',
            'tileSize': 512,
            'maxzoom': 14
            });
            // add the DEM source as a terrain layer with exaggerated height
            map.setTerrain({ 'source': 'mapbox-dem', 'exaggeration': 1.5 });
            
            // add a sky layer that will show when the map is highly pitched
            map.addLayer({
            'id': 'sky',
            'type': 'sky',
            'paint': {
            'sky-type': 'atmosphere',
            'sky-atmosphere-sun': [0.0, 0.0],
            'sky-atmosphere-sun-intensity': 15
            }
            });
            var layers = map.getStyle().layers;
 
var labelLayerId;
for (var i = 0; i < layers.length; i++) {
if (layers[i].type === 'symbol' && layers[i].layout['text-field']) {
labelLayerId = layers[i].id;
break;
}
}
 
map.addLayer(
{
'id': '3d-buildings',
'source': 'composite',
'source-layer': 'building',
'filter': ['==', 'extrude', 'true'],
'type': 'fill-extrusion',
'minzoom': 15,
'paint': {
'fill-extrusion-color': '#aaa',
 
// use an 'interpolate' expression to add a smooth transition effect to the
// buildings as the user zooms in
'fill-extrusion-height': [
'interpolate',
['linear'],
['zoom'],
15,
0,
15.05,
['get', 'height']
],
'fill-extrusion-base': [
'interpolate',
['linear'],
['zoom'],
15,
0,
15.05,
['get', 'min_height']
],
'fill-extrusion-opacity': 0.6
}
},
labelLayerId
);
});

            
            this.places.forEach(function(place) {
             var popup = new mapboxgl.Popup({ offset: 25 }).setText(
             place.desc
            );
            
            var marker = new mapboxgl.Marker()
            .setLngLat([place.long, place.lat])
            .setPopup(popup)
            .addTo(map);
            console.log(marker);
            });
            
            
            console.log(map);
            },
            },
            };
</script>