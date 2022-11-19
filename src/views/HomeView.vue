<script setup>
import {ref, onMounted, computed} from "vue";
const data = ref();
const key = import.meta.env.VITE_OURA_API_KEY;

function fetchData() {
  return fetch('https://api.ouraring.com/v1/readiness?access_token=' + key, {
    method: 'get',
    // headers: { 'Content-Type': 'application/json' }
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
        let readiness = response.readiness
        let lastValue = readiness[Object.keys(readiness)[Object.keys(readiness).length - 1]]
        data.value = lastValue.score;
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
  } else {}
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
    <br>
  </div>
</template>
