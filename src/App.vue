<script setup lang="ts">
import { ref } from "vue";
import Card from "./components/Card.vue";

const description = ref<string>("Accepting it will open a pandora’s box");
const revert = ref<boolean>(false);

const reject = () => {
  console.log("rejected");
};
const accepted = () => {
  console.log("accepted");
};
</script>

<template>
  <main>
    <Card :description="description" :revert="revert" @reject="reject" @accepted="accepted" />
  </main>
</template>
