<template>

<div>

  <swiper style="height:100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
    <div  v-for="(imgId,i) in imgIdList" :key="i">
      <swiper-item >
        <text v-show="show" :class="isEndways[i]?'exif_text_base exif_text_white':'exif_text_base exif_text_black'" v-text="exifs[i]"></text>
        <image @click="showExif" :src="'https://photo.tuchong.com/' + userId + '/f/' + imgId + '.jpg'"  mode="widthFix"  :class="isEndways[i]?'detail_img':'detail_img_c'" />
        <!--<text class="exif_text" v-for="info in exifs[i]" v-text="info"></text>-->
      </swiper-item>
    </div>
  </swiper>
  <!--<button @click="showExif" class="img_btn" type="default" plain="true" :loading="loading" >图片参数</button>-->
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
        // loading: true,
        imgIdList: [],
        isEndways: [],
        current: 0,
        exifs: [],
        userId: '',
        show: false
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
        // this.showLoading()
        const i = e.mp.detail.current
        this.getExifData(i + 1)
        this.current = i
      },
      getExifData (i) {
        if (!this.imgIdList[i]) {
          return false
        }
        const that = this
        fly.get(`https://api.tuchong.com/images/${that.imgIdList[i]}/exif`)
          .then(function (rsp) {
            // console.log(rsp)
            const data = rsp.data
            if (data.result === 'SUCCESS') {
              const e = data.exif['摘要']
              let info = ''
              for (let item of e) {
                info += `${item.desc}: ${item.content}\n`
              }
              // that.exifs[i] = `${e[5].content}\n${e[0].content}\n${e[1].content}\n${e[2].content}\n${e[3].content}\n${e[4].content}`
              that.exifs[i] = info
            }
            // wx.hideLoading()
          })
          .catch(function (error) {
            // wx.hideLoading()
            console.log(error)
          })
      },
      showLoading () {
        wx.showLoading({
          title: 'Loading...'
          // mask: true
        })
      },
      showExif () {
        this.show = !this.show
      }
      // animationfinish (e) {
      //   console.log('第' + e.mp.detail.current + '张轮播图滑动结束')
      // },

    },
    mounted () {
      const p = this.$root.$mp.query.p
      if (!p) {
        console.log('navigateBack')
        return wx.navigateBack()
      }
      // this.showLoading()
      const r = this.$root.$mp.appOptions[p]
      this.imgIdList = r.imgIdList
      this.userId = r.userId
      this.isEndways = r.isEndways
      // this.content = r.content
      // console.log(r)
      this.getExifData(0)
      this.getExifData(1)
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

  .exif_text_base{
    font-size:23rpx;
    line-height:32rpx;
    position:absolute;
    bottom:80rpx;
    padding:20rpx;
    width:100%;
  }
  .exif_text_white{
    color:rgba(255,255,255,0.7);
    background-color: rgba(85,85,85,0.1);
  }
  .exif_text_black{
    color:rgba(0,0,0,0.7);
    background-color: rgba(85,85,85,0.1);
  }

  .img_btn{
    position:absolute;
    bottom:48rpx;
    width:100%;
    height:60rpx;
    font-size:27rpx;
    line-height:60rpx;
  }

</style>
