<script setup>
import trainerArea from "./trainerArea.vue"; 
import remaining from "./remaining.vue";
import { ref, nextTick } from "vue";
const emit = defineEmits (['updateScore'])

const Start = ref(true); // variable pour afficher l'espace de  démarrage du jeu
const Game = ref(false); // variable pour afficher l'espace du jeu
const End = ref(true); // variable pour afficher l'espace de resultat du jeu
let count = ref(); // variable pour compter le nombre de clic
let left = ref(0); //variable pour changer la position de la balle au clic vers la gauche
let top = ref(0); //variable pour changer la position de la balle au clic vers le haut
let second = ref(0); // variable pour compter le temps  en milisecond
let score = ref(0); // variable pour stocker le score
const numClicks = ref(5); // variable pour le nombre de clics de l'utilisateur sur la balle
let width = ref(50); //variable pour changer la taille de la balle à chaque clic
let height = ref(50); //variable pour changer la hauteur de la balle à chaque clic
let lon = ref () // variable aléatoire  ajouté  pour changer la taille de la balle à chaque clic
let allScores = ref ([]) // tableau pour stocker le temps mis par l'utilisateur à la fin du jeu
let  couleurs= ['#FF5733', '#33FF57', '#3357FF', '#F833FF', '#33FFF8',
'#FFFF33', '#FF33F8', '#33FFFB', '#FF8333', '#33FF83',
'#3373FF', '#F883FF', '#3383FF', '#FF3383', '#83FF33',
'#5733FF', '#FF573A', '#33FF5A', '#335AFF', '#F833FA'] // tableau de couleur
let a = ref(); // variable parcourir le tableau de couleur de façon aleatoire
let misses = ref(0) // variable pour compter le nombre de clics ratés
// const changePosition = ref(true)
// const colorChangeEnabled = ref(true);

// Propriétés réactives pour les dimensions de la zone de jeu
let containerCercleWidth = ref(800); // Largeur par défaut
let containerCercleHeight = ref(300); // Hauteur par défaut


const isChangeColorActive = ref(true); // variable pour changer la couleur de la balle si l'utilisateur coche la case changement ou variation de couleur au clic 
const ischangePosition = ref(true)  // variable pour changer la position de la balle au clic si l'utilisateur coche la case variation de position 

const toggleColorChange = () => {
  colorChangeEnabled.value = !colorChangeEnabled.value;
}; // fonction pour changer la couleur de la balle au clic si l'utilisateur coche la case changement de coleur au clic




// fonction pour commencer le jeu et afficher l'espace de jeu au clic sur le bouton commencer
function startGame() {
  count.value = numClicks.value  // affecte le nombre de clic sur la balle au compteur 
  Start.value = false; 
  Game.value = true; 
  // End.value = true;
  timer();
  nextTick(() => {
updateContainerCercleSize(containerCercleWidth.value, containerCercleHeight.value);
});
}


// fonction pour recommencer le jeu au clic sur le bouton reprendre
function restartGame() {
  second.value = 0; //initialise le temps à 0
  count.value = numClicks.value; // affecte le nombre de clic sur la balle au compteur
  Start.value = false;
  Game.value = true;
  End.value = true;
  misses.value = 0; //initialise le nombre de clic ratés à 0
  timer(); //appelle la fonction timer
}

// fonction pour recommencer le jeu au clic sur le bouton recommencer , il renvoie du l'espace de  démarrage du jeu avec choix 
function recommencer () {
    Start.value = true;
    End.value = true;
    misses.value = 0; //initialise le nombre de clic ratés à 0
    second.value = 0; //initialise le temps à 0
  }
 // fonction pour changer la position de la balle au clic
function changePosition() {
  count.value--; //diminue le nombre de clic sur la balle au clic
  if (count.value === 0) {  //si le nombre de clic est égal à 0 , on affiche l'espace de resultat du jeu
    Game.value = false;
    End.value = !true;
  }
 


  top.value = Math.floor(Math.random() * 100); // variable pour changer la position de la balle au clic vers le haut de facon aleatoire entre 0 et 100
  left.value = Math.floor(Math.random() * 100); // variable pour changer la position de la balle au clic vers la gauche de facon aleatoire entre 0 et 100


//condition pour changer  la  taille de la balle au clic quand la case variation de taille est coche
  if (ischangePosition.value) {
  lon.value = Math.floor(Math.random() * 200);
  width.value =lon.value;
  height.value =lon.value;
}
// condition pour changer la couleur de la balle au clic quand la case changement de couleur ou variation de couleur est coche
  if (isChangeColorActive.value  ) {

  a.value = Math.floor(Math.random() * couleurs.length-1); // recuperer la valeaur aleatoire de l'index pour le d tableau de couleur
  top.value = Math.floor(Math.random() * 250);  //  changer la position de la balle au clic vers le haut de facon aleatoire entre 0 et 250
  left.value = Math.floor(Math.random() * 250); // changer la position de la balle au clic vers la gauche de facon aleatoire entre 0 et 250
  }
  
//appel de la fonction taille
  taille();

}
 //fonction pour changer la taille de la balle au clic dans l'intervalle de 50px et 150 px
