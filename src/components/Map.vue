<template>
  <div>
    <!-- <v-flex xs12 sm10 md8 offset-md2 offset-sm1> -->
    <!-- <MapText :datamuseums="datamuseums" /> -->
    <!-- </v-flex> -->
    <v-flex xs12 md8 offset-md2 lg3>
      <v-select
        :items="states"
        item-text="state_name"
        item-value="state_id"
        v-model="selected_states"
        label="Filtra por estado"
        solo
        multiple
        chips
      ></v-select>
      <!-- @change="filterByState" -->
    </v-flex>
    <v-flex lg10 offset-lg1 xs12>
      <v-card id="map" class="elevation-0">
        <!-- <l-map :zoom="zoom" :center="center">
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
                </v-checkbox>
              </v-flex>
            </v-card>
          </l-control>
        </l-map>-->
        <v-card id="controller" v-if="false">
          <v-flex sm12>
            <v-card-title>
              <h3 class="text-xs-center">Buscar por categoría</h3>
            </v-card-title>
            <v-checkbox
              v-for="(category, index) in museums_categories"
              class="ma-0 pa-0 small-category-checkbox"
              :color="museums_categories[index]['hex_color']"
              v-model="selected"
              :key="index"
              :value="category.name"
              :name="category.name"
              @change="filterByCategory"
            >
              <template slot="label">
                <span>{{category.name === 'TND' ? 'Temática no definida' : category.name}}</span>
              </template>
            </v-checkbox>
          </v-flex>
        </v-card>
        <div id="map-container"></div>
      </v-card>
    </v-flex>
  </div>
</template>

<script>
import mexican_states from "@/common/mexican-states.ts";

import MapText from "@/components/MapText.vue";
// import {
//   LMap,
//   LTileLayer,
//   LMarker,
//   L,
//   LPopup,
//   LTooltip,
//   LControl,
//   LLayerGroup
// } from "vue2-leaflet";

const setProperty = (category, property) => {
  category = !!category ? category : "TND";
  let categories = {
    Historia: { class: "history", color: "#FFAD00" },
    Arqueología: { class: "archeology", color: "#4FA03B" },
    "Ciencia y tecnología": { class: "science", color: "#4687C1" },
    Arte: { class: "arts", color: "#C1D415" },
    Especializado: { class: "specialized", color: "#C761AD" },
    Antropología: { class: "anthropology", color: "#EE3840" },
    TND: { class: "not-defined", color: "#E0E0E0" }
  };
  return categories[category][property];
};

