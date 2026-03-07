<template>

<div class="page">

  <h2>Lista de países del mundo</h2>

  <!--Indicador de carga-->
  <p v-if="loading">Cargando paises...</p>

  <p v-if="!loading && paises.length === 0">
    No hay paises para mostrar
  </p>

  <div class="countries" v-if="!loading && paises.length > 0">

    <!-- LISTA GENERADA CON v-for -->

    <div
      class="card"
      v-for="pais in paises"
      :key="pais.name.common"
    >

      <img :src="pais.flags.png" />

      <h3>{{ pais.name.common }}</h3>

      <p><strong>Capital:</strong> {{ pais.capital?.[0] }}</p>

      <p><strong>Región:</strong> {{ pais.region }}</p>

      <p><strong>Población:</strong> {{ pais.population.toLocaleString() }}</p>

    </div>

  </div>

</div>

</template>


<script setup>

import { ref, onMounted } from "vue"

const paises = ref([])
const loading = ref(true)

onMounted(async () => {

  const respuesta = await fetch(
  "https://restcountries.com/v3.1/all?fields=name,flags,capital,region,subregion,population,languages,currencies"
  )

  const data = await respuesta.json()

  paises.value = data
  loading.value = false

})

</script>


<style>

.page{
  max-width:1200px;
  margin:auto;
  padding:40px;
  font-family:Segoe UI;
}

h2{
  text-align:center;
  margin-bottom:30px;
}

.countries{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
  gap:20px;
}

.card{
  background:white;
  border-radius:12px;
  padding:15px;
  box-shadow:0 4px 10px rgba(0,0,0,0.1);
  transition:0.3s;
}

.card:hover{
  transform:translateY(-5px);
}

.card img{
  width:100%;
  border-radius:8px;
}

.card h3{
  margin-top:10px;
}

</style>
