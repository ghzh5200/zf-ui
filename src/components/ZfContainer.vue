<template>
  <div
    class="zf-container"
    v-bind="$attrs"
    v-on="$listeners"
    :style="`height:${height}px`"
    :zf-scroll-x="scrollX"
    :zf-scroll-y="openScrollY"
    @mousewheel="handlerScroll"
  >
    <div v-if="loading" class="zf-loading-wraper"><i class="zf-icon-loading"></i></div>
    <div
      class="zf-container-content"
      ref="zfContainerContent"
      :style="{transform: `translateY(${containerContentScrollTop}px)`}"
      :zf-close-select="closeSelect"
      :zf-transition="!closeTransition"
    >
      <slot></slot>
    </div>
    <div class="zf-scroll-y" v-if="openScrollY" @mousedown="handlerMousedown">
      <i :style="{height: height / containerContentHeight * 100 + '%', transform: `translateY(${scrollScrollTop}px)`}" :zf-transition="!closeTransition"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "zf-container",
  inheritAttrs: false,
  data() {
    return {
      containerContentScrollTop: -this.scrollTop,
      scrollScrollTop: 0,
      containerContentHeight: 0,
      closeSelect: false, //禁用内容区拖拽时选择文本（css控制）
      isMousedown: false, //是否在滚动条上按下鼠标
      mousedownOffsetY: 0, //按下scroll时鼠标的位置
      mousedownScrollTop: 0, //按下scroll时scroll的top值
      isTouch: false,
      closeTransition: false,
      openScrollY: this.scrollY,//是否允许滚动
    };
  },
  props: {
    scrollX: {
      type: Boolean,
      default: false
    },
    scrollY: {
      type: Boolean,
      default: false
    },
    height: [Number, Boolean],
    peak: {
      type: [Number, Boolean],
      default: 50
    },
    sloe: {
      type: [Number, Boolean],
      default: 50
    },
    scrollTop: {
      type: [Number, Boolean],
      default: 0
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  mounted() {
    this.init();
    window.addEventListener("mousemove", this.handlerMousemove, true);
    window.addEventListener("mouseup", this.handlerMouseup, true);
    const container = document.querySelectorAll('.zf-container-content');
    const watchDom = new MutationObserver(this.init);
    container.forEach(item => {
      watchDom.observe(item, { childList: true, subtree: true, attributes: true })
    })
  },
  methods: {
    init(){
      this.containerContentHeight = this.$refs.zfContainerContent.offsetHeight;
      if(this.scrollTop != 0){
        this.containerContentScrollTop = -this.scrollTop;
        this.scrollScrollTop = (-this.containerContentScrollTop / this.containerContentHeight) * this.height;
      }
      if(this.containerContentHeight <= this.height){
        this.containerContentScrollTop = 0;
        this.scrollScrollTop = 0;
        this.openScrollY = false;
      }else{
        if(this.openScrollY) return;
        this.openScrollY = true;
      }
      console.log('初始化成功')
    },
    handlerScroll(e) {
      if(this.containerContentHeight <= this.height || !this.openScrollY) return;
      this.containerContentScrollTop += e.wheelDeltaY;
      this.scrollScrollTop = (-this.containerContentScrollTop / this.containerContentHeight) * this.height;
      this.restrictScroll(e);
      this.touchTopOrBottom();
      return false;
    },
    handlerMousemove(e) {
      if (this.isMousedown) {
        //当滑动滚动条时计算内容ScrollTop和滚动条的ScrollTop
        this.scrollScrollTop = this.mousedownScrollTop + e.pageY - this.mousedownOffsetY;
        this.containerContentScrollTop = -((this.scrollScrollTop / this.height) * this.containerContentHeight);
        this.restrictScroll(e);
        this.touchTopOrBottom();
      }
    },
    handlerSelectstart(close){
      //是否阻止文本选择
      document.onselectstart = () => {
        return close;
      }
    },
    restrictScroll(e){
      //限制滚动或拖动范围
      if (this.containerContentScrollTop <= (-this.containerContentHeight) + this.height) {
        this.containerContentScrollTop = (-this.containerContentHeight) + this.height;
        this.scrollScrollTop = (-this.containerContentScrollTop / this.containerContentHeight) * this.height;
        return;
      }
      if (this.containerContentScrollTop >= 0) {
        this.containerContentScrollTop = 0;
        this.scrollScrollTop = 0;
        return;
      }
      e.preventDefault(); //在可滚动范围内阻止冒泡
    },
    touchTopOrBottom(){
      //监听触底触顶
      if(this.containerContentHeight + this.containerContentScrollTop - this.height <= (this.sole || 0)){
        if(this.isTouch) return;
        this.$emit('touchmonitor',{touch: 'bottom'});
        this.isTouch = true;
        return;
      }
      if(this.containerContentHeight + this.containerContentScrollTop >= this.containerContentHeight - (this.peak || 0)){
        if(this.isTouch) return;
        this.$emit('touchmonitor',{touch: 'top'});
        this.isTouch = true;
        return;
      }
      this.isTouch = false;
    },
    handlerMousedown(e) {
      this.isMousedown = true;
      this.mousedownOffsetY = e.pageY;
      this.mousedownScrollTop = this.scrollScrollTop;
      this.closeSelect = true;
      this.closeTransition = true;
      this.handlerSelectstart(false);
    },
    handlerMouseup() {
      this.isMousedown = false;
      this.closeSelect = false;
      this.closeTransition = false;
      this.handlerSelectstart(true);
    },
  },
  watch:{
    height(){
      //避免出现容器高度不够也显示滚动条
      this.init();
    }
  }
};
</script>
