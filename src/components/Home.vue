<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        :label="'Ahorro total'"
        label-fecha="23 de julio 2022"
        :monto="valor"
        :total-monto="700000"
      >
        <template #grafico> <Graphic :lista="movementFilter" /> </template>
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
import { computed, ref } from "vue";

const movementFilter = computed(() => {
  const ultimosDias = movement.value
    .filter((item) => {
      const hoy = new Date();
      const oldDate = hoy.setDate(hoy.getDate() - 30);

      return item.date > oldDate;
    })
    .map((item) => item.valor);

  return ultimosDias.map((item, index) => {
    const ultimosMovimientos = ultimosDias.slice(0, index);

    return ultimosMovimientos.reduce((suma, valor) => {
      return suma + valor;
    }, 0);
  });

  // return ultimosDias;

  // return movement.map((item) => item.valor);
});

let valor = null;
const movement = ref([
  {
    id: 1,
    title: "Compra en HomeCenter",
    description: "Compra de articulos de aseo",
    valor: -100000,
    date: new Date("06/06/2022"),
  },
  {
    id: 2,
    title: "Compra en Parque Central",
    description: "Compra de Celular",
    valor: -450000,
    date: new Date("06/15/2022"),
  },
  {
    id: 3,
    title: "Compra en FastFood",
    description: "Compra de Comida",
    valor: -60000,
    date: new Date("06/20/2022"),
  },
  {
    id: 4,
    title: "Compra en Porthos",
    description: "Compra de Hamburguesas 🍔",
    valor: -31000,
    date: new Date("07/02/2022"),
  },
  {
    id: 5,
    title: "Consignacion",
    description: "Transferencia de pago de Nomina",
    valor: 750000,
    date: new Date("07/03/2022"),
  },
  {
    id: 6,
    title: "Compra en Long Hang",
    description: "Compra de comida china",
    valor: -19000,
    date: new Date("07/05/2022"),
  },
  {
    id: 7,
    title: "Compra en Spotify",
    description: "Pago mensual del servicio",
    valor: -24900,
    date: new Date("07/10/2022"),
  },
  {
    id: 8,
    title: "Consignacion",
    description: "Transferencia de Britney Ordoñez",
    valor: 20000,
    date: new Date("07/01/2022"),
  },
  {
    id: 9,
    title: "Compra en Netflix",
    description: "Pago de servicio mensual",
    valor: -29900,
    date: new Date("07/10/2022"),
  },
  {
    id: 10,
    title: "Consignacion",
    description: "Transferencia de Victor Cervantes",
    valor: 50000,
    date: new Date("07/20/2022"),
  },
]);

const crear = (nuevoMov) => {
  movement.value.push(nuevoMov);
};

const eliminarMov = (id) => {
  const index = movement.value.findIndex((item) => item.id === id);
  movement.value.splice(index, 1);
};
</script>

<style>
</style>