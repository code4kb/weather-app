<template>
    <div>
        <h1>Weather</h1>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Weather',
    data() {
        return {
            weatherData:null,
            cargando: true,
            huboerror: false
        }
    },
    mounted(){
        this.getAll();
    },
    methods: {
        getAll(){
            this.cargando = true //the loading begin
            axios
                .get('weather.json')   
                .then(response => {
                    this.weatherData = response.data
                })
                .catch(error => {
                    console.log(error)
                    this.huboerror = true
                })
                .finally(() => this.cargando = false)
                }
    }    
}
</script>

<style>

</style>