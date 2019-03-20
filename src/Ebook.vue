<template>
  <div class="ebook">
    <div class="title-wrapper">
      <div class="left">
        <span class="icon-back icon"></span>
      </div>
      <div class="right">
        <div class="iocn-wrapper">
          <span class="icon-cart icon"></span>
        </div>
         <div class="iocn-wrapper">
          <span class="icon-person icon"></span>
        </div>
         <div class="iocn-wrapper">
          <span class="icon-more icon"></span>
        </div>
      </div>
    </div>
    <div class="read-wrapper">
      <div id="read">
      </div>
        <div class="mask">
          <div class="left" @click="prevPage"></div>
          <div class="center"></div>
          <div class="right" @click="nextPage"></div>
        </div>
    </div>
  </div>
</template>
<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  methods: {
    showEpub () {
      this.book = new Epub(DOWNLOAD_URL)
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      this.rendition.display()
    },
    prevPage(){
      if(this.rendition){
        this.rendition.prev();
      }
    },
    nextPage(){
      if(this.rendition){
        this.rendition.next();
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
