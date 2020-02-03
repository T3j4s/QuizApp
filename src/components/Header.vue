<template>
  <div>
    <b-nav>
      <b-nav-item disabled>
        <b>Quiz Application</b>
      </b-nav-item>
      <b-nav-item disabled
        >Score: {{ correctCount }}/{{ totalCount }}</b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["correctCount", "totalCount"]
};
</script>
