<template>
  <div class="memo-app">
    <div v-if="editMemoIndex === null">
      <h1>一覧</h1>
      <div class="memo-app-item index">
        <MemoIndex
          :memoTitles="memos.map(memo=>memo.title)"
          @edit-start="editStart"
        />
      </div>
    </div>
      <div v-else>
        <h1>編集</h1>
        <div class="memo-app-item editer">
          <MemoEditer
            :editMemoJSON="JSON.stringify(memos[editMemoIndex])"
            :memoTitles="memos.map(memo=>memo.title)"
            @edit-end="editEnd"
            @memo-delete="memoDelete"
            @index-to-app-root="editStart"
          /><!--参照渡し回避のためJSONでデータを渡す。オブジェクトの参照渡しで子コンポーネントでprops→computed getで受けとるとデータが常時親コンポーネントと同期する。-->
        </div>
      </div>
  </div>
</template>

<script>
import MemoIndex from './components/MemoIndex.vue'
import MemoEditer from './components/MemoEditer.vue'

export default {
  name: 'MemoApp',
  data() {
    return {
    editMemoIndex : null,
    memos : []
    }
  },
  created() {
    this.memos = JSON.parse(localStorage.getItem('memo-app'))
    this.memos == null ? this.memos = [{title: '+', body: ''}] : this.memos.push({title: '+', body: ''})
  },
  components: {
    MemoIndex,
    MemoEditer
  },
  methods: {
    editStart(index) {
      this.editMemoIndex = index
    },
    editEnd(editedMemo) {
      this.memos[this.editMemoIndex] = editedMemo
      localStorage.setItem("memo-app", JSON.stringify(this.memos))
      this.checkHavePlus()
      this.editMemoIndex = null
    },
    checkHavePlus() {
      if (JSON.stringify(this.memos.slice(-1)[0]) != JSON.stringify({title: '+', body: ''})) {
        this.memos.push({title: '+', body: ''})
      }
    },
    memoDelete() {
      this.memos.splice(this.editMemoIndex, 1)
      localStorage.setItem("memo-app", JSON.stringify(this.memos))
      this.checkHavePlus()
      this.editMemoIndex = null
    }
  }
}
</script>

<style>
#memo-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
.memo-app {
  width: 980px;
  margin-left: auto;
  margin-right: auto;
  padding: 5px;
  border-radius: 5px;
  box-shadow: rgba(0, 0, 0, .4) 0 0 8px;
  /* floatを使った場合の対策 */
  overflow: hidden;
}
.memo-app-item {
  border: solid 2px;
  border-radius: 15px;
  padding: 10px;
  margin: 40px 20px;
}
.editer {
  display: flex;
}
h1 {
  position: relative;
  top: 35px;
  left: 25px;
}
button {
  padding: 0;
  border: none;
  border-bottom: solid 1px;
  background-color: white;
  text-decoration-color: blue;
}
</style>
