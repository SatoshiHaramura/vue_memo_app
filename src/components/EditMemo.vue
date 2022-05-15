<template>
  <section class="section">
    <div class="content">
      <h1 class="title">編集</h1>
      <textarea v-model.trim="inputItem" class="textarea" placeholder="メモを入力してください。" rows="5">
      </textarea><br>
      <div class="field is-grouped">
        <p class="control">
          <button @click="updateMemo" type="button" class="button is-success is-outlined is-rounded">編集
          </button>
        </p>
        <p class="control">
          <button @click="deleteMemo" type="button" class="button is-danger is-outlined is-rounded">削除
          </button>
        </p>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'EditMemo',
  props: {
    memo: {
      type: Object,
      default: () => {
        return {
          id: undefined,
          content: ''
        }
      }
    }
  },
  data() {
    return {
      enteredItem: ''
    }
  },
  computed: {
    inputItem: {
      get() {
        return this.memo.content
      },
      set(memo) {
        this.enteredItem = memo
      }
    }
  },
  methods: {
    updateMemo() {
      const enteredMemo = {
        id: this.memo.id,
        content: this.enteredItem
      }
      this.$emit('update-memo', enteredMemo)
      this.initializeEnterItem()
    },
    deleteMemo() {
      this.$emit('delete-memo')
      this.initializeEnterItem()
    },
    initializeEnterItem() {
      this.enteredItem = ''
    }
  }
}
</script>

<style>

</style>
