<script setup>
import { RouterLink, RouterView } from "vue-router";
import gameArea from "@/components/gameArea.vue";
import information from "@/components/information.vue";
import { ref } from "vue";

const scores = ref([])
// sendScores recupère les scores et les affiches dans la console
function sendScores(score){
  scores.value = score
  console.log(scores.value);
}
</script>

<template>
  <div>
    <!--  création de la barre de navigation -->
    <nav class="barre">
      <div class="rl1">
        <router-link class="l1" to="/">HUMAN BENCHMARK</router-link>
        <router-link to="/">DASHBOARD</router-link>
      </div>
      <div class="rl2">
        <router-link class="l2" to="/">SIGN UP</router-link>
        <router-link to="/">LOGIN</router-link>
      </div>
    </nav>
  </div>
<!-- Dans le composant gameArea on appel la fonction sendScores et on récupere les scores de manière dynamique -->
  <gameArea  @update-score="sendScores"/>
  <!-- Dans le composant information on affiche les scores de façon dynamique-->
  <information :scores = "scores"/>
  <RouterView />
</template>

<style scoped>
 /* barre de navigation style */
.barre {
  margin: 0;
  padding: 0;
  justify-content: center;
  display: flex;
  gap: 20%;
  font-size: 20px;
  margin: 18px;
}

/* div style */
.rl1 {
  justify-content: space-between;
  gap: 2%;
}
/* router-link style */
.l1 {
  margin-right: 25px;
}
/* div style */
.rl2 {
  /* display: flex; */
  gap: 5%;
  margin-right: 20px;
}
/* router-link style */
.l2 {
  margin-right: 20px;
}


a {
 color: black;
text-decoration: none;
padding: 10px;

}

a:hover {
  background-color: #e6e6e6;
  
}

</style>