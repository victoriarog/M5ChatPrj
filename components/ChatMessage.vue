<template>
    <div class="border border-blue-100 rounded-lg"><slot /></div>
  </template>
  
  <script setup>
  import { toRefs } from "vue";
  const props = defineProps(["message"]);
  const { message } = toRefs(props);
  </script>