<template>
  <nav class="navbar navbar-dark bg-dark">
    <a class="navbar-brand fw-bold" href="#">Tab Teacher</a>
      <select class="rounded p-1" v-model="studyMethod">
        <option value="Note to Tab">Note to Tab</option>
        <option value="Tab to Note">Tab to Note</option>
      </select>
  </nav>
  <div class="container height">
    <NoteToTabComponent v-if="studyMethod == 'Note to Tab'" />
    <TabToNoteComponent v-if="studyMethod == 'Tab to Note'" />
  </div>
</template>

<script>
// @ is an alias to /src
import NoteToTabComponent from '@/components/NoteToTabComponent.vue'
import TabToNoteComponent from '@/components/TabToNoteComponent.vue'

export default {
  name: 'HomeView',
  components: {
    NoteToTabComponent,
    TabToNoteComponent
  },
  data () {
    return {
      studyMethod: 'Note to Tab'
    }
  }
}
</script>

<style>
nav {
  padding: 30px;
}

nav a {
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.height {
  height: 50vh;
}
</style>
