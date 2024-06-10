<script>
import CardWidget from './components/CardWidget.vue';
import Loader from './components/Loader.vue';
export default {
  components: {
    Loader,
    CardWidget,
  },
  data() {
    return {
      dataWeather: null,
      latitude: 0,
      longitude: 0,
      imgUrl: '',
      locationName: '',
    };
  },
  methods: {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          this.latitude = position.coords.latitude;
          this.longitude = position.coords.longitude;
        });
      }
    },
    async getDateWeather() {
      const data = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&units=metric&appid=b03ad4edd819db73e6e3bd625d476c9a`,
      );

      this.dataWeather = await data.json();

      this.locationName = this.dataWeather.weather[0].description;
    },
    async getPhoto() {
      if (this.locationName !== '') {
        const data = await fetch(
          `https://api.unsplash.com/search/photos?query=${this.locationName}&client_id=Pd0UMRRdgiONmygFYphu3JTi3XOcRyYMQHG-4Mt5OEU`,
        );

        const dataObject = await data.json();
        this.imgUrl = await dataObject.results[0].urls.small;
      }
    },
  },
  mounted() {
    this.getLocation();
  },
  watch: {
    latitude() {
      this.getDateWeather();
    },
    locationName() {
      this.getPhoto();
    },
  },
};
</script>

<template>
  <Loader v-if="dataWeather === null" />
  <CardWidget
    v-else
    :imgUrl="imgUrl"
    :temp="dataWeather.main.temp.toFixed()"
    :name="dataWeather.name"
    :description="dataWeather.weather[0].description"
    :temp-min="dataWeather.main.temp_min.toFixed()"
    :temp-max="dataWeather.main.temp_max.toFixed()"
    :icon="dataWeather.weather[0].icon"
  />
</template>

<style scoped lang="scss"></style>
