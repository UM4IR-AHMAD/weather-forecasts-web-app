
<template>
  <section id="main-section">
    <header>
      <logo/>
      <search @lat-lon="getWeatherForecasts" />
      <h3>
        <a @click.prevent="tempType = 'C'" href="#">°C</a> |<a @click.prevent="tempType = 'F'" href="#">°F</a>
      </h3>
    </header>
    <main v-if="forecastsData">
      <weatherCard :tempType="tempType" :city="selectedCity" :todayForecasts="forecastsData.days[0]" />
      <section id="daily-forecast-section">
        <h3>Daily Forecast</h3>
          <weather-mini-card :daysData="forecastsData.days"/>
      </section>
    </main>
    <fullSpinner v-if="loading"/>
    <error v-if="showError" @hide="hideError"/>
</section>
</template>

<style scoped>

#main-section{
  max-width: 1024px;
  min-width: 350px;
  border-radius: 5px;
  margin: 60px auto;
  background-color: #FAFAFA;
}

header{
  height: 30px;
  padding: 5px 10px;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0px 7px 5px 0px rgba(77, 2, 253, 0.295);

}

section#daily-forecast-section{
  padding: 0 10px;
}

section#daily-forecast-section h3{
  border-bottom: 2px solid black;
}


a{
  text-decoration: none;
}


</style>

<script setup>
import axios from "axios";
import {ref, computed} from "vue";
import logo from './components/header_partials/logo.vue';
import search from './components/header_partials/search.vue';
import weatherCard from './components/weather-card.vue';
import weatherMiniCard from './components/weather-mini-card.vue';
import fullSpinner from './components/others/spinner.vue';
import error from "./components/others/error.vue";

let tempType = ref('C');


let loading = ref(false);
let showError = ref(false);
let hideError = ()=> showError.value=false;

let selectedCity = ref(null);
let forecastsData = ref(null);
let getWeatherForecasts = (city) => {
  loading.value = true;
  axios.get(`https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/${city.lat},${city.lon}/next5days?unitGroup=metric&key=${import.meta.env.VITE_VISUALCROSSING_API_KEY}&include=days&elements=datetime,tempmax,tempmin,temp,humidity,windspeed,description,conditions,icon`)
      .then(response => {
        console.log(response.data);
        selectedCity.value = city;
        forecastsData.value = response.data;
        loading.value = false;
      })
      .catch(error => {
        loading.value = false;
        showError.value = true;
      });
}
</script>
