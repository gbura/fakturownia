<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";

interface Props {
  title?: string;
  description?: string;
  revert: boolean;
}

interface API_Response {
  fact: string;
  length: number;
}

const BASE_URL = "https://catfact.ninja/fact" as string;

const { title = "Do you want to accept?", revert = false } = defineProps<Props>();

const emit = defineEmits<{
  (e: "reject"): void;
  (e: "accepted"): void;
}>();

const handleReject = () => {
  emit("reject");
};

const factData = ref<API_Response | null>(null);

const handleGetFacts = async (): Promise<void> => {
  try {
    const res = await fetch(BASE_URL);
    if (!res.ok) {
      throw new Error("Failed to fetch data");
    }

    const data: API_Response = await res.json();
    factData.value = data;

    console.log(data);
    console.log(factData.value.fact);

    emit("accepted");
  } catch (error) {
    console.error(error);
  }
};
</script>

<template>
  <div class="container">
    <div class="container__text">
      <h1 class="container__header">{{ title }}</h1>
      <p v-if="description" class="container__description">
        {{ description }}
      </p>
    </div>
    <div class="container__buttons">
      <button
        class="container__button"
        :class="revert ? 'container__button--accept' : 'container__button--reject'"
        @click="handleReject"
      >
        Reject
      </button>
      <button
        class="container__button"
        :class="revert ? 'container__button--reject' : 'container__button--accept'"
        @click="handleGetFacts"
      >
        Accept changes
      </button>
    </div>
  </div>
</template>

<style scoped lang="scss">
@use "@/assets/_colors.scss" as *;

$font-size-base: 1rem;
$font-size-large: 1.5rem;
$gap-medium: 2.5rem;
$gap-small: 1rem;

.container {
  width: 100%;
  padding: 2rem;
  border-radius: 1rem;
  background-color: $card-backgroundColor;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  gap: $gap-medium;

  &__text {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }
  &__header {
    font-size: $font-size-large;
    line-height: 29px;
  }
  &__description {
    color: $light-gray;
    line-height: 24px;
  }
  &__buttons {
    display: flex;
    flex-direction: column;
    width: 100%;
    justify-content: space-evenly;
    align-items: center;
    gap: $gap-small;
  }
  &__button {
    width: 100%;
    padding: 0.875rem 1.25rem;
    border-radius: 0.5rem;
    border: 1px solid $black;
    font-weight: 500;
    font-size: $font-size-base;

    &--accept {
      background-color: $black;
      color: $white-text;
    }

    &--reject {
      background-color: $white;
      color: $black;
    }
  }
}

@media (min-width: 500px) {
  .container {
    &__buttons {
      flex-direction: row;
      min-width: 400px;
      max-width: 440px;
    }
  }
}
</style>
