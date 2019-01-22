<template>
  <div id="map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker
        :key="marker['museo_id']"
        v-for="marker in fixedMuseums"
        :lat-lng="[marker.gmaps_latitud, marker.gmaps_longitud]"
        :visible="marker['visible']"
      >
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
      <l-control>
        <div class="container-fluid">
          <div class="row">
            <div class="col-lg-12">
              <h5 class="text-center">Buscar por Categoría</h5>
              <div v-for="tipo in tipos" :key="tipo">
                <input
                  type="checkbox"
                  :name="tipo"
                  :value="tipo"
                  @click="filterByTipo"
                  v-model="selected"
                >
                <label for="tipo">{{tipo}}</label>
              </div>
            </div>
          </div>
        </div>
      </l-control>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, L, LPopup, LControl } from "vue2-leaflet";
export default {
  name: "Map",
  props: ["museums"],
  // el: "#map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LControl
  },
  data: function() {
    return {
      // map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      tipos: ["TND", "Historia"],
      selected: [],
      fixedMuseums: [],
      museumsData: []
    };
  },
  mounted: function() {
    // console.log(this.museums)
    // this.fixedMuseums = this.museumsData
    // .filter(
    //   m =>
    //     m["gmaps_latitud"] !== undefined || m["gmaps_longitud"] !== undefined
    // )
    // .map(m => {
    //   !m["museo_tematica_n1"] ? (m["museo_tematica_n1"] = "TND") : m;
    //   m['visible'] = true;
    //   return m;
    // }
    // );
    // return this.fixedMuseums
    this.fixedMuseums = this.museums;
    this.museumsData = this.museums;
  },
  computed: {
    
  },
  methods: {
    filterByTipo() {
      console.log(this.selected);
      // for (let i = 0; i < this.museumsData.length; i++) {
      //   return this.fixedMuseums.filter(m =>
      //     m["museo_tematica_n1"] === this.selected[i]
      //   );

      // }
    }
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 50vh;
}
</style>