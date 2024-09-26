<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input class="search-bar" type="text" placeholder="Search Country/City ..." v-model="query"
          @keypress="fetchData">
      </div>
      <div v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.sys.country }}, {{ weather.name }}</div>
          <div class="date">{{ nowDate }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.floor(weather.main.temp - 273) }}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      apiKey: 'a9b90d4ff1b93c787f5b7c13cd3d6dd7',
      api: 'https://api.openweathermap.org/data/2.5',
      query: '',
      weather: {},
      nowDate: ``,
    }
  },
  methods: {
    fetchData(event) {
      if (event.key == 'Enter') {
        /* fetch(`${this.api}/weather?q=${this.query}&appid=${this.apiKey}`)
           .then(res => {
             return res.json()
           }).then(response => {
             console.log(response)
             this.weather = response
           })*/
        axios.get(`${this.api}/weather?q=${this.query}&appid=${this.apiKey}`)
          .then(response => {
            this.weather = response.data
          })
      }
    },
    nowDateCalcute() {
      let monthNames = [
        "January", "February", "March", "April", "May", "June",
        "July", "August", "September", "October", "November", "December"
      ]
      let weekDays = ['Sunday', 'Monday', 'Tuesday', 'Wedneday', 'Thursday', 'Friday', 'Saturday']
      let date = new Date()
      let year = date.getFullYear();
      let month = date.getMonth();
      let day = date.getDate();
      let weekDay = date.getDay();
      this.nowDate = `${weekDays[weekDay]} ${day} ${monthNames[month]} ${year}`
    },
  },
  mounted() {
    this.nowDateCalcute()
  }
}


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  background: linear-gradient(135deg, #89CFF0, #87CEFA);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}


main {
  background-color: rgba(255, 255, 255, 0.1);
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
  text-align: center;
  width: 350px;
  opacity: 0;
  transform: translateY(50px);
  animation: fadeInUp 1s ease forwards;
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.search-box {
  margin-bottom: 20px;
}

.search-bar {
  width: 100%;
  padding: 10px;
  border-radius: 25px;
  border: none;
  outline: none;
  font-size: 16px;
  transition: transform 0.3s ease;
}

.search-bar:focus {
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

.search-bar::placeholder {
  color: rgba(34, 30, 30, 0.6);
}

.location-box {
  margin-bottom: 20px;
}

.location {
  font-size: 24px;
  font-weight: bold;
  animation: fadeIn 1.2s ease-in-out;
}

.date {
  font-size: 18px;
  font-weight: 300;
  animation: fadeIn 1.4s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

.weather-box {
  margin-top: 20px;
  animation: fadeInUp 1.6s ease-in-out;
}

.temp {
  font-size: 48px;
  font-weight: bold;
}

.weather {
  font-size: 24px;
  font-style: italic;
}

.weather-box .temp::after {
  content: "°C";
}
</style>
