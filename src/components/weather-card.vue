<template>
  <section>
    <div id="weather-card-header">
      <h3>{{ city.name }}, {{ city.state }}, {{ city.country }}</h3>
      <h3>{{ todayForecasts.datetime }}</h3>
    </div>
    <div id="condition">
      <div>{{ todayForecasts.conditions }}</div>
      <img :src="`/icons/weather/${todayForecasts.icon}.svg`">
    </div>
    <div id="main-details">
      <span id="temperature">{{ temperature }}°{{tempType}}</span>
      <div id="humidity-wind">
            <span>
              <i class="fas fa-wind"></i>
              <span id="wind">{{ todayForecasts.windspeed }}</span>km/h
            </span>
        <span>|</span>
        <span>
              <i class="fas fa-tint"></i>
              <span id="humidity">{{ todayForecasts.humidity }}</span>%
            </span>
      </div>
    </div>
    <div id="other-details">
      {{ todayForecasts.description }}
    </div>
  </section>

</template>

<style scoped>

section {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 10px;
}

#main-details {
  text-align: center;
}

#weather-card-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

#temperature {
  font-size: 70px;
}

#condition {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#condition > div {
  margin: 30px 0;
}

#condition img {
  width: 150px;
}

#humidity-wind {
  font-size: medium;
}


#humidity-wind > span {
  margin: 0 5px;
}

#humidity {
  margin-left: 5px;
}

#wind {
  margin-left: 5px;

}

#other-details {
  margin: 60px 0;
}
</style>

<script setup>
import {computed, ref} from "vue";

let props = defineProps(['city', 'todayForecasts', 'tempType']);
let temperature = ref(computed(() =>  props.tempType === 'C' ? props.todayForecasts.temp : (props.todayForecasts?.temp * 9/5 ) + 32 ));


</script>