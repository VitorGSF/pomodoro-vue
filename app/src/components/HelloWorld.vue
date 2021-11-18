<template>
  <div>
    TIMER POMODORO
    <div>
      <h1>
        {{displayMinutes}} : {{displaySeconds}}
      </h1>
    </div>
    <div>
      <button @click="start">INICIAR</button>
      <button @click="stop">PAUSAR</button>
      <button @click="reset" v-if="isRunning">REINICIAR</button>
    </div>
    <br>
    <div>
      <button @click="pomodoro">POMODORO</button>
      <button @click="interval">INTERVALO</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      instanceTime: null,
      totalSeconds: 25 * 60,
      intervalTime: 5 * 60,
      pomodoroTime: true
    }
  },
  computed: {
    displayMinutes() {
      if (this.pomodoroTime) {
        const minutes = Math.floor(this.totalSeconds / 60)
        return this.formaterTime(minutes)
      } else {
        const minutes = Math.floor(this.intervalTime / 60)
        return this.formaterTime(minutes)
      }
    },
    displaySeconds() {
      if (this.pomodoroTime) {
        const seconds = this.totalSeconds % 60
        return this.formaterTime(seconds)
      } else {
        const seconds = this.intervalTime % 60
        return this.formaterTime(seconds)
      }
      
    }
  },
  methods: {
    formaterTime(time) {
      if (time < 10) {
        return '0' + time
      }
      return time.toString()
    },
    stop() {
      this.isRunning = false
      clearInterval(this.instanceTime)
    },
    start() {
      this.stop()
      this.isRunning = true
      this.instanceTime = setInterval(() => {
        if (this.pomodoroTime){
          if (this.totalSeconds == 0) {
            let context = new AudioContext(),
            oscillator = context.createOscillator()
            
            oscillator.type = 'sine'
            oscillator.connect(context.destination)
            oscillator.start(0)
          } else {
            this.totalSeconds -= 1
          }
        } else {
          if (this.intervalTime == 0) {
            let context = new AudioContext(),
            oscillator = context.createOscillator()
            
            oscillator.type = 'sine'
            oscillator.connect(context.destination)
            oscillator.start(0)
          } else {
            this.intervalTime -= 1
          }
        }
      }, 1000)
    },
    reset() {
      this.stop()
      if (this.pomodoroTime) {
        this.totalSeconds = 25 * 60
      } else {
        this.intervalTime = 5 * 60
      }
      
    },
    interval() {
      this.pomodoroTime = false
    },
    pomodoro() {
      this.pomodoroTime = true
    }
  }
}
</script>

<style scoped>
div {
  text-align: center;
}
button{
  margin: 0 2px;
}
</style>
