<template>
  <div id="app" class="container">
    <div class="listBlock">
      <p class="listBlock_status">進行中</p>
    </div>
    <Product v-for="(item,index) in dealingList" :key="'dealing'+index" :item="item"/>
    <div class="listBlock">
       <p class="listBlock_status">已完成</p>
    </div>
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
  box-sizing: border-box;
}

p{
  margin: 0;
}

.container{
  width: 650px;
  margin: 0 auto;
  border: 2px solid #E5E5E5;
}

.listBlock{
  padding: 20px 33px;
  background-color: #F5F5F5;
  border-bottom: 2px solid #E5E5E5;
  &_status{
    border-left: 7px solid #64AD48;
    text-indent: 1rem;
    font-size: 26px;
  }
}
</style>
