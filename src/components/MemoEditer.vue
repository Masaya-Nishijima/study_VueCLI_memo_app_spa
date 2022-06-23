<template>
  <MemoIndex
    class="edit-index"
    :memoTitles="memoTitles"
    @edit-start="indexToAppRoot"
  />
  <div class="edit-main">
    <div class="edit-input">
      <input
        type="text"
        v-model="editMemo.title"
      />
      <textarea
        v-model="editMemo.body"
      />
    </div>
    <div class="edit-button">
      <button class="save-button" @click="memoSave()">保存</button>
      <button class="delete-button" @click="memoDelete()">削除</button>
    </div>
  </div>
</template>

<script>
import MemoIndex from './MemoIndex.vue'
export default {
  name: 'MemoEditer',
  props: {
    editMemoJSON: JSON,
    memoTitles: Object
  },
  components: {
    MemoIndex
  },
  computed: {
    editMemo:{
      get() {
        return JSON.parse(this.editMemoJSON)
      }
    }
  },
  methods: {
    memoSave() {
      this.$emit('edit-end', this.editMemo)
    },
    memoDelete() {
      this.$emit('memo-delete')
    },
    indexToAppRoot(index){
      this.$emit('index-to-app-root', index)
    }
  }
}
</script>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}

.edit-index {
  flex-grow: 1;
}
.edit-main {
  flex-grow: 3;
}
.edit-input {
  border: solid 1px;
  padding: 5px;
  border-radius: 5px;
}
textarea {
  margin-top: 5px;
  height: 10em;
}
.edit-button {
  display: flex;
  flex-direction: row;
}
.edit-button button{
  border: solid 1px;
  margin-top: 10px;
  border-radius: 5px;
}
.save-button {
  flex-grow: 3;
  margin-right: 15px;
  background-color: skyblue;
}
.delete-button {
  flex-grow: 1;
  margin-left: 15px;
  background-color: gray;
  color: white;
}
</style>
