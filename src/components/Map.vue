<template>
  <div class="container mt-3 mt-sm-5" id="app">
    <div class="row">
      <div class="col-md-9">
        <div class="map" id="map"></div>
      </div>
      <div class="col-md-3">
        <div class="form-check" v-for="layer in museumsData" :key="layer.id">
          <label class="form-check-label">
            <input
              class="form-check-input"
              type="checkbox"
              v-model="layer.active"
              @change="layerChanged(layer.id, layer.active)"
            >
            {{ layer.name }}
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, L, LPopup, LControl } from "vue2-leaflet";
export default {
  name: "Map",
  props: {
    museums: {
      type: Array
    }
  },
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
      map: null,
      tileLayer: null,
      zoom: 13,
      center: L.latLng(19.432608, -99.133209),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      topics: ["Historia", "TND"],
      selected: [],
      museumsData: [],
      layers: this.museums
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.museumsData = this.museums;
      this.initMap();
      this.initLayers();
      // this.editData();
    });
  },
  methods: {
    layerChanged(layerId, active) {
      const layer = this.layers.find(layer => layer.id === layerId);
      console.log(layer);
      layer.features.forEach(feature => {
        if (active) {
          feature.leafletObject.addTo(this.map);
        } else {
          feature.leafletObject.removeFrom(this.map);
        }
      });
    },
    initLayers() {
      // this.museumsData.forEach(layer => {
      //   layer["museums"].forEach(feature => {
      //     // L.marker([feature.lat, feature.lng]).bindPopup(feature.nombre);
      //     this.addMarker(feature);
      //   });
      //   console.log(layer);
      // });

      this.layers.forEach(layer => {
        const markerFeatures = layer.features.filter(
          feature => feature.type === "marker"
        );
        markerFeatures.forEach(feature => {
          feature.leafletObject = L.marker(feature.coords).bindPopup(
            feature.nombre
          );
        });
      });
    },
    initMap() {
      this.map = L.map("map").setView([19.432608, -99.133209], 12);
      this.tileLayer = L.tileLayer(
        "https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png",
        {
          maxZoom: 18,
          attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>'
        }
      );

      this.tileLayer.addTo(this.map);
    },
    addMarker(el) {
      let newMarker = new L.marker([el.lat, el.lng]).addTo(this.map);
    }
    // editData() {
    //   console.log(this.museums);
    //   let fixedMuseums = this.museums.map(category => {
    //     category["features"] = category["features"].map((museum, index) => {
    //       museum["id"] = index;
    //       museum["type"] = "marker";
    //       museum["coords"] = [museum["lat"], museum["lng"]];
    //       delete museum["oldID"];
    //       delete museum["lat"];
    //       delete museum["lng"];
    //       return museum;
    //     });
    //     return category;
    //   });
    //   console.log(fixedMuseums);
    // }
  }
};
</script>

<style scoped lang="scss">
#map {
  height: 50vh;
}
</style>