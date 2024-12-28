<template>
  <div  id="error-wrapper">
    <div id="error">
      <h1 id="error-icon"><i class="fas fa-exclamation-circle"></i></h1>
      <div>{{ msg??null ? msg : 'Weather forecasts request failed.' }}</div>
      <div>Try again.</div>
      <button @click="$emit('hide')">OK</button>
    </div>
  </div>
</template>

<style scoped>
#error-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

#error {
  text-align: center;
  padding: 20px;
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid white;
  width: 250px;
  background: white;
}

#error-icon{
  font-size: xxx-large;
  padding: 0;
  margin: 0;
  color: red;
}

#error div:nth-child(3){
  margin: 10px 0;
  color: red;
}

</style>

<script setup>
let props = defineProps(['msg']);

if (props.msg)
  console.log(props.msg);

</script>
