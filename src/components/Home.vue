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
        <template #grafico> Grafico </template>
        <template #action> Acción </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movement" />
    </template>
  </Layout>
</template>

<script setup>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";

let valor = null;
let movement = [
  {
    id: 1,
    title: "Compra en HomeCenter",
    description: "Compra de articulos de aseo",
    valor: -100000,
  },
  {
    id: 2,
    title: "Compra en Parque Central",
    description: "Compra de Celular",
    valor: -450000,
  },
  {
    id: 3,
    title: "Compra en FastFood",
    description: "Compra de Comida",
    valor: -60000,
  },
  {
    id: 4,
    title: "Compra en Porthos",
    description: "Compra de Hamburguesas 🍔",
    valor: -31000,
  },
  {
    id: 5,
    title: "Consignacion",
    description: "Transferencia de pago de Nomina",
    valor: 1380000,
  },
  {
    id: 6,
    title: "Compra en Long Hang",
    description: "Compra de comida china",
    valor: -19000,
  },
  {
    id: 7,
    title: "Compra en Spotify",
    description: "Pago mensual del servicio",
    valor: -24900,
  },
  {
    id: 8,
    title: "Consignacion",
    description: "Transferencia de Britney Ordoñez",
    valor: 20000,
  },
  {
    id: 9,
    title: "Compra en Netflix",
    description: "Pago de servicio mensual",
    valor: -29900,
  },
  {
    id: 10,
    title: "Consignacion",
    description: "Transferencia de Victor Cervantes",
    valor: 50000,
  },
];
</script>

<style>
</style>