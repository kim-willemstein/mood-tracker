<script setup>
import {ref, onMounted, computed} from "vue";
const data = ref();

function fetchData() {
  return fetch('https://api.ouraring.com/v1/readiness?access_token=4NEDVOR74O626DUGSG4AQKM42NV44Z6V', {
    method: 'get',
  })
      .then(response => {
        if (!response.ok) {
          const error = new Error(response.statusText);
          error.json = response.json();
          throw error;
        }
        return response.json();
      })
      .then(response => {
        data.value = response.readiness[5].score;
      })
}

const calculateScore = computed(() => {
  if (data.value > 84) {
    return 'Perfect!'
  } else if (data.value > 70) {
    return 'Alright!'
  } else if (data.value > 60) {
    return 'kinda meh'
  } else if (data.value > 50){
    return 'dead'
  } else;
})

onMounted(() => {
  fetchData();
});
</script>

<template>
  <div>
    Today i'm feeling {{ calculateScore }}
    <br><br>
    Readiness score: {{ data }}
    <small>
      <br>
<!--      batterij tonen? Leuk als stukje art in rive animatie</small>-->
  </div>
</template>
