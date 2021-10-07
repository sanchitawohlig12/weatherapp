<template>
  <v-container>
    <h1 class="display-1 text-center justify-center">Weather App</h1>
    <v-flex xs12>
      <v-card color="blue-grey darken-2" dark>
        <v-card-text>
          <v-layout row wrap>
            <v-flex
              v-if="weather.weather"
              xs12
              md4
              class="text-xs-center"
              style="text-align: center"
            >
              <h2 style="font-weight: bolder">Temperature</h2>
              <br />
              <h6 class="display-1">{{ weather.name }}</h6>
              <img :src="icon" alt="weather icon" />
              <p>
                <span class="display-1">{{ temp() }}&deg;c</span>
                <span class="caption ml-4">{{
                  weather.weather[0].description
                }}</span>
              </p>
            </v-flex>
            <v-flex
              v-if="weather.weather"
              xs12
              md4
              class="text-xs-center"
              style="text-align: center"
            >
              <h2>Wind and Pressure:</h2>
              <br />
              <h3 class="headline">
                Wind:{{ weather.wind.speed }} m/s({{ weather.wind.deg }} &deg;)
              </h3>
              <h3 class="headline mt-4">
                Humidity:{{ weather.main.humidity }}%
              </h3>
              <h3 class="headline mt-4">
                Pressure:{{ weather.main.pressure }} hPa
              </h3>
            </v-flex>
            <v-flex
              v-if="weather.weather"
              xs12
              md4
              class="text-xs-center"
              style="text-align: center"
            >
              <h2>Min and Max Temperature</h2>
              <br />
              <h3 class="headline mt-4">
                Max Temperature:{{ Math.round(weather.main.temp_max - 273) }}
                &deg;C
              </h3>
              <h3 class="headline mt-4">
                Min Temperature:{{ Math.round(weather.main.temp_min - 273) }}
                &deg;C
              </h3>
            </v-flex>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs12 class="mt-4">
      <v-form @submit.prevent="getWeatherInfo">
        <v-text-field
          v-model="city"
          label="Enter City Name"
          solo
        ></v-text-field>
      </v-form>
    </v-flex>
  </v-container>
</template>

<script>
export default {
  asyncData({ params, $axios }) {
    return $axios
      .$get(
        `https://api.openweathermap.org/data/2.5/weather?q=Mumbai&appid=f92984077a81de9be6161b29fa81004d`
      )
      .then((res) => {
        return { weather: res }
      })
  },
  data() {
    return {
      city: 'Mumbai',
    }
  },

  computed: {
    icon() {
      return this.weather.weather
        ? `https://openweathermap.org/img/w/${this.weather.weather[0].icon}.png`
        : ''
    },
  },
  methods: {
    getWeatherInfo() {
      this.$axios
        .$get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=f92984077a81de9be6161b29fa81004d`
        )
        .then((res) => (this.weather = res))
    },
    temp() {
      return this.weather.main ? Math.round(this.weather.main.temp - 273) : ''
    },
  },
}
</script>

<style>
</style>