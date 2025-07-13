<template>
  <section>
    <fieldset>
      <legend>Form Definition</legend>
      <div class="form-group">
        <label for="input">Input Count:</label>
        <input id="input" type="number" v-model.number="formDefinition.input" min="0" />
      </div>
      <div class="form-group">
        <label for="select">Select Count:</label>
        <input id="select" type="number" v-model.number="formDefinition.select" min="0" />
      </div>
    </fieldset>
  </section>
</template>

<script setup lang="ts">
import { reactive, watch, type Prop, type PropType } from 'vue';
import type { FormDefinition } from '@/types/form-definition';

const props = defineProps({
  count: {
    type: Object as PropType<FormDefinition>,
    required: true,
  }
})

const emit = defineEmits<{
  (event: 'update:form-definition', value: FormDefinition): void;
}>();

const formDefinition = reactive<FormDefinition>({
  input: props.count.input || 0,
  select: props.count.select || 0,
});

watch(formDefinition, ({ input, select }) => {
  emit('update:form-definition', { input: Math.max(input, 0), select: Math.max(select, 0) })
}, {
  deep: true,
});

defineOptions({
  name: 'FormDefinition',
})
</script>
