<template>
    <div class="container">
         <div id="weather">
        Overcast:  {{overcast}}
        <span class="temperature">{{currentTemp}}°</span><br>
        <span id="temp-values">Min {{minTemp}}° <br> Max {{maxTemp}}°</span>
      </div>
      <div id="info">
        Sunrise: {{sunrise}}
        Sunset: {{sunset}}
        Humidity: {{humidity}}
        Pressure:  {{pressure}}
        Wind: {{wind}}
      </div>
    </div>
</template>

<script>
    export default {
        el: '#app',
        data: function() {
         return  {
           currentTemp: '',
            minTemp: '',
            maxTemp:'',
            sunrise: '',
            sunset: '',
            pressure: '',
            humidity: '',
            wind: '',
            overcast: '',
            icon: ''
         }

        },
        created: function() {
            //let url = "http://api.openweathermap.org/data/2.5/weather?q=Melbourne&units=metric&APPID=a48c6560a2e66a254e3d6fb961dc1e4c";
            axios.get('https://cors-anywhere.herokuapp.com/http://api.openweathermap.org/data/2.5/weather', { params: {
                    q: 'Melbourne,AU',
                    appid: 'bad3e42bbdd0c77587f16d3c9b4145e3',
                    units: 'metric'
                }}).
                then(response => {
                this.currentTemp = response.data.main.temp;
                this.minTemp = response.data.main.temp_min;
                this.maxTemp = response.data.main.temp_max;
                this.pressure = response.data.main.pressure;
                this.humidity = response.data.main.humidity + '%';
                this.wind = response.data.wind.speed + 'm/s';
                this.overcast = response.data.weather[0].description;
                this.icon = "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
                this.sunrise = new Date(response.data.sys.sunrise*1000).toLocaleTimeString("en-GB").slice(0,4);
                this.sunset = new Date(response.data.sys.sunset*1000).toLocaleTimeString("en-GB").slice(0,4);
            })
            .catch(error => {
                console.log(error);
            });
            },
        }

</script>
