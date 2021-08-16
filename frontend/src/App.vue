<template>
  <div id="main" v-bind:class="isDay ? 'day':'night'">
    <div class="container my-5">
      <h1 class="title text-center">Weather in:</h1>
      <form action="" class="search-location" v-on:submit.prevent="getWeather">
        <input 
          type="text" 
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="What City?"
          autocomplete="off"
          v-model="citySearch"
        >
      </form>
      <div class="card rounded my-3 shadow-lg back-card overflow-hidden">
        <div icon='sunny'>
          <span class="sun"></span>
        </div>
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p> {{ weather.cityName }}</p>
            <span>...</span>
            <p class=""> {{ weather.country }} </p>
          </div>
        </div>
        <div class="card-body">
          <div class="card-mid">
            <!-- Row #1 -->
            <div class="row">
              <div class="col-12 text-center temp">
                <span> {{ weather.temperature }} &deg;C</span>
                <p class="my-4"> {{ weather.description }} </p>
              </div>
            </div>
            <!-- Row #2 -->
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="" alt="">
                   {{ weather.lowTemp }} &deg;C
                </p>
                <p>
                  <img src="" alt="">
                   {{ weather.highTemp }} &deg;C
                </p>
              </div>
            </div>
            <div class="row card-bottom px-5 py-4">
              <!-- Col -->
              <div class="col text-center">
                <p> {{ weather.feelsLike }} &deg;C</p>
                <span>Feels Like</span>
              </div>
              <!-- Col -->
              <div class="col text-center">
                <p> {{ weather.humidity }} %</p>
                <span>Humidity</span>
              </div>
            </div>
          </div>
        </div>      
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      isDay: true,
      citySearch: '',
      weather: {
        cityName: 'Gwarinpa',
        country: 'NG',
        temperature: 12,
        description: 'Clouds Everywhere',
        lowTemp: '19',
        highTemp: '30',
        feelsLike: '20',
        humidity: '55', 
      }
    }
  },
  methods: {
    getWeather: async function() {
      const key = "5299e83c9cf2fef510fb8ddaa3206ac8";
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`
      const response = await fetch(baseURL)
      const data = await response.json()
      console.log(data);
      this.citySearch ='';
      this.weather.cityName = data.name
      this.weather.country = data.sys.country
      this.weather.temperature = Math.round(data.main.temp)
      this.weather.lowTemp = Math.round(data.main.temp_min)
      this.weather.highTemp = Math.round(data.main.temp_max)
      this.weather.feelsLike = Math.round(data.main.feels_like)
      this.weather.humidity = Math.round(data.main.humidity)
      this.weather.description = data.weather[0].description

      const timeOfDay = data.weather[0].icon
      if(timeOfDay.includes('n')) {
        this.isDay = false;
      } else {
        this.isDay = true;
      }

    }
  }
}
</script>

<style>
 @import './assets/custom.css';
 @import './assets/animation.css';
</style>
