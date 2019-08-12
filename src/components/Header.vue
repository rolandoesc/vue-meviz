<template>
  <v-flex>
    <v-toolbar dark color="#d4145a" id="banner">
      <v-flex class="text-xs-center" id="header-text">
        <!-- <img src="../assets/avisado_logo.png" id="logo"> -->
        <h1 class="text-left text-uppercase">Museos en México</h1>
      </v-flex>
    </v-toolbar>
    <h2 class="subtitle">NUESTRA GUÍA PARA QUE LOS DESCUBRAS</h2>
    <h4 class="text-left">- Por Avisado</h4>
  </v-flex>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Roboto+Slab");

#banner {
  height: auto;
  #header-text {
    h1,
    h4,
    h2 {
      color: white;
      font-weight: bold;
    }

    h1 {
      font-family: "Roboto Slab", serif;
      font-size: 4rem;
    }
    h4 {
      font-style: italic;
      font-size: 1.5rem;
    }
    img#logo {
      max-width: 150px;
      max-height: 150px;
    }
  }
}
h2.subtitle {
  // color: #fcee21;
  color: #d4145a;
  font-family: "Open Sans Condensed", serif;
  font-size: 2.5rem;
}
</style>