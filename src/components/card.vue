<template>
  <div>
    <p class="card">
      <!--{{image_info.title}}-->
      <navigator :url="'../item/main?p='+image_info.title" class="weui-media-box weui-media-box_appmsg" hover-class="weui-cell_active">
        <div class="weui-media-box__hd weui-media-box__hd_in-appmsg">
          <!--<image class="weui-media-box__thumb" :src="icon60" />-->
          <image class="weui-media-box__thumb" :src="getImgurl" />
        </div>
        <div class="weui-media-box__bd weui-media-box__bd_in-appmsg">
          <div class="weui-media-box__title">标题一</div>
          <div class="weui-media-box__desc">由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。</div>
        </div>
      </navigator>
    </p>
  </div>
</template>

<script>
export default {
  props: {image_info: Object},
  computed: {
    // https://photo.tuchong.com/ + user_id +/f/ + img_id
    getImgurl () {
      const info = this.image_info.images[0]
      const userId = info.user_id
      const imgId = info.img_id
      return `https://photo.tuchong.com/${userId}/f/${imgId}.jpg`
    }
  }
}
</script>

<style>
.card {
  padding: 10px;
}
</style>
