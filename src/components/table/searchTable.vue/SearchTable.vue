<template>
  <DataTable class="search-table" :value="tableValues">
    <template #header>
      <div class="search-table__header">
        <InputGroup class="search-table__search-input">
          <SplitButton :model="columnToSearchOptions">
            <template #dropdownicon>
              <i class="pi pi-sliders-v custom_icon"></i>
            </template>
          </SplitButton>

          <InputText
            @keydown.enter="filterValues"
            v-model="filters.searchTxt"
            placeholder="Search..."
          />
          <InputGroupAddon
            @click="filters.searchTxt = ''"
            class="close"
            v-show="filters.searchTxt.length"
          >
            <i class="pi pi-times"></i>
          </InputGroupAddon>
          <Button @click="filterValues" severity="info" label="Search" />
        </InputGroup>

        <MultiSelect
          placeholder="Поля таблицы"
          class="search-table__columns-select"
          :options="columnOptions"
          v-model:selectedOptions="filters.showedColumns"
        />
      </div>
    </template>
    <template v-for="column of tableColumns" :key="column.field">
      <Column
        :class="
          column.field === filters.columnToSearch
            ? 'active'
            : ''
        "
        :field="column.field"
        :header="column.header"
      >
      </Column>
    </template>
  </DataTable>
</template>

<script setup lang="ts">
import DataTable from "primevue/datatable";
import Column from "primevue/column";
import MultiSelect from "../../multiSelect/MultiSelect.vue";
import InputText from "primevue/inputtext";
import InputGroup from "primevue/inputgroup";
import Button from "primevue/button";

import type { Option } from "../../multiSelect/multiSelect.i";

import { computed, reactive, ref, watch } from "vue";
import InputGroupAddon from "primevue/inputgroupaddon";
import SplitButton from "primevue/splitbutton";

interface TableItem {
  [field: string]: any;
}

interface TableColumn {
  field: string;
  header: string;
}

interface FiltersState {
  searchTxt: string;
  showedColumns: Option[];
  columnToSearch: TableColumn["field"];
}

const anyOption = "any";

const props = defineProps<{
  value: TableItem[];
  columns: TableColumn[];
}>();

const filters = reactive<FiltersState>({
  searchTxt: "",
  showedColumns: [],
  columnToSearch: anyOption,
});

const tableValues = ref(props.value);
const tableColumns = ref(props.columns);

const columnOptions = computed(() =>
  props.columns.map((c) => ({
    name: c.header,
    code: c.field,
  }))
);

const columnToSearchOptions = computed(() =>
  [
    {
      header: anyOption,
      field: anyOption,
    },
    ...tableColumns.value,
  ].map((c) => ({
    label: c.header,
    command: () => {
      filters.columnToSearch = c.field;
    },
  }))
);

function filterValues() {
  if (filters.columnToSearch !== anyOption) {
    tableValues.value = props.value.filter((item) =>
      item[filters.columnToSearch]
        .toLowerCase()
        .includes(filters.searchTxt.toLowerCase())
    );
    return;
  }

  if (filters.showedColumns.length) {
    tableValues.value = props.value.filter((item) =>
      filters.showedColumns.some((column) =>
        item[column.code]
          .toLowerCase()
          .includes(filters.searchTxt.toLowerCase())
      )
    );
    return;
  }

  tableValues.value = props.value.filter((item) =>
    Object.values(item).some((value: string) =>
      value.toLowerCase().includes(filters.searchTxt.toLowerCase())
    )
  );
}

watch(
  () => filters.searchTxt,
  (value) => {
    if (!value.length) {
      tableValues.value = props.value;
    }
  }
);

watch(
  () => filters.showedColumns,
  (value) => {
    tableColumns.value = value.length
      ? props.columns.filter((column) =>
          filters.showedColumns.some(
            (showedColumn) => showedColumn.code === column.field
          )
        )
      : props.columns;
    filters.columnToSearch = anyOption;
    filterValues();
  }
);
</script>

<style lang="scss">
.close {
  cursor: pointer;
}

.search-table {

  th.active {
    background: var(--p-datatable-header-cell-selected-background);
  }
  &__header {
    display: flex;
    gap: 0.5rem;
    width: 100%;

    .p-splitbutton-button {
      display: none;
    }
    .p-splitbutton-dropdown:has(i.custom_icon) {
      border-top-right-radius: 0;
      border-bottom-right-radius: 0;
      border-top-left-radius: var(--p-inputgroup-addon-border-radius);
      border-bottom-left-radius: var(--p-inputgroup-addon-border-radius);
    }
  }

  &__search-input {
    flex: 80;
  }

  &__columns-select {
    flex: 20;
    max-width: 20%;
  }
}
</style>
