<script setup>
import trainerArea from "./trainerArea.vue";
import remaining from "./remaining.vue";
import { ref, nextTick, onMounted } from "vue";
const emit = defineEmits(["updateScore"]);

const Start = ref(true); // variable pour afficher l'espace de  démarrage du jeu
const Game = ref(false); // variable pour afficher l'espace du jeu
const End = ref(true); // variable pour afficher l'espace de resultat du jeu
let count = ref(); // variable pour compter le nombre de clic
let top = ref(0); //variable pour changer la position de la balle au clic vers le haut
let left = ref(0); //variable pour changer la position de la balle au clic vers la gauche
let second = ref(0); // variable pour compter le temps  en milisecond
let score = ref(0); // variable pour stocker le score
const numClicks = ref(5); // variable pour le nombre de clics de l'utilisateur sur la balle
let width = ref(65); //variable pour changer la taille de la balle à chaque clic
let height = ref(65); //variable pour changer la hauteur de la balle à chaque clic
let lon = ref(); // variable aléatoire  ajouté  pour changer la taille de la balle à chaque clic
let allScores = ref([]); // tableau pour stocker le temps mis par l'utilisateur à la fin du jeu
let couleurs = [
  "#FF5733",
  "#33FF57",
  "#3357FF",
  "#F833FF",
  "#33FFF8",
  "#FFFF33",
  "#FF33F8",
  "#33FFFB",
  "#FF8333",
  "#33FF83",
  "#3373FF",
  "#F883FF",
  "#3383FF",
  "#FF3383",
  "#83FF33",
  "#5733FF",
  "#FF573A",
  "#33FF5A",
  "#335AFF",
  "#F833FA",
]; // tableau de couleur

let a = ref(); // variable parcourir le tableau de couleur de façon aleatoire
let misses = ref(0); // variable pour compter le nombre de clics ratés

// Propriétés réactives pour les dimensions de la zone de jeu
const isChangeColorActive = ref(true); // variable pour changer la couleur de la balle si l'utilisateur coche la case changement ou variation de couleur au clic
const ischangePosition = ref(true); // variable pour changer la position de la balle au clic si l'utilisateur coche la case variation de position

const toggleColorChange = () => {
  colorChangeEnabled.value = !colorChangeEnabled.value;
}; // fonction pour changer la couleur de la balle au clic si l'utilisateur coche la case changement de coleur au clic

// fonction pour commencer le jeu et afficher l'espace de jeu au clic sur le bouton commencer
function startGame() {
  count.value = numClicks.value; // affecte le nombre de clic sur la balle au compteur
  misses.value = 0; //initialise le nombre de clic ratés à 0
  Start.value = false;
  Game.value = true;
  timer();
}

// fonction pour recommencer le jeu au clic sur le bouton reprendre
function restartGame() {
  second.value = 0; //initialise le temps à 0
  count.value = numClicks.value; // affecte le nombre de clic sur la balle au compteur
  Start.value = false;
  misses.value = 0; //initialise le nombre de clic ratés à 0
  Game.value = true;
  End.value = true;
  timer(); //appelle la fonction timer
}

// fonction pour recommencer le jeu au clic sur le bouton recommencer , il renvoie du l'espace de  démarrage du jeu avec choix
function recommencer() {
  Start.value = true;
  End.value = true;
  misses.value = 0; //initialise le nombre de clic ratés à 0
  second.value = 0; //initialise le temps à 0
}
// fonction pour changer la position de la balle au clic
function changePosition() {
  count.value--; //diminue le nombre de clic sur la balle au clic
  if (count.value === 0) {
    //si le nombre de clic est égal à 0 , on affiche l'espace de resultat du jeu
    Game.value = false;
    End.value = !true;
  }

  // Calculez la nouvelle position en tenant compte de la taille de la balle
 
  top.value = Math.floor(Math.random() * 60);
  left.value = Math.floor(Math.random() *80 );
  
  //condition pour changer  la  taille de la balle au clic quand la case variation de taille est coche
  if (ischangePosition.value) {
    lon.value = Math.floor(Math.random() * 200);
    width.value = lon.value;
    height.value = lon.value;
  }
  // condition pour changer la couleur de la balle au clic quand la case changement de couleur ou variation de couleur est coche
  if (isChangeColorActive.value) {
    a.value = Math.floor(Math.random() * couleurs.length - 1); // recuperer la valeaur aleatoire de l'index pour le d tableau de couleur
  }

  //appel de la fonction taille
  taille();
}
//fonction pour changer la taille de la balle au clic dans l'intervalle de 50px et 150 px
function taille() {
  //
  lon.value = Math.floor(Math.random() * 130);

  if (lon.value > 50 && lon.value < 150) {
    // si la valeur aleatoire  à ajouter au width et height, est comprise entre 50 et 150

    //affecter la valeur aleatoire au width et height
    width.value = lon.value;
    height.value = lon.value;
  } else {
    //sinon mettre le width et height a 60px
    width.value = 60;
    height.value = 60;
  }

}

