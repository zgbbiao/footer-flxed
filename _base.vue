<!--
 * @Author: your name
 * @Date: 2020-04-18 19:56:38
 * @LastEditTime: 2020-07-28 19:32:08
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \whyqh5\src\components\footer-flxed\_base.vue
 -->
<template>
  <div :class="b('wrapper', ['wrapper'])">
    <div v-show="isShowFilex">
      <div
        :class="b('wrapper', ['fixed-footer'])"
        :style="{
          height: fixedHeight + 'px',
        }"
      ></div>
      <div
        :class="
          b('wrapper', [
            'fixed',
            isPC ? 'fixed--pc-border-1px-top' : 'fixed--model-border-1px-top',
          ])
        "
        ref="wrapperFixed"
      >
        <slot></slot>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'CommonFooterFixed',
  props: {
    isflxed: {
      type: Boolean,
      default: false,
    },
    isTopBor: {
      type: [String, Boolean],
      default: true,
    },
  },
  data() {
    return {
      scrollTop: 0,
      fixedHeight: 0,
      screenHeight: 0,
      isPC: false
    };
  },
  computed: {
    isShowFilex() {
      return this.isflxed ? this.isflxed : this.scrollTop > this.screenHeight;
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.scrollShowFilexBtn();
    });
  },
  methods: {
    scrollShowFilexBtn() {
      const that = this;
      if (this.isShowFilex) {
        const footerFixDom = document.querySelector(
            '.CommonFooterFixed__wrapper--fixed'
          )
          that.fixedHeight = footerFixDom.clientHeight;
      }
      window.addEventListener('scroll', function() {
        const screenHeight = screen.height;
        that.screenHeight = screenHeight;
        // console.log('screenHeight', screenHeight);
        const scrollTop = that.getScrollTop();
        // console.log('scrollTop', scrollTop);
        that.scrollTop = scrollTop;
        if (scrollTop > screenHeight) {
          const footerFixDom = document.querySelector(
            '.CommonFooterFixed__wrapper--fixed'
          );
          if (footerFixDom && that.fixedHeight > 0) {
          } else {
            that.fixedHeight = footerFixDom.clientHeight;
            // console.log(footerFixDom.style.width.replace('px', ''));
          }
        }
      });
    },
    getScrollTop() {
      var scrollPos;
      if (window.pageYOffset) {
        scrollPos = window.pageYOffset;
      } else if (document.compatMode && document.compatMode != 'BackCompat') {
        scrollPos = document.documentElement.scrollTop;
      } else if (document.body) {
        scrollPos = document.body.scrollTop;
      }
      return scrollPos;
    },
  },
};
</script>
<style lang="less">
.CommonFooterFixed {
  &__wrapper {
    &--wrapper {
    }
    &--fixed {
      position: fixed;
      bottom: 0;
      width: 100%;
      display: block;
      z-index: 100;
      // padding: 10px 15px;
      background: #fff;
      // max-width: 375px;
      &--model-border-1px-top:before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        border-top: 1px solid #e5e5e5;
        transform: scaleY(0.5);
        transform-origin: left top;
      }
      &--pc-border-1px-top:before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        border-top: 1px solid #e5e5e5;
        transform-origin: left top;
      }
    }
  }
}
</style>
