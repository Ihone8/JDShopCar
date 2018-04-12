<template>
  <div id="app">
    <shopcar></shopcar>
  </div>
</template>

<script>
import JDShopCar from './components/JDShopCar'
export default {
  name: 'App',
  data:{},
  components: {
    shopcar:JDShopCar
  },
  mounted(){
    //this.GetFormText();
  },
  methods:{
    GetFormText:function(text){
       //alert(text)
      //  this.$http.get("./../static/data.json")
      //  .then(function(data){
      //    console.log(data.data)
      //  })
    }
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
