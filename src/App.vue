<template>
  <div id="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress.enter="fetchWeather"
        />
        <select class="dropdown-menu" v-model="selectedOption" @change="navigateToLink">
          <option value="" disabled selected>Tugas Fiere</option>
          <option v-for="option in options" :key="option.label" :value="option.link">{{ option.label }}</option>
        </select>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      api_key: '9aad72101bdeee05711e8adb37977aad',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      selectedOption: '',
      options: [
        { label: 'Tugas 1', link: 'https://tugas1fiere.vercel.app/' },
        { label: 'Tugas 2', link: 'https://fiere223510111.vercel.app/' },
        { label: 'Tugas 3', link: 'https://223510824.vercel.app/' },
        { label: 'Tugas 4', link: 'https://tugas04.vercel.app/' },
        { label: 'Tugas 5', link: 'https://tugas005.vercel.app/' },
        { label: 'Tugas 6', link: 'https://tugas6-pink.vercel.app/' },
        { label: 'Tugas 7', link: '' }
      ]
    };
  },
  methods: {
    fetchWeather() {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => res.json())
        .then(this.setResults)
        .catch(err => console.error('Error fetching weather data:', err));
    },
    setResults(results) {
      console.log(results); // Logging hasil fetch request
      this.weather = results;
    },
    dateBuilder() {
      const d = new Date();
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    navigateToLink() {
      if (this.selectedOption) {
        window.open(this.selectedOption, '_blank');
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image: url('./assets/wat.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  display: flex;
  justify-content: center; /* Memusatkan secara horizontal */
  align-items: center; /* Memusatkan secara vertikal */
  margin-bottom: 30px;
  gap: 10px; /* Menambahkan jarak antara elemen */
}

.search-box .search-bar {
  display: block;
  width: 30%; /* Mengurangi lebar menjadi 70% dari elemen induknya */
  padding: 10px; /* Mengurangi padding menjadi 10px */
  color: #313131;
  font-size: 20px;
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

.search-box .dropdown-menu {
  padding: 10px; /* Padding dropdown menu */
  font-size: 20px;
  color: #313131;
  background-color: rgba(255, 255, 255, 0.5);
  border: none;
  outline: none;
  border-radius: 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.search-box .dropdown-menu:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
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

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
