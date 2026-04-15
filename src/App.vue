<script setup lang="ts">
import { ref } from "vue"

const message = ref<string>("Salut")

</script>


<template>
  <h1>{{ message }}</h1>
</template>
