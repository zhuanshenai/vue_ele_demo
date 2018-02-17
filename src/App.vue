<template>
  <div id="app">
    <page-header :seller='seller'></page-header>
    <div class='tab'>
      <div class='tab_item'>
        <router-link to='/goods'>商品</router-link>
      </div>
      <div class='tab_item'>
        <router-link to='/ratings'>评价</router-link>
      </div>
      <div class='tab_item'>
        <router-link to='/seller'>商家</router-link>
      </div>
    </div>
    <router-view :seller='seller' :goods='goods' :ratings='ratings'></router-view>
  </div>
</template>

<script>
import pageHeader from './components/header/pageHeader'
export default {
  components: {
    pageHeader
  },
  data(){
    return {
      goods: [],
      ratings: [],
      seller: {}
    }
  },
  created(){
    this.$http.get('../static/data.json').then(res =>{
      this.goods = res.body.goods;
      this.ratings = res.body.ratings;
      this.seller = res.body.seller;
    })
  }
}
</script>

<style lang='sass'>
@import './assets/reset.sass'
#app
  .tab
    height: 40px
    display: flex
    width: 100%
    border-bottom: 1px solid #e5e5e5
    .tab_item
      flex: 1
      text-align: center
      height: 40px
      line-height: 40px
      a
        color: #4d555d
        font-weight: 400
        &.router-link-active 
          color: #f01414 
</style>
