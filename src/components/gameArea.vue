<script setup>
import trainerArea from "./trainerArea.vue";
import remaining from "./remaining.vue";
import { ref, nextTick } from "vue";
const emit = defineEmits (['updateScore'])

const Start = ref(true);
const Game = ref(false);
const End = ref(true);
let count = ref();
let left = ref(0);
let top = ref(0);
let second = ref(0);
let score = ref(0);
const numClicks = ref(5);
let width = ref(50);
let height = ref(50);
let lon = ref ()
let allScores = ref ([])
let  couleurs= ['#FF5733', '#33FF57', '#3357FF', '#F833FF', '#33FFF8',
'#FFFF33', '#FF33F8', '#33FFFB', '#FF8333', '#33FF83',
'#3373FF', '#F883FF', '#3383FF', '#FF3383', '#83FF33',
'#5733FF', '#FF573A', '#33FF5A', '#335AFF', '#F833FA']
let a = ref();
let misses = ref(0) // variable pour stocker

// Propriétés réactives pour les dimensions de la zone de jeu
let containerCercleWidth = ref(800); // Largeur par défaut
let containerCercleHeight = ref(300); // Hauteur par défaut


const isChangeColorActive = ref(true);
const ischangePosition = ref(true)

const toggleColorChange = () => {
  colorChangeEnabled.value = !colorChangeEnabled.value;
};

function startGame() {
  count.value = numClicks.value
  Start.value = false;
  Game.value = true;
  // End.value = true;
  timer();
  nextTick(() => {
updateContainerCercleSize(containerCercleWidth.value, containerCercleHeight.value);
});
}



function restartGame() {
  second.value = 0;
  count.value = numClicks.value;
  Start.value = false;
  Game.value = true;
  End.value = true;
  misses.value = 0;
  timer();
}

function recommencer () {
    Start.value = true;
    End.value = true;
    misses.value = 0;
    second.value = 0;
  }

function changePosition() {
  count.value--;
  if (count.value === 0) {
    Game.value = false;
    End.value = !true;
  }
 


  top.value = Math.floor(Math.random() * 100);
  left.value = Math.floor(Math.random() * 100);



  if (ischangePosition.value) {
  lon.value = Math.floor(Math.random() * 200);
  width.value =lon.value;
  height.value =lon.value;
}

  if (isChangeColorActive.value  ) {

  a.value = Math.floor(Math.random() * couleurs.length-1);
  top.value = Math.floor(Math.random() * 250);
  left.value = Math.floor(Math.random() * 250);
  }
  

  taille();

}

function taille(){
  
  lon.value = Math.floor(Math.random() * 130);
    
  
  if (lon.value > 50 && lon.value<150  ){

    
    width.value =lon.value;
    height.value =lon.value;


   
  } else{
    width.value = 60;
    height.value = 60;

  }
  
  console.log(lon.value);  
}


function timer() {
  second.value++;
  // console.log(second.value);
  let timeout = setTimeout(timer, 1);
  if (count.value === 0 && second.value !== 0) {
    stockerScore()
    console.log(allScores.value);
    clearTimeout(timeout);
    
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


 

   function stockerScore() {
    allScores.value.push(second.value); // Ajoutez le score actuel au tableau des scores
    emit('updateScore', allScores.value)
}



// Fonction pour mettre à jour les dimensions
function updateContainerCercleSize(newWidth, newHeight) {
containerCercleWidth.value = newWidth;
containerCercleHeight.value = newHeight;
}


console.log(containerCercleHeight);


</script>



<template>

<div class="container">


    <!-- Ajouter des champs de saisie pour les dimensions -->
    <div v-if="Start" class="dimensions-cercle-container">
      <h2>Veillez saisir les dimensions de votre espace de jeu:</h2>
        <label for="">Largeur: 
          <input type="number" v-model="containerCercleWidth"  min="100" placeholder="Largeur de la zone de jeu" />
        </label><br>
        <label for=""> Hauteur: 
          <input type="number" v-model="containerCercleHeight"  min="100" placeholder="Hauteur de la zone de jeu" />
        </label>
    </div>

  <div class="container" @click="handleMisses">
    <div v-if="Start">
      <h1>Bienvenue dans le jeu !</h1>
      <div class="cercle" ></div>
      <p>
        Choisissez le nombre de cible et atteigner le, le plus vite possible !
      </p>
      <!-- <input type="number"> -->
      <div class="input-button">
          <input type="number" id="clicks" v-model="numClicks" min="5" @input="validateNumClicks"><br>
          <label>
              <input type="checkbox"  v-model="isChangeColorActive"  >
              Activer les couleurs aléatoires
          </label>
          <div class="checkbox2">
            <input type="checkbox" v-model="ischangePosition">
            <label for="checkbox2">Variation de la taille de la boule</label>
          </div>
          <button @click="startGame" :disabled="numClicks < 5" class="gamebutton">Commencer le jeu</button>
      </div>
      
    </div>

   


    <main class="zone" v-if="Game" >

      <ul class="ligne">
        <li>
          <p>Remaining: {{ count }}</p>
        </li>
        <li>
          <p>Misses: {{ misses }}</p>
        </li>
      </ul>

    

      <div class="container-cercle" :style="{ width: containerCercleWidth + 'px', height: containerCercleHeight + 'px' }"  @click="handleMisses"  >
        <div
          class="cercle"
          :style="{ top: top + 'px', left: left + 'px' , width: width + 'px' , backgroundColor : couleurs[a],  height: height + 'px'} "
           @click="changePosition"></div>
      </div>
    </main>

    <div v-if="!End">
      <div class="cercle"></div>
      <p class="cercle-p">Temps moyen par click</p>
      <p class="second">{{ second + " ms" }}</p>
      <div>Misses : {{ misses }}</div>
      <p class="save">Save your score to see how you compare</p>
      <button class="restast-button" @click="restartGame">Reprendre</button>
      <button class="recom" @click="recommencer"  >Recommencer</button>
    </div>
  </div>
</div>
</template>


<style scoped>

.dimensions-cercle-container {
  display: flex;
  flex-direction: column;
  
}


.container-cercle {

  /* height: 300px;
  width: 800px; */
border: 5px solid rgb(89, 255, 0);
  height: 300px;
  width: 600px;
  overflow: hidden;
  border: 1px solid darkturquoise;
  padding: 10%;

}



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
  transform: translateX(10px);
  margin-top: 35px;
  color: inherit;
  background-color: yellow;
  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
}

.restast-button:hover {
  cursor: pointer;
}

.recom:hover {
  cursor: pointer
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
  height: 550px;
  background-color: #2b87d1;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}

.recom {
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

.input-button {
  margin: 0;
  padding: 0;
  text-align: center;
}

/* main h1  {
  margin-bottom: 100px;
 
} */

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
  flex-wrap: wrap;
  max-width: 150px;
  overflow: initial;
 
  
}

#clicks {
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

.zone {
/* display: flex;
justify-content: center; */

  color: white;

  /* font-size: 22px; */

}

span {
    padding-left: 110px;
}
 .miss {

  margin-bottom: 50px;
  font-size: 30px;
  font-weight: bold;
  display: flex;
  padding-left: 70px;
/* justify-content: center; */
 }

h4 {
  color: red;
}
.ligne{
  display: flex;
  justify-content: space-around;
}
li{
  list-style-type: none;
  gap:50 px;
}
 </style>