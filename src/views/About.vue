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
            style: 'mapbox://styles/mapbox/streets-v11', // style URL
            center: [37.6173, 55.7558], // starting position [lng, lat]
            zoom: 9 // starting zoom
            });
            var popup = new mapboxgl.Popup({ offset: 25 }).setText(
            'Construction on the Washington Monument began in 1848.'
            );
            var el = document.createElement('div');
            el.id = 'marker';
            var marker = new mapboxgl.Marker()
            .setLngLat([37.6208, 55.7539])
            .setPopup(popup)
            .addTo(map);
            console.log(marker);
            console.log(map);
            },
            },
            };
</script>