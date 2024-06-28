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
    
      <div class="annonce">
        <h2>C'est ici qu'une autre annonce s'affiche !</h2>
      </div>
      <div class="flex-container">
        <div class="third-container">
          <h3 class="">Entraîneur de visée</h3>
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
        <div class="graph third-container">
          <div><canvas id="myChartjs"></canvas></div>
        </div>
      </div>

      <div class="annonce">
        <h2>C'est ici qu'une autre annonce s'affiche !</h2>
      </div>
    
  </div>
</template>


<style scoped>

.container {
  width: 1200px;
  margin: auto;

}

h2 {
  text-align: center;
  padding-block: 2.5rem;
}

h3 {
text-align: center;
}


.flex-container {
  display: flex;
  justify-content: space-between;
  align-items: center;

}


.annonce {
  background-color: #fff;
  margin-block: 1.5rem;
  width: 100%;
  border-radius: 15px;
}
.third-container {
  background-color: #fff;
  height: 500px;
  width: 500px;
  line-height: 2.5rem;
  border-radius: 25px;
  padding-inline: 10px;
  text-align: justify;
  padding-inline: 2rem;
}


</style>