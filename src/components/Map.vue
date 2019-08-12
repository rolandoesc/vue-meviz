<template>
  <div>
    <!-- <v-flex xs12 sm10 md8 offset-md2 offset-sm1> -->
    <MapText :datamuseums="datamuseums" />
    <!-- </v-flex> -->
    <v-flex xs12 md8 offset-md2 lg3>
      <v-select
        :items="states"
        item-text="name"
        item-value="name"
        v-model="selected_states"
        label="Filtra por estado"
        solo
        multiple
        chips
        @change="filterByState"
      ></v-select>
    </v-flex>
    <v-flex lg10 offset-lg1 xs12>
      <v-card id="map" class="elevation-0">
        <l-map :zoom="zoom" :center="center">
          <l-tile-layer :url="url" :attribution="attribution" layerType="base"></l-tile-layer>

          <l-layer-group v-for="category in museums" :key="category.name" layer-type="overlay">
            <l-marker
              :key="marker['id']"
              v-for="marker in category.features"
              :lat-lng="marker.coords"
              :icon="markerColor(category.name)"
            >
              <l-tooltip>{{marker['nombre']}}</l-tooltip>
              <l-popup>
                <v-card class="elevation-0">
                  <v-card-title class="pb-0">
                    <h3 class="text-xs-center">{{marker['nombre']}}</h3>
                  </v-card-title>
                  <p>
                    <i class="fas fa-map-marker-alt" :style="{color: getColor(marker['tematica'])}"></i>
                    <!-- {{}} -->
                    &nbsp;Ubicación: {{marker['calle_numero']}}, {{marker['colonia']}}, {{marker['nom_loc']}}, {{marker['nom_ent']}}
                  </p>
                  <p>
                    <i class="fas fa-tag" :style="{color: getColor(marker['tematica'])}"></i>
                    &nbsp;Temática: {{marker['tematica']}}
                  </p>
                </v-card>
              </l-popup>
            </l-marker>
          </l-layer-group>

          <l-control>
            <v-card id="controller">
              <v-flex sm12>
                <v-card-title>
                  <h3 class="text-xs-center">Buscar por categoría</h3>
                </v-card-title>
                <v-checkbox
                  v-for="(category, index) in museums_categories"
                  @change="filterByCategory"
                  class="ma-0 pa-0 small-category-checkbox"
                  :color="museums_categories[index]['hex_color']"
                  v-model="selected"
                  :key="index"
                  :value="category.name"
                  :name="category.name"
                >
                  <template slot="label">
                    <span>{{category.name === 'TND' ? 'Temática no definida' : category.name}}</span>
                  </template>
                  <!-- {{markerColor(category.name)}}-->
                </v-checkbox>
              </v-flex>
            </v-card>
          </l-control>
        </l-map>
      </v-card>
    </v-flex>
  </div>
</template>

<script>
import MapText from "@/components/MapText.vue";
import {
  LMap,
  LTileLayer,
  LMarker,
  L,
  LPopup,
  LTooltip,
  LControl,
  LLayerGroup
} from "vue2-leaflet";
import { filter } from "minimatch";
export default {
  name: "Map",
  props: ["datamuseums", "states"],
  // el: "#map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LControl,
    LTooltip,
    LLayerGroup,
    MapText
  },
  data: function() {
    return {
      // map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      museums_categories: [
        { name: "Historia", checked: false, hex_color: "#FFAD00" },
        { name: "Arqueología", checked: false, hex_color: "#4FA03B" },
        { name: "Ciencia y tecnología", checked: false, hex_color: "#4687C1"  },
        { name: "Arte", checked: false, hex_color: "#C1D415"  },
        { name: "Especializado", checked: false, hex_color: "#C761AD" },
        { name: "Antropología", checked: false, hex_color: "#EE3840" },
        { name: "TND", checked: false, hex_color: "#E0E0E0" }
      ],
      selected: [],
      museums: [],
      selected_states: []
    };
  },
  mounted: function() {
    this.museums = this.datamuseums;
    console.log(this.datamuseums);
    // this.museums_categories.map(el => {
    //   el.checked = true;
    //   return el;
    // });
  },
  methods: {
    filterByCategory($event) {
      console.log($event)
      if ($event.length > 0) {
        let shown_museums = [];
        let filtered = [];
        $event.forEach(category => {
          filtered = this.datamuseums.filter(el => el['name'] === category);
          shown_museums.push(filtered[0])
        })
        return (this.museums = shown_museums);
      } else return (this.museums = this.datamuseums);
    },
    filterByState() {
      const selected_states = [...this.selected_states]
      console.log('esto es museums', this.museums)
      if (this.selected_states.length > 0) {
        let shown_museums = [];
        let filtered_states = [];
        let museums = [...this.datamuseums]
        selected_states.forEach(state => {
          let selected_museums = museums.reduce((acum, curr) => {
            // if (curr.features.filter())
            let current_filtered = curr['features']
              .filter(category => { 
                return category['nom_ent'] === state})
              // console.table(current_filtered)
              curr['features'] = current_filtered;
            return acum = curr
          }, [])
          if (selected_museums.length > 0)
            Array.prototype.push.apply(filtered_states, selected_museums)
        })
        console.log(filtered_states);
        return (this.museums = filtered_states);
      }
      else this.museums = this.datamuseums
    },
    markerColor(color) {
      let cat_color = this.museums_categories.filter(el => el.name === color);
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
    },
    getColor(category) {
      if (!!category) {
        let found_category = this.museums_categories.filter(
          el => el.name === category
        );
        return found_category[0]["hex_color"];
      } else return "E0E0E0";
    }
  },
  computed: {
    items() {
      return this.datamuseums.map(museums => museums['features'])
    }
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 50vh;
  z-index: 0;
}
#controller {
  padding: 1 5px;
  background: #fff;
  opacity: 0.95;
  border: 2px solid rgba(0, 0, 0, 0.2);
  color: #333;
  // .category {
  //   padding: 2px;
  //   text-align: left;

  //   label {
  //     margin-left: 5px;
  //     line-height: 10pt;
  //   }
  // }
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
.small-category-checkbox {
  transform: scale(0.75);
}
</style>