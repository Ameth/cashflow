<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        :label="'Ahorro total'"
        :label-fecha="fechaMov"
        :monto="valor"
        :total-monto="saldoTotal"
      >
        <template #grafico>
          <Graphic :lista="movementFilter" @select="select" />
        </template>
        <template #action> <Action @crear="crear" /> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements @eliminar="eliminarMov" :movements="movement" />
    </template>
  </Layout>
</template>

<script setup>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Graphic from "@/components/Resume/Graphic.vue";
import Action from "@/components/Action.vue";
import Movements from "@/components/Movements/Index.vue";
import { computed, ref, onMounted } from "vue";

const movementFilter = computed(() => {
  const ultimosDias = movement.value
    .filter((item) => {
      const hoy = new Date();
      const oldDate = hoy.setDate(hoy.getDate() - 30);

      return item.date > oldDate;
    })
    .map((item) => {
      return {
        valor: item.valor,
        date: item.date,
      };
    });

  return ultimosDias.map((item, index) => {
    const ultimosMovimientos = ultimosDias.slice(0, index + 1);

    return {
      valor: ultimosMovimientos.reduce((suma, item) => {
        return suma + item.valor;
      }, 0),
      date: item.date,
    };
  });

  // return ultimosDias;

  // return movement.map((item) => item.valor);
});

const valor = ref(null);
const fechaMov = ref(null);
const movement = ref([]);

onMounted(() => {
  const mov = JSON.parse(localStorage.getItem("movements"));
  if (Array.isArray(mov)) {
    movement.value = mov.map((item) => {
      return { ...item, date: new Date(item.date) };
    });
  }
});

const select = (data) => {
  valor.value = data.valor;
  fechaMov.value = data.date ? data.date.toDateString() : null;
};

const crear = (nuevoMov) => {
  movement.value.push(nuevoMov);
  guardar();
};

const eliminarMov = (id) => {
  const index = movement.value.findIndex((item) => item.id === id);
  movement.value.splice(index, 1);
  guardar();
};

const guardar = () => {
  localStorage.setItem("movements", JSON.stringify(movement.value));
};

const saldoTotal = computed(() => {
  return movement.value.reduce((suma, item) => {
    return suma + item.valor;
  }, 0);
});
</script>

<style>
</style>