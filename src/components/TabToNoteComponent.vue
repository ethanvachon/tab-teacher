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
      <div class="card shadow p-2">
        <div class="py-1">
          <span>G |--{{}}--</span>
        </div>
        <div class="py-1">
          <span>D |--{{}}--</span>
        </div>
        <div class="py-1">
          <span>A |--{{}}--</span>
        </div>
        <div class="py-1">
          <span>E |--{{}}--</span>
        </div>
        <div class="d-flex justify-content-center pt-2">
          <button class="btn-sm btn-success" @click="submitAnswer()">Submit</button>
        </div>
      </div>
    </div>
    <div class="col-12 col-md-6 d-flex align-items-center justify-content-center">
      <div class="card shadow p-2">
        <div class="py-1">
          <input type="number" class="w-25" v-model="input">
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
      input: '',
      answer: [],

      notes: ['C', 'C#', 'Db', 'D', 'D#', 'Eb', 'E', 'F', 'F#', 'Gb', 'G', 'G#', 'Ab', 'A', 'A#', 'Bb', 'B'],
      correctAnswers: 0
    }
  },
  methods: {
    submitAnswer () {
      console.log('checking answer')
    },
    nextNote () {
      this.reset()
      for (let i = 1; i <= 4; i++) {
        const tab = Math.floor(Math.random() * (this.notes.length))
        switch (tab) {
          case 1:
            const index = Math.floor(Math.random() * (this.Gnotes.length))
            this.Gtab.push(index)
            this.answer.push(this.Gnotes[index])
            break
          case 2:
            const index = Math.floor(Math.random() * (this.Dnotes.length))
            this.Dtab.push(index)
            this.answer.push(this.Dnotes[index])
            break
          case 3:
            const index = Math.floor(Math.random() * (this.Anotes.length))
            this.Atab.push(index)
            this.answer.push(this.Anotes[index])
            break
          case 4:
            const index = Math.floor(Math.random() * (this.Enotes.length))
            this.Etab.push(index)
            this.answer.push(this.Enotes[index])
            break
        }
      }
    },
    reset () {
      this.Gtab = []
      this.Dtab = []
      this.Atab = []
      this.Etab = []
      this.input = null
    },
    hardReset () {
      console.log('hard resetting?')
    }
  }
}
</script>

<style scoped>
#staff {
  width: 8em;
}
</style>
