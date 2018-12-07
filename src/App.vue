<template>
  <div id="app">
    <h1>即時畫面</h1>
    <div v-for="(item,index) in list" :key="index" style="display:inline-block">
      <img alt="Vue logo" @click="takeid(item.deletehash)" :src="item.link" style="width:400px;height:400px;">
    </div>
    <div>
      <button @click="remove">delete</button>
    </div>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'
const id = '0b83078a77addc3'; 
const token = '835efb5cbf4703dea60f226fd32660ff3729e2a6';
const album = '5YE5iYO'; 
export default {
  name: 'app',
  data(){
    return{
      list:null,
      uuid:'',
    }
  },
  components: {
    HelloWorld
  },
  methods:{
    takeid(id){
      this.uuid=id;
      console.log(this.uuid)
    },
    remove(){
      for(var i in this.list){
          console.log(this.list)
           if(this.list[i].deletehash==this.uuid){
             this.list.splice(i,1);
             let vm = this
             axios({
               method: 'GET',
                url:"https://api.imgur.com/3/album/" + album + "/remove_images?ids="+ vm.uuid,
              // responseType: 'json',
                'headers': {
                'Authorization': 'Bearer ' + token,
                'content-type': 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW',
                //'Authorization': 'Client-ID' + id
              },
               // form:'ids[]='+vm.uuid
             }).then(function(response){
                console.log(response.data)
              }).catch(function(error){
                  console.log(error)
              }) 
           }
        }
    }
  },
  mounted(){
    let vm = this;
      axios({
          method: 'GET',
          url:"https://api.imgur.com/3/album/" + album + "/images",
         // responseType: 'json',
          'headers': {
          'Authorization': 'Bearer ' + token,
          //'Authorization': 'Client-ID' + id
        }

      }).then(function(response){
        console.log(response.data)
        vm.list=response.data.data
      }).catch(function(error){
          console.log(error)
       })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
