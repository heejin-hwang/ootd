<template>
  <div>
    <div id="weather">
      <input type="text" v-model="city" />
      <button type="submit" @click="getWeather()"></button>
      <br />
      <!-- <h2 v-text="city"></h2> -->
      <h2>{{ city }}</h2>
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
      <img src="../assets/img/sun-rise.svg" width="3%" alt />
      {{sunrise}}
      <img src="../assets/img/sun-set.svg" width="3%" alt />
      {{sunset}}
      <img src="../assets/img/wet.svg" width="3%" alt />
      {{humidity}}
      <img src="../assets/img/wind.svg" width="3%" alt />
      {{wind}}
    </div>
  </div>
</template>

<script>
import axios from "axios";

const frontUrl = "https://api.openweathermap.org/data/2.5/weather?appid=";
const apiKey = "36b444fed9e29cbe7135eb4355f1d355";

let self;
let pos;

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
        window.console.log("등록요청");
        navigator.geolocation.getCurrentPosition(showPosition); // async, callback 요청 
        window.console.log("요청 완료");
      } else {
        alert("Geolocation is not supported by this browser.");
      }

      function showPosition(position) {
        window.console.log("위치 결과 받아옴");
        pos = position

        self.getLocation()
      }
    },

    getLocation() {
      let position = pos
      window.console.log(position.coords.latitude);
      
      const url = `${frontUrl}${apiKey}&lat=${position.coords.latitude}&lon=${position.coords.longitude}`;
      window.console.log(url);

      

      axios
        .get(url) // 김씨 날씨 정보 알려주세요
        .then(response => { // 김씨가 '제가 날씨 정보 가지고 왔어요'
          //window.console.log(response.data.name)
          //window.console.log(response.data)   
      
          //window.console.log('1')
          //window.console.log(this)
          //window.console.log('2')

          this.city = response.data.name;

          //window.console.log(this)

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
    },

    getWeather() {
      let url = `${frontUrl}${apiKey}&q=${this.city}&units=metric`;

      window.console.log('날씨 알려주세요 요청')

      axios
        .get(url)
        .then(response => {
          window.console.log(this)

          window.console.log('오늘 맑아요')

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

      window.console.log('요청 완료')
    }
  },
  beforeMount() {
    //this.getGeolocation();
    
  },
  created() {
    window.console.log('생성')
    self = this;
    this.getGeolocation();
  },
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
