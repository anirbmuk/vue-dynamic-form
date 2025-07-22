<template>
  <section>
    <fieldset>
      <legend>Form Definition</legend>
      <div class="form-container">
        <div class="form-group">
          <label for="input">Input Count:</label>
          <input id="input" type="number" v-model.number="formDefinition.input" min="0" />
        </div>
        <div class="form-group">
          <label for="select">Select Count:</label>
          <input id="select" type="number" v-model.number="formDefinition.select" min="0" />
        </div>
        <div class="form-group">
          <label for="yesno">YesNo Count:</label>
          <input id="yesno" type="number" v-model.number="formDefinition.yesno" min="0" />
        </div>
      </div>
    </fieldset>
  </section>
</template>

<script setup lang="ts">
import { reactive, watch, type PropType } from 'vue';
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
  yesno: props.count.yesno || 0,
});

watch(formDefinition, ({ input, select, yesno }) => {
  emit('update:form-definition', {
    input: Math.max(input, 0),
    select: Math.max(select, 0),
    yesno: Math.max(yesno, 0)
  })
}, {
  deep: true,
});

defineOptions({
  name: 'FormDefinition',
})
</script>
