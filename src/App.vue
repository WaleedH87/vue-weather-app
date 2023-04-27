<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ?
  'warm' : ''">
    <main>
      <div class="logo-box">
        <img alt="logo" src="https://em-content.zobj.net/thumbs/240/apple/354/sun-behind-small-cloud_1f324-fe0f.png"/>
        <h1 class='logo-text'>Vue Weather App</h1>
        <div class="slogan"><p>Created by Waleed H.</p></div>
        </div>
      
       
      
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder='Enter your location...' 
        v-model='query'
        @keypress="fetchWeather"
        />
        
      </div>
      
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}{{ units }}</div>
          <div class="weather-status">{{ weather.weather[0].main }}</div>
          <div class="feels-like">Feels like {{ Math.round(weather.main.feels_like) }}{{ units }}</div>
        </div>
      </div>

    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data (){
    return {
      // Change API key below
      api_key:'c4c414299c57a00e8599362a05913df4',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      units: '°C'
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January","February","March","April","May","June","July",
      "August","September","October","November","December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];      
      

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

@font-face {
  font-family: 'mons';
  src: url(./fonts/Montserrat-VariableFont_wght.ttf);
}

body {
  font-family: 'mons', sans-serif;

}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
min-height: 100vh;
padding: 25px;

background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}



.logo-box {
  width: 50%;
  margin: auto;
  padding: 10px 25px;
  display: flex;
  flex-direction: column;
  justify-content: center; /* center horizontally */
  align-items: center; /* center vertically */
  color: white;
  block-size: auto;
  
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin-top: 30px;
  margin-bottom: 30px;
  box-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}
  
.logo-box img {
  display: inline-block;
  width: 20%;
  height: auto;
}

.logo-box h1 {
  font-size: 48px;
  font-weight: 900;
  text-shadow: 2px 4px rgba(255, 255, 255, 0.25);
  text-align: center;
  margin-top: 10px;
}

.slogan {
  font-size: 20px;
  font-weight: 200;
  font-style: italic;
  text-align: center;
  margin-top: 5 px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 50%;
  padding: 15px;
  margin: 0 auto;

  color: #313131;
  font-size: 12px;

  appearance: none;
  border: none;
  outline: none;
  background: none;


  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;

}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 80px;
  font-weight: 900;

  text-shadow: 2px 4px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.weather-box .weather-status {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.weather-box .feels-like {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}


</style>
