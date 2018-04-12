<template>
  <div id="translateForm">
      <form v-on:submit="formSubmit">
          <input type="text" placeholder="输入要翻译的内容" v-model="textToTranslate">
          <select name="" id="">
              <option value="en">English</option>
          </select>
          <input type="submit" value="翻译">
      </form>
  </div>
</template>

<script>


export default {
  name: 'translateForm',
  data:function(){
      return {
          textToTranslate: ""
      }
  },
  methods:{
      formSubmit:function(e){
          //alert(this.textToTranslate)
          this.$emit("formSubmit",this.textToTranslate)
          e.preventDefault();
      }
  }
}
</script>

<style>

</style>
