<script setup>
import { ref } from "vue";
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
} from "vue";

let countries = ref(null);

//onBeforeMount(()=> {console.log("On Before Mount");})
onMounted(() => {
  countries.value = obtainData();
});

async function obtainData() {
  try {
    const response = await fetch(
      "https://ih-countries-api.herokuapp.com/countries"
    );
    if (!response.ok) throw new Error("Error getting countries");
    const jsonResponse = await response.json();

    countries.value = orderLocalList(jsonResponse);
    // countries.value = jsonResponse;

    return jsonResponse;
  } catch (err) {
    console.error("Error data getting!", err);
  }
}

function orderList(list) {
  list.sort((a, b) => {
    if (a.name.common < b.name.common) {
      return -1;
    }
    if (a.name.common > b.name.common) {
      return 1;
    }
    return 0;
  });
  return list;
}

function orderLocalList(list) {
  list.sort((a, b) => a.name.common.localeCompare(b.name.common));
  return list;
}

//obtainData();
</script>

<template>
  <div class="container-country-list">
    <ul v-if="countries">
      <li
        v-for="(country, index) in countries"
        :key="index"
        class="country-item"
      >
        <router-link :to="`/list/${country.alpha3Code}`" class="country-link">
          <img
            :src="`https://flagpedia.net/data/flags/icon/72x54/${country.alpha2Code.toLowerCase()}.png`"
            alt="Flag of {{ alphaCodeCountries.name.common }}"
            class="flag"
          />
          <span class="country-name">{{ country.name.common }}</span>
        </router-link>
      </li>
    </ul>
  </div>
  <router-view></router-view>
</template>

<style scoped>
.container-country-list {
  display: flex;
  flex-direction: column;

  overflow-y: auto;
  width: 30%;
  border: 1px solid #ccc;
  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
}
.container-country-list::-webkit-scrollbar {
  width: 12px;
}
.containerCountriesList::-webkit-scrollbar-track {
  background: #f1f1f1;
}
.container-country-list::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
  height: 30px;
}
.container-country-list::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

.country-item {
  width: 100%;
  height: 150px;
  display: flex;
  align-items: center;
  transition: background-color 0.3s ease;
  border-bottom: 1px solid #e0e0e0;
  text-align: center;
}
.country-item:hover {
  background-color: #f0f0f0;
}
.country-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  color: #333;
  width: 100%;
}

.flag {
  width: 32px;
  height: auto;
  object-fit: cover;
}

.country-name {
  font-size: 16px;
  color: #1976d2;
}

@media (max-width: 800px) {
  .container-country-list {
      width: 50%;
      flex-direction: column;
    }
}

</style>
