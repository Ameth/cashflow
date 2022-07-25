<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 300 200"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      />
      <polyline
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#04B500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />
    </svg>
    <p>Últimos 30 dias</p>
  </div>
</template>

<script setup>
import { computed, ref, watch } from "vue";

const props = defineProps({
  lista: {
    type: Array,
    default: () => [],
  },
});

const valorToPixels = (valor) => {
  const min = Math.min(...props.lista.map((item) => item.valor));
  const max = Math.max(...props.lista.map((item) => item.valor));

  const valorAbsoluto = valor + Math.abs(min);
  const min_max = Math.abs(max) + Math.abs(min);

  return 200 - ((valorAbsoluto * 100) / min_max) * 2;
};

const zero = computed(() => {
  return valorToPixels(0);
});

const points = computed(() => {
  const total = props.lista.length;
  return props.lista.reduce((points, item, index) => {
    const x = (300 / total) * (index + 1);
    const y = valorToPixels(item.valor);
    // console.log(y);
    return `${points} ${x},${y}`;
  }, `0,${valorToPixels(props.lista.length ? props.lista[0].valor : 0)}`);
});

const showPointer = ref(false);
const pointer = ref(0);

const emit = defineEmits(["select"]);

watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / props.lista.length));
  if (index < 0 || index > props.lista.length) return;

  // console.log("emit", props.lista[index - 1]);
  emit("select", props.lista[index - 1]);
});

const tap = ({ target, touches }) => {
  showPointer.value = true;
  // console.log(target, touches);
  const elementWidth = target.getBoundingClientRect().width;
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  pointer.value = ((touchX - elementX) * 300) / elementWidth;
};

const untap = () => {
  showPointer.value = false;
  emit("select", { valor: null, date: null });
};
</script>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>