<template>
  <div>
    <div class="wrap">
      <!--{{image_info.title}}-->
      <!--<navigator :url="'../detail/main?p='+image_info.author_id" class="weui-media-box weui-media-box_appmsg"-->
                 <!--hover-class="weui-cell_active">-->
        <!--<div v-for="item in imgIdList" :key="item" style="width:100%;" class="weui-media-box__hd weui-media-box__hd_in-appmsg">-->
        <!--<image class="weui-media-box__thumb" :src="icon60" />-->
        <!--<image  class="weui-media-box__thumb" style="width: 100%; background-color: #eeeeee;"  mode="widthFix"-->
               <!--:src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" />-->
        <!--</div>-->
        <swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
          <div v-for="(item,index) in imgIdList" :key="index" @click="predivImage">
            <swiper-item>
              <image :src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" class="slide-image" />
            </swiper-item>
          </div>
        </swiper>

      <!--<swiper indicator-dots="{{indicatorDots}}"-->
              <!--autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">-->
        <!--<block v-for="(item,index) in imgIdList" :key="index">-->
          <!--<swiper-item>-->
            <!--<image src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" class="slide-image" width="355" height="150"/>-->
          <!--</swiper-item>-->
        <!--</block>-->
      <!--</swiper>-->
        <!--<div class="weui-media-box__bd weui-media-box__bd_in-appmsg">-->
        <!--<div class="weui-media-box__title">{{image_info.title}}</div>-->
        <!--<div class="weui-media-box__desc">{{image_info.content}}</div>-->
        <!--</div>-->
      <!--</navigator>-->
    </div>
  </div>
</template>

<script>
  export default {
    props: {image_info: Object},
    data () {
      return {
        userId: this.image_info.author_id,
        imgIdList: [],
        imgUrlList: [],
        indicatorDots: false,
        autoplay: false,
        interval: 5000,
        duration: 1000
      }
    },
    computed: {
      // https://photo.tuchong.com/ + user_id +/f/ + img_id
      // getImgurl (detail) {
      //   const info = detail
      //   const userId = info.user_id
      //   const imgId = info.img_id
      //   return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
      // }
    },
    methods: {
      // getImgurl (detail) {
      //   const info = detail
      //   const userId = info.user_id
      //   const imgId = info.img_id
      //   return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
      // }
      predivImage (e) {
        console.log(e)
        const that = this
        wx.previewImage({
          current: that.imgUrlList[e.identifier], // 当前显示图片的http链接
          urls: that.imgUrlList // 需要预览的图片http链接列表
        })
      }
    },
    mounted () {
      const imgList = this.image_info
      // const userId = img_list.author_id
      // const imgId0 = imgList.images[0].img_id
      const len = imgList.image_count
      const _imgIdList = this.imgIdList
      // for (let i = 0; i < len; i++) {
      //   _imgIdList.push(imgId0+)
      // }
      let i = 0
      while (i < len) {
        _imgIdList.push(imgList.images[i].img_id)
        this.imgUrlList.push('https://photo.tuchong.com/' + this.userId + '/f/' + imgList.images[i].img_id + '.jpg')
        i++
      }

      const app = getApp()
      app.globalData = { store: {
        imgList: _imgIdList
      } }
      // console.log('--------')
      // console.log(app)
    }
  }
</script>

<style>

</style>
