<template>
    <section v-if="huboerror">
        <div>    
            <p>Sorry, it is not possible to get the information at this time, please try again later.</p>
        </div>        
    </section>

    <section v-else>

        <div class="cardContainer">
            <div class="card">
                <h1>WEATHER APP 📱</h1>
                <div class="w-stats">
                    <div>
                        <h3>{{city}}, <span class="location">{{country}}</span></h3> 
                    </div>
                </div>
                <div class="w-icon">
                    <img class="w-image" :src="`${icon}`">
                </div>
                <div class="w-temp">
                    <h1>{{temp}}°</h1>
                </div>        
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Weather',
    data() {
        return {
            weatherData:null,
            city:null,
            country:null,
            temp:null,
            icon:null,
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
            axios
                .get(url, {
                    params: {
                        lat: -34.74,
                        lon: -58.39
                    }
                }
                )
                .then(response => {
                    this.weatherData = response.data
                    this.city = this.weatherData.name
                    this.country = this.weatherData.sys.country
                    this.temp = this.weatherData.main.temp
                    this.icon = this.weatherData.weather[0].icon
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
* {
    box-sizing: border-box;
}
body{
    margin: 0;
    padding: 0;
    background: -webkit-linear-gradient(to bottom, #36d1dc , #5b86e5); 
    background: linear-gradient(to bottom, #36d1dc , #5b86e5, #5b86e5); 
    background-repeat: no-repeat;
    height: 100vh;
    width: 100vw;
}
.card {
    position: absolute;    
    top: 20%;
    left: 35%;    
    text-align: center;
    width: calc(25% - 20px);
    padding: 10px;
    border-radius: 10px;
    margin: 10px;
    background-color:#ffffff;
}
.card p {
    font-size: 18px;
}
.card h1 {
    font-weight: 600;
    font-size: 32px;
    color:#5b86e5;
}
.card h2 {
    font-size: 18px;
}
.card h3 {
    font-size: 22px;
}
.location {
    font-size: 15px;
}
.cardContainer:after {
    content: "";
    display: table;
    clear: both;
}
.w-img{
    width:120px;
    max-width:120px;
    height: 120px;
    max-height: 120px;
}
@media screen and (max-width: 380px) {
    .card {
        width: 100%;
    }
}
</style>