//la fonction timer permet de compter le temps et est lancée au demarrage du jeu
function timer() {
  second.value++; // incremente le temps
  // console.log(second.value);
  let timeout = setTimeout(timer, 1); // appel de la fonction timer dans le  setTimeout pour lancer la fonction timer a chaque 1 milliseconde dans la variable timeout
  if (count.value === 0 && second.value !== 0) {
    //si le nombre de clic est égal à 0 et la valeur du temps est differente de 0, on affiche l'espace de resultat du jeu
    stockerScore();
    console.log(allScores.value);
    clearTimeout(timeout); // arrete la fonction timer
  }
}

//fonction pour recuperer les clics ratés

function handleMisses(event) {
  
  if (Game.value && event.target.className != "cercle") {
    //si le jeu est actif et que le clique n'est fait pas  sur le cercle , on incremente le nombre de clics ratés
    misses.value++;
  }

}

//fonction pour stocker les scores
function stockerScore() {
  allScores.value.push(second.value); // ajoute le temps du dernier jeu joué au tableau des scores
  emit("updateScore", allScores.value); // envoie le tableau des scores au composant information pour les afficher dans le composant gameArea
  allScores.value.sort((a, b) => a - b);
}





  

</script>


<template>
  <div class="wrap">
   
    <div class="container">
      <div v-if="Start" >
        <h1>Bienvenue dans le jeu !</h1>
        <div class="cercle"></div>
        <!-- <input type="number"> -->
        <div class="input-button">
          <p> Choisissez le nombre de cible et atteigner le, le plus vite possible !</p>
          <input
            type="number"
            id="clicks"
            v-model="numClicks"
            min="5"
            @input="validateNumClicks"
          /><br />
          <!-- entre du nombre de clics  que l'on veut faire dans la partie-->
          
            <ul>
              <li>
                <div>
                  <label for="1">Activer les couleurs aléatoires</label>
                  <input type="checkbox" v-model="isChangeColorActive" id="1"/>
                </div>
                <!-- coche pour changer la couleur de la balle-->
              </li>

              <li>
                <div>
                  <label for="2">Variation de la taille de la boule</label>
                  <input type="checkbox" v-model="ischangePosition" id="2">
                </div>
                <!-- coche pour changer la taille de la balle au clic -->
              </li>
            </ul>
          
          <button
            @click="startGame"
            :disabled="numClicks < 5" 
            class="gamebutton"
          >
            Commencer le jeu
          </button>
          <!-- bouton pour commencer le jeu avec nombre de clics superieur à 5 -->
        </div>
      </div>

      <main
       class="zone"
       v-if="Game"
       @click="handleMisses">
        <!--  div conteneur de l'espace du jeu  -->


        <div class="container-cercle">
          <div  class="ligne">
            <ul>
              <li>
                <p>Remaining: {{ count }}</p>
                <!--affiche le nombre de clics restant-->
              </li>
              <li>
                <p>Misses: {{ misses }}</p>
                <!--affiche le nombre de clics ratés-->
              </li>
            </ul>
          </div>
          <div
            class="cercle"
            :style="{
              top: top + '%',
              left: left + '%',
              width: width + 'px',
              backgroundColor: couleurs[a],
              height: height + 'px',
            }"
            @click="changePosition"
          >
            <!--la fonction changePosition  et le style permettent de changer la position de la balle , la taille de la balle et la couleur de la balle au clic-->
          </div>
        </div>
      </main>

      <div v-if="!End" class="end">
        <!--  div conteneur de l'espace de resultat du jeu  -->
        <div class="cercle"></div>
        <p class="cercle-p">Temps de jeu par click</p>
        <p class="second">{{ second + " ms" }}</p>
        <!-- affiche le temps du dernier jeu -->
        <h4 class="misses">Misses : {{ misses }}</h4>
        <h4 class="best">Meilleur score: {{ allScores[0] }} ms</h4>
        <!-- affiche le nombre de clics ratés -->
        <p class="save">Save your score to see how you compare</p>
        <div>
          <button class="restast-button" @click="restartGame">Reprendre</button>
          <!-- bouton pour reprendre le jeu -->
          <button class="recom" @click="recommencer">Recommencer</button>
        </div>
        <!-- bouton pour recommencer le jeu -->
      </div>
    </div>
  </div>