export default {
  name: "Map",
  props: {
    datamuseums: {
      type: Array,
      required: true
    }
  },
  components: {
    MapText
  },
  data: function() {
    return {
      map: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      museums_categories: [
        { name: "Historia", checked: false, hex_color: "#FFAD00" },
        { name: "Arqueología", checked: false, hex_color: "#4FA03B" },
        { name: "Ciencia y tecnología", checked: false, hex_color: "#4687C1" },
        { name: "Arte", checked: false, hex_color: "#C1D415" },
        { name: "Especializado", checked: false, hex_color: "#C761AD" },
        { name: "Antropología", checked: false, hex_color: "#EE3840" },
        { name: "TND", checked: false, hex_color: "#E0E0E0" }
      ],
      selected: [],
      museums: [],
      states: mexican_states,
      selected_states: [],
      markers: [],
      control_layers: undefined
    };
  },
  methods: {
    filterByCategory($event) {
      console.log($event);
      // if ($event.length > 0) {
      //   let shown_museums = [];
      //   let filtered = [];
      //   $event.forEach(category => {
      //     filtered = this.datamuseums.filter(el => el["name"] === category);
      //     shown_museums.push(filtered[0]);
      //   });
      //   return (this.museums = shown_museums);
      // } else return (this.museums = this.datamuseums);
      this.markers = undefined;
      if ($event.length > 0) {
        let shown_museums = [];
        let filtered = [];
        $event.forEach(category => {
          filtered = this.museums[0]["features"].filter(
            el => el["museo_tematica_n1"] === category
          );
          shown_museums = shown_museums.concat(filtered);
        });
        return this.renderMarkers(shown_museums);
      } else return this.markers(this.museums);
    },
    // filterByState() {
    //   const selected_states = [...this.selected_states];
    //   console.log("esto es museums", this.museums);
    //   if (this.selected_states.length > 0) {
    //     let shown_museums = [];
    //     let filtered_states = [];
    //     let museums = [...this.datamuseums];
    //     selected_states.forEach(state => {
    //       let selected_museums = museums.reduce((acum, curr) => {
    //         // if (curr.features.filter())
    //         let current_filtered = curr["features"].filter(category => {
    //           return category["nom_ent"] === state;
    //         });
    //         // console.table(current_filtered)
    //         curr["features"] = current_filtered;
    //         return (acum = curr);
    //       }, []);
    //       if (selected_museums.length > 0)
    //         Array.prototype.push.apply(filtered_states, selected_museums);
    //     });
    //     console.log(filtered_states);
    //     return (this.museums = filtered_states);
    //   } else this.museums = this.datamuseums;
    // },
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
      // return L.divIcon({
      //   className: "marker",
      //   html: svg,
      //   iconAnchor: [24, 24],
      //   // popupAnchor: [0, -35]
      // });
      let icon_url = 'data:image/svg+xml;base64,' + btoa(svg);
      return L.icon({
          iconUrl: icon_url,
          iconSize: [24,24]
        })
    },
    getColor(category) {
      if (!!category) {
        let found_category = this.museums_categories.filter(
          el => el.name === category
        );
        return found_category[0]["hex_color"];
      } else return "E0E0E0";
    },
    initMap() {
      this.map = L.map("map-container").setView([19.432608, -99.133209], 13);

      L.tileLayer(
        "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token="+process.env.VUE_APP_MAPBOX_TOKEN,
        {
          attribution:
            'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
          maxZoom: 18,
          id: "mapbox/streets-v11",
          accessToken:
            process.env.VUE_APP_MAPBOX_TOKEN
        }
      ).addTo(this.map);

      this.control_layers = L.control
        .layers(null, null, {
          position: "bottomright",
          collapsed: false
        })
        .addTo(this.map);
      this.museums.forEach(state => {
        this.renderMarkers(state["features"], state['name']);
      });
      // this.createPane();
    },
    renderMarkers(museums_array, name=false) {
      let markers = [...museums_array];
      // console.log('museums', this.museums)
      let layer_group = new L.LayerGroup().addTo(this.map)
      if (name === 'Ciudad de México') layer_group.addTo(this.map)
      console.log(name)
      this.control_layers.addOverlay(layer_group, name)
      this.markers = markers.map(marker => {
        let coords = [marker["gmaps_latitud"], marker["gmaps_longitud"]];
        let marker_icon = this.markerColor(marker['museo_tematica_n1'])
        let current_marker = L.marker(coords, {icon: marker_icon}).addTo(/*this.map*/layer_group);
        // L.DomUtil.addClass(current_marker, setProperty(marker['museo_tematica_n1'], 'class'));
        this.popupDetails(current_marker, marker);
        return current_marker;
      });
      return this.markers;
    },
    popupDetails(
      target_marker,
      {
        museo_nombre,
        museo_tematica_n1,
        museo_colonia,
        museo_calle_numero,
        nom_loc,
        nom_ent
      }
    ) {
      let template = `
        <v-card class="elevation-0">
          <v-card-title class="pb-0">
            <h3 class="text-xs-center">${museo_nombre}</h3>
          </v-card-title>
          <p >
            <i class="fas fa-map-marker-alt" style="color: ${setProperty(
              museo_tematica_n1,
              "color"
            )};">
            </i>
              &nbsp;Ubicación: ${museo_calle_numero}, ${museo_colonia}, ${nom_loc}, ${nom_ent}
          </p>
          <p>
            <i class="fas fa-tag" style="color: ${setProperty(
              museo_tematica_n1,
              "color"
            )};" ></i>
              &nbsp;Temática: ${museo_tematica_n1}
          </p>
        </v-card>
      `;
      // :style="{color: ${this.getColor(museo_tematica_n1)}}"
      return target_marker.bindPopup(template);
    },
    createPane() {
      let pane_template = `
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
                </v-checkbox>
              </v-flex>
            </v-card>
      `;

      var customControl = L.Control.extend({
        options: {
          position: "topright"
        },

        onAdd: function(map) {
          var container = L.DomUtil.create(
            "div",
            "leaflet-bar leaflet-control leaflet-control-custom"
          );

          container.style.backgroundColor = "white";
          container.value = pane_template;
          let controller = document.getElementById("controller");
          container.appendChild(controller);
          return container;
        }
      });
      return this.map.addControl(new customControl());
    }
  },
  mounted() {
    this.museums = this.datamuseums;
    console.log(mexican_states);
    // let states = this.datamuseums.map((feature, index) => {
    //   let format = {
    //     state_name: feature['name'],
    //     state_id: index
    //   }
    //   return format
    // })
    // console.log(states)
    // let segmented_museums = this.museums[0]['features'].reduce((total_museums, current_museum) => {
    //   if (!total_museums[current_museum['nom_ent']])
    //     total_museums[current_museum['nom_ent']] = []
    //   let museum = current_museum;
    //   if (!museum['museo_tematica_n1'])
    //     museum['museo_tematica_n1'] = 'TND'

    //   total_museums[current_museum['nom_ent']].push(museum)
    //   return total_museums
    // }, {})
    // let segmented_states_json = Object.keys(segmented_museums).map((key, index) => {
    //   let new_json = {
    //     id: index,
    //     active: undefined,
    //     name: key,
    //     features: segmented_museums[key]
    //   }
    //   return new_json
    // })
    // console.log(segmented_states_json)
    this.initMap();
    // console.log(this.map);
    // console.log(L);
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 50vh;
  z-index: 0;
  #map-container {
    height: 100%;
  }
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
    #map-container {
      height: 200px;
    }
  }
}
.small-category-checkbox {
  transform: scale(0.75);
}

/*


*/
.history {
  color: #ffad00 !important;
  background-color: #ffad00 !important;
}

.archeology {
  color: #4fa03b !important;
  background-color: #4fa03b !important;
}

.science {
  color: #4687c1 !important;
  background-color: #4687c1 !important;
}

.arts {
  color: #c1d415 !important;
  background-color: #c1d415 !important;
}

.specialized {
  color: #c761ad !important;
  background-color: #c761ad !important;
}

.anthropology {
  color: #ee3840 !important;
  background-color: #ee3840 !important;
}

.not-defined {
  color: #e0e0e0 !important;
  background-color: #e0e0e0 !important;
}
</style>