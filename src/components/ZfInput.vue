<template>
  <div
    :class="[type == 'textarea' ? 'zf-textarea-wrap' : 'zf-input-wrap']"
    :disabled="disabled"
    ref="zfInputWrap"
  >
    <template v-if="type == 'textarea'">
      <textarea
        class="zf-textarea"
        ref="zfTextarea"
        :style="{minHeight: zfTextareaMinHeight}"
        :value="inputValue"
        :maxlength="maxlength"
        :disabled="disabled"
        :zf-autosize="autosize"
        :zf-close-resize="closeResize"
        v-bind="$attrs"
        v-on="inputListeners"
        @compositionstart="compositionstart"
        @compositionend="compositionend"
      ></textarea>
      <i
        class="zf-maxlength"
        v-if="showLength && maxlength && type=='textarea'"
      >{{textLength + '/' + maxlength}}</i>
    </template>
    <template v-else>
      <i class="zf-icon" v-if="icon" :class="icon"></i>
      <input
        class="zf-input"
        ref="zfInput"
        :value="inputValue"
        :maxlength="maxlength"
        :type="typeValue"
        :disabled="disabled"
        v-bind="$attrs"
        v-on="inputListeners"
        @compositionstart="compositionstart"
        @compositionend="compositionend"
      />
      <slot></slot>
      <div class="zf-wrap">
        <i
          class="zf-icon-clear"
          v-if="showClear && clearable"
          @click="clearInput"
          zf-cursor-pointer
        ></i>
        <i
          :class="eyesIcon"
          v-if="showEye && type == 'password'"
          @mousedown="showPwd('zf-icon-openEyes','text')"
          @mouseup="showPwd('zf-icon-closeEyes','password')"
          zf-cursor-pointer
        ></i>
        <i v-if="showLength && maxlength && type=='text'">{{textLength + '/' + maxlength}}</i>
      </div>
    </template>
  </div>
</template>
<script>
export default {
  name: "zf-input",
  inheritAttrs: false,
  data() {
    return {
      showClear: false,
      typeValue: this.type,
      eyesIcon: "zf-icon-closeEyes",
      inputValue: this.value ? String(this.value).slice(0, this.maxlength) : "",
      textLength: this.value
        ? String(this.value).length > this.maxlength
          ? this.maxlength
          : String(this.value).length
        : 0,
      inputStart: false, //输入法输入开始和结束
      zfTextareaMinHeight: false,
      zfInputWrapInitialHeight: 0 //外框包裹曾的初始高度
    };
  },
  mounted() {
    this.zfTextareaMinHeight = this.$refs.zfInputWrap.offsetHeight + "px";
    this.zfInputWrapInitialHeight = this.$refs.zfInputWrap.offsetHeight;
  },
  props: {
    value: [String, Number, Boolean],
    type: [String, Boolean],
    icon: [String, Boolean],
    clearable: {
      type: Boolean,
      default: false
    },
    maxlength: String,
    showLength: {
      type: Boolean,
      default: false
    },
    showEye: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    autosize: {
      type: Boolean,
      default: false
    },
    closeResize: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    showPwd(eyesIcon, typeValue) {
      this.eyesIcon = eyesIcon;
      this.typeValue = typeValue;
    },
    clearInput() {
      this.$emit("input", "");
      this.inputValue = "";
      this.showClear = false;
      this.textLength = 0;
      this.$refs.zfInput.focus();
    },
    compositionstart() {
      //输入法输入开始事件
      this.inputStart = true;
    },
    compositionend(e) {
      //输入法输入结束事件
      this.inputStart = false;
      this.textLength = e.target.value.length;
    },
    /**
     *input事件说明
     *当父组件未绑定任何事件的时候value的值为空或一个固定值，而为了满足v-model正常使用，所以子组件的value必须等于父组件的value，此时如果父组件没有绑定任何事件（v-model），
     *则子组件是无法改变自己的value，因为value永远为空或一个和固定值($emit为空)
     *为了能改变自己的value值，要么给父组件绑定事件(v-model通过$emit改变)要么用一个三方变量inputValue来储存父级传过来的value，
     *当父组件未绑定事件(v-model)时子组件只需要改变inputValue即可改变自己的value值，就不会受父级传过来的value所影响。
     *而当父组件绑定了事件(v-model)时子组件只需要this.$emit("input", event.target.value);即可
     *当前功能应用场景为：当父组件未传事件的情况下，input还能正常绑定事件'@input'并改变自己的value
     */
    handlerInput(e) {
      this.inputValue = e.target.value;
      if (!this.inputStart) {
        this.textLength = e.target.value.length;
      }
      if (e.target.value.length > 0) {
        this.showClear = true;
      } else {
        this.showClear = false;
      }
      if (this.autosize) {
        const [textareaMinHeight, wrapInitialHeight] = [
          parseInt(this.zfTextareaMinHeight),
          parseInt(this.zfInputWrapInitialHeight)
        ];
        console.log(
          e,
          parseInt(getComputedStyle(e.target,null).lineHeight + "px"),
          this.zfTextareaMinHeight,
          this.$refs.zfTextarea.scrollHeight
        );
        if ( textareaMinHeight != this.$refs.zfTextarea.scrollHeight && wrapInitialHeight <= this.$refs.zfTextarea.scrollHeight ) {
          if(this.$refs.zfTextarea.scrollHeight < textareaMinHeight){
            this.zfTextareaMinHeight = parseInt(this.zfTextareaMinHeight) - parseInt(getComputedStyle(e.target,null).lineHeight);
            this.zfTextareaMinHeight <= wrapInitialHeight && (this.zfTextareaMinHeight = wrapInitialHeight)
            this.zfTextareaMinHeight += 'px';
            console.log(this.zfTextareaMinHeight, parseInt(getComputedStyle(e.target,null).lineHeight))
          }else{
            (this.zfTextareaMinHeight = this.$refs.zfTextarea.scrollHeight + "px");
          } 
        }
      }
    }
  },
  computed: {
    inputListeners() {
      // `Object.assign` 将所有的对象合并为一个新对象
      return Object.assign(
        {}, //这里要有个空对象不知道为什么，不然父组件不绑定事件会报错
        // 我们从父级添加所有的监听器
        this.$listeners,
        // 然后我们添加自定义监听器，
        // 或覆写一些监听器的行为
        {
          // 这里确保组件配合 `v-model` 的工作
          input: event => {
            //负责改变父级value
            this.$emit("input", event.target.value);
            //负责改变自己value
            this.handlerInput(event);
          }
        }
      );
    }
  }
};
</script>