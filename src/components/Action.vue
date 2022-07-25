<template>
  <button @click="showModal = true">Agregar movimiento</button>
  <teleport to="#app">
    <Modal v-show="showModal" @cerrar="showModal = false">
      <form @submit.prevent="submit">
        <div class="field">
          <label for="titulo">Titulo</label>
          <input
            type="text"
            name="titulo"
            id="titulo"
            v-model="title"
            autocomplete="off"
          />
        </div>
        <div class="field">
          <label for="valor">Valor</label>
          <input
            type="number"
            name="valor"
            id="valor"
            v-model="valor"
            autocomplete="off"
          />
        </div>
        <div class="field">
          <label for="descripcion">Descripción</label>
          <textarea
            name="descripcion"
            id="descripcion"
            rows="4"
            v-model="description"
          ></textarea>
        </div>
        <div class="field">
          <label for="tipo-movimiento">Tipo de movimiento</label>
          <label class="radio-label">
            <input
              type="radio"
              name="tipo-movimiento"
              id="tipo-movimiento-ingreso"
              v-model="movType"
              value="Ingreso"
            />
            <span>Ingreso</span>
          </label>
          <label class="radio-label">
            <input
              type="radio"
              name="tipo-movimiento"
              id="tipo-movimiento-gasto"
              v-model="movType"
              value="Gasto"
            />
            <span>Gasto</span>
          </label>
        </div>
        <div class="action">
          <button>Agregar movimiento</button>
        </div>
      </form>
    </Modal>
  </teleport>
</template>

<script setup>
import { ref } from "vue";
import Modal from "@/components/Modal.vue";

const showModal = ref(false);
const title = ref("");
const valor = ref(0);
const description = ref("");
const movType = ref("Ingreso");

const emit = defineEmits(["crear"]);

const submit = () => {
  showModal.value = false;
  emit("crear", {
    title: title.value,
    description: description.value,
    valor: movType.value === "Ingreso" ? valor.value : -valor.value,
    date: new Date(),
    id: new Date().getTime(),
  });

  title.value = "";
  valor.value = 0;
  description.value = "";
  movType.value = "Ingreso";
};
</script>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
  cursor: pointer;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>