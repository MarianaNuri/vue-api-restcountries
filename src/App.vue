<template>

  <div>

    <header class="header">
      <h1>Explorador de Países</h1>
      <p>Consumo de API REST Countries con Vue 3</p>
    </header>

    <CountriesView />

  </div>

</template>

<script setup>

import CountriesView from "./components/CountriesView.vue"

</script>

<style>

body{
  margin:0;
  background:#f4f6f8;
}

.header{
  text-align:center;
  padding:40px;
  background:#42b883;
  color:white;
}

</style>