<template>
  <div class="container my-5">
    <h1 class="title text-center" style="color: #4793ff">Weather</h1>
    <form class="search-location" v-on:submit.prevent="getWeather">
      <input
        type="text"
        class="form-control text-muted form-rounded p-4 shadow-sm"
        placeholder="Enter the city?"
        autocomplete="off"
        v-model="searchCity"
      />
      <button class="btn btn" v-on:click="addCity">Add city</button>
    </form>

    <div
      class="list"
      @remove="remove(index)"
      v-for="item in items"
      v-bind:key="item"
      v-bind:li="item"
    >
      {{ item }}
      <button class="close" @click="remove">x</button>
    </div>

    <p class="text-center my-3"></p>
    <div class="card rounded my-3 shadow-lg back-card overflow-hidden">
      <div class="card-top text-center">
        <div class="city-name my-3">
          <p>{{ weather.city }}</p>

          <p class="">{{ weather.country }}</p>
        </div>
      </div>
      <div class="card-body">
        <div class="card-mid">
          <div class="row">
            <div class="col-12 text-center temp">
              <span>{{ weather.temp }}&deg;C</span>
              <p class="my-4">{{ weather.description }}</p>
            </div>
          </div>
          <div class="row">
            <div class="col d-flex justify-content-between px-5 mx-5">
              <p><span>Min temp: </span> {{ weather.low_temp }}&deg;C</p>

              <p><span>Max temp: </span> {{ weather.hight_temp }}&deg;C</p>
            </div>
          </div>
        </div>
        <div class="card-bottom px-5 py-4 row">
          <div class="col text-center">
            <p>{{ weather.feels_like }}&deg;C</p>
            <span>Feels like</span>
          </div>
          <div class="col text-center">
            <p>{{ weather.humidity }}%</p>
            <span>humidity</span>
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
      searchCity: "",
      weather: {
        city: "",
        country: "",
        temp: "",
        description: "",
        feels_like: "",
        low_temp: "",
        hight_temp: "",
        humidity: "",
      },
      items: [],
    };
  },
  methods: {
    getWeather: async function () {
      const key = "c9209b7eea3ebd7cb5b3bbd134d66113";
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.searchCity}&units=metric&appid=${key}`;
      const res = await fetch(url);
      const data = await res.json();
      this.searchCity = "";
      this.weather.city = data.name;
      this.weather.country = data.sys.country;
      this.weather.temp = Math.round(data.main.temp);
      this.weather.description = data.weather.description;
      this.weather.feels_like = Math.round(data.main.feels_like);
      this.weather.low_temp = Math.round(data.main.temp_min);
      this.weather.hight_temp = Math.round(data.main.temp_max);
      this.weather.humidity = data.main.humidity;
    },
    addCity() {
      this.items.push(this.searchCity);
    },
    remove(index) {
      this.items.splice(index, 1);
    },
  },
};
</script>

<style></style>
