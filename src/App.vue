<template>
  <div class="app">
    <Main></Main>
  </div>
</template>

<script>
import Main from "@/components/Main.vue";

export default {
  name: "App",
  components: {
    Main,
  },
  data() {},
  methods: {},
};
</script>

<style scoped>
.app * {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  list-style-type: none;
}
.app {
  margin: 0;
  padding: 0;
}
</style>
