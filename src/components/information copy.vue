<script setup>
import { ref, onMounted, watch } from 'vue';
import Chart from 'chart.js/auto';


const myScores = ref([])

const label = ref([])

let count = ref(0)

const props = defineProps({
  scores : {
    type:Array,
  },
 
})


watch(()=>props.scores,(score)=>{
  myScores.value = score
  label.value.push(`partie ${count.value++}`)
  // chartjs();
  console.log("myScores => ", myScores.value);
  console.log(label.value);
})


async function chartjs() {

  const canvas = document.getElementById('myChart');


  if (Chart.instances.length > 0) {
        Chart.instances.forEach(instance => {
            if (instance.chart.canvas.id === 'myChart') {
                instance.destroy();
            }
        });
    }

    const ctx = canvas.getContext('2d');


  new Chart(
    ctx,
    {
      type: 'line',
      data: {
        labels: label.value,
        datasets: [
          {
            label: '',
            data: myScores.value,
            borderColor: 'rgb(43,135,209)',
            tension: 0.5,
            // cubicInterpolationMode: 'cubic-bezier',
          }
        ]
      },
      
    }
  );
}

watch(() => myScores.value,  chartjs);


onMounted(() => {
  // chartjs();
  myScores.value.forEach(partie => {
  label.value.push(`partie ${count.value++}`)
  console.log(count.value);
});
  console.log(label.value);
});

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
        <div v-for="(score, index) in myScores" :key="index">
              <h4>Score {{ index + 1 }}: {{ score }}</h4>
          </div>
          <div style="width: 800px;"><canvas id="myChartjs"></canvas></div>
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


</style>