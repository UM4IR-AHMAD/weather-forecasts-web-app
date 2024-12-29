<template>
  <div>
    <div class="search-input-wrapper">
      <input @input="searchApiCall" v-model="searchInput" type="text" id="search-input" placeholder="Enter City Name">
      <ul id="dropdownList" class="dropdown-list">
        <li v-for="city in top5Cities" :id="city.lat+city.lon" @click="extractLatLon(city)" class="dropdown-item">
          {{ city.name }}, {{ city.state }}, {{ city.country }}
        </li>
      </ul>
    </div>
    <fullSpinner v-if="loading"/>
    <error v-if="showError" @hide="hideError" :msg="errorMsg"/>
  </div>
</template>

<style scoped>
/*input{*/
/*    position: relative;*/
/*    height: 100%;*/
/*    border: 0;*/
/*}*/

button {
  border: 0;
}

button:hover {
  cursor: pointer;
}


.search-input-wrapper {
  /*width: 200px;*/
  height: 100%;
  position: relative;
}

#search-input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

.dropdown-list {
  display: block;
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  border: 1px solid #ccc;
  background-color: white;
  list-style-type: none;
  margin: 0;
  padding: 0;
  max-height: 200px;
  overflow-y: auto;
}

.dropdown-item {
  padding: 8px;
  cursor: pointer;
}

.dropdown-item:hover {
  background-color: #f0f0f0;
}

</style>
<script setup>
import {ref, reactive} from 'vue';
import axios from 'axios';
import fullSpinner from '../others/spinner.vue';
import error from '../others/error.vue';


const emits = defineEmits(['latLon']);
let loading = ref(false);

let showError = ref(false);
let hideError = () => {
  showError.value = false;
  errorMsg.value = null;
};
let errorMsg = ref(null);


let searchInput = ref('');
let timeout;
let searchApiCall = () => {
  clearTimeout(timeout);
  timeout = setTimeout(() => {
    if (searchInput.value.trim()) {
      getTop5Places();
    }
  }, 1200);
}


let top5Cities = ref(null);
let getTop5Places = () => {
  loading.value = true;
  axios.get(`http://api.openweathermap.org/geo/1.0/direct?q=${searchInput.value}&limit=5&appid=${import.meta.env.VITE_OPENWEATHERMAP_APP_ID}`)
      .then(response => {
        if (response.data.length < 1) {
          console.log(response.data);
          errorMsg.value = 'This city is not exists';
          showError.value = true;
        }
        top5Cities.value = response.data;
        loading.value = false;
      })
      .catch(error => {
        showError.value = true;
        loading.value = false;
      });
}


let extractLatLon = city => {
  searchInput.value = `${city.name}, ${city.state}, ${city.country}`;
  top5Cities.value = null;
  emits('latLon', city);
}

</script>