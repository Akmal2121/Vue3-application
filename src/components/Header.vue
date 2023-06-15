<template>
  <header>
    <ul>
      <li
        v-for="(item, index) in titleTexts"
        :key="index"
        @click="getTitle(index)"
      >
        {{ item }}
      </li>
    </ul>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    titleTexts: {
      type: Array,
      required: true,
    },
    totalSpending: {
      type: Array,
      required: true,
    },
  },

  methods: {
    getTitle(index) {
      this.$emit("getTitle", index);
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
ul {
  display: flex;
  justify-content: space-between;
  list-style-type: none;
  padding: 10px 0px;
}
li {
  padding: 5px;
  cursor: pointer;
}
li:hover {
  color: rgba(0, 0, 0, 0.432);
}
header {
  background-color: rgb(184, 184, 184);
}
</style>
