<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp>16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text"
          class="search-bar"
          placeholder="Search for location..."
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>

      <div class="weather-warp" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>

      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '60be4a3e8ee0557e30a24f4a233dc0c1',
      url_base:'https://api.openweathermap.org/data/2.5',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == 'Enter') {
        fetch(`${this.url_base}/weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
      console.log(this.weather);
    },
    dateBuilder(){
      let d = new Date();

      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 
      'August', 'September', 'October', 'November', 'December'];

      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thrusday', 'Friday', 'Saturday'];

      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let date = d.getDate();
      let year = d.getFullYear();

      return `${date} ${month} ${year}, ${day} `; 
    }
  },
}
</script>

<style>
  #app{
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;

  }
  #app.warm{
    background-image: url('./assets/warm-bg.jpg');
  }

  main{
    height: 100vh;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.5), rgba(0,0,0,0.75));
  }

  .search-box{
    width: 80%;
    margin: 0 auto;
    text-align: center;
    padding: 30px 0px;
  }

  .search-box .search-bar{
    width: 100%;
    padding: 15px 20px;
    border-radius: 0 16px 0 16px;
    background-color: rgba(225, 225, 225, 0.5);
    border: none;
    font-size: 20px;
    color: white;

  }

  .search-box .search-bar::placeholder{
    color: #FFF;
    font-weight: 600;
  }

  .search-box .search-bar:focus{
    outline: none;
    border-radius: 16px 0 16px 0;
    background-color: rgba(225, 225, 225, 0.75);
  }

  .location-box{
    text-align: center;
  }

  .location-box .location{
    font-size: 30px;
    color: #fff;
    font-weight: 600;
  }

  .location-box .date{
    font-size: 20px;
    color: #FFF;
    font-family: cursive
  }

  .weather-box{
    text-align: center;
  }

  .weather-box .temp{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100px;
    height: 100px;
    margin: 15px auto;
    font-size: 45px;
    font-weight: 700;
    color:white;
    background-color:gray;
    border-radius: 15px;
  }

  .weather-box .weather{
    font-size: 30px;
    font-weight: 600;
    color: #FFF;
  }
</style>
