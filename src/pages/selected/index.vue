<template>
  <div>
    <view class="img_wrap_view" v-for="(item,index) in image_list" :key="item._id">
      <card :image_info="item"></card>
    </view>
    <div v-if="isLoad == 1" class="weui-loadmore">
      <div class="weui-loading"></div>
      <div class="weui-loadmore__tips">正在加载</div>
    </div>
    <div v-if="isLoad == 2" class="weui-loadmore weui-loadmore_line">
      <div class="weui-loadmore__tips weui-loadmore__tips_in-line">暂无数据</div>
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
        right_height: 0,
        left_height: 0,
        count: 0,
        isLoad: 0
      }
    },
    components: {
      card
    },
    methods: {
      getSelectedData (direction) {
        const that = this
        const imageList = this.image_list
        let _count = this.count
        const limit = 10
        if (direction === 'pull') {
          _count = 0
        }
        fly.get('http://127.0.0.1:9000/api/v1/selected/?token=770fed4ca2aabd20ae9a5dd77471&limit=' + limit + '&skip=' + _count)
          .then(function (rsp) {
            if (rsp.data.status === 'ok') {
              const _d = rsp.data.data
              if (direction === 'pull') {
                if (imageList === []) {
                  imageList.push(..._d)
                } else {
                  // todo?
                  // const len = _d.length
                  // let f = 0
                  // for (let i=0;i<len;i++){
                  //   if (_d[i]._id )
                  // }
                  imageList.unshift(..._d)
                }
              } else {
                imageList.push(..._d)
              }
              that.isLoad = 0
              that.count += _d.length
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

    async onPullDownRefresh () { // 下拉刷新
      this.isLoad = 1
      // this.clearState()
      await this.getSelectedData('pull')
      wx.stopPullDownRefresh()
    },
    onReachBottom () { // 上拉加载
      this.isLoad = 1
      this.getSelectedData('bottom')
    },

    mounted () {
      // 调用应用实例的方法获取全局数据
      this.getSelectedData()
    }

  }
</script>

<style scoped>
  .img_wrap_view{
    /*column-count:2;*/
    column-gap: 0px;
    width: 100%;
  }
</style>
