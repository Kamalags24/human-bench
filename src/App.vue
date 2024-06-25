<script setup>
import gameArea from "@/components/gameArea.vue";
import information from "@/components/information.vue";
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
  <information />
</template>

<style scoped>

</style>
