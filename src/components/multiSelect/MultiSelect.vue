<template>
  <InputGroup>
    <MultiSelect
      class="multiselect"
      v-model="selectedOptions"
      display="chip"
      :options="options"
      optionLabel="name"
      :placeholder="placeholder"
    >
      <template #dropdownicon>
        <i class="pi pi-angle-down"></i>
        <Button
          @click.stop="clearSelected"
          v-show="selectedOptions.length"
          icon="pi pi-times"
          severity="danger"
        />
      </template>
    </MultiSelect>
  </InputGroup>
</template>

<script setup lang="ts">
import MultiSelect from "primevue/multiselect";
import InputGroup from "primevue/inputgroup";
import Button from "primevue/button";

import type { Option } from "./multiSelect.i.ts";

withDefaults(
  defineProps<{
    placeholder?: string;
    options: Option[];
  }>(),
  {
    placeholder: "Select Cities",
  }
);

const selectedOptions = defineModel<Option[]>("selectedOptions", {
  default: [],
});

const clearSelected = () => {
  selectedOptions.value = [];
};
</script>

<style lang="scss"></style>
