<template>
    <q-page class="flex column">
        <div class="col q-pt-lg q-px-md">
            <q-input
                v-model="search"
                placeholder="Search"
                dark
                borderless
            >
                <template v-slot:before>
                    <q-icon
                        @click="getLocation"
                        name="my_location"
                    />
                </template>

                <template v-slot:append>
                    <q-btn round dense flat icon="search" />
                </template>
            </q-input>
        </div>

        <template v-if="weatherData">
            <div class="col text-white text-center">
                <div class="text-h4 text-weight-light">
                    Ukraine
                </div>
                <div class="text-h6 text-weight-light">
                    Rain
                </div>
                <div class="text-h1 text-weight-thin">
                    22&deg;
                </div>
            </div>
        </template>

        <template v-else>
            <div class="col column text-center text-white">
                <div class="col text-h2 text-weight-thin">
                    Quasar<br>Weather
                </div>

                <q-btn
                    @click="getLocation"
                    color="col"
                    flat
                >
                    <q-icon left size="3em" name="my_location" />
                    <div>Find my location</div>
                </q-btn>
            </div>
        </template>
    </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: ''
    }
  },
  methods: {
    getLocation () {
      navigator.geolocation.getCurrentPosition(position => {
        console.log('position: ', position)
        this.lat = position.coords.latitude
        this.lon = position.coords.latitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords () {
      this.axious(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`)
        .then(response => {
          this.weatherData = response.data
        })
    }
  }
}
</script>

<style lang="sass">
.q-page
    background: linear-gradient(to bottom, #3a7bd5, #3a6073);
</style>
