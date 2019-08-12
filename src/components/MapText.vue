<template>
  <v-flex pa-2 xs12 sm10 md8 lg6 offset-md2 offset-sm1>
    <v-card>
      <v-card-title class="pb-0">
        <h3>¿Qué debo entender de esta visualización?</h3>
      </v-card-title>
      <v-card-text>
        <ul class="text-xs-left">
          <li>
            En México hay un total de
            <b>{{total}}</b>
            museos dedicados a una gran variedad de temas. Lo agrupamos en {{datamuseums.length}} categorías.
          </li>
          <li>
            La finalidad de este proyecto es conocer todos los museos del territorio mexicano,
            su ubicación y especialización.
          </li>
          <li>
            La base de datos utilizada fue por parte del
            <a
              href="https://sic.cultura.gob.mx/"
              target="_blank"
            >Sistema de Información Cultural (SIC)</a>, una extensión
            de la secretaría de cultura.
          </li>
        </ul>
      </v-card-text>
      <p class="text-xs-left"></p>
    </v-card>
  </v-flex>
</template>

<script>
export default {
  props: {
    datamuseums: {
      type: Array,
      required: false
    }
  },
  data() {
    return {
      categories: undefined
    };
  },
  methods: {},
  computed: {
    total() {
      let total = 0;
      for (let i = 0; i < this.datamuseums.length; i++) {
        total = total + this.datamuseums[i]["features"].length;
      }
      return total;
    }
  },
  mounted() {
    this.categories = this.datamuseums;
  }
};
</script>

<style lang="scss" scoped>
</style>