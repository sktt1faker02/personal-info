<template>
  <div class="card px-4 py-8 bg-white" :class="[shadow, radius]">
    <slot />
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  shadow: "shadow-sm" | "shadow-md" | "shadow-lg";
  radius: "rounded-sm" | "rounded-md" | "rounded-lg";
}>();
</script>
