<template>
  <div
    @click="$emit('tabClicked', label)"
    ref="containerRef"
    class="rounded-lg bg-gray-100 flex items-center justify-center gap-2 text-gray-700 px-3 py-2 cursor-pointer"
  >
    <slot name="icon"></slot>

    <span ref="labelRef">
      {{ props.label }}
    </span>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import { useMotion } from '@vueuse/motion';

const props = defineProps({
  label: {
    type: String,
  },
  isSelected: {
    type: Boolean,
    default: true,
  },
});

const containerRef = ref(null);
const labelRef = ref(null);

const isSelected = computed(() => props.isSelected);

const { apply } = useMotion(labelRef, {
  enter: {
    opacity: 0,
    x: 20,
    display: 'none',
  },
  hover: {
    opacity: 1,
    display: 'inline',
    x: 0,
    transition: {
      type: 'keyframes',
      stiffness: 250,
      stiffness: 250,
      damping: 20,
      duration: 300,
      ease: 'easeOut',
    },
  },
});

watch(
  isSelected,
  (value) => {
    if (value) {
      apply('hover');
    } else {
      apply('enter');
    }
  },
  {
    deep: true,
    immediate: true,
  }
);
</script>
