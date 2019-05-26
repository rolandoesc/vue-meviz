<template>
  <!-- <div class="container-fluid">
    <div class="row">
      <div class="col-lg-4 offset-lg-4">
        <h3 class="text-uppercase text-center">
          Visítalos todos
        </h3>
      </div>
    </div>

  </div>-->

  <div class="container-fluid">
    <div class="row justify-content-center">
      <div class="col-lg-6 text-center footer">
        <h3 class="first-line">Visítalos todos</h3>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-lg-6 text-center footer">
        <h4 class="second-line">¡Son tuyos!</h4>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300|Roboto+Slab");

.container-fluid {
  div.row {
    div.footer {
      text-transform: uppercase;
      .first-line {
        color: #4c4c4c;
        font-weight: bold;
      }
      .second-line {
        color: #d4145a;
      }
    }
  }
}
</style>