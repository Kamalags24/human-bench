<script setup>
import { ref, onMounted, watch } from 'vue';


const myScores = ref([])  // variable pour récupérer les scores

const props = defineProps({  //cons props pour la recupération des scores depuis le composant gameArea
  scores : {
    type:Array,
  }
}) // cons props 


watch(()=>props.scores,(score)=>{ //watch  , observe les scores de façon dynamique et recupère les scores finaux et les affiches sur la vue App.vue
  myScores.value = score  // dans le composant information on recupère les scores  et on les affiches sur la vue App.vue
  console.log(myScores.value);

  


})


</script>


<template>
  <div class="container">
    <div class="second-container">
      <p>C'est ici qu'une autre annonce s'affiche !</p>
    </div>
    <div class="flex-container">
      <div class="third-container">
          <h2>Entraîneur de visée</h2>
          <p>Cliquez sur les cibles aussi rapidement et précisément que possible.</p>
          <p>Cela teste les réflexes et la coordination œil-main.</p>
          <p>Une fois que vous aurez cliqué sur 30 cibles, votre score et votre temps moyen par cible seront affichés.</p>
          <p>Les scores de ce test sont plus lents que ceux du simple test de temps de réaction, car vous devez réagir puis déplacer le curseur.</p>
          <p>Il est préférable de réaliser ce test avec un écran de souris ou de tablette. Il est difficile de réussir avec les trackpads.</p>
      </div>

      <div class="container-four">
        <h2>Scores - Meilleur Score {{ myScores[0] }} </h2>
        <div   class="fourth-container" v-for="(score, index) in myScores" :key="index">
          <h4>Score {{ index + 1 }}: {{ score }}</h4>
          
        </div>


        <!-- <img src="/public/statt.png" alt=""> -->
      </div>
    </div>
    <div class="container-five">
  <h2>C'est ici qu'une autre annonce s'affiche !</h2>
   </div>
  </div>

</template>


<style scoped>
.container {
  background-color: #e6e6e6;
}

.second-container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 1000px;
  height: 150px;
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 40px;
  background-color: #fff;
}

.third-container {
  background-color: white;
  padding: 30px;
  padding-right: 20px;
  margin: 0 auto;
}
  .container-four {
    background-color: white;
    padding-left: 20px;
    margin: 0 auto;
    width: 500px;
    height:auto;
    display: flex;
    flex-direction: column;
    column-count: 2 ;
    column-span: all;
    
    
  }

.container-four {
  background-color: white;
  padding-left: 20px;
  margin: 0 auto; 
  width: 500px;
  height: 400px;
}

.container-five {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 1000px;
  height: 150px;
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 25px;
  background-color: #fff;
}

.flex-container {
  /* display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr;
  gap: 0px 0px;
  grid-auto-flow: row;
  grid-template-areas:
    ". . ."
    ". . ."
    ". . ."; */

  display: flex;
  margin: 0 auto;
}
.fourth-container {
  
 
width: 100%; 
height: auto;


  
}


</style>