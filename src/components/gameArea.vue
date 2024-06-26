<script setup>
import trainerArea from "./trainerArea.vue";
import remaining from "./remaining.vue";
import { ref } from "vue";

const Start = ref(true);
const Game = ref(true);
const End = ref(true);
let count = ref();
let left = ref(0);
let top = ref(0);
let second = ref(0);
let score = ref(0);
const numClicks = ref(5);

function startGame() {
  count.value = numClicks.value
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
</script>



<template>
  <div class="container">
    <div v-if="Start">
      <h1>Bienvenue dans le jeu !</h1>
      <div class="cercle" ></div>
      <p>
        Choisissez le nombre de cible et atteigner le, le plus vite possible !
      </p>
      <!-- <input type="number"> -->
      <div class="input-button">
          <p class="label">Nombre de clics minimum (minimum 5):</p><br>
          <input type="number" id="clicks" v-model="numClicks" min="5" @input="validateNumClicks"><br>
          <button @click="startGame" :disabled="numClicks < 5" class="gamebutton">Commencer le jeu</button>
      </div>
      
    </div>

    <main class="dashboard" v-if="!Game">
      <h1>Remaining: {{ count }}</h1>
      <div class="container-cercle" >
        <div
          class="cercle"
          :style="{ top: top + 'px', left: left + 'px' }"
          @click="changePosition"
        ></div>
      </div>
    </main>

    <div v-if="!End">
      <div class="cercle"></div>
      <p class="cercle-p">Temps moyen par click</p>
      <p class="second">{{ second + " ms" }}</p>
      <p class="save">Save your score to see how you compare</p>
      <button class="restast-button" @click="restartGame">Reprendre</button>
    </div>
  </div>
</template>


<style scoped>
.cercle-p {
  color: white;
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 28px;
  margin-top: 10px;
}

.restast-button {
  margin: 0;
  padding: 13px;
  text-align: center;
  transform: translateX(130px);
  margin-top: 35px;
  color: inherit;
  background-color: yellow;
  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
}

.save {
  font-family: sans-serif;
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 20px;
  margin-top: 25px;
}

.second {
  margin: 0;
  padding: 0;
  text-align: center;
  font-weight: bold;
  font-size: 60px;
  font-family: sans-serif;
}

.container {
  width: 100%;
  height: 500px;
  background-color: #2b87d1;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

h1 {
  margin: 0;
  padding: 0;
  text-align: center;
}

.input-button {
  margin: 0;
  padding: 0;
  text-align: center;
}

main h1 {
  margin-bottom: 100px;
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

input {
  width: 300px;
  height: 30px;
  border: none;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px, rgba(0, 0, 0, 0.22) 0px 15px 12px;
  /* margin-top: 70px; */
  margin-bottom: 10px;
}

.label {
  margin-bottom: 8px;
  margin-top: 8px;
  font-size: 20px;
}

.gamebutton {
  color: inherit;
  background-color: yellow;
  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
  padding: 8px;
  margin-top: 20px;
}

.gamebutton:hover {
  cursor: pointer
}


/* h1 {
  margin: 0 auto;
  color: white;
}

.gamebutton {
  margin-top: 30px;
}

label {
  color: white;
  padding-top: 29px;
}

.container {
  background-color: #2b87d1;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  height: 500px;
}

.container p {
  color: white;
}

input {
  width: 300px;
  height: 30px;
  border: none;
  border-radius: 10px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px, rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
  margin-top: 70px;
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
} */
</style>