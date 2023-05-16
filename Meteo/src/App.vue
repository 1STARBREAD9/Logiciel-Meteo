<script>
import { Suspense } from 'vue'
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "~" + this.city + "~"
    },

    showtemp() {
      return "Temperature" + this.info.main.temp

    },
    showFellsLike() {
      return "Ressenti comme" + this.info.main.feels_like
    },
    showMaxTemp() {
      return "Temperature maximale" + this.info.main.temp_max
    },
    showMinTemp() {
      return "Temperature minimale" + this.info.main.temp_min
    },
  },
  methods: {
    getMeteo() {
      if (this.city.trim().length < 2) {
        this.error = "Le nom de la ville est trop petit :"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8bafe57715b0a0e6e80bef9f5c83c485`)
        .then(res => (this.info = res.data))
    }

  }


}








</script>

<template>
  <div class="wrapper">
    <h1>Logiciel Météo</h1>
    <p>Savoir la météo {{ city == "" ? "dans votre ville" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Votre ville">
    <button v-if="city != ''" @click="getMeteo()">Savoir la météo</button>
    <button disabled v-else="city != ''">Entrez la ville</button>
    <p class="error"> {{ error }}</p>

    <div v-if="info != null">
      <p>{{ showtemp }}</p>
      <p>{{ showFellsLike }}</p>
      <p>{{ showMaxTemp }}</p>
      <p>{{ showMinTemp }}</p>
    </div>

  </div>
</template>

<style scoped>
.error {
  color: red;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background-color: rgb(35, 13, 112);
  padding: 20px;
  text-align: center;
  color: white;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 20px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid black;
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper button {
  background: orange;
  color: white;
  border-radius: 10px;
  border: 2px solid orange;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:focus {
  transform: scale(1.1) translateY(-5px);
}

.wrapper input:focus {
  border-bottom-color: brown;
}

.wrapper button:disabled {
  background: rgb(80, 60, 21);
  cursor: not-allowed;
}
</style>
