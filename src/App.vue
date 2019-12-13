<template>
  <div id="app">
    <zf-button :type="false" icon="zf-icon-qq" diffuse loading>点击联系QQ</zf-button>
    <zf-link icon="zf-icon-wx" href="#" @click="btnClick">你好我的连接</zf-link>
    <zf-input
      placeholder="你好请输入"
      icon="zf-icon-wx"
      type="text"
      maxlength="10"
      @input="input1"
      show-length
      show-eye
      clearable
      disabled
    />

    <zf-input
      class="jiujiu"
      placeholder="你好请输入"
      type="textarea"
      maxlength="100"
      v-model="message"
      show-length
    />

    <div style="overflow:hidden;">
      <zf-container class="container" :height="winheight" scroll-y style="background:#eee;">nihao</zf-container>
    </div>
    <zf-follow-container>
      <template #content-container>
        我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>
        我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>
        我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>
      </template>
      <template #follow-container>
        我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>
        我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>我是跟随<br>
      </template>
    </zf-follow-container>
    <p>{{message}}</p>
  </div>
</template>

<script>
import ZfButton from "./components/ZfButton";
import ZfLink from "./components/ZfLink";
import ZfInput from "./components/ZfInput";
import ZfContainer from "./components/ZfContainer";
import ZfFollowContainer from "./components/ZfFollowContainer";

export default {
  name: "App",
  data() {
    return {
      loading: true,
      message: "",
      winheight: 0
    };
  },
  mounted() {
    this.winheight = 3000;
  },
  components: {
    ZfButton,
    ZfLink,
    ZfInput,
    ZfContainer,
    ZfFollowContainer
  },
  methods: {
    btnClick(e) {
      console.log("触发了点击事件", e);
      this.loading = !this.loading;
    },
    input1(e) {
      console.log(e);
      this.message = e;
    },
    touch(e) {
      console.log(e);
    }
  }
};
</script>

<style>
#app { font-family: "Avenir", Helvetica, Arial, sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; text-align: center; color: #2c3e50; margin-top: 60px;}
.jiujiu { width: 300px; min-height: 100px; }
</style>
