<template>
  <div class="flex items-center gap-2">
    <input type="radio" :id="id" v-model="model" :value="value" />
    <label :for="id">{{ label }}</label>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  label: string;
  id: string;
  value: string;
  //   name: string;
}>();

const model = defineModel();
</script>
