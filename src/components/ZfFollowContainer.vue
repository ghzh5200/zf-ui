<template>
  <div class="zf-follow-container-wrap">
    <div class="zf-user-container-content" :style="{width: contentBoxWidth}"><slot name="content-container"></slot></div>
    <div :zf-relative="isBottom" class="zf-follow-container" ref="ZfFlollowContainer" :style="{width: followBoxWidth}" :zf-order="reverseFloat">
      <div :zf-fixed="isFixed" :zf-absolute="isBottom" class="zf-follow-container-content" :style="{left:leftNumber+'px',transform:'translateY('+translateNumber+'px)'}">
        <slot name="follow-container"></slot>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: "zf-follow-container",
    inheritAttrs: false,
    data() {
      return {
        windowScrollYPast: 0, //窗口滚动条高度(轮动的上一次)，用于判断窗口是上轮动还是下轮动
        leftNumber: 0, //右侧浮窗当想对文档定位时left距离
        isFixed: false,
        isBottom: false,
        translateNumber: 0, //当浮窗处于fixed状态时的上下滑动值
        lockTranslateNumberTop: 0,
        lockTranslateNumberDown: 0,
        maxTranslateNumber: 0, //当浮窗处于fixed状态时的最大滑动值
        lockWinTopIsGoTop: 0, //当浮窗处于fixed状态时，上滑动时刻的window.scrollY的值
        lockWinTopIsGoDown: 0, //当浮窗处于fixed状态时，下滑动时刻的window.scrollY的值
      }
    },
    props:{
      stopIn: {
        type: [Number],
        default: 0
      },
      contentBoxWidth: {
        type: [String, Boolean],
        default: '70%'
      },
      followBoxWidth: {
        type: [String, Boolean],
        default: '30%'
      },
      reverseFloat: {
        type: Boolean,
        default: false
      },
    },
    mounted(){
      window.addEventListener('scroll', this.handlerScroll, true);
      window.addEventListener('resize', this.domResize);
    },
    methods:{
      //浮窗滚动跟随
      handlerScroll() {
        //当前函数为何很少用else？因为if判断中都带有唯一条件，值大多只会被赋值一次，而不是重复滚动赋值
        if (!this.$refs.ZfFlollowContainer) return;
        const containerTop = this.$refs.ZfFlollowContainer.offsetTop;
        const containerHeight = this.$refs.ZfFlollowContainer.offsetHeight;
        const containerLeft = this.$refs.ZfFlollowContainer.offsetLeft;
        const containerChildHeight = this.$refs.ZfFlollowContainer.children[0].offsetHeight;
        const winTop = window.scrollY;
        const winHidth = window.innerHeight;
        const containerScrollCurrentTop = (containerTop + containerChildHeight) - (winTop + winHidth);// 计算container底部与window底部差
        //获取当containerChild处于fixed状态时候的最大上下滑动值
        if (!this.maxTranslateNumber) {
          this.maxTranslateNumber = containerChildHeight - winHidth + this.stopIn; // stopIn为top抛出预算高度
        }
        //当container未触底的情况下，检测是否让containerChild开启fixed定位，并设定left值
        if (!this.isBottom) {
          //当浮窗底部到达可视区底部时开启fixed定位
          if (containerScrollCurrentTop <= 0 && !this.isFixed) {
            this.leftNumber = containerLeft;
            this.isFixed = true;
          }
          //当浮窗顶部到达可视区顶部时关闭fixed定位
          if (containerScrollCurrentTop > this.translateNumber && this.isFixed) {
            this.isFixed = false;
            this.translateNumber = 0;//同时清0浮窗translate值
          }
        }
        //检测containerChild是否触底
        if (winTop + winHidth >= containerTop + containerHeight && !this.isBottom) {
          this.isBottom = true;
          this.isFixed = false;
          this.leftNumber = 0;
          this.translateNumber = 0;//同时清0浮窗translate值
        }
        //检测containerChild是否不再触底
        if (winTop + winHidth < containerTop + containerHeight && this.isBottom) {
          this.isBottom = false;
          this.isFixed = true;
          this.leftNumber = containerLeft;
        }
        if(containerChildHeight <= winHidth) return;
        //计算滚动条向上还是向下
        if (this.isFixed) {
          if (this.scrollIsGoTop()) {
            this.lockWinTopIsGoDown = 0;
            if (!this.lockWinTopIsGoTop) {
              console.log('向上')
              this.lockWinTopIsGoTop = winTop;
            }
            this.lockTranslateNumberDown = 0;
            if (!this.lockTranslateNumberTop) {
              this.lockTranslateNumberTop = this.translateNumber || 0;
            }
            if (this.translateNumber < this.maxTranslateNumber) {
              this.translateNumber = this.lockTranslateNumberTop + this.lockWinTopIsGoTop - winTop;
            } else {
              if (this.translateNumber != this.maxTranslateNumber) {
                this.translateNumber = this.maxTranslateNumber;
              }
            }
          } else {
            this.lockWinTopIsGoTop = 0;
            if (!this.lockWinTopIsGoDown) {
              console.log('向下')
              this.lockWinTopIsGoDown = winTop;
            }
            this.lockTranslateNumberTop = 0;
            if (!this.lockTranslateNumberDown) {
              this.lockTranslateNumberDown = this.translateNumber || 0;
            }
            if (this.translateNumber > 0) {
              this.translateNumber = this.lockTranslateNumberDown + this.lockWinTopIsGoDown - winTop;
            } else {
              if (this.translateNumber != 0) {
                this.translateNumber = 0;
              }
            }
          }
        }
      },
      // 滚动条向上向下监听
      scrollIsGoTop() {
        if (window.scrollY > this.windowScrollYPast) {
          this.windowScrollYPast = window.scrollY;
          return false;
        }
        this.windowScrollYPast = window.scrollY;
        return true;
      },
      //窗口变化监听
      domResize() {
        if (!this.isBottom) {
          this.leftNumber = this.$refs.ZfFlollowContainer.offsetLeft;
        }
      }
    }
  }
</script>