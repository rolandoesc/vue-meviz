<template>
  <div id="map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="marker"></l-marker>
    </l-map>
    <p>{{museums[0]["museo_nombre"]}}</p>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, L } from "vue2-leaflet";
export default {
  name: "Map",
  props: ['museums'],
  // el: "#map",
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data: function() {
    return {
      // map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(19.432608, -99.133209)
    };
  },
  mounted: function() {
    this.$nextTick(() => {
      // this.map = this.$refs.map.mapObject;
    });
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 45vh;
}
</style>