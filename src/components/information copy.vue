<script setup>
import { ref, onMounted, watch } from "vue";
import Chart from "chart.js/auto";

const myScores = ref([]); // variable pour récupérer les scores

const label = ref([]);

let count = ref(1);

const props = defineProps({
  scores: {
    type: Array,
  },
});

watch(
  () => props.scores,
  (score) => {
    myScores.value = score;
    console.log(score);
    label.value.push(`partie ${count.value++}`);
    updateChart();
    console.log("myScores => ", myScores.value);
    console.log(label.value);
  }
);

async function chartjs() {
  const ctx = document.getElementById("myChartjs");

  new Chart(ctx, {
    type: "line",
    data: {
      labels: label.value,
      datasets: [
        {
          title : "Statistiques"
          label: `Score - Meilleur score ${myScores.value[0]}`,
          data: myScores.value,
          borderColor: "rgb(43,135,209)",
          tension: 0.5,
          // cubicInterpolationMode: 'cubic-bezier',
        },
      ],
    },
  });
}

function updateChart() {
  if (chartInstance) {
    chartInstance.destroy();
  }
  chartjs();
}

onMounted(() => {
  myScores.value = JSON.parse(localStorage.getItem("allScores"));

  myScores.value.forEach((partie) => {
    label.value.push(
      `Partie ${count.value <= myScores.value.length ? count.value++ : ""}`
    );
    console.log(count.value);
  });

  chartjs();
});
</script>

<template>
  <div class="container">
    <div class="wrap">
      <div class="second-container">
        <p>C'est ici qu'une autre annonce s'affiche !</p>
      </div>
      <div class="flex-container">
        <div class="third-container">
          <h2>Entraîneur de visée</h2>
          <p>
            Cliquez sur les cibles aussi rapidement et précisément que possible.
          </p>
          <p>Cela teste les réflexes et la coordination œil-main.</p>
          <p>
            Une fois que vous aurez cliqué sur 30 cibles, votre score et votre
            temps moyen par cible seront affichés.
          </p>
          <p>
            Les scores de ce test sont plus lents que ceux du simple test de temps
            de réaction, car vous devez réagir puis déplacer le curseur.
          </p>
          <p>
            Il est préférable de réaliser ce test avec un écran de souris ou de
            tablette. Il est difficile de réussir avec les trackpads.
          </p>
        </div>
      </div>
      <div class="container-five">
        <h2>C'est ici qu'une autre annonce s'affiche !</h2>
      </div>
    </div>
    <div class="graph">
      <div>
        <h3>Statistiques</h3>
        <canvas id="myChartjs"></canvas>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: #e6e6e6;
  display: flex;
  gap: 20px;
  width: 900px;
  margin: 0 auto;
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
.graph {
  background-color: white;
  padding-left: 0px;
  width: 600px;
  height: 600px;
  display: flex;
  flex-direction: row;
}

.container-five {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 1000px;
  height: 150px;
  margin-top: 10px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 40px;
  background-color: #f8f4f4;
}

.flex-container {
  display: flex;
  margin: 0 auto;
}
.fourth-container {
  width: 100%;
  height: auto;
}

/*tablette*/

@media screen and (max-width: 768px) {
  .container {
    background-color: #e6e6e6;
  }

  .second-container {
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 300px;
    height: 150px;
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 15px;
    background-color: #fff;
  }

  .third-container {
    background-color: rgb(253, 232, 99);
    padding: 30px;
    padding-right: 2px;
    margin: 0 auto;

    height: auto;
  }
  .container-four {
    background-color: white;
    padding-left: 20px;
    margin: 0 auto;
    width: 500px;
    height: auto;
    display: flex;
    flex-direction: column;
    column-count: 2;
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
    width: 300px;
    height: 150px;
    /* margin-top: 20px;
  margin-bottom: 20px; */
    font-weight: bold;
    font-size: 25px;
    background-color: #e8d796;

    margin: 0 auto;
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

  h2 {
    font-size: 15px;
  }
}

/*mobile*/

@media screen and (max-width: 469px) {
  .container {
    background-color: #e6e6e6;
  }

  .second-container {
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 300px;
    height: 150px;
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 15px;
    background-color: #fff;
  }

  .third-container {
    background-color: rgb(253, 247, 211);
    padding: 30px;
    padding-right: 2px;
    margin: 0 auto;

    height: auto;
  }
  .container-four {
    background-color: white;
    padding-left: 20px;
    margin: 0 auto;
    width: 500px;
    height: auto;
    display: flex;
    flex-direction: column;
    column-count: 2;
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
    width: 300px;
    height: 150px;
    /* margin-top: 20px;
margin-bottom: 20px; */
    font-weight: bold;
    font-size: 25px;
    background-color: #e8d796;

    margin: 0 auto;
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
    flex-direction: column;
    margin: 0 auto;
  }
  /* .fourth-container {


width: 100%; 
height: auto;



} */

  h2 {
    font-size: 15px;
  }
}
</style>
