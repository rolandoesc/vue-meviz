<template>
  <v-app id="app">
    <v-content>
      <Header />
      <Map :datamuseums="museums" v-if="museums.length > 0" :states="mexican_states"></Map>
      <Footer></Footer>
    </v-content>
  </v-app>
</template>

<script>
// Components
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Map from "./components/Map.vue";
import { initMuseums } from "@/common/firebase-initializer.js";
import mexican_states from '@/common/mexican-states.js'

export default {
  name: "app",
  components: {
    Header,
    Footer,
    Map
  },
  firebase: {
    museums: initMuseums()
  },
  data() {
    return {
      
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
