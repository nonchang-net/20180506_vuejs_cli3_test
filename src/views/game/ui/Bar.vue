<template>
  <div class="main">
		<div class="inner" v-bind:style="width" />
		<div class="border" />
  </div>
</template>


<script lang="ts">
// import Styler from "./Styler";
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  data() {
    return {
      myValue: 0.0
    };
  }
})
export default class Bar extends Vue {
  // @Prop({ type: Number })
  // firstValue: number = 0.6;

  // data() {
  //   return {
  //     myValue: 0.6
  //   };
  // }

  mounted() {
    // this.inner = undefined;
    // console.log(`Bar.vue mounted! 2`);
  }

  //compuited getter
  get width(): {} {
    const style = `${Math.floor(this.$data.myValue * 100)}%`;
    // console.log(`computed width() entered. ${style}`);
    return { width: style };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
div.main {
  background: rgba(0, 0, 0, 0.3);
  height: 8px;
  width: 100%;
  flex: 0 2 auto;
  position: relative;
  //padding: 1px;
  margin-bottom: 1px;
  border-radius: 3px;

  div.inner {
    // background: rgba(0,255,0,1);
    background: linear-gradient(
      180deg,
      rgba(0, 255, 14, 0.2) 0%,
      rgba(42, 121, 9, 1) 30%,
      rgba(42, 255, 9, 1) 40%,
      rgba(31, 255, 0, 0.1) 100%
    );
    // background: linear-gradient(180deg, rgba(0,255,14,1) 0%, rgba(217,255,71,1) 38%, rgba(31,255,0,1) 100%);
    height: 80%;
    margin: 1px;
    // border-radius: 3px;
  }
  div.border {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: 1px solid gray;
    border-radius: 3px;
  }
}
</style>


