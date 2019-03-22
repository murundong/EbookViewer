<template>
  <div class="ebook">
    <title-bar :ifTitleAndMenuShow='ifTitleAndMenuShow'></title-bar>
    <div class="read-wrapper">
      <div id="read">
      </div>
        <div class="mask">
          <div class="left" @click="prevPage"></div>
          <div class="center" @click="toggleTitleAndMenu"></div>
          <div class="right" @click="nextPage"></div>
        </div>
    </div>
    <menu-bar 
    :ifTitleAndMenuShow='ifTitleAndMenuShow'
    :fontSizeList='fontSizeList'
    :defaultSize='defaultSize'
    @setFontSize= 'setFontSize'
     ref="menuBar"></menu-bar>
  </div>
</template>
<script>
import Epub from 'epubjs'
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  data(){
    return{
      ifTitleAndMenuShow: false,
      defaultSize: 16,
      fontSizeList: [
        {'fontSize': 12},
        {'fontSize': 14},
        {'fontSize': 16},
        {'fontSize': 18},
        {'fontSize': 20},
        {'fontSize': 22}
      ]
    }
  },
  components: {
    TitleBar, MenuBar
  },
  methods: {
    toggleTitleAndMenu(){
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if(!this.ifTitleAndMenuShow){
        this.$refs.menuBar.ifSettingShow = false
      }
    },
    showEpub () {
      this.book = new Epub(DOWNLOAD_URL)
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      this.rendition.display()
      this.thems = this.rendition.themes
      this.setFontSize(this.defaultSize)
    },
    prevPage(){
      if(this.rendition){
        this.rendition.prev()
      }
    },
    nextPage(){
      if(this.rendition){
        this.rendition.next()
      }
    },
    setFontSize(fontSize) {
      this.defaultSize = fontSize
      if(this.thems){
        this.thems.fontSize(this.defaultSize + 'px')
      }
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>
<style lang='scss' scoped>
  @import "assets/styles/global";
  .ebook {
    position: relative;
    .read-wrapper{
      .mask{
        position: absolute;
        top:0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 100;
        display:flex;
        .left{
          flex: 0 0 px2rem(100);
        }
        .center{
          flex: 1;
        }
        .right{
          flex: 0 0 px2rem(100);
        }

      }
    }
  }
</style>
