<template>


  <swiper style="height:100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
    <div  v-for="(imgId,i) in imgIdList" :key="i">
      <swiper-item >
        <image :src="'https://photo.tuchong.com/' + userId + '/f/' + imgId + '.jpg'"  mode="widthFix"  :class="isEndways[i]?'detail_img':'detail_img_c'" />
        <text>{{content}}</text>
        <!--https://api.tuchong.com/images/ + imgIdList[i] + /exif-->
        <!--<image :src="url"  mode="heightFix" else class="detail_img_rotate"/>-->
      </swiper-item>
    </div>
  </swiper>

</template>

<script>
  export default {
    data () {
      return {
        imgIdList: [],
        isEndways: [],
        // content: '',
        userId: '',
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
      const r = this.$root.$mp.appOptions[p]
      this.imgIdList = r.imgIdList
      this.userId = r.userId
      this.isEndways = r.isEndways
      // this.content = r.content
      // console.log(r)
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

<style >
  .detail_img{
    width:100%;
    /*vertical-align:middle;*/
  }
  .detail_img_c{
    margin-top:200rpx;
    width:100%;
    /*vertical-align:middle;*/
  }

</style>
