<template>
  <div>
    <button type="button" @click="emit('update:modelValue', 'yes')" :selected="modelValue === 'yes'">Yes</button>
    <button type="button" @click="emit('update:modelValue', 'no')" :selected="modelValue === 'no'">No</button>
  </div>
</template>

<script setup lang="ts">
defineProps({
  modelValue: {
    type: String,
    default: 'yes',
  }
});
const emit = defineEmits<{
  (event: 'update:modelValue', value: 'yes' | 'no'): void;
}>();
defineOptions({
  name: 'YesNo',
});
</script>