</template>

<style scoped>

* {
  box-sizing: border-box;
}

.container {
  width: 100%;
  height: 80vh;
  background-color: #2b87d1;
  /* display: flex; */
  justify-content: center;
  align-items: center;
  align-content: center;
  color: white;
}



h1 {
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 40px;
  margin-bottom: 20px;
}

li {
  padding: 10px;
  margin: 0;
}

ul {
  margin: 0;
  padding: 0;
  justify-content: center;
}

.ligne ul li {
  display: inline-block;
  padding: 30px;
}  

.start {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  align-items: center;
}


.zone {
 
 color: white;
 /* display: flex; */
 flex-direction: column;
 border: 2px solid rgb(2, 255, 225);
 width: 100%;
 height: 100%;
}
.container-cercle {
  /* padding: 30%; */
  width: 90%;
  height: 90%;
  margin: auto;
  padding: 10px;
  border: 1px solid orange;
  

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
  cursor: pointer; /*fait pointer le bouton */
}

.recom:hover {
  cursor: pointer; /*fait pointer le bouton */
}

.save {
  font-family: sans-serif;
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 20px;
  margin-top: 15px;
}

.second {
  margin: 0;
  padding: 0;
  text-align: center;
  font-weight: bold;
  font-size: 60px;
  font-family: sans-serif;
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
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 25px;
}


label {
  margin-right: 15px;
}


.input-button input {
  font-size: 25px;
  text-align: center;
  font-weight: 700;
  margin-top: 15px;
  height: 17px;
  width: 20px;

}

.input-button li {
display: flex;
text-align: center;
align-items: center;
align-content: center;
}
.input-button label {
  font-size: 18px;
}




.cercle {
  background-color: #ff000080;
  border: 1px solid #ff000080;
  border-radius: 50%;
  width: 100px;
  height: 103px;
  position: relative;
  top: 5px;
  left: 47%;
  margin-bottom: 15px;
 
}

#clicks {
  width: 300px;
  height: 30px;
  border: none;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
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
  cursor: pointer;
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

.misses {
  color: rgb(245, 77, 77);
  font-weight: 600;


  
}

.best {
  color: rgb(25, 255, 25);
  font-weight: 600;
}


.ligne {
  display: flex;
  justify-content: space-around; /* aligner les éléments sur la même ligne  et séparer les éléments  d'une marge */
}
li {
  list-style-type: none;
  gap: 50 px;
}

/* .wrap {
  text-align: center;
} */

.end {
  width: 700px;
  margin: 0 auto;
  /* display: flex; */
  flex-direction: column;
  justify-content: center;
 align-content: center;
 border: 1px greenyellow solid;
 text-align: center;
 font-size: 25px;
 padding: 20px;

}

/*tablette*/

