<script setup>
import { RouterLink, RouterView } from "vue-router";
import gameArea from "@/components/gameArea.vue";
import information from "@/components/information.vue";
import { ref, onMounted, watch } from "vue";

const scores = ref([])

// Charger les scores depuis le localStorage
function loadScoresFromLocalStorage() {
  const storedScores = localStorage.getItem('allScores');
  if (storedScores) {
    scores.value = JSON.parse(storedScores);
  }else {
    scores.value = [];
  }
  console.log(scores.value);
}

// Sauvegarder les scores dans le localStorage
function saveScoresToLocalStorage() {
  // console.log("saveScoresToLocalStorage", localStorage.setItem('allScores', JSON.stringify(scores)));
  localStorage.setItem('allScores', JSON.stringify(scores.value));
}

// Fonction pour mettre à jour les scores
function sendScores(score) { //score est le tableau de scores que l'enfant envoie
  console.log('fromSendscore', score);
  scores.value.push(score[score.length - 1]);
 
  saveScoresToLocalStorage(scores.value);

 
}



// Charger les scores depuis le localStorage lors du montage du composant
onMounted(() => {
  loadScoresFromLocalStorage();
});

</script>


<template>
  <div class="borne" >
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
 color: rgb(9, 9, 9);
text-decoration: none;
padding: 10px;

}

a:hover {
  background-color: #e6e6e6;
  
}

/*tablette*/


@media screen and (max-width: 768px) {
  .barre {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: flex-start;
    font-size: 15px;
    
  }
  .rl1, .rl2 {
   
    display: flex;
    flex-direction: row;

    align-items: center;
    
    margin: 0;
    padding: 0;
    gap: 0;
    
  }
}
  /*mobile*/


@media screen and (max-width: 480px) {
  .barre {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: flex-start;
    font-size: 12px;
    font-weight: bold;
  }
  .rl1{
   
    display: flex;
    flex-direction: column;

    align-items: center;
    
  
    padding: 10px;
    gap: 10px;
     

     
  }

  .rl2{
   
   display: flex;
   flex-direction: column;

   align-items: center;
   
 
   padding: 10px;
   gap: 10px;
    
    
 }

.borne {
  display: flex;
  flex-direction: column;
  margin-left: 20px;
  margin-right: 20px;
} 

.l1{
  margin-left: 20PX;
}

a {
  
  padding: 3px;
}

}
</style>