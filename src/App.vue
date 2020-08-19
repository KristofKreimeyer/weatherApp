<template>
  <div id="app" :class="typeof weather.request !== 'undefined' && weather.current.temperature > 16 ? 'warm' : 'cold'">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrapper" v-if="typeof weather.request !== 'undefined'">
        {{weather.main}}
        <div class="location-box">
          <div class="location">{{ weather.location.name }}, {{weather.location.country}}</div>
          <div class="date">{{ weather.location.localtime }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ weather.current.temperature }}</div>
          <div class="weather">{{ weather.current.weather_descriptions }}</div>
        </div>
      </div>
      <div class="weather-undefined" v-if="typeof weather.request == 'undefined'">
        <p>Please enter City</p>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: "9fa733415776dd6573d1f8414b5cb883",
      url_base: 'http://api.weatherstack.com/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}current?access_key=${this.api_key}&query=${this.query}`).then(res => {
          return res.json();
        }).then(this.setResults);
      }
      this.replaceString()
    },
    setResults (results) {
      this.weather = results;
    },
    replaceString () {
      if(document.getElementsByClassName("weather").length === 1) {
        var str = document.getElementsByClassName("weather").textContent;
        console.log(str)
        str.replace("[", "");
      }
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

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }
  #app.cold {
    background-image: url("./assets/cold-bg.jpg");
  }
  #app.warm {
    background-image: url("./assets/warm-bg.jpg");
  }
  main {
    min-height: 100vh;
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

    color: #313131;
    font-size: 1.25rem;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0 0 8px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.5);
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    background-color: rgba(255,255,255,0.75);
    box-shadow: 0 0 16px rgba(0,0,0,0.25);
    border-radius: 16px 0 16px 0;
  }

  .location-box .location {
    color: #fff;
    font-size: 2rem;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0, 0.25);
  }

  .location-box .date {
    color: #fff;
    font-size: 1rem;
    font-weight: 200;
    font-style: italic;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temperature {
    display: inline-block;
    padding: 10px 25px;
    color: #ffffff;
    font-size: 7rem;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.25);
    border-radius: 1rem;
    margin: 30px 0;
    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  .weather-box .weather {
    color: #ffffff;
    font-size: 2.75rem;
    font-style: italic;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0,0,0,0.25);

  }

  .weather-undefined p {
    color: #fff;
    font-size: 2rem;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0, 0.25);
  }
</style>
