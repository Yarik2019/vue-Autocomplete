<template>
  <autoCompele class="container" 
              :items="customers" 
              title="Look for a user"
              @change="onChange"
              :shouldReset="true" 
              @selectedItem="customerSelected"
              />
</template>

<script>
import autoCompele from './components/autoCompele.vue'
import customers from './assets/customers'
export default {
  name: 'App',
  data(){
    return{
      customers
    }
  },
  components: {
    autoCompele
  },
  methods:{
    customerSelected(customer){
      console.log(customer)
    },
    onChange(value){
      console.log(value)
    }
  },
  mounted(){
    this.customers = customers;
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.container{
  max-width: 800px;
  width: 100%;
  margin: 0 auto;
}
</style>
