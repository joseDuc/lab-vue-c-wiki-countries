<script setup>
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";
const route = useRoute();
//const routeParam = route.params.alpha3Code;
let alphaCodeCountries = ref(null);
async function obtainCountry() {
  try {
    const response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    if (!response.ok) throw new Error("Error al obtener los datos");
    const jsonResponse = await response.json();
    console.log(jsonResponse);
    alphaCodeCountries.value = jsonResponse.find(
      (alphaCodeCountries) =>
        alphaCodeCountries.alpha3Code === route.params.alpha3Code
    );
    console.log(alphaCodeCountries.value);
  } catch (err) {
    console.error("Something went wrong!", err);
  }
}
obtainCountry();
watch(
  () => route.params.alpha3Code,
  () => {
    obtainCountry();
  }
);
</script>
<template>
  <div class="container">
    <div class="countryEspecific" v-if="alphaCodeCountries?.name">
      <div class="tittleCountry">
        <img
          :src="`https://flagpedia.net/data/flags/icon/72x54/${alphaCodeCountries.alpha2Code.toLowerCase()}.png`"
          alt="Bandera de {{ alphaCodeCountries.name.common }}"
        />
        <h2>{{ alphaCodeCountries.name.common }}</h2>
      </div>
      <div class="info two-columns">
        <p><strong>Capital</strong></p>
        <div>
          <p v-for="capital in alphaCodeCountries.capital">{{ capital }}</p>
        </div>
      </div>
      <div class="two-columns">
        <p><strong>Area</strong></p>
        <p>{{ alphaCodeCountries.area.toLocaleString() }} km²</p>
      </div>
      <div class="borders two-columns">
        <p><strong>Borders</strong></p>
        <ul>
          <li v-for="border in alphaCodeCountries.borders" :key="border">
            <router-link :to="`/list/${border}`">{{ border }}</router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  width: 40%;
  margin-left: 0;
  margin-right: 0;
}
.countryEspecific {
  width: 500px;
  padding: 20px;
  background-color: #f8f9fa;
  border-left: 1px solid #7d91a5;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}
.tittleCountry {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}
img {
  width: 25%;
}
h2 {
  margin: 10px;
}
.two-columns {
  display: flex;
  align-items: flex-start;
  justify-content: space-around;
  ul{
    width: 100px;
  }
}
p {
  width: 100px;
}
</style>
