<template>
  <div>
    p:{{p}}
    <div v-for="i in imgUrlList" :key="i" v-text="i"></div>
  </div>
</template>

<script>
  // import card from '@/components/card'
  // import Fly from 'flyio/dist/npm/wx'

  // let fly = new Fly()
  export default {
    data () {
      return {
        p: 0,
        imgList: []
      }
    },
    // components: {
    //   card
    // },
    methods: {
      predivImage (e) {
        // console.log(e)
        const that = this
        wx.previewImage({
          current: that.imgList[e.identifier], // 当前显示图片的http链接
          urls: that.imgList // 需要预览的图片http链接列表
        })
      }
    },
    mounted () {
      const p = this.$root.$mp.query.p
      if (!p) {
        console.log('navigateBack')
        return wx.navigateBack()
      }
      this.p = p
      const app = getApp()
      this.imgUrlList = app.globalData.store.imgUrlList
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
  .img_wrap_view {
    /*column-count:2;*/
    column-gap: 0px;
    width: 100%;
  }
</style>
