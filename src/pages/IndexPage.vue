<template>
<q-card class="my-card" v-for="(Code, index) in Codes" :key="index">
      <img :src="codesReference[Code].day.image" height="100" style="width:100px">
      <q-card-section>
        <div class="text-h6">{{codesReference[Code].day.description}}</div>
        <div class="text-subtitle2">by Me</div>
      </q-card-section>
      <q-card-section class="q-pt-none">
        Bonjour tout le monde
      </q-card-section>
    </q-card>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import codes from 'src/data/code.json'

defineOptions({
  name: 'IndexPage'
});

const meteoD = ref('')
const imageD = ref('')
const Codes= ref([])
const codesReference = ref(codes)

onMounted (async () => {
  const meteo = await axios.get('https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&daily=weather_code,temperature_2m_max,temperature_2m_min,sunrise,sunset,uv_index_max,uv_index_clear_sky_max&current=weather_code')
  const etat= meteo.data.current.weather_code
  Codes.value = meteo.data.daily.weather_code
  imageD.value =codes[etat].day.image
  meteoD.value = codes[etat].day.description
})
