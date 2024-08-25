<template>
  <form @submit.prevent="getTitle" class="link-field">
    <InputGroup class="link-field__input-group">
      <InputText
        :readonly="isTitled"
        :disabled="loading"
        ref="input"
        class="link-field__input"
        :class="isTitled ? 'link-field__input--titled' : ''"
        v-model="link[showedValue]"
        size="large"
        placeholder="https://"
        @blur="getTitle"
        @click="goToUrl"
      />
      <Button
        @click="editUrl"
        v-if="isTitled"
        icon="pi pi-pencil"
        severity="secondary"
      />
    </InputGroup>
    <small v-show="loading">Загрузка...</small>
  </form>
</template>

<script setup lang="ts">
import InputGroup from "primevue/inputgroup";
import InputText from "primevue/inputtext";
import Button from "primevue/button";

import { reactive, ref, computed, nextTick } from "vue";
import type { Link } from "./linkField.i";

const input = ref<{ $el: HTMLInputElement } | null>(null);

const link = reactive<Link>({
  title: "",
  url: "",
});

const loading = ref<boolean>(false);
const showedValue = ref<keyof Link>("url");

const isTitled = computed(() => {
  return !!link.title.length;
});

const getTitle = async () => {
  if (!isTitled) return;

  loading.value = true;

  fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(link.url)}`)
    .then((response) => {
      if (response.status === 200) return response.text();
      throw new Error("Network response was not ok.");
    })
    .then((html) => {
      const doc = new DOMParser().parseFromString(html, "text/html");
      const title = doc.querySelector("title");

      link.title = title?.innerText as string;
      showedValue.value = "title";
    })
    .catch(() => alert("Не корректный url"))
    .finally(() => {
      loading.value = false;
    });
};

const goToUrl = () => {
  if (isTitled.value) {
    window.open(link.url, "_blank")?.focus();
  }
};

const editUrl = async () => {
  if (isTitled.value) {
    link.title = "";
    showedValue.value = "url";
    await nextTick();
    input.value && input.value.$el.focus();
  }
};
</script>

<style lang="scss">
.link-field {
  &__input-group {
    display: flex;
    align-items: center;
  }

  input.link-field__input {
    flex-grow: 1;
    &--titled {
      background: none;
      cursor: pointer;
      text-decoration: underline;
    }
  }
}
</style>