function taille(){
  //
  lon.value = Math.floor(Math.random() * 130);
    
  
  if (lon.value > 50 && lon.value<150  ){ // si la valeur aleatoire  à ajouter au width et height, est comprise entre 50 et 150

    //affecter la valeur aleatoire au width et height
    width.value =lon.value;
    height.value =lon.value;


   
  } else{ //sinon mettre le width et height a 60px
    width.value = 60;
    height.value = 60;

  }
  
  console.log(lon.value);  
}

//la fonction timer permet de compter le temps et est lancée au demarrage du jeu
function timer() {
  second.value++; // incremente le temps
  // console.log(second.value);
  let timeout = setTimeout(timer, 1); // appel de la fonction timer dans le  setTimeout pour lancer la fonction timer a chaque 1 milliseconde dans la variable timeout
  if (count.value === 0 && second.value !== 0) { //si le nombre de clic est égal à 0 et la valeur du temps est differente de 0, on affiche l'espace de resultat du jeu
    stockerScore()
    console.log(allScores.value);
    clearTimeout(timeout); // arrete la fonction timer
    
  }
  
}




   //fonction pour recuperer les clics ratés

   function handleMisses(element) {
    if (Game.value && element.target.className != "cercle"
    ) { //si le jeu est actif et que le clique n'est fait pas  sur le cercle , on incremente le nombre de clics ratés
      misses.value++
      
    }
    
    // console.log(misses.value);
    // console.log(element);
    
   }


 
  //fonction pour stocker les scores
   function stockerScore() {
    allScores.value.push(second.value); // ajoute le temps du dernier jeu joué au tableau des scores
    emit('updateScore', allScores.value)  // envoie le tableau des scores au composant information pour les afficher dans le composant gameArea
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
          <input type="number" id="clicks" v-model="numClicks" min="5" @input="validateNumClicks"><br>  <!-- entre du nombre de clics  que l'on veut faire dans la partie-->
          <label>
      <input type="checkbox"  v-model="isChangeColorActive"  >  <!-- coche pour changer la couleur de la balle-->
      Activer les couleurs aléatoires
              <input type="checkbox"  v-model="isChangeColorActive"  >
              Activer les couleurs aléatoires
          </label>
          <div class="checkbox2">
            <input type="checkbox" v-model="ischangePosition">  <!-- coche pour changer la taille de la balle au clic -->
            <label for="checkbox2">Variation de la taille de la boule</label>
          </div>
          <button @click="startGame" :disabled="numClicks < 5" class="gamebutton">Commencer le jeu</button>  <!-- bouton pour commencer le jeu avec nombre de clics superieur à 5 -->
      </div>
      
    </div>


    <main class="zone" v-if="Game" > <!--  div conteneur de l'espace du jeu  -->

      <ul class="ligne">
        <li>
          <p>Remaining: {{ count }}</p> <!--affiche le nombre de clics restant-->
        </li>
        <li>
          <p>Misses: {{ misses }}</p> <!--affiche le nombre de clics ratés-->
        </li>
      </ul>

      <!-- <div class="container-cercle" @click="handleMisses" >  div conteneur du l'espace du jeu qui prent en compte les clics ratés -->

      <div class="container-cercle" :style="{ width: containerCercleWidth + 'px', height: containerCercleHeight + 'px' }"  @click="handleMisses"  >

        <div
          class="cercle"
          :style="{ top: top + 'px', left: left + 'px' , width: width + 'px' , backgroundColor : couleurs[a],  height: height + 'px'} "
           @click="changePosition" >
          <!--la fonction changePosition  et le style permettent de changer la position de la balle , la taille de la balle et la couleur de la balle au clic-->
        </div>
      </div>
    </main>

    <div v-if="!End">  <!--  div conteneur de l'espace de resultat du jeu  -->
      <div class="cercle"></div>
      <p class="cercle-p">Temps moyen par click</p>
      <p class="second">{{ second + " ms" }}</p> <!-- affiche le temps du dernier jeu -->
      <div>Misses : {{ misses }}</div> <!-- affiche le nombre de clics ratés -->
      <p class="save">Save your score to see how you compare</p>
      <button class="restast-button" @click="restartGame">Reprendre</button> <!-- bouton pour reprendre le jeu -->
      <button class="recom" @click="recommencer"  >Recommencer</button> <!-- bouton pour recommencer le jeu -->
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
  width: 800px;
  overflow: hidden; /*evite le debordement de la balle*/
  
  padding: 10%; /* evite le debordement de la balle */
 
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
  cursor: pointer;  /*fait pointer le bouton */
}

.recom:hover {
  cursor: pointer /*fait pointer le bouton */
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
  justify-content: space-around; /* aligner les éléments sur la même ligne  et séparer les éléments  d'une marge */
}
li{
  list-style-type: none;
  gap:50 px;
}
 </style>