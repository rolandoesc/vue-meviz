<template>
  <div class="offset-lg-1 col-lg-10 col-md-6 col-sm-6 justify-content-center">
    <p class="text-center">
      En México hay un total de {{total}} museos dedicados a una gran variedad de temas. 7 en total.
      La finalidad de este proyecto es conocer todos los museos del territorio mexicano,
      su ubicación y categoría.
      La base de datos utilizada fue por parte del Sistema de Información Cultural (SIC), una extensión
      de la secretaría de cultura.
    </p>
  </div>
</template>

<script>
export default {
  prosp: {
    datamuseums: {
      type: Array,
      required: false,
      default: 0
    }
  },
  data() {
    return {
      total: 0,
      categories: undefined
    };
  },
  methods: {
    totalMuseums() {
      let total = 0;
      for (let i = 0; i < this.categories; i++) {
        console.log(this.categories);
        total = total + this.categories[i]["features"].length;
      }
      return (this.total = total);
    }
  },
  mounted() {
    this.categories = this.datamuseums;
    this.totalMuseums();
  }
};
</script>

<style lang="scss" scoped>
</style>