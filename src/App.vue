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
      <div>
        <input
          class="outline-none px-2 py-2 bg-white mb-6"
          @keyup.enter="fetchingdata"
          v-model="location"
          type="text"
          placeholder="Enter a city"
        />
        {{ linknum }}
      </div>
      <div v-if="datas">
        <div class="mb-2 text-2xl text-white rounded-r-lg">{{ datas.name }} {{ datas.sys.country }}</div>
        <img class="mb-2" :src="`http://openweathermap.org/img/wn/${datas.weather[0].icon}@2x.png`" alt="">
        <div class="text-2xl text-white text-center mb-2">{{ datas.weather[0].main }}</div>
        <div class="text-2xl text-white text-center mb-2">{{ datas.weather[0].description }}</div>
        <div class="flex justify-center">
          <div v-if="!temp" @dblclick="toggleTemp" class="text-2xl text-white p-4 cursor-pointer">
            {{ Math.round(datas.main.temp - 273.15) }}°C
          </div>
          <div v-else @dblclick="toggleTemp" class="text-2xl text-white p-4 cursor-pointer">
            {{ Math.round((datas.main.temp - 273.15) * 9/5 + 32) }}°F
          </div>
        </div>
      </div>
      <div v-else>
        <div v-if="loading" class="spinner"></div>
      </div>
    </div>
  </div>
  <div class="fixed bottom-10 right-10">
    <div v-if="showFAB" class="transition duration-150 ease-in-out">
      <div class="cursor-pointer  text-white bg-gray-700 px-3 py-1 rounded-full mb-2" @click="toggleTemp">°F</div>
      <div  class="cursor-pointer  text-white bg-gray-700 px-3 py-1 rounded-full mb-2" @click="toggleTemp">°C</div>
      <div  class="cursor-pointer  text-white bg-gray-700 px-3 py-1 rounded-full mb-2 mx-auto" @click="lung = ar">ar</div>
      <div  class="cursor-pointer  text-white bg-gray-700 px-3 py-1 rounded-full mb-2 mx-auto" @click="lung = en">en</div>
    </div>
    <div @click="showFAB = !showFAB" class="text-2xl text-white bg-gray-300 px-4 py-2 rounded-full cursor-pointer">
      {{ icon }}
    </div>
  </div>
  <RouterView />
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      icon:'+',
      lung:'ar',
      api_key: "c4541237dca615dffc4205a40a9373c2",
      datas: null,
      showFAB: false, 
      linknum: "",
      temp: false,   
      location: "",
      id: "",
      loading: false,  
    };
  },
  methods: {
    fetchingdata() {
      this.loading = true; 
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&lang=${this.lung}&appid=${this.api_key}`
        )
        .then((response) => {
          this.datas = response.data;
          this.loading = false;  
          console.log(this.datas);
        })
        .catch((error) => {
          this.loading = false; 
          console.log(error);
        });
    },
    toggleTemp() {
      this.temp = !this.temp;
      
    },
    changeicon(){
      if(this.showFAB === true){
        this.icon = '*'
      }else{
        this.icon = '+'
      }
    }
  },
};
</script>

<style scoped>
.spinner {
    width: 50px;
    height: 50px;
    border: 5px solid #ccc; 
    border-top: 5px solid #3498db;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
</style>
