<template>
  <div class="page_wrap">
    <image :class="isfull ? 'img_full':'img_no_full'" @click="handleStart" :src="url" mode="widthFix"/>
    <button class="splash_start" @click="handleStart">立即体验</button>
  </div>
</template>

<script>
  import Fly from 'flyio/dist/npm/wx'

  let fly = new Fly()

  export default {
    data () {
      return {
        isfull: true,
        url: ''
      }
    },
    methods: {
      handleStart (e) {
        wx.switchTab({
          url: '../selected/main'
        })
      }
    },
    created () {
      const that = this
      fly.get('http://127.0.0.1:9000/api/v1/wallpaper/?token=770fed4ca2aabd20ae9a5dd77471')
        .then(function (rsp) {
          if (rsp.data.status === 'ok') {
            const _d = rsp.data.data
            const _img = _d[0].images[0]
            _img.height > _img.width ? that.isfull = true : that.isfull = false
            that.url = 'https://photo.tuchong.com/' + _img.user_id + '/f/' + _img.img_id + '.jpg'
          } else {
            console.log('status is error')
          }
        })
        .catch(function (error) {
          console.log(error)
        })
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

  .img_full {
    height: 100%;
    width: 100%;
  }
  .img_no_full {
    width: 100%;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }

  .splash_start {
    position: absolute;
    bottom: 100rpx;
    left: 50%;
    color: #7A7A7A;
    transform: translateX(-50%);
    background-color: rgba(251, 251, 251, 0.6);
  }
</style>
