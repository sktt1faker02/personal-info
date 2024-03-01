<template>
  <label class="text-primary" :for="id">{{ label }}</label>
  <select class="p-3 rounded-md outline-none border border-tertiary focus:border-primary hover:primary" v-model="model">
    <option value="" hidden>Select {{ label }}</option>
    <option v-for="option in options" :key="option.id" :value="option.value">{{ option.label }}</option>
  </select>
</template>

<script setup lang="ts">
import type { Options } from "../types/types";

const props = defineProps<{
  id: string;
  label?: string;
  options: Options[];
  placeholder?: string;
}>();

const model = defineModel();
</script>
