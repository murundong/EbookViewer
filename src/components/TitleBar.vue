<template>
   <transition name='slide-down'>
    <div class="title-wrapper"
      v-show="ifTitleAndMenuShow">
      <!-- <div class="left">
        <span class="icon-back icon"></span>
      </div> -->
      <div class="title">劫天运小说交流群：
        <span>376289925</span>
        </div>
      <!-- <div class="right">
        <div class="iocn-wrapper">
          <span class="icon-cart icon"></span>
        </div>
         <div class="iocn-wrapper">
          <span class="icon-person icon"></span>
        </div>
         <div class="iocn-wrapper">
          <span class="icon-more icon"></span>
        </div>
      </div> -->
    </div>
    </transition>
</template>

<script>
export default {
    props: {
        ifTitleAndMenuShow: {
            type: Boolean,
            default: false
        }
    }
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global.scss';
 .title-wrapper{
      position: absolute;
      top:0;
      left:0;
      width:100%;
      z-index: 101;
      height: px2rem(48);
      background: #fff;
      box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
      display: flex;
      .title{
        width: 100%;
        @include center;
        font-size: px2rem(16);
        span{
          color: red;
        }
      }
      .left{
        flex: 0 0 px2rem(60);
        @include center;
      }
      .right{
        flex:1;
        display:flex;
        justify-content: flex-end;
        .iocn-wrapper{
          flex:0 0 px2rem(40);
          @include center;
          .icon-cart{
            font-size: px2rem(22);
          }
        }
      }
    }
</style>
