<script setup>
import { RouterLink, RouterView } from "vue-router";
import gameArea from "@/components/gameArea.vue";
</script>

<template>
  <div>
    <nav>
      <router-link to="/">HUMAN BENCHMARK</router-link>
      <router-link to="/">DASHBOARD</router-link>
      <router-link to="/">SIGN UP</router-link>
      <router-link to="/">LOGIN</router-link>
    </nav>
  </div>
  <gameArea />
  <RouterView />
</template>

<style scoped>

</style>
