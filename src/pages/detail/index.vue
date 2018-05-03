<template>
  <!--<div>-->
    <!--<div v-for="i in imgUrlList" :key="i" v-text="i"></div>-->
    <!--<image v-for="(v,i) in imgUrlList" :src="v" mode="widthFix" :key="i" class="card-image"/>-->
    <!--<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">-->
    <!--<div v-for="(url,index) in imgUrlList" :key="index">-->
    <!--<swiper-item>-->
    <!--<image :src="url" class="slide-image" mode="widthFix" />-->
    <!--</swiper-item>-->
    <!--</div>-->
    <!--</swiper>-->

    <!--<swiper style="width:100%;height: 100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">-->
      <!--<div style="width:100%;height: 100vh;" v-for="(url,i) in imgUrlList" :key="i">-->
        <!--<swiper-item style="width:100%;height: 100vh;">-->
          <!--<image :src="url"  mode="widthFix" style="width:100%;"/>-->
        <!--</swiper-item>-->
      <!--</div>-->
    <!--</swiper>-->

  <swiper style="height:100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
    <div  v-for="(url,i) in imgUrlList" :key="i">
      <swiper-item >
        <image :src="url"  mode="widthFix" style="width:100%;vertical-align:middle;"/>
      </swiper-item>
    </div>
  </swiper>


  <!--</div>-->
</template>

<script>
  // import card from '@/components/card'
  // import Fly from 'flyio/dist/npm/wx'

  // let fly = new Fly()
  export default {
    data () {
      return {
        imgUrlList: [],
        indicatorDots: true,
        autoplay: false,
        interval: 5000,
        duration: 900,
        circular: false
      }
    },
    // components: {
    //   card
    // },
    computed: {
    },
    methods: {
      // predivImage (e) {
      //   // console.log(e)
      //   const that = this
      //   wx.previewImage({
      //     current: that.imgList[e.identifier], // 当前显示图片的http链接
      //     urls: that.imgList // 需要预览的图片http链接列表
      //   })
      // }
      swiperChange (e) {
        console.log('第' + e.mp.detail.current + '张轮播图发生了滑动')
      },
      animationfinish (e) {
        console.log('第' + e.mp.detail.current + '张轮播图滑动结束')
      }

    },
    mounted () {
      const p = this.$root.$mp.query.p
      if (!p) {
        console.log('navigateBack')
        return wx.navigateBack()
      }
      // this.p = p
      this.imgUrlList = this.$root.$mp.appOptions[p]
    }
    // async onPullDownRefresh () { // 下拉刷新
    //   this.isLoad = 1
    //   // this.clearState()
    //   await this.getSelectedData('pull')
    //   wx.stopPullDownRefresh()
    // },
    // onReachBottom () { // 上拉加载
    //   this.isLoad = 1
    //   this.getSelectedData('bottom')
    // },

    // created () {
    //   // 调用应用实例的方法获取全局数据
    //   this.getSelectedData()
    // }

  }
</script>

<style scoped>

</style>
