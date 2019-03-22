<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div
        class="menu-wrapper"
        v-show="ifTitleAndMenuShow"
        :class="{'hideBoxShadow':ifSettingShow || !ifTitleAndMenuShow}"
      >
        <div class="icon-wrapper">
          <span class="icon-menu icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-progress icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-bright icon"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting">
          <span class="icon-a icon">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow">
        <div class="setting-font-size">
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
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ifSettingShow: false
    }
  },
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: Array,
    defaultSize: Number
  },
  methods: {
    showSetting() {
      this.ifSettingShow = !this.ifSettingShow
    },
    setFontSize(fontSize){
      this.$emit('setFontSize', fontSize)
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
}
</style>
