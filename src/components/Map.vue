<template>
  <div id="map">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution" layerType="base"></l-tile-layer>
      <l-layer-group v-for="category in museums" :key="category.name" layer-type="overlay">
        <l-marker
          :key="marker['id']"
          v-for="marker in category.features"
          :lat-lng="marker.coords"
          :icon="markerColor(category.name)"
        >
          <l-popup>
            <div class="container-fluid">
              <div class="row">
                <div class="col-xs-6 col-lg-12">
                  <h5 class="text-center">{{marker['nombre']}}</h5>
                  <p>
                    <i class="fas fa-map-marker-alt"></i>
                    &nbsp;Ubicación: {{marker['calle_numero']}}, {{marker['colonia']}}, {{marker['nom_loc']}}, {{marker['nom_ent']}}
                  </p>
                  <p>
                    <i class="fas fa-tag"></i>
                    &nbsp;Temática: {{marker['tematica']}}
                  </p>
                </div>
              </div>
            </div>
          </l-popup>
        </l-marker>
      </l-layer-group>

      <l-control>
        <div class="container-fluid">
          <div class="row">
            <div class="col-lg-12" id="controller">
              <h5 class="text-center">Buscar por Categoría</h5>
              <div v-for="(tipo, index) in tipos" :key="index" class="input-group category">
                <input
                  @change="filterByTipo"
                  type="checkbox"
                  :name="tipo.name"
                  :value="tipo.name"
                  v-model="selected"
                >
                <label for="tipo">{{tipo.name}}</label>
              </div>
            </div>
          </div>
        </div>
      </l-control>
    </l-map>
  </div>
</template>

<script>
import {
  LMap,
  LTileLayer,
  LMarker,
  L,
  LPopup,
  LControl,
  LLayerGroup
} from "vue2-leaflet";
export default {
  name: "Map",
  props: ["datamuseums"],
  // el: "#map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LControl,
    LLayerGroup
  },
  data: function() {
    return {
      // map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      tipos: [
        { name: "Historia", checked: false },
        { name: "Arqueología", checked: false },
        { name: "Ciencia y tecnología", checked: false },
        { name: "Arte", checked: false },
        { name: "Especializado", checked: false },
        { name: "Antropología", checked: false },
        { name: "TND", checked: false }
      ],
      selected: [],
      museums: [],
      markersColors: [
        { name: "Historia", hex_color: "#FFAD00" },
        { name: "Arqueología", hex_color: "#4FA03B" },
        { name: "Ciencia y tecnología", hex_color: "#4687C1" },
        { name: "Arte", hex_color: "#C1D415" },
        { name: "Especializado", hex_color: "#C761AD" },
        { name: "Antropología", hex_color: "#EE3840" },
        { name: "TND", hex_color: "#E0E0E0" }
      ]
    };
  },
  mounted: function() {
    this.museums = this.datamuseums;
  },
  methods: {
    filterByTipo({ target }) {
      if (this.selected.length > 0) {
        let shownMuseums = [];
        for (let i = 0; i < this.selected.length; i++) {
          let filtered = this.datamuseums.filter(
            el => el.name === this.selected[i]
          );
          shownMuseums.push(filtered[0]);
        }
        return (this.museums = shownMuseums);
      } else return (this.museums = this.datamuseums);
    },
    markerColor(color) {
      let cat_color = this.markersColors.filter(el => el.name === color);
      const svg = `<?xml version="1.0"?>
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Layer_1" x="0px" y="0px" viewBox="0 0 511.999 511.999" style="enable-background:new 0 0 511.999 511.999;" xml:space="preserve" width="24px" height="24px" class="">
  <g>
    <path style="fill:${
      cat_color[0]["hex_color"]
    }" d="M454.848,198.848c0,159.225-179.751,306.689-179.751,306.689c-10.503,8.617-27.692,8.617-38.195,0  c0,0-179.751-147.464-179.751-306.689C57.153,89.027,146.18,0,256,0S454.848,89.027,454.848,198.848z" data-original="#EE3840" class="active-path"/>
    <path style="fill:#FFFFFF" d="M256,298.89c-55.164,0-100.041-44.879-100.041-100.041S200.838,98.806,256,98.806  s100.041,44.879,100.041,100.041S311.164,298.89,256,298.89z" data-original="#FFE1D6" class="" data-old_color="#ffffff"/>
  </g> 
</svg>
`;
      return L.divIcon({
        className: "marker",
        html: svg,
        iconAnchor: [24, 24],
        popupAnchor: [0, -35]
      });
    }
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 50vh;
}
#controller {
  padding: 1 5px;
  background: #fff;
  opacity: 0.95;
  border: 2px solid rgba(0, 0, 0, 0.2);
  color: #333;
  .category {
    padding: 2px;
    text-align: left;

    label {
      margin-left: 5px;
      line-height: 10pt;
    }
  }
}
@media screen and (max-width: 575.98px) {
  #map {
    height: 100vh !important;
  }
}
@media (min-width: 576px) and (max-width: 767.98px) {
  #map {
    height: 100vh !important;
  }
}
</style>