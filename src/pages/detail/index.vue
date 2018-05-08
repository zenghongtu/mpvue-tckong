<template>

<div>

  <swiper style="height:100vh;" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange">
    <div  v-for="(imgId,i) in imgIdList" :key="i">
      <swiper-item >
        <text v-show="show" :class="isEndways[i]?'exif_text_base exif_text_white':'exif_text_base exif_text_black'" >{{exifs[i]}}</text>
        <!--<button open-type="share" :id="imgId">share</button>-->
        <!--<image @click="showExif" :src="'https://photo.tuchong.com/' + userId + '/f/' + imgId + '.jpg'"  mode="widthFix"  :class="isEndways[i]?'detail_img':'detail_img_c'" />-->
        <image @click="showExif" :src="'https://lf1-tccdn-tos.pstatp.com/img/tuchong.fullscreen/'+imgId+'~cs_1080x1920.jpeg'"  mode="widthFix"  class="detail_img" />
        <!--<text class="exif_text" v-for="info in exifs[i]" v-text="info"></text>-->
      </swiper-item>
    </div>
  </swiper>
      <text class="img_order">{{current+1}}/{{imgCount}}</text>
      <button v-show="show" open-type="share" class="share_btn" >Share</button>
      <!--<button @click="predivImage"  class="predivImage_btn"  type="default" plain="true" >Preview</button>-->
      <button v-show="show" @click="wallpaperHandler"  class="wallpaper_btn" >Original</button>
</div>

</template>

<script>
  import Fly from 'flyio/dist/npm/wx'
  let fly = new Fly()
  export default {
    data: function () {
      return {
        indicatorDots: false,
        autoplay: false,
        interval: 5000,
        duration: 900,
        circular: false,
        // loading: true,
        imgIdList: [],
        imgCount: 1,
        isEndways: [],
        current: 0,
        exifs: [],
        userId: '',
        title: '图虫精选',
        show: false
      }
    },
    onShareAppMessage (e) {
      // const imgId = e.target.id
      // const url = 'https://photo.tuchong.com/' + this.userId + '/f/' + imgId + '.jpg'
      return {
        title: '来自图虫·精选：' + this.title,
        // imageUrl: url,
        success: function (res) {
          wx.showToast({
            title: '分享成功！',
            icon: 'success',
            duration: 400
          })
        }
      }
    },
    // components: {
    //   card
    // },
    computed: {
    },
    methods: {
      wallpaperHandler (e) {
        // console.log(e)
        // console.log('1')
        const imgUrlList = []
        // const userId = this.userId
        for (let imgId of this.imgIdList) {
          imgUrlList.push(`https://lf1-tccdn-tos.pstatp.com/img/tuchong.fullscreen/${imgId}~cs.jpeg`)
        }
        const i = this.current
        wx.previewImage({
          current: imgUrlList[i],
          urls: imgUrlList,
          fail (e) {
            console.log(e)
          }
        })
      },
      swiperChange (e) {
        // this.showLoading()
        const i = e.mp.detail.current
        this.getExifData(i + 1)
        this.current = i
        // this.show = !this.show
      },
      getExifData (i) {
        if (!this.imgIdList[i]) {
          return false
        }
        // if (this.show === true){
        //   this.show = false
        // }
        // this.show = !this.show
        const that = this
        fly.get(`https://api.tuchong.com/images/${that.imgIdList[i]}/exif`)
          .then(function (rsp) {
            // console.log(rsp)
            const data = rsp.data
            if (data.result === 'SUCCESS') {
              const e = data.exif['摘要']
              let info = ''
              if (e) {
                for (let item of e) {
                  info += `${item.desc}: ${item.content}\n`
                }
              // that.exifs[i] = `${e[5].content}\n${e[0].content}\n${e[1].content}\n${e[2].content}\n${e[3].content}\n${e[4].content}`
                that.exifs[i] = info
                // that.show = !that.show
              } else {
                that.exifs[i] = '获取相片信息失败，请稍后尝试'
              }
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
      this.show = false
      const id_ = this.$root.$mp.query.id
      this.current = 0
      this.exifs = []
      if (!id_) {
        console.log('navigateBack')
        return wx.navigateBack()
      }
      // this.showLoading()
      // const r = this.$root.$mp.appOptions[id_]
      try {
        const r = wx.getStorageSync(id_)
        if (r) {
          this.imgIdList = r.imgIdList
          this.imgCount = r.imgIdList.length
          this.userId = r.userId
          this.getExifData(0)
          this.getExifData(1)
          this.isEndways = r.isEndways
          this.title = r.title
          wx.setNavigationBarTitle({
            title: r.title
          })
        }
      } catch (e) {
        console.log('error:wx.getStorageSync-' + id_)
        // console.log(e)
      }
      // wx.getStorageSync({
      //   key: id_,
      //   success (e) {
      //     console.log(1)
      //     console.log(e)
      //   },
      //   fail () {
      //
      //   }
      // })
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
    margin-top:100rpx;
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

  .share_btn{
    position:absolute;
    bottom:9rpx;
    height:60rpx;
    font-size:25rpx;
    line-height:60rpx;
    right:30rpx;
    width:25%;
    color:rgba(255,255,255,0.8);
    background-color: rgba(85,85,85,0.1);
  }

  .wallpaper_btn{
    position:absolute;
    bottom:9rpx;
    height:60rpx;
    font-size:25rpx;
    line-height:60rpx;
    left:30rpx;
    width:25%;
    color:rgba(255,255,255,0.8);
    background-color: rgba(85,85,85,0.1);
  }

  .img_order{
    position:absolute;
    bottom:9rpx;
    height:60rpx;
    font-size:35rpx;
    line-height:60rpx;
    left:50%;
    transform: translateX(-50%);
    color:rgba(85,85,85,0.9)
  }

</style>
