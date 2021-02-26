<template>
  <div class="container">
    <div class="user-content-callout">
      <div class="grid">
        <div id="nombre">{{ id+1 }}. {{ nombre }}</div>
        <div id="numero">{{ numero }}</div>
        <div id="botones">
          <button type="button" class="btn-outline btn-outline-small primary" :disabled="numero == 0" @click="$emit('disminuir', id)">
            <font-awesome-icon icon="minus-square" size="xs" /> 
            <span>Disminuir</span> 
          </button>
          <button type="button" class="btn-outline btn-outline-small primary" :disabled="numero == 20" @click="$emit('incrementar', id)">
            <font-awesome-icon icon="plus-square" size="xs" /> 
            <span>Aumentar</span> 
          </button>
          <button type="button" class="btn-outline btn-outline-small danger" @click="$emit('removeContador', id)">
            <font-awesome-icon icon="trash-alt" size="xs" /> 
            <span>Eliminar</span> 
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["id", "nombre", "numero"],
  
};
</script>
<style scoped>
.container {
  padding: 0 15px 0 15px;
  display: grid;
  place-items: center;
}
.user-content-callout {
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
  width: 100%;
}

.user-content-callout .grid {
  display: grid;
  width: 100%;
  grid-template-columns: 3fr auto 2fr;
  align-items: center;
}

.user-content-callout #nombre {
  color: #0069ff;
  -ms-flex-positive: 1;
  flex-grow: 1;
  -ms-flex-negative: 1;
  flex-shrink: 1;
  font-size: 0.7em;
  line-height: 1.6;
  margin: 0;
  padding: 0;
  font-weight: bold;
}

.user-content-callout #numero {
  color: #000;
  -ms-flex-positive: 1;
  flex-grow: 1;
  -ms-flex-negative: 1;
  flex-shrink: 1;
  font-size: 0.5em;
  line-height: 1.6;
  padding: 10px;
  border-radius: 3px;
  border: 1px solid grey;
  background-color: #fff;
  text-align: center;
  width: 50px;
}

.user-content-callout #botones {
  padding-bottom: 10px;
}

</style>
