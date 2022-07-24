<template>
  <div class="movement">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img
        @click="remove"
        src="@/assets/trash-icon.svg"
        alt="borrar"
        title="Borrar"
      />
      <p :class="{ green: valor > 0, red: valor < 0 }">{{ valorFormateado }}</p>
    </div>
  </div>
</template>

<script setup>
import { toRefs, computed } from "vue";
import formatearMoneda from "@/js/currencyFormater";

const props = defineProps({
  id: Number,
  title: String,
  description: String,
  valor: Number,
});

const emit = defineEmits(["remove"]);

const remove = () => {
  console.log(props.id);
  emit("remove", props.id);
};

const valorFormateado = computed(() => {
  return formatearMoneda.format(props.valor);
});

// const { title } = toRefs(props);
</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
  cursor: pointer;
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>