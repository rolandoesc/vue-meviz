<template>
  <div id="map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :key="marker['id']" v-for="marker in filterByTipo" :lat-lng="[marker['lat'], marker['lng']]" :visible="marker['visible']">
        <l-popup>
          <div class="container-fluid">
            <div class="row">
              <div class="col-lg-12">
                <h4 class="text-center">{{marker['nombre']}}</h4>
                <p>Ubicación: {{marker['calle_numero']}}, {{marker['colonia']}}, {{marker['nom_loc']}}, {{marker['nom_ent']}}</p>
                <p>Temática: {{marker['tematica_n1']}}</p>


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
              <div v-for="tipo in topics" :key="tipo">
                <input
                  type="checkbox"
                  :value="tipo"
                  v-model="selected"
                >
                <label for="tipo">{{tipo}}</label>
              </div>
            </div>
          </div>
        </div>
      </l-control>
    </l-map>
    <!-- <button @click="selected">Press</button> -->
    <pre>
      {{ $data.selected | json }}
    </pre>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, L, LPopup, LControl } from "vue2-leaflet";
export default {
  name: "Map",
  props: ['museums'],
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
      topics : ['Historia', 'TND'],
      selected: [],
      selectedMuseums: this.museums
    };
  },
  mounted: function() {
    this.$nextTick(() => {
      this.museums = this.museums.map(m => {
        !m['tematica_n1'] ? m['tematica_n1'] = 'TND' : m;
        return m;
        } )
      this.museums = this.museums.map(m => {m['visible'] = true
        return m
        } )
      return this.museums;
    });
  },
  computed: {
    filterByTipo: function () {
      console.log('computing')
      // return this.selectedMuseums = this.museums.reduce((total, current) => {
      //   for(let topic of this.selected) {
      //     current['tematica_n1'] === topic ? total += current : total;
      //   }
      //   return total
      // }, this.museums)
      // return this.museums.map(m => {
      //   m['tematica_n1'] !== 'Historia' ? m['visible'] = false : m['visible'] = true;
      //   return m
      //   } );
      return this.selectedMuseums
    }
  }
};
</script>

<style scoped lang="scss">
#map {
  height:50vh;
}
</style>