<template>
  <div>
    <p class="card">
      <!--{{image_info.title}}-->
      <navigator :url="'../item/main?p='+image_info.title" class="weui-media-box weui-media-box_appmsg"
                 hover-class="weui-cell_active">
        <div v-for="item in imgIdList" :key="item" class="weui-media-box__hd weui-media-box__hd_in-appmsg">
        <!--<image class="weui-media-box__thumb" :src="icon60" />-->
        <image  class="weui-media-box__thumb" style="width: 200px; height: 200px; background-color: #eeeeee;"  mode="aspectFit"
               :src="'https://photo.tuchong.com/'+userId+'/f/'+item+'.jpg'" />
        </div>
        <div class="weui-media-box__bd weui-media-box__bd_in-appmsg">
        <!--<div class="weui-media-box__title">{{image_info.title}}</div>-->
        <!--<div class="weui-media-box__desc">{{image_info.content}}</div>-->
        </div>
      </navigator>
    </p>
  </div>
</template>

<script>
  export default {
    props: {image_info: Object},
    data () {
      return {
        userId: this.image_info.author_id,
        imgIdList: []
      }
    },
    computed: {
      // https://photo.tuchong.com/ + user_id +/f/ + img_id
      // getImgurl (item) {
      //   const info = item
      //   const userId = info.user_id
      //   const imgId = info.img_id
      //   return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
      // }
    },
    methods: {
      // getImgurl (item) {
      //   const info = item
      //   const userId = info.user_id
      //   const imgId = info.img_id
      //   return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
      // }
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
        i++
      }
    }
  }
</script>

<style>
  .card {
    padding: 10px;
  }
</style>
