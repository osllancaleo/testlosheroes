<template>
  <section>
    <div id="zeroContador" v-if="contadores.length == 0">
      No existen contadores creados<br/>
      <ButtonAdd />
    </div>
    <Contador
      v-for="(contador, index) in contadores"
      :key="index"
      :id="index"
      :nombre="contador.nombre"
      :numero="contador.numero"
      @removeContador="removeContadorFinal"
      @incrementar="incrementarFinal"
      @disminuir="disminuirFinal"
    />
  </section>
</template>

<script>
export default {
  data: () => ({
    contadores: [],
  }),
  mounted() {
    console.log("App mounted!");
    if (localStorage.getItem("contadores")) {
      this.contadores = JSON.parse(localStorage.getItem("contadores"));
      console.log(this.contadores);
    }
    this.$root.$on("agregarContador", (contadores) => {
      this.contadores = contadores;
    });
  },
  methods: {
    removeContadorFinal: function (id) {
      this.contadores = JSON.parse(localStorage.getItem("contadores"));
      this.contadores.splice(id, 1);
      const parsed = JSON.stringify(this.contadores);
      localStorage.setItem("contadores", parsed);
      this.$root.$emit('cambiaSuma');
      this.$root.$emit("totalContadores", this.contadores.length);
    },

    incrementarFinal: function (id) {
      this.contadores = JSON.parse(localStorage.getItem("contadores"));
      this.contadores[id].numero++;
      const parsed = JSON.stringify(this.contadores);
      localStorage.setItem("contadores", parsed);
      this.$root.$emit('cambiaSuma');
    },
    disminuirFinal: function (id) {
      this.contadores = JSON.parse(localStorage.getItem("contadores"));
      this.contadores[id].numero--;
      const parsed = JSON.stringify(this.contadores);
      localStorage.setItem("contadores", parsed);
      this.$root.$emit('cambiaSuma');

    },
  },
};
</script>

<style scoped>
.container {
  margin: 0 auto;
  display: grid;
  justify-content: center;
  align-items: center;
  grid-template-rows: auto 1fr auto;
  padding: 10rem;
}

#zeroContador {
  -ms-flex-align: center;
  align-items: center;
  background: rgba(0, 105, 255, 0.05);
  border: 1px solid #0069ff;
  border-radius: 3px;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-direction: row;
  flex-direction: row;
  -ms-flex-wrap: nowrap;
  flex-wrap: nowrap;
  font-size: 24px;
  margin: 15px 0;
  padding: 16px 24px;
  width: 80%;
}
</style>
