<template>
  <div class="recommend" ref="recommend">
    <scroll ref='scroll' class="recommend-content" v-bind:data="getDisclist">
      <div>
        <div v-if="recommends.length" class="slider-wrapper">
          <slider>
            <div v-for="item in recommends" class='slider-item'>
              <a :href="item.linkUrl">
                <img @load="loadImage" :src="item.picUrl" alt="">
              </a>
            </div>
          </slider>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">
            热门歌单搜索
          </h1>
          <ul>
            <li v-for="item in getDisclist" class="item">
              <div class="icon">
                <img :src="item.imgurl"  width="60" height="60" alt="">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  /* eslint-disable no-new */
  import Slider from 'base/slider/slider'
  import Scroll from 'base/scroll/scroll'
  import {getRecommend} from 'api/recommend'
  import {getDisclist} from 'api/recommend'
  import {ERR_OK} from 'api/config'

  export default {
    data(){
      return{
        recommends: [],
        getDisclist:[]
      }
    },
    created() {
      
      this._getDisclist()
      setTimeout(()=>{
        this._getRecommend()
      },2000)
    },
    methods: {
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            // console.log(res.data.slider) 
            this.recommends = res.data.slider
          }
        })
      },
      _getDisclist(){
        getDisclist().then((res)=>{
          if(res.code === ERR_OK){
            console.log(res.data) 
            this.getDisclist = res.data.list
          }
        })
      },
      loadImage(){
        if(!this.checkLoaded){
          this.$refs.scroll.refresh()
          this.checkLoaded = true
        }
      }
    },
    components: {
      Slider,
      Scroll
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"

  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>