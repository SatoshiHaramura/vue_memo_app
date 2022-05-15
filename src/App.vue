<template>
  <div id="app">
    <ListMemo
    :memos="memos"
    @change-edit-mode="changeEditMode"
    @change-append-mode="changeAppendMode"/>

    <EditMemo
    v-show="isEditMode || isAppendMode"
    :memo="memos[targetIndex]"
    @update-memo="updateMemo"
    @delete-memo="deleteMemo"/>
  </div>
</template>

<script>
import ListMemo from '@/components/ListMemo'
import EditMemo from '@/components/EditMemo'
const LOCAL_STORAGE_KEY = 'memos'
const LIST_MODE = 'LIST_MODE'
const EDIT_MODE = 'EDIT_MODE'
const APPEND_MODE = 'APPEND_MODE'

export default {
  name: 'App',
  components: {
    ListMemo,
    EditMemo
  },
  data() {
    return {
      memos: [],
      nextMemoId: 1,
      currentMode: LIST_MODE,
      targetIndex: undefined
    }
  },
  mounted() {
    if (localStorage.getItem(LOCAL_STORAGE_KEY)) {
      try {
        this.memos = JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY))
      } catch(e) {
        localStorage.removeItem(LOCAL_STORAGE_KEY)
      }
    }
  },
  computed: {
    isEditMode() {
      return this.currentMode === EDIT_MODE
    },
    isAppendMode() {
      return this.currentMode === APPEND_MODE
    }
  },
  methods: {
    changeEditMode(index) {
      this.currentMode = EDIT_MODE
      this.targetIndex = index
    },
    changeAppendMode() {
      this.currentMode = APPEND_MODE
      this.targetIndex = undefined
    },
    changeListMode() {
      this.currentMode = LIST_MODE
      this.targetIndex = undefined
    },
    updateMemo(memo) {
      if (!memo.content) return
      if (this.isEditMode) {
        this.memos.splice(this.targetIndex, 1, memo)
      } else if (this.isAppendMode) {
        memo.id = this.nextMemoId++
        this.memos.push(memo)
      }
      this.saveMemos()
      this.changeListMode()
    },
    deleteMemo() {
      if (this.isEditMode) {
        this.memos.splice(this.targetIndex, 1)
        this.saveMemos()
      }
      this.changeListMode()
    },
    saveMemos() {
      localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(this.memos))
    }
  }
}
</script>

<style lang="scss">
@import 'bulma/css/bulma.css';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  margin: 100px 100px;
  border: 1px solid #ccc;
  border-radius: 10px 10px 10px 10px
}
</style>
