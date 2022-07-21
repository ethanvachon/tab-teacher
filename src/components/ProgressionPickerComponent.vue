<template>
  <div class="row my-2">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow mt-2" v-if="randomProgression">
        <div class="d-flex border-bottom p-2">
          <h5 class="m-0">{{randomScale[0]}} {{randomMode}}</h5>
        </div>
        <div class="d-flex justify-content-around p-2">
          <div :key="index" v-for="(note, index) in randomProgression">
            <p>{{toNumeral(note)}}</p>
            <p class="m-0 px-2">{{randomScale[note - 1]}}</p>
          </div>
        </div>
        <div class="d-flex justify-content-center py-2 border-top">
          <button class="btn-sm btn-primary" @click="generateScale()">Generate</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProgressionPickerComponent',
  mounted () {
    this.generateScale()
    this.generateProgression()
  },
  data () {
    return {
      notes: ['C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C', 'C#/Db', 'D', 'D#/Eb', 'E', 'F', 'F#/Gb', 'G', 'G#/Ab', 'A', 'A#/Bb', 'B', 'C'],
      tonics: ['C', 'Db', 'D', 'Eb', 'E', 'F', 'G', 'Ab', 'A', 'Bb', 'B'],
      numerals: [
        ['I', 'ii', 'iii', 'IV', 'V', 'vi', 'vii°'],
        ['i', 'ii°', 'III', 'iv', 'v', 'VI', 'VII'],
        ['i', 'ii°', 'III+', 'iv', 'V', 'VI', 'vii°']
      ],
      progressions: [
        [1, 5, 4, 6],
        [1, 4, 5],
        [1, 4, 6, 5],
        [6, 4, 1, 5]
      ],

      randomMode: null,
      randomScale: null,
      randomProgression: null
    }
  },
  methods: {
    generateScale () {
      const mode = this.generateMode()
      const tonic = this.tonics[Math.floor(Math.random() * this.tonics.length)]
      const scale = [tonic]
      for (let i = 1; i <= 7; i++) {
        let aryElement = tonic
        if (!this.notes.includes(tonic)) {
          aryElement = this.notes.find(n => n.includes(tonic))
        }
        let toPush = this.notes[this.notes.indexOf(aryElement) + mode[i]]
        if (toPush.includes('/')) {
          if (toPush[0] === scale[scale.length - 1][0]) {
            toPush = toPush.split('/')[1]
          } else {
            toPush = toPush.split('/')[0]
          }
        }
        if (toPush[0] === scale[scale.length - 1][0]) {
          let newNote = `${this.notes[this.notes.indexOf(toPush) + 1]}b`
          if (newNote.includes('/')) {
            newNote = newNote.split('/')[1]
          }
          toPush = newNote
        }
        scale.push(toPush)
      }
      this.randomScale = scale
      this.generateProgression()
    },
    generateMode () {
      const rand = Math.floor(Math.random() * 3)
      if (rand === 0) {
        // major
        this.randomMode = 'Major'
        return [0, 2, 4, 5, 7, 9, 11, 12]
      } else if (rand === 1) {
        // minor
        this.randomMode = 'Minor'
        return [0, 2, 3, 5, 7, 8, 10, 12]
      } else if (rand === 2) {
        // harmonic minor
        this.randomMode = 'Harmonic Minor'
        return [0, 2, 3, 5, 7, 8, 11, 12]
      }
    },
    generateProgression () {
      this.randomProgression = this.progressions[Math.floor(Math.random() * this.progressions.length)]
    },
    toNumeral (index) {
      if (this.randomMode === 'Major') {
        return this.numerals[0][index - 1]
      } else if (this.randomMode === 'Minor') {
        return this.numerals[1][index - 1]
      } else if (this.randomMode === 'Harmonic Minor') {
        return this.numerals[2][index - 1]
      }
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