@media screen and (max-width: 768px) {


  h1 {
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 20px;
  margin-bottom: 10px;
}

li {
  padding: 0px;
  margin: 10px;
}

ul {
  margin: 0;
  padding: 0;
  justify-content: center;
}




.container-cercle {
  padding: 10%;
  position: relative;
  width: 80%;

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
  transform: translateX(3px);
  margin-top: 35px;
  color: inherit;
  background-color: yellow;

  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
  margin-right: 50px;
}

.restast-button:hover {
  cursor: pointer; /*fait pointer le bouton */
}

.recom:hover {
  cursor: pointer; /*fait pointer le bouton */
}

.save {
  font-family: sans-serif;
  margin: 0;
  padding: 0;
  text-align: center;
  font-size: 20px;
  margin-top: 15px;
}

.second {
  margin: 0;
  padding: 0;
  text-align: center;
  font-weight: bold;
  font-size: 40px;
  font-family: sans-serif;
}

.container {

width: 100%;
background-color: #2b87d1;
display: flex;
justify-content: center;
align-items: center;
color: white;
margin: 0 auto;
margin-top: 20px;
}

.recom {
  margin: 0 auto;
  padding: 13px;
  text-align: center;
  transform: translateX(20px);
  margin-top: 35px;
  color: inherit;
  background-color: yellow;
  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
 margin : 0 auto;
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
  width: 85px;
  height: 82px;
  position: relative;
  top: 10px;
  left: 38%;
  /* margin: auto; */
  max-width: 100px;
  max-height: 100px;
  margin-top: 20px;
  margin-bottom: 20px;
}

#clicks {
  width: 300px;
  height: 30px;
  border: none;
  border-radius: 10px;
  box-shadow: rgba(8, 8, 8, 0.3) 0px 19px 38px,
    rgba(6, 6, 6, 0.22) 0px 15px 12px;
  /* margin-top: 70px; */
  margin-bottom: 10px;
  margin-top: 15px;
}



.gamebutton {
  color: inherit;
  background-color: yellow;
  border-radius: 10px;
  border: none;
  color: black;
  font-weight: bold;
  padding: 10px;
  margin-top: 25px;
}

.cercle-p {
color: white;
margin: 0;
padding: 10px;
text-align: center;
font-size: 30px;
margin-top: 15px;
}

}


/*mobile*/
@media screen and (max-width: 480px) {


h1 {
margin: 0;
padding: 0;
text-align: center;
font-size: 15px;
margin-bottom: 20px;
}




.cercle-p {
color: white;
margin: 0;
padding: 10px;
text-align: center;
font-size: 20px;
margin-top: 15px;
}

.restast-button {
margin: 0;
padding: 10px;
text-align: center;
transform: translateX(13px);
margin-top: 35px;
color: inherit;
background-color: yellow;

border-radius: 10px;
border: none;
color: black;
font-weight: bold;
margin-right: 50px;
}



.save {
font-family: sans-serif;
margin: 0;
padding: 0;
text-align: center;
font-size: 12px;
margin-top: 15px;
}

.second {
margin: 0;
padding: 0;
text-align: center;
font-weight: bold;
font-size: 35px;
font-family: sans-serif;
}

.container {

width: 100%;
background-color: #2b87d1;
display: flex;
justify-content: center;
align-items: center;
color: white;
margin: 0 auto;
margin-top: 20px;
}

.recom {
margin: 0 auto;
padding: 10px;
text-align: center;
transform: translateX(13px);
margin-top: 35px;
color: inherit;
background-color: yellow;
border-radius: 10px;
border: none;
color: black;
font-weight: bold;
margin : 0 auto;
}




#clicks {
width: 150px;
height: 30px;
border: none;
border-radius: 10px;
box-shadow: rgba(8, 8, 8, 0.3) 0px 19px 38px,
  rgba(3, 3, 3, 0.22) 0px 15px 12px;
margin-top: 10px;
margin-bottom: 20px;
}


.gamebutton {
color: inherit;
background-color:yellow;
border-radius: 10px;
border: none;
color: black;
font-weight: bold;
padding: 10px;
margin-top: 25px;
}

.cercle {
 

  border-radius: 50%;
  width: 70px;
  height: 70px;
  position: relative;
  top: 10px;
  left: 38%;
  /* margin: auto; */
  max-width: 80px;
  max-height: 80px;
  margin-bottom: 20px;
}

}



</style>
