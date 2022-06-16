<template>
    <button class='item-container'>
        <p>{{dateBuilder(day,true)}}</p>
        <img :src="icon" />
        <p>{{dayTemperature}} °C</p>
    </button>
</template>

<script>
export default {
    name: 'WeekDay',
    props: ['forecast', 'dateBuilder', 'day'],
    data() {
       return {
        icon: `http://openweathermap.org/img/wn/${this.forecast.daily[this.day].weather[0].icon}@2x.png`,
       }
    },
    computed: {
        dayTemperature() {
            return Math.round((this.forecast.daily[this.day].temp.day + this.forecast.daily[this.day].temp.night) / 2);
        },
    },
}
</script>

<style scoped>
.item-container {
    background-color: rgba(255,255,255,0.25);
    border: none;
    font-family: 'Roboto', sans-serif;
    font-size: 16px;
    color: #bfe2db;
    border-radius: 20px;
    margin: 5px;
    padding: 5px
}
.item-container:hover {
    background-color: rgba(255,255,255,0.7);
    color: #28282B;
}

</style>