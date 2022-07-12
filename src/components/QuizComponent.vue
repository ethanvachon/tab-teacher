<template>
  <div class="w-100 h-100 d-flex justify-content-around align-items-center" v-if="studyMethod == 'Tab to Note'">
    <div class="card">test</div>
    <div class="card">test</div>
  </div>
  <div class="w-100 h-100 d-flex justify-content-around align-items-center" v-if="studyMethod == 'Note to Tab'">
    <div class="card">test2</div>
    <div class="card">test2</div>
  </div>
</template>

<script>
export default {
  name: 'QuizComponent',
  props: {
    studyMethod: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
