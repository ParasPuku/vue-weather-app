<template>
  <div class="top-container">
    <!-- <header>
      <h1>Vue Weather Report App</h1>
    </header> -->
    <div class="main-container">
      <div
        :class="
          typeof weather.main !== 'undefined' && setTempBackground > 16
            ? 'search-box cloud'
            : 'search-box mist'
        "
      >
        <input
          type="text"
          class="search-bar"
          placeholder="Search your cities..."
          v-model="query"
          @keypress="fetchWeather"
        />

        <div class="error-code" v-if="weather.cod === '404'">
          <p class="error-msg">{{weather.message}}</p>
          <p class="try-again">Try again...</p>
        </div>

        <div
          class="weather-container"
          v-if="typeof weather?.main != 'undefined'"
        >
          <div class="weather-wrap">
            <div class="location-wrap">
              <p class="location">
                {{ weather?.name }}, {{ weather?.sys?.country }}
              </p>
              <p class="date-and-time">
                {{ this.dateBuilder() }}
              </p>
            </div>
          </div>
          <div class="weather-degree">
            <div class="celceius">
              <p class="celceius-report">
                {{ Math.round(weather?.main?.temp) }}°c
              </p>
            </div>
          </div>
          <div class="weather-forcast">
            <div
              class="forcast"
              v-for="item in weather.weather"
              v-bind:key="item.id"
            >
              <p class="forcast-report">
                {{ item.main }}
              </p>
              <!-- <span>{{ item.description }}</span> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "weather",
  data() {
    return {
      api_key: "0225d04d78bb3fb8563858a3cc45bc27",
      api_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      setTempBackground: 0,
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        try {
          fetch(
            `${this.api_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
          )
            .then((res) => {
              return res.json();
            })
            .then(this.setResults);
        } catch (e) {
          console.log("error", e);
        }
      }
    },
    setResults(data) {
      this.weather = data;
      console.log("Weather", this.weather);
      console.log("Weather name", this.weather.name);
      this.setTempBackground = data?.main?.temp;
      this.query = ""
    },
    getWeatherForcast() {
      const report = weather?.weather?.map((data) => {
        return data.main;
      });
      return report;
    },
    dateBuilder() {
      const d = new Date();
      const months = [
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
      const weeks = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      const day = weeks[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
@import "./assets/base.css";

.search-box {
  /* background: url(/src/assets/cold-bg.jpg); */
  width: 360px;
  height: 609px;
  display: block;
  background-repeat: no-repeat;
  background-size: cover;
}

.search-box.cloud {
  background: url(/src/assets/cold-bg.jpg);
}

.search-box.mist {
  background: url(/src/assets/warm-bg.jpg);
}

.main-container {
  max-width: 1280px;
  margin: 0 auto;
  font-weight: normal;
  display: flex;
  justify-content: center;
  align-items: center;
}

input.search-bar {
  width: 300px;
  padding: 8px 15px;
  border: 0px solid #ff4af4;
  margin-top: 20px;
  margin-left: 30px;
  outline: 0;
  font-size: 16px;
  letter-spacing: 2px;
  box-shadow: 0px 0px 3px 2px #ffdbfd;
  border-radius: 0px 10px 2px 10px;
  color: #858585;
}
p.location {
  color: #ffffff;
  font-size: 32px;
  margin-top: 20px;
  text-align: center;
  text-shadow: 0px 2px rgb(0 0 0 / 25%);
  letter-spacing: 2px;
  font-weight: bold;
}
p.date-and-time {
  text-align: center;
  color: #a8a8a8;
}
.celceius {
  background: #bcbcbc;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 150px;
  height: 80px;
}
.weather-degree {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
}
p.celceius-report {
  font-size: 60px;
  font-weight: bold;
  color: #ffffff;
}
.weather-forcast {
  display: flex;
  align-items: center;
  justify-content: center;
}
.forcast-report {
  font-size: 50px;
  color: #ffffff;
  font-weight: bold;
  font-style: italic;
}
p.error-msg {
    font-size: 25px;
    color: #ffff;
    text-align: center;
    margin: 50px 0 5px;
    letter-spacing: 2px;
    text-transform: capitalize;
}
p.try-again {
    color: #ffff;
    text-align: center;
    letter-spacing: 1px;
}
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    background-color: #a6a6a6;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
