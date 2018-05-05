<template>

  <div class="wrap">
    <!--{{image_info.title}}-->
    <!--<navigator :url="'../detail/main?p='+image_info.author_id" class="weui-media-box weui-media-box_appmsg"-->
    <!--hover-class="weui-cell_active">-->
    <!--<div v-for="item in imgIdList" :key="item" style="width:100%;" class="weui-media-box__hd weui-media-box__hd_in-appmsg">-->
    <!--<image class="weui-media-box__thumb" :src="icon60" />-->
    <!--<image  class="weui-media-box__thumb" style="width: 100%; background-color: #eeeeee;"  mode="widthFix"-->
    <!--:src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" />-->
    <!--</div>-->
    <!--<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" :circular="circular" @change="swiperChange" @animationfinish="animationfinish">-->
    <!--<div v-for="(item,index) in imgIdList" :key="index" @click="predivImage">-->
    <!--<swiper-item>-->
    <!--<image :src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" class="slide-image" />-->
    <!--</swiper-item>-->
    <!--</div>-->
    <!--</swiper>-->

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
    <div class="top_wrap">
      <text class="text_title">{{image_info.title}}\n</text>
    </div>
    <navigator :url="'../detail/main?p='+image_info._id" style="position: relative;top:0;left:0;">
      <image :src="'https://photo.tuchong.com/' + userId + '/f/' + image_info.images[0].img_id + '.jpg'" mode="widthFix"
             class="card-image"/>
      <image class="count_icon" src="../../static/images/pic.png"></image>
      <text class="image_count">{{image_info.image_count}}</text>
      <text class="text_content" v-show="getContent">{{getContent}}</text>
    </navigator>
    <div class="bottom_wrap">
      <image :src="image_info.author.icon" class="profile"></image>
      <div class="text_wrap">
        <text class="text_name">{{image_info.author.name}}</text>
        <br/>
        <text class="text_time">{{image_info.crt_time}}</text>
        <div class="info_wrap">
          <text> 🔥 {{image_info.views}}</text>
          <text> 👍️ {{image_info.favorites}}</text>
          <text> 🤔 {{image_info.comments }}</text>
        </div>
      </div>
      <!--<br/>-->
      <!--<text class="text_tags">[{{image_info.tags}}]</text>-->
    </div>
    <view class="line"></view>

  </div>


</template>

<script>
  export default {
    props: {image_info: Object},
    data: function () {
      return {
        userId: this.image_info.author_id,
        // imgIdList: [],
        imgCount: this.image_info.image_count
        // imgUrlList: [],
        // direction: []
        // indicatorDots: false,
        // autoplay: false,
        // interval: 5000,
        // duration: 1000
      }
    },
    computed: {
      getContent () {
        const c = this.image_info.content
        let r = ''
        if (!c) {
          r = false
        } else {
          r = c.replace('\n', '')
        }
        return r
      }
      // getTitle () {
      //   const t = this.image_info.title
      //   const r = t.length > 18 ? t.slice(0, 19) + '...' : t
      //   return r
      // }
    },
    methods: {
      // getImgurl (detail) {
      //   const info = detail
      //   const userId = info.user_id
      //   const imgId = info.img_id
      //   return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
      // }
      // predivImage (e) {
      //   console.log(e)
      //   const that = this
      //   wx.previewImage({
      //     current: that.imgUrlList[e.identifier], // 当前显示图片的http链接
      //     urls: that.imgUrlList // 需要预览的图片http链接列表
      //   })
      // }
    },
    mounted () {
      const imgList = this.image_info

      const len = imgList.image_count

      let i = 0
      const imgIdList = []
      const isEndways = []
      while (i < len) {
        // _imgIdList.push(imgList.images[i].img_id)
        const _img = imgList.images[i]
        const _id = _img.img_id
        // this.imgUrlList.push('https://photo.tuchong.com/' + this.userId + '/f/' + _id + '.jpg')
        imgIdList.push(_id)
        isEndways.push(_img.height * 0.8 > _img.width)
        i++
      }

      let id_ = '' + imgList._id
      this.$root.$mp.appOptions[id_] = {imgIdList, isEndways, userId: this.userId}
      // app.globalData.store[id_] = this.imgUrlList
      // console.log('--------')
      // console.log(this.image_info._id)
      // console.log(app)
      // console.log(app)
    }
  }
</script>

<style>
  .card-image {
    width: 100%;
  }

  .top_wrap {
    padding: 15rpx 10rpx 0;
  }

  .text_title {
    font-size: 40rpx;
    float: left;
  }

  .image_count {
    position: absolute;
    top: 80rpx;
    left: 64rpx;
    color: rgba(255, 255, 255, 0.5);
  }

  .count_icon {
    position: absolute;;
    top: 82rpx;
    left: 16rpx;
    width: 42rpx;
    height: 44rpx;
  }

  .text_content {
    font-size: 24rpx;
    padding:0 20rpx;
    /*overflow: hidden;*/
    /*text-overflow: ellipsis;*/
    /*white-space: nowrap;*/
    /*display: block;*/
    /*padding-left: 10px;*/
    /*line-height: 32px;*/
  }

  .profile {
    width: 50rpx;
    height: 50rpx;
    border: 0 solid #ff0000;
    border-radius: 100rpx;
    background-color: #f10b2e;
    float: left;
  }

  .text_name,
  .text_time {
    margin-left: 10rpx;
  }

  .text_wrap {
    margin-left: 20rpx;
    font-size: 22rpx;
  }

  .info_wrap {
    width: 360rpx;
    float: right;
    line-height: 0;
    font-size: 30rpx;
  }

  .bottom_wrap {
    padding: 0 20rpx;
    height: 80rpx;
    margin-bottom:-10rpx;
  }

  .line {
    width: 100%;
    height:25rpx;
    background:rgba(85,85,85,0.1);
  }
</style>
