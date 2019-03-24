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
    :defaultThems='defaultThems'
    :themeList='themeList'
    @setThems='setThems'
    :bookAvailable = 'bookAvailable'
    @onProgressChange = 'onProgressChange'
    @jumpTo = 'jumpTo'
    :navigation = 'navigation'
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
      defaultThems:0,
      bookAvailable:false,
      fontSizeList: [
        {'fontSize': 12},
        {'fontSize': 14},
        {'fontSize': 16},
        {'fontSize': 18},
        {'fontSize': 20},
        {'fontSize': 22}
      ],
      themeList:[
        {
          name:'default',
          style:{ body:{color:'#000',background:'#fff'} }
        },
        {
          name:'eye',
          style:{ body:{color:'#000',background:'#ceeaba'} }
        },
        {
          name:'night',
          style:{ body:{color:'#fff',background:'#000'} }
        },
        {
          name:'gold',
          style:{ body:{color:'#000',background:'rgb(243,236,226)'} }
        }
      ],
      navigation: {}
    }
  },
  components: {
    TitleBar, MenuBar
  },
  methods: {
    getDefault(){
      if(localStorage.getItem('defaultSize')){
        this.defaultSize = parseInt(localStorage.getItem('defaultSize'))
        // this.setFontSize(this.defaultSize)
      }
      if(localStorage.getItem('defaultThems')){
        this.defaultThems = parseInt(localStorage.getItem('defaultThems'))
        // this.setThems(this.defaultThems)
      }
    },
    jumpTo(href){
      this.rendition.display(href)
      this.hideMenuAndTitle()
    },
    hideMenuAndTitle(){
      this.ifTitleAndMenuShow = false
      this.$refs.menuBar.hideContent()
      this.$refs.menuBar.hideSetting()
    },
    onProgressChange(progresss){
      const percentage = progresss / 100
      const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(location)
    },
    toggleTitleAndMenu(){
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if(!this.ifTitleAndMenuShow){
        this.$refs.menuBar.ifSettingShow = false
        this.$refs.menuBar.ifThemeShow = false
        this.$refs.menuBar.ifProgressShow = false
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
      this.setThems(this.defaultThems)
      this.registerThems()
      this.book.ready.then(() => {
        this.navigation = this.book.navigation
        return this.book.locations.generate()
      }).then(res => {
        this.locations = this.book.locations
        this.bookAvailable = true
      })
    },
    setThems(index){
      this.thems.select(this.themeList[index].name)
      this.defaultThems = index
      localStorage.setItem('defaultThems',index)
    },
    registerThems(){
      this.themeList.forEach(thems => {
        this.thems.register(thems.name,thems.style)
      })
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
        localStorage.setItem('defaultSize',fontSize)
        this.thems.fontSize(this.defaultSize + 'px')
      }
    }
  },
  mounted () {
    this.getDefault()
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
