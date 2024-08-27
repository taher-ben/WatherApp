<template>
  <div>
    <input @keyup.enter="fetchingdata" v-model="location" type="text" placeholder="Put your numver">
    {{ linknum }}
  </div>
  <div v-for="(item,index) in datas" :key="index" class="text-red-300 text-4xl">
    {{ item.id  }}
    {{ item.postID  }}
    {{ item.name  }}
    {{ item.email }}
    {{ item.body }}
    {{ list.weather.id }}
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      api_key:'c4541237dca615dffc4205a40a9373c2',
      datas:null,
      linknum: '',
      location:'Tripoli',
      id:''
    }
  },
  methods:{
    fetchingdata(){
      axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${this.location}&appid=c4541237dca615dffc4205a40a9373c2`)
      .then( (response) => {
        this.datas = response.data;
        console.log(this.datas)
      }).catch(function(error){
        console.log(error)
      })
    },
  }
}
</script>
