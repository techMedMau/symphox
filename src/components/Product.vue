<template>
  <div class="product">
    <div>
      <img :src="item.logo" alt="logo" width="312" height="312" />
    </div>
    <div>
      <time>{{item.date}}</time>
      <p>{{item.status.type}}</p>
      <h4>{{item.name}}</h4>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Product',
  props:{
    item: Object,
    required: true,
  },
  data(){
    return{

    }
  }
}
</script>

<style>

</style>
