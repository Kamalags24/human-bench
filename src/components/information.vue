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
      <p>This is Where the Ad displays!</p>
    </div>
    <div class="flex-container">
      <div class="third-container">
          <h2>Aim Trainer</h2>
          <p>Click the targets as quickly and accurately as you can.</p>
          <p>This tests reflexes and hand-eye coordination.</p>
          <p>Once you've clicked 30 targets, your score and average time per target will be displayed.</p>
          <p>Scores in this test are slower than the simple reaction time test, because you must react and then move the cursor.</p>
          <p>This test is best taken with a mouse or tablet screen. Trackpads are difficult to score well with.</p>
      </div>

      <div class="container-four">
        <h2>Scores</h2>
        <div class="fourth-container" v-for="(score, index) in myScores" :key="index"> <!--  affiche les scores de façon dynamique  -->
              <h4>Score {{ index + 1 }}: {{ score }}</h4> <!--suivant l'index , on affiche le score correspondant , l'index s'incremente  chaque fois que l'on clique sur le bouton reprendre-->
          </div>

          <!-- <img src="/public/statt.png" alt=""> -->
      </div>
    </div>
    <div class="container-five">
  <h2>This is Where Another Ad displays!</h2>
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