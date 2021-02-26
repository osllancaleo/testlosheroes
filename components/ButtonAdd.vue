<template>
  <div>
    <button type="button" class="btn-outline primary" @click="showModal" :disabled="this.contadoresNum == 20">
      <font-awesome-icon icon="plus" /> 
      <span>Agregar nuevo contador</span>
    </button>
    <modal
      ref="modal"
      v-show="isModalVisible"
      @close="closeModal"
      
    />
  </div>
</template>
<script>
import modal from "/components/Modal.vue";

export default {
  components: {
    modal,
  },
  data() {
    return {
      isModalVisible: false,
      contadoresNum: '',
    };
  },
  mounted() {
     this.$root.$on("totalContadores", (numero) => {
      this.contadoresNum = numero;
    });
  },
  methods: {
    showModal() {
      this.isModalVisible = true;

    },
    closeModal() {
      this.isModalVisible = false;
    },
  },
};
</script>
<style >
@import "assets/styles/buttons.css";

#addContador {
  border: none;
  font: inherit;
  color: inherit;
  text-align: left;
  background-color: #82c91e;
  cursor: pointer;
  padding: 15px 10px 15px 10px;
  border-radius: 5px;
  margin: 5px;
  font-size: 1em;
  width: auto;
}
</style>