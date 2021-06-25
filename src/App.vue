<template>
  <div id="app" :class="typeof weather.main != 'undefined' && Math.round(weather.main.temp) <= 15 ? new Date().getHours() >= 20 ? 'dark-cold': 'cold' : new Date().getHours() >= 20 ? 'dark-warm' : ''">
    <main class="content">
      <div class="search-box">
        <input type="search" class="search" placeholder="Search Location..." ref="search" v-model="location" @keypress.enter="FindWeather">
      </div>
      <div class="info" v-if="typeof weather.main != 'undefined'">
        <div class="location-date">
          <h1 class="location">{{ weather.name }}, {{ weather.sys.country }}</h1>
          <p class="date">{{ Today }}</p>
        </div>

        <div class="temp">
          <span>{{ Math.round(weather.main.temp) }}°</span>
        </div>

        <div class="weather">
          <img :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`" :alt="weather.weather[0].main">
          <p>{{ weather.weather[0].main }}</p>
        </div>
      </div>
      <div class="warning" v-else>
        <h1>Search Location</h1>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key: '8678eb8247c7f2cf6fe0e7d2bc5d66a6',
      location: '',
      weather: {}
    }
  },
  methods: {
    FindWeather(){
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&units=metric&appid=${this.api_key}`)
        .then(result  => result.json()).then(res => this.weather = res)
    }
  },
  computed: {
    Today(){
      let date = new Date()
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

      let day = days[date.getDay()]
      let month = months[date.getMonth()]
      let year = date.getFullYear()

      return `${day}, ${month} ${date.getDate()}, ${year}`
    }
  },
  mounted() {
    this.$refs.search.focus()
  }
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  transition: 0.5s ease;
}

#app {
  text-align: center;
  background: url('assets/warm-bg.jpeg') no-repeat bottom fixed;
  background-size: cover;
  height: 100vh;
  color: #FFFFFF;
  text-shadow: 3px 4px 20px rgba(0, 0, 0, 0.25);
}

#app.cold{
  background: url('assets/cold-bg.jpg') no-repeat bottom fixed;
  background-size: cover;
}

#app.dark-cold{
  background: url('assets/dark-cold-bg.jpg') no-repeat bottom fixed;
  background-size: cover
}

#app.dark-warm{
  background: url('assets/dark-warm-bg.jpg') no-repeat bottom fixed;
  background-size: cover
}

.content{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.4));
  z-index: 111;
}

.weather-status{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 0;
}

.search-box{
  padding: 1.2rem;
}

.search-box .search{
  width: 100%;
  outline: none;
  border: none;
  padding: .5rem;
  height: 40px;
  background: #FFFFFF;
  box-shadow: 3px 4px 20px rgba(0, 0, 0, 0.25);
  border-radius: 0 10px;
}

.search::placeholder{
  line-height: 25px;
  color: #7E7E7E;
}

.info, .warning{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  width: max-content;
}

.temp{
  margin: 3rem auto;
}

.location-date .location{
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.location-date .date{
  font-size: 1.2rem;
}

.temp span{
  font-size: 4rem;
  position: relative;
}

.temp span::before{
  content: 'C';
  position: absolute;
  right: -25px;
  bottom: 5px;
  font-size: 2.5rem;
}

.weather p{
  font-size: 3rem;
  margin-bottom: 1rem;
}

</style>
