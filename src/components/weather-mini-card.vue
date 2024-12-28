<template>
  <section>
    <template v-for="(data, index) in daysData">
      <div v-if="index>0" id="mini-card">
        <div class="date">{{ getDayName(data.datetime) }}</div>
        <div class="day">{{ data.conditions }}</div>
        <h2 class="condition">
          <img :src="`/icons/weather/${data.icon}.svg`">
        </h2>
        <div class="temperatures">
          <span>Low: <span id="low">{{ data.tempmin }}</span>°c</span>
          |
          <span>High: <span id="high">{{ data.tempmax }}</span>°c</span>
        </div>
      </div>
    </template>
  </section>
</template>


<style scoped>
section {
  font-size: small;
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
}

#mini-card {
  /*flex: 1;*/
  /*min-width: 200px;*/
  margin: 10px 0;
  text-align: center;
  box-shadow: 0 2px 3px 0 rgba(0, 0, 0, 0.3);
  padding: 5px;
}

.condition {
  margin: 10px 0;
}

.condition > img {
  width: 100px;
  height: 50px;
}

.date {
  font-weight: bolder;
}
</style>

<script setup>
let props = defineProps(['daysData', 'weatherIcons']);

let getDayName = (date) => new Date(date).toLocaleString('en-US', {weekday: 'long'});

</script>
