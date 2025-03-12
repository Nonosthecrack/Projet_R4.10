<script setup>
import { useRoute } from "vue-router";

const title = "User profile";
const { params } = useRoute();

const username = params.username;
</script>

<template>
  <h1>{{ title }}</h1>
  <h2>{{ username }}</h2>
</template>
