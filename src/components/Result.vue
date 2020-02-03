<template>
  <div>
    <div class="result">
      <h4>Quiz Completed</h4>
      <h5>Your Score is {{ correctCount }}/{{ totalCount }}</h5>
      <b-button @click="reload" variant="info">Reload</b-button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["correctCount", "totalCount"],
  methods: {
    reload() {
      window.reload();
    }
  }
};
</script>

<style scoped>
.result {
  margin-top: 150px;
}
</style>
