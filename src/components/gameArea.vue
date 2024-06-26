<script setup>
import trainerArea from "./trainerArea.vue";
import remaining from "./remaining.vue";
import { ref } from "vue";

const Start = ref(true);
const Game = ref(false);
const End = ref(false);
let count = ref(5);
let left = ref(0);
let top = ref(0);
let second = ref(0);
let score = ref(0);
let  couleurs= ['#FF5733', '#33FF57', '#3357FF', '#F833FF', '#33FFF8',
'#FFFF33', '#FF33F8', '#33FFFB', '#FF8333', '#33FF83',
'#3373FF', '#F883FF', '#3383FF', '#FF3383', '#83FF33',
'#5733FF', '#FF573A', '#33FF5A', '#335AFF', '#F833FA']
let a = ref();
let misses = ref(0) // variable pour stocker


function startGame() {
  Start.value = false;
  Game.value = true;
  End.value = false;
  timer();
}

function restartGame() {
  count.value = 5;
  Start.value = false;
  Game.value = true;
  End.value = false;
  second.value = 0; // Réinitialiser le chronomètre
  misses.value = 0;
  timer();
} 

function changePosition() {
  count.value--;
  if (count.value === 0) {
    Game.value = false;
    End.value = true;
  }
  top.value = Math.floor(Math.random() * 100);
  left.value = Math.floor(Math.random() * 100);

  a.value = Math.floor(Math.random() * couleurs.length-1);
  console.log(top.value)
  
  // top.value = Math.floor(Math.random() * 250);
  // left.value = Math.floor(Math.random() * 250);
  
}
// let score = setInterval(() => {console.log(count.value)}, 1000);

function timer() {
  second.value++;
  // console.log(second.value);
  let timeout = setTimeout(timer, 1);
  if (count.value === 0) {
    clearTimeout(timeout);
 
console.log("Le temps passé est : ", second.value);
  }
}



   //fonction pour recuperer les misses 

   function handleMisses(element) {
    if (Game.value && element.target.className != "cercle"
    ) {
      misses.value++
      
    }
    // console.log(misses.value);
    // console.log(element);
    
   }


</script>



<template>

  <div class="container">
    <div v-if="Start" class="zone">
      <h1>Bienvenue dans le jeu</h1>
      <div class="cercle" @click="startGame"></div>
      <p>
        Hit 30 targets as quickly as you can. Click the target above to begin
      </p>
    </div>

    <main class="zone" v-if="Game">
      <h1>Remaining: {{ count }}</h1>
      <h3>Misses: {{ misses }}</h3>
      <div class="container-cercle" @click="handleMisses" >
        <div
          class="cercle"
          :style="{ top: top + 'px', left: left + 'px' , backgroundColor : couleurs[a] }"
          @click="changePosition"

        ></div>
      </div>
    </main>

    <div v-if="End" class="zone">
      <div class="cercle"></div>
      <p>Average time per target</p>
      <h2>{{ second }} ms</h2>
      <h4>Misses: {{ misses }}</h4>
      <p>Save your score to see how you compare</p>
      <button @click="restartGame">Try again</button>
    </div>
  </div>
</template>


<style scoped>
.container {
  background-color: #2b87d1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  height: 800px;

}


.cercle {
  background-color: #ff000080;
  border: 1px solid #ff000080;
  border-radius: 50%;
  width: 100px;
  height: 100px;
  position: relative;
  top: 0;
  left: 0;
  margin: auto;
}

button {
  background-color: yellow;
  border: 1px solid yellow;
  border-radius: 20px;
  width: 100px;
  height: 30px;
}

.container-cercle {
  height: 350px;
  width: 660px;
}

.zone {
  color: white;
  width: 500px;
  height: 300px;
  text-align: center;
  font-size: 22px;
  margin: 20px auto;
}
h4 {
  color: red;
}

</style>