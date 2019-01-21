<template>
  <div class="container-fluid" id="banner">
    <div class="row">
      <div class="col-lg-2 offset-lg-3">
        <img src="../assets/avisado_logo.png" alt="img-fluid">
      </div>
      <div class="col-lg-5">
        <h1 class="text-left text-uppercase">Museos en México</h1>
        <h2 class="text-left text-uppercase">NUESTRA GUÍA PARA QUE LOS DESCUBRAS</h2>
        <h4 class="text-left">- Por Avisado</h4>
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    
  }
</script>

<style scoped lang="scss">
@import url('https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Roboto+Slab');

#banner {
  background-color: #D4145A;
  .row > div {
    h1, h4, h2{
      color: white;
      font-weight: bold;
    }
    h2 {
      color: #FCEE21;
      font-family: 'Open Sans Condensed', serif;
      font-size: 2.5rem;
    }
    h1 {
      font-family: 'Roboto Slab', serif;
      font-size: 4rem;
    }
    h4 {
      font-style: italic;
      font-size: 1.5rem;
    }

  }
}

</style>