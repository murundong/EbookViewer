<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div
        class="menu-wrapper"
        v-show="ifTitleAndMenuShow"
        :class="{'hideBoxShadow':ifSettingShow || !ifTitleAndMenuShow}"
      >
        <div class="icon-wrapper">
          <span class="icon-menu icon" @click="showSetting(3)"></span>
        </div>
        <!-- <div class="icon-wrapper">
          <span class="icon-progress icon" @click="showSetting(2)"></span>
        </div> -->
        <div class="icon-wrapper">
          <span class="icon-bright icon" @click="showSetting(1)"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(0)">
          <span class="icon-a icon">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow || ifThemeShow || ifProgressShow">
        <div class="setting-font-size"  v-if="settingTag == 0">
          <div class="preview" :style="{fontSize:fontSizeList[0].fontSize + 'px'}">A</div>
          <div class="select">
            <div class="select-wrapper"
            v-for="(item,index) in fontSizeList"
            :key="index"
            @click="setFontSize(item.fontSize)">
              <div class="line"></div>
              <div class="pointer-wrapper" >
                <div class="pointer" v-show="defaultSize == item.fontSize">
                  <div class="small-pointer"></div>
                </div>
              </div>
              <div class="line"></div>
            </div>
          </div>

          <div
            class="preview"
            :style="{fontSize:fontSizeList[fontSizeList.length-1].fontSize + 'px'}"
          >A</div>
        </div>
        <div class="setting-theme"  v-else-if="settingTag == 1">
         <div class="setting-theme-item" v-for="(item,index) in themeList"
         :key="index"
         @click='setThems(index)'>
            <div class="preview" :style="{background:item.style.body.background}"
            :class="{'no-border':item.style.body.background !== '#fff'}"></div>
            <div class="text"
            :class="{'selected':index === defaultThems}">{{ item.name }}</div>
         </div>
        </div>
        <div class="setting-progress" v-else-if="settingTag == 2">
          <div class="progress-wrapper">
            <input class="progress"  type="range" max="100" min="0" step="1"
            @change="onProgressChange($event.target.value)"
            @input="onProgressInput($event.target.value)"
            :value="progress"
            :disabled="!bookAvailable"
            ref="progress">
            <div class="text-wrapper">
              <span>{{ bookAvailable ? progress + '%' : '加载中……' }}</span>
            </div>
          </div>
        </div>
      </div>
    </transition>
    <content-view
    v-show="ifShowContent"
    :ifShowContent='ifShowContent'
    :navigation="navigation"
    :bookAvailable='bookAvailable'
    @jumpTo = 'jumpTo'
    ></content-view>
    <transition name="fade">
      <div class="content-mask" v-show="ifShowContent" @click="hideContent">

      </div>
    </transition>
  </div>
</template>

<script>
import ContentView from '@/components/ContentView'
export default {
  data() {
    return {
      ifSettingShow: false,
      ifThemeShow: false,
      ifProgressShow:false,
      settingTag: 0,
      progress: 0,
      ifShowContent:false
    }
  },
  components:{
    ContentView
  },
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: Array,
    defaultSize: Number,
    themeList: Array,
    defaultThems: Number,
    bookAvailable: Boolean,
    navigation: Object
  },
  methods: {
    jumpTo(href){
      this.$emit('jumpTo',href)
    },
    hideContent(){
      this.ifShowContent = false
    },
    onProgressInput(progress){
      this.progress = progress
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
    },
    onProgressChange(progress){
      this.$emit('onProgressChange',progress)
    },
    showSetting(tag) {
      this.settingTag = tag
      if(tag === 0){
        this.ifThemeShow = false
        this.ifProgressShow = false
        this.ifSettingShow = !this.ifSettingShow
      }else if(tag === 1){
        this.ifSettingShow = false
        this.ifProgressShow = false
        this.ifThemeShow = !this.ifThemeShow
      }else if(tag === 2){
        this.ifThemeShow = false
        this.ifSettingShow = false
        this.ifProgressShow = !this.ifProgressShow
      }else if(tag === 3){
        this.ifShowContent = true
        this.ifThemeShow = false
        this.ifProgressShow = false
        this.ifSettingShow = false
      }
    },
    setFontSize(fontSize){
      this.$emit('setFontSize', fontSize)
    },
    setThems(index){
      this.$emit('setThems',index)
    },
    hideSetting() {
      this.ifThemeShow = false
      this.ifProgressShow = false
      this.ifSettingShow = false
    }
  }
}
</script>

