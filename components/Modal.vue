<template>
  <ValidationObserver v-slot="{ invalid }" ref="form">
    <form @submit.prevent="onSubmit">
      <transition name="modal-fade">
        <div class="modal-backdrop">
          <div
            class="modal"
            role="dialog"
            aria-labelledby="modalTitle"
            aria-describedby="modalDescription"
          >
            <div class="modal-header" id="modalTitle">
              <slot name="headerModal"
                >Agregar nuevo contador
                <button
                  type="button"
                  class="btn-close"
                  @click="close"
                  aria-label="Close modal"
                >
                  x
                </button>
              </slot>
            </div>
            <div class="modal-body" id="modalDescription">
              <slot name="bodyModal">
                <div style="float: left; margin-left: 20px">
                  <ValidationProvider
                    name="Nombre Contador"
                    rules="required|alpha_num|max:20"
                    v-slot="{ errors }"
                  >
                    <label>Nombre del contador:</label>
                    <input v-model="contadorNombre" class="input" type="text" />
                    <span id="error">{{ errors[0] }}</span>
                  </ValidationProvider>
                </div>
              </slot>
            </div>
            <div class="modal-footer">
              <slot name="footerModal">
                <button
                  type="button"
                  class="btn-outline default"
                  @click="close"
                  aria-label="Cancelar"
                >
                  <font-awesome-icon icon="times-circle" />
                  <span>Cancelar!</span>
                </button>
                <button
                  type="submit"
                  :disabled="invalid"
                  class="btn-outline success"
                  aria-label="Agregar contador"
                >
                  <font-awesome-icon icon="check-circle" />
                  <span>Agregar contador</span>
                </button>
              </slot>
            </div>
          </div>
        </div>
      </transition>
    </form>
  </ValidationObserver>
</template>
<script>
import { ValidationProvider } from "vee-validate";

export default {
  name: "modal",
  components: {
    ValidationProvider,
  },
  data: () => ({
    contadorNombre: "",
    contadores: [],
  }),
  mounted() {
    if (localStorage.getItem("contadores")) {
      try {
        this.contadores = JSON.parse(localStorage.getItem("contadores"));
      } catch (e) {
        localStorage.removeItem("contadores");
      }
    }
    this.$root.$on("saveContadorOutside", () => {
      return this.saveContador();
    });
  },
  methods: {
    close() {
      this.contadorNombre = null;
      this.$nextTick(() => {
        this.$refs.form.reset();
      });
      this.$emit("close");
    },

    onSubmit() {
      if (localStorage.getItem("contadores")) {
        try {
          this.contadores = JSON.parse(localStorage.getItem("contadores"));
        } catch (e) {
          localStorage.removeItem("contadores");
          this.contadores = [];
        }
      }
      this.contadores.push({ nombre: this.contadorNombre, numero: 0 });
      this.saveContador();
      this.close();
    },

    saveContador() {
      const parsed = JSON.stringify(this.contadores);
      localStorage.setItem("contadores", parsed);
      this.$root.$emit("agregarContador", this.contadores);
      this.$root.$emit("totalContadores", this.contadores.length);
    },

    updateMessage: async function () {
      this.contadores = "updated";
      console.log(this.$el.textContent); // => 'not updated'
      await this.$nextTick();
      console.log(this.$el.textContent); // => 'updated'
    },
  },
};
</script>
<style scoped>
@import "assets/styles/buttons.css";

*,
*::before,
*::after {
  box-sizing: border-box;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}

.modal-header,
.modal-footer {
  padding: 15px;
  display: flex;
}

.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: #4aae9b;
  justify-content: space-between;
  align-items: center;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
  text-align: left;
}

.btn-close {
  border: none;
  font-size: 20px;
  padding: 5px;
  cursor: pointer;
  font-weight: bold;
  color: #4aae9b;
  background: transparent;
}

.input {
  font-size: 12px;
  font-size: Max(12px, 1em);
  font-family: inherit;
  padding: 0.25em 0.5em;
  background-color: #fff;
  border: 2px solid #8b8a8b;
  border-radius: 4px;
  transition: 180ms box-shadow ease-in-out;
}

.input:focus {
  border-color: #0069ff;
  box-shadow: 0 0 0 3px hsl(215, 100%, 90%, 0.8);
  outline: 3px solid transparent;
}

.input:not(textarea) {
  line-height: 1;
  height: 2.25rem;
}

label {
  font-size: 12px;
  font-weight: 500;
  line-height: 1;
}
input,
label {
  display: block;
}
#error {
  color: #ff2323;
  font-size: 10px;
}
</style>