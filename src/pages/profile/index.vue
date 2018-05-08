<template>
  <div class="msg_wrap">
      <view class="section__title">e-mail:</view>
      <input class="input" name="input" v-model="msgData.openid" placeholder="that I can contact you" />
    <br>
    <view class="section__title">nickname:</view>
      <input class="input"  name="input" v-model="msgData.username" placeholder="your nickname" />
    <br>
    <view class="section__title">message:</view>
      <input class="input"  name="input" v-model="msgData.msg" placeholder="what you want to talk?" />
    <br>
    <button @click="confirm" class="confirm_btn">提交</button>

  </div>
</template>

<script>
// import card from '@/components/card'

      import Fly from 'flyio/dist/npm/wx'

      let fly = new Fly()
      export default {
        data: function () {
          return {
            msgData: {
              openid: '',
              username: '',
              msg: ''
            }
          }
        },
        methods: {
          confirm () {
            wx.showLoading({
              title: 'loading...',
              mask: true
            })
            this.request()
          },
          request () {
            const that = this
            fly.post(`https://api.heta.xyz/v1/message/?token=770fed4ca2aabd20ae9a5dd77471`, {data: this.msgData})
        .then(function (rsp) {
          if (rsp.data.status === 'ok') {
            wx.hideLoading()
            wx.showToast({
              title: 'success',
              icon: 'success',
              duration: 500
            })
            that.msgData.openid = ''
            that.msgData.username = ''
            that.msgData.msg = ''
          } else {
            console.log(rsp)
            wx.hideLoading()
            wx.showToast({
              title: 'fail.',
              icon: 'none',
              duration: 500
            })
          }
        })
        .catch(function (error) {
          console.log(error)
          wx.hideLoading()
        })
          }
        },
        created () {
    // 调用应用实例的方法获取全局数据
    // this.getUserInfo()
        }
      }
</script>
<style scoped>
  .info_wrap{
    width: 100%;
    height: 100%;
    /*display: flex;*/
    /*justify-content: center;*/
    /*align-items: center;*/
  }
  .avatar_wrap{
    width:100%;
  }
  .avatar {
    width: 200rpx;
    height: 200rpx;
    /*border: 0 solid #ff0000;*/
    /*border-radius: 100rpx;*/
    /*background-color: #f10b2e;*/
    /*float: left;*/
  }

  .input{
    border:1px solid #ccc;
  }
  .msg_wrap{
    padding:10rpx 30rpx;
  }
  .confirm_btn{
    margin-top:20rpx;
  }
</style>

