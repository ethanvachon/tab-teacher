<template>
  <div class="row my-2" v-if="answered !== 10">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow py-2 px-4 mt-2">
        <h5 class="m-0">{{correctAnswers}}/10 Correct</h5>
        <h5 class="m-0"><span v-if="answered !== 0">{{Math.round((correctAnswers/answered) * 100)}}</span><span v-else>--</span>%</h5>
        <button class="btn-sm btn-primary mt-2" @click="hardReset()">Reset</button>
      </div>
    </div>
  </div>
  <div class="row mt-5" v-if="answered !== 10">
    <div class="col-12 col-md-6 d-flex align-items-center justify-content-center">
      <div class="card shadow p-1 mb-2" v-if="answers.length === 4">
        <div class="py-1 d-flex">
          <span>G |---<span :key="index" v-for="(note, index) in answers"><span v-if="note.string == 'Gtab'">{{note.index}}</span><span v-else>--</span>---</span></span>
        </div>
        <div class="py-1 d-flex">
          <span>D |---<span :key="index" v-for="(note, index) in answers"><span v-if="note.string == 'Dtab'">{{note.index}}</span><span v-else>--</span>---</span></span>
        </div>
        <div class="py-1 d-flex">
          <span>A |---<span :key="index" v-for="(note, index) in answers"><span v-if="note.string == 'Atab'">{{note.index}}</span><span v-else>--</span>---</span></span>
        </div>
        <div class="py-1 d-flex">
          <span>E |---<span :key="index" v-for="(note, index) in answers"><span v-if="note.string == 'Etab'">{{note.index}}</span><span v-else>--</span>---</span></span>
        </div>
        <div class="d-flex justify-content-center pt-2">
          <button class="btn-sm btn-success" @click="submitAnswer()">Submit</button>
        </div>
      </div>
    </div>
    <div class="col-12 col-md-6 d-flex align-items-center justify-content-center">
      <div class="card shadow p-2">
        <div class="py-1">
          <input type="text" class="input-width mx-1" v-model="input[0]">
          <input type="text" class="input-width mx-1" v-model="input[1]">
          <input type="text" class="input-width mx-1" v-model="input[2]">
          <input type="text" class="input-width mx-1" v-model="input[3]">
        </div>
        <div class="d-flex justify-content-center pt-2">
          <button class="btn-sm btn-success" @click="submitAnswer()">Submit</button>
        </div>
      </div>
    </div>
  </div>
  <div class="row" v-if="answered === 10">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow p-2 mt-3">
        <h4 v-if="correctAnswers == 10">Perfect! You scored a {{Math.round((correctAnswers/10) * 100)}}%</h4>
        <h4 v-if="correctAnswers >= 8 && correctAnswers != 10">Congrats! You scored a {{Math.round((correctAnswers/10) * 100)}}%</h4>
        <h4 v-if="correctAnswers >= 5 && correctAnswers < 7">Not bad! You scored a {{Math.round((correctAnswers/10) * 100)}}%</h4>
        <h4 v-if="correctAnswers < 5">Try again! You scored a {{Math.round((correctAnswers/10) * 100)}}%</h4>
        <h5 class="m-0">{{correctAnswers}}/10 Correct</h5>
        <button class="btn-sm btn-primary mt-3" @click="hardReset()">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TabToNoteComponent',
  mounted () {
    this.nextNote()
  },
  data () {
    return {
      Gnotes: ['G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F'],
      Gtab: [],
      Dnotes: ['D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C'],
      Dtab: [],
      Anotes: ['A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G'],
      Atab: [],
      Enotes: ['E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D'],
      Etab: [],
      input: ['', '', '', ''],
      answers: [],
      answered: 0,

      notes: ['C', 'C#', 'Db', 'D', 'D#', 'Eb', 'E', 'F', 'F#', 'Gb', 'G', 'G#', 'Ab', 'A', 'A#', 'Bb', 'B'],
      correctAnswers: 0
    }
  },
  methods: {
    submitAnswer () {
      this.answered++
      let correct = true
      for (let i = 0; i < this.input.length; i++) {
        const answer = this[this.answers[i].string[0] + 'notes'][this.answers[i].index]
        if (this.input[i] !== answer.split('/')[0] && this.input[i] !== answer.split('/')[1]) {
          correct = false
        }
      }
      if (correct === true) {
        this.correctAnswers++
      }
      this.nextNote()
    },
    nextNote () {
      this.reset()
      for (let i = 1; i <= 4; i++) {
        const tab = Math.floor((Math.random() * 4) + 1)
        let index
        switch (tab) {
          case 1:
            index = Math.floor(Math.random() * (this.Gnotes.length))
            this.answers.push({
              note: this.Gnotes[index],
              index: index,
              string: 'Gtab'
            })
            break
          case 2:
            index = Math.floor(Math.random() * (this.Dnotes.length))
            this.answers.push({
              note: this.Dnotes[index],
              index: index,
              string: 'Dtab'
            })
            break
          case 3:
            index = Math.floor(Math.random() * (this.Anotes.length))
            this.answers.push({
              note: this.Anotes[index],
              index: index,
              string: 'Atab'
            })
            break
          case 4:
            index = Math.floor(Math.random() * (this.Enotes.length))
            this.answers.push({
              note: this.Enotes[index],
              index: index,
              string: 'Etab'
            })
            break
        }
      }
    },
    reset () {
      this.Gtab = []
      this.Dtab = []
      this.Atab = []
      this.Etab = []
      this.answers = []
      this.input = ['', '', '', '']
    },
    hardReset () {
      this.answered = 0
      this.nextNote()
    }
  }
}
</script>

<style scoped>
#staff {
  width: 8em;
}

.input-width {
  width: 4em;
}
</style>
