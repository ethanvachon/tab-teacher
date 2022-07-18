<template>
  <div class="row my-2" v-if="notes.length !== 0">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow py-2 px-4 mt-2">
        <h5 class="m-0">{{correctAnswers}}/17 Correct</h5>
        <h5 class="m-0"><span v-if="(17 - notes.length) !== 0">{{Math.round((correctAnswers/(17 - notes.length)) * 100)}}</span><span v-else>--</span>%</h5>
        <button class="btn-sm btn-primary mt-2" @click="hardReset()">Reset</button>
      </div>
    </div>
  </div>
  <div class="row mt-5" v-if="notes.length !== 0">
    <div class="col-12 col-md-6 d-flex align-items-center justify-content-center">
      <div class="d-flex justify-content-end shadow mb-2" id="staff"></div>
    </div>
    <div class="col-12 col-md-6 d-flex align-items-center justify-content-center">
      <div class="card shadow p-2">
        <div class="py-1">
          <span>G |--<input type="number" class="w-25" v-model="Ginput">--</span>
        </div>
        <div class="py-1">
          <span>D |--<input type="number" class="w-25" v-model="Dinput">--</span>
        </div>
        <div class="py-1">
          <span>A |--<input type="number" class="w-25" v-model="Ainput">--</span>
        </div>
        <div class="py-1">
          <span>E |--<input type="number" class="w-25" v-model="Einput">--</span>
        </div>
        <div class="d-flex justify-content-center pt-2">
          <button class="btn-sm btn-success" @click="submitAnswer()">Submit</button>
        </div>
      </div>
    </div>
  </div>
  <div class="row" v-if="notes.length === 0">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow p-2 mt-3">
        <h4 v-if="correctAnswers == 17">Perfect! You scored a {{Math.round((correctAnswers/(17 - notes.length)) * 100)}}%</h4>
        <h4 v-if="correctAnswers >= 14 && correctAnswers != 17">Congrats! You scored a {{Math.round((correctAnswers/(17 - notes.length)) * 100)}}%</h4>
        <h4 v-if="correctAnswers >= 9 && correctAnswers < 14">Not bad! You scored a {{Math.round((correctAnswers/(17 - notes.length)) * 100)}}%</h4>
        <h4 v-if="correctAnswers < 9">Try again! You scored a {{Math.round((correctAnswers/(17 - notes.length)) * 100)}}%</h4>
        <h5 class="m-0">{{correctAnswers}}/17 Correct</h5>
        <button class="btn-sm btn-primary mt-3" @click="hardReset()">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
import Vex from 'vexflow'

export default {
  name: 'NoteToTabComponent',
  mounted () {
    this.nextNote()
  },
  data () {
    return {
      Ginput: null,
      Gnotes: ['G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F'],
      Dinput: null,
      Dnotes: ['D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C'],
      Ainput: null,
      Anotes: ['A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G'],
      Einput: null,
      Enotes: ['E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D'],
      answer: '',

      notes: ['C', 'C#', 'Db', 'D', 'D#', 'Eb', 'E', 'F', 'F#', 'Gb', 'G', 'G#', 'Ab', 'A', 'A#', 'Bb', 'B'],
      correctAnswers: 0,
      VF: Vex.Flow,
      context: null,
      stave: null
    }
  },
  methods: {
    submitAnswer () {
      let correct = true
      if (this.Ginput !== null && this.Ginput !== '') {
        const noteInput = this.Gnotes[this.Ginput]
        if (noteInput.includes('/')) {
          if (noteInput.split('/')[0] !== this.answer && noteInput.split('/')[1] !== this.answer) {
            correct = false
          }
        } else {
          if (noteInput !== this.answer) {
            correct = false
          }
        }
      }
      if (this.Dinput !== null && this.Dinput !== '') {
        const noteInput = this.Dnotes[this.Dinput]
        if (noteInput.includes('/')) {
          if (noteInput.split('/')[0] !== this.answer && noteInput.split('/')[1] !== this.answer) {
            correct = false
          }
        } else {
          if (noteInput !== this.answer) {
            correct = false
          }
        }
      }
      if (this.Ainput !== null && this.Ainput !== '') {
        const noteInput = this.Anotes[this.Ainput]
        if (noteInput.includes('/')) {
          if (noteInput.split('/')[0] !== this.answer && noteInput.split('/')[1] !== this.answer) {
            correct = false
          }
        } else {
          if (noteInput !== this.answer) {
            correct = false
          }
        }
      }
      if (this.Einput !== null && this.Einput !== '') {
        const noteInput = this.Enotes[this.Einput]
        if (noteInput.includes('/')) {
          if (noteInput.split('/')[0] !== this.answer && noteInput.split('/')[1] !== this.answer) {
            correct = false
          }
        } else {
          if (noteInput !== this.answer) {
            correct = false
          }
        }
      }
      if (this.Einput === null && this.Ainput === null && this.Dinput === null && this.Ginput === null) {
        correct = false
      }
      if (correct === true) {
        this.correctAnswers++
      }
      this.nextNote()
    },
    nextNote () {
      this.notes = this.notes.filter(n => n !== this.answer)
      if (this.notes.length === 0) {
        return
      }
      this.reset()
      this.answer = this.notes[Math.floor(Math.random() * (this.notes.length))]
      const vf = new Vex.Flow.Factory({ renderer: { elementId: 'staff' } })
      const score = vf.EasyScore()
      const system = vf.System()
      system.addStave({
        voices: [score.voice(score.notes(`${this.answer}4/w`))]
      }).addClef('treble').addTimeSignature('4/4')
      vf.draw()
    },
    reset () {
      this.Ginput = null
      this.Dinput = null
      this.Ainput = null
      this.Einput = null
      const staff = document.getElementById('staff')
      if (staff) {
        while (staff.hasChildNodes()) {
          staff.removeChild(staff.lastChild)
        }
      }
    },
    hardReset () {
      this.notes = ['C', 'C#', 'Db', 'D', 'D#', 'Eb', 'E', 'F', 'F#', 'Gb', 'G', 'G#', 'Ab', 'A', 'A#', 'Bb', 'B']
      this.answer = ''
      this.correctAnswers = 0
      const self = this
      const checkExist = setInterval(function () {
        if (document.getElementById('staff')) {
          self.nextNote()
          clearInterval(checkExist)
        }
      }, 100)
    }
  }
}
</script>

<style scoped>
#staff {
  width: 8em;
}
</style>
