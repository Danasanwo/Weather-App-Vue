<template>
  <div id="app" :class=" weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input
          type="text"
          name="search-bar"
          v-model="query"
          placeholder="search..."
          class="search-bar"
          
        />
        <input
          type="submit"
          @click="fetchWeather"
          value="search"
          class="submit"
        />
      </div>
      <!-- <Details /> -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
// import Details from './components/Details'

export default {
  name: "App",
  // components: 'Details'
  // ,
  data() {
    return {
      api_key: "b9c3cdd15c3bcdffd87a1959563caaee",
      base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather() {
      fetch(
        `${this.base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },
    setResults(results) {
      this.weather = results;
      console.log(results);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-image: url("./assets/cloudy.jpg");
  background-size: cover;
  background-position: bottom;
}

#app.warm {
  background-image: url('./assets/sunny.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  margin-bottom: 5px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;
  

  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  transition: 0.4s;
}

.search-bar:focus {
  box-shadow: rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.8);
}

.submit {
  appearance: none;
  border: none;
  outline: none;
  background: none;

  border-radius: 8px;
  color: #313131;
  background-color: rgba(255, 255, 255, 1);
  width: 50%;
  font-size: 20px;
}

.location {
  color: white;
  font-size: 32px;
  font-weight: 500px;
  text-align: center;
}

.date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
}

.temp {
  padding: 10px 25px;
  font-size: 102px;
  color: white;
  font-weight: 800;
  margin: 10px 0px;
}

.weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
}
</style>
