<template>
  <div :class="weather.main !== undefined && weather.main.temp > 16 ? 'warm' : 'app'">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..." 
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="weather.main !== undefined" >
        <div class="location-box">
          <div class="location">
            {{weather.name}}, {{weather.sys.country}}
          </div>
          <div class="date"> {{dateBuilder()}} </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather"> {{weather.weather[0].main}} </div>
        </div>

        <div class="weather-detail">
          <div class="sunrise">Sun Rise:   <span>{{timeBuilder(weather.sys.sunrise)}}</span></div>
          <div class="sunset">Sun Set:    <span>  {{timeBuilder(weather.sys.sunset)}}</span> </div>
          <div class="humidity">Humidity:    <span>  {{weather.main.humidity}} %</span> </div>
          <div class="desc">Description:    <span>  {{weather.weather[0].description}}</span> </div>
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
      api_key: '',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather:{}
    }
  },
  methods:{
    fetchWeather (ev){
      if (ev.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res =>{
            return res.json()
          }).then(this.setResults);
      }
    },

    setResults(result){
      this.weather = result
    },

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    timeBuilder(unix_timestamp){
      var date = new Date(unix_timestamp * 1000);      
      var hours = date.getHours();
      var minutes = "0" + date.getMinutes();
      var seconds = "0" + date.getSeconds();
      var formattedTime = hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2);
      return formattedTime
    }
  },
  components: {
    
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'Montserrat', sans-serif;
}
.app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.warm {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  margin-bottom: 30px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;  
  
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 800;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color:  #fff;
  font-size: 20px;
  font-style: italic;
  font-weight: 100;
  text-align: center;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color:#FFF;
  font-size: 48px;
  font-weight: bolder;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-detail{
  margin-top: 15px;
  color: #FFF;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  padding: 20px;
  font-size: 20px;
}

.weather-detail span{
  float: right;
}
</style>