<style lang='scss' scoped>
@import "@/assets/styles/global.scss";
.menu-bar {
  .setting-wrapper {
    z-index: 101;
    position: absolute;
    left: 0;
    bottom: px2rem(48);
    height: px2rem(60);
    width: 100%;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    background: #fff;
    .setting-font-size {
      display: flex;
      height: 100%;
      flex: 1;
      @include center;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select {
        flex: 1;
        display: flex;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child{
            .line{
              &:first-child{
                border-top: none;
              }
            }
          }
          &:last-child{
            .line{
              &:last-child{
                border-top: none;
              }
            }
          }
          .line {
            flex: 1;
            height: 0;
            border-top: 1px solid #ccc;
          }
          .pointer-wrapper {
            position: relative;
            width: 0px;
            border-left: 1px solid #ccc;
            height: px2rem(7);
            .pointer {
              @include center;
              position: absolute;
              top: px2rem(-8);
              left:px2rem(-10);
              height: px2rem(20);
              width: px2rem(20);
              border-radius: 50%;
              border: px2rem(1) solid #ccc;
              background: #fff;
              box-shadow: 0 px2rem(2) px2rem(2) rgba(0, 0, 0, .15);
              .small-pointer{
                width: px2rem(5);
                height: px2rem(5);
                border-radius: 50%;
                background: #000;
              }
            }
          }
        }
      }
    }
    .setting-theme{
      display: flex;
      height: 100%;
      .setting-theme-item{
        flex: 1;
        display: flex;
        flex-direction: column;
        padding: px2rem(5);
        box-sizing: border-box;
        .preview{
          flex: 1;
          width: 100%;
          border: px2rem(1) solid #ccc;
          box-sizing: border-box;
          &.no-border{
            border: none;
          }
        }
        .text{
          flex: 0 0 px2rem(30);
          font-size: px2rem(16);
          color: #ccc;
          @include center;
          &.selected{
            color:#333;
          }
        }
      }
    }
    .setting-progress{
      position: relative;
      width: 100%;
      height: 100%;
      .progress-wrapper{
        width: 100%;
        height: 100%;
        @include center;
        padding: 0 px2rem(30);
        box-sizing: border-box;
        .progress{
          width: 100%;
          -webkit-appearance: none;
          height: px2rem(2);
          background: -webkit-linear-gradient(#999,#999) no-repeat,#ddd;
          background-size: 0 100%;
          &:focus{
            outline: none;
          }
          &::-webkit-slider-thumb{
            -webkit-appearance: none;
            height: px2rem(20);
            width: px2rem(20);
            border-radius: 50%;
            background: #fff;
            border: px2rem(1) solid #ddd;
            box-shadow: 0 px2rem(2) px2rem(2) 0 rgba(0,0,0,.15);
          }
        }
        .text-wrapper{
          position: absolute;
          left: 0;
          bottom: 0;
          width: 100%;
          font-size: px2rem(14);
          color: #333;
          @include center;
        }
      }
    }
  }
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    z-index: 101;
    display: flex;
    height: px2rem(48);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    &.hideBoxShadow {
      box-shadow: none;
    }
    .icon-wrapper {
      flex: 1;
      @include center;
      .icon-bright,
      .icon-progress {
        font-size: px2rem(25);
      }
    }
  }
  .content-mask{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: 100%;
    background: rgba(51,51,51,.8);
  }
}
</style>
