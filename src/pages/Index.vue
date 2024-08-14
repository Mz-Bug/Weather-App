<template>
  <q-page class="flex column">
    <q-img src="https://check-ur-weather.netlify.app/static/media/bg.7b83f7cda99cf768cbb6.jpg" height="100vh"
      fit="cover">
    </q-img>

    <!-- ------for search------ -->
    <div class="absolute-center">
      <div class="col q-pt-lg q-px-md ">
        <q-input @keyup.enter="getWeatherBySearch" :options="options" v-model="search" dark placeholder="Search"
          borderless class="bg-black q-px-lg q-mb-xl rounded-borders">
          <template v-slot:before>
            <q-icon @click="getlocation" name="my_location" />
          </template>

          <template v-slot:append>
            <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
          </template>
        </q-input>
      </div>
      <!-- ------for search------ -->

      <!-- -----------for name and degree----------- -->
      <template v-if="weatherdata">

            <div class="col  text-center text-white">
              <div class="text-h4 text-weight-light">{{ weatherdata.name }}</div>
              <div class="text-h6 text-weight-light">
                {{ weatherdata.weather[0].main }}
              </div>
              <div class="text-h1 text-weight-thin q-my-lg relative-position">
                <span>{{ Math.round(weatherdata.main.temp) }}</span>
                <span class="text-h4 relative-position degree">&deg;C</span>
              </div>
            </div>


      </template>
      <template v-else>

        <q-btn @click="getlocation" class="col q-my-xl">
          <q-icon left size="3em" name="my_location" color="white" />
          <div class="text-white">Find My location</div>
        </q-btn>

      </template>
    </div>
    <!-- -----------for name and degree----------- -->
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherdata: null,
      lat: null,
      lon: null,
      options: ["lahore", "islamabad", "Twitter", "Apple", "Oracle"],
    };
  },
  methods: {
    getlocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        console.log("position: ", position);
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
      });
    },
    getWeatherByCoords() {
      this.$axios(
        `https://api.openweathermap.org/data/2.5/weather?lat=35&lon=139&appid=1f79d04870143c5a62490196ef1eb00c&units=metric`
      ).then((response) => {
        this.weatherdata = response.data;
      });
    },
    getWeatherBySearch() {
      this.$axios(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=1f79d04870143c5a62490196ef1eb00c&units=metric`
      ).then((response) => {
        this.weatherdata = response.data;
      });
    },
  },
};
</script>

<style lang="scss">
.q-page {
  background: linear-gradient(to top, #000000, #434343);
}

.degree {
  top: -44px;
}

.absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}
</style>
