<template>
  <main class="main-page">
    <h1>Тестовое Задание</h1>
    <div class="main-page__section">
      <h4>1. Поле link</h4>
      <LinkField />
    </div>
    <div class="main-page__section">
      <h4>2. DateTime picker</h4>
      <TimePicker />
    </div>
    <div class="main-page__section">
      <h4>3. Сброс значений в Select</h4>
      <MultiSelect :options="cities" />
    </div>
    <div class="main-page__section">
      <h4>4. Сброс значений в Select</h4>
      <SingleColumn :scheduleString="apiString" />
    </div>
    <div class="main-page__section">
      <h4>5. Таблица с поиском (расширенное задание)</h4>
      <SearchTable :value="citiesForSearch" :columns="citiesColumns" />
    </div>
  </main>
</template>

<script setup lang="ts">
import LinkField from "../components/linkField/LinkField.vue";
import TimePicker from "../components/timePicker/TimePicker.vue";
import MultiSelect from "../components/multiSelect/MultiSelect.vue";
import SingleColumn from "../components/table/scheduleColumn/ScheduleColumn.vue";
import SearchTable from "../components/table/searchTable.vue/SearchTable.vue";

const cities = [
  { name: "New York", code: "NY" },
  { name: "Rome", code: "RM" },
  { name: "London", code: "LDN" },
  { name: "Istanbul", code: "IST" },
  { name: "Paris", code: "PRS" },
];

const citiesForSearch = [
  { name: "New York", code: "NY", timeZone: "a" },
  { name: "Rome", code: "RM", timeZone: "b" },
  { name: "London", code: "LDN", timeZone: "c" },
  { name: "Istanbul", code: "IST", timeZone: "d" },
  { name: "Paris", code: "PRS", timeZone: "r" },
];

const citiesColumns = [
  {
    field: "name",
    header: "Name",
  },
  {
    field: "code",
    header: "Code",
  },
  {
    field: "timeZone",
    header: "Time zone",
  },
];

const apiString =
  "[13:36:53] Расчетное время: 9 мин[13:36:58] Открыть клапан откачки К1[13:36:58] Включить вакуумный насос[13:36:58] Закрыть клапан К5[13:36:58] Закрыть клапан дистилляции К2[13:36:58] Ожидание: 8 с[13:37:06] Заливка 2.2мл. в испаритель[13:37:06] Заданно 26.50602409638554 шагов[13:37:09] Заливка перекиси завершена[13:37:09] Открыть клапан дистилляции К2[13:42:09] Включить нагрев испарителя[13:42:09] Закрыть клапан дистилляции К2[13:42:09] конечное давление1.0960040758227925 торр[13:42:09] Выпаривание через К2[13:42:09] Выпаривание длилось5 мин[13:42:09] Откачка до 1 торр[13:42:15] Закрыть клапан откачки К1[13:43:09] Открыть клапан откачки К1[13:43:14] Аппаратное смещение 0 денсит. = -0.313683180809021[13:43:14] Закрыть клапан дистилляции К2";

</script>

<style lang="scss">
.main-page {
  display: flex;
  align-items: center;
  flex-direction: column;

  &__section {
    max-width: 800px;
    width: 100%;
  }
}
</style>
