<template>
  <div class="weather" :class="weatherClass">
    <div class="container">

      <div class="card weather-form">
        <input type="text" class="weather-form_input" v-model="searchQuery" @keyup.enter="weatherSearch"
               placeholder="Enter city">
        <button class="weather-form_btn" @click="weatherSearch">Search</button>
      </div>

      <p class="card weather-load" v-if="loading">Loading...</p>


      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">

        <div class="card" v-if="error">Error</div>

        <div class="weather-info_text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>

        </div>
      </div>

    </div>
    <div class="weather-bg">
      <div>
        <img class="weather-bg_img bg" src="./assets/bg.jpg" alt="App Background">
        <img class="weather-bg_img overcast" src="./assets/overcast.jpg" alt="Overcast">
        <img class="weather-bg_img partly-cloudy" src="./assets/partly-cloudy.jpeg" alt="Partly Cloudy ">
        <img class="weather-bg_img sunny" src="./assets/sunny.jpeg" alt="Sunny">

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return{
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny';
      } else if (this.description.includes('Overcast')) {
        return 'overcast';
      } else if (this.description.includes('Partly cloudy')) {
        return 'partly-cloudy';
      } else {
        return '';
      }
    }
  },
  methods:{
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=217eba0b479043ba9f4213502242808&q=${this.searchQuery}`)
          .then(response => response.json())
          .then(data =>{
            this.loading = false;
            this.location = data.location.name;
            this.temperature = data.current.temp_c;
            this.description= data.current.condition.text;
            this.resetSearchQuery();
          })
          .catch(error => {
            this.error = true;
            this.loading = false;
            console.error(error);
          });
    },
    resetSearchQuery() {
      this.searchQuery = '';
    }
  }
}
</script>