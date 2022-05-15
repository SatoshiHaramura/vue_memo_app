<template>
  <section class="section">
    <div class="content">
      <h1 class="title">一覧</h1>
      <ul>
        <li v-for="(memo, index) in memos" :key="memo.id">
          <span @click="changeEditMode(index)">{{ showTitle(memo.content) }}</span>
        </li>
      </ul>
      <button @click="changeAppendMode" class="button is-light is-small">
        +
      </button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ListMemo',
  props: {
    memos: Array
  },
  methods: {
    changeEditMode(index) {
      this.$emit('change-edit-mode', index)
    },
    changeAppendMode() {
      this.$emit('change-append-mode')
    },
    showTitle(memo) {
      return memo.split(/\n/)[0]
    }
  }
}
</script>

<style>
li {
  word-break : break-all;
}
</style>
