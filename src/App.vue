<template>
  <div id="app" class="container">
    <Product v-for="(item,index) in dealingList" :key="'dealing'+index" :item="item"/>
    <Product v-for="(item,index) in completeList" :key="'complete'+index" :item="item" :isComplete="true"/>
  </div>
</template>

<script>
import Product from './components/Product'

export default {
  name: 'App',
  components: {
    Product
  },
  data(){
    return {
      productList: null,
    }
  },
  created(){
    this.load()
  },
  methods: {
    async load(){
      try {
        let data = await fetch('http://localhost:3000/orders')
        if(!data.ok){
          throw Error('no data available')
        }
        this.productList = await data.json()
      } catch (err) {
        console.log(err.message)
      }
    },
  },
  computed: {
    dealingList(){
      if(!this.productList) return
      const arr = this.productList
      .filter(item => item.status.code === 1 || item.status.code === 2)
      arr.map((item)=>{
        let date = item.date.split('/')
        date[0] = date[0]*1+1911+''
        item.dateMilliseconds = Date.parse(date.join('-'))
      })
      arr.sort((a, b) => b.dateMilliseconds-a.dateMilliseconds)
      return arr
    },
    completeList(){
      if(!this.productList) return
      const arr = this.productList.filter(item => item.status.code === 3 || item.status.code === 4)
      arr.map((item)=>{
        let date = item.date.split('/')
        date[0] = date[0]*1+1911+''
        console.log(date)
        item.dateMilliseconds = Date.parse(date.join('-'))
      })
      arr.sort((a, b) => b.dateMilliseconds-a.dateMilliseconds)
      return arr
    }
  }
}
</script>

<style lang="scss">
* {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  // color: #2c3e50;
  box-sizing: border-box;
}

p{
  margin: 0;
}

.container{
  width: 650px;
  margin: 0 auto;
}
</style>
