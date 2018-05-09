<template>
  <div class="search_wrap">
    <view class="search_header">
      <input class="search_input" v-model="q" :placeholder="subtitle" placeholder-class="search_placeholder"
             focus="true"/>
      <button class="search_btn" @click="handleSearch">Search</button>
    </view>
    <div v-show="show_result" class="search_body">
      <view class="img_wrap_view" v-for="(item,index) in image_list" :key="item._id">
      <card :image_info="item"></card>
    </view>
      <!--<card :image_info="image_list[0]"></card>-->

      <div v-if="isLoad == 1" class="weui-loadmore">
        <div class="weui-loading"></div>
        <div class="weui-loadmore__tips">正在拼命加载中...</div>
      </div>
      <div v-if="isLoad == 2" class="weui-loadmore weui-loadmore_line">
        <div class="weui-loadmore__tips weui-loadmore__tips_in-line">暂无新鲜相片，晚点再来哦.</div>
      </div>
    </div>
  </div>
</template>

<script>
  import card from '@/components/card'
  import Fly from 'flyio/dist/npm/wx'

  let fly = new Fly()
  export default {
    components: {
      card
    },
    data () {
      return {
        q: '',
        subtitle: '请在此输入搜索内容',
        show_result: false,
        image_list: [],
        page: 1,
        isLoad: 0,
        posts: 0,
        count: 0,
        isSelect: 0
      }
    },
    methods: {
      getSearchData: function (src = '') {
        const that = this
        const imageList = this.image_list
        fly.get(`https://api.heta.xyz/v1/search/?s=${this.q}&page=${this.page}&token=770fed4ca2aabd20ae9a5dd77471`)
          .then(function (rsp) {
            console.log(rsp)
            if (rsp.data.status === 'ok') {
              const data = rsp.data.data
              const _d = data.post_list
              that.posts = data.posts
              const newList = []
              if (that.isSelect) {
                imageList.pop()
                that.isSelect = 0
              }
              for (let item of _d) {
                item.crt_time = item.published_at
                item._id = item.post_id
                item.author = {name: item.site.name, icon: item.site.icon}
                delete item.published_at
                delete item.post_id
                delete item.site
                newList.push(item)
              }
              imageList.push(...newList)
              if (src !== 'button') {
                that.page += 1
              }
              that.show_result = true
              that.isLoad = 0
            } else {
              console.log('no new data')
              that.isLoad = 2
            }
          })
          .catch(function (error) {
            console.log(error)
          })
      },
      getSelectedData () {
        const that = this
        const imageList = this.image_list
        fly.get('https://api.heta.xyz/v1/wallpaper/?token=770fed4ca2aabd20ae9a5dd77471')
          .then(function (rsp) {
            if (rsp.data.status === 'ok') {
              const _d = rsp.data.data
              imageList.pop()
              imageList.push(..._d)
              // console.log(imageList)
              that.isLoad = 0
              that.show_result = true
              that.isSelect = 1
            } else {
              console.log('no new data')
              that.isLoad = 2
            }
          })
          .catch(function (error) {
            console.log(error)
          })
      },

      resetData () {
        this.image_list = []
      },

      handleSearch () {
        this.resetData()
        this.getSearchData('button')
      }
    },
    async onPullDownRefresh () { // 下拉刷新
      this.isLoad = 1
      // this.clearState()
      await this.getSelectedData()
      wx.stopPullDownRefresh()
    },
    // onReachBottom () { // 上拉加载
    //   this.isLoad = 1
    //   this.getSearchData()
    // }

    mounted: function () {
      this.getSelectedData()
    }
  }
</script>

<style>
  .search_wrap {
    width: 100%;
    height: 100vh;
  }

  .search_header {
    padding: 15rpx 30rpx;
  }

  .search_input {
    width: 500rpx;
    height: 60rpx;
    /*border:2rpx;*/
    border: 2rpx solid #ccc;
    display: inline-block;
    vertical-align: middle;
  }

  .search_btn {
    width: 135rpx;
    height: 66rpx;
    display: inline-block;
    font-size: 30rpx;
    line-height: 66rpx;
    vertical-align: middle;
    margin-left: 10rpx;
  }

  .search_body {
    width: 100%;
    height: 100%;
  }

  .img_wrap_view {

  }

</style>
