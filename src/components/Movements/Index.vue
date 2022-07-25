<template>
  <div class="movements">
    <h2 class="title">Historial</h2>
    <div class="content">
      <Movement
        v-for="{ id, title, description, valor, date } in movements"
        :key="id"
        :id="id"
        :title="title"
        :description="description"
        :valor="valor"
        :date="date"
        @remove="eliminar"
      />
    </div>
  </div>
</template>

<script setup>
import { toRefs } from "vue";
import Movement from "@/components/Movements/Movement.vue";

const props = defineProps({
  movements: {
    type: Array,
    default: () => [],
  },
});
const { movements } = toRefs(props);

const emit = defineEmits("eliminar");

const eliminar = (id) => {
  emit("eliminar", id);
};
</script>

<style scoped>
.movements {
  max-height: 100%;
  padding: 0 8px;
  margin-bottom: 14px;
}
.title {
  margin: 8px 16px 24px 16px;
  color: var(--brand-blue);
}
.content {
  max-height: 68vh;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow-y: scroll;
}
</style>