<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp >= 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ getDate() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "22663df1bbcabf7d8953a29c76e79122",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((response) => {
            return response.json();
          })
          .then(this.setResults);
      }
    },
    setResults(data) {
      this.weather = data;
    },
    getDate() {
      let date = new Date();
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
      let day = days[date.getDay()];
      let d = date.getDate();
      let month = months[date.getMonth()];
      let year = date.getFullYear();
      return `${day} ${d} ${month} ${year}`;
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
  font-family: "Nunito", sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.45),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  color: black;
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  transition: 0.4s;
}
::placeholder {
  color: rgba(0, 0, 0, 0.75);
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.7);
}

.location-box .location {
  color: white;
  font-size: 2rem;
  font-weight: 600;
  text-align: center;
  text-shadow: 0px 0px 6px rgba(255, 255, 255, 0.4);
}

.location-box .date {
  color: white;
  font-size: 1.725rem;
  font-weight: 600;
  font-style: italic;
  text-align: center;
  text-shadow: 0px 0px 6px rgba(255, 255, 255, 0.3);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: rgba(255, 255, 255, 0.95);
  font-size: 100px;
  font-weight: 600;
  text-shadow: 3px 3px 20px #8f828293, -2px 1px 30px #dbdbe893;
  margin: 2rem 0;
  border-radius: 50%;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.weather-box .weather {
  color: white;
  font-size: 50px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 0px 0px 6px rgba(255, 255, 255, 0.3);
}
</style>
