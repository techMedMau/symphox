<template>
  <div class="product">
    <div class="img_inner">
      <img
        class="img_resp"
        :class="{ grayScale: isComplete }"
        :src="item.logo"
        alt="logo"
        width="312"
        height="312" />
    </div>
    <div class="product_info">
      <div class="flex-between">
        <p class="product_info_status" :style="{color: isComplete? 'black':'#64AD48'}">{{item.status.type}}</p>
        <time v-if="!isComplete" class="product_info_date">預計出貨：{{item.date}}</time>
      </div>
      <p class="product_info_name">{{item.name}}</p>
    </div>
    <span class="material-icons" style="color: rgb(108, 108, 108);">
arrow_forward_ios
</span>
  </div>
</template>

<script>
export default {
  name: 'Product',
  props:{
    item: {
      type:Object,
      required: true,
    },
    isComplete: {
      type: Boolean,
      required: false,
      default: false,
    }

  }
}
</script>

<style lang="scss">

.product{
  &+&, &+.listBlock{
    border-top: 2px solid #E5E5E5;
  }
  display: flex;
  width: 100%;
  height: 190px;
  align-items: center;
  padding: 33px;
  &_info{
    margin-right: 15px;
    width: 395px;
    &_date{
      top:20px;
      right: 10px;
      font-size: 25px;
    }
    &_status{
      font-size: 25px;
    }
    &_name{
      margin-top: 20px;
      color: rgb(108, 108, 108);
      letter-spacing: 1px;
      line-height: 2.5rem;
      font-size: 26px;
    }
  }
}

.img{
  &_inner{
    width: 100px;
    margin-right: 33px;
  }
  &_resp{
    width: 100px;
    height: auto;
    display: block;
  }
}

.grayScale{
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
}

.flex-between{
  display: flex;
  justify-content: space-between;
}

</style>
