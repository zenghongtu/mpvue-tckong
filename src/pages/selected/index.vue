<template>
  <div>
    <view class="img_wrap_view" v-for="(item,index) in image_list" :key="item._id">
      <card :image_info="item"></card>
    </view>
    <div v-if="isLoad == 1" class="weui-loadmore">
      <div class="weui-loading"></div>
      <div class="weui-loadmore__tips">正在拼命加载中...</div>
    </div>
    <div v-if="isLoad == 2" class="weui-loadmore weui-loadmore_line">
      <div class="weui-loadmore__tips weui-loadmore__tips_in-line">暂无新鲜相片，晚点再来哦.</div>
    </div>
  </div>
</template>

<script>
  import card from '@/components/card'
  import Fly from 'flyio/dist/npm/wx'

  let fly = new Fly()
  export default {
    data () {
      return {
        image_list: [],
        count: 0,
        isLoad: 0
      }
    },
    components: {
      card
    },
    methods: {
      getSelectedData () {
        const that = this
        const imageList = this.image_list
        let _count = this.count
        const limit = 10
        // if (direction === 'pull') {
        //   _count = 0
        // }
        fly.get('https://api.heta.xyz/v1/selected/?token=770fed4ca2aabd20ae9a5dd77471&limit=' + limit + '&skip=' + _count)
          .then(function (rsp) {
            if (rsp.data.status === 'ok') {
              const _d = rsp.data.data
              // if (direction === 'pull') {
              //   if (imageList === []) {
              //     imageList.push(..._d)
              //     that.count += _d.length
              //   } else {
              //     for (let item of _d) {
              //       if (item._id in imageList) {
              //         break
              //       } else {
              //         imageList.unshift(item)
              //         that.count += 1
              //       }
              //     }
              //   }
              // } else {
              imageList.push(..._d)
              that.count += _d.length
              // }
              that.isLoad = 0
            } else {
              console.log('no new data')
              that.isLoad = 2
            }
          })
          .catch(function (error) {
            console.log(error)
          })
      }
    },

    // async onPullDownRefresh () { // 下拉刷新
    //   this.isLoad = 1
    //   // this.clearState()
    //   await this.getSelectedData('pull')
    //   wx.stopPullDownRefresh()
    // },
    onReachBottom () { // 上拉加载
      this.isLoad = 1
      this.getSelectedData('bottom')
    },

    mounted () {
      this.getSelectedData()
    }

  }
</script>

<style scoped>
  /*.img_wrap_view{*/
    /*!*column-count:2;*!*/
    /*!*column-gap: 0px;*!*/
    /*width: 100%;*/
  /*}*/
</style>
