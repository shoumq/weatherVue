<template>
  <header>
    <div class="container">
      <a href="/" class="logo">Weather</a>
      <form class="form__city">
        <input type="text" class="input__city" v-model="city">
        <button @click="searchCity" class="primary">Search</button>
      </form>
      <p>{{ city }}</p>
    </div>
  </header>

  <main>
    <div class="container">
      <h1 class="city">{{ city }}</h1>

      <div class="wb_row">
        <swiper
            :slides-per-view="4"
            :space-between="50"
        >
          <swiper-slide v-for="(item, index) in dataArr" :key="index">
            <WeatherBlock :temp="Math.round(item.main.temp - 273.15) + ' °C'"
                          :time="(item.dt_txt).split(' ')[1].split(':')[0] + ':' + (item.dt_txt).split(' ')[1].split(':')[1]"
                          :date="(item.dt_txt).split(' ')[0].split('-')[1] + '.' + (item.dt_txt).split(' ')[0].split('-')[2]"
                          :desc="item.weather['description']"/>
          </swiper-slide>
        </swiper>
      </div>
    </div>
  </main>
</template>

<script>
import WeatherBlock from "@/components/WeatherBlock/WeatherBlock.vue";
import axios from "axios";
import {Swiper, SwiperSlide} from "swiper/vue";

import 'swiper/css';

export default {
  name: 'App',
  data() {
    return {
      city: "",
      dataArr: [],
      chCity: ""
    }
  },
  components: {
    WeatherBlock,
    Swiper,
    SwiperSlide,
  },
  mounted() {
    axios
        .get('http://ipwho.is')
        .then(response => (this.city = response.data.city))
        .catch(error => this.city = error);

    axios
        .get('http://api.openweathermap.org/data/2.5/forecast?q=Moscow&appid=779b1da14dc79dc4ba5ec20b63057396')
        .then(response => (this.dataArr = response.data.list))
        .catch(error => this.dataArr = error);
  },
  setup() {
    const onSwiper = (swiper) => {
      console.log(swiper);
    };
    const onSlideChange = () => {
      console.log('slide change');
    };
    return {
      onSwiper,
      onSlideChange,
    };
  }
}
</script>
