<template>
  <div class="home">
    <h1>My map</h1>
    <div id="map"></div>
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
}
#map {
  width: 100%;
  height: 500px;
}
</style>

<script>
/* global mapboxgl, mapboxSdk */

// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  data: function() {
    return {
      places: [
        // { lat: -25.363, lng: 131.044, description: "A place in Australia" },
        // { lat: -33.8675, lng: 151.207, description: "The main city!" },
        { address: "215 W Ohio St, Chicago, IL", description: "Actualize Coding Bootcamp" },
        { address: "Navy Pier", description: "A touristy amusement park" },
      ],
    };
  },
  mounted: function() {
    mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_API_KEY;
    var mapboxClient = mapboxSdk({ accessToken: mapboxgl.accessToken });
    var map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [-87.6298, 41.8781],
      zoom: 12,
    });
    this.places.forEach(place => {
      mapboxClient.geocoding
        .forwardGeocode({
          query: place.address,
          autocomplete: false,
          limit: 1,
        })
        .send()
        .then(function(response) {
          if (response && response.body && response.body.features && response.body.features.length) {
            var feature = response.body.features[0];
            var popup = new mapboxgl.Popup({ offset: 25 }).setText(place.description);
            new mapboxgl.Marker()
              .setLngLat(feature.center)
              .setPopup(popup)
              .addTo(map);
          }
        });
    });
  },
};
</script>
