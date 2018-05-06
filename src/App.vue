<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>

    {{ this.$data.testcounter2 }}

    <Bar ref="bar" v-bind:val=0.9 />

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import Bar from "@/views/game/ui/Bar.vue";

@Component({
  components: { Bar },
  data() {
    return {
      testcounter2: 123
    };
  }
})
export default class App extends Vue {
  mounted() {
    console.log(`App mounted! 20180506 2343`);
    const bar = this.$refs.bar as Bar;
    // console.log(bar, bar.$data.myValue);

    setInterval(() => {
      this.$data.testcounter2++;
      bar.$data.myValue = (this.$data.testcounter2 / 100) % 1;
      // console.log(this.$data.testcounter2, bar.$data.myValue);
    }, 1000);
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Century Gothic", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  // padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
