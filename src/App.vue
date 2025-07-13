<template>
  <header>
    <h1>Vue Dynamic Form</h1>
  </header>

  <main>
    <VueFormDefinition
      :count="initialFormDefinition"
      @update:form-definition="updateFormDefinition"
     />
    <VueFormImplementation
      :count="formDefinition"
     />
  </main>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
import type { FormDefinition } from '@/types/form-definition';
import VueFormDefinition from './components/FormDefinition.vue'
import VueFormImplementation from './components/FormImplementation.vue'

const initialFormDefinition: FormDefinition = {
  input: 0,
  select: 0,
};

const formDefinition = reactive<FormDefinition>(initialFormDefinition);

const updateFormDefinition = ({ input, select }: FormDefinition) => {
  formDefinition.input = input;
  formDefinition.select = select;
};
</script>
