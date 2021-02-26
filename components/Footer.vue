<template>
  <footer>
    <div class="grid_footer">
      <div id="suma">Suma de contadores: {{ contadoresSum }}</div>
      <div id="nuevoContador">
        <ButtonAdd />
      </div>
    </div>
  </footer>
</template>
<script>
export default {
  data() {
    return {
      contadoresSum: 0,
    };
  },
  methods: {
    actualizarSuma() {
      if (localStorage.getItem("contadores")) {
        try {
          this.sumaProv = 0;
          this.contadores = JSON.parse(localStorage.getItem("contadores"));
          this.contadores.forEach((element) => {
            this.sumaProv += element.numero;
          });
          this.contadoresSum = this.sumaProv;
        } catch (e) {
          localStorage.removeItem("contadores");
          this.contadoresSum = 0;
        }
      }
    },
  },
  mounted() {
    this.actualizarSuma();

    this.$root.$on("cambiaSuma", () => {
      this.actualizarSuma();
    });
  },
};
</script>
<style >
footer {
  position: inherit;
  right: 0;
  bottom: 0;
  left: 0;
  padding: 1rem;
  background-color: #efefef;
  text-align: center;
}
.grid_footer {
  display: grid;
  width: 100%;
  grid-template-columns: 50% 50%;
  align-items: center;
}

.grid_footer #nuevoContador {
  text-align: right;
}
</style>