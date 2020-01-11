<template>
  <div>
    <div id="weather">
      <input type="text" v-model="city">
      <button type="submit" @click="getWeather()"></button>
      <br>
      <h2 v-text="city"></h2>
      <h3 class="temperature">{{currentTemp}}°</h3>
      <br />
      <h4 id="temp-values">
        Min {{minTemp}}°
        <br />
        Max {{maxTemp}}°
        <p v-if="maxTemp - minTemp > 10">일교차가 커요. 감기에 유의하세요.</p>
      </h4>
    </div>
    <div id="info">
      <img src="../assets/img/sun-rise.svg" width=3% alt="">{{sunrise}}
      <img src="../assets/img/sun-set.svg" width=3% alt="">{{sunset}}
      <img src="../assets/img/wet.svg" width=3% alt="">{{humidity}}
      <img src="../assets/img/wind.svg" width=3% alt="">{{wind}}
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      currentTemp: "",
      minTemp: "",
      maxTemp: "",
      sunrise: "",
      sunset: "",
      pressure: "",
      humidity: "",
      wind: "",
      overcast: "",
      icon: ""
    };
  },
  methods: {
    getGeolocation() {
    
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(showPosition);
          window.console.log('dd');
          
        } else {
          alert("Geolocation is not supported by this browser.");
        }
      
      function showPosition(position) {
        window.console.log(position.coords.latitude)
        const url =
        `https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=36b444fed9e29cbe7135eb4355f1d355`;
        // x.innerHTML = "Latitude: " + position.coords.latitude +
        // "<br>Longitude: " + position.coords.longitude;
        axios
        .get(url)
        .then(response => {
          this.city = response.data.main.name;
          this.currentTemp = response.data.main.temp;
          this.minTemp = response.data.main.temp_min;
          this.maxTemp = response.data.main.temp_max;
          this.humidity = response.data.main.humidity + "%";
          this.wind = response.data.wind.speed + "m/s";
          this.overcast = response.data.weather[0].description;
          this.icon =
            "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
          this.sunrise = new Date(response.data.sys.sunrise * 1000)
            .toLocaleTimeString("en-GB")
            .slice(0, 4);
          this.sunset = new Date(response.data.sys.sunset * 1000)
            .toLocaleTimeString("en-GB")
            .slice(0, 4);
        })
        .catch(error => {
          window.console.log(error);
        });
      }
      
    },


    getWeather() {
      let url =
        `http://api.openweathermap.org/data/2.5/weather?q=${
            this.city
          }&units=metric&APPID=36b444fed9e29cbe7135eb4355f1d355`;
          
      axios
        .get(url)
        .then(response => {
          this.city = response.data.main.name;
          this.currentTemp = response.data.main.temp;
          this.minTemp = response.data.main.temp_min;
          this.maxTemp = response.data.main.temp_max;
          this.humidity = response.data.main.humidity + "%";
          this.wind = response.data.wind.speed + "m/s";
          this.overcast = response.data.weather[0].description;
          this.icon =
            "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
          this.sunrise = new Date(response.data.sys.sunrise * 1000)
            .toLocaleTimeString("en-GB")
            .slice(0, 4);
          this.sunset = new Date(response.data.sys.sunset * 1000)
            .toLocaleTimeString("en-GB")
            .slice(0, 4);
        })
        .catch(error => {
          window.console.log(error);
        });
    }
  },
  beforeMount() {
    this.getGeolocation();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
