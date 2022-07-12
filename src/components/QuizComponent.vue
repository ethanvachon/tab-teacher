<template>
  <div class="w-100 h-100 d-flex justify-content-around align-items-center" v-if="studyMethod == 'Tab to Note'">
    <div class="card">test</div>
    <div class="card">test</div>
  </div>
  <div class="row mt-5" v-if="studyMethod == 'Note to Tab'">
    <div class="col-6">
      <div class="d-flex justify-content-end" id="staff"></div>
    </div>
    <div class="col-6 d-flex align-items-center justify-content-center">
      <div class="card p-2">
        <div class="py-1">
          <span>G |--<input type="text" class="w-25">--</span>
        </div>
        <div class="py-1">
          <span>D |--<input type="text" class="w-25">--</span>
        </div>
        <div class="py-1">
          <span>A |--<input type="text" class="w-25">--</span>
        </div>
        <div class="py-1">
          <span>E |--<input type="text" class="w-25">--</span>
        </div>
        <div class="d-flex justify-content-center pt-2">
          <button class="btn-sm btn-primary">Submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vex from 'vexflow'

export default {
  name: 'QuizComponent',
  props: {
    studyMethod: String
  },
  mounted () {
    this.nextNote()
  },
  data () {
    return {
      notes: ['C', 'D', 'E', 'F', 'G', 'A', 'B'],
      VF: Vex.Flow,
      context: null,
      stave: null
    }
  },
  methods: {
    nextNote () {
      const note = this.notes[Math.floor(Math.random() * (this.notes.length))]
      const accidental = ['', '#', 'b'][Math.floor(Math.random() * (2 + 1))]
      var vf = new Vex.Flow.Factory({ renderer: { elementId: 'staff' } })
      var score = vf.EasyScore()
      var system = vf.System()
      system.addStave({
        voices: [score.voice(score.notes(`${note}${accidental}4/w`))]
      }).addClef('treble').addTimeSignature('4/4')
      vf.draw()
    }
  }
}
</script>

<style scoped>
.card {

}
</style>
