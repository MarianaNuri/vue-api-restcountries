<template>

<div class="page">

  <h2>Lista de países del mundo</h2>

  <!--Campo de busqueda-->
  <input type="text" placeholder="Buscar pais ..." v-model="busqueda" class="search">

  <!--USO DEL v-if-->
  <!--Indicador de carga con v-if-->
  <div v-if="loading || buscando" class="loader-container">
    <div class="loader"></div>
    <p>Cargando paises...</p>
  </div>

  <!--Mensaje si no hay resultados-->
  <div v-if="!loading && !buscando && paisesFiltrados.length === 0" class="no-results">
    No hay paises para mostrar
    <br>
    Intenta escribir otro

  </div>

  <div class="countries" v-if="!loading && paisesFiltrados.length > 0">

    <!-- LISTA GENERADA CON v-for -->

    <div
      class="card"
      v-for="pais in paisesFiltrados"
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
import { ref, onMounted, computed, watch } from "vue"

const paises = ref([])
const loading = ref(true)
const buscando = ref(false)
const busqueda = ref("")

onMounted(async () => {

  const inicio = Date.now()

  const respuesta = await fetch(
  "https://restcountries.com/v3.1/all?fields=name,flags,capital,region,subregion,population,languages,currencies"
  )

  const data = await respuesta.json()

  paises.value = data
  
  /*Retardo para que se carguen los datos */
  const tiempo = Date.now() - inicio
  const restante = 2000 - tiempo

  setTimeout (() => {
    loading.value = false
  }, restante > 0 ? restante : 0)

})

/*Filtro de busqueda */
const paisesFiltrados = computed (() => {
  return paises.value.filter(pais =>
    pais.name.common.toLowerCase().includes(busqueda.value.toLowerCase())
  )
})

/*Tiempo de busqueda*/
watch (busqueda, () => {
  buscando.value = true

  setTimeout(() => {
    buscando.value = false
  }, 2000)
})

</script>
<style>
body{
  background:#f5f7fb;
}


.page{
  width:100%;
  max-width: 1400px;
  margin:auto;
  padding:40px;
  font-family:Segoe UI, sans-serif;
}

h2{
  text-align:center;
  margin-bottom:30px;
}

.loader-container {
  text-align: center;
  margin-top: 30px;
}

.loader {
  border: 6px solid #f3f3f3;
  border-top: 6px solid #42b883;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto 10px auto;
}

@keyframes spin{
  0%{ transform: rotate(0deg);}
  100%{ transform: rotate(360deg);}
}

.no-results{
  text-align:center;
  margin-top:40px;
  font-size:18px;
  background:#fff3f3;
  color:#c0392b;
  padding:20px;
  border-radius:10px;
  border:1px solid #f5c6c6;
}

.countries{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
}

.search {
  display: block;
  margin: 0 auto 30px auto;
  padding: 10px;
  width: 300px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 16px;
}

.card{
 background:white;
  border-radius:14px;
  padding:18px;
  box-shadow:0 8px 20px rgba(0,0,0,0.08);
  transition:0.25s;
  text-align:center;
}

.card:hover{
  transform:translateY(-8px);
  box-shadow:0 12px 25px rgba(0,0,0,0.15);
}

.card img{
  width:100%;
  height:150px;
  object-fit:cover;
  border-radius:10px;
}

.card h3{
  margin-top:12px;
  font-size:20px;
  color:#333;
}

.card p{
  font-size:14px;
  color:#555;
  margin:5px 0;
}


.countries{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(260px,1fr));
  gap:25px;
}

</style>
