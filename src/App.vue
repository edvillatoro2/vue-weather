<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 32 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" 
        class="search-bar" 
        placeholder="Enter City, State, or Zipcode"
        v-model="query"
        @keypress="fetchWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature"> {{ Math.round(weather.main.temp) }}°f</div>
            <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      api_key: 'b850d1b9e0cc5084230335bf2381580f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&appid=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January","February","March","April","May","June","July",
      "August","September","October","November","December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url('./assets/cold.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm.jpg');
}

main {
  height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #315b5c;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-image: linear-gradient(to top, #dfe9f3 0%, white 100%);
  border-radius: 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(0,0,0,0.95);
  background-image: linear-gradient(to top, #dfe9f3 100%, white 0%);
  border-radius: 16px 0px;
}

.location-box .location {
  color: azure;
  font-size: 32px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date {
  color: azure;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: oblique;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 20px;
  color: azure;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  border-radius: 15px;
}

.weather-box .weather {
  color: azure;
  font-size: 50px;
  font-weight: 700;
  font-style: oblique;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
