<template>
    <nav>
        <WeekDay 
            v-for="(days, i) in forecast.daily" :key="i" :day="i" 
            @click="activated(i)"
            :forecast="forecast" 
            :dateBuilder="dateBuilder"
            :class="[arr[i] ? 'working' : '']"
        />
    </nav>
</template>

<script>
import WeekDay from './WeekDay.vue'

export default {
    name: 'WeekMenu',
    data() {
        return {
            day: 0,
            arr: [],
        }
    },
    components: {WeekDay},
    props: ['forecast', 'dateBuilder'],
    methods: {
       activated(i) {
        this.$emit('toggle', i)
        for (let j = 0; j < 8; j++){
            if (j == i) this.arr[j] = true;
            else this.arr[j] = false;
        }
       },
    },
    emits: ['toggle']    
}
</script>

<style scoped>
nav {
    display: flex;
    flex-direction: row;
}
.working {
    box-shadow: 0 0 16px white;
    background-color: rgba(255,255,255,0.7);
    color: #28282B;
}
@media (max-width: 950px) {
    nav {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
    }
}
@media (max-width: 470px) {
    nav {
        grid-template-columns: 1fr 1fr;
    }
}
</style>