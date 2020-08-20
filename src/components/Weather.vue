<template>
    <section v-if="isError">
        <div>    
            <p>Sorry, it is not possible to get the information at this time, please try again later.</p>
        </div>        
    </section>

    <section v-else>

        <div class="cardContainer">
            <div class="card">
                <h1>WEATHER APP 📱</h1>

                <div v-if="errorLocation">
                    Sorry, but the following error
                    occurred: {{errorLocation}}
                </div>                
                <div v-if="gettingLocation">
                    <i>Getting your location...</i>
                </div>

                <div v-if="loading" class="spinner">
                    Loading...
                </div>                    

                <template v-if="loading === false">
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
                </template>
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
            loading: true,
            isError: false,
            gettingLocation: false,
            errorLocation: null
        }
    },
    created() {
    },
    mounted(){
    },
    methods: {
        getApiWeather(latitude, longitude){
            this.loading = true //the loading begin
            this.gettingLocation = true;
            const url = "https://fcc-weather-api.glitch.me/api/current";
            axios
                .get(url, {
                    params: {
                        lat: latitude,
                        lon: longitude
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
                    this.isError = true
                })
                .finally(() => 
                    this.loading = false,
                    this.gettingLocation = false
                )
        },
        initGeolocation(){
            // we determine if it supports geolocation
            if(!navigator.geolocation){
                this.errorLocation = "<strong>Geolocation is not supported by your browser</strong>";
                return;
            }

            navigator.geolocation.getCurrentPosition(this.createUrl, this.errorGeoLocation);
            this.gettingLocation = false;
        },
        createUrl(position){
            const lat = position.coords.latitude;
            const lon = position.coords.longitude;
            this.gettingLocation = true;

            this.getApiWeather(lat, lon);
        },
        errorGeoLocation(){
            this.errorLocation = "Unable to retrieve your location";
        }
    },
    beforeMount(){
        this.initGeolocation();
    },
}
</script>

<style>
* {
    box-sizing: border-box;
}
body{
    margin: 0;
    padding: 0;
    background: rgb(49,95,198);
    background: radial-gradient(circle, rgba(49,95,198,1) 0%, rgba(81,226,236,1) 63%);
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
.w-image{
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
.spinner {
    position: absolute;
    left: 50%;
    top: 50%;
    height:100px;
    width:100px;
    margin:0px auto;
    -webkit-animation: rotation .6s infinite linear;
    -moz-animation: rotation .6s infinite linear;
    -o-animation: rotation .6s infinite linear;
    animation: rotation .6s infinite linear;
    border-left:6px solid rgba(0,174,239,.15);
    border-right:6px solid rgba(0,174,239,.15);
    border-bottom:6px solid rgba(0,174,239,.15);
    border-top:6px solid rgba(0,174,239,.8);
    border-radius:100%;
}
@-webkit-keyframes rotation {
    from {-webkit-transform: rotate(0deg);}
    to {-webkit-transform: rotate(359deg);}
}
@keyframes rotation {
    from {-moz-transform: rotate(0deg);}
    to {-moz-transform: rotate(359deg);}
}
@-o-keyframes rotation {
    from {-o-transform: rotate(0deg);}
    to {-o-transform: rotate(359deg);}
}
</style>