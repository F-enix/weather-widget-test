<template>
  <div id="app" :style="backgroundPos()">
    <main>

      <div class="location-box">
        <div class="location">Thessaloniki, Greece</div>
        <div class="date">{{ active_day }}</div>
      </div>
      
      <div v-if="forecast?.current" class="weather-wrap">
        <WeatherAnalytics 
        @currentVal="currentValues"
        :info_weather="info_weather"
        :info_temp="info_temp"
        :info_description="info_description"
        :weather_icon="weather_icon"
        :info_feels_like="info_feels_like"
        :info_humidity="info_humidity"
        :info_clouds="info_clouds"
        :info_wind_speed="info_wind_speed"
        :info_wind_deg="info_wind_deg"
        :info_pressure="info_pressure"
        />
      </div>

      <div class="day-selector">
        <Weekmenu @toggle="toggleDay" :forecast="forecast" :dateBuilder="dateBuilder" />        
      </div>

      <div class="chart">
        <LineChart :forecast="forecast" :dateBuilder="dateBuilder" />
      </div>
    </main>
  </div>
</template>

<script>
import Weekmenu from './components/WeekMenu.vue'
import WeatherAnalytics from './components/WeatherAnalytics.vue'
import LineChart from './components/LineChart.vue'

export default {
  name: 'App',
  components: { Weekmenu, WeatherAnalytics, LineChart },
  emits: ['toggle', 'currentVal'],
  props: [],
  data() {
    return {
      api_key: 'db895bbaa3e1431a45e8b88e92854504',
      url_base: 'https://api.openweathermap.org/data/2.5/onecall?lat=40.5873&lon=22.9482&exclude=minutely,hourly,alerts&units=metric',
      forecast: '',
      weather_icon: '',
      info_pressure: '',
      info_wind_deg: '',
      info_wind_speed: '',
      info_clouds: '',
      info_humidity: '',
      info_feels_like: '',
      info_temp: '',
      info_description: '',
      info_weather: 'Current Weather',
      active_day: '',
      pos: 'center',
    }
  },
  methods: {
    fetchWeather() {
      fetch(`${this.url_base}&appid=${this.api_key}`)
        .then(response => {
          return response.json();
        })
        .then(this.setResults);
    },
    setResults(results) {
      this.forecast = results;
      this.active_day = this.dateBuilder(0);
      this.info_weather = "Current Weather";
      this.info_temp = Math.round(results.current.temp);
      this.info_description = results.current.weather[0].description;
      this.info_feels_like = Math.round(results.current.feels_like);
      this.info_humidity = results.current.humidity;
      this.info_clouds = results.current.clouds;
      this.info_wind_speed = results.current.wind_speed;
      this.info_wind_deg = results.current.wind_deg;
      this.info_pressure = results.current.pressure;
      this.weather_icon = `http://openweathermap.org/img/wn/${results.current.weather[0].icon}@2x.png`;
    },
    currentValues() {
      this.fetchWeather();
    },
    dateBuilder(extra, flag) {
      let d = new Date();
      let t = new Date(d);
      t.setDate(t.getDate() + extra);
      let months = ["January", "February", "March", "April", "May", "June", "July", "August",
      "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      if(extra > 0){
        day = days[t.getDay()];
        date = t.getDate();
        month = months[t.getMonth()];
        year = t.getFullYear();
      }
      if (flag) return day;
      return `${day} ${date} ${month} ${year}`;
    },
    backgroundPos(){
      if (this.info_temp < 15) return "background-position: right";
      else if (this.info_temp < 25) return "background-position: center";
      else return "background-position: left";
    },
    toggleDay(day) {
      this.info_weather = this.dateBuilder(day,true) + '\'s Forecast';
      this.info_temp = Math.round((this.forecast.daily[day].temp.day + this.forecast.daily[day].temp.night) / 2);
      this.info_description = this.forecast.daily[day].weather[0].description;
      this.info_feels_like = Math.round((this.forecast.daily[day].feels_like.day + this.forecast.daily[day].feels_like.night) / 2);
      this.info_humidity = this.forecast.daily[day].humidity;
      this.info_clouds = this.forecast.daily[day].clouds;
      this.info_wind_speed = this.forecast.daily[day].wind_speed;
      this.info_wind_deg = this.forecast.daily[day].wind_deg;
      this.info_pressure = this.forecast.daily[day].pressure;
      this.active_day = this.dateBuilder(day);
      this.weather_icon = `http://openweathermap.org/img/wn/${this.forecast.daily[day].weather[0].icon}@2x.png`;
    },
  },
  created() {
    this.fetchWeather()
  },
  mounted() {
    //this.fetchWeather()
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,400;0,500;0,700;1,300&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Roboto', sans-serif;
  text-align: center;
  //color: #2c3e50;
  color: #bfe2db;
}
#app {  
  background-image: url('./assets/back-img.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 30px;
  background-image: linear-gradient(to bottom, rgba(0,0,0, 0.3), rgba(0,0,0,8));
}
.day-selector {
  width: 100%;
  margin-bottom: 30px;
  display: flex;
  justify-content: center;
}
.location-box {
  margin: 40px 0;
}
.location-box .location {
  font-size: 32px;
  font-weight: 500;
  min-width: 200px;
  text-shadow: 1px 3px rgba(0,0,0,0.3);
}
.location-box .date{
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  min-width: 200px;
}
.chart {
  display: flex;
  width: 100%;
  justify-content: center;
}

</style>
