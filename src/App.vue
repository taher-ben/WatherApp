<template>
  <div
    class="bg-cover -z-10 bg-no-repeat w-full h-screen blur-sm animated-bg"
    style="
      background-image: url(/src/assets/bg.jpeg);
      background-size: cover; 
      background-position: 0 0;
    "
  ></div>
  <div
    class="absolute top-[50%] left-[50%] -translate-x-[50%] -translate-y-[50%] bg-gray-600 bg-opacity-50 px-16 pb-16 rounded-xl"
  >
    <div class="flex flex-col items-center">
      <h3 class="mb-3 text-white text-2xl font-bold pt-16"> HD4K WEATHER </h3>
      <h5 class="py-8 text-balance text-white">Enter a city to get the weather</h5>
      <div class="">
        <input
          class="outline-none px-2 py-2 bg-white mb-6"
          @keyup.enter="fetchingdata"
          v-model="location"
          type="text"
          placeholder="Put your number"
        />
        {{ linknum }}
      </div>
      <div v-if="datas">
        <div class="mb-2 text-2xl text-white rounded-r-lg">{{ datas.name }} {{ datas.sys.country }}</div>
        <img class="mb-2" :src="`http://openweathermap.org/img/wn/${datas.weather[0].icon}@2x.png`" alt="">
        <div class="text-2xl text-white text-center mb-2">{{ datas.weather[0].main }}</div>
        <div class="text-2xl text-white text-center mb-2">{{ datas.weather[0].description }}</div>
        <div class="flex justify-center">
          <div @dblclick="temp = true" class="text-2xl text-white p-4 cursor-pointer" v-if="temp == false">
            {{ Math.round(datas.main.temp - 272.15) }}°C
          </div>
          <div class="text-2xl text-white p-4 cursor-pointer" @dblclick="temp = false" v-else>{{ Math.round(datas.main.temp) }}°F</div>
        </div>
      </div>
      <div v-else>No data</div>
    </div>
  </div>
</template>

<!-- <style scoped>
.animated-bg {
  animation: loopBackground 5s linear infinite; 
}

@keyframes loopBackground {
  0% {
    background-position: 0% ;
  }
  50% {
    background-position: 100% ; 
  }
  100% {
    background-position: 0%;
  }
  50% {
    background-position: 100% ;
  }
  0% {
    background-position: 0%;
  }
}
</style> -->

<script>
import axios from "axios";
export default {
  data() {
    return {
      api_key: "c4541237dca615dffc4205a40a9373c2",
      datas: null,
      linknum: "",
      temp: null,
      location: "tripoli",
      id: "",
    };
  },
  methods: {
    fetchingdata() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&lang=ar&appid=${this.api_key}`
        )
        .then((response) => {
          this.datas = response.data;
          console.log(this.datas);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    template() {
      this.temp = !this.temp;
    },
  },
  mounted() {
    console.log(this.datas);
    this.fetchingdata();
  },
};
</script>
