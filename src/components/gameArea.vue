<template>
  <div class="container">
    <div class="cercle">

    </div>
    <p>Average time per target</p>
    <p>Save your score to see how you compare</p>
    <button>Try Again</button>
  </div>
</template>

<script setup>
import trainerArea from "./trainerArea.vue";
</script>

<style scoped>

.container {
  background-color: #2b87d1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

}
.cercle{
  background-color: #FF000080;
  border: 1px solid #FF000080;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  
}

</style>