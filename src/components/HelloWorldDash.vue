<template>
  <h1>{{ msg }}</h1>

  <p>
    <a href="https://vitejs.dev/guide/features.html" target="_blank">
      Vite Documentation
    </a>
    |
    <a href="https://v3.vuejs.org/" target="_blank">Vue 3 Documentation</a>
  </p>

  <p>
    <button type="button" class="ourbutton" @click="state.count++">count is: {{ state.count }}</button>
    <input type="number" v-model="state.price">
  </p>
  <p>
    Akcje:
    <button type="button" class="ourbutton" @click="foo(state.count)">Alert</button>
    <button type="button" class="ourbutton" @click="callApi()"> Sprawdź ceny</button>
    <button type="button" class="ourbutton" @click="getLocation()"> Sprawdź pozycję</button>
  </p>
  <p>
    Widoczna podana cena: {{ state.price }}
    <br>
    <span v-if="state.longitude">Pozycja: x={{state.longitude}}&y={{state.latitude}} </span>
  </p>
  <div>
    <strong>Z naszej bazy...</strong>
    <ul>
      <li v-for="p in state.prices">
        {{ p.cenakg }}
      </li>
    </ul>
  </div>


</template>


<!--------------------------------->

<script setup>
import {defineProps, reactive} from 'vue'

//Wczytywane do komponentu (przez inny który go używa)
defineProps({
  msg: String
})

//Dane obecnego komponentu
const state = reactive({count: 10, price: 8.50, prices: []})


//Metody/funkcje których będziemy używali w komponencie
let foo = (e) => {
  alert(`Msg: ${e}`);
}

//Przykład działającego zapytania http
let callApi = async () => {
  // const response = await fetch("https://jsonplaceholder.typicode.com/todos");
  const response = await fetch("http://10.10.0.200:3030/ceny");
  state.prices = await response.json();
}

//Lokalizacja
const showPosition = position => {
  state.longitude = position.coords.longitude;
  state.latitude = position.coords.latitude;
};


const getLocation = () => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
    console.log("Geolocation is not supported by this browser.");
  }
}



</script>

<!---------------------------------------->
<!--Style komponentu-->
<style scoped>
a {
  color: #42b983;
}

.ourbutton {
  background-color: lightblue;
  height: 25px;
  border-radius: 5px;
}

</style>
