<script setup>
import trainerArea from "./trainerArea.vue";
import remaining from "./remaining.vue";
import { ref } from "vue";

const Start = ref(true);
const Game = ref(true);
const End = ref(true);
let count = ref(5);
let left = ref(0);
let top = ref(0);
let second = ref(0);
let score = ref(0);
let  couleurs= ["#e74c3c", "#2ecc71", "#f1c40f", "#9b59b6", "#1abc9c"]
let a = ref()
// let r = ref()
// let b=ref ()
// let g=  ref()
// let rgb = ref ()


function startGame() {
  Start.value = false;
  Game.value = false;
  End.value = true;
  timer();
}

function restartGame() {
  count.value = 5;
  Start.value = false;
  Game.value = false;
  End.value = true;
  timer();
}

function changePosition() {
  count.value--;
  if (count.value === 0) {
    Game.value = true;
    End.value = false;
  }

  top.value = Math.floor(Math.random() * 100);
  left.value = Math.floor(Math.random() * 100);

  a.value = Math.floor(Math.random() * couleurs.length-1);
  console.log(a)
  console.log(Math.random ())
  
}
// let score = setInterval(() => {console.log(count.value)}, 1000);

function timer() {
  second.value++;
  // console.log(second.value);
  let timeout = setTimeout(timer, 1);
  if (count.value === 0) {
    clearTimeout(timeout);
  }
}
console.log("Le temps passé est : ", second.value);

function back(){
  console.log("retour")
  r.value = Math.floor(Math.random()*256) 
  g.value=  Math.floor(Math.random()*256) 
  b.value=  Math.floor(Math.random()*256)
  // console.log(r.value)
  // rgb=
  // console.log (rgb)
}

</script>



<template>
  <div class="container"  @click="back">
    <div v-if="Start">
      <h1>Bienvenue dans le jeu</h1>
      <div class="cercle" @click="startGame"></div>
      <p>
        Hit 30 targets as quickly as you can. Click the target above to begin
      </p>
    </div>

    <main class="dashboard" v-if="!Game">
      <h1>Remaining: {{ count }}</h1>
      <div class="container-cercle" >
        <div
          class="cercle"
          :style="{ top: top + 'px', left: left + 'px' , backgroundColor : couleurs[a] }"
          
          
          @click="changePosition"
        ></div>
      </div>
    </main>

    <div v-if="!End">
      <div class="cercle"></div>
      <p>Average time per target</p>
      <p>{{ second }}</p>
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
  height: 500px;
}

.container-cercle {
  display: block;
  position: relative;
}

.cercle {
  background-color: #ff000080;
  border: 1px solid #ff000080;
  border-radius: 50%;
  width: 50px;
  height: 50px;
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
  border: 1px solid black;
  height: 300px;
  width: 800px;
}

</style>