<template>
  <div class="container-fluid" id="banner">
    <div class="row">
      <!--  -->
      <div class="col-xs-1 col-sm-3 col-lg-2 offset-lg-2 d-none d-sm-block">
        <img src="../assets/avisado_logo.png" id="logo">
      </div>
      <div class="col-xs-11 col-sm-9 col-lg-5">
        <h1 class="text-left text-uppercase">Museos en México</h1>
        <h2 class="text-left text-uppercase">NUESTRA GUÍA PARA QUE LOS DESCUBRAS</h2>
        <h4 class="text-left">- Por Avisado</h4>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Roboto+Slab");

#banner {
  background-color: #d4145a;
  .row > div {
    h1,
    h4,
    h2 {
      color: white;
      font-weight: bold;
    }
    h2 {
      color: #fcee21;
      font-family: "Open Sans Condensed", serif;
      font-size: 2.5rem;
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
</style>