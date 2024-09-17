<script setup>
const { isActive } = defineProps({
  isActive: {
    type: Boolean,
    required: true,
  },
});
const emit = defineEmits(["click"]);
</script>

<template>
  <button
    class="w-40 h-15 m-8 p-5 rounded-[5px] bg-midnight text-white text-center text-lg leading-none cursor-pointer btn-shading-bn"
    :class="isActive ? [] : ['bg-mist', 'cursor-not-allowed']"
    :disabled="!isActive"
    @click="emit('click')"
  >
    <slot></slot>
  </button>
</template>
