<template>
  <div>
 <l-map ref="myMap">
  </l-map>
  </div>
</template>

<script>
import {LMap, LTileLayer, LMarker } from 'vue2-leaflet';
  export default {
    name: 'Map',
    components: {
      LMap,
      LTileLayer,
      LMarker
    }
    
  }
</script>

<style scoped>

</style>