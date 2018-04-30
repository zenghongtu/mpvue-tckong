<!--<template>-->
  <!--<div class="container" @click="clickHandle('test click', $event)">-->

    <!--<div class="userinfo" @click="bindViewTap">-->
      <!--<img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />-->
      <!--<div class="userinfo-nickname">-->
        <!--<card :text="userInfo.nickName"></card>-->
      <!--</div>-->
    <!--</div>-->

    <!--<div class="usermotto">-->
      <!--<div class="user-motto">-->
        <!--<card :text="motto"></card>-->
      <!--</div>-->
    <!--</div>-->

    <!--<form class="form-container">-->
      <!--<input type="text" class="form-control" v-model="motto" placeholder="v-model" />-->
      <!--<input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />-->
    <!--</form>-->
    <!--<a href="/pages/counter/main" class="counter">去往Vuex示例页面</a>-->
  <!--</div>-->
<!--</template>-->

<!--<script>-->
  <!--import card from '@/components/card'-->

  <!--export default {-->
    <!--data () {-->
      <!--return {-->
        <!--motto: 'Hello World',-->
        <!--userInfo: {}-->
      <!--}-->
    <!--},-->

    <!--components: {-->
      <!--card-->
    <!--},-->

    <!--methods: {-->
      <!--bindViewTap () {-->
        <!--const url = '../logs/main'-->
        <!--wx.navigateTo({ url })-->
      <!--},-->
      <!--getUserInfo () {-->
        <!--// 调用登录接口-->
        <!--wx.login({-->
          <!--success: () => {-->
            <!--wx.getUserInfo({-->
              <!--success: (res) => {-->
                <!--this.userInfo = res.userInfo-->
              <!--}-->
            <!--})-->
          <!--}-->
        <!--})-->
      <!--},-->
      <!--clickHandle (msg, ev) {-->
        <!--console.log('clickHandle:', msg, ev)-->
      <!--}-->
    <!--},-->

    <!--created () {-->
      <!--// 调用应用实例的方法获取全局数据-->
      <!--this.getUserInfo()-->
    <!--}-->
  <!--}-->
<!--</script>-->

<!--<style scoped>-->
  <!--.userinfo {-->
    <!--display: flex;-->
    <!--flex-direction: column;-->
    <!--align-items: center;-->
  <!--}-->

  <!--.userinfo-avatar {-->
    <!--width: 128rpx;-->
    <!--height: 128rpx;-->
    <!--margin: 20rpx;-->
    <!--border-radius: 50%;-->
  <!--}-->

  <!--.userinfo-nickname {-->
    <!--color: #aaa;-->
  <!--}-->

  <!--.usermotto {-->
    <!--margin-top: 150px;-->
  <!--}-->

  <!--.form-control {-->
    <!--display: block;-->
    <!--padding: 0 12px;-->
    <!--margin-bottom: 5px;-->
    <!--border: 1px solid #ccc;-->
  <!--}-->

  <!--.counter {-->
    <!--display: inline-block;-->
    <!--margin: 10px auto;-->
    <!--padding: 5px 10px;-->
    <!--color: blue;-->
    <!--border: 1px solid blue;-->
  <!--}-->
<!--</style>-->

<template>
  <div class="page">
    <view class="page__hd">
      <view class="page__title">Swiper</view>
      <view class="page__desc">滑块视图容器，这里采用小程序原生 swiper 组件实现。</view>
    </view>
    <div class="page__bd page__bd_spacing">
      <swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
        <div v-for="item in imgUrls" :key="index">
          <swiper-item>
            <image :src="item" class="slide-image" />
          </swiper-item>
        </div>
      </swiper>
            <button class="md-splash__start" @click="handleStart">立即体验</button>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        indicatorDots: true,
        autoplay: true,
        interval: 5000,
        duration: 900,
        circular: true,
        imgUrls: [
          'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
          'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
          'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
        ]
      }
    },
    methods: {
      swiperChange (e) {
        console.log('第' + e.mp.detail.current + '张轮播图发生了滑动')
      },
      animationfinish (e) {
        console.log('第' + e.mp.detail.current + '张轮播图滑动结束')
      },
      handleStart (e) {
        wx.switchTab({
          url: '../selected/main'
        })
      }
    }
  }
</script>

<style>
  .slide-image {
    width: 100%;
    height: 100%;
  }
</style>
