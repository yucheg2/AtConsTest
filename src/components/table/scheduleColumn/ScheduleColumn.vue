<template>
  <DataTable :value="stringWithSpaces">
    <Column :field="field" header="Расписание"> 
        <template #body="slotProps">
        <span v-html="slotProps.data[field]"></span>
        </template>
    </Column>
  </DataTable>
</template>

<script setup lang="ts">
import DataTable from "primevue/datatable";
import Column from "primevue/column";

import { computed } from "vue";

const props = withDefaults(
  defineProps<{
    scheduleString: string;
    field?: string;
    header?: string;
  }>(),
  {
    field: "schedule",
    header: "Расписание",
  }
);

const stringWithSpaces = computed(() => [
  {
    [props.field]: props.scheduleString
      .replace(/\[\d{2}:\d{2}:\d{2}\]\s*/g, ($1) => `<br/>${$1}`)
      .slice(5),
  },
]);
</script>

<style scoped></style>
