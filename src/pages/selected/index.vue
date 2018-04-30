<template>
  <div>
    <view v-for="(item,key) in image_list" :key="key">{{item.title}}</view>
  </div>
</template>

<script>
  // import card from '@/components/card'
  import Fly from 'flyio/dist/npm/wx'

  let fly = new Fly()
  export default {
    data () {
      return {
        image_list: [],
        count: 0
      }
    },
    //
    // components: {
    //   card
    // },

    methods: {
      getSelectedData (direction = 'pull') {
        const that = this
        const imageList = this.image_list
        let _count = this.count
        if (direction === 'pull') {
          _count = 0
        }
        fly.get('http://127.0.0.1:9000/api/v1/selected/?token=770fed4ca2aabd20ae9a5dd77471&limit=30&skip=' + _count)
          .then(function (rsp) {
            if (rsp.data.status === 'ok') {
              const _d = rsp.data.data
              if (direction === 'bottom') {
                // console.log(_d[0]._id)
                imageList.push(..._d)
              } else {
                // console.log(imageList[0])
                // console.log(imageList[0]._id)
                if (imageList[0]) {
                // if (_d[0]._id !== imageList[0]._id) {
                  // const str = JSON.stringify(imageList)
                  // console.log(imageList)

                  // if (str.search('' + _d[0]._id)) {
                  imageList.unshift(..._d)
                  console.log('change')
                  // }
                // }
                } else {
                  imageList.push(..._d)
                  console.log('push ')
                }
                console.log(imageList)
              }
              that.count += 10
            } else {
              console.log('no data')
            }
          })
          .catch(function (error) {
            console.log(error)
          })
      }
    },

    async onPullDownRefresh () { // 下拉刷新
      // this.clearState()
      await this.getSelectedData()
      wx.stopPullDownRefresh()
    },
    onReachBottom () { // 上拉加载
      this.getSelectedData('bottom')
    },

    created () {
      // 调用应用实例的方法获取全局数据
      this.getSelectedData()
    }

  }
</script>

<style scoped>

</style>
