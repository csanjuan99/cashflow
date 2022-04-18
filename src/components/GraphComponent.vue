<template>
  <div>
    <svg
        @touchstart="onTouch"
        @touchmove="onTouch"
        @touchend="onTouchEnd"
        viewBox="0 0 300 200">
      <line
          stroke="#c4c4c4"
          stroke-width="2"
          x1="0"
          :y1="zero"
          x2="300"
          :y2="zero"
      />
      <polyline
          stroke="#0D96BF"
          stroke-width="2"
          fill="none"
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
    <p class="font-bold text-lg">Ultimos 30 dias</p>
  </div>
</template>

<script setup>
import {computed, defineProps, ref, toRefs, defineEmits, watch} from "vue";

const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});

const {amounts} = toRefs(props);

const amountToPixel = (amount) => {
  const max = Math.max(...amounts.value);
  const min = Math.min(...amounts.value);
  const diff = Math.abs(max - min);
  const amountAbs = amount + Math.abs(min);
  return 200 - (amountAbs / diff) * 200;
}

const zero = computed(() => {
  return amountToPixel(0) ? amountToPixel(0) : 100;
});

const points = computed(() => {
  const size = amounts.value.length;
  return amounts.value.reduce((points, amount, i) => {
    const x = (300 / size) * (i + 1);
    const y = amountToPixel(amount);
    return `${points} ${x}, ${y}`;
  }, `0,200`);
});

const showPointer = ref(false);
let pointer = ref(0);
const emit = defineEmits(["select"]);

watch(pointer, (value) => {
  const index = Math.ceil((value / (300 / amounts.value.length)));
  if (index < 0 || index > amounts.value.length) return;
  emit("select", amounts.value[index - 1]);
})

const onTouch = ({target, touches}) => {
  showPointer.value = true;
  const width = target.getBoundingClientRect().width;
  const x = target.getBoundingClientRect().x;
  const touch = touches[0].clientX;
  pointer.value = ((touch - x) * 300) / width;
}

const onTouchEnd = () => {
  showPointer.value = false;
}

</script>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>