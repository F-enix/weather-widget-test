<template>
    <div class="weather-box">
        <p>{{ info_weather }}</p>
        <div @click="$emit('currentVal')" class="temp">{{ info_temp }} °C
            <div class="description">
                <label for="image">{{ info_description }}</label>
                <img id="image" :src="weather_icon"/>
            </div>
            <div class="tooltip">Go to current weather</div>
        </div>
        
        <div class="weather-info">
            <div class="container">
                <p>Feels like: <span>{{ info_feels_like }} °C</span></p>
                <p>Humidity: <span>{{info_humidity}}%</span></p>
                <p>Cloudiness: <span>{{info_clouds}}%</span></p>
            </div>
            <div class="container">
                <p>Wind Speed: <span>{{info_wind_speed}} m/s</span></p>
                <p>Wind Degree: <span>{{info_wind_deg}}°</span></p>
                <p>Pressure: <span>{{info_pressure}} hPa</span></p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'WeatherAnalytics',
    emits: ['currentVal'],
    props: [
        'info_weather',
        'info_temp',
        'info_description',
        'weather_icon',
        'info_feels_like',
        'info_humidity',
        'info_clouds',
        'info_wind_speed',
        'info_wind_deg',
        'info_pressure'
    ],
    
}
</script>

<style scoped>
.weather-box {
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: center;
  margin-bottom: 100px;
}
.weather-box .temp {
  position: relative;
  display: flex;
  flex: 1;
  justify-content: space-between;
  align-items: center;
  padding: 10px 25px;
  font-size: 80px;
  font-weight: 700;
  margin: 20px;
  min-width: 400px;
  max-width: 650px;
  
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 20px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .temp:hover {
    background-color: rgba(255,255,255,0.7);
    color: #28282B;
}
.weather-box .temp:active {
    box-shadow: 0 0 16px white;
}
.weather-box .temp .description {
  display: flex;
  flex-direction: column;
  font-size: 15px;
  max-width: 160px;
  min-width: 100px;
  text-shadow: 1px 2px rgba(0,0,0,0.25);
}
.weather-box .temp .tooltip {
    position: absolute;
    top: -25px;
    right: 200px;
    background-color: rgba(255,255,255,0.25);
    color: #bfe2db;
    border-radius: 5px;
    padding: 3px;
    font-size: 12px;
    text-shadow: none;
    pointer-events: none;
    white-space: nowrap;
    opacity: 0;
    transition: opacity 0.4s ;
}
.weather-box .temp:hover .tooltip {
    opacity: 0.8;
}
.weather-info {
  display: flex;
  flex-direction: column;
  text-align: left;

  min-width: 200px;
  padding: 0 15px;
}
p {
  margin: 2px;
}
span {
  color: rgb(35, 94, 255);
}
@media (max-width: 725px){
  .weather-box{
    flex-direction: column;
  }
  .weather-info{
    min-width: 400px;
    flex-direction: row;
    justify-content: space-between;
  }
}
@media (max-width: 399px) {
  .weather-box .temp {
    flex-direction: column;
    min-width: 250px;
  }
  .weather-info {
    min-width: 200px;
    flex-direction: column;
  }
}
</style>