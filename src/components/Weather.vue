<template>
        
        <h2>Weather App</h2>

    
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
            const url = "https://fcc-weather-api.glitch.me/api/current";
            /*const params = new URLSearchParams();
            params.append('lat', -38);
            params.append('lon', -57);*/
            axios
                .get(url, {
                    params: {
                        lat: -38,
                        lon: -57
                    }
                }
                )
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
*{
    margin: 0;
    padding: 0;
}
body{
    background: -webkit-linear-gradient(to bottom, #49e3ee , #82a2df); 
    background: linear-gradient(to bottom, #49e3ee , #5085da, #5793f3); 
    background-repeat: no-repeat;
    height: 100vh;
    width: 100vw;
}
.small-container {
	max-width: 420px;
    text-align: center;
}
.card {
    padding: 30px;
    background: #fff;
    border-radius: 20px;
    margin: 10px;
}
.shadow1 { 
    box-shadow: 0 5px 10px rgba(154,160,185,.05), 0 15px 40px rgba(166,173,201,.2);
}

</style>