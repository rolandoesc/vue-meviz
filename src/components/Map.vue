<template>
  <div id="map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :key="marker" v-for="marker in museums" :lat-lng="[marker.gmaps_latitud, marker.gmaps_longitud]">
        <l-popup>
          <div class="container-fluid">
            <div class="row">
              <div class="col-lg-12">
                <h4 class="text-center">{{marker['museo_nombre']}}</h4>
                <p>Ubicación: {{marker['museo_calle_numero']}}, {{marker['museo_colonia']}}, {{marker['nom_loc']}}, {{marker['nom_ent']}}</p>
                <p>Temática: {{marker['museo_tematica_n1']}}</p>


              </div>
            </div>
          </div>
        </l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, L, LPopup } from "vue2-leaflet";
export default {
  name: "Map",
  props: ['museums'],
  // el: "#map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  data: function() {
    return {
      // map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
    };
  },
  mounted: function() {
    this.$nextTick(() => {
      this.museums.map(m => !m['museo_tematica_n1'] ? m['museo_tematica_n1'] = 'TND' : m )
    });
  }
};
</script>

<style scoped lang="scss">
#map {
  height:50vh;
}
</style>