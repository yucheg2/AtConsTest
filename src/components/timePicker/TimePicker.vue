<template>
  <div class="time-picker">
    <label for="timePicker">Значение переменной {{ formattedValue }}</label>
    <DatePicker id="timePicker" showIcon v-model="time" timeOnly />
  </div>
</template>

<script setup lang="ts">
import DatePicker from "primevue/datepicker";
import formatTime from "../../utils/formatTime";
import { ref, computed } from "vue";

const time = ref<Date>(new Date());

const formattedValue = computed(() => {
  return `${formatTime(time.value.getHours())} : ${formatTime(
    time.value.getMinutes()
  )}`;
});
</script>

<style lang="scss">
.time-picker {
  display: flex;
  flex-direction: column;
}
</style>
