<template>
  <div class="row my-2">
    <div class="col-12 d-flex justify-content-center">
      <div class="card shadow mt-2" v-if="randomProgression">
        <div class="d-flex border-bottom p-2">
          <h5 class="m-0">{{randomScale[0]}} {{randomMode}}</h5>
        </div>
        <div class="d-flex justify-content-around p-2">
          <p class="m-0 px-2" :key="index" v-for="(note, index) in randomProgression">{{randomScale[note - 1]}}</p>
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
      //   tonics: ['C', 'Db', 'D', 'Eb', 'E', 'F', 'G', 'Ab', 'A', 'Bb', 'B'],
      tonics: ['Db'],
      progressions: [
        [1, 5, 4, 6]
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
        scale.push(toPush)
      }
      this.randomScale = scale
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
