<template>
  <ul>
    <li
      v-for="(memoTitle, index) in memoTitles"
      :key="memoTitle.id"
    >
      <button @click="edit(index)">
        {{memoTitle}}
      </button>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'MemoIndex',
  props: {
    memoTitles: Object
  },
  methods:{
    edit(index) {
      this.$emit('edit-start', index)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- <style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style> -->
