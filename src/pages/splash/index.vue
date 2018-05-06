<template>
  <div class="page_wrap">

    <swiper style="height:100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" >
      <div  v-for="(imgUrl,i) in imgUrls" :key="i">
        <swiper-item >
          <image @click="showExif" :src="imgUrl"  mode="widthFix"  class="img_full"/>
          <!--<text class="exif_text" v-for="info in exifs[i]" v-text="info"></text>-->
        </swiper-item>
      </div>
    </swiper>
    <div v-show="show" class="text_wrap">
      <text class="img_info">title: {{title}}</text><br/>
      <text class="img_info">author: {{author}} </text><br/>
      <text class="img_info">date: {{crt_time}}</text>
    </div>
    <button class="splash_start" @click="handleStart" > 点此进入 </button>
    <text class="copyright_text">©此相片(册)由图虫摄影爱好者分享，版权归原创作者所有</text>
  </div>
</template>

<script>
  import Fly from 'flyio/dist/npm/wx'

  let fly = new Fly()

  export default {
    data: function () {
      return {
        indicatorDots: true,
        autoplay: false,
        interval: 5000,
        duration: 900,
        circular: false,
        imgUrls: [],
        title: '',
        // content: '',
        crt_time: '',
        author: '',
        // imageCount: 1,
        show: false
      }
    },
    methods: {
      handleStart (e) {
        wx.switchTab({
          url: '../selected/main'
        })
      }
      // swiperChange (e) {
      //   // this.showLoading()
      //   const i = e.mp.detail.current
      //   if (this.imageCount === i) {
      //     wx.switchTab({
      //       url: '../selected/main'
      //     })
      //   }
      // }
    },
    created () {
      wx.showLoading({
        title: '加载中...',
        mask: true
      })
      const imgUrls = this.imgUrls
      const that = this
      fly.get('https://api.heta.xyz/v1/wallpaper/?token=770fed4ca2aabd20ae9a5dd77471')
        .then(function (rsp) {
          if (rsp.data.status === 'ok') {
            const _d = rsp.data.data[0]
            const imgIds = _d.images
            that.title = _d.title
            that.content = _d.content
            that.crt_time = _d.crt_time
            that.author = _d.author.name
            that.image_count = _d.image_count
            let i = 0
            for (let item of imgIds) {
              // https://lf1-tccdn-tos.pstatp.com/img/tuchong.fullscreen/15562999~cs_1080x1920_q75.jpeg
              imgUrls.push(`https://lf1-tccdn-tos.pstatp.com/img/tuchong.fullscreen/${item.img_id}~cs_1080x1920_q75.jpeg`)
              i++
              if (i > 3) {
                break
              }
            }
            that.show = true
          } else {
            console.log('status is error')
            wx.showToast({
              title: '无法访问，请稍后尝试',
              mask: true,
              icon: 'none',
              duration: 1500
            })
          }
        })
        .catch(function (error) {
          console.log(error)
          wx.showToast({
            title: '网络出错，请稍后尝试',
            mask: true,
            icon: 'none',
            duration: 1500
          })
        })
      wx.hideLoading()
    }
  }

</script>

<style>
  .page_wrap {
    height: 100vh;
    width: 100%;
    background-color: rgba(238, 238, 238,0.2);
    position: relative;
  }

  .text_wrap{
    position: absolute;
    top:30rpx;
    left:10rpx;
    background-color: rgba(0,0,0,0.1);
    color:rgba(255,255,255,0.5);
    padding:20rpx;
  }
  /*.img_info{*/
    /*width:100%;*/
  /*}*/
  .img_full {
    /*height: 100%;*/
    width: 100%;
  }
  /*.img_no_full {*/
    /*width: 100%;*/
    /*position: absolute;*/
    /*top: 50%;*/
    /*transform: translateY(-50%);*/
  /*}*/

  .splash_start {
    position: absolute;
    bottom: 100rpx;
    left: 50%;
    color: rgba(255,255,255,0.5);
    transform: translateX(-50%);
    /*width:100%;*/
    /*background-color: rgba(85,85,85, 0.1);*/
    background-color: transparent;
  }
  .copyright_text{
    position: absolute;
    bottom:35rpx;
    text-align: center;
    width: 100%;
    font-size:25rpx;
    color:rgba(0,0,0,0.4);
  }
</style>
