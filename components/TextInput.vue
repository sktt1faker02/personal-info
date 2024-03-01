<template>
  <div class="flex flex-col gap-2">
    <label class="text-primary" :for="id">{{ label }}</label>
    <input class="p-3 rounded-md outline-none border border-tertiary focus:border-primary hover:primary" :type="type" :placeholder="placeholder" :id="id" :required="required" v-model="model" />
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  label: string | null;
  id: string;
  placeholder?: string;
  required?: boolean;
  type: string;
}>();

const model = defineModel();
</script>
