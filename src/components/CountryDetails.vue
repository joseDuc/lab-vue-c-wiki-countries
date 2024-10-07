<script setup>
import { ref, onMounted, watch } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
let country = ref(null);

onMounted(() => {
  country.value = obtainCountry();
});

watch(
  () => route.params.alpha3Code,
  () => {
    obtainCountry();
  }
);

async function obtainCountry() {
  try {
    const response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries/" +
        route.params.alpha3Code
    );

    if (!response.ok) throw new Error("Error getting data");
    country.value = await response.json();

    /*const response1 = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    */
    /*alphaCodeCountries.value = jsonResponse.find(
      (alphaCodeCountries) =>
        alphaCodeCountries.alpha3Code === route.params.alpha3Code
    );
    */
  } catch (err) {
    console.error("Something went wrong!", err);
  }
}
//obtainCountry();
</script>
<template>
  <div class="container">
    <div class="country" v-if="country?.name">
      <div class="title-country">
        <img
          :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
          alt="Flag of {{ country.name.common }}"
        />
        <h2>{{ country.name.common }}</h2>
      </div>
      <div class="data-par">
        <p><strong>Capital</strong></p>
        <div>
          <p v-for="capital in country.capital">{{ capital }}</p>
        </div>
      </div>
      <div class="data-par">
        <p><strong>Area</strong></p>
        <p>{{ country.area.toLocaleString() }} km²</p>
      </div>
      <div class="data-par">
        <p><strong>Borders</strong></p>
        <ul>
          <li v-for="border in country.borders" :key="border">
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
  .country {
    width: 500px;
    padding: 20px;
    background-color: #f8f9fa;
    border-left: 1px solid #7d91a5;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;

    .title-country {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 30px;

      img {
        width: 25%;
      }

      h2 {
        margin: 10px;
      }
    }

    .data-par {
      display: flex;
      align-items: flex-start;
      justify-content: space-around;
      ul {
        width: 100px;
      }

      p {
        width: 100px;
      }
    }
  }
}
</style>
