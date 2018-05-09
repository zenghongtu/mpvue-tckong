<template>

  <div class="wrap">

    <div @click="top_wrap_handler"  class="top_wrap">
      <text class="text_title">{{image_info.title}}</text>
    </div>
    <navigator class="img_nav" :url="'../detail/main?id='+image_info._id">
      <!--<image :src="'https://photo.tuchong.com/' + userId + '/f/' + image_info.images[0].img_id + '.jpg'" mode="widthFix"-->
      <image
        :src="'https://lf1-tccdn-tos.pstatp.com/img/tuchong.fullscreen/'+image_info.images[0].img_id+'~cs_360x640_q75.jpeg'"
        mode="widthFix"
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
          <text v-show="viewHandler"> 🔥 {{image_info.views}}</text>
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
      },
      viewHandler () {
        return (this.image_info.views !== undefined)
      }
      // getTitle () {
      //   const t = this.image_info.title
      //   const r = t.length > 18 ? t.slice(0, 19) + '...' : t
      //   return r
      // }
    },
    methods: {
      top_wrap_handler () {
        wx.setClipboardData({
          data: this.image_info.url,
          success: function (res) {
            wx.getClipboardData({
              success: function (res) {
                // console.log(res.data)
                wx.showToast({
                  title: '图虫地址成功复制到剪切板',
                  icon: 'none',
                  duration: 1200
                })
              }
            })
          }
        })
      }
    },
    mounted: function () {
      const imageInfo = this.image_info

      const len = imageInfo.image_count

      let i = 0
      const imgIdList = []
      const isEndways = []
      while (i < len) {
        // _imgIdList.push(imageInfo.images[i].img_id)
        const _img = imageInfo.images[i]
        const _id = _img.img_id
        // this.imgUrlList.push('https://photo.tuchong.com/' + this.userId + '/f/' + _id + '.jpg')
        // if (_img.width / _img.height < 1.2) {
        imgIdList.push(_id)
        // }
        // isEndways.push(_img.width / _img.height < 0.85)
        i++
      }

      let id_ = '' + imageInfo._id
      wx.setStorage({
        key: id_,
        data: {imgIdList, isEndways, userId: this.userId, title: imageInfo.title}
      })
      // this.$root.$mp.appOptions[id_] = {imgIdList, isEndways, userId: this.userId}
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
    padding: 34rpx 0;
    position: relative;
    top: 0;
    left: 0;
  }

  .text_title {
    color: rgba(0, 0, 0, 0.7);
    position: absolute;
    top: 10rpx;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    text-align: center;
  }

  .img_nav {
    position: relative;
    top: 0;
    left: 0;
  }

  .image_count {
    position: absolute;
    top: 26rpx;
    left: 64rpx;
    color: rgba(255, 255, 255, 0.5);
  }

  .count_icon {
    position: absolute;;
    top: 30rpx;
    left: 16rpx;
    width: 42rpx;
    height: 44rpx;
  }

  .text_content {
    font-size: 24rpx;
    padding: 0 20rpx;
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
    margin-bottom: -10rpx;
  }

  .line {
    width: 100%;
    height: 25rpx;
    background: rgba(85, 85, 85, 0.1);
  }
</style>
