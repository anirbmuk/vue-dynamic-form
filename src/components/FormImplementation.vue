<template>
  <section>
    <!-- Dynamic form container with fieldset for semantic grouping -->
    <fieldset>
      <legend>Form Implementation</legend>

      <!-- Dynamically generate input fields based on count.input -->
      <!-- Each input gets a unique key and ID for proper Vue reactivity -->
      <div class="form-container">
        <div v-for="i in count.input" :key="`input-${i}`" class="form-group">
          <label :for="`input-${i}`">Input {{ i }}:</label>
          <input :id="`input-${i}`" type="text" v-model="objectRefs[`input-${i}`].value" autocomplete="off" />
        </div>
      </div>

      <!-- Dynamically generate select dropdowns based on count.select -->
      <!-- Each select gets a unique key and ID, with predefined options -->
      <div class="form-container">
        <div v-for="j in count.select" :key="`select-${j}`" class="form-group">
          <label :for="`select-${j}`">Select {{ j }}:</label>
          <select :id="`select-${j}`" v-model="objectRefs[`select-${j}`].value">
            <option value="option1">Option 1</option>
            <option value="option2">Option 2</option>
            <option value="option3">Option 3</option>
            <option value="option4">Option 4</option>
            <option value="option5">Option 5</option>
          </select>
        </div>
      </div>

      <!-- Dynamically generate select dropdowns based on count.select -->
      <!-- Each select gets a unique key and ID, with predefined options -->
      <div class="form-container">
        <div v-for="k in count.yesno" :key="`yesno-${k}`" class="form-group">
          <label>Toggle {{ k }}
            <YesNo :id="`yesno-${k}`" v-model="objectRefs[`yesno-${k}`].value" />
          </label>
        </div>
      </div>

      <!-- Debug section: Display all current form field values -->
      <!-- Shows the reactive state of each dynamically created field -->
      <template v-for="key in objectRefKeys" :key="key">
        <dl style="display: flex; gap: 0.5rem; align-items: center;">
          <dt style="display: flex; gap: 0.5rem; align-items: center;">
            <span style="font-weight: 900;">Value for</span>
            <span style="font-weight: 900; border-bottom: 1px solid black;">{{  key }}</span>
          </dt>
          <dd style="font-weight: 500;">{{ objectRefs[key].value }}</dd>
        </dl>
      </template>
    </fieldset>
  </section>
</template>

<script setup lang="ts">
import {
  reactive,
  ref,
  watch,
  type PropType,
  type Ref
} from 'vue';
import type { FormDefinition } from '@/types/form-definition';
import YesNo from '@/components/YesNo.vue';

const props = defineProps({
  count: {
    type: Object as PropType<FormDefinition>,
    required: true,
  },
});

// Reactive state management for dynamic form fields
// objectRefs: Stores reactive refs for each form field (input-1, input-2, select-1, etc.)
const objectRefs: Record<string, Ref<string | 'yes' | 'no'>> = {};

// objectRefKeys: Reactive Set to track which keys currently exist
// Used for template iteration and cleanup logic
const objectRefKeys = reactive<Set<string>>(new Set<string>());

// objectRefsList: Alternative array representation of refs (currently unused)
// Could be used for bulk operations or debugging
const objectRefsList = reactive<Ref<string>[]>([])

// Watch for changes in the form definition (count prop)
// This is the core logic that dynamically creates/destroys form fields
watch(props.count, (newCount) => {
  // Reset tracking collections for the new configuration
  objectRefsList.length = 0; // Clear the list
  objectRefKeys.clear(); // Clear the set to track new keys

  // Create reactive refs for input fields based on newCount.input
  for (let i = 1; i <= newCount.input; i++) {
    const key = `input-${i}`;
    objectRefKeys.add(key);
    // Only create new ref if it doesn't already exist (prevents recreation on re-renders)
    if (objectRefs[key] === undefined) {
      console.log(`Creating ref for ${key}`);
      const refValue = ref<string>('');
      objectRefs[key] = refValue;
    }
  }

  // Create reactive refs for select fields based on newCount.select
  for (let j = 1; j <= newCount.select; j++) {
    const key = `select-${j}`;
    objectRefKeys.add(key);
    // Only create new ref if it doesn't already exist
    if (objectRefs[key] === undefined) {
      console.log(`Creating ref for ${key}`);
      const refValue = ref<string>('');
      objectRefs[key] = refValue;
    }
  }

  // Create reactive refs for yesno fields based on newCount.select
  for (let k = 1; k <= newCount.yesno; k++) {
    const key = `yesno-${k}`;
    objectRefKeys.add(key);
    // Only create new ref if it doesn't already exist
    if (objectRefs[key] === undefined) {
      console.log(`Creating ref for ${key}`);
      const refValue = ref<'yes' | 'no'>('yes');
      objectRefs[key] = refValue;
    }
  }

  // Cleanup: Remove refs for fields that are no longer needed
  // This prevents memory leaks and keeps objectRefs in sync with current form definition
  for (const key in objectRefs) {
    if (!objectRefKeys.has(key)) {
      console.log(`Deleting ref for ${key}`);
      delete objectRefs[key];
    }
  }

  // Populate the array representation of refs (for potential future use)
  for (const key in objectRefs) {
    objectRefsList.push(objectRefs[key]);
  }

}, { immediate: true }); // immediate: true ensures this runs on component mount

// Commented out watcher for debugging objectRefsList changes
// watch(() => objectRefsList, (newRefs) => {
//   console.log('Object refs list updated:', newRefs);
// }, { immediate: true });

defineOptions({
  name: 'FormImplementation',
});
</script>
