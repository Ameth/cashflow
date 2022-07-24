<template>
  <main>
    <p>{{ labelActual }}</p>
    <h1>{{ valorFormateado }}</h1>
    <div class="graphic">
      <slot name="grafico"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>

<script setup>
import { computed } from "vue";
import formatearMoneda from "@/js/currencyFormater";

const props = defineProps({
  label: {
    type: String,
    default: null,
  },
  labelFecha: String,
  monto: {
    type: Number,
    default: null,
  },
  totalMonto: Number,
});

// const formatearMoneda = new Intl.NumberFormat("es-CO", {
//   style: "currency",
//   currency: "COP",
// });

const valorTotal = computed(() => {
  return props.monto !== null ? props.monto : props.totalMonto;
});

const valorFormateado = computed(() => {
  // console.log(valorTotal.value);
  return formatearMoneda.format(valorTotal.value);
});

const labelActual = computed(() => {
  return props.label !== null ? props.label : props.labelFecha;
});
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